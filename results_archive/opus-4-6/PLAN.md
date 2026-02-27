# Implementation Plan: ShowBiz

## Overview

A personal TV/movie companion app for collecting, organising, rating, and discovering entertainment. Users maintain a personal library with statuses, interest levels, tags, and ratings. Discovery is powered by AI (Ask chat, Alchemy concept-blending, Explore Similar) and traditional catalog search. Built with Next.js and Supabase.

---

## Phase 0: Project Scaffolding & Infrastructure

### 0.1 Initialise Next.js Project

- `npx create-next-app@latest` with App Router, TypeScript strict mode, Tailwind CSS, ESLint
- Directory structure following the fractal architecture from CLAUDE.md:

```
src/
  app/                  # Next.js App Router pages
  config/               # Global constants, env validation
  theme/                # Design tokens, Tailwind config extensions
  components/           # Shared UI primitives (ShowTile, StatusChip, RatingBar, etc.)
  hooks/                # Global hooks (useSupabase, useUser, useNamespace)
  utils/                # Global pure functions (merge logic, date helpers)
  lib/                  # External service clients (supabase, tmdb, ai)
  types/                # Shared TypeScript types
  pages/
    HomePage/
    FindPage/
    DetailPage/
    PersonPage/
    SettingsPage/
```

### 0.2 Environment & Configuration

- `.env.example` with all required variables:

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=         # Supabase project URL
NEXT_PUBLIC_SUPABASE_ANON_KEY=    # Supabase anon/public key
SUPABASE_SERVICE_ROLE_KEY=        # Server-only; never exposed to browser

# Namespace / Build Isolation
NEXT_PUBLIC_NAMESPACE_ID=         # Unique per build/run; partitions all data

# Default User (benchmark/dev mode)
NEXT_PUBLIC_DEFAULT_USER_ID=      # Opaque stable string or UUID
NEXT_PUBLIC_DEFAULT_USER_NAME=    # Display name for dev user

# TMDB (Content Catalog)
TMDB_API_KEY=                     # Server-only TMDB v3 API key
NEXT_PUBLIC_TMDB_IMAGE_BASE=https://image.tmdb.org/t/p  # Public image CDN

# AI Provider
AI_API_KEY=                       # Server-only; OpenAI/Anthropic key
AI_MODEL=                         # e.g. gpt-4o, claude-sonnet-4-20250514
AI_BASE_URL=                      # Optional; for OpenAI-compatible endpoints
```

- `src/config/env.ts` validates all required env vars at startup with clear error messages
- `.gitignore` excludes `.env*` except `.env.example`

### 0.3 Developer Experience Scripts

- `npm run dev` - start Next.js dev server
- `npm run build` - production build
- `npm test` - run Vitest test suite
- `npm run test:reset` - reset test data for the configured namespace
- `npm run db:migrate` - apply Supabase migrations
- `npm run db:seed` - seed test data into current namespace
- `npm run lint` - ESLint + type checking

### 0.4 Supabase Client Setup

- `src/lib/supabase/client.ts` - browser client using anon key
- `src/lib/supabase/server.ts` - server client using service role key (for API routes only)
- All queries scoped by `namespace_id` via a helper that wraps `.eq('namespace_id', config.namespaceId)`

---

## Phase 1: Database Schema & Migrations

### 1.1 Core Tables

Migration: `001_initial_schema.sql`

**`shows`** - main entity table

| Column | Type | Notes |
|--------|------|-------|
| `id` | `text` | PK. TMDB ID as string |
| `namespace_id` | `text` | NOT NULL. Build isolation |
| `user_id` | `text` | NOT NULL. User ownership |
| `title` | `text` | NOT NULL |
| `show_type` | `text` | `movie`, `tv`, `person`, `unknown` |
| `external_ids` | `jsonb` | Optional catalog cross-refs |
| `overview` | `text` | |
| `genres` | `text[]` | Genre name strings |
| `tagline` | `text` | |
| `homepage` | `text` | |
| `original_language` | `text` | |
| `spoken_languages` | `text[]` | ISO 639-1 codes |
| `languages` | `text[]` | |
| `poster_url` | `text` | Full URL |
| `backdrop_url` | `text` | Full URL |
| `logo_url` | `text` | Full URL |
| `network_logos` | `text[]` | |
| `vote_average` | `double precision` | |
| `vote_count` | `integer` | |
| `popularity` | `double precision` | |
| `last_air_date` | `timestamptz` | |
| `first_air_date` | `timestamptz` | |
| `release_date` | `timestamptz` | |
| `runtime` | `integer` | Minutes (movies) |
| `budget` | `bigint` | |
| `revenue` | `bigint` | |
| `series_status` | `text` | |
| `number_of_episodes` | `integer` | |
| `number_of_seasons` | `integer` | |
| `episode_run_time` | `integer[]` | |
| `my_tags` | `text[]` | Default `{}` |
| `my_tags_update_date` | `timestamptz` | |
| `my_score` | `double precision` | |
| `my_score_update_date` | `timestamptz` | |
| `my_status` | `text` | CHECK constraint: `active`, `next`, `later`, `done`, `quit`, `wait` |
| `my_interest` | `text` | CHECK constraint: `excited`, `interested` |
| `my_interest_update_date` | `timestamptz` | |
| `my_status_update_date` | `timestamptz` | |
| `ai_scoop` | `text` | |
| `ai_scoop_update_date` | `timestamptz` | |
| `details_update_date` | `timestamptz` | |
| `creation_date` | `timestamptz` | Default `now()` |
| `is_test` | `boolean` | Default `false` |
| `provider_data` | `jsonb` | `{ countries: { AU: { flatrate: [8], rent: [2] } } }` |

Indexes:
- `UNIQUE(namespace_id, user_id, id)` - composite PK
- `idx_shows_namespace_user_status ON (namespace_id, user_id, my_status)`
- `idx_shows_namespace_user_tags ON (namespace_id, user_id) USING gin(my_tags)`

**`cloud_settings`** - per-user synced settings

| Column | Type | Notes |
|--------|------|-------|
| `id` | `text` | Default `'globalSettings'` |
| `namespace_id` | `text` | NOT NULL |
| `user_id` | `text` | NOT NULL |
| `user_name` | `text` | NOT NULL |
| `version` | `double precision` | Epoch seconds for conflict resolution |
| `catalog_api_key` | `text` | |
| `ai_api_key` | `text` | |
| `ai_model` | `text` | |

Constraint: `UNIQUE(namespace_id, user_id, id)`

**`app_metadata`** - data model versioning

| Column | Type | Notes |
|--------|------|-------|
| `namespace_id` | `text` | PK |
| `data_model_version` | `integer` | Default 3 |

### 1.2 Row Level Security

Migration: `002_rls_policies.sql`

- Enable RLS on all tables
- Policies scope reads/writes to matching `namespace_id` and `user_id`
- Service role bypasses RLS for admin/test operations

### 1.3 Test Data Reset

Migration/Script: `003_test_reset_function.sql`

- `DELETE FROM shows WHERE namespace_id = $1 AND is_test = true`
- `DELETE FROM shows WHERE namespace_id = $1` (full reset variant)
- Exposed via `npm run test:reset` calling a Supabase RPC or direct query

---

## Phase 2: Core Data Layer

### 2.1 TypeScript Types

`src/types/show.ts` - mirrors the storage schema TS file:
- `Show`, `ShowType`, `MyStatusType`, `MyInterestType`
- `ProviderData`, `ProviderTypeIdLists`
- `CloudSettings`, `AppMetadata`, `FilterConfiguration`
- `LocalSettings`, `UIState`

### 2.2 Show Repository

`src/lib/repositories/showRepository.ts`

Server-side data access (all queries auto-scoped by namespace + user):
- `getShows(filters?)` - fetch collection with optional status/tag/genre/decade/score filters
- `getShow(showId)` - single show by ID
- `upsertShow(show)` - insert or merge using the merge policy (see 2.3)
- `removeShow(showId)` - delete + clear all My Data
- `updateMyStatus(showId, status, interest?)`
- `updateMyScore(showId, score)`
- `updateMyTags(showId, tags)`
- `updateAiScoop(showId, scoop)`
- `getAllTags()` - distinct tags across user's collection
- `resetNamespace(namespaceId)` - destructive test reset

### 2.3 Merge Logic

`src/utils/mergeShow.ts`

Implements the merge/overwrite policy from the storage schema doc:
- Non-my fields: `selectFirstNonEmpty(newValue, oldValue)` - never overwrite non-empty with empty
- My fields: resolve by timestamp - newer wins
- `details_update_date` set to now after merge
- `creation_date` only set on first creation

Unit tests for merge edge cases:
- New catalog data with empty fields should not clobber existing data
- User edits with newer timestamps win over older synced values
- First creation sets creation_date; subsequent merges do not

### 2.4 Business Rules

`src/utils/collectionRules.ts`

Implements save triggers and defaults from PRD section 5:
- **Save triggers**: setting status, choosing interest, rating unsaved show, adding tag to unsaved show
- **Default values**: save without explicit status -> Later + Interested; save via rating -> Done
- **Removal**: clear status -> remove show + all My Data; requires confirmation
- **Interest relevance**: only applies when status is Later

Unit tests for each rule.

---

## Phase 3: External Service Integrations

### 3.1 TMDB Client

`src/lib/tmdb/client.ts` - server-side only (API key not exposed)

Endpoints:
- `searchShows(query, mediaType?)` - search movies + TV
- `getShowDetails(tmdbId, mediaType)` - full detail with append_to_response (credits, videos, recommendations, similar, watch/providers, images)
- `getPersonDetails(personId)` - person with combined credits
- `getSeasonDetails(tvId, seasonNumber)`
- `getTrending(mediaType, timeWindow)` - for empty state suggestions

`src/lib/tmdb/mapper.ts` - maps TMDB responses to `Show` type:
- Handles field mapping rules from storage-schema.md
- Genre ID -> name mapping
- Image path -> full URL construction
- Date parsing with multiple formats
- Logo selection (best-rated, prefer English)

Next.js API routes (under `src/app/api/tmdb/`):
- `GET /api/tmdb/search?q=&type=` - proxied search
- `GET /api/tmdb/show/[mediaType]/[id]` - proxied detail
- `GET /api/tmdb/person/[id]` - proxied person
- `GET /api/tmdb/show/[mediaType]/[id]/season/[num]` - proxied season

### 3.2 AI Client

`src/lib/ai/client.ts` - server-side, provider-agnostic

- Supports OpenAI-compatible API (covers OpenAI, Anthropic via proxy, local models)
- Configurable via `AI_API_KEY`, `AI_MODEL`, `AI_BASE_URL`
- Falls back to user-configured key/model from CloudSettings if env vars absent
- Streaming support for Scoop generation

`src/lib/ai/prompts/` - prompt templates per surface:
- `askPrompt.ts` - Ask chat system prompt + library context injection
- `askMentionsPrompt.ts` - Ask with structured showList output
- `scoopPrompt.ts` - Scoop generation (structured mini blog-post)
- `conceptsPrompt.ts` - Single-show concept extraction
- `multiConceptsPrompt.ts` - Multi-show concept extraction (Alchemy)
- `conceptRecsPrompt.ts` - Concept-based recommendations
- `summarizePrompt.ts` - Conversation summarisation

All prompts follow the voice/personality spec: warm, opinionated, spoiler-safe, vibe-first, specific.

`src/lib/ai/parser.ts` - structured output parsing:
- `parseShowList(raw)` - parse `Title::externalId::mediaType;;...` format
- `parseConcepts(raw)` - parse bullet list of concepts
- `parseRecommendations(raw)` - parse rec list with reasons
- Retry once on parse failure with stricter formatting instructions
- Fallback to unstructured commentary + search handoff

Next.js API routes (under `src/app/api/ai/`):
- `POST /api/ai/ask` - chat completion (streaming)
- `POST /api/ai/scoop` - scoop generation (streaming)
- `POST /api/ai/concepts` - concept extraction
- `POST /api/ai/recommendations` - concept-based recs

### 3.3 AI Context Builder

`src/lib/ai/context.ts`

Builds taste-aware context for AI prompts:
- Formats user library summary (titles, statuses, ratings, tags)
- Truncates to token budget
- Conversation summarisation for Ask (after ~10 messages)

---

## Phase 4: Theme & Shared Components

### 4.1 Theme System

`src/theme/tokens.ts` - design tokens:
- Colour palette (dark theme primary - entertainment app aesthetic)
- Typography scale matching font size settings (XS through XXL)
- Spacing scale
- Border radii
- Shadows/elevations
- Status colours (Active, Later/Interested, Later/Excited, Wait, Done, Quit)

`src/theme/tailwind.ts` - Tailwind theme extension using tokens

### 4.2 Shared Components

Each component follows humble-component pattern with co-located hooks.

**Display Components:**
- `ShowTile` - poster + title + status badge + rating badge; handles in-collection indicator
- `ShowGrid` - responsive grid of ShowTiles
- `ShowStrip` - horizontal scrollable row of ShowTiles (for recommendations, mentioned shows)
- `PersonCard` - person image + name
- `RatingBar` - community score display bar
- `StatusChipGroup` - row of status/interest chips (Active, Interested, Excited, Wait, Done, Quit)
- `TagChips` - tag display + add/remove
- `ConceptChips` - selectable concept pills
- `MediaTypeToggle` - All / Movies / TV toggle
- `FilterSidebar` - navigation panel with filters

**Interactive Components:**
- `RatingSlider` - user rating input (0-10 scale)
- `TagPicker` - modal/popover for adding tags from library or creating new
- `SearchInput` - debounced text input
- `ConfirmDialog` - reusable confirmation modal (used for status removal)
- `StreamingText` - progressively rendered text (for Scoop/Ask)

---

## Phase 5: Pages & Features

### 5.1 Layout & Navigation

`src/app/layout.tsx` - root layout:
- Sidebar (FilterSidebar) + main content area
- Global navigation: Home, Find/Discover, Settings
- Responsive: sidebar collapses on mobile

`src/app/(main)/layout.tsx` - authenticated layout:
- Injects user context (namespace_id + user_id)
- Dev-mode identity: reads from env or shows dev user selector

### 5.2 Collection Home Page

```
src/pages/HomePage/
  HomePage.tsx
  hooks/useCollectionData.ts
  hooks/useFilterState.ts
  features/
    StatusSection/
      StatusSection.tsx
      hooks/useStatusSection.ts
    EmptyState/
      EmptyState.tsx
