# Implementation Plan: Showbiz Benchmark

## Executive Summary

Build a Next.js + Supabase application for TV/movie collection and AI-powered discovery. The system tracks user collections (status, tags, ratings), generates AI scoops, and enables concept-based recommendations through Alchemy and Explore Similar features.

**Benchmark constraints:** Next.js latest stable, Supabase persistence, namespace isolation, dev-mode identity injection, no Docker required.

---

## Architecture Overview

### Technology Stack
- **Runtime:** Next.js 15 (App Router)
- **Database:** Supabase (PostgreSQL)
- **ORM:** Supabase JS client with typed queries
- **Styling:** Tailwind CSS + theme tokens
- **State:** React Query for server state, Zustand for client state

### Directory Structure

```
src/
├── config/
│   ├── env.ts              # Environment validation
│   └── constants.ts        # App-wide constants
├── theme/
│   ├── tokens.ts           # Design tokens (colors, spacing)
│   └── index.ts            # Theme exports
├── components/
│   ├── ui/                 # Shared primitives (Button, Input, Chip)
│   └── layout/             # Layout components (Header, Footer)
├── hooks/
│   ├── useAuth.ts          # Auth state (dev identity injection)
│   └── useNamespace.ts     # Namespace management
├── utils/
│   ├── merge.ts            # Show merge logic
│   ├── ai.ts               # AI prompting utilities
│   └── formatter.ts        # Date, runtime formatters
├── pages/
│   ├── Home/
│   │   ├── Home.tsx
│   │   └── features/
│   │       ├── LibrarySection/
│   │       └── StatusFilter/
│   ├── Search/
│   │   └── Search.tsx
│   ├── Ask/
│   │   └── Ask.tsx
│   ├── Alchemy/
│   │   └── Alchemy.tsx
│   ├── ShowDetail/
│   │   └── ShowDetail.tsx
│   └── Settings/
│       └── Settings.tsx
├── lib/
│   ├── supabase.ts         # Supabase client
│   └── api/                # Server routes
│       ├── ai/
│       │   ├── scoop.ts
│       │   ├── ask.ts
│       │   └── concepts.ts
│       └── shows/
│           ├── search.ts
│           └── recommendations.ts
└── types/
    ├── show.ts
    ├── user.ts
    └── ai.ts
```

---

## Phase 1: Foundation (Days 1-2)

### 1.1 Project Setup
- Initialize Next.js 15 with TypeScript, Tailwind
- Configure ESLint, Prettier
- Set up directory structure per fractal architecture
- Create .env.example with all required variables

### 1.2 Environment Configuration
```env
# Required
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=

# Optional (dev mode)
DEV_USER_ID=                      # Dev identity injection
NAMESPACE_ID=                     # Build/run namespace

# AI (optional)
AI_API_KEY=                       # For AI features
AI_MODEL=                         # e.g., "claude-3-haiku-20240307"
```

### 1.3 Database Schema (Supabase)

**Tables:**

```sql
-- Shows table
CREATE TABLE shows (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  show_type TEXT NOT NULL,  -- 'movie' | 'tv' | 'person' | 'unknown'
  overview TEXT,
  genres TEXT[],
  poster_url TEXT,
  backdrop_url TEXT,
  logo_url TEXT,
  vote_average FLOAT,
  vote_count INT,
  popularity FLOAT,
  release_date DATE,
  first_air_date DATE,
  last_air_date DATE,
  runtime INT,
  budget INT,
  revenue INT,
  number_of_seasons INT,
  number_of_episodes INT,
  series_status TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  is_test BOOLEAN DEFAULT FALSE,
  namespace_id TEXT NOT NULL,
  user_id TEXT NOT NULL,
  
  -- User-specific fields
  my_status TEXT,
  my_status_update_date TIMESTAMPTZ,
  my_interest TEXT,
  my_interest_update_date TIMESTAMPTZ,
  my_tags TEXT[],
  my_tags_update_date TIMESTAMPTZ,
  my_score FLOAT,
  my_score_update_date TIMESTAMPTZ,
  ai_scoop TEXT,
  ai_scoop_update_date TIMESTAMPTZ,
  
  -- Constraints
  UNIQUE(namespace_id, external_id)
);

-- Indexes
CREATE INDEX idx_shows_namespace_user ON shows(namespace_id, user_id);
CREATE INDEX idx_shows_namespace_type ON shows(namespace_id, show_type);
CREATE INDEX idx_shows_my_status ON shows(namespace_id, my_status) WHERE my_status IS NOT NULL;

-- Cloud settings (optional, for synced preferences)
CREATE TABLE cloud_settings (
  id TEXT PRIMARY KEY DEFAULT 'globalSettings',
  user_name TEXT NOT NULL,
  version BIGINT NOT NULL,
  ai_model TEXT,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- App metadata
CREATE TABLE app_metadata (
  key TEXT PRIMARY KEY,
  value INT NOT NULL,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Insert defaults
INSERT INTO app_metadata (key, value) VALUES ('dataModelVersion', 3);
```

### 1.4 Supabase Client Setup
- Create singleton client in `lib/supabase.ts`
- Configure RLS policies for namespace/user isolation
- Set up type generation from schema

