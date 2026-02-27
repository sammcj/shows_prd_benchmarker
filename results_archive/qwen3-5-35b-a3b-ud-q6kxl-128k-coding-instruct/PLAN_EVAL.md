# Plan Evaluation

## 1. Requirements Extraction

### Pass 1: Functional Areas

1. **Collection Management** - Status system, interest levels, saving triggers, removing shows, re-adding
2. **Tags & Filtering** - Free-form tags, filter views, media-type toggle
3. **Collection Home** - Library display, status grouping, tile indicators, empty states
4. **Search** - External catalog text search
5. **AI Chat (Ask)** - Conversational discovery, mentions, starter prompts, show-seeded Ask
6. **Alchemy** - Multi-show concept blending discovery flow
7. **Explore Similar** - Per-show concept-based AI recommendations
8. **AI Scoop** - Personality-driven taste review generation and caching
9. **AI Voice & Quality** - Shared persona, voice pillars, guardrails, fallbacks
10. **Concept System** - Concept generation rules, selection UX, quality constraints
11. **Show Detail Page** - Detail page layout, sections, toolbar, narrative hierarchy
12. **Person Detail** - Cast/crew person profiles, analytics, filmography
13. **Settings & Data Management** - App settings, user settings, export/import
14. **Data Persistence & Merge** - Storage rules, timestamps, merge policy, sync, data continuity
15. **Infrastructure & Isolation** - Namespace, identity, auth, env vars, developer experience, benchmark compliance

### Pass 2: Requirements by Area

#### Collection Management

- PRD-001 | `critical` | Setting any status saves show to collection | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-002 | `critical` | Choosing interest chip saves show to collection | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-003 | `critical` | Rating unsaved show saves to collection | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-004 | `critical` | Adding tag to unsaved show saves to collection | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-005 | `critical` | Default save values: status=Later, interest=Interested | `showbiz_prd.md > 5.3 Default Values When Saving`
- PRD-006 | `critical` | First save via rating defaults status to Done | `showbiz_prd.md > 5.3 Default Values When Saving`
- PRD-007 | `critical` | Core statuses: Active, Later, Wait, Done, Quit | `showbiz_prd.md > 4.2 Status System`
- PRD-008 | `important` | Interested/Excited surface as primary chips, set Later + interest level | `showbiz_prd.md > 4.2 Status System`
- PRD-009 | `critical` | Removing status removes show and clears all My Data | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-010 | `important` | Removal triggered by reselecting active status with confirmation | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-011 | `detail` | Removal confirmation with option to suppress after repeated removals | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-012 | `important` | Interest only applies when status is Later | `showbiz_prd.md > 4.3 Interest Levels`
- PRD-013 | `detail` | Interest levels: Interested = normal priority, Excited = high priority | `showbiz_prd.md > 4.3 Interest Levels`

#### Tags & Filtering

- PRD-014 | `important` | Tags are free-form user labels, show can have many | `showbiz_prd.md > 4.4 Tags`
- PRD-015 | `important` | Tags implicitly form personal tag library | `showbiz_prd.md > 4.4 Tags`
- PRD-016 | `important` | Tag filters in sidebar: one per tag plus "No tags" | `showbiz_prd.md > 4.5 Filters`
- PRD-017 | `important` | Data filters: genre, decade, community score ranges | `showbiz_prd.md > 4.5 Filters`
- PRD-018 | `important` | Media-type toggle: All / Movies / TV applies on top of filters | `showbiz_prd.md > 4.5 Filters`
- PRD-019 | `detail` | Default filter: All Shows | `showbiz_prd.md > 4.5 Filters`

#### Collection Home

- PRD-020 | `critical` | Library grouped by status: Active (larger), Excited, Interested, Other (collapsed) | `showbiz_prd.md > 7.1 Collection Home`
- PRD-021 | `important` | Tiles show poster, title, and My Data badges | `showbiz_prd.md > 7.1 Collection Home`
- PRD-022 | `important` | In-collection indicator on tiles when My Status exists | `showbiz_prd.md > 5.9 Tile Indicators`
- PRD-023 | `important` | User rating indicator on tiles when My Rating exists | `showbiz_prd.md > 5.9 Tile Indicators`
- PRD-024 | `detail` | Empty state: no collection prompts Search/Ask | `showbiz_prd.md > 7.1 Collection Home`
- PRD-025 | `detail` | Empty state: filter yields none shows "No results found" | `showbiz_prd.md > 7.1 Collection Home`

#### Search

- PRD-026 | `critical` | Text search by title/keywords in external catalog | `showbiz_prd.md > 7.2 Search`
- PRD-027 | `important` | Search results in poster grid | `showbiz_prd.md > 7.2 Search`
- PRD-028 | `important` | In-collection items marked in search results | `showbiz_prd.md > 7.2 Search`
- PRD-029 | `important` | Selecting search result opens Show Detail | `showbiz_prd.md > 7.2 Search`
- PRD-030 | `detail` | Search auto-open on launch if "Search on Launch" enabled | `showbiz_prd.md > 7.2 Search`
- PRD-031 | `important` | Search has no AI voice (straightforward catalog experience) | `ai_voice_personality.md > 1 Persona Summary`

#### AI Chat (Ask)

- PRD-032 | `critical` | Chat UI with user/assistant turns for conversational discovery | `showbiz_prd.md > 7.3 Ask`
- PRD-033 | `important` | AI mentions shows inline; mentioned shows in horizontal strip | `showbiz_prd.md > 7.3 Ask`
- PRD-034 | `important` | Tapping mentioned show opens Detail or hands off to Search | `showbiz_prd.md > 7.3 Ask`
- PRD-035 | `important` | Welcome view shows 6 random starter prompts; user can refresh | `showbiz_prd.md > 7.3 Ask`
- PRD-036 | `important` | Conversation context retained; older turns summarised after ~10 messages | `showbiz_prd.md > 7.3 Ask`
- PRD-037 | `important` | Structured mention output: commentary + showList format | `ai_prompting_context.md > 3.2 Ask with Mentions`
- PRD-038 | `important` | Ask About a Show: seed conversation with show context from Detail | `showbiz_prd.md > 7.3 Ask (Variants)`
- PRD-039 | `detail` | Ask chat history is session-only, cleared on reset/leaving | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-040 | `important` | Conversation summarisation preserves persona/tone | `ai_prompting_context.md > 4 Conversation Summarization`

