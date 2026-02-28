# Showbiz Implementation Plan

## Executive Summary

Build a personal TV/movie companion app called **Showbiz** that helps users collect, organize, rate, and discover entertainment through a taste-aware AI assistant. The app combines traditional collection management with concept-based discovery powered by a consistent AI persona.

**Tech Stack:** Next.js (latest stable) + Supabase
**Architecture:** Fractal architecture (pages → features → sub-features) with humble components (logic extracted to hooks)

---

## 1. Data Model

### Core Entities

**Show** (public catalog data)
- `id`: UUID (primary key)
- `external_id`: string (catalog identifier)
- `title`: string
- `overview`: text
- `year`: integer
- `runtime`: integer (minutes for movies, seasons for TV)
- `genres`: string[]
- `language`: string
- `poster_url`: string
- `backdrop_url`: string
- `logo_url`: string
- `imdb_rating`: float
- `seasons`: TVSeason[] (TV only)
- `cast`: CastMember[]
- `crew`: CrewMember[]
- `providers`: StreamingProvider[]
- `budget`: float (movies)
- `revenue`: float (movies)

**MyData** (user overlay on shows)
- `id`: UUID (primary key)
- `namespace_id`: string (build isolation)
- `user_id`: string (opaque stable identifier)
- `show_id`: UUID (FK to Show)
- `status`: enum('active', 'later', 'wait', 'done', 'quit')
- `interest`: enum('interested', 'excited') | null (only for 'later' status)
- `rating`: float(1-10) | null
- `tags`: string[]
- `scoop_id`: UUID (FK to AI_Scoop, nullable)
- `created_at`: timestamp
- `updated_at`: timestamp
- `scoop_updated_at`: timestamp | null (for freshness)
- **Unique constraint**: `(namespace_id, user_id, show_id)`

**AI_Scoop** (generated content)
- `id`: UUID (primary key)
- `namespace_id`: string
- `user_id`: string
- `show_id`: UUID
- `content`: text (markdown)
- `model_version`: string
- `created_at`: timestamp
- **Unique constraint**: `(namespace_id, user_id, show_id)`

**Concept** (discovery ingredients)
- `id`: UUID (primary key)
- `namespace_id`: string
- `user_id`: string
- `show_id`: UUID (nullable for multi-show concepts)
- `label`: string (1-3 words)
- `axis`: enum('structure', 'vibe', 'emotion', 'dynamics', 'craft', 'genre-flavor')
- `created_at`: timestamp

**Session** (Ask chat history)
- `id`: UUID (primary key)
- `namespace_id`: string
- `user_id`: string
- `title`: string
- `messages`: JSON[] (structured with role/content/show_refs)
- `created_at`: timestamp
- `updated_at`: timestamp

### Indexes

```sql
CREATE INDEX idx_mydata_namespace_user ON MyData(namespace_id, user_id);
CREATE INDEX idx_mydata_show ON MyData(show_id);
CREATE INDEX idx_aiscoop_namespace_user ON AI_Scoop(namespace_id, user_id);
CREATE INDEX idx_concept_namespace_user ON Concept(namespace_id, user_id);
CREATE INDEX idx_session_namespace_user ON Session(namespace_id, user_id);
```

---

## 2. API Contract

### Collection Endpoints

**GET /api/collection**
- Query params: `status` (optional filter), `tag` (optional filter), `search` (optional)
- Returns: `MyData[]` with nested Show data

**POST /api/collection**
- Body: `{ showId, status, interest?, rating?, tags? }`
- Auto-save triggers:
  - Setting status → creates/updates MyData
  - Choosing interest → updates MyData.interest
  - Rating unsaved show → creates MyData with status='done'
  - Adding tag to unsaved show → creates MyData with status='later', interest='interested'
- Returns: MyData

**DELETE /api/collection/:showId**
- Clears MyData (removal confirmation required)
- May cascade delete AI_Scoop if show not in collection

### Show Detail Endpoints

**GET /api/shows/:id**
- Returns: Show + MyData (if exists for current user)

**POST /api/shows/:id/scoop**
- Generates AI Scoop (streams progressively)
- Cache: 4 hours (check `scoop_updated_at`)
- Only persists long-term if show in collection
- Returns: AI_Scoop

**GET /api/shows/:id/scoop**
- Returns cached AI_Scoop if exists

### Search & Discovery Endpoints

**GET /api/search**
- Query params: `q`, `page`, `limit`
- Returns: Paginated Show[]

**POST /api/ask**
- Body: `{ query, sessionId?, context? }`
- Returns: `{ commentary, showList? }` (structured format for mentions)
- showList format: `Title::externalId::mediaType;;Title2::externalId::mediaType;;...`

**POST /api/concepts**
- Body: `{ showIds: string[], count?: number }`
- Returns: Concept[] (1-3 words, evocative, bullet list)
- Multi-show: concepts must be shared across all inputs