```

**Behaviour:**
- Fetches user's collection filtered by current sidebar filter + media type toggle
- Groups shows into status sections:
  1. Active (larger tiles, prominent)
  2. Excited (Later + Excited)
  3. Interested (Later + Interested)
  4. Other (collapsible: Wait, Quit, Done, unclassified Later)
- Empty states: no collection -> CTA to Search/Ask; filter yields none -> "No results found"
- Tiles show poster, title, status badge, rating badge

### 5.3 Find/Discover Page

```
src/pages/FindPage/
  FindPage.tsx
  hooks/useFindMode.ts
  features/
    Search/
      Search.tsx
      hooks/useSearch.ts
    Ask/
      Ask.tsx
      hooks/useAskChat.ts
      hooks/useConversationContext.ts
      features/
        MentionedShows/
          MentionedShows.tsx
        StarterPrompts/
          StarterPrompts.tsx
    Alchemy/
      Alchemy.tsx
      hooks/useAlchemy.ts
      features/
        ShowPicker/
          ShowPicker.tsx
        ConceptSelector/
          ConceptSelector.tsx
        AlchemyResults/
          AlchemyResults.tsx
```

**Search (Find -> Search):**
- Text search via TMDB API
- Results in poster grid
- In-collection items marked with badge
- Tapping opens Detail page

**Ask (Find -> Ask):**
- Chat UI with user/assistant turns
- Welcome view with 6 random starter prompts (from the 80-prompt bank), refresh button
- Messages stream in progressively
- Mentioned shows appear in horizontal strip below each assistant message
- Tapping mentioned show opens Detail
- Conversation context maintained; summarised after ~10 messages
- "Ask About a Show" variant: seeded with show context when entering from Detail

**Alchemy (Find -> Alchemy):**
- Step 1: Select 2+ starting shows (search collection + catalog)
- Step 2: Tap "Conceptualize Shows" -> AI extracts shared concepts
- Step 3: Select 1-8 concept catalysts
- Step 4: Tap "ALCHEMIZE!" -> AI returns 6 recommendations with reasons
- Step 5: Optional "More Alchemy!" to chain (use results as new inputs)
- Backtracking: changing shows clears concepts/results
- Clear step progression UX

**Mode Switcher:**
- Tabs or segmented control: Search | Ask | Alchemy
- Persistent at top of Find page

### 5.4 Show Detail Page

```
src/pages/DetailPage/
  DetailPage.tsx
  hooks/useShowDetail.ts
  hooks/useMyDataActions.ts
  features/
    HeaderMedia/
      HeaderMedia.tsx
      hooks/useMediaCarousel.ts
    CoreFacts/
      CoreFacts.tsx
    MyRelationship/
      MyRelationship.tsx
      hooks/useStatusActions.ts
    Overview/
      Overview.tsx
    Scoop/
      Scoop.tsx
      hooks/useScoop.ts
    AskAbout/
      AskAbout.tsx
    Recommendations/
      Recommendations.tsx
    ExploreSimilar/
      ExploreSimilar.tsx
      hooks/useExploreSimilar.ts
    Providers/
      Providers.tsx
    CastCrew/
      CastCrew.tsx
    Seasons/
      Seasons.tsx
    BudgetRevenue/
      BudgetRevenue.tsx
```

**Section order (preserving the spec):**
1. Header media carousel (backdrops/posters/logos, trailers when available)
2. Core facts (year, runtime/seasons, genres) + community score
3. My Rating (slider; rating unsaved show auto-saves as Done)
4. My Status + Interest chips in toolbar (setting status saves; reselecting removes after confirmation)
5. My Tags (display + picker; adding tag to unsaved auto-saves as Later + Interested)
6. Overview text
7. AI Scoop toggle ("Give me the scoop!" / "Show the scoop" / "The Scoop")
   - Streams progressively
   - 4-hour freshness cache
   - Only persists if show is in collection
8. "Ask about this show" CTA -> navigates to Ask with show context
9. Recommendations strand (TMDB similar/recommended)
10. Explore Similar (Get Concepts -> select -> Explore Shows)
11. Streaming availability (providers by region)
12. Cast & Crew (horizontal strips -> Person Detail)
13. Seasons (TV only, expandable)
14. Budget vs Revenue (movies, when data available)

**Auto-save business rules applied throughout:**
- Rating unsaved show -> saves as Done
- Adding tag to unsaved show -> saves as Later + Interested
- Setting any status -> saves with that status
- Selecting Interested/Excited -> saves as Later + that interest
- Reselecting active status -> confirmation dialog -> removes show + clears My Data

### 5.5 Person Detail Page

```
src/pages/PersonPage/
  PersonPage.tsx
  hooks/usePersonDetail.ts
  features/
    PersonHeader/
      PersonHeader.tsx
    Analytics/
      Analytics.tsx
      hooks/usePersonAnalytics.ts
    Filmography/
      Filmography.tsx