#### Alchemy

- PRD-041 | `critical` | Select 2+ starting shows from library and global catalog | `showbiz_prd.md > 4.7 Alchemy Session`
- PRD-042 | `critical` | Conceptualize Shows generates shared concept catalysts | `showbiz_prd.md > 7.4 Alchemy`
- PRD-043 | `critical` | User selects 1-8 concept catalysts | `showbiz_prd.md > 7.4 Alchemy`
- PRD-044 | `critical` | ALCHEMIZE returns 6 recommended shows with reasons | `concept_system.md > 6 Concepts to Recs Contract`
- PRD-045 | `important` | Chain another round using results as new inputs | `showbiz_prd.md > 4.7 Alchemy Session`
- PRD-046 | `important` | Step clarity with cards/sections UX | `showbiz_prd.md > 7.4 Alchemy`
- PRD-047 | `important` | Backtracking allowed; changing shows clears concepts/results | `showbiz_prd.md > 7.4 Alchemy`
- PRD-048 | `detail` | Alchemy results are session-only, cleared when leaving | `showbiz_prd.md > 5.7 AI Data Persistence`

#### Explore Similar

- PRD-049 | `critical` | Get Concepts extracts concepts for single show | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-050 | `critical` | User selects concepts then Explore Shows fetches AI recs | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-051 | `important` | Explore Similar returns 5 recommendations per round | `concept_system.md > 6 Concepts to Recs Contract`

#### AI Scoop

- PRD-052 | `important` | AI-generated personality description, spoiler-safe by default | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-053 | `important` | Scoop generated on demand from Show Detail | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-054 | `important` | Scoop cached with 4-hour freshness, regenerates on demand | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-055 | `important` | Scoop persisted only if show is in user collection | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-056 | `important` | Scoop structured: personal take, stack-up, centerpiece, fit/warnings, verdict | `ai_prompting_context.md > 3.3 Scoop`
- PRD-057 | `detail` | Scoop toggle copy: "Give me the scoop!" / "Show the scoop" / "The Scoop" | `detail_page_experience.md > 3.4 Overview + Scoop`
- PRD-058 | `detail` | Scoop streams progressively, shows "Generating..." not blank | `detail_page_experience.md > 3.4 Overview + Scoop`
- PRD-059 | `detail` | Scoop length target: ~150-350 words | `ai_voice_personality.md > 4.1 Scoop`

#### AI Voice & Quality

- PRD-060 | `important` | All AI surfaces share one consistent persona | `ai_voice_personality.md > 1 Persona Summary`
- PRD-061 | `important` | AI stays within TV/movies domain; redirects if asked to leave | `ai_prompting_context.md > 1 Shared Rules`
- PRD-062 | `critical` | Spoiler-safe by default unless user explicitly requests spoilers | `showbiz_prd.md > 8 Cross-Cutting Rules`
- PRD-063 | `important` | Opinionated and honest; acknowledges mixed reception | `ai_voice_personality.md > 2 Non-Negotiable Voice Pillars`
- PRD-064 | `important` | Specific vibe/structure/craft reasoning over generic genre summaries | `ai_voice_personality.md > 2 Non-Negotiable Voice Pillars`
- PRD-065 | `important` | All recommended titles resolve to real catalog items | `ai_prompting_context.md > 1 Shared Rules`
- PRD-066 | `important` | Unresolvable recs shown non-interactive or handed to Search | `ai_prompting_context.md > 5 Guardrails & Fallbacks`
- PRD-067 | `detail` | Structured output parse failure: retry once, then fallback | `ai_prompting_context.md > 5 Guardrails & Fallbacks`

#### Concept System

- PRD-068 | `important` | Concepts are 1-3 word evocative bullets, no explanation | `concept_system.md > 4 Generation Rules`
- PRD-069 | `important` | No generic concepts ("good characters", "great story") | `concept_system.md > 4 Generation Rules`
- PRD-070 | `important` | Concepts diverse across axes (structure, vibe, emotion, craft) | `concept_system.md > 4 Generation Rules`
- PRD-071 | `detail` | Concepts ordered by strength (best first) | `concept_system.md > 4 Generation Rules`
- PRD-072 | `important` | Multi-show concepts represent shared commonality across all inputs | `concept_system.md > 4 Generation Rules`
- PRD-073 | `important` | Concept recs explicitly reference selected concepts in reasoning | `concept_system.md > 6 Concepts to Recs Contract`
- PRD-074 | `detail` | Concept recs bias toward recent shows but allow classics | `concept_system.md > 6 Concepts to Recs Contract`
- PRD-075 | `important` | 8 concepts generated by default | `discovery_quality_bar.md > 2.3 Concepts`
- PRD-076 | `detail` | Selecting/unselecting concepts clears downstream results | `concept_system.md > 5 Selection UX Rules`
- PRD-077 | `detail` | UI hint "pick the ingredients you want more of" | `concept_system.md > 5 Selection UX Rules`

#### Show Detail Page