**POST /api/explore-similar**
- Body: `{ showId, selectedConcepts: string[] }`
- Returns: Recommendation[] (5 recs)
- Each rec: `{ show, reason: string }`

**POST /api/alchemy**
- Body: `{ showIds: string[], selectedConcepts: string[] }`
- Returns: Recommendation[] (6 recs)

### Person Endpoints

**GET /api/persons/:id**
- Returns: Person + filmography[]

**GET /api/persons/:id/analytics**
- Returns: `{ showsInCollection: number, avgRating: float, genreBreakdown: {} }`

---

## 3. Page Structure (Fractal Architecture)

### Pages Directory

```
src/pages/
├── _app.tsx
├── _document.tsx
├── index.tsx                    # Collection Home
├── search.tsx                   # Search/Discover hub
├── shows/
│   └── [id].tsx                 # Show Detail
├── persons/
│   └── [id].tsx                 # Person Detail
├── settings.tsx                 # Settings
└── api/                         # API routes (Next.js)
    ├── collection/
    │   ├── index.ts
    │   └── [showId].ts
    ├── shows/
    │   └── [id]/
    │       ├── index.ts
    │       └── scoop.ts
    ├── search.ts
    ├── ask.ts
    ├── concepts.ts
    ├── explore-similar.ts
    ├── alchemy.ts
    └── persons/
        ├── [id].ts
        └── [id]/analytics.ts
```

---

## 4. Feature Breakdown

### 4.1 Collection Home (`/index.tsx`)

**Purpose:** First screen showing user's collection organized by status.

**Structure:**
```
CollectionHome/
├── CollectionHome.tsx           # Page wrapper
└── features/
    ├── StatusFilterBar/
    │   └── StatusFilterBar.tsx
    ├── CollectionList/
    │   ├── CollectionList.tsx
    │   ├── hooks/
    │   │   └── useCollection.ts
    │   └── components/
    │       ├── ShowCard/
    │       │   ├── ShowCard.tsx
    │       │   └── ShowCard.test.tsx
    │       └── EmptyState/
    │           └── EmptyState.tsx
    └── QuickActions/
        └── QuickActions.tsx
```

**State:**
- `statusFilter`: Status | 'all'
- `tagFilter`: string | null
- `searchQuery`: string
- `collection`: MyDataWithShow[]

**Hooks:**
- `useCollection(statusFilter, tagFilter, searchQuery)` → fetches from `/api/collection`

**Components:**
- `ShowCard`: displays show poster, title, status chip, rating, quick actions (rate, tag, change status)

---

### 4.2 Search/Discover Hub (`/search.tsx`)

**Purpose:** Central hub with 3 modes: Search, Ask (AI chat), Alchemy (concept blending).

**Structure:**
```
SearchHub/
├── SearchHub.tsx                # Page wrapper
└── features/
    ├── ModeSwitcher/
    │   └── ModeSwitcher.tsx     # Tabs: Search | Ask | Alchemy
    ├── SearchMode/
    │   ├── SearchMode.tsx
    │   ├── hooks/
    │   │   └── useSearch.ts
    │   └── components/
    │       └── SearchResults/
    │           └── SearchResults.tsx
    ├── AskMode/
    │   ├── AskMode.tsx
    │   ├── hooks/
    │   │   ├── useAskSession.ts
    │   │   └── useAskAI.ts
    │   └── components/
    │       ├── ChatInput/
    │       │   └── ChatInput.tsx
    │       ├── ChatMessage/
    │       │   ├── ChatMessage.tsx
    │       │   └── ChatMessage.test.tsx
    │       └── MentionedShows/
    │           └── MentionedShows.tsx
    └── AlchemyMode/
        ├── AlchemyMode.tsx
        ├── hooks/
        │   ├── useAlchemySession.ts
        │   └── useAlchemyAI.ts
        └── components/
            ├── InputShowSelector/
            │   └── InputShowSelector.tsx
            ├── ConceptSelector/
            │   ├── ConceptSelector.tsx
            │   └── hooks/
            │       └── useConceptGeneration.ts
            └── RecommendationList/
                └── RecommendationList.tsx
```

**Search Mode:**
- Simple query → shows results
- No AI voice (straightforward catalog search)

**Ask Mode:**
- Conversational chat with AI
- Session management (create, load, summarize old turns)
- Mentioned shows extraction (structured format)
- Voice: conversational, 1-3 paragraphs + lists

**Alchemy Mode:**
- Select ≥2 shows → generate concepts → select concepts → get recommendations
- State machine: selecting → generating → selecting recs
- 6 recommendations per round

**Hooks:**
- `useSearch(query)` → SearchResults
- `useAskSession()` → sessionId, createSession, loadSession
- `useAskAI(sessionId, messages)` → stream response
- `useAlchemySession()` → showIds, concepts, recommendations
- `useConceptGeneration(showIds)` → generate concepts
- `useAlchemyAI(showIds, concepts)` → recommendations