### 1.5 Dev Identity Injection
- Server route middleware accepts `X-User-Id` header in development
- Fallback to `DEV_USER_ID` env var
- Middleware sets `userId` context for all API calls
- Document clearly: disabled in production

---

## Phase 2: Core Data Layer (Days 3-4)

### 2.1 Type Definitions
- Implement all types from storage-schema.md
- Define Show, ProviderData, CloudSettings, AppMetadata
- Add enums: ShowType, MyStatusType, MyInterestType

### 2.2 Show Merge Logic
- Implement `selectFirstNonEmpty()` for catalog fields
- Implement timestamp-based conflict resolution for user fields
- Create `mergeCatalogIntoShow()` function
- Test with edge cases (empty arrays, null values, timestamps)

### 2.3 Repository Layer
- `ShowRepository` with methods:
  - `getById(namespaceId, userId, showId)`
  - `search(namespaceId, userId, query)`
  - `upsert(namespaceId, userId, show)`
  - `getByStatus(namespaceId, userId, status)`
  - `deleteByNamespace(namespaceId)` -- for test reset
- `AiRepository` with methods:
  - `getScoop(namespaceId, userId, showId)`
  - `saveScoop(namespaceId, userId, showId, scoop, timestamp)`

### 2.4 API Routes
- `POST /api/shows/search` - Catalog search
- `GET /api/shows/:id` - Get show details
- `POST /api/shows/merge` - Merge catalog into stored show
- `POST /api/shows/:id/status` - Update status/interest
- `POST /api/shows/:id/tags` - Update tags
- `POST /api/shows/:id/score` - Update rating

---

## Phase 3: AI Integration (Days 5-7)

### 3.1 AI Voice & Personality
- Document persona: friendly TV/movie nerd, opinionated, spoiler-safe
- Create prompt templates per surface:
  - **Scoop:** 150-350 words, mini blog-post of taste
  - **Ask:** Conversational, 1-3 paragraphs + lists
  - **Concepts:** Bullet list, 1-3 words, evocative
  - **Recommendations:** 1-3 sentence reasons per show

### 3.2 AI Routes
- `POST /api/ai/scoop` - Generate scoop for show
  - Cache: 4 hours, only persist if show in collection
  - Stream response if supported
- `POST /api/ai/ask` - Chat discovery
  - Maintain conversation history (summarize old turns)
  - Parse structured output for "mentioned shows"
- `POST /api/ai/concepts` - Generate concepts
  - Single-show or multi-show (shared concepts)
  - Return 8 concepts by default
- `POST /api/ai/recommendations` - Concept-based recs
  - 5 recs (Explore Similar) or 6 recs (Alchemy)
  - Include reasons tied to selected concepts

### 3.3 Prompt Engineering
- Shared rules: TV/movie only, spoiler-safe, opinionated
- Context injection: user library, current show, concepts
- Fallback strategy: retry with stricter formatting, then handoff to Search
- Guardrails: redirect if asked to leave TV/movie domain

---

## Phase 4: Pages & Features (Days 8-14)

### 4.1 Home Page (`/`)
**Features:**
- `LibrarySection`: Shows grouped by status
  - Active, Excited, Interested, Other groups
  - Show cards with poster, title, status chip
- `StatusFilter`: Filter library by status
  - Chip selector for each status
  - Persist last selected filter in localStorage

**Hooks:**
- `useLibrary`: Fetch shows by status with namespace/user scope

### 4.2 Search Page (`/search`)
**Features:**
- Search input with debounced query
- Results grid with pagination
- Show cards with basic info + "Add to Collection" button
- Click result → navigate to ShowDetail

**Hooks:**
- `useSearch`: Query catalog with namespace/user scope

### 4.3 Ask Page (`/ask`)
**Features:**
- Chat interface with message list
- Input field for user queries
- "Mentioned shows" row (parsed from AI response)
- Conversation history sidebar (collapsible)

**Hooks:**
- `useAsk`: Manage chat state, call AI route
- `parseMentionedShows`: Extract structured show list from response

### 4.4 Alchemy Page (`/alchemy`)
**Features:**
- **Step 1:** Select 2+ shows from library or search
- **Step 2:** Generate concepts from selected shows
- **Step 3:** Select up to 8 concepts
- **Step 4:** Get recommendations based on concepts
- Concept chips with selection state
- Recommendation cards with reasons

**Hooks:**
- `useAlchemy`: Manage multi-step state
- `useConcepts`: Fetch concepts for shows
- `useConceptRecommendations`: Fetch recs by concepts

### 4.5 Show Detail Page (`/shows/:id`)
**Features (narrative hierarchy):**
1. Header carousel (backdrop/poster/logo/trailer)
2. Core facts row (year/runtime + community score)
3. Tag chips (My Tags)
4. Overview text + Scoop toggle
5. "Ask about this show" CTA
6. Genres + languages
7. Traditional recommendations strand
8. Explore Similar (concepts → recs)
9. Providers ("Stream It")
10. Cast, Crew
11. Seasons (TV only)
12. Budget/Revenue (movies)