- PRD-078 | `critical` | Detail page is single source of truth: facts + My Data + discovery | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-079 | `important` | Header media: backdrops/posters/logos and videos when available | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-080 | `important` | Core facts: year, runtime or seasons/episodes, genres, languages | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-081 | `important` | Community score + My Rating section with rating slider | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-082 | `important` | My Status + Interest chips; setting status saves, reselecting removes | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-083 | `important` | My Tags display + picker on Detail page | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-084 | `important` | Traditional recommendations strand of similar/recommended shows | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-085 | `important` | Streaming availability section | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-086 | `important` | Cast and Crew horizontal strands linking to Person Detail | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-087 | `detail` | Seasons section (TV only) | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-088 | `detail` | Budget vs Revenue section (movies when available) | `showbiz_prd.md > 7.5 Show Detail Page`
- PRD-089 | `important` | Status/Interest chips in toolbar above scroll, not in body | `detail_page_experience.md > 3.3 My Relationship Controls`
- PRD-090 | `detail` | Section hierarchy follows specified narrative order | `detail_page_experience.md > 3 Narrative Hierarchy`
- PRD-091 | `detail` | Header graceful fallback to poster/backdrop only when no trailers | `detail_page_experience.md > 3.1 Header Media`

#### Person Detail

- PRD-092 | `important` | Person profile: image gallery, name, bio | `showbiz_prd.md > 7.6 Person Detail Page`
- PRD-093 | `important` | Person analytics charts: ratings, genres, projects-by-year | `showbiz_prd.md > 7.6 Person Detail Page`
- PRD-094 | `important` | Filmography grouped by year | `showbiz_prd.md > 7.6 Person Detail Page`
- PRD-095 | `important` | Selecting a credit opens Show Detail | `showbiz_prd.md > 7.6 Person Detail Page`

#### Settings & Data Management

- PRD-096 | `important` | Font size / readability setting | `showbiz_prd.md > 7.7 Settings`
- PRD-097 | `detail` | Search on launch toggle | `showbiz_prd.md > 7.7 Settings`
- PRD-098 | `important` | Username setting (synced across devices) | `showbiz_prd.md > 7.7 Settings`
- PRD-099 | `important` | AI provider API key configuration (env vars for benchmark) | `showbiz_prd.md > 7.7 Settings`
- PRD-100 | `important` | AI model selection setting (synced) | `showbiz_prd.md > 7.7 Settings`
- PRD-101 | `important` | Content catalog provider API key setting (synced) | `showbiz_prd.md > 7.7 Settings`
- PRD-102 | `critical` | Export My Data as .zip with JSON backup, ISO-8601 dates | `showbiz_prd.md > 7.7 Settings`

#### Data Persistence & Merge

- PRD-103 | `critical` | User overlay shown everywhere; user edits always win over refreshed data | `showbiz_prd.md > 4.1 Show (Display rule)`
- PRD-104 | `critical` | Non-my fields use selectFirstNonEmpty merge (never overwrite non-empty with empty) | `storage-schema.md > Merge/overwrite policy`
- PRD-105 | `critical` | My fields resolve by timestamp (newer wins) | `storage-schema.md > Merge/overwrite policy`
- PRD-106 | `important` | Every user field tracks last modification timestamp | `showbiz_prd.md > 5.6 Timestamps`
- PRD-107 | `important` | Timestamps used for sorting, conflict resolution, AI cache freshness | `showbiz_prd.md > 5.6 Timestamps`
- PRD-108 | `important` | Re-adding same show preserves latest My Data, refreshes public metadata | `showbiz_prd.md > 5.5 Re-adding Same Show`
- PRD-109 | `important` | Optional cross-device sync: library and settings consistent | `showbiz_prd.md > 5.10 Data Sync`
- PRD-110 | `important` | Sync conflicts resolve per field by most recent edit timestamp | `showbiz_prd.md > 5.10 Data Sync`
- PRD-111 | `important` | Duplicate items detected and merged transparently | `showbiz_prd.md > 5.10 Data Sync`
- PRD-112 | `important` | Preserve user libraries across updates; automatic migration on upgrade | `showbiz_prd.md > 5.11 Data Continuity`
- PRD-113 | `important` | Data model version tracking for migrations | `storage-schema.md > AppMetadata`

#### Infrastructure & Isolation

- PRD-114 | `critical` | Must use Next.js (latest stable) as application runtime | `showbiz_infra_rider_prd.md > 2 Benchmark Baseline`
- PRD-115 | `critical` | Must use Supabase as persistence layer | `showbiz_infra_rider_prd.md > 2 Benchmark Baseline`
- PRD-116 | `critical` | .env.example with all required variables and comments | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-117 | `critical` | .gitignore excludes .env* secrets except .env.example | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-118 | `critical` | Build runs by filling env vars without editing source code | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-119 | `critical` | Secrets must not be committed to repo | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-120 | `important` | Supabase: browser uses anon/public key; elevated keys server-only | `showbiz_infra_rider_prd.md > 3.1 Credential handling rules`
- PRD-121 | `critical` | Scripts for: start app, run tests, reset test data for namespace | `showbiz_infra_rider_prd.md > 3.2 One-command developer experience`
- PRD-122 | `important` | Repeatable schema definition (migrations); fresh DB state deterministically | `showbiz_infra_rider_prd.md > 3.3 Database evolution artifacts`
- PRD-123 | `critical` | Each build operates inside single stable namespace identifier | `showbiz_infra_rider_prd.md > 4.1 Build/run namespace`
- PRD-124 | `critical` | Two namespaces do not read/write each other's data | `showbiz_infra_rider_prd.md > 4.1 Build/run namespace`
- PRD-125 | `critical` | Destructive testing scoped to namespace | `showbiz_infra_rider_prd.md > 4.1 Build/run namespace`
- PRD-126 | `critical` | All user-owned records associated with user_id | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-127 | `important` | System behaves as if multiple users could exist | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-128 | `important` | user_id is opaque stable string or UUID | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-129 | `important` | Dev identity injection mechanism (X-User-Id header or similar) | `showbiz_infra_rider_prd.md > 5.1 Auth policy`
- PRD-130 | `important` | Dev identity disabled or gated for production mode | `showbiz_infra_rider_prd.md > 5.1 Auth policy`
- PRD-131 | `important` | OAuth migration requires config changes, not schema redesign | `showbiz_infra_rider_prd.md > 5.2 Migration to real OAuth`
- PRD-132 | `critical` | Persisted user data stored server-side; backend is source of truth | `showbiz_infra_rider_prd.md > 6.1 Source of truth`
- PRD-133 | `important` | Safe to clear local storage/reinstall without losing user data | `showbiz_infra_rider_prd.md > 6.2 Cache is disposable`
- PRD-134 | `important` | Destructive testing without global database teardown | `showbiz_infra_rider_prd.md > 7 Destructive Testing Rules`
- PRD-135 | `important` | Docker not required for benchmark | `showbiz_infra_rider_prd.md > 8 Cloud Agent Compatibility`