---

### 4.3 Show Detail (`/shows/[id].tsx`)

**Purpose:** Single source of truth for a show: public facts + user's version + discovery launchpad.

**Structure:**
```
ShowDetail/
├── ShowDetail.tsx               # Page wrapper
└── features/
    ├── HeaderCarousel/
    │   ├── HeaderCarousel.tsx
    │   └── components/
    │       └── MediaPlayer/
    │           └── MediaPlayer.tsx
    ├── CoreFacts/
    │   └── CoreFacts.tsx
    ├── Toolbar/
    │   ├── Toolbar.tsx
    │   ├── components/
    │   │   ├── StatusChips/
    │   │   │   └── StatusChips.tsx
    │   │   ├── RatingBar/
    │   │   │   └── RatingBar.tsx
    │   │   └── TagsInput/
    │   │       └── TagsInput.tsx
    │   └── hooks/
    │       └── useToolbarActions.ts
    ├── OverviewSection/
    │   ├── OverviewSection.tsx
    │   └── components/
    │       └── ScoopToggle/
    │           └── ScoopToggle.tsx
    ├── ScoopSection/
    │   ├── ScoopSection.tsx
    │   ├── hooks/
    │   │   └── useScoop.ts
    │   └── components/
    │       └── ScoopContent/
    │           └── ScoopContent.tsx
    ├── AskAboutShow/
    │   └── AskAboutShow.tsx       # CTA that seeds Ask with show context
    ├── TraditionalRecommendations/
    │   └── TraditionalRecommendations.tsx
    ├── ExploreSimilar/
    │   ├── ExploreSimilar.tsx
    │   ├── hooks/
    │   │   └── useExploreSimilar.ts
    │   └── components/
    │       ├── GetConceptsCTA/
    │       │   └── GetConceptsCTA.tsx
    │       ├── ConceptChips/
    │       │   └── ConceptChips.tsx
    │       └── ExploreResults/
    │           └── ExploreResults.tsx
    └── ProvidersSection/
        └── ProvidersSection.tsx
```

**Section Order (narrative hierarchy):**
1. Header media carousel
2. Core facts (year, runtime, community score)
3. Tag chips
4. Overview + Scoop toggle
5. Ask about this show CTA
6. Genres + languages
7. Recommendations strand
8. Explore Similar (concepts → recs)
9. Streaming providers
10. Cast, Crew
11. Seasons (TV only)
12. Budget/Revenue (movies)

**Toolbar Actions:**
- Status chips: Active, Later+Interested, Later+Excited, Wait, Done, Quit
- Rating bar: 1-10 scale
- Tags input: add/remove tags
- Auto-save triggers implemented

**Scoop UX:**
- Toggle: "Give me the scoop!" → "Show the scoop" → "The Scoop" (open)
- Progressive streaming: "Generating…" → content
- Freshness check: regenerate after 4 hours
- Persistence: only if show in collection

**Explore Similar Flow:**
1. Tap "Get Concepts"
2. Display concept chips
3. User selects 1+ concepts
4. Tap "Explore Shows"
5. Display 5 recommendations with reasons

**Hooks:**
- `useShow(id)` → Show + MyData
- `useScoop(showId)` → AI_Scoop, refreshScoop(), isFresh()
- `useExploreSimilar(showId)` → concepts, recommendations, generateConcepts(), getRecommendations()
- `useToolbarActions()` → setStatus(), setRating(), addTag(), removeTag()

---

### 4.4 Person Detail (`/persons/[id].tsx`)

**Purpose:** Show person's filmography and analytics.

**Structure:**
```
PersonDetail/
├── PersonDetail.tsx             # Page wrapper
└── features/
    ├── PersonHeader/
    │   └── PersonHeader.tsx
    ├── Filmography/
    │   ├── Filmography.tsx
    │   └── components/
    │       └── FilmographyItem/
    │           └── FilmographyItem.tsx
    └── Analytics/
        ├── Analytics.tsx
        └── components/
            ├── ShowsInCollection/
            │   └── ShowsInCollection.tsx
            ├── AvgRating/
            │   └── AvgRating.tsx
            └── GenreBreakdown/
                └── GenreBreakdown.tsx
```

---

### 4.5 Settings (`/settings.tsx`)

**Purpose:** AI config, integrations, data management.

**Structure:**
```
Settings/
├── Settings.tsx                 # Page wrapper
└── features/
    ├── AICustomization/
    │   ├── AICustomization.tsx
    │   └── components/
    │       ├── VoiceSelector/
    │       │   └── VoiceSelector.tsx
    │       └── SpoilerPreference/
    │           └── SpoilerPreference.tsx
    ├── Integrations/
    │   └── Integrations.tsx
    └── DataManagement/
        ├── DataManagement.tsx
        └── components/
            ├── ExportButton/
            │   └── ExportButton.tsx
            └── ImportRestore/
                └── ImportRestore.tsx
```

