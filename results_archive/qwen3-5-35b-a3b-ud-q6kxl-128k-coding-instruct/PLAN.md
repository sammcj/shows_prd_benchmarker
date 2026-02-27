# Showbiz Implementation Plan

## Overview

Build a Next.js + Supabase application for personal entertainment tracking with AI-powered discovery. The app manages a library of TV shows and movies, allowing users to track status, rate, tag, and receive personalized recommendations through concept-based AI discovery.

## Architecture

### Tech Stack
- **Framework**: Next.js 14 (App Router)
- **Runtime**: Node.js server + React server components
- **Database**: Supabase (PostgreSQL)
- **ORM**: Prisma (migration tooling)
- **AI**: OpenAI-compatible API (configurable provider)

### Directory Structure
```
src/
├── config/           # Environment variables, constants
├── theme/            # Design tokens
├── components/       # Shared UI primitives
├── hooks/            # Global hooks
├── utils/            # Pure functions
├── lib/              # Database client, AI clients
├── db/               # Prisma schema, migrations
├── pages/
│   └── Explore/
│   └── Detail/
│   └── Search/
│   └── Alchemy/
│   └── Ask/
│   └── Profile/
└── app/              # Next.js routes
    ├── api/          # API routes
    │   ├── auth/
    │   ├── shows/
    │   ├── ai/
    │   └── settings/
```

## Database Schema

### Core Tables

**profiles**
- id (uuid, primary key) - opaque user identifier
- namespace_id (uuid, indexed) - build isolation
- display_name (text)
- created_at, updated_at

**shows**
- id (text, primary key) - external catalog ID
- user_id (uuid, indexed)
- namespace_id (uuid, indexed)
- title (text, required)
- show_type (enum: movie, tv, person, unknown)
- overview (text)
- genres (text[])
- tagline (text)
- homepage (text)
- original_language (text)
- spoken_languages (text[])
- poster_url (text)
- backdrop_url (text)
- logo_url (text)
- vote_average (float)
- vote_count (int)
- popularity (float)
- release_date (date)
- first_air_date (date)
- last_air_date (date)
- runtime (int) - movies
- number_of_seasons (int) - TV
- number_of_episodes (int) - TV
- status (text) - series status
- budget (int) - movies
- revenue (int) - movies
- creation_date (timestamp)
- details_update_date (timestamp)
- is_test (boolean, default false)

**my_data**
- id (uuid, primary key)
- show_id (text, indexed)
- user_id (uuid, indexed)
- namespace_id (uuid, indexed)
- my_status (enum: active, later, wait, done, quit)
- my_status_update_date (timestamp)
- my_interest (enum: interested, excited)
- my_interest_update_date (timestamp)
- my_tags (text[])
- my_tags_update_date (timestamp)
- my_score (float)
- my_score_update_date (timestamp)

**ai_scoop**
- id (uuid, primary key)
- show_id (text, indexed)
- user_id (uuid, indexed)
- namespace_id (uuid, indexed)
- content (text)
- created_at (timestamp)
- expires_at (timestamp)

**settings**
- id (text, primary key) - default: "globalSettings"
- namespace_id (uuid, indexed)
- user_name (text)
- ai_api_key (text)
- ai_model (text)
- catalog_api_key (text)
- version (timestamp)

**app_metadata**
- id (text, primary key) - default: "metadata"
- data_model_version (int, default 3)

### Migrations

1. Create profiles table with namespace isolation
2. Create shows table with full catalog schema
3. Create my_data table for user overlays
4. Create ai_scoop table with TTL logic
5. Create settings and metadata tables
6. Add indexes on (namespace_id, user_id) combinations

## API Routes

### Identity (Dev Mode)
- `POST /api/auth/login` - Accepts body `{ userId?: string, namespaceId?: string }`
  - Returns user session with dev identity injection
  - If no userId provided, generates deterministic UUID from namespace

### Shows
- `GET /api/shows?id=` - Fetch show by external ID
- `POST /api/shows` - Create/show merge (catalog + user data)
- `GET /api/shows/library` - Fetch user's library with filters
- `PUT /api/shows/:id/status` - Update status/interest
- `PUT /api/shows/:id/tags` - Update tags
- `PUT /api/shows/:id/score` - Update rating
- `GET /api/shows/search?q=` - Catalog search
- `GET /api/shows/:id/recommendations` - Traditional recommendations

### AI
- `POST /api/ai/scoop` - Generate scoop for show
  - Caches for 4 hours, only if user has show in collection
  - Returns structured content with sections
- `POST /api/ai/ask` - Conversational chat
  - Maintains conversation context with summarization
  - Extracts show mentions in structured format
- `POST /api/ai/concepts` - Generate concepts
  - Single show or multi-show (Alchemy)
  - Returns 8 bullet concepts, 1-3 words each
- `POST /api/ai/recommendations` - Concept-based recs
  - Takes selected concepts + optional library context
  - Returns 5 (Explore Similar) or 6 (Alchemy) recommendations

