# Plan Evaluation

Evaluation of `results/PLAN.md` against the full PRD corpus (`showbiz_prd.md`, `showbiz_infra_rider_prd.md`, and all supporting documents in `docs/prd/supporting_docs/`).

---

## 1. Requirements Extraction

### Pass 1: Functional Areas

| # | Functional Area | Description |
|---|----------------|-------------|
| 1 | Collection Management | Save/remove rules, defaults, auto-save triggers, re-adding behaviour |
| 2 | Status & Interest System | Core statuses, interest levels, transitions, chips |
| 3 | Tags & Filtering | Free-form tags, filter views, data filters, media-type toggle |
| 4 | App Structure & Collection Home | Navigation layout, Find hub, collection display, status grouping, empty states |
| 5 | Search & Catalog Integration | Text search, poster grid results, TMDB proxy, in-collection marking |
| 6 | AI Chat (Ask) | Chat UI, mentioned shows, starter prompts, conversation context, Ask About Show |
| 7 | Alchemy Discovery | Multi-show selection, shared concepts, recommendations, chaining |
| 8 | Explore Similar | Single-show concepts, concept selection, concept-based AI recs |
| 9 | AI Scoop | On-demand generation, caching, persistence, streaming, structure |
| 10 | Show Detail Page | Section hierarchy, media header, core facts, My Data controls, providers |
| 11 | Person Detail | Gallery, bio, analytics charts, filmography, navigation |
| 12 | Settings & Data Management | App/user/AI settings, export/backup |
| 13 | Data Persistence & Integrity | Schema, merge rules, timestamps, sync, data continuity |
| 14 | Identity & Isolation | Namespace, user_id, dev auth, OAuth migration path |
| 15 | Infrastructure & Developer Experience | Env config, scripts, migrations, Next.js, Supabase, cloud-agent compat |
| 16 | AI Personality & Discovery Quality | Persona, voice pillars, surface adaptations, concept quality, real-show integrity |

---

### Pass 2: Requirements by Functional Area

#### Collection Management

- PRD-001 | `critical` | Show has public data plus user overlay (My Data) | `showbiz_prd.md > 4.1 Show`
- PRD-002 | `critical` | User's overlaid version displayed everywhere show appears | `showbiz_prd.md > 4.1 Show (Display rule)` + `showbiz_prd.md > 8. Cross-Cutting Rules (rule 1)`
- PRD-003 | `critical` | Show is in collection when it has assigned status | `showbiz_prd.md > 5.1 Collection Membership`
- PRD-004 | `critical` | Save triggers: set status, choose interest, rate unsaved, tag unsaved | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-005 | `critical` | Default save without explicit status: Later + Interested | `showbiz_prd.md > 5.3 Default Values When Saving`
- PRD-006 | `important` | First save via rating defaults status to Done | `showbiz_prd.md > 5.3 Default Values When Saving`
- PRD-007 | `critical` | Removing show clears status, interest, tags, rating, AI Scoop | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-008 | `important` | Removal warning confirmation with option to stop asking | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-009 | `important` | Re-adding preserves latest My Data, refreshes public metadata | `showbiz_prd.md > 5.5 Re-adding the Same Show`

#### Status & Interest System

- PRD-010 | `critical` | Core statuses: Active, Later, Wait, Done, Quit | `showbiz_prd.md > 4.2 Status System`
- PRD-011 | `detail` | Next status present in data model but not surfaced in UI | `showbiz_prd.md > 4.2 Status System`
- PRD-012 | `important` | Interested/Excited chips set status=Later + interest level | `showbiz_prd.md > 4.2 Status System`
- PRD-013 | `important` | Interest only applies when status is Later | `showbiz_prd.md > 4.3 Interest Levels`
- PRD-014 | `detail` | Interest may be retained when status changes from Later | `showbiz_prd.md > 4.3 Interest Levels`

#### Tags & Filtering

- PRD-015 | `important` | Free-form tags; show can have many tags | `showbiz_prd.md > 4.4 Tags`
- PRD-016 | `important` | Tags implicitly form personal tag library | `showbiz_prd.md > 4.4 Tags`
- PRD-017 | `important` | Tags power filters and grouping across the app | `showbiz_prd.md > 4.4 Tags` + `showbiz_prd.md > 4.5 Filters`
- PRD-018 | `important` | Filter views: All Shows, per-tag filters, No Tags filter | `showbiz_prd.md > 4.5 Filters`
- PRD-019 | `important` | Data filters: genre, decade, community score ranges | `showbiz_prd.md > 4.5 Filters`
- PRD-020 | `important` | Media-type toggle: All / Movies / TV on top of any filter | `showbiz_prd.md > 4.5 Filters`

#### App Structure & Collection Home

- PRD-021 | `critical` | Layout: filters/nav panel + main content area | `showbiz_prd.md > 6. App Structure`
- PRD-022 | `critical` | Persistent Find/Discover entry point in primary nav | `showbiz_prd.md > 6. App Structure`
- PRD-023 | `important` | Persistent Settings entry point in primary nav | `showbiz_prd.md > 6. App Structure`
- PRD-024 | `important` | Find hub modes: Search, Ask, Alchemy with mode switcher | `showbiz_prd.md > 6. App Structure`
- PRD-025 | `critical` | Collection grouped: Active, Excited, Interested, Other (collapsed) | `showbiz_prd.md > 7.1 Collection Home`
- PRD-026 | `important` | Active section has prominent/larger tiles | `showbiz_prd.md > 7.1 Collection Home`
- PRD-027 | `important` | Other statuses (Wait, Quit, Done) in collapsed group | `showbiz_prd.md > 7.1 Collection Home`
- PRD-028 | `important` | Tiles show poster, title, and My Data badges | `showbiz_prd.md > 7.1 Collection Home`
- PRD-029 | `important` | Empty states: no collection prompts Search/Ask; filter yields "No results" | `showbiz_prd.md > 7.1 Collection Home`

#### Search & Catalog Integration

- PRD-030 | `critical` | Text search by title/keywords in global catalog | `showbiz_prd.md > 7.2 Search`
- PRD-031 | `important` | Search results displayed in poster grid | `showbiz_prd.md > 7.2 Search`
- PRD-032 | `important` | In-collection items marked in search results | `showbiz_prd.md > 7.2 Search`
- PRD-033 | `important` | Selecting search result opens Show Detail | `showbiz_prd.md > 7.2 Search`
- PRD-034 | `detail` | Search auto-opens on launch if user enabled setting | `showbiz_prd.md > 7.2 Search`

#### AI Chat (Ask)

- PRD-035 | `critical` | Chat UI with user/assistant turns | `showbiz_prd.md > 7.3 Ask`
- PRD-036 | `important` | Friendly, opinionated, spoiler-safe tone in responses | `showbiz_prd.md > 7.3 Ask`
- PRD-037 | `important` | AI mentions shows inline; appear in horizontal strip | `showbiz_prd.md > 7.3 Ask`
- PRD-038 | `important` | Tapping mentioned show opens Detail or hands off to Search | `showbiz_prd.md > 7.3 Ask`
- PRD-039 | `important` | Welcome view: 6 random starter prompts with refresh | `showbiz_prd.md > 7.3 Ask`
- PRD-040 | `important` | Conversation context retained; summarised after ~10 messages | `showbiz_prd.md > 7.3 Ask`
- PRD-041 | `important` | "Ask About a Show" variant seeded with show context | `showbiz_prd.md > 7.3 Ask` + `detail_page_experience.md > 3.5`
- PRD-042 | `important` | Structured output: commentary + showList format (Title::id::type;;...) | `ai_prompting_context.md > 3.2`
- PRD-043 | `important` | Conversation summaries preserve persona/tone | `ai_prompting_context.md > 4`

#### Alchemy Discovery

- PRD-044 | `critical` | Select 2+ starting shows from library and global catalog | `showbiz_prd.md > 4.7 Alchemy Session` + `showbiz_prd.md > 7.4 Alchemy`
- PRD-045 | `critical` | AI extracts shared concept catalysts from selected shows | `showbiz_prd.md > 4.7` + `showbiz_prd.md > 7.4`
- PRD-046 | `important` | User selects 1-8 concept catalysts | `showbiz_prd.md > 4.7` + `concept_system.md > 5`
- PRD-047 | `critical` | AI returns 6 recommendations with short reasons | `showbiz_prd.md > 4.7` + `concept_system.md > 6`
- PRD-048 | `important` | Chain another round using results as new inputs | `showbiz_prd.md > 4.7` + `showbiz_prd.md > 7.4`
- PRD-049 | `important` | Step clarity (cards/sections) and backtracking allowed | `showbiz_prd.md > 7.4 Alchemy`
- PRD-050 | `important` | Changing starting shows clears concepts and results | `showbiz_prd.md > 7.4 Alchemy`

#### Explore Similar (Concept Discovery)

- PRD-051 | `critical` | Get Concepts extracts concepts for a single show | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-052 | `critical` | User selects concepts then Explore Shows for AI recs | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-053 | `important` | Explore Similar returns 5 recommendations per round | `concept_system.md > 6`
- PRD-054 | `important` | Selecting/unselecting concepts clears downstream results | `concept_system.md > 5`

#### AI Scoop

- PRD-055 | `important` | AI-generated personality description, spoiler-safe by default | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-056 | `important` | Generated on demand from Show Detail page | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-057 | `important` | Cached for freshness; regenerates after 4 hours on demand | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-058 | `important` | Persisted only if show is in user's collection | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-059 | `important` | Streams progressively; user sees generating state, not blank | `detail_page_experience.md > 3.4`
- PRD-060 | `detail` | Toggle copy states: "Give me the scoop!" / "Show the scoop" / "The Scoop" | `detail_page_experience.md > 3.4`
- PRD-061 | `important` | Structured output: take, stack-up, Scoop centerpiece, fit/warnings, verdict | `ai_prompting_context.md > 3.3` + `ai_voice_personality.md > 4.1`
- PRD-062 | `detail` | Length target: 150-350 words | `ai_voice_personality.md > 4.1`

#### Show Detail Page

- PRD-063 | `critical` | Header media: backdrops/posters/logos and trailers when available | `showbiz_prd.md > 7.5 Show Detail`
- PRD-064 | `important` | Core facts: year, runtime or seasons/episodes, genres, languages | `showbiz_prd.md > 7.5 Show Detail`
- PRD-065 | `important` | Community score display + My Rating slider | `showbiz_prd.md > 7.5 Show Detail`
- PRD-066 | `important` | Rating unsaved show auto-saves as Done | `showbiz_prd.md > 7.5 Show Detail`
- PRD-067 | `important` | My Status + Interest chips; setting saves, reselecting removes | `showbiz_prd.md > 7.5 Show Detail`
- PRD-068 | `important` | My Tags display + picker; adding tag to unsaved auto-saves | `showbiz_prd.md > 7.5 Show Detail`
- PRD-069 | `detail` | Overview section | `showbiz_prd.md > 7.5 Show Detail`
- PRD-070 | `important` | "Ask about this show" CTA navigates to Ask with context | `showbiz_prd.md > 7.5 Show Detail` + `detail_page_experience.md > 3.5`
- PRD-071 | `important` | Traditional recommendations strand (similar/recommended) | `showbiz_prd.md > 7.5 Show Detail`
- PRD-072 | `important` | Streaming availability section | `showbiz_prd.md > 7.5 Show Detail`
- PRD-073 | `important` | Cast & Crew horizontal strands linking to Person Detail | `showbiz_prd.md > 7.5 Show Detail`
- PRD-074 | `detail` | Seasons section (TV only) | `showbiz_prd.md > 7.5 Show Detail`
- PRD-075 | `detail` | Budget vs Revenue (movies when data available) | `showbiz_prd.md > 7.5 Show Detail`
- PRD-076 | `important` | Section order preserved per narrative hierarchy spec | `detail_page_experience.md > 3`
- PRD-077 | `important` | Header graceful fallback when no trailers or backdrops | `detail_page_experience.md > 3.1` + `detail_page_experience.md > 5`
- PRD-078 | `detail` | Status/Interest chips in toolbar, not in scroll body | `detail_page_experience.md > 3.3`
- PRD-079 | `detail` | Tile indicators: in-collection badge and user rating badge | `showbiz_prd.md > 5.9 Tile Indicators`

#### Person Detail

- PRD-080 | `important` | Image gallery, name, bio | `showbiz_prd.md > 7.6 Person Detail`
- PRD-081 | `important` | Analytics charts: avg ratings, top genres, projects-by-year | `showbiz_prd.md > 7.6 Person Detail`
- PRD-082 | `important` | Filmography grouped by year | `showbiz_prd.md > 7.6 Person Detail`
- PRD-083 | `important` | Selecting a credit opens Show Detail | `showbiz_prd.md > 7.6 Person Detail`

#### Settings & Data Management

- PRD-084 | `important` | Font size / readability setting | `showbiz_prd.md > 7.7 Settings`
- PRD-085 | `detail` | Search on launch toggle setting | `showbiz_prd.md > 7.7 Settings`
- PRD-086 | `detail` | Username setting (synced if sync enabled) | `showbiz_prd.md > 7.7 Settings`
- PRD-087 | `important` | AI provider API key setting (never committed to repo) | `showbiz_prd.md > 7.7 Settings`
- PRD-088 | `detail` | AI model selection setting (synced if sync enabled) | `showbiz_prd.md > 7.7 Settings`
- PRD-089 | `detail` | Content catalog provider API key setting | `showbiz_prd.md > 7.7 Settings`
- PRD-090 | `important` | Export My Data: .zip with JSON backup, dates ISO-8601 | `showbiz_prd.md > 7.7 Settings`

#### Data Persistence & Integrity

- PRD-091 | `important` | Every user field tracks last modification timestamp | `showbiz_prd.md > 5.6 Timestamps`
- PRD-092 | `important` | Non-my fields merge: selectFirstNonEmpty, never overwrite with empty | `storage-schema.md > Merge / overwrite policy`
- PRD-093 | `important` | My fields merge: resolve by timestamp, newer wins | `storage-schema.md > Merge / overwrite policy`
- PRD-094 | `detail` | detailsUpdateDate set to now after merge | `storage-schema.md > Merge / overwrite policy`
- PRD-095 | `detail` | creationDate set only on first creation | `storage-schema.md > Merge / overwrite policy`
- PRD-096 | `important` | AI Scoop persisted if in collection, 4hr freshness | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-097 | `important` | Alchemy results/reasons are session-only, not persisted | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-098 | `important` | Ask chat history is session-only, not persisted | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-099 | `important` | Mentioned shows strip is session-only | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-100 | `critical` | AI recs map to real shows via external ID + title match | `showbiz_prd.md > 5.8 AI Recommendations`
- PRD-101 | `important` | Unresolvable recs shown non-interactive or handed to Search | `showbiz_prd.md > 5.8 AI Recommendations` + `ai_prompting_context.md > 5`
- PRD-102 | `important` | Parse failure: retry once with stricter instructions, then fallback | `ai_prompting_context.md > 5`
- PRD-103 | `important` | Data sync: library consistent across devices, per-field conflict resolution | `showbiz_prd.md > 5.10 Data Sync`
- PRD-104 | `important` | Data continuity: libraries preserved across updates, auto migration | `showbiz_prd.md > 5.11 Data Continuity`
- PRD-105 | `important` | Transient data (cast, crew, seasons, videos, recs) not persisted | `storage-schema.md > Stored entities`
- PRD-106 | `detail` | Local settings stored: autoSearch, fontSize | `storage-schema.md > Local settings`
- PRD-107 | `detail` | UI state stored: hideStatusRemovalConfirmation, count, lastFilter | `storage-schema.md > UI state`
- PRD-108 | `detail` | CloudSettings version (epoch seconds) for conflict resolution | `storage-schema.md > CloudSettings`
- PRD-109 | `detail` | AppMetadata tracks dataModelVersion for migrations | `storage-schema.md > AppMetadata`

#### Identity & Isolation

- PRD-110 | `critical` | Every user-owned record scoped to user_id | `showbiz_prd.md > 8. Cross-Cutting Rules (rule 7)` + `showbiz_infra_rider_prd.md > 4.2`
- PRD-111 | `critical` | Each build uses stable namespace_id to partition all data | `showbiz_prd.md > 8. Cross-Cutting Rules (rule 8)` + `showbiz_infra_rider_prd.md > 4.1`
- PRD-112 | `critical` | Two namespaces cannot read/write each other's data | `showbiz_infra_rider_prd.md > 4.1`
- PRD-113 | `critical` | Destructive testing scoped to namespace | `showbiz_infra_rider_prd.md > 4.1`
- PRD-114 | `important` | System behaves as if multiple users could exist | `showbiz_infra_rider_prd.md > 4.2`
- PRD-115 | `important` | user_id is opaque stable string or UUID | `showbiz_infra_rider_prd.md > 4.2`
- PRD-116 | `important` | Effective partition is (namespace_id, user_id) | `showbiz_infra_rider_prd.md > 4.3`
- PRD-117 | `important` | Dev identity injection mechanism for benchmark mode | `showbiz_infra_rider_prd.md > 5.1`
- PRD-118 | `important` | Dev auth documented and gated for production | `showbiz_infra_rider_prd.md > 5.1`
- PRD-119 | `important` | OAuth migration requires config changes, not schema redesign | `showbiz_infra_rider_prd.md > 5.2`
- PRD-120 | `critical` | Persisted user data stored server-side (backend is source of truth) | `showbiz_infra_rider_prd.md > 6.1` + `showbiz_prd.md > 8. Cross-Cutting Rules (rule 9)`
- PRD-121 | `important` | Clearing client storage must not lose user data | `showbiz_infra_rider_prd.md > 6.2` + `showbiz_prd.md > 8. Cross-Cutting Rules (rule 9)`
- PRD-122 | `critical` | No global database teardown required to reset tests | `showbiz_infra_rider_prd.md > 7`

#### Infrastructure & Developer Experience

- PRD-123 | `critical` | Must use Next.js (latest stable) as application runtime | `showbiz_infra_rider_prd.md > 2. Benchmark Baseline`
- PRD-124 | `critical` | Must use Supabase as persistence layer | `showbiz_infra_rider_prd.md > 2. Benchmark Baseline`
- PRD-125 | `important` | Must not assume Docker is available | `showbiz_infra_rider_prd.md > 2. Benchmark Baseline` + `showbiz_infra_rider_prd.md > 8`
- PRD-126 | `critical` | .env.example with all required variables and comments | `showbiz_infra_rider_prd.md > 3.1`
- PRD-127 | `critical` | .gitignore excludes .env* secrets except .env.example | `showbiz_infra_rider_prd.md > 3.1`
- PRD-128 | `critical` | Build runs by filling env vars without editing source code | `showbiz_infra_rider_prd.md > 3.1`
- PRD-129 | `critical` | Secrets must not be committed to repo | `showbiz_infra_rider_prd.md > 3.1`
- PRD-130 | `important` | Browser/client code uses anon key; elevated key server-only | `showbiz_infra_rider_prd.md > 3.1`
- PRD-131 | `critical` | One-command scripts: start app, run tests, reset test data | `showbiz_infra_rider_prd.md > 3.2`
- PRD-132 | `critical` | Repeatable schema definition (migrations); fresh DB deterministic | `showbiz_infra_rider_prd.md > 3.3`

#### AI Personality & Discovery Quality

- PRD-133 | `important` | AI persona: fun, chatty TV/movie nerd friend | `ai_voice_personality.md > 1`
- PRD-134 | `important` | All AI surfaces feel like one consistent persona | `ai_voice_personality.md > 1`
- PRD-135 | `important` | Voice pillars: joy-forward, opinionated honesty, vibe-first, specific, concise | `ai_voice_personality.md > 2`
- PRD-136 | `important` | AI stays within TV/movies domain, redirects if asked to leave | `ai_prompting_context.md > 1`
- PRD-137 | `important` | Taste-aware AI: uses library + My Data + session context | `showbiz_prd.md > 8. Cross-Cutting Rules (rule 3)`
- PRD-138 | `important` | Spoiler-safe by default across all surfaces | `showbiz_prd.md > 8. Cross-Cutting Rules (rule 4)`
- PRD-139 | `detail` | Tone sliders: 70% friend / 30% critic, 60% hype / 40% measured | `ai_voice_personality.md > 3`
- PRD-140 | `detail` | Search has no AI voice | `ai_voice_personality.md > 1`
- PRD-141 | `important` | Concepts: 1-3 words, evocative, spoiler-free, no generic filler | `ai_prompting_context.md > 3.4` + `concept_system.md > 4`
- PRD-142 | `important` | Concept specificity over genericity ("good characters" invalid) | `concept_system.md > 4`
- PRD-143 | `important` | Concept diversity across axes (structure/vibe/emotion/craft) | `concept_system.md > 4`
- PRD-144 | `detail` | Concepts ordered by strength (best "aha" first) | `concept_system.md > 4`
- PRD-145 | `important` | Concept recs reference selected concepts explicitly in reasoning | `ai_prompting_context.md > 3.5` + `concept_system.md > 6`
- PRD-146 | `detail` | Concept recs: recent bias but allow classics/hidden gems | `concept_system.md > 6`
- PRD-147 | `detail` | 8 concepts generated by default | `discovery_quality_bar.md > 2.3`
- PRD-148 | `critical` | Real-show integrity: every rec maps to real catalog item | `discovery_quality_bar.md > 1.5` + `showbiz_prd.md > 5.8` + `showbiz_prd.md > 8 (rule 2)`

```
Total: 148 requirements (34 critical, 89 important, 25 detail) across 16 functional areas
```

---

## 2. Coverage Table

| PRD-ID | Requirement | Severity | Coverage | Evidence | Gap |
|--------|-------------|----------|----------|----------|-----|
| PRD-001 | Show has public data plus user overlay | `critical` | `full` | Phase 1.1 schema: public fields + my_* fields; Phase 2.1 TypeScript types | |
| PRD-002 | User's version displayed everywhere show appears | `critical` | `partial` | Phase 5.2 tiles show My Data badges; Phase 5.3 search marks in-collection | Plan doesn't articulate a global rule that all surfaces overlay user data; it's implicit in component design but not stated as a cross-cutting contract |
| PRD-003 | Show in collection when it has assigned status | `critical` | `full` | Phase 2.4 "collectionRules.ts" + Phase 1.1 my_status column | |
| PRD-004 | Save triggers: status, interest, rate unsaved, tag unsaved | `critical` | `full` | Phase 2.4 "Save triggers" lists all four + Phase 5.4 auto-save rules | |
| PRD-005 | Default save: Later + Interested | `critical` | `full` | Phase 2.4 "Default values: save without explicit status -> Later + Interested" | |
| PRD-006 | Save via rating defaults to Done | `important` | `full` | Phase 2.4 "save via rating -> Done" + Phase 5.4 "Rating unsaved show -> saves as Done" | |
| PRD-007 | Removing show clears all My Data | `critical` | `full` | Phase 2.4 "Removal: clear status -> remove show + all My Data" + Phase 2.2 removeShow | |
| PRD-008 | Removal warning with option to suppress | `important` | `partial` | Phase 5.4 "confirmation dialog -> removes show" + Phase 4.2 ConfirmDialog | Plan mentions confirmation but not the "stop asking after repeated removals" suppression mechanism |
| PRD-009 | Re-adding preserves My Data, refreshes public | `important` | `full` | Phase 2.3 merge logic: "non-empty wins" + timestamp-based My field resolution | |
| PRD-010 | Core statuses: Active, Later, Wait, Done, Quit | `critical` | `full` | Phase 1.1 CHECK constraint lists all statuses | |
| PRD-011 | Next status in data model but not in UI | `detail` | `full` | Phase 1.1 CHECK constraint includes `next`; no UI surface exposes it | |
| PRD-012 | Interested/Excited chips set Later + interest | `important` | `full` | Phase 5.4 "Selecting Interested/Excited -> saves as Later + that interest" | |
| PRD-013 | Interest only applies when status is Later | `important` | `full` | Phase 2.4 "Interest relevance: only applies when status is Later" | |
| PRD-014 | Interest retained when status changes from Later | `detail` | `missing` | `none` | Plan does not address whether interest is cleared or retained on status change away from Later |
| PRD-015 | Free-form tags; show can have many | `important` | `full` | Phase 1.1 `my_tags text[]`; Phase 4.2 TagChips/TagPicker | |
| PRD-016 | Tags form implicit personal tag library | `important` | `full` | Phase 2.2 `getAllTags()` - distinct tags across collection | |
| PRD-017 | Tags power filters and grouping | `important` | `full` | Phase 4.2 FilterSidebar; Phase 5.2 sidebar filter integration | |
| PRD-018 | Filter views: All Shows, per-tag, No Tags | `important` | `partial` | Phase 5.2 sidebar filters + Phase 2.2 getShows(filters?) | Plan does not explicitly mention the "No Tags" filter view for tagless shows |
| PRD-019 | Data filters: genre, decade, community score | `important` | `full` | Phase 2.2 getShows with genre/decade/score filters; Phase 4.2 FilterSidebar | |
| PRD-020 | Media-type toggle: All / Movies / TV | `important` | `full` | Phase 4.2 MediaTypeToggle; Phase 5.2 media type toggle at top | |
| PRD-021 | Layout: filters/nav panel + main content | `critical` | `full` | Phase 5.1 "Sidebar (FilterSidebar) + main content area" | |
| PRD-022 | Persistent Find/Discover entry point | `critical` | `full` | Phase 5.1 "Global navigation: Home, Find/Discover, Settings" | |
| PRD-023 | Persistent Settings entry point | `important` | `full` | Phase 5.1 "Global navigation: Home, Find/Discover, Settings" | |
| PRD-024 | Find hub: Search, Ask, Alchemy with mode switcher | `important` | `full` | Phase 5.3 "Mode Switcher: Tabs or segmented control: Search | Ask | Alchemy" | |
| PRD-025 | Collection grouped by status sections | `critical` | `full` | Phase 5.2 "Groups shows into status sections: Active, Excited, Interested, Other" | |
| PRD-026 | Active section has prominent/larger tiles | `important` | `full` | Phase 5.2 "Active (larger tiles, prominent)" | |
| PRD-027 | Other statuses in collapsed group | `important` | `full` | Phase 5.2 "Other (collapsible: Wait, Quit, Done, unclassified Later)" | |
| PRD-028 | Tiles show poster, title, My Data badges | `important` | `full` | Phase 5.2 "Tiles show poster, title, status badge, rating badge" | |
| PRD-029 | Empty states: no collection CTA, filter no results | `important` | `full` | Phase 5.2 "Empty states: no collection -> CTA to Search/Ask; filter yields none -> No results found" | |
| PRD-030 | Text search in global catalog | `critical` | `full` | Phase 3.1 `searchShows(query, mediaType?)` + Phase 5.3 Search feature | |
| PRD-031 | Search results in poster grid | `important` | `full` | Phase 5.3 "Results in poster grid" | |
| PRD-032 | In-collection items marked in search | `important` | `full` | Phase 5.3 "In-collection items marked with badge" | |
| PRD-033 | Selecting result opens Detail | `important` | `full` | Phase 5.3 "Tapping opens Detail page" | |
| PRD-034 | Search auto-opens if setting enabled | `detail` | `partial` | Phase 5.6 Settings page mentions "Search on launch toggle" | Plan mentions the setting exists but doesn't describe the launch-time auto-open behaviour implementation |
| PRD-035 | Chat UI with user/assistant turns | `critical` | `full` | Phase 5.3 Ask: "Chat UI with user/assistant turns" | |
| PRD-036 | Friendly, opinionated, spoiler-safe tone | `important` | `full` | Phase 7.1 base personality prompt; Phase 7.2 Ask prompt | |
| PRD-037 | Mentioned shows in horizontal strip | `important` | `full` | Phase 5.3 "Mentioned shows appear in horizontal strip below each assistant message" | |
| PRD-038 | Tapping mentioned show opens Detail | `important` | `full` | Phase 5.3 "Tapping mentioned show opens Detail" | |
| PRD-039 | Welcome view: 6 random starters with refresh | `important` | `full` | Phase 5.3 "Welcome view with 6 random starter prompts (from the 80-prompt bank), refresh button" | |
| PRD-040 | Context retained; summarised after ~10 messages | `important` | `full` | Phase 5.3 "Conversation context maintained; summarised after ~10 messages" + Phase 3.3 context builder | |
| PRD-041 | Ask About a Show seeded with show context | `important` | `full` | Phase 5.3 "'Ask About a Show' variant: seeded with show context when entering from Detail" | |
| PRD-042 | Structured output: commentary + showList format | `important` | `full` | Phase 3.2 `askMentionsPrompt.ts` returns `{ commentary, showList }` with format specified | |
| PRD-043 | Conversation summaries preserve persona/tone | `important` | `full` | Phase 7.2 summarizePrompt.ts "1-2 sentence summary preserving persona tone" | |
| PRD-044 | Select 2+ starting shows from library + catalog | `critical` | `full` | Phase 5.3 Alchemy "Step 1: Select 2+ starting shows (search collection + catalog)" | |
| PRD-045 | AI extracts shared concept catalysts | `critical` | `full` | Phase 5.3 Alchemy "Step 2: Tap 'Conceptualize Shows' -> AI extracts shared concepts" + Phase 3.2 multiConceptsPrompt | |
| PRD-046 | User selects 1-8 concepts | `important` | `full` | Phase 5.3 Alchemy "Step 3: Select 1-8 concept catalysts" | |
| PRD-047 | AI returns 6 recommendations with reasons | `critical` | `full` | Phase 5.3 Alchemy "Step 4... AI returns 6 recommendations with reasons" + Phase 7.2 "6 recs (Alchemy)" | |
| PRD-048 | Chain rounds using results as inputs | `important` | `full` | Phase 5.3 "Step 5: Optional 'More Alchemy!' to chain (use results as new inputs)" | |
| PRD-049 | Step clarity and backtracking | `important` | `full` | Phase 5.3 "Clear step progression UX" + "Backtracking: changing shows clears concepts/results" | |
| PRD-050 | Changing shows clears concepts/results | `important` | `full` | Phase 5.3 "Backtracking: changing shows clears concepts/results" | |
| PRD-051 | Get Concepts for single show | `critical` | `full` | Phase 5.4 "Explore Similar (Get Concepts -> select -> Explore Shows)" + Phase 3.2 conceptsPrompt | |
| PRD-052 | Select concepts then Explore Shows | `critical` | `full` | Phase 5.4 section 10 Explore Similar flow described | |
| PRD-053 | Explore Similar returns 5 recs | `important` | `full` | Phase 7.2 "5 recs (Explore Similar)" | |
| PRD-054 | Selecting/unselecting concepts clears downstream | `important` | `missing` | `none` | Plan does not explicitly state that changing concept selection clears downstream recommendation results |
| PRD-055 | AI Scoop is spoiler-safe personality description | `important` | `full` | Phase 7.1 "Spoiler-safe by default" + Phase 7.2 scoopPrompt.ts | |
| PRD-056 | Scoop generated on demand from Detail | `important` | `full` | Phase 5.4 "AI Scoop toggle" | |
| PRD-057 | Scoop cached; regenerates after 4 hours | `important` | `full` | Phase 5.4 "4-hour freshness cache" | |
| PRD-058 | Scoop persisted only if in collection | `important` | `full` | Phase 5.4 "Only persists if show is in collection" | |
| PRD-059 | Scoop streams progressively | `important` | `full` | Phase 5.4 "Streams progressively" + Phase 3.2 streaming support + Phase 4.2 StreamingText component | |
| PRD-060 | Scoop toggle copy states | `detail` | `full` | Phase 5.4 "'Give me the scoop!' / 'Show the scoop' / 'The Scoop'" | |
| PRD-061 | Scoop structure: take, stack-up, Scoop, fit, verdict | `important` | `full` | Phase 7.2 "Structured output: personal take, honest stack-up, The Scoop centerpiece, fit/warnings, verdict" | |
| PRD-062 | Scoop length: 150-350 words | `detail` | `full` | Phase 7.2 "150-350 words target" | |
| PRD-063 | Detail header: backdrops/posters/logos/trailers | `critical` | `full` | Phase 5.4 "Header media carousel (backdrops/posters/logos, trailers when available)" | |
| PRD-064 | Core facts: year, runtime/seasons, genres | `important` | `full` | Phase 5.4 "Core facts (year, runtime/seasons, genres) + community score" | |
| PRD-065 | Community score + My Rating slider | `important` | `full` | Phase 5.4 section 3: "My Rating (slider...)" + Phase 4.2 RatingSlider | |
| PRD-066 | Rating unsaved auto-saves as Done | `important` | `full` | Phase 5.4 "Rating unsaved show -> saves as Done" | |
| PRD-067 | Status + Interest chips; saves/removes | `important` | `full` | Phase 5.4 section 4 describes chips, saving, and removal confirmation | |
| PRD-068 | Tags display + picker; tag unsaved auto-saves | `important` | `full` | Phase 5.4 section 5 + "Adding tag to unsaved show -> saves as Later + Interested" | |
| PRD-069 | Overview section | `detail` | `full` | Phase 5.4 section 6 "Overview text" | |
| PRD-070 | Ask about this show CTA | `important` | `full` | Phase 5.4 section 8 "'Ask about this show' CTA -> navigates to Ask with show context" | |
| PRD-071 | Traditional recommendations strand | `important` | `full` | Phase 5.4 section 9 "Recommendations strand (TMDB similar/recommended)" | |
| PRD-072 | Streaming availability section | `important` | `full` | Phase 5.4 section 11 "Streaming availability (providers by region)" | |
| PRD-073 | Cast & Crew to Person Detail | `important` | `full` | Phase 5.4 section 12 "Cast & Crew (horizontal strips -> Person Detail)" | |
| PRD-074 | Seasons section (TV only) | `detail` | `full` | Phase 5.4 section 13 "Seasons (TV only, expandable)" | |
| PRD-075 | Budget vs Revenue (movies) | `detail` | `full` | Phase 5.4 section 14 "Budget vs Revenue (movies, when data available)" | |
| PRD-076 | Section order preserved per hierarchy spec | `important` | `partial` | Phase 5.4 lists numbered sections in order | Plan's section order differs from detail_page_experience.md: plan puts Tags after Status (section 5), but spec puts Tags before Overview (section 3); plan omits separate Genres+Languages section that spec lists at position 6 |
| PRD-077 | Header graceful fallback | `important` | `partial` | Phase 5.4 mentions "trailers when available" | Plan does not explicitly describe fallback behaviour when backdrops/trailers are missing (poster-only, logo layout) |
| PRD-078 | Status chips in toolbar not scroll body | `detail` | `full` | Phase 5.4 "Status/Interest chips in toolbar" | |
| PRD-079 | Tile indicators: collection + rating badges | `detail` | `full` | Phase 4.2 ShowTile "handles in-collection indicator" + Phase 5.2 badges | |
| PRD-080 | Person: image gallery, name, bio | `important` | `full` | Phase 5.5 "Image gallery, name, bio" | |
| PRD-081 | Person: analytics charts | `important` | `full` | Phase 5.5 "Analytics charts: Average project ratings, Top genres, Projects per year" | |
| PRD-082 | Person: filmography grouped by year | `important` | `full` | Phase 5.5 "Filmography grouped by year" | |
| PRD-083 | Person: credit opens Show Detail | `important` | `full` | Phase 5.5 "Tapping a credit opens Show Detail" | |
| PRD-084 | Font size / readability setting | `important` | `full` | Phase 5.6 "Font size selector (XS-XXL)" | |
| PRD-085 | Search on launch toggle | `detail` | `full` | Phase 5.6 "Search on launch toggle" | |
| PRD-086 | Username setting | `detail` | `full` | Phase 5.6 "Username (editable)" | |
| PRD-087 | AI API key setting (not in repo) | `important` | `full` | Phase 5.6 "API key input (masked)" + Phase 0.2 server-only `AI_API_KEY` | |
| PRD-088 | AI model selection | `detail` | `full` | Phase 5.6 "model selection dropdown" | |
| PRD-089 | Catalog provider API key setting | `detail` | `full` | Phase 5.6 "TMDB API key input (masked)" | |
| PRD-090 | Export: .zip JSON backup, ISO-8601 dates | `important` | `full` | Phase 8.1 "Packages as StorageSnapshot JSON... .zip... All dates ISO-8601 encoded" | |
| PRD-091 | User fields track modification timestamps | `important` | `full` | Phase 1.1 schema includes all *_update_date columns | |
| PRD-092 | Non-my merge: selectFirstNonEmpty | `important` | `full` | Phase 2.3 "Non-my fields: selectFirstNonEmpty(newValue, oldValue) - never overwrite non-empty with empty" | |
| PRD-093 | My fields merge by timestamp | `important` | `full` | Phase 2.3 "My fields: resolve by timestamp - newer wins" | |
| PRD-094 | detailsUpdateDate set to now after merge | `detail` | `full` | Phase 2.3 "detailsUpdateDate set to now after merge" | |
| PRD-095 | creationDate set on first creation only | `detail` | `full` | Phase 2.3 "creation_date only set on first creation" | |
| PRD-096 | AI Scoop persistence + 4hr freshness | `important` | `full` | Phase 5.4 "4-hour freshness cache" + "Only persists if show is in collection" | |
| PRD-097 | Alchemy results are session-only | `important` | `partial` | Phase 5.3 Alchemy feature is session-based | Plan does not explicitly state that Alchemy results are ephemeral/session-only; this is implied by the feature being page-scoped but not stated as a data rule |
| PRD-098 | Ask chat history is session-only | `important` | `partial` | Phase 5.3 Ask feature is session-based | Plan does not explicitly state Ask history is session-only and cleared on leaving; implied but not stated |
| PRD-099 | Mentioned shows strip is session-only | `important` | `partial` | Phase 5.3 mentions strip derived from current context | Same issue: session-only lifecycle not explicitly called out as a data rule |
| PRD-100 | AI recs map to real shows via external ID | `critical` | `full` | Phase 3.2 parser: parseShowList, parseRecommendations; structured output with TMDB IDs | |
| PRD-101 | Unresolvable recs non-interactive or Search handoff | `important` | `full` | Phase 3.2 "Fallback to unstructured commentary + search handoff" | |
| PRD-102 | Parse failure: retry once then fallback | `important` | `full` | Phase 3.2 "Retry once on parse failure with stricter formatting instructions" | |
| PRD-103 | Data sync across devices, per-field conflict | `important` | `partial` | Phase 2.3 merge logic handles conflict resolution | Plan handles merge logic but does not describe a sync mechanism (real-time listeners, polling, etc.) or how cross-device sync is triggered |
| PRD-104 | Data continuity across updates, auto migration | `important` | `partial` | Phase 1.1 `app_metadata` table with `data_model_version` | Plan has a metadata version table but does not describe the migration strategy that preserves data when the model changes |
| PRD-105 | Transient data (cast, crew, etc.) not persisted | `important` | `full` | Phase 1.1 schema only includes persisted fields; Phase 3.1 fetches transient data per request | |
| PRD-106 | Local settings: autoSearch, fontSize | `detail` | `full` | Key Architectural Decisions section 2: "LocalStorage used only for UI preferences (font size, auto-search...)" | |
| PRD-107 | UI state: hideStatusRemovalConfirmation, count, lastFilter | `detail` | `partial` | Key Decisions mention "removal confirmation suppression" | Plan mentions suppression but does not detail the counter mechanism (statusRemovalCountKey) or lastSelectedFilter persistence |
| PRD-108 | CloudSettings version for conflict resolution | `detail` | `full` | Phase 1.1 cloud_settings table includes `version double precision` (epoch seconds) | |
| PRD-109 | AppMetadata tracks dataModelVersion | `detail` | `full` | Phase 1.1 app_metadata table with data_model_version | |
| PRD-110 | All user records scoped to user_id | `critical` | `full` | Phase 6.2 "All user-owned records scoped to (namespace_id, user_id)" + Phase 1.1 schema | |
| PRD-111 | Build uses stable namespace_id | `critical` | `full` | Phase 6.1 "namespace_id read from NEXT_PUBLIC_NAMESPACE_ID env var" | |
| PRD-112 | Two namespaces isolated | `critical` | `full` | Phase 6.1 "Every database query includes namespace_id" + Phase 9.2 integration test | |
| PRD-113 | Destructive testing scoped to namespace | `critical` | `full` | Phase 1.3 "DELETE... WHERE namespace_id = $1" + Phase 9.3 test data management | |
| PRD-114 | System behaves as if multiple users exist | `important` | `full` | Phase 6.2 "Dev-only user selector (dropdown in dev toolbar) for testing multi-user" | |
| PRD-115 | user_id is opaque string or UUID | `important` | `full` | Phase 0.2 "NEXT_PUBLIC_DEFAULT_USER_ID... Opaque stable string or UUID" | |
| PRD-116 | Effective partition (namespace_id, user_id) | `important` | `full` | Phase 1.1 UNIQUE(namespace_id, user_id, id) + Phase 6.2 | |
| PRD-117 | Dev identity injection for benchmark | `important` | `full` | Phase 6.3 devAuth.ts: "reads user_id from env or X-User-Id header" | |
| PRD-118 | Dev auth gated for production | `important` | `full` | Phase 6.3 "Gated behind NODE_ENV !== 'production' check" | |
| PRD-119 | OAuth migration: config changes not schema redesign | `important` | `full` | Phase 6.2 "Designed so replacing with real OAuth later requires only auth wiring changes" | |
| PRD-120 | User data stored server-side | `critical` | `full` | Key Decisions section 2: "No client-side IndexedDB or localStorage for collection data" | |
| PRD-121 | Clearing client storage doesn't lose data | `important` | `full` | Key Decisions section 2: "Supabase as sole persistence" for collection data | |
| PRD-122 | No global DB teardown to reset tests | `critical` | `full` | Phase 1.3 namespace-scoped reset + Phase 9.3 "No global database teardown required" | |
| PRD-123 | Must use Next.js (latest stable) | `critical` | `full` | Phase 0.1 "npx create-next-app@latest" with App Router | |
| PRD-124 | Must use Supabase as persistence | `critical` | `full` | Phase 0.4 Supabase client setup; entire persistence layer is Supabase | |
| PRD-125 | Must not assume Docker | `important` | `full` | Plan does not reference Docker as a requirement; uses hosted Supabase | |
| PRD-126 | .env.example with all variables | `critical` | `full` | Phase 0.2 lists full .env.example content | |
| PRD-127 | .gitignore excludes .env* except .env.example | `critical` | `full` | Phase 0.2 ".gitignore excludes .env* except .env.example" | |
| PRD-128 | Build runs via env vars, no code edits | `critical` | `full` | Phase 0.2 env validation + all config via env vars | |
| PRD-129 | Secrets not committed | `critical` | `full` | Phase 0.2 server-only keys (SUPABASE_SERVICE_ROLE_KEY, TMDB_API_KEY, AI_API_KEY) | |
| PRD-130 | Browser uses anon key; elevated key server-only | `important` | `full` | Phase 0.4 "browser client using anon key" + "server client using service role key (for API routes only)" | |
| PRD-131 | One-command scripts: dev, test, reset | `critical` | `full` | Phase 0.3 lists npm run dev, npm test, npm run test:reset | |
| PRD-132 | Repeatable migrations; deterministic fresh DB | `critical` | `full` | Phase 1.1 "001_initial_schema.sql" + Phase 0.3 "npm run db:migrate" | |
| PRD-133 | AI persona: fun chatty nerd friend | `important` | `full` | Phase 7.1 "'Fun, chatty TV/movie nerd friend' persona" | |
| PRD-134 | All AI surfaces one consistent persona | `important` | `full` | Phase 7.1 "Shared system prompt fragment used across all surfaces" | |
| PRD-135 | Voice pillars: joy-forward, honest, vibe-first, specific, concise | `important` | `full` | Phase 7.1 lists all pillars: "Joy-forward, opinionated honesty, vibe-first, specific not generic" | |
| PRD-136 | AI stays within TV/movies domain | `important` | `full` | Phase 7.1 "TV/movies domain only" | |
| PRD-137 | Taste-aware AI uses library + My Data | `important` | `full` | Phase 3.3 AI Context Builder: "Formats user library summary (titles, statuses, ratings, tags)" | |
| PRD-138 | Spoiler-safe by default | `important` | `full` | Phase 7.1 "Spoiler-safe by default" | |
| PRD-139 | Tone sliders: 70/30 friend/critic, 60/40 hype/measured | `detail` | `full` | Phase 7.1 "70% friend / 30% critic, 60% hype / 40% measured" | |
| PRD-140 | Search has no AI voice | `detail` | `missing` | `none` | Plan does not explicitly state that the Search surface has no AI personality |
| PRD-141 | Concepts: 1-3 words, evocative, no generic | `important` | `full` | Phase 7.2 Concepts prompt: "Bullet list only, 1-3 words each, evocative, no plot" | |
| PRD-142 | Concept specificity; "good characters" invalid | `important` | `partial` | Phase 7.2 mentions "evocative" concepts | Plan does not explicitly call out the anti-pattern of generic concepts or include validation/quality rules for concept output |
| PRD-143 | Concept diversity across axes | `important` | `partial` | Phase 7.2 "Covers structure/vibe/emotion/craft/genre-flavour axes" | Mentioned as prompt guidance but not as a validation or quality gate |
| PRD-144 | Concepts ordered by strength | `detail` | `partial` | Phase 7.2 "Ordered by strength (best 'aha' first)" | Mentioned as prompt instruction but no implementation detail on enforcing ordering |
| PRD-145 | Concept recs reference concepts in reasoning | `important` | `full` | Phase 7.2 "Each with concise reason naming which concepts align" | |
| PRD-146 | Concept recs: recent bias, allow classics | `detail` | `full` | Phase 7.2 "Recent bias but allows classics/hidden gems" | |
| PRD-147 | 8 concepts generated by default | `detail` | `missing` | `none` | Plan does not specify the default number of concepts generated |
| PRD-148 | Real-show integrity: recs map to real catalog items | `critical` | `full` | Phase 3.2 parser + structured TMDB IDs + Phase 7.2 "Real catalog items with TMDB IDs" | |

---

## 3. Coverage Scores

**Counts by coverage level:**

| Coverage | Critical | Important | Detail | Total |
|----------|----------|-----------|--------|-------|
| `full` | 33 | 77 | 19 | 129 |
| `partial` | 1 | 11 | 3 | 15 |
| `missing` | 0 | 1 | 3 | 4 |

**Unrounded totals:** 148 requirements; full=129, partial=15, missing=4.

```
Critical:  (33 * 1.0 + 1 * 0.5) / 34 * 100 = 98.5%  (33.5 of 34 critical requirements)
Important: (77 * 1.0 + 11 * 0.5) / 89 * 100 = 92.7%  (82.5 of 89 important requirements)
Detail:    (19 * 1.0 + 3 * 0.5) / 25 * 100 = 82.0%  (20.5 of 25 detail requirements)
Overall:   (129 * 1.0 + 15 * 0.5) / 148 * 100 = 92.2% (148 total requirements)
```

---

## 4. Top Gaps

1. **PRD-002 | `critical` | User's overlaid version displayed everywhere show appears**
   The plan implements user data display in individual components (tiles, detail page, search results) but does not articulate a cross-cutting contract that "whenever a show surfaces anywhere, the user's My Data overlay must be present." Without this as an explicit principle in the plan, it's easy for new surfaces (AI recommendation cards, Alchemy results, mentioned-shows strips) to render shows without checking for and displaying user data. This is the highest-severity gap because it's a foundational UX rule that affects every surface.

2. **PRD-103 | `important` | Data sync across devices, per-field conflict resolution**
   The plan includes merge logic that handles conflict resolution at the field level, but it does not describe how cross-device sync is actually triggered. There's no mention of Supabase realtime subscriptions, polling intervals, or any mechanism for a second device to detect and pull changes. The merge logic exists but the transport layer that exercises it is absent.

3. **PRD-104 | `important` | Data continuity across updates, auto migration**
   The plan includes an `app_metadata` table with `data_model_version` and numbered SQL migrations, but it does not describe an upgrade path for existing user data when the schema evolves. If a field is added, renamed, or restructured, the plan has no migration runner or strategy to transform existing rows. Users could lose data or encounter errors after an update.

4. **PRD-008 | `important` | Removal warning with option to suppress after repeated removals**
   The plan describes a confirmation dialog for show removal but omits the progressive suppression mechanism where, after several confirmations, the user is offered the option to stop seeing the warning. This maps to the `hideStatusRemovalConfirmation` and `statusRemovalCountKey` UI state from the storage schema. Without it, power users face unnecessary friction.

5. **PRD-054 | `important` | Selecting/unselecting concepts clears downstream results (Explore Similar)**
   The plan explicitly covers this rule for Alchemy ("changing shows clears concepts/results") but does not state the equivalent for Explore Similar: that changing concept selection should clear the recommendation results. This is an important state management detail that, if missed, would show stale recommendations misaligned with the currently selected concepts.

---

## 5. Coverage Narrative

**Overall posture.** This is a strong plan with gaps concentrated in edge-case specificity and cross-cutting contracts rather than missing features. Every major product surface is present: collection management, all five discovery paths (Search, Ask, Alchemy, Explore Similar, Scoop), the detail page, person profiles, settings, and data export. The plan also thoroughly addresses the infrastructure rider requirements -- namespace isolation, user identity, dev auth, environment configuration, and one-command scripts are all explicitly covered. At 92.2% overall coverage and 98.5% on critical requirements, the plan is ready for implementation with targeted amendments rather than structural rework.

**Strength clusters.** The plan is strongest in three areas. First, *infrastructure and isolation* (Identity & Isolation, Infrastructure & DevEx) is covered almost perfectly -- the namespace/user partitioning model, Supabase setup, dev auth gating, environment configuration, and test reset strategy are all concrete and well-specified. Second, *the core data layer* (Collection Management, Data Persistence) shows a deep understanding of the PRD's business rules: save triggers, default values, merge policy (selectFirstNonEmpty, timestamp-based My field resolution), and removal semantics are all directly addressed with planned unit tests. Third, *AI prompt architecture* (AI Personality, Ask, Alchemy, Explore Similar, Scoop) is unusually detailed for an implementation plan -- specific prompt files, structured output formats, voice pillar integration, and fallback strategies are all present.

**Weakness clusters.** The gaps are not randomly scattered; they cluster around two patterns. The first is *lifecycle and state management for ephemeral data*. The plan handles feature logic well but does not consistently articulate the data lifecycle rules: that Alchemy results, Ask history, and mentioned-shows strips are session-only and cleared on navigation, or that concept selection changes in Explore Similar should invalidate downstream results. These are specified in the PRD's "AI Data Persistence" table (section 5.7) and concept system spec, but the plan treats them as implicit rather than explicit contracts. The second cluster is *cross-cutting UX polish rules* -- the progressive removal-confirmation suppression, the "No Tags" filter, the detail page section ordering discrepancy relative to the supporting spec, and header fallback behaviour. These are individually minor but collectively suggest the supporting documents (detail_page_experience.md, storage-schema.md UI state section) were not consulted with the same rigour as the main PRD and infrastructure rider.

**Risk assessment.** If this plan were executed as-is, the most likely user-visible issue would be subtle inconsistency in how shows display across surfaces. A show the user has rated and tagged would correctly display its status badge on the Home page and in Search results, but might appear as a "bare" catalog item in an Alchemy recommendation row or a mentioned-shows strip in Ask, because no cross-cutting rule forces those surfaces to check the user's collection. A QA reviewer would notice first that the Alchemy results card for a show already in collection doesn't carry the user's rating badge. The second likely catch would be the lack of a sync transport: a user who opens the app on a second device and finds their collection empty (because no sync subscription or poll exists) would immediately lose trust, even though the merge logic is sound.

**Remediation guidance.** The gaps fall into three categories of remaining work. First, a short *data lifecycle addendum* is needed: a section (or additions to Phase 2) that explicitly specifies which data is ephemeral, what events clear it, and how concept-selection state changes propagate. This is a specification gap, not an architectural one. Second, the *cross-cutting display rule* (PRD-002) should be elevated to an explicit principle in the plan, with a utility or hook (e.g., `useShowWithOverlay`) that all show-rendering surfaces consume. Third, the *sync transport* and *data migration strategy* require genuine architectural decisions: will Supabase Realtime subscriptions be used? Will migrations run automatically on app load or via a separate script? These are not fit-and-finish items; they need planning before implementation begins.