---

## 5. Shared Components

```
src/components/
├── ui/                          # Primitives
│   ├── Button/
│   │   └── Button.tsx
│   ├── Chip/
│   │   └── Chip.tsx
│   ├── Dialog/
│   │   └── Dialog.tsx
│   ├── Input/
│   │   └── Input.tsx
│   ├── ProgressBar/
│   │   └── ProgressBar.tsx
│   └── Skeleton/
│       └── Skeleton.tsx
├── layout/
│   ├── Navbar/
│   │   └── Navbar.tsx
│   ├── Footer/
│   │   └── Footer.tsx
│   └── Container/
│       └── Container.tsx
└── icons/
    └── index.tsx                # Icon exports
```

---

## 6. Global Hooks

```
src/hooks/
├── useDebounce.ts
├── useLocalStorage.ts
├── useMediaQuery.ts
├── useSessionStorage.ts
└── useFetch.ts
```

---

## 7. Global Utils

```
src/utils/
├── date.ts
├── formatting.ts
├── validation.ts
├── showListParser.ts           # Parse "Title::id::type;;..." format
└── testUtils.ts
```

**showListParser.ts:**
```typescript
// Parse structured format
export function parseShowList(showList: string): Array<{ title: string; externalId: string; mediaType: 'movie' | 'tv' }> {
  return showList.split(';;').map(part => {
    const [title, externalId, mediaType] = part.split('::');
    return { title, externalId, mediaType };
  });
}

// Format to structured string
export function formatShowList(shows: Array<{ title: string; externalId: string; mediaType: 'movie' | 'tv' }>): string {
  return shows.map(s => `${s.title}::${s.externalId}::${s.mediaType}`).join(';;');
}
```

---

## 8. Config & Theme

```
src/config/
├── env.ts                       # Environment validation
├── constants.ts
│   ├── scoopFreshnessMs = 4 * 60 * 60 * 1000  // 4 hours
│   ├── CONCEPT_COUNT = 8
│   ├── EXPLORE_SIMILAR_RECS = 5
│   └── ALCHEMY_RECS = 6
└── features.ts

src/theme/
├── tokens.ts                    # Design tokens (colors, spacing, typography)
├── colors.ts
├── typography.ts
└── index.ts
```

---

## 9. Environment Setup

### .env.example

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# AI Provider (example: OpenAI)
OPENAI_API_KEY=your-api-key

# Dev Identity (BENCHMARK MODE - disable for production)
NEXT_PUBLIC_DEV_USER_ID=dev-user-123
NEXT_PUBLIC_DEV_NAMESPACE_ID=benchmark-namespace

# App
NEXT_PUBLIC_APP_NAME=Showbiz
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

### .gitignore

```
.env
.env.local
.env.*.local
*.env
```

---

## 10. Database Migrations

```
supabase/migrations/
└── 001_initial_schema.sql
```