### Settings
- `GET /api/settings` - Fetch user settings
- `PUT /api/settings` - Update settings
- `POST /api/settings/export` - Export library as JSON zip
- `POST /api/settings/import` - Import from JSON (post-benchmark)

### Metadata
- `GET /api/metadata/version` - Return data model version

## Feature Implementation

### 1. Exploration Page (Explore)
**File**: `src/pages/Explore/Explore.tsx`

**Layout**:
- Top: Filter bar (genre, decade, my status, community score)
- Main: Grid of show tiles
- Each tile: poster, title, year, community score, my status chip

**Hooks**:
- `useExploreFilters` - manages filter state, URL sync
- `useLibrary` - fetches library with filter application
- `useShowTile` - hover actions, quick status update

**Sub-Features**:
- `FilterBar/FilterBar.tsx` - genre/decade/status filters
- `ShowGrid/ShowGrid.tsx` - responsive grid layout
- `ShowTile/ShowTile.tsx` - individual show card

### 2. Detail Page
**File**: `src/pages/Detail/Detail.tsx`

**Sections (in order)**:
1. Header carousel (backdrop/poster/logo/trailer)
2. Core facts row (year, runtime/seasons, community score)
3. Tag chips (my tags)
4. Overview + Scoop toggle
5. Genres + languages
6. Traditional recommendations strand
7. Explore Similar (concepts → recs)
8. Streaming providers
9. Cast, Crew
10. Seasons (TV only)
11. Budget/Revenue (movies)

**Toolbar** (fixed, above scroll):
- Status chips: Active, Later, Wait, Done, Quit
- Interest chips: Interested, Excited
- Rating bar (0-10)

**Hooks**:
- `useDetailShow` - fetches show + merges my_data + ai_scoop
- `useDetailActions` - status/interest/rating/tag handlers
- `useScoop` - fetch/generate scoop with cache invalidation
- `useConcepts` - generate and select concepts for Explore Similar

**Sub-Features**:
- `HeaderCarousel/HeaderCarousel.tsx`
- `OverviewSection/OverviewSection.tsx`
- `ScoopSection/ScoopSection.tsx`
- `ExploreSimilar/ExploreSimilar.tsx`
- `ProvidersSection/ProvidersSection.tsx`
- `CastCrewSection/CastCrewSection.tsx`

### 3. Search Page
**File**: `src/pages/Search/Search.tsx`

**Layout**:
- Search input (autosuggest on catalog)
- Results grid (similar to Explore)
- Quick filters

**Hooks**:
- `useSearch` - debounced catalog search
- `useCatalogFetch` - fetches show details from catalog

### 4. Ask Page (AI Chat)
**File**: `src/pages/Ask/Ask.tsx`

**Layout**:
- Chat messages (user + AI)
- Input field with suggestions
- "Mentioned Shows" strip (auto-extracted from conversation)

**Hooks**:
- `useConversation` - manages chat state, context summarization
- `useAskAI` - streams AI response, extracts mentions
- `useShowMentions` - parses structured mentions from AI output

**AI Contract**:
- Output format: `{ commentary: string, showList: string }`
- showList format: `Title::externalId::mediaType;;...`
- Fallback: unstructured + Search handoff

### 5. Alchemy Page
**File**: `src/pages/Alchemy/Alchemy.tsx`

**Flow**:
1. User selects 2+ shows from library
2. Click "Conceptualize Shows"
3. AI generates concepts (shared across all shows)
4. User selects up to 8 concepts
5. Click "Explore Shows"
6. AI returns 6 recommendations with concept-based reasoning

**Hooks**:
- `useAlchemySelection` - manages show selection state
- `useAlchemyConcepts` - generates and selects concepts
- `useAlchemyRecs` - fetches concept-based recommendations

### 6. Profile Page
**File**: `src/pages/Profile/Profile.tsx`

**Sections**:
- User settings (display name, AI model, API keys)
- Library stats (total shows, by status, by decade)
- Export data (JSON zip)
- Reset test data (namespace cleanup)

**Hooks**:
- `useProfileSettings` - manages settings CRUD
- `useLibraryStats` - computes library analytics

## AI Implementation

### Voice & Style Rules
- Warm, playful, opinionated
- Spoiler-safe by default
- Specific over generic (no "good characters")
- Actionable recommendations

### Scoop Generation
**Prompt Structure**:
```
You are a trusted friend giving a taste review. 
Structure:
1. Personal take (your immediate reaction)
2. Honest stack-up (what works, what doesn't)
3. The Scoop (emotional centerpiece - why watch?)
4. Fit/warnings (who should skip, who will love)
5. Verdict (one-line recommendation)

Rules:
- No spoilers
- Be opinionated, not encyclopedic
- 300-500 words total
- Acknowledge mixed reception if relevant
```

**Cache Policy**:
- Expires after 4 hours
- Only persists if show in user collection
- Regenerate on demand