#### Navigation & App Structure

- PRD-136 | `important` | Filters/navigation panel with All Shows, tag filters, data filters | `showbiz_prd.md > 6 App Structure`
- PRD-137 | `important` | Persistent Find/Discover entry point from primary navigation | `showbiz_prd.md > 6 App Structure`
- PRD-138 | `important` | Persistent Settings entry point from primary navigation | `showbiz_prd.md > 6 App Structure`
- PRD-139 | `important` | Find/Discover hub: Search, Ask, Alchemy with clear mode switcher | `showbiz_prd.md > 6 App Structure`
- PRD-140 | `important` | Taste-aware AI: uses library + My Data + session context | `showbiz_prd.md > 8 Cross-Cutting Rules`
- PRD-141 | `detail` | Last selected filter persisted across sessions | `storage-schema.md > UI state`

```
Total: 141 requirements (35 critical, 84 important, 22 detail) across 15 functional areas
```

---

## 2. Coverage Table

| PRD-ID | Requirement | Severity | Coverage | Evidence | Gap |
| ------ | ----------- | -------- | -------- | -------- | --- |
| PRD-001 | Setting any status saves show to collection | `critical` | `partial` | API route `PUT /api/shows/:id/status` and `POST /api/shows` exist | Auto-save trigger logic (unsaved show + status = create) not specified as business rule |
| PRD-002 | Choosing interest chip saves show to collection | `critical` | `partial` | Interest chips listed in Detail toolbar; my_interest enum in schema | Trigger behavior (interest chip on unsaved show creates it) not specified |
| PRD-003 | Rating unsaved show saves to collection | `critical` | `partial` | `PUT /api/shows/:id/score` API route and rating bar in toolbar | Auto-save-on-rate for unsaved shows not specified as a rule |
| PRD-004 | Adding tag to unsaved show saves to collection | `critical` | `partial` | `PUT /api/shows/:id/tags` API route exists | Auto-save-on-tag for unsaved shows not specified |
| PRD-005 | Default save values: status=Later, interest=Interested | `critical` | `missing` | none | No mention of default values when implicitly saving |
| PRD-006 | First save via rating defaults status to Done | `critical` | `missing` | none | No mention of rating-specific default status |
| PRD-007 | Core statuses: Active, Later, Wait, Done, Quit | `critical` | `full` | my_data table enum: `active, later, wait, done, quit`; Detail toolbar lists same | |
| PRD-008 | Interested/Excited as primary chips setting Later + interest | `important` | `partial` | Interest chips in Detail toolbar; my_interest enum exists | Does not state that selecting these chips sets status=Later + interest level |
| PRD-009 | Removing status removes show and clears all My Data | `critical` | `missing` | none | Individual show removal behavior not described; only namespace-level reset exists |
| PRD-010 | Removal triggered by reselecting active status with confirmation | `important` | `missing` | none | No removal interaction pattern described |
| PRD-011 | Removal confirmation with option to suppress | `detail` | `missing` | none | No confirmation UX or suppression logic |
| PRD-012 | Interest only applies when status is Later | `important` | `missing` | none | No conditional applicability rule for interest |
| PRD-013 | Interest levels: Interested = normal, Excited = high priority | `detail` | `partial` | my_interest enum `interested, excited` in schema | Enum exists but priority semantics not described |
| PRD-014 | Tags are free-form user labels, show can have many | `important` | `full` | my_tags text[] in my_data table; `PUT /api/shows/:id/tags` | |
| PRD-015 | Tags implicitly form personal tag library | `important` | `partial` | FilterBar mentioned in Explore page | Tag library aggregation from user data not explicitly described |
| PRD-016 | Tag filters in sidebar: one per tag plus "No tags" | `important` | `partial` | FilterBar exists but lists "genre, decade, my status, community score" | Tag-based filters and "No tags" filter not mentioned |
| PRD-017 | Data filters: genre, decade, community score ranges | `important` | `full` | Explore page: "Filter bar (genre, decade, my status, community score)" | |
| PRD-018 | Media-type toggle: All / Movies / TV on top of filters | `important` | `missing` | none | No media-type toggle described anywhere |
| PRD-019 | Default filter: All Shows | `detail` | `missing` | none | No default filter state specified |
| PRD-020 | Library grouped by status: Active (larger), Excited, Interested, Other (collapsed) | `critical` | `partial` | Explore page shows a library grid with filter bar | Library is a flat filterable grid, not grouped by status sections with differentiated tile sizes |
| PRD-021 | Tiles show poster, title, and My Data badges | `important` | `full` | "Each tile: poster, title, year, community score, my status chip" | |
| PRD-022 | In-collection indicator on tiles when My Status exists | `important` | `full` | "my status chip" on tiles in Explore page | |
| PRD-023 | User rating indicator on tiles when My Rating exists | `important` | `missing` | none | Tile description includes community score but not user rating badge |
| PRD-024 | Empty state: no collection prompts Search/Ask | `detail` | `missing` | none | No empty states described |
| PRD-025 | Empty state: filter yields none shows "No results found" | `detail` | `missing` | none | No empty states described |
| PRD-026 | Text search by title/keywords in external catalog | `critical` | `full` | `GET /api/shows/search?q=` and Search page with search input | |
| PRD-027 | Search results in poster grid | `important` | `full` | "Results grid (similar to Explore)" | |
| PRD-028 | In-collection items marked in search results | `important` | `missing` | none | Search page does not mention collection indicators on results |
| PRD-029 | Selecting search result opens Show Detail | `important` | `partial` | Detail page exists; Search page has results grid | Explicit navigation from search result to Detail not stated |
| PRD-030 | Search auto-open on launch if enabled | `detail` | `missing` | none | No autoSearch feature mentioned |
| PRD-031 | Search has no AI voice (straightforward catalog experience) | `important` | `missing` | none | No explicit statement that Search is AI-free |
| PRD-032 | Chat UI with user/assistant turns | `critical` | `full` | Ask page: "Chat messages (user + AI), Input field with suggestions" | |
| PRD-033 | AI mentions shows inline in horizontal strip | `important` | `full` | "Mentioned Shows strip (auto-extracted from conversation)" | |
| PRD-034 | Tapping mentioned show opens Detail or Search handoff | `important` | `partial` | Mentioned shows strip exists; AI Contract mentions Search handoff | Explicit tap-to-navigate behavior not described in Ask page |
| PRD-035 | Welcome view with 6 random starter prompts, refreshable | `important` | `missing` | none | No welcome view or starter prompts mentioned |
| PRD-036 | Context retained; older turns summarised after ~10 messages | `important` | `full` | "Last 5-7 turns full context, Older turns summarized to 1-2 sentences" | |
| PRD-037 | Structured mention output: commentary + showList format | `important` | `full` | AI Contract: `{ commentary: string, showList: string }` with explicit format | |
| PRD-038 | Ask About a Show: seed conversation with show context | `important` | `partial` | Context Building: "Seed with current show context if applicable" | Seeding logic mentioned but no "Ask about this show" CTA on Detail page |
| PRD-039 | Ask chat history is session-only | `detail` | `partial` | Chat is session-based by architecture | Not explicitly stated as a persistence rule |
| PRD-040 | Conversation summarisation preserves persona/tone | `important` | `missing` | none | Summarisation exists but persona preservation not specified |
| PRD-041 | Select 2+ shows from library and global catalog for Alchemy | `critical` | `partial` | "User selects 2+ shows from library" in Alchemy flow | Only library mentioned; global catalog as input source omitted |
| PRD-042 | Conceptualize Shows generates shared concepts | `critical` | `full` | "AI generates concepts (shared across all shows)" | |
| PRD-043 | User selects 1-8 concept catalysts | `critical` | `full` | "User selects up to 8 concepts" | |
| PRD-044 | ALCHEMIZE returns 6 recommended shows with reasons | `critical` | `full` | "AI returns 6 recommendations with concept-based reasoning" | |
| PRD-045 | Chain another round using results as new inputs | `important` | `missing` | none | No chaining or "More Alchemy!" flow described |
| PRD-046 | Step clarity with cards/sections UX | `important` | `missing` | none | No step-based UX pattern described for Alchemy |
| PRD-047 | Backtracking allowed; changing shows clears downstream | `important` | `missing` | none | No backtracking or state-clearing logic described |
| PRD-048 | Alchemy results are session-only | `detail` | `missing` | none | No persistence scope for Alchemy results stated |
| PRD-049 | Get Concepts extracts concepts for single show | `critical` | `full` | ExploreSimilar sub-feature; `POST /api/ai/concepts` "Single show or multi-show" | |
| PRD-050 | User selects concepts then Explore Shows fetches AI recs | `critical` | `full` | Detail page: "Explore Similar (concepts -> recs)"; `POST /api/ai/recommendations` | |
| PRD-051 | Explore Similar returns 5 recommendations per round | `important` | `full` | "Returns 5 (Explore Similar) or 6 (Alchemy) recommendations" | |
| PRD-052 | AI-generated personality description, spoiler-safe by default | `important` | `full` | Scoop prompt: "No spoilers", "Be opinionated, not encyclopedic" | |
| PRD-053 | Scoop generated on demand from Show Detail | `important` | `full` | `POST /api/ai/scoop` and ScoopSection in Detail page | |
| PRD-054 | Scoop cached with 4-hour freshness | `important` | `full` | "Expires after 4 hours" in Cache Policy and API route | |
| PRD-055 | Scoop persisted only if show is in collection | `important` | `full` | "Only persists if show in user collection" | |
| PRD-056 | Scoop structured: personal take, stack-up, centerpiece, fit, verdict | `important` | `full` | Scoop prompt structure lists all 5 sections | |
| PRD-057 | Scoop toggle copy: "Give me the scoop!" / "Show the scoop" / "The Scoop" | `detail` | `missing` | none | No toggle copy variants specified |
| PRD-058 | Scoop streams progressively, shows "Generating..." | `detail` | `missing` | none | No streaming UX or loading state described |
| PRD-059 | Scoop length target: ~150-350 words | `detail` | `partial` | Plan says "300-500 words total" | Length target differs from spec's 150-350 words |
| PRD-060 | All AI surfaces share one consistent persona | `important` | `partial` | Voice & Style Rules describe attributes (warm, playful, opinionated) | Attributes listed but "one consistent persona across all surfaces" not stated as a rule |
| PRD-061 | AI stays within TV/movies domain; redirects if asked to leave | `important` | `missing` | none | No domain constraint or redirect rule mentioned |
| PRD-062 | Spoiler-safe by default | `critical` | `full` | "Spoiler-safe by default" in Voice Rules; "No spoilers" in Scoop prompt | |
| PRD-063 | Opinionated and honest; acknowledges mixed reception | `important` | `full` | "Opinionated, not encyclopedic"; "Acknowledge mixed reception if relevant" | |
| PRD-064 | Specific vibe/structure/craft reasoning over generic | `important` | `full` | "Specific over generic (no 'good characters')" and "Actionable recommendations" | |
| PRD-065 | All recommended titles resolve to real catalog items | `important` | `full` | "Must resolve to real catalog items" in Recommendations section | |
| PRD-066 | Unresolvable recs shown non-interactive or handed to Search | `important` | `full` | AI Contract: "Fallback: unstructured + Search handoff" | |
| PRD-067 | Structured output parse failure: retry once, then fallback | `detail` | `missing` | none | No retry-then-fallback logic for parse failures |
| PRD-068 | Concepts are 1-3 word evocative bullets, no explanation | `important` | `full` | "1-3 words each"; "Bullet list only, no explanation" | |
| PRD-069 | No generic concepts ("good characters", "great story") | `important` | `full` | "Evocative, not generic" in concept prompt rules | |
| PRD-070 | Concepts diverse across axes (structure, vibe, emotion, craft) | `important` | `full` | "Cover different axes: structure, vibe, emotion, craft" | |
| PRD-071 | Concepts ordered by strength (best first) | `detail` | `full` | "Order by strength (best first)" | |
| PRD-072 | Multi-show concepts represent shared commonality | `important` | `full` | "Must represent commonality across ALL inputs" | |
| PRD-073 | Concept recs explicitly reference selected concepts in reasoning | `important` | `full` | "Reason must explicitly reference which concept(s) it matches" | |
| PRD-074 | Concept recs bias toward recent shows but allow classics | `detail` | `full` | "Bias toward recent but allow classics" | |
| PRD-075 | 8 concepts generated by default | `important` | `full` | "Returns 8 bullet concepts, 1-3 words each" | |
| PRD-076 | Selecting/unselecting concepts clears downstream results | `detail` | `missing` | none | No state-clearing on concept selection change |
| PRD-077 | UI hint "pick the ingredients you want more of" | `detail` | `missing` | none | No concept selection copy/guidance |
| PRD-078 | Detail page is single source of truth: facts + My Data + discovery | `critical` | `full` | Full Detail page with 11 sections covering all three dimensions | |
| PRD-079 | Header media: backdrops/posters/logos and videos | `important` | `full` | "Header carousel (backdrop/poster/logo/trailer)" | |
| PRD-080 | Core facts: year, runtime or seasons/episodes, genres, languages | `important` | `full` | "Core facts row (year, runtime/seasons, community score)" and "Genres + languages" section | |
| PRD-081 | Community score + My Rating section with rating slider | `important` | `full` | Community score in core facts row; "Rating bar (0-10)" in toolbar | |
| PRD-082 | My Status + Interest chips on Detail page | `important` | `full` | "Status chips: Active, Later, Wait, Done, Quit" and "Interest chips: Interested, Excited" in toolbar | |
| PRD-083 | My Tags display + picker on Detail page | `important` | `full` | "Tag chips (my tags)" as section 3 of Detail page | |
| PRD-084 | Traditional recommendations strand | `important` | `full` | "Traditional recommendations strand" as section 6 | |
| PRD-085 | Streaming availability section | `important` | `full` | "Streaming providers" as section 8 | |
| PRD-086 | Cast and Crew strands linking to Person Detail | `important` | `full` | "Cast, Crew" as section 9 | |
| PRD-087 | Seasons section (TV only) | `detail` | `full` | "Seasons (TV only)" as section 10 | |
| PRD-088 | Budget vs Revenue section (movies when available) | `detail` | `full` | "Budget/Revenue (movies)" as section 11 | |
| PRD-089 | Status/Interest chips in toolbar above scroll | `important` | `full` | "Toolbar (fixed, above scroll)" with status/interest/rating | |
| PRD-090 | Section hierarchy follows specified narrative order | `detail` | `partial` | Plan lists 11 sections in a defined order | Order differs from detail_page_experience spec (e.g., "Ask about this show" CTA absent, reordered sections) |
| PRD-091 | Header graceful fallback to poster/backdrop when no trailers | `detail` | `missing` | none | No fallback behavior for missing media |
| PRD-092 | Person profile: image gallery, name, bio | `important` | `missing` | none | No Person Detail page in plan; directory structure omits it entirely |
| PRD-093 | Person analytics charts: ratings, genres, projects-by-year | `important` | `missing` | none | No Person Detail page |
| PRD-094 | Filmography grouped by year | `important` | `missing` | none | No Person Detail page |
| PRD-095 | Selecting a credit opens Show Detail | `important` | `missing` | none | No Person Detail page |
| PRD-096 | Font size / readability setting | `important` | `missing` | none | Settings page lists "display name, AI model, API keys" only; no font size |
| PRD-097 | Search on launch toggle | `detail` | `missing` | none | No autoSearch setting |
| PRD-098 | Username setting (synced) | `important` | `full` | profiles.display_name and settings table; Profile page lists "display name" | |
| PRD-099 | AI provider API key configuration | `important` | `full` | settings.ai_api_key; AI_API_KEY env var; Profile page lists "API keys" | |
| PRD-100 | AI model selection setting | `important` | `full` | settings.ai_model; AI_MODEL env var; Profile page lists "AI model" | |
| PRD-101 | Content catalog provider API key setting | `important` | `full` | settings.catalog_api_key; CATALOG_API_KEY env var | |
| PRD-102 | Export My Data as .zip with JSON backup, ISO-8601 dates | `critical` | `partial` | "POST /api/settings/export - Export library as JSON zip"; "Export data (JSON zip)" in Profile | ISO-8601 date encoding not specified |
| PRD-103 | User overlay shown everywhere; user edits always win | `critical` | `full` | Data Merge Strategy preserves user edits; timestamp-based my field resolution | |
| PRD-104 | Non-my fields use selectFirstNonEmpty merge | `critical` | `full` | "selectFirstNonEmpty(newValue, oldValue)" explicitly described with rules | |
| PRD-105 | My fields resolve by timestamp (newer wins) | `critical` | `full` | "Compare update dates, Keep newer timestamp" in My Fields merge | |
| PRD-106 | Every user field tracks last modification timestamp | `important` | `full` | Schema includes my_status_update_date, my_interest_update_date, my_tags_update_date, my_score_update_date | |
| PRD-107 | Timestamps used for sorting, conflict resolution, AI cache | `important` | `partial` | Timestamps used for merge conflict resolution and AI cache (4hr scoop) | Sorting by timestamp not mentioned |
| PRD-108 | Re-adding same show preserves My Data, refreshes public metadata | `important` | `partial` | Merge strategy preserves user edits across catalog refreshes | Re-adding after removal scenario not explicitly addressed |
| PRD-109 | Optional cross-device sync: library and settings consistent | `important` | `missing` | none | No sync mechanism or cross-device consistency described |
| PRD-110 | Sync conflicts resolve per field by most recent timestamp | `important` | `partial` | Merge strategy uses timestamp resolution for my fields | Framed as catalog merge, not as cross-device sync conflict resolution |
| PRD-111 | Duplicate items detected and merged transparently | `important` | `partial` | "POST /api/shows - Create/show merge (catalog + user data)" handles merging | Duplicate detection not explicitly described as a separate concern |
| PRD-112 | Preserve user libraries across updates; automatic migration | `important` | `partial` | app_metadata with data_model_version; Prisma migrations in migration path | No explicit data migration strategy for preserving user data across schema changes |
| PRD-113 | Data model version tracking for migrations | `important` | `full` | app_metadata table with data_model_version (int, default 3) | |
| PRD-114 | Must use Next.js (latest stable) | `critical` | `full` | "Next.js 14 (App Router)" | |
| PRD-115 | Must use Supabase as persistence layer | `critical` | `full` | "Supabase (PostgreSQL)" with official client libraries implied | |
| PRD-116 | .env.example with all required variables | `critical` | `full` | Full Environment Variables section with all required vars | |
| PRD-117 | .gitignore excludes .env* except .env.example | `critical` | `missing` | none | No .gitignore configuration mentioned |
| PRD-118 | Build runs by filling env vars without code edits | `critical` | `full` | Environment Variables section defines all configurable values; architecture supports config-only setup | |
| PRD-119 | Secrets must not be committed to repo | `critical` | `missing` | none | No explicit secret management policy stated |
| PRD-120 | Supabase: browser uses anon key; elevated keys server-only | `important` | `full` | NEXT_PUBLIC_SUPABASE_ANON_KEY (client) and SUPABASE_SERVICE_ROLE_KEY (server) separated | |
| PRD-121 | Scripts for: start app, run tests, reset test data | `critical` | `full` | Scripts: "dev", "test", "test:reset" all present | |
| PRD-122 | Repeatable schema definition; fresh DB deterministically | `important` | `full` | Prisma migrations; "db:migrate", "db:push", "db:seed" scripts | |
| PRD-123 | Each build operates in single stable namespace | `critical` | `full` | "Each build gets unique namespace_id (UUID)" in Namespace Isolation | |
| PRD-124 | Two namespaces do not read/write each other's data | `critical` | `full` | "Two namespaces never see each other's data" | |
| PRD-125 | Destructive testing scoped to namespace | `critical` | `full` | Reset Test Data SQL uses `WHERE namespace_id = $1` | |
| PRD-126 | All user-owned records associated with user_id | `critical` | `full` | user_id (uuid, indexed) on all relevant tables | |
| PRD-127 | System behaves as if multiple users could exist | `important` | `full` | Partition key "(namespace_id, user_id)" supports multi-user | |
| PRD-128 | user_id is opaque stable string or UUID | `important` | `full` | user_id is uuid type in schema | |
| PRD-129 | Dev identity injection mechanism | `important` | `full` | "Accept X-User-Id header in API routes" in Development Mode | |
| PRD-130 | Dev identity disabled for production mode | `important` | `full` | "Header injection disabled" in Production Mode | |
| PRD-131 | OAuth migration requires config changes, not schema redesign | `important` | `full` | "Migration path to real OAuth (no schema changes needed)" in Success Criteria | |
| PRD-132 | Persisted user data stored server-side; backend source of truth | `critical` | `full` | All data in Supabase; API routes mediate all access; architecture is server-authoritative | |
| PRD-133 | Safe to clear local storage without losing data | `important` | `missing` | none | No explicit statement that client storage is disposable |
| PRD-134 | Destructive testing without global database teardown | `important` | `full` | Namespace-scoped DELETE statements; "Keep profiles and settings" | |
| PRD-135 | Docker not required for benchmark | `important` | `missing` | none | No mention of Docker optionality |
| PRD-136 | Filters/navigation panel with All Shows, tag filters, data filters | `important` | `partial` | Explore page has FilterBar | FilterBar is a page-level component, not a persistent sidebar/navigation panel |
| PRD-137 | Persistent Find/Discover entry point from primary navigation | `important` | `partial` | Pages exist for Search, Ask, Alchemy | No persistent navigation structure or Find/Discover hub described |
| PRD-138 | Persistent Settings entry point from primary navigation | `important` | `partial` | Profile page exists | Not described as a persistent navigation entry point |
| PRD-139 | Find/Discover hub: Search, Ask, Alchemy with mode switcher | `important` | `partial` | Separate pages exist for each mode | No hub page or mode switcher; modes are separate page routes |
| PRD-140 | Taste-aware AI: uses library + My Data + session context | `important` | `partial` | Ask: "Include user library summary"; Recs: "optional library context" | Library context is optional, not mandated; My Data integration unclear |
| PRD-141 | Last selected filter persisted across sessions | `detail` | `missing` | none | No filter persistence mechanism |

---

## 3. Coverage Scores

**Overall score:**

```
score = (74 x 1.0 + 28 x 0.5) / 141 x 100
      = (74 + 14) / 141 x 100
      = 88 / 141 x 100
      = 62.4%
```

**Score by severity tier:**

```
Critical:  (23 x 1.0 + 7 x 0.5) / 35 x 100 = 26.5 / 35 x 100 = 75.7%  (23 full + 7 partial of 35 critical requirements)
Important: (47 x 1.0 + 17 x 0.5) / 84 x 100 = 55.5 / 84 x 100 = 66.1%  (47 full + 17 partial of 84 important requirements)
Detail:    (4 x 1.0 + 4 x 0.5) / 22 x 100 = 6 / 22 x 100 = 27.3%  (4 full + 4 partial of 22 detail requirements)
Overall:   62.4% (141 total requirements)
```

---

## 4. Top Gaps

**1. PRD-009 | `critical` | Removing status removes show and clears all My Data**
The plan describes namespace-level data resets but never addresses individual show removal from a user's collection. Without this, users have no way to uncollect a show, which is a core collection management action present in multiple user journeys. This is the most fundamental CRUD gap in the plan.

**2. PRD-005 | `critical` | Default save values: status=Later, interest=Interested**
The plan defines APIs for updating status, interest, tags, and rating but never specifies what defaults apply when a show is implicitly saved (e.g., via tagging or interest chip). Without defaults, the saving triggers (PRD-001 through PRD-004) cannot produce consistent collection membership, and downstream features like status-grouped Collection Home break unpredictably.