**Migration SQL:**

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Namespace (build isolation, not user-facing)
CREATE TABLE namespaces (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT UNIQUE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Shows (public catalog)
CREATE TABLE shows (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  external_id TEXT NOT NULL,
  title TEXT NOT NULL,
  overview TEXT,
  year INTEGER,
  runtime INTEGER,
  genres TEXT[],
  language TEXT,
  poster_url TEXT,
  backdrop_url TEXT,
  logo_url TEXT,
  imdb_rating FLOAT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- MyData (user overlay)
CREATE TABLE my_data (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  namespace_id UUID NOT NULL REFERENCES namespaces(id) ON DELETE CASCADE,
  user_id TEXT NOT NULL,
  show_id UUID NOT NULL REFERENCES shows(id) ON DELETE CASCADE,
  status TEXT NOT NULL CHECK (status IN ('active', 'later', 'wait', 'done', 'quit')),
  interest TEXT CHECK (interest IN ('interested', 'excited')),
  rating FLOAT CHECK (rating >= 1 AND rating <= 10),
  tags TEXT[],
  scoop_id UUID,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  scoop_updated_at TIMESTAMP WITH TIME ZONE,
  UNIQUE(namespace_id, user_id, show_id)
);

-- AI Scoop
CREATE TABLE ai_scoop (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  namespace_id UUID NOT NULL REFERENCES namespaces(id) ON DELETE CASCADE,
  user_id TEXT NOT NULL,
  show_id UUID NOT NULL REFERENCES shows(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  model_version TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  UNIQUE(namespace_id, user_id, show_id)
);

-- Concepts
CREATE TABLE concepts (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  namespace_id UUID NOT NULL REFERENCES namespaces(id) ON DELETE CASCADE,
  user_id TEXT NOT NULL,
  show_id UUID REFERENCES shows(id) ON DELETE CASCADE,
  label TEXT NOT NULL,
  axis TEXT CHECK (axis IN ('structure', 'vibe', 'emotion', 'dynamics', 'craft', 'genre-flavor')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Sessions (Ask chat)
CREATE TABLE sessions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  namespace_id UUID NOT NULL REFERENCES namespaces(id) ON DELETE CASCADE,
  user_id TEXT NOT NULL,
  title TEXT NOT NULL,
  messages JSONB NOT NULL DEFAULT '[]',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_mydata_namespace_user ON my_data(namespace_id, user_id);
CREATE INDEX idx_mydata_show ON my_data(show_id);
CREATE INDEX idx_aiscoop_namespace_user ON ai_scoop(namespace_id, user_id);
CREATE INDEX idx_concept_namespace_user ON concepts(namespace_id, user_id);
CREATE INDEX idx_session_namespace_user ON sessions(namespace_id, user_id);

-- Row Level Security (RLS)
ALTER TABLE my_data ENABLE ROW LEVEL SECURITY;
ALTER TABLE ai_scoop ENABLE ROW LEVEL SECURITY;
ALTER TABLE concepts ENABLE ROW LEVEL SECURITY;
ALTER TABLE sessions ENABLE ROW LEVEL SECURITY;

-- Policy: Users can only access their own data
CREATE POLICY "Users access own my_data" ON my_data
  FOR ALL USING (user_id = current_setting('app.current_user_id', true));

CREATE POLICY "Users access own ai_scoop" ON ai_scoop
  FOR ALL USING (user_id = current_setting('app.current_user_id', true));

CREATE POLICY "Users access own concepts" ON concepts
  FOR ALL USING (user_id = current_setting('app.current_user_id', true));

CREATE POLICY "Users access own sessions" ON sessions
  FOR ALL USING (user_id = current_setting('app.current_user_id', true));

-- Helper function to set current user (for API routes)
CREATE FUNCTION app.set_current_user(user_id TEXT) RETURNS VOID AS $$
  BEGIN
    SET app.current_user_id = user_id;
  END;
$$ LANGUAGE plpgsql;
```

---

## 11. AI Prompting Strategy

### Shared System Prompt

```
You are Showbiz, a fun, chatty TV/movie nerd friend who loves entertainment deeply.
You have sharp taste and aren't afraid to make a call.
You're generous with context and insider info.
You stay spoiler-safe unless explicitly invited otherwise.
You keep things light even when being critical.

Voice pillars:
1. Joy-forward and warm - you want the user to have a good night
2. Opinionated honesty - don't gush for no reason
3. Vibe-first, spoiler-safe - focus on tone, feeling, style, charm, themes
4. Specific, not generic - use concrete flavor, not genre boilerplate
5. Short when needed, lush when earned - concise by default

Stay within TV/movies domain. If asked outside, redirect back.
Be actionable: recommended titles must resolve to real catalog items.
```

### Surface-Specific Prompts

**Scoop Prompt:**
```
Write a personality-driven "taste review" for this show. Structure:

1. Personal take (make a stand)
2. Honest "stack-up" vs reviews
3. The Scoop (emotional centerpiece, ~150-200 words)
4. Fit/warnings (who it's for, who should skip)
5. Verdict ("Worth it?" gut check)

Length: ~150-350 words total. Stream progressively.
```

**Ask Prompt:**
```
Respond like a friend in dialogue (not an essay).
Be willing to pick favorites confidently.
Use simple formatting and bulleted lists when recommending multiple titles.
Length: 1-3 tight paragraphs, then a list if recommending multiple.

If the user asks for depth, expand accordingly.
```

**Concepts Prompt:**
```
Generate 8 short, evocative concept "ingredients" for this show.
Rules:
- 1-3 words each
- Vibe/structure/thematic ingredients, no plot
- Clever and specific; avoid genre clichés
- Order by strength (best "aha" concepts first)

Examples: "hopeful absurdity", "case-a-week", "quirky found-family"
Avoid: "good characters", "great story", "funny"

Output as bullet list only.
```

**Explore Similar Prompt:**
```
Generate 5 recommendations based on these selected concepts.
For each recommendation:
- Return real show with valid external catalog ID
- Provide concise reason (1-3 sentences) that explicitly references selected concepts
- Don't write synopses

Bias toward recent shows but allow classics/hidden gems.
```

---

## 12. Testing Strategy

### Unit Tests

**Location:** Adjacent to source files (`Component.test.tsx`)

**Focus Areas:**
- Utility functions (showListParser, date formatting, validation)
- Custom hooks (useDebounce, useFetch)
- Component rendering with mocked data
- AI output parsing

**Example:**
```typescript
// utils/showListParser.test.ts
import { parseShowList, formatShowList } from './showListParser';

describe('parseShowList', () => {
  it('parses structured format correctly', () => {
    const input = 'Show 1::ext123::tv;;Show 2::ext456::movie';
    const expected = [
      { title: 'Show 1', externalId: 'ext123', mediaType: 'tv' },
      { title: 'Show 2', externalId: 'ext456', mediaType: 'movie' }
    ];
    expect(parseShowList(input)).toEqual(expected);
  });
});
```

### Integration Tests

**Location:** `tests/e2e/`

**Focus Areas:**
- Collection CRUD operations
- AI endpoint responses (format, streaming)
- Concept generation and recommendation flow
- Session management

**Example:**
```typescript
// tests/e2e/alchemy-flow.test.ts
describe('Alchemy Flow', () => {
  it('completes full alchemy cycle', async () => {
    // 1. Select 2+ shows
    // 2. Generate concepts
    // 3. Select concepts
    // 4. Get recommendations
    // 5. Verify 6 recs with concept references in reasons
  });
});
```

### Visual Tests

**Tool:** Percy or Chromatic

**Critical Screens:**
- Collection Home (all status filters)
- Show Detail (all sections)
- Ask chat (messages, mentioned shows)
- Alchemy mode (concept selection, results)

---

## 13. Quality Assurance

### Discovery Quality Bar

**Dimensions (score 0-2 each, passing ≥7/10):**

1. **Voice Adherence** (≥1)
   - Feels like same persona as Scoop/Ask
   - Warm, playful, opinionated
   - Spoiler-safe by default
   - No generic filler

2. **Taste Alignment** (≥1)
   - Recs grounded in concepts/user library
   - Reasons cite specific shared ingredients
   - User says "yeah, that tracks"

3. **Surprise Without Betrayal**
   - 1-2 recs pleasantly unexpected but defensible

4. **Specificity of Reasoning**
   - Each rec has concrete "because" tied to concepts/vibe/structure

5. **Real-Show Integrity** (=2, non-negotiable)
   - Every recommendation maps to real catalog item
   - No hallucinated titles or wrong IDs

### Surface-Specific Minimums

- **Scoop**: Sections present, balanced, "The Scoop" paragraph centerpiece, honest about mixed reviews
- **Ask**: Direct answer in first 3-5 lines, bulleted lists for multi-recs, confident picks
- **Concepts**: 8 concepts, 1-3 words, evocative, no generic placeholders
- **Explore Similar**: 5 recs, each reason names which concept(s) it matches
- **Alchemy**: 6 recs, each reason names which concept(s) it matches

---

## 14. Infrastructure & Execution

### Dev Identity Injection (Benchmark Mode)

```typescript
// src/lib/identity.ts
export function getCurrentUserId(): string {
  // Dev mode: use injected identity
  if (typeof window !== 'undefined') {
    return window.__DEV_USER_ID__ || process.env.NEXT_PUBLIC_DEV_USER_ID || 'default-user';
  }
  return process.env.NEXT_PUBLIC_DEV_USER_ID || 'default-user';
}

export function getNamespaceId(): string {
  return process.env.NEXT_PUBLIC_DEV_NAMESPACE_ID || 'benchmark-namespace';
}
```

**API Route Middleware:**
```typescript
// src/pages/api/middleware.ts
export function withIdentity(handler) {
  return async (req, res) => {
    const userId = req.headers['x-user-id'] as string || process.env.NEXT_PUBLIC_DEV_USER_ID;
    const namespaceId = req.headers['x-namespace-id'] as string || process.env.NEXT_PUBLIC_DEV_NAMESPACE_ID;
    
    // Set for RLS
    await supabase.rpc('set_current_user', { user_id: userId });
    
    // Attach to context
    req.userId = userId;
    req.namespaceId = namespaceId;
    
    return handler(req, res);
  };
}
```

### Destructive Testing Support

```sql
-- Reset test data within namespace (no global teardown)
CREATE FUNCTION reset_namespace_test_data(namespace_uuid UUID) RETURNS VOID AS $$
BEGIN
  DELETE FROM ai_scoop WHERE namespace_id = namespace_uuid;
  DELETE FROM concepts WHERE namespace_id = namespace_uuid;
  DELETE FROM my_data WHERE namespace_id = namespace_uuid;
  DELETE FROM sessions WHERE namespace_id = namespace_uuid;
  -- Note: shows table is shared catalog, not deleted
END;
$$ LANGUAGE plpgsql;
```

### One-Command Scripts

**package.json:**
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:reset": "jest --clearCache && jest",
    "db:migrate": "supabase db push",
    "db:reset": "supabase db reset"
  }
}
```

---

## 15. Build & Deploy

### Build Process

1. **Setup:**
   ```bash
   cp .env.example .env.local
   pnpm install
   ```

2. **Database:**
   ```bash
   pnpm db:migrate
   ```

3. **Development:**
   ```bash
   pnpm dev
   ```

4. **Production Build:**
   ```bash
   pnpm build
   pnpm start
   ```

### Supabase Setup

1. Create new Supabase project
2. Run migration: `supabase/migrations/001_initial_schema.sql`
3. Configure RLS policies (included in migration)
4. Set environment variables in hosting platform

### Hosting Options

- **Vercel:** Recommended for Next.js (automatic deployments)
- **Netlify:** Alternative with good Supabase integration
- **Docker:** Optional for self-hosting (not required for benchmark)

---

## 16. Open Questions & Decisions

### Decisions Made

1. **Status System:** "Later" with Interest subtypes (Interested/Excited)
2. **Auto-Save:** Setting status, choosing interest, rating unsaved show, adding tag to unsaved show all trigger auto-save
3. **Scoop Persistence:** Only persists long-term if show in collection
4. **Cache Strategy:** Client cache is disposable; server is source of truth
5. **Namespace:** Build isolation, not user-facing concept

### Open Questions (For Future)

1. Should "Next" become first-class status (separate from "Active")?
2. Should we support named custom lists beyond tags?
3. Should AI Scoop on unsaved show auto-save when generated?
4. Import/Restore from export zip - out of scope for v1
5. Explicit myStatus filters in sidebar - not surfaced in v1

---

## 17. Implementation Phases

### Phase 1: Foundation (Week 1-2)

- [ ] Project setup (Next.js, Supabase, TypeScript config)
- [ ] Database schema & migrations
- [ ] Environment setup (.env.example, .gitignore)
- [ ] Shared components (Button, Chip, Dialog, Input, ProgressBar)
- [ ] Global hooks (useDebounce, useLocalStorage, useFetch)
- [ ] Utils (date, formatting, validation, showListParser)
- [ ] Theme tokens (colors, spacing, typography)

### Phase 2: Core Data Flow (Week 2-3)

- [ ] API routes: GET/POST/DELETE /api/collection
- [ ] API routes: GET /api/shows/:id
- [ ] Collection Home page with status filters
- [ ] ShowCard component
- [ ] Toolbar components (StatusChips, RatingBar, TagsInput)
- [ ] Auto-save triggers implementation

### Phase 3: Show Detail (Week 3-4)

- [ ] Show Detail page structure
- [ ] HeaderCarousel with MediaPlayer
- [ ] OverviewSection with ScoopToggle
- [ ] ScoopSection with progressive streaming
- [ ] TraditionalRecommendations strand
- [ ] ProvidersSection

### Phase 4: AI Integration (Week 4-5)

- [ ] AI prompting system (shared + surface-specific prompts)
- [ ] POST /api/shows/:id/scoop endpoint
- [ ] useScoop hook with cache & freshness logic
- [ ] POST /api/ask endpoint with session management
- [ ] AskMode components (ChatInput, ChatMessage, MentionedShows)
- [ ] parseShowList / formatShowList integration

### Phase 5: Discovery (Week 5-6)

- [ ] POST /api/concepts endpoint
- [ ] POST /api/explore-similar endpoint
- [ ] POST /api/alchemy endpoint
- [ ] SearchMode (basic search)
- [ ] AlchemyMode (2+ shows → concepts → recs)
- [ ] ExploreSimilar components (GetConceptsCTA, ConceptChips, ExploreResults)

### Phase 6: Polish & Testing (Week 6-7)

- [ ] Person Detail page
- [ ] Settings page
- [ ] Unit tests (utils, hooks, components)
- [ ] Integration tests (API flows)
- [ ] Visual tests (critical screens)
- [ ] Quality bar validation (AI outputs)
- [ ] Destructive testing support
- [ ] Documentation (.env.example comments, inline docs)

### Phase 7: QA & Hardening (Week 7-8)

- [ ] Lint & typecheck passes
- [ ] Performance optimization (lazy loading, memoization)
- [ ] Error handling & edge cases
- [ ] Accessibility audit
- [ ] Mobile responsiveness check
- [ ] Final quality bar assessment
- [ ] Documentation finalization

---

## 18. Success Criteria

### Functional

- [ ] Collection CRUD operations work correctly
- [ ] Auto-save triggers fire on correct actions
- [ ] Scoop generates, caches (4hrs), and persists correctly
- [ ] Ask chat maintains session context
- [ ] Mentioned shows strip parses structured format correctly
- [ ] Concept generation produces 8 specific, evocative concepts
- [ ] Explore Similar returns 5 recs with concept references
- [ ] Alchemy returns 6 recs with concept references
- [ ] All recommendations map to real catalog items

### Quality

- [ ] Voice adherence score ≥1
- [ ] Taste alignment score ≥1
- [ ] Real-show integrity score =2
- [ ] Total quality score ≥7/10
- [ ] No hallucinated recommendations
- [ ] All AI outputs feel like consistent persona

### Technical

- [ ] .env.example exists with all variables documented
- [ ] All user records scoped to user_id
- [ ] Namespace isolation works
- [ ] Destructive tests scoped to namespace
- [ ] Lint passes (no errors, warnings ≤ threshold)
- [ ] Typecheck passes (no errors)
- [ ] Tests pass (unit + integration)
- [ ] OAuth migration path exists without schema changes

---

## 19. Appendix: File Structure Summary

```
shows_prd_benchmarker_tmp/
├── .env.example
├── .gitignore
├── package.json
├── tsconfig.json
├── next.config.js
├── README.md
├── AGENTS.md
├── docs/
│   └── prd/
│       ├── showbiz_prd.md
│       ├── showbiz_infra_rider_prd.md
│       └── supporting_docs/
├── supabase/
│   └── migrations/
│       └── 001_initial_schema.sql
├── src/
│   ├── config/
│   │   ├── env.ts
│   │   ├── constants.ts
│   │   └── features.ts
│   ├── theme/
│   │   ├── tokens.ts
│   │   ├── colors.ts
│   │   ├── typography.ts
│   │   └── index.ts
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   └── icons/
│   ├── hooks/
│   │   ├── useDebounce.ts
│   │   ├── useLocalStorage.ts
│   │   ├── useMediaQuery.ts
│   │   ├── useSessionStorage.ts
│   │   └── useFetch.ts
│   ├── utils/
│   │   ├── date.ts
│   │   ├── formatting.ts
│   │   ├── validation.ts
│   │   ├── showListParser.ts
│   │   └── testUtils.ts
│   ├── lib/
│   │   ├── supabase.ts
│   │   ├── identity.ts
│   │   └── ai/
│   │       ├── prompts.ts
│   │       └── client.ts
│   ├── pages/
│   │   ├── _app.tsx
│   │   ├── _document.tsx
│   │   ├── index.tsx
│   │   ├── search.tsx
│   │   ├── shows/
│   │   │   └── [id].tsx
│   │   ├── persons/
│   │   │   └── [id].tsx
│   │   ├── settings.tsx
│   │   └── api/
│   ├── styles/
│   │   └── globals.css
│   └── types/
│       └── index.ts
└── tests/
    ├── unit/
    ├── integration/
    └── e2e/
```

---

## 20. Appendix: Type Definitions

```typescript
// src/types/index.ts

type Status = 'active' | 'later' | 'wait' | 'done' | 'quit';
type Interest = 'interested' | 'excited';
type MediaKind = 'movie' | 'tv';

interface Show {
  id: string;
  external_id: string;
  title: string;
  overview?: string;
  year?: number;
  runtime?: number;
  genres: string[];
  language?: string;
  poster_url?: string;
  backdrop_url?: string;
  logo_url?: string;
  imdb_rating?: number;
  seasons?: TVSeason[];
  cast?: CastMember[];
  crew?: CrewMember[];
  providers?: StreamingProvider[];
  budget?: number;
  revenue?: number;
}

interface TVSeason {
  season_number: number;
  episode_count: number;
  name: string;
  overview?: string;
  poster_url?: string;
  air_date?: string;
}

interface CastMember {
  id: string;
  name: string;
  character: string;
  profile_url?: string;
  order?: number;
}

interface CrewMember {
  id: string;
  name: string;
  job: string;
  profile_url?: string;
}

interface StreamingProvider {
  id: string;
  name: string;
  logo_url?: string;
  web_url?: string;
}

interface MyData {
  id: string;
  namespace_id: string;
  user_id: string;
  show_id: string;
  status: Status;
  interest: Interest | null;
  rating: number | null;
  tags: string[];
  scoop_id: string | null;
  created_at: string;
  updated_at: string;
  scoop_updated_at: string | null;
}

interface AI_Scoop {
  id: string;
  namespace_id: string;
  user_id: string;
  show_id: string;
  content: string;
  model_version: string;
  created_at: string;
}

interface Concept {
  id: string;
  namespace_id: string;
  user_id: string;
  show_id: string | null;
  label: string;
  axis: 'structure' | 'vibe' | 'emotion' | 'dynamics' | 'craft' | 'genre-flavor';
  created_at: string;
}

interface Recommendation {
  show: Show;
  reason: string;
}

interface ChatMessage {
  role: 'user' | 'assistant';
  content: string;
  show_refs?: Array<{ title: string; externalId: string; mediaType: MediaKind }>;
  timestamp: string;
}

interface Session {
  id: string;
  namespace_id: string;
  user_id: string;
  title: string;
  messages: ChatMessage[];
  created_at: string;
  updated_at: string;
}

interface AskRequest {
  query: string;
  sessionId?: string;
  context?: { showId?: string };
}

interface AskResponse {
  commentary: string;
  showList?: string; // "Title::externalId::mediaType;;..."
}

interface ConceptsRequest {
  showIds: string[];
  count?: number;
}

interface ExploreSimilarRequest {
  showId: string;
  selectedConcepts: string[];
}

interface AlchemyRequest {
  showIds: string[];
  selectedConcepts: string[];
}
```

---

**Plan Complete.** This implementation plan covers all requirements from the PRD and supporting documents, with clear structure, data model, API contracts, feature breakdown, and quality criteria.