```

**Behaviour:**
- Image gallery, name, bio
- Analytics charts:
  - Average project ratings over time
  - Top genres (bar chart)
  - Projects per year (timeline)
- Filmography grouped by year
- Tapping a credit opens Show Detail

### 5.6 Settings Page

```
src/pages/SettingsPage/
  SettingsPage.tsx
  features/
    AppSettings/
      AppSettings.tsx
    UserSettings/
      UserSettings.tsx
    AISettings/
      AISettings.tsx
    IntegrationSettings/
      IntegrationSettings.tsx
    DataManagement/
      DataManagement.tsx
      hooks/useExport.ts
```

**Sections:**
- **App settings:** Font size selector (XS-XXL), Search on launch toggle
- **User:** Username (editable)
- **AI:** API key input (masked), model selection dropdown
- **Integrations:** TMDB API key input (masked)
- **Your Data:** "Export My Data" button -> downloads .zip containing JSON backup (all shows + My Data, dates ISO-8601)

---

## Phase 6: Identity & Isolation

### 6.1 Namespace Isolation

- `namespace_id` read from `NEXT_PUBLIC_NAMESPACE_ID` env var
- Every database query includes `namespace_id` in WHERE clause
- Test reset only deletes within the namespace

### 6.2 User Identity (Dev Mode)

- `user_id` read from `NEXT_PUBLIC_DEFAULT_USER_ID` env var
- Dev-only user selector (dropdown in dev toolbar) for testing multi-user
- All user-owned records scoped to `(namespace_id, user_id)`
- Designed so replacing with real OAuth later requires only auth wiring changes, not schema changes

### 6.3 Auth Middleware

`src/lib/auth/devAuth.ts`:
- In dev/test mode: reads user_id from env or X-User-Id header
- Gated behind `NODE_ENV !== 'production'` check
- Structured so OAuth middleware slots in at the same boundary

---

## Phase 7: AI Personality & Prompts

### 7.1 Base Personality Prompt

Shared system prompt fragment used across all surfaces:
- "Fun, chatty TV/movie nerd friend" persona
- Joy-forward, opinionated honesty, vibe-first, specific not generic
- Spoiler-safe by default
- TV/movies domain only
- Tone sliders: 70% friend / 30% critic, 60% hype / 40% measured

### 7.2 Surface-Specific Prompts

**Scoop** (`scoopPrompt.ts`):
- Structured output: personal take, honest stack-up, The Scoop centerpiece, fit/warnings, verdict
- 150-350 words target
- Includes show metadata + user's rating/status if available

**Ask** (`askPrompt.ts`):
- Conversational, 1-3 paragraphs + lists for multi-recs
- Includes user library summary for taste awareness
- Supports "Ask about this show" variant with show context seed

**Ask with Mentions** (`askMentionsPrompt.ts`):
- Returns structured `{ commentary, showList }` object
- `showList` format: `Title::tmdbId::mediaType;;Title2::tmdbId::mediaType;;...`

**Concepts** (`conceptsPrompt.ts`):
- Bullet list only, 1-3 words each, evocative, no plot
- Covers structure/vibe/emotion/craft/genre-flavour axes
- For multi-show: shared concepts across all inputs
- Ordered by strength (best "aha" first)

**Concept Recommendations** (`conceptRecsPrompt.ts`):
- 5 recs (Explore Similar) or 6 recs (Alchemy)
- Each with concise reason naming which concepts align
- Real catalog items with TMDB IDs
- Recent bias but allows classics/hidden gems

**Conversation Summary** (`summarizePrompt.ts`):
- 1-2 sentence summary preserving persona tone
- Replaces older turns after ~10 messages

### 7.3 Starter Prompts

`src/config/starterPrompts.ts`:
- Bank of ~80 conversation starter prompts
- Welcome view randomly selects 6
- Refresh generates new random 6

---

## Phase 8: Data Export

### 8.1 Export Endpoint

`POST /api/data/export`:
- Fetches all shows for (namespace_id, user_id)
- Fetches cloud_settings
- Packages as `StorageSnapshot` JSON
- Creates .zip file with the JSON
- All dates ISO-8601 encoded
- Returns downloadable zip

---

## Phase 9: Testing Strategy

### 9.1 Unit Tests (Vitest)

Co-located with source files:
- `mergeShow.test.ts` - merge logic (non-empty wins, timestamp resolution)
- `collectionRules.test.ts` - save triggers, defaults, removal
- `tmdbMapper.test.ts` - TMDB response -> Show mapping
- `aiParser.test.ts` - structured output parsing (showList, concepts, recs)
- `env.test.ts` - env validation

### 9.2 Integration Tests

- API route tests with mocked Supabase/TMDB/AI clients
- Namespace isolation: two namespaces don't see each other's data
- User isolation: two users in same namespace don't see each other's data
- Save/remove lifecycle: save -> verify -> update -> remove -> verify gone

### 9.3 Test Data Management

- `npm run test:reset` resets only `is_test = true` records in the current namespace
- Seed script creates known test fixtures for deterministic testing
- No global database teardown required

---

## Phase 10: Quality & Polish

### 10.1 Error Handling

- All API routes return structured errors
- Client-side error boundaries per feature
- AI failures: retry once, then graceful fallback (show search handoff)
- TMDB failures: show meaningful empty states

### 10.2 Loading States

- Skeleton loaders for show grids/details
- Streaming text component for AI outputs (Scoop, Ask)
- Progressive loading: show header + core facts first, then enrichment sections

### 10.3 Responsive Design

- Mobile: single column, bottom nav, collapsible sidebar
- Tablet: sidebar + content
- Desktop: full sidebar + spacious content area

---

## Implementation Order

| Step | Deliverable | Dependencies |
|------|-------------|--------------|
| 1 | Project scaffolding, env config, Supabase client | None |
| 2 | Database migrations + RLS | Step 1 |
| 3 | TypeScript types + Show repository + merge logic | Step 2 |
| 4 | Business rules (collection rules) + unit tests | Step 3 |
| 5 | TMDB client + mapper + API routes | Step 1 |
| 6 | Theme tokens + shared components (ShowTile, StatusChips, etc.) | Step 1 |
| 7 | Layout + navigation shell + dev auth | Steps 2, 6 |
| 8 | Collection Home page | Steps 4, 6, 7 |
| 9 | Search feature (Find -> Search) | Steps 5, 6, 7 |
| 10 | Show Detail page (all sections except AI) | Steps 4, 5, 6 |
| 11 | AI client + prompt templates | Step 1 |
| 12 | AI Scoop (Detail page) | Steps 10, 11 |
| 13 | Ask chat (Find -> Ask) | Steps 9, 11 |
| 14 | Concepts + Explore Similar (Detail page) | Steps 10, 11 |
| 15 | Alchemy (Find -> Alchemy) | Steps 9, 11, 14 |
| 16 | Person Detail page | Steps 5, 6 |
| 17 | Settings page + data export | Steps 3, 7 |
| 18 | Integration tests + test reset | Steps 3, 4 |
| 19 | Responsive polish + error states + loading states | All |

---

## Key Architectural Decisions

1. **Next.js App Router** with server components for data fetching, client components for interactivity. API routes proxy TMDB/AI to keep keys server-side.

2. **Supabase as sole persistence**. No client-side IndexedDB or localStorage for collection data. LocalStorage used only for UI preferences (font size, auto-search, filter state, removal confirmation suppression).

3. **TMDB as content catalog**. All show/person/season data sourced from TMDB. Mapped to our Show type via the mapper layer.

4. **Provider-agnostic AI**. OpenAI-compatible API interface. Model and provider configurable via env vars or user settings.

5. **Namespace + User isolation from day one**. Every query partitioned. Schema designed so OAuth replaces dev auth without migration.

6. **Fractal feature architecture**. Each feature self-contained. Humble components with logic in hooks. No index.tsx files.

7. **Merge-on-refresh, never overwrite**. Catalog refreshes use selectFirstNonEmpty. User data wins by timestamp. Data continuity across updates guaranteed.