**3. PRD-006 | `critical` | First save via rating defaults status to Done**
This is the "rate-to-save" user journey. The plan has a rating API and UI element but does not define the business rule that rating an unsaved show sets its status to Done. A user who rates a show from Search will not see it in their library without this rule.

**4. PRD-117 | `critical` | .gitignore excludes .env* secrets except .env.example**
The plan defines environment variables and a .env.example but never mentions .gitignore configuration. In a benchmark context where secrets are injected via env files, omitting .gitignore is a security compliance failure that could result in committed credentials.

**5. PRD-119 | `critical` | Secrets must not be committed to repo**
Related to PRD-117 but broader: the plan has no explicit secret management policy. While the architecture implies separation (server-only keys), the absence of a stated rule means a developer could commit API keys or service role secrets without violating any documented constraint.

---

## 5. Coverage Narrative

**Overall posture.** This plan is structurally sound at the infrastructure and AI specification layers but has concerning holes in collection management business rules, navigation architecture, and several feature-level UX specifications. It reads as a strong technical blueprint for "how to build the data layer and AI integrations" but an incomplete specification for "how the product actually behaves when a user interacts with it." An implementer following this plan would produce a working database, functional API routes, and on-brand AI outputs, but would have to guess at critical user-facing behaviors like implicit saves, removal semantics, default values, and navigation flow. The overall score of 62.4% reflects this split: deep coverage where it chooses to go deep, and silence where it does not.

**Strength clusters.** The plan is strongest in three areas. First, **infrastructure and isolation** (75.7% critical score) is well-specified: namespace partitioning, user identity, dev identity injection, Supabase integration, environment variables, and migration-ready schema are all concretely addressed with SQL, env var definitions, and explicit behavioral rules. Second, **AI prompt contracts and concept generation** are thoroughly covered. The Scoop prompt structure, Ask mention format, concept generation rules (1-3 words, diverse axes, ordered by strength), and recommendation contracts (5 vs 6 recs, concept-referenced reasoning, real catalog resolution) are all present with enough specificity to implement. Third, the **Show Detail page** is comprehensively sectioned with all 11+ required components listed, a fixed toolbar specified, and sub-feature components named. The data merge strategy (selectFirstNonEmpty for catalog fields, timestamp-based resolution for user fields) is one of the most precisely specified parts of the entire plan.

**Weakness clusters.** Gaps concentrate in three patterns. First, **collection management business rules** are systematically absent. The plan defines the data structures (status enum, interest enum, tags array, rating field) and the API endpoints for updating them, but does not specify the *trigger behaviors* that connect user actions to state changes: what happens when you rate an unsaved show? what defaults apply? how does removal work? what clears what? This is the gap between "the database supports it" and "the product does it." Second, **Person Detail is entirely missing** as a feature. The plan's directory structure, page list, and feature implementation sections omit it completely, despite the PRD defining a full Person page with image gallery, bio, analytics charts, filmography, and navigation back to Show Detail. Third, **navigation and app structure** is under-specified. The plan describes individual pages but not the persistent navigation shell, the Find/Discover hub with its mode switcher, the sidebar filter panel, the media-type toggle, or the relationship between filter state and library display. These are not edge cases; they define how users move between features.