**Toolbar (sticky):**
- Status chips: Active, Later, Excited, Wait, Done, Quit
- Reselecting triggers removal confirmation
- Rating bar (auto-saves as Done)
- My Score display

**Scoop Toggle:**
- States: "Give me the scoop!" / "Show the scoop"
- Streams progressively
- Freshness: regenerate after 4 hours

**Hooks:**
- `useShow`: Fetch show + merge catalog
- `useScoop`: Get/save scoop with cache check
- `useExploreSimilar`: Fetch concepts + recommendations
- `useStatus`: Update status/interest with conflict resolution

### 4.6 Settings Page (`/settings`)
**Features:**
- App config (autoSearch, fontSize)
- API key management (catalog, AI)
- Data export (JSON download)
- Test controls (reset namespace data)

**Hooks:**
- `useSettings`: Sync settings with localStorage + cloud
- `useExport`: Generate and download data

---

## Phase 5: UI Components (Days 15-17)

### 5.1 Shared Primitives
- **Button:** Primary, secondary, ghost variants
- **Chip:** Selectable, dismissible, status-colored
- **Input:** Text, search, with validation
- **Card:** Show card, recommendation card
- **Modal:** Confirmation dialogs
- **Skeleton:** Loading states
- **Toast:** Success/error notifications

### 5.2 Theme System
- Define color tokens (primary, secondary, status colors)
- Spacing scale (4px base)
- Typography scale (XS to XXL)
- Shadows, borders, radii

### 5.3 Layout Components
- **Header:** Navigation, namespace display
- **Footer:** Simple footer
- **Container:** Centered content with max-width
- **Grid:** Responsive grid for cards

---

## Phase 6: Testing & Quality (Days 18-20)

### 6.1 Unit Tests
- Show merge logic (all edge cases)
- AI prompt formatting
- Type parsers (mentioned shows)
- Timestamp conflict resolution

### 6.2 Integration Tests
- API routes with mocked Supabase
- Dev identity injection flow
- Namespace isolation verification

### 6.3 Visual Tests (if applicable)
- Show Detail page layout
- Alchemy multi-step flow
- Status filter interactions

### 6.4 Golden Set Validation
- Define 3-5 test scenarios for discovery quality
- Validate voice adherence, taste alignment, specificity
- Score using rubric (Voice ≥1, Taste ≥1, Integrity =2, Total ≥7)

---

## Phase 7: Polish & Documentation (Days 21-22)

### 7.1 Error Handling
- Graceful fallbacks for AI failures
- Network error states
- Empty states with helpful copy

### 7.2 Loading States
- Skeleton loaders for all data fetches
- Progressive streaming for AI responses
- Optimistic UI for status updates

### 7.3 Documentation
- Update README with setup instructions
- Document dev identity injection clearly
- Add comments for complex merge logic
- Create .env.example with descriptions

### 7.4 Performance
- Code splitting for routes
- Image optimization
- Debounced search inputs
- Cached API responses

---

## Migration Path to Production

### Authentication
- Replace dev identity injection with Supabase Auth
- Schema unchanged: `user_id` remains opaque string
- Migrate session handling to Supabase client

### Database
- Current schema is production-ready
- Add indexes for query optimization
- Consider partitioning for large namespaces

### AI Provider
- Swap AI provider by changing `AI_API_KEY` and `AI_MODEL`
- Prompt templates remain unchanged
- Add provider-specific error handling

---

## Risk Mitigation

### High Risk
1. **AI response quality**
   - Mitigation: Iterative prompt testing, golden set validation
2. **Merge conflicts**
   - Mitigation: Comprehensive unit tests for all timestamp scenarios
3. **Namespace isolation bugs**
   - Mitigation: Integration tests verifying cross-namespace separation

### Medium Risk
1. **Supabase RLS complexity**
   - Mitigation: Start with simple policies, add complexity incrementally
2. **AI streaming UI**
   - Mitigation: Fallback to full response if streaming fails

---

## Success Criteria

- [ ] All 8 pages implemented with full functionality
- [ ] AI features working (Scoop, Ask, Concepts, Recommendations)
- [ ] Namespace isolation verified
- [ ] Dev identity injection documented and working
- [ ] Test reset command clears namespace data
- [ ] Export/backup functional
- [ ] Lint and typecheck pass
- [ ] No hardcoded secrets or credentials
- [ ] .env.example complete and accurate

---

## Command Scripts

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "typecheck": "tsc --noEmit",
    "test": "jest",
    "test:reset": "supabase db reset --namespace ${NAMESPACE_ID}"
  }
}
```

---

## Timeline Summary

| Phase | Duration | Deliverables |
|-------|----------|--------------|
| 1. Foundation | 2 days | Project setup, schema, dev identity |
| 2. Core Data | 2 days | Types, merge logic, repository, API routes |
| 3. AI Integration | 3 days | AI routes, prompts, caching |
| 4. Pages | 7 days | All 8 pages with features |
| 5. UI Components | 3 days | Primitives, theme, layout |
| 6. Testing | 3 days | Unit, integration, visual tests |
| 7. Polish | 2 days | Error handling, docs, performance |

**Total: 22 days**