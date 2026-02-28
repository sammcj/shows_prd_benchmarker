# Plan Evaluation

## 1. Requirements Extraction

### Pass 1: Functional Areas

1. **Collection Management** — Status system, interest levels, tags, saving triggers, defaults, removal, collection display
2. **Search & Catalog** — External catalog search, result display, catalog integration
3. **AI Chat (Ask)** — Conversational discovery, mentions, session context, welcome view
4. **Alchemy Discovery** — Multi-show concept blending, chaining, step flow
5. **Explore Similar** — Per-show concept-based discovery from Detail
6. **AI Scoop** — Personality-driven show reviews, streaming, caching
7. **Concept System** — Concept generation quality, counts, selection rules
8. **Show Detail Page** — Layout, section hierarchy, critical states
9. **Person Detail** — Profiles, filmography, analytics charts
10. **Settings & User Data** — App settings, AI config, integrations, export/backup
11. **AI Voice & Personality** — Consistent persona, tone pillars, behavioral contracts
12. **Data Persistence & Business Rules** — Timestamps, merge policy, sync, continuity, AI data lifecycle
13. **Navigation & App Structure** — Layout, entry points, filters, mode switching
14. **Infrastructure & Isolation** — Namespace, identity, env setup, benchmark compliance, testing

### Pass 2: Requirements by Functional Area

#### Collection Management

- PRD-001 | `critical` | Show has My Data overlay (status, interest, tags, rating, scoop) | `showbiz_prd.md > 4.1 Show`
- PRD-002 | `critical` | User's overlaid version displayed wherever show appears | `showbiz_prd.md > 4.1 Show (Display Rule)`
- PRD-003 | `critical` | Five core statuses: Active, Later, Wait, Done, Quit | `showbiz_prd.md > 4.2 Status System`
- PRD-004 | `important` | Interested/Excited surface as primary chips, set Later + Interest | `showbiz_prd.md > 4.2 Status System`
- PRD-005 | `important` | Removing status removes show and clears all My Data with confirmation | `showbiz_prd.md > 4.2 + 5.4`
- PRD-006 | `important` | Interest only applies when status is Later | `showbiz_prd.md > 4.3 Interest Levels`
- PRD-007 | `important` | Tags are free-form labels powering filters and grouping | `showbiz_prd.md > 4.4 Tags`
- PRD-008 | `critical` | Show in collection iff it has an assigned status | `showbiz_prd.md > 5.1 Collection Membership`
- PRD-009 | `critical` | Four saving triggers: status, interest, rating unsaved, tag unsaved | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-010 | `critical` | Default save: Later+Interested; rating defaults to Done | `showbiz_prd.md > 5.3 Default Values`
- PRD-011 | `important` | Removal confirmation with option to suppress after repeated removals | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-012 | `detail` | Re-adding preserves My Data, refreshes public metadata, timestamp merge | `showbiz_prd.md > 5.5 Re-adding the Same Show`
- PRD-013 | `important` | Collection grouped: Active (large), Excited, Interested, Others collapsed | `showbiz_prd.md > 7.1 Collection Home`
- PRD-014 | `important` | Media-type toggle: All / Movies / TV | `showbiz_prd.md > 7.1 Collection Home`
- PRD-015 | `detail` | Empty states: no collection prompts Search/Ask; filter yields "No results" | `showbiz_prd.md > 7.1 Collection Home`
- PRD-016 | `detail` | Tiles show poster, title, and My Data badges | `showbiz_prd.md > 7.1 Collection Home`
- PRD-017 | `detail` | Tile in-collection indicator and user rating indicator | `showbiz_prd.md > 5.9 Tile Indicators`

#### Search & Catalog

- PRD-018 | `critical` | Text search by title/keywords in global catalog | `showbiz_prd.md > 7.2 Search`
- PRD-019 | `important` | Search results in poster grid with in-collection items marked | `showbiz_prd.md > 7.2 Search`
- PRD-020 | `important` | Selecting search result opens Show Detail | `showbiz_prd.md > 7.2 Search`
- PRD-021 | `detail` | Search auto-opens on launch if "Search on Launch" enabled | `showbiz_prd.md > 7.2 Search`
- PRD-022 | `important` | Search has no AI voice (straightforward catalog experience) | `ai_voice_personality.md > 1 Persona Summary`

#### AI Chat (Ask)

- PRD-023 | `critical` | Chat UI with user/assistant turns for conversational discovery | `showbiz_prd.md > 7.3 Ask`
- PRD-024 | `important` | Friendly, opinionated, spoiler-safe tone in Ask | `showbiz_prd.md > 7.3 Ask`
- PRD-025 | `important` | AI mentions shows inline; mentioned shows in horizontal strip | `showbiz_prd.md > 7.3 Ask`
- PRD-026 | `important` | Tapping mentioned show opens Detail or Search handoff | `showbiz_prd.md > 7.3 Ask`
- PRD-027 | `detail` | Welcome view: 6 random starter prompts with refresh | `showbiz_prd.md > 7.3 Ask`
- PRD-028 | `important` | Session context retained; older turns auto-summarized after ~10 msgs | `showbiz_prd.md > 7.3 Ask`
- PRD-029 | `important` | Ask About Show from Detail seeds conversation with show context | `showbiz_prd.md > 7.3 Ask`
- PRD-030 | `important` | Structured output: commentary + showList (Title::externalId::mediaType;;) | `ai_prompting_context.md > 3.2 Ask with Mentions`
- PRD-031 | `detail` | Conversation summarization preserves persona tone | `ai_prompting_context.md > 4 Conversation Summarization`

#### Alchemy Discovery

- PRD-032 | `critical` | Select 2+ shows, conceptualize, select concepts (max 8), get 6 recs | `showbiz_prd.md > 7.4 Alchemy`
- PRD-033 | `important` | Alchemy input shows from both library and global catalog | `showbiz_prd.md > 4.7 Alchemy Session`
- PRD-034 | `important` | Step clarity with cards/sections and backtracking allowed | `showbiz_prd.md > 7.4 Alchemy`
- PRD-035 | `important` | Chaining: use results as new inputs for another round | `showbiz_prd.md > 4.7 Alchemy Session`
- PRD-036 | `detail` | Changing shows clears concepts and results | `showbiz_prd.md > 7.4 Alchemy`

#### Explore Similar

- PRD-037 | `critical` | Get Concepts -> select concepts -> Explore Shows for AI recs | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-038 | `important` | Explore Similar returns 5 recs per round | `concept_system.md > 6 Concepts -> Recommendations Contract`
- PRD-039 | `important` | Recs have concise reasons referencing selected concepts | `concept_system.md > 6 + ai_prompting_context.md > 3.5`
- PRD-040 | `detail` | UI hint "pick the ingredients you want more of" | `concept_system.md > 5 Selection UX Rules`

#### AI Scoop

- PRD-041 | `important` | AI-generated personality-driven spoiler-safe description | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-042 | `important` | Generated on demand from Show Detail | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-043 | `important` | Cached with 4-hour freshness, regenerated on demand | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-044 | `important` | Persisted only if show is in user's collection | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-045 | `important` | Streams progressively; user sees "Generating..." not blank | `detail_page_experience.md > 3.4 Overview + Scoop`
- PRD-046 | `detail` | Toggle copy: "Give me the scoop!" / "Show the scoop" / "The Scoop" | `detail_page_experience.md > 3.4 Overview + Scoop`
- PRD-047 | `important` | Structured as mini blog: personal take, stack-up, centerpiece, fit, verdict | `ai_prompting_context.md > 3.3 Scoop`

#### Concept System

- PRD-048 | `important` | Concepts are 1-3 word evocative ingredients (vibe/structure/craft) | `concept_system.md > 4 Generation Rules`
- PRD-049 | `important` | Specificity over genericity; no "good characters" etc. | `concept_system.md > 4 + discovery_quality_bar.md > 2.3`
- PRD-050 | `important` | Multi-show concepts must be shared across all inputs | `concept_system.md > 4 Generation Rules`
- PRD-051 | `detail` | 8 concepts generated by default | `discovery_quality_bar.md > 2.3 Concepts`
- PRD-052 | `detail` | Concepts diverse across axes (structure/vibe/emotion/craft) | `concept_system.md > 4 Generation Rules`
- PRD-053 | `detail` | Concepts ordered by strength (best "aha" first) | `concept_system.md > 4 Generation Rules`

#### Show Detail Page

- PRD-054 | `critical` | Show Detail as single source of truth: public facts + My Data + discovery | `showbiz_prd.md > 7.5 Show Detail`
- PRD-055 | `important` | 12-section narrative hierarchy preserved in order | `detail_page_experience.md > 3 Narrative Hierarchy`
- PRD-056 | `important` | Header media carousel with graceful fallback to poster/logo | `detail_page_experience.md > 3.1 Header Media`
- PRD-057 | `important` | Status/interest chips in toolbar (not scroll body) | `detail_page_experience.md > 3.3 My Relationship Controls`
- PRD-058 | `important` | Streaming availability section | `showbiz_prd.md > 7.5 Show Detail #10`
- PRD-059 | `important` | Cast & Crew horizontal strands linking to Person Detail | `showbiz_prd.md > 7.5 Show Detail #11`
- PRD-060 | `detail` | Seasons section (TV only) | `showbiz_prd.md > 7.5 Show Detail #12`
- PRD-061 | `detail` | Budget vs Revenue (movies, when available) | `showbiz_prd.md > 7.5 Show Detail #13`
- PRD-062 | `detail` | TV vs Movie handled gracefully (seasons vs runtime, episode counts) | `detail_page_experience.md > 5 Critical States`

#### Person Detail

- PRD-063 | `important` | Person profile: image gallery, name, bio | `showbiz_prd.md > 7.6 Person Detail`
- PRD-064 | `important` | Analytics charts: avg ratings, top genres, projects-by-year | `showbiz_prd.md > 7.6 + 4.10 Person`
- PRD-065 | `important` | Filmography grouped by year | `showbiz_prd.md > 7.6 Person Detail`
- PRD-066 | `important` | Selecting a credit opens Show Detail | `showbiz_prd.md > 7.6 Person Detail`

#### Settings & User Data

- PRD-067 | `important` | Font size / readability setting | `showbiz_prd.md > 7.7 Settings`
- PRD-068 | `detail` | Search on launch setting | `showbiz_prd.md > 7.7 Settings`
- PRD-069 | `important` | Username setting (synced if enabled) | `showbiz_prd.md > 7.7 Settings`
- PRD-070 | `important` | AI provider API key setting (env var in benchmark; never committed) | `showbiz_prd.md > 7.7 Settings`
- PRD-071 | `important` | AI model selection setting | `showbiz_prd.md > 7.7 Settings`
- PRD-072 | `important` | Catalog provider API key setting | `showbiz_prd.md > 7.7 Settings`
- PRD-073 | `critical` | Export My Data: .zip with JSON backup, ISO-8601 dates | `showbiz_prd.md > 7.7 Settings + 8 Cross-Cutting #6`

#### AI Voice & Personality

- PRD-074 | `important` | Consistent AI persona across all surfaces (one character) | `ai_voice_personality.md > 1 Persona Summary`
- PRD-075 | `important` | Five voice pillars: joy-forward, opinionated honesty, vibe-first, specific, concise | `ai_voice_personality.md > 2 Voice Pillars`
- PRD-076 | `important` | Stay within TV/movies domain; redirect if asked to leave | `ai_prompting_context.md > 1 Shared Rules`
- PRD-077 | `important` | Spoiler-safe by default across all AI surfaces | `ai_prompting_context.md > 1 Shared Rules`
- PRD-078 | `important` | AI is taste-aware: uses library + My Data + session context | `ai_prompting_context.md > 2 Shared Inputs`

#### Data Persistence & Business Rules

- PRD-079 | `important` | Per-field modification timestamps for all user fields | `showbiz_prd.md > 5.6 Timestamps`
- PRD-080 | `important` | AI data persistence rules (Scoop persisted, Alchemy/Ask session-only) | `showbiz_prd.md > 5.7 AI Data Persistence`
- PRD-081 | `critical` | AI recommendations map to real shows via external ID lookup | `showbiz_prd.md > 5.8 + 8 Cross-Cutting #2`
- PRD-082 | `important` | Non-my field merge: selectFirstNonEmpty (never overwrite with empty) | `storage-schema.md > Merge/overwrite policy`
- PRD-083 | `important` | My-field merge by timestamp (newer wins) | `storage-schema.md > Merge/overwrite policy`
- PRD-084 | `detail` | Optional cross-device sync with per-field timestamp resolution | `showbiz_prd.md > 5.10 Data Sync`
- PRD-085 | `important` | Data continuity: preserve libraries across updates with auto-migration | `showbiz_prd.md > 5.11 Data Continuity`
- PRD-086 | `important` | Unresolvable recs shown non-interactive or Search handoff | `ai_prompting_context.md > 5 Guardrails`
- PRD-087 | `detail` | Failed AI parsing: retry once then fallback to unstructured + Search | `ai_prompting_context.md > 5 Guardrails`

#### Navigation & App Structure

- PRD-088 | `important` | Filters/navigation panel with sidebar views over collection | `showbiz_prd.md > 6 App Structure`
- PRD-089 | `important` | Persistent Find/Discover and Settings entry points | `showbiz_prd.md > 6 App Structure`
- PRD-090 | `important` | Find/Discover hub with mode switcher: Search, Ask, Alchemy | `showbiz_prd.md > 6 App Structure`
- PRD-091 | `important` | Filter types: All, per-tag, "No tags", genre, decade, score, media-type | `showbiz_prd.md > 4.5 Filters`

#### Infrastructure & Isolation

- PRD-092 | `critical` | Next.js (latest stable) as application runtime | `showbiz_infra_rider_prd.md > 2 Benchmark Baseline`
- PRD-093 | `critical` | Supabase as persistence layer | `showbiz_infra_rider_prd.md > 2 Benchmark Baseline`
- PRD-094 | `critical` | .env.example with all required variables | `showbiz_infra_rider_prd.md > 3.1 Env variable interface`
- PRD-095 | `important` | .gitignore excludes .env* except .env.example | `showbiz_infra_rider_prd.md > 3.1`
- PRD-096 | `important` | Build runs by filling env vars, no code edits | `showbiz_infra_rider_prd.md > 3.1`
- PRD-097 | `important` | Secrets not committed; client uses anon key, elevated keys server-only | `showbiz_infra_rider_prd.md > 3.1`
- PRD-098 | `critical` | One-command scripts: start app, run tests, reset test data | `showbiz_infra_rider_prd.md > 3.2`
- PRD-099 | `important` | Repeatable schema definition (migrations) for deterministic DB state | `showbiz_infra_rider_prd.md > 3.3`
- PRD-100 | `critical` | Namespace isolation: builds don't read/write each other's data | `showbiz_infra_rider_prd.md > 4.1`
- PRD-101 | `critical` | All user-owned records associated with user_id | `showbiz_infra_rider_prd.md > 4.2`
- PRD-102 | `important` | Effective data partition: (namespace_id, user_id) | `showbiz_infra_rider_prd.md > 4.3`
- PRD-103 | `important` | Dev identity injection (documented, gated for production) | `showbiz_infra_rider_prd.md > 5.1`
- PRD-104 | `important` | OAuth migration requires config changes only, not schema redesign | `showbiz_infra_rider_prd.md > 5.2`
- PRD-105 | `critical` | Backend is source of truth; clearing client storage doesn't lose data | `showbiz_infra_rider_prd.md > 6.1 + showbiz_prd.md > 8 #9`
- PRD-106 | `important` | Destructive testing scoped to namespace, no global teardown | `showbiz_infra_rider_prd.md > 7`
- PRD-107 | `important` | Docker not required for benchmark | `showbiz_infra_rider_prd.md > 8`

Total: 107 requirements (20 critical, 68 important, 19 detail) across 14 functional areas

## 2. Coverage Table

| PRD-ID | Requirement | Severity | Coverage | Evidence | Gap |
| ------ | ----------- | -------- | -------- | -------- | --- |
| PRD-001 | Show has My Data overlay (status, interest, tags, rating, scoop) | critical | full | Section 1 Data Model: MyData entity with status, interest, rating, tags, scoop_id | |
| PRD-002 | User's overlaid version displayed wherever show appears | critical | partial | Section 1: MyData joined with Show via show_id FK | Plan never states the "display rule" -- that every show surface must prefer the user-overlaid version over raw catalog data |
| PRD-003 | Five core statuses: Active, Later, Wait, Done, Quit | critical | full | Section 10 Migration SQL: `status TEXT NOT NULL CHECK (status IN ('active', 'later', 'wait', 'done', 'quit'))` | |
| PRD-004 | Interested/Excited surface as primary chips, set Later + Interest | important | full | Section 4.3 Toolbar: "Status chips: Active, Later+Interested, Later+Excited, Wait, Done, Quit" | |
| PRD-005 | Removing status removes show and clears all My Data with confirmation | important | full | Section 2 API: "DELETE /api/collection/:showId -- Clears MyData (removal confirmation required); May cascade delete AI_Scoop" | |
| PRD-006 | Interest only applies when status is Later | important | partial | Section 1: `interest ... | null (only for 'later' status)` as comment | No enforcement of what happens to interest when status changes away from Later; SQL constraint doesn't link interest to status |
| PRD-007 | Tags are free-form labels powering filters and grouping | important | full | Section 1: MyData.tags field; Section 2 API: tag filter param; Section 4.1: StatusFilterBar | |
| PRD-008 | Show in collection iff it has an assigned status | critical | full | Section 1: MyData requires non-null status; Section 16: "Collection Membership" decision | |
| PRD-009 | Four saving triggers: status, interest, rating unsaved, tag unsaved | critical | full | Section 2 API POST /api/collection: all four auto-save triggers listed explicitly | |
| PRD-010 | Default save: Later+Interested; rating defaults to Done | critical | full | Section 2 API: "Rating unsaved show -> creates MyData with status='done'" and "Adding tag -> status='later', interest='interested'" | |
| PRD-011 | Removal confirmation with option to suppress after repeated removals | important | partial | Section 2 API: "removal confirmation required" | Missing: option to suppress confirmation after repeated removals (hideStatusRemovalConfirmation) |
| PRD-012 | Re-adding preserves My Data, refreshes public metadata, timestamp merge | detail | missing | none | Plan does not address re-adding a previously saved show or preserving existing My Data on re-encounter |
| PRD-013 | Collection grouped: Active (large), Excited, Interested, Others collapsed | important | partial | Section 4.1: CollectionList and StatusFilterBar components | Missing: 4-group hierarchy with Active as larger tiles, Excited/Interested sections, and Others as collapsed group |
| PRD-014 | Media-type toggle: All / Movies / TV | important | partial | Section 4.1: StatusFilterBar exists | Missing: dedicated media-type toggle; API collection endpoint lacks media-type filter param |
| PRD-015 | Empty states: no collection prompts Search/Ask; filter yields "No results" | detail | partial | Section 4.1: EmptyState component exists | Missing: two distinct empty state behaviours (no collection vs. filter yields none) |
| PRD-016 | Tiles show poster, title, and My Data badges | detail | full | Section 4.1: "ShowCard: displays show poster, title, status chip, rating, quick actions" | |
| PRD-017 | Tile in-collection indicator and user rating indicator | detail | partial | Section 4.1: ShowCard shows status chip and rating | Missing: explicit in-collection badge as a standalone indicator on tiles across all surfaces |
| PRD-018 | Text search by title/keywords in global catalog | critical | full | Section 2 API: "GET /api/search -- Query params: q, page, limit"; Section 4.2 SearchMode | |
| PRD-019 | Search results in poster grid with in-collection items marked | important | partial | Section 4.2: SearchResults component | Missing: explicit in-collection marking in search results |
| PRD-020 | Selecting search result opens Show Detail | important | full | Section 4.2: SearchMode results link to Show Detail page | |
| PRD-021 | Search auto-opens on launch if "Search on Launch" enabled | detail | missing | none | Plan Settings section does not include "Search on Launch" setting; no autoSearch mechanism addressed |
| PRD-022 | Search has no AI voice (straightforward catalog experience) | important | full | Section 4.2 SearchMode: "No AI voice (straightforward catalog search)" | |
| PRD-023 | Chat UI with user/assistant turns for conversational discovery | critical | full | Section 4.2 AskMode: ChatInput, ChatMessage, MentionedShows components | |
| PRD-024 | Friendly, opinionated, spoiler-safe tone in Ask | important | full | Section 11 Ask Prompt: "Respond like a friend in dialogue... Be willing to pick favorites confidently" | |
| PRD-025 | AI mentions shows inline; mentioned shows in horizontal strip | important | full | Section 4.2: MentionedShows component; Section 2 API: showList in Ask response | |
| PRD-026 | Tapping mentioned show opens Detail or Search handoff | important | partial | Section 4.2: MentionedShows component exists | Missing: explicit behaviour for tapping (navigate to Detail or Search handoff if mapping fails) |
| PRD-027 | Welcome view: 6 random starter prompts with refresh | detail | missing | none | Plan does not address Ask welcome view, starter prompts, or refresh mechanism |
| PRD-028 | Session context retained; older turns auto-summarized after ~10 msgs | important | partial | Section 4.2 AskMode: "Session management (create, load, summarize old turns)" | Missing: ~10 message threshold for summarization trigger |
| PRD-029 | Ask About Show from Detail seeds conversation with show context | important | full | Section 4.3: "AskAboutShow.tsx # CTA that seeds Ask with show context" | |
| PRD-030 | Structured output: commentary + showList (Title::externalId::mediaType;;) | important | full | Section 2 API: "Returns: { commentary, showList? }"; Section 7 showListParser.ts with format spec | |
| PRD-031 | Conversation summarization preserves persona tone | detail | partial | Section 4.2: "summarize old turns" mentioned | Missing: explicit requirement that summaries preserve persona tone rather than sterile voice |
| PRD-032 | Select 2+ shows, conceptualize, select concepts (max 8), get 6 recs | critical | full | Section 4.2 AlchemyMode: "Select >= 2 shows -> generate concepts -> select concepts -> get recommendations; 6 recommendations per round" | |
| PRD-033 | Alchemy input shows from both library and global catalog | important | partial | Section 4.2: InputShowSelector component | Missing: explicit mention that input shows can come from both collection and global catalog |
| PRD-034 | Step clarity with cards/sections and backtracking allowed | important | partial | Section 4.2: "State machine: selecting -> generating -> selecting recs" | Missing: explicit UX for step clarity (cards/sections) and backtracking behaviour |
| PRD-035 | Chaining: use results as new inputs for another round | important | partial | Section 4.2 AlchemyMode state machine described | Missing: explicit "More Alchemy!" chaining mechanism using results as new inputs |
| PRD-036 | Changing shows clears concepts and results | detail | partial | Section 4.2: state machine implies state transitions | Missing: explicit rule that changing input shows clears downstream concepts and results |
| PRD-037 | Get Concepts -> select concepts -> Explore Shows for AI recs | critical | full | Section 4.3 ExploreSimilar: "Get Concepts -> display concept chips -> select -> Explore Shows -> 5 recommendations" | |
| PRD-038 | Explore Similar returns 5 recs per round | important | full | Section 4.3: "Display 5 recommendations with reasons"; Section 8: EXPLORE_SIMILAR_RECS = 5 | |
| PRD-039 | Recs have concise reasons referencing selected concepts | important | full | Section 11 Explore Similar Prompt: "Provide concise reason... that explicitly references selected concepts" | |
| PRD-040 | UI hint "pick the ingredients you want more of" | detail | missing | none | Plan does not include concept selection copy guidance |
| PRD-041 | AI-generated personality-driven spoiler-safe description | important | full | Section 11 Scoop Prompt: personality-driven "taste review"; shared voice pillar "vibe-first, spoiler-safe" | |
| PRD-042 | Generated on demand from Show Detail | important | full | Section 4.3: ScoopSection with useScoop hook; Section 2 API: POST /api/shows/:id/scoop | |
| PRD-043 | Cached with 4-hour freshness, regenerated on demand | important | full | Section 4.3: "Freshness check: regenerate after 4 hours"; Section 8: scoopFreshnessMs constant | |
| PRD-044 | Persisted only if show is in user's collection | important | full | Section 4.3: "Persistence: only if show in collection" | |
| PRD-045 | Streams progressively; user sees "Generating..." not blank | important | full | Section 4.3: 'Progressive streaming: "Generating..." -> content' | |
| PRD-046 | Toggle copy: "Give me the scoop!" / "Show the scoop" / "The Scoop" | detail | full | Section 4.3: 'Toggle: "Give me the scoop!" -> "Show the scoop" -> "The Scoop" (open)' | |
| PRD-047 | Structured as mini blog: personal take, stack-up, centerpiece, fit, verdict | important | full | Section 11 Scoop Prompt: all five sections listed (personal take, stack-up, Scoop, fit/warnings, verdict) | |
| PRD-048 | Concepts are 1-3 word evocative ingredients (vibe/structure/craft) | important | full | Section 11 Concepts Prompt: "1-3 words each; Vibe/structure/thematic ingredients" | |
| PRD-049 | Specificity over genericity; no "good characters" etc. | important | full | Section 11 Concepts Prompt: 'Avoid: "good characters", "great story", "funny"' | |
| PRD-050 | Multi-show concepts must be shared across all inputs | important | full | Section 2 API: "Multi-show: concepts must be shared across all inputs" | |
| PRD-051 | 8 concepts generated by default | detail | full | Section 11: "Generate 8 short, evocative concept 'ingredients'"; Section 8: CONCEPT_COUNT = 8 | |
| PRD-052 | Concepts diverse across axes (structure/vibe/emotion/craft) | detail | partial | Section 11: prompt mentions "Vibe/structure/thematic ingredients" | Missing: explicit diversity requirement across distinct axes (structure, vibe, emotion, dynamics, craft) |
| PRD-053 | Concepts ordered by strength (best "aha" first) | detail | full | Section 11 Concepts Prompt: "Order by strength (best 'aha' concepts first)" | |
| PRD-054 | Show Detail as single source of truth: public facts + My Data + discovery | critical | full | Section 4.3: ShowDetail page with all major sections covering facts, user data, and discovery | |
| PRD-055 | 12-section narrative hierarchy preserved in order | important | full | Section 4.3: "Section Order (narrative hierarchy)" lists all 12 sections matching the PRD | |
| PRD-056 | Header media carousel with graceful fallback to poster/logo | important | partial | Section 4.3: HeaderCarousel with MediaPlayer component | Missing: graceful fallback behaviour when trailers/backdrops are unavailable |
| PRD-057 | Status/interest chips in toolbar (not scroll body) | important | full | Section 4.3: Toolbar as separate feature with StatusChips, RatingBar, TagsInput sub-components | |
| PRD-058 | Streaming availability section | important | full | Section 4.3: ProvidersSection component at position 9 in hierarchy | |
| PRD-059 | Cast & Crew horizontal strands linking to Person Detail | important | full | Section 4.3: Cast, Crew at position 10; Person Detail page linked via /persons/[id] | |
| PRD-060 | Seasons section (TV only) | detail | full | Section 4.3: "Seasons (TV only)" at position 11 in hierarchy | |
| PRD-061 | Budget vs Revenue (movies, when available) | detail | full | Section 4.3: "Budget/Revenue (movies)" at position 12 in hierarchy | |
| PRD-062 | TV vs Movie handled gracefully (seasons vs runtime, episode counts) | detail | partial | Section 4.3: seasons and budget/revenue sections distinguished by media type | Missing: explicit graceful handling of runtime vs episode counts for TV vs Movie |
| PRD-063 | Person profile: image gallery, name, bio | important | full | Section 4.4: PersonHeader component | |
| PRD-064 | Analytics charts: avg ratings, top genres, projects-by-year | important | partial | Section 4.4 Analytics: AvgRating, GenreBreakdown components; API returns avgRating, genreBreakdown | Missing: projects-by-year chart and its backing API data |
| PRD-065 | Filmography grouped by year | important | full | Section 4.4: Filmography with FilmographyItem component | |
| PRD-066 | Selecting a credit opens Show Detail | important | full | Section 4.4: FilmographyItem navigates to Show Detail (implied by routing) | |
| PRD-067 | Font size / readability setting | important | missing | none | Plan Settings page does not include font size or readability controls |
| PRD-068 | Search on launch setting | detail | missing | none | Plan Settings page does not include Search on Launch toggle |
| PRD-069 | Username setting (synced if enabled) | important | missing | none | Plan omits CloudSettings entity entirely; no username field in Settings UI or data model |
| PRD-070 | AI provider API key setting (env var in benchmark; never committed) | important | partial | Section 9: OPENAI_API_KEY in .env.example; Section 4.5: Integrations section exists | Missing: explicit API key management UI in Settings for user-entered keys |
| PRD-071 | AI model selection setting | important | partial | Section 4.5: AICustomization exists with VoiceSelector | Missing: AI model selection control (VoiceSelector addresses persona, not model choice) |
| PRD-072 | Catalog provider API key setting | important | partial | Section 4.5: Integrations section exists | Missing: catalog API key in .env.example and explicit UI for managing it |
| PRD-073 | Export My Data: .zip with JSON backup, ISO-8601 dates | critical | partial | Section 4.5: ExportButton component exists | Missing: export format specification (.zip with JSON content, ISO-8601 date encoding) |
| PRD-074 | Consistent AI persona across all surfaces (one character) | important | full | Section 11: single shared system prompt used by all AI surfaces | |
| PRD-075 | Five voice pillars: joy-forward, honesty, vibe-first, specific, concise | important | full | Section 11: all five voice pillars listed verbatim in shared system prompt | |
| PRD-076 | Stay within TV/movies domain; redirect if asked to leave | important | full | Section 11: "Stay within TV/movies domain. If asked outside, redirect back." | |
| PRD-077 | Spoiler-safe by default across all AI surfaces | important | full | Section 11: "Vibe-first, spoiler-safe" pillar; Scoop Prompt reinforces | |
| PRD-078 | AI is taste-aware: uses library + My Data + session context | important | partial | Section 11: AI prompts defined; Section 2 API: Ask takes context param | Missing: explicit mechanism for injecting user's library and My Data into AI context for taste-awareness |
| PRD-079 | Per-field modification timestamps for all user fields | important | partial | Section 1: MyData has created_at, updated_at, scoop_updated_at | Missing: separate per-field timestamps (myStatusUpdateDate, myInterestUpdateDate, myTagsUpdateDate, myScoreUpdateDate) |
| PRD-080 | AI data persistence rules (Scoop persisted, Alchemy/Ask session-only) | important | partial | Scoop persistence and Session entity addressed | Missing: explicit rules stating Alchemy results and mentioned shows strip are session-only |
| PRD-081 | AI recommendations map to real shows via external ID lookup | critical | full | Section 2 API: external_id-based lookups; Section 11: "Return real show with valid external catalog ID"; Section 13: Real-Show Integrity | |
| PRD-082 | Non-my field merge: selectFirstNonEmpty (never overwrite with empty) | important | missing | none | Plan does not address catalog data merge policy for non-user fields |
| PRD-083 | My-field merge by timestamp (newer wins) | important | missing | none | Plan does not address per-field timestamp-based conflict resolution for My Data fields |
| PRD-084 | Optional cross-device sync with per-field timestamp resolution | detail | missing | none | Plan does not address cross-device sync mechanism |
| PRD-085 | Data continuity: preserve libraries across updates with auto-migration | important | missing | none | Plan has no data model versioning or migration strategy for preserving user data across app updates |
| PRD-086 | Unresolvable recs shown non-interactive or Search handoff | important | missing | none | Plan AI prompts require real shows but no fallback for when resolution fails |
| PRD-087 | Failed AI parsing: retry once then fallback to unstructured + Search | detail | missing | none | Plan does not address AI output parsing failure recovery |
| PRD-088 | Filters/navigation panel with sidebar views over collection | important | partial | Section 4.1: StatusFilterBar component | Missing: full sidebar/navigation panel with All Shows view, tag filters, data filters |
| PRD-089 | Persistent Find/Discover and Settings entry points | important | full | Section 3: search.tsx and settings.tsx as top-level pages; Navbar component | |
| PRD-090 | Find/Discover hub with mode switcher: Search, Ask, Alchemy | important | full | Section 4.2: ModeSwitcher component "Tabs: Search | Ask | Alchemy" | |
| PRD-091 | Filter types: All, per-tag, "No tags", genre, decade, score, media-type | important | partial | Section 2 API: status and tag filters in collection endpoint | Missing: genre, decade, community score filters; "No tags" filter |
| PRD-092 | Next.js (latest stable) as application runtime | critical | full | Plan built entirely on Next.js; Section 3 page structure, Section 14 scripts | |
| PRD-093 | Supabase as persistence layer | critical | full | Section 10: Supabase migrations; Section 14: Supabase setup; Section 9: env vars | |
| PRD-094 | .env.example with all required variables | critical | full | Section 9: .env.example defined with Supabase, AI, Dev Identity, App variables | |
| PRD-095 | .gitignore excludes .env* except .env.example | important | full | Section 9: .gitignore listing excludes all .env variants | |
| PRD-096 | Build runs by filling env vars, no code edits | important | full | Section 15: build process uses `cp .env.example .env.local` then standard commands | |
| PRD-097 | Secrets not committed; client uses anon key, elevated keys server-only | important | full | Section 9: NEXT_PUBLIC_ prefix for client-safe keys; OPENAI_API_KEY without prefix (server-only) | |
| PRD-098 | One-command scripts: start app, run tests, reset test data | critical | full | Section 14: package.json scripts for dev, test, test:reset, db:migrate, db:reset | |
| PRD-099 | Repeatable schema definition (migrations) for deterministic DB state | important | full | Section 10: supabase/migrations/001_initial_schema.sql with full SQL | |
| PRD-100 | Namespace isolation: builds don't read/write each other's data | critical | full | Section 1: namespace_id on all user tables; Section 10: RLS policies; Section 14: reset_namespace_test_data scoped | |
| PRD-101 | All user-owned records associated with user_id | critical | full | Section 1: user_id on MyData, AI_Scoop, Concept, Session; Section 10: RLS policies | |
| PRD-102 | Effective data partition: (namespace_id, user_id) | important | full | Section 1: UNIQUE(namespace_id, user_id, show_id) constraint on MyData | |
| PRD-103 | Dev identity injection (documented, gated for production) | important | full | Section 14: getCurrentUserId(), getNamespaceId(), withIdentity middleware with env-based dev mode | |
| PRD-104 | OAuth migration requires config changes only, not schema redesign | important | full | Section 16: "OAuth migration path exists without schema changes"; user_id is opaque string | |
| PRD-105 | Backend is source of truth; clearing client storage doesn't lose data | critical | full | Section 16: "Client cache is disposable; server is source of truth" | |
| PRD-106 | Destructive testing scoped to namespace, no global teardown | important | full | Section 14: reset_namespace_test_data SQL function scoped to namespace_uuid | |
| PRD-107 | Docker not required for benchmark | important | full | Section 15: no Docker in build process; hosted Supabase as primary path | |

## 3. Coverage Scores

**Score by severity tier:**

```
Critical:  (18 x 1.0 + 2 x 0.5) / 20 x 100 = 95.00%  (19 of 20 critical requirements covered)
Important: (42 x 1.0 + 20 x 0.5) / 68 x 100 = 76.47%  (52 of 68 important requirements covered)
Detail:    (6 x 1.0 + 6 x 0.5) / 19 x 100 = 47.37%  (9 of 19 detail requirements covered)
Overall:   (66 x 1.0 + 28 x 0.5) / 107 x 100 = 74.77% (107 total requirements)
```

**Breakdown:**

| Severity | Full | Partial | Missing | Total |
| -------- | ---- | ------- | ------- | ----- |
| critical | 18 | 2 | 0 | 20 |
| important | 42 | 20 | 6 | 68 |
| detail | 6 | 6 | 7 | 19 |
| **Total** | **66** | **28** | **13** | **107** |

## 4. Top Gaps

**1. PRD-082 | `important` | Non-my field merge: selectFirstNonEmpty (never overwrite with empty)**

The storage schema mandates that catalog refreshes never overwrite a non-empty stored string/array with an empty value. Without this, a catalog provider returning partial data (e.g., missing overview or genres on a re-fetch) will silently blank out fields the user relies on for browsing. This corrupts the library experience without any user action.

**2. PRD-083 | `important` | My-field merge by timestamp (newer wins)**

The PRD requires per-field timestamp-based conflict resolution for My Data fields (status, interest, tags, rating). The plan uses a single generic `updated_at` on MyData rather than per-field timestamps. Without this, any sync or concurrent-edit scenario risks overwriting a user's intentional field change with a stale value from another device or session -- directly violating the "user's version takes precedence" cross-cutting rule.

**3. PRD-085 | `important` | Data continuity: preserve libraries across updates with auto-migration**

The PRD explicitly states users must never lose their collection due to an update. The plan includes no data model versioning, no AppMetadata entity tracking `dataModelVersion`, and no migration strategy for evolving the schema while preserving user data. In a benchmark context where the schema will iterate, this is a structural omission that risks data loss.

**4. PRD-086 | `important` | Unresolvable recs shown non-interactive or Search handoff**

The AI prompts require recommendations to resolve to real catalog items, but there is no fallback pathway when resolution fails. The PRD and ai_prompting_context.md both specify that unresolvable titles should appear non-interactive or be handed off to Search. Without this, a hallucinated or unresolvable AI recommendation becomes a dead end or a runtime error rather than a graceful degradation.

**5. PRD-073 | `critical` | Export My Data: .zip with JSON backup, ISO-8601 dates**

The plan includes an ExportButton component but specifies nothing about the export format. The PRD requires a `.zip` containing JSON with ISO-8601 encoded dates. "Your data is yours" is a cross-cutting principle (Rule #6), making data portability a first-class concern. Without format specification, the export feature is an empty shell -- the component exists but the contract is undefined.

## 5. Coverage Narrative

### Overall Posture

This is a structurally sound plan with good coverage of the core product loop -- collection management, discovery surfaces, AI integration, and benchmark infrastructure -- but with concerning gaps in data integrity rules and settings completeness. At 74.8% overall coverage, the plan would produce a functional prototype that demonstrates the product's primary value proposition, but would ship with silent data corruption risks and an incomplete settings experience. The 95% critical coverage is strong; the problem lies in the important tier (76.5%) and particularly the detail tier (47.4%), where accumulated omissions erode the polish that distinguishes a working prototype from a faithful rebuild.

### Strength Clusters

The plan is strongest in two areas. First, **infrastructure and isolation** is nearly flawless: namespace partitioning, user identity scoping, dev identity injection, RLS policies, destructive testing support, environment variable management, and OAuth migration readiness are all concretely specified with working code samples. A benchmark evaluator checking rider compliance would find little to fault. Second, **AI prompting and personality** is thoroughly covered: the shared system prompt, surface-specific prompts (Scoop, Ask, Concepts, Explore Similar), voice pillars, and quality bar dimensions are all present with the right level of specificity. The plan correctly identifies the AI persona as a shared resource rather than per-surface reimplementation. The **Show Detail page** is also well-specified, with the 12-section hierarchy, toolbar controls, Scoop toggle states, and Explore Similar flow all explicitly addressed with component structures and hooks.

### Weakness Clusters

Gaps concentrate in three areas rather than being randomly scattered. The most concerning cluster is **data integrity and merge rules**: the plan omits the selectFirstNonEmpty merge policy for catalog data, per-field timestamp-based conflict resolution for My Data, the re-adding/preservation behaviour, data continuity across version updates, and the CloudSettings/AppMetadata entities from the storage schema. These are not edge cases -- they are the rules that prevent silent data corruption during normal usage (catalog refreshes, device switching, app updates). The second cluster is **settings and configuration completeness**: font size, search on launch, username, explicit API key management UI, AI model selection, and catalog API key are all either missing or only partially addressed. The plan's Settings page has three feature sections but their contents don't match the PRD's settings inventory. The third cluster is **UX detail and edge-case behaviour**: fallback states (header without trailers, unresolvable recommendations, AI parsing failures), Ask welcome view with starter prompts, media-type toggle, collection grouping hierarchy, filter completeness (genre, decade, score, "No tags"), and copy guidance for concept selection are all missing or underspecified.

### Risk Assessment

If this plan were executed as-is, the most likely failure mode would surface during **catalog data refresh cycles**. When a user's saved show is refreshed from the catalog provider and the provider returns incomplete data (a common occurrence with metadata APIs), the absence of the selectFirstNonEmpty merge policy means existing overview text, genre lists, or image URLs could be silently overwritten with nulls. A user browsing their collection would see shows with missing posters, blank overviews, or empty genre tags -- with no indication that data was lost and no way to recover it. The second failure a QA reviewer would catch is the **settings page**, which would feel hollow: no font size control, no username, no model selector, no catalog key management, despite the PRD listing all of these as explicit features. A stakeholder reviewing the settings screen would immediately notice the gap between the plan's ambition (VoiceSelector, SpoilerPreference) and the PRD's actual requirements.

### Remediation Guidance

The data integrity cluster requires **new plan sections**, not just more detail on existing ones. The plan needs a dedicated "Data Merge & Sync" section specifying the selectFirstNonEmpty rule, per-field timestamp schema changes (replacing the single `updated_at` with field-specific timestamps), the re-adding flow, and an AppMetadata entity for data model versioning. This is architectural work that changes the schema and API contracts. The settings cluster requires **acceptance criteria expansion**: each setting from the PRD needs a corresponding component and data model entry, which is primarily a completeness exercise rather than an architectural decision. The UX detail cluster requires **missing specification work**: starter prompts, fallback behaviours, collection grouping hierarchy, and filter types need to be specified as concrete acceptance criteria rather than left implicit. None of these require fundamental rethinking of the plan's architecture -- the fractal structure, API design, and AI integration approach are all sound. The remediation is additive, filling in the rules and edge cases that turn a working skeleton into a faithful implementation.