**Risk assessment.** If executed as-is, the most likely failure mode is a user attempting to manage their collection and encountering inconsistent or absent behavior. Specifically: a user rates a show from Search, expects it to appear in their library as "Done," and it either does not appear or appears with no status. They then try to remove it and find no mechanism to do so. This breaks the two most fundamental user journeys (rate-to-save and collection maintenance) and would be the first thing a QA reviewer flags. Secondary to that, the lack of a Person Detail page would be noticed whenever a user taps a cast member on a Show Detail page and nothing happens, breaking the "talent deep-dive" journey entirely.

**Remediation guidance.** The weakness clusters require different types of planning work. The collection management gaps need **business rule specifications**: a section defining the state machine for show lifecycle (unsaved to saved to removed), the trigger conditions, default values, and clearing semantics. This is not new architecture; the data structures already exist. It is acceptance criteria for the existing APIs. The Person Detail gap requires a **new plan section**: a page definition with layout, hooks, API routes (person lookup, filmography fetch), and navigation wiring, following the same pattern as the existing Detail page. The navigation gaps require **architectural specification**: a persistent layout component that wraps all pages, defines the sidebar/filter panel, the top-level navigation entries, the Find/Discover hub with mode switching, and the media-type toggle. The detail-tier gaps (empty states, toggle copy, streaming UX, filter persistence) are individually minor but collectively represent missing UX specification that would typically live in a design system or interaction spec addendum.