### Ask Chat
**Context Building**:
- Last 5-7 turns full context
- Older turns summarized to 1-2 sentences
- Include user library summary (top genres, status distribution)
- Seed with current show context if applicable

**Mention Extraction**:
- Parse AI response for show references
- Format: `Title::externalId::mediaType;;...`
- UI renders clickable mentions strip

### Concepts
**Single Show**:
```
Generate 8 concepts capturing the core feeling of [show].

Rules:
- 1-3 words each
- Evocative, not generic
- Cover different axes: structure, vibe, emotion, craft
- Order by strength (best first)
- Bullet list only, no explanation
```

**Multi-Show (Alchemy)**:
```
Generate concepts shared across these shows: [shows].

Rules:
- Must represent commonality across ALL inputs
- 10-12 concepts (larger pool for selection)
- Same quality rules as single-show
```

### Recommendations
**Concept-Based**:
```
Recommend [5/6] shows based on these concepts: [selected concepts].

For each:
- Return: title, externalId, mediaType, reason
- Reason must explicitly reference which concept(s) it matches
- Bias toward recent but allow classics
- Must resolve to real catalog items
```

## Data Merge Strategy

### Catalog → Show Merge
**Non-My Fields** (`selectFirstNonEmpty`):
- Never overwrite non-empty stored value with empty/nil
- Prefer catalog if stored is empty
- Example: if stored.overview is "existing" and catalog.overview is null, keep "existing"

**My Fields** (timestamp-based):
- Compare update dates
- Keep newer timestamp
- If only one has timestamp, keep that
- Preserves user edits across catalog refreshes

**Timestamps**:
- `creation_date`: set once on first create
- `details_update_date`: set on every catalog merge
- `my_*_update_date`: set on every user edit

## Namespace Isolation

### Model
- Each build gets unique `namespace_id` (UUID)
- All user records have `user_id`
- Partition key: `(namespace_id, user_id)`
- Two namespaces never see each other's data

### Dev Identity Injection
**Development Mode**:
- Accept `X-User-Id` header in API routes
- If header present, use that as user_id
- Otherwise, generate from namespace or use default

**Production Mode**:
- Header injection disabled
- Real OAuth required (migration path)

### Reset Test Data
```
DELETE FROM my_data WHERE namespace_id = $1;
DELETE FROM ai_scoop WHERE namespace_id = $1;
DELETE FROM shows WHERE namespace_id = $1;
-- Keep profiles and settings
```

## Environment Variables

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=server-only-key

# AI
AI_PROVIDER=openai
AI_API_KEY=your-ai-key
AI_MODEL=gpt-4o

# Catalog API (TMDB or similar)
CATALOG_API_KEY=your-catalog-key

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## Scripts

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "test": "jest",
    "test:reset": "prisma migrate reset --force",
    "db:migrate": "prisma migrate dev",
    "db:push": "prisma db push",
    "db:seed": "ts-node prisma/seed.ts",
    "db:generate": "prisma generate"
  }
}
```

## Testing Strategy

### Unit Tests
- Merge logic (selectFirstNonEmpty, timestamp resolution)
- Concept generation quality (specificity checks)
- Mention parsing (structured format validation)
- Filter application logic

### Integration Tests
- API routes (auth, shows, AI, settings)
- Database CRUD operations
- Namespace isolation verification

### Visual Tests
- Explore page layout
- Detail page sections
- AI chat interface
- Alchemy flow states

## Migration Path

### Phase 1: Core Infrastructure
1. Set up Next.js + Supabase + Prisma
2. Implement database migrations
3. Build identity/namespace system
4. Create basic API routes

### Phase 2: Library Management
1. Show CRUD + catalog merge
2. My data (status, interest, tags, rating)
3. Explore page with filters
4. Detail page (basic)

### Phase 3: AI Integration
1. Scoop generation + caching
2. Ask chat with mentions
3. Concept generation
4. Recommendation engine

### Phase 4: Discovery Features
1. Alchemy page (full flow)
2. Search page
3. Profile/settings page
4. Export data

### Phase 5: Polish
1. Visual testing
2. Performance optimization
3. Error handling
4. Documentation

## Open Questions

1. **AI Scoop on unsaved show**: Should generating scoop auto-save show as Later?
2. **Next status**: Should "Next" be first-class in UI or just a tag?
3. **Catalog source**: TMDB vs other providers - need vendor-agnostic catalog layer
4. **Import/restore**: Out of scope for benchmark, but design schema to support later

## Success Criteria

- [ ] All AI surfaces produce on-brand output (voice, specificity, real shows)
- [ ] Namespace isolation prevents data collisions
- [ ] Catalog merge preserves user data correctly
- [ ] All CRUD operations work via API
- [ ] One-command dev experience (`npm run dev`)
- [ ] Export data as JSON zip
- [ ] Reset test data without global teardown
- [ ] Migration path to real OAuth (no schema changes needed)