# Plan Evaluation: Showbiz Benchmark

---

## 1. Requirements Extraction

### Pass 1: Functional Areas

1. **Collection & Status Management** -- Saving triggers, default values, removal, re-adding, collection membership, status types, interest levels
2. **Tags & Filtering** -- Free-form tags, sidebar filters, data filters, media-type toggle
3. **Collection Home** -- Library display, status grouping, tile layout, empty states
4. **Search & Catalog Integration** -- External catalog search, result display, field mapping, merge rules
5. **AI Chat (Ask)** -- Conversational discovery, mentioned shows, starter prompts, Ask About a Show
6. **AI Scoop** -- Personality reviews, freshness/caching, persistence, streaming, toggle UX
7. **Alchemy** -- Multi-show concept blending, step flow, chaining, session data
8. **Explore Similar & Concepts** -- Per-show concepts, concept generation rules, quality constraints, selection UX
9. **Show Detail Page** -- Narrative hierarchy, sections, toolbar controls, critical states
10. **Person Detail** -- Person profiles, filmography, analytics, credit navigation
11. **Settings & User Data** -- Configuration, export/backup, API keys, username
12. **AI Voice & Personality** -- Persona consistency, voice pillars, domain guardrails, fallbacks
13. **Data Persistence & Integrity** -- Schema, timestamps, merge rules, sync, data continuity, tile indicators
14. **Infrastructure & Isolation** -- Namespace, user identity, dev auth, env vars, DX scripts, cloud-agent compatibility

### Pass 2: Requirements by Functional Area

#### Collection & Status Management

- PRD-001 | `critical` | Show has user overlay "My Data" fields | `showbiz_prd.md > 4.1 Show`
- PRD-002 | `critical` | User-overlaid version displayed everywhere show appears | `showbiz_prd.md > 4.1 Show`
- PRD-003 | `critical` | Collection membership defined by assigned status | `showbiz_prd.md > 5.1 Collection Membership`
- PRD-004 | `critical` | Four saving triggers (status, interest chip, rating, tag) | `showbiz_prd.md > 5.2 Saving Triggers`
- PRD-005 | `important` | Default save without explicit status: Later + Interested | `showbiz_prd.md > 5.3 Default Values`
- PRD-006 | `important` | First save via rating defaults status to Done | `showbiz_prd.md > 5.3 Default Values`
- PRD-007 | `important` | Clearing status removes show and clears all My Data | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-008 | `detail` | Removal confirmation suppressible after repeated removals | `showbiz_prd.md > 5.4 Removing from Collection`
- PRD-009 | `important` | Re-adding preserves My Data and refreshes public metadata | `showbiz_prd.md > 5.5 Re-adding`
- PRD-010 | `critical` | Core statuses: Active, Later, Wait, Done, Quit | `showbiz_prd.md > 4.2 Status System`
- PRD-011 | `important` | Interested/Excited chips map to Later + interest level | `showbiz_prd.md > 4.2 Status System`
- PRD-012 | `important` | Interest only applies when status is Later | `showbiz_prd.md > 4.3 Interest Levels`

#### Tags & Filtering

- PRD-013 | `important` | Free-form tags forming implicit personal tag library | `showbiz_prd.md > 4.4 Tags`
- PRD-014 | `important` | Sidebar tag filters: one per tag plus "No tags" | `showbiz_prd.md > 4.5 Filters`
- PRD-015 | `important` | Data filters: genre, decade, community score ranges | `showbiz_prd.md > 4.5 Filters`
- PRD-016 | `important` | Media-type toggle: All / Movies / TV on any filter | `showbiz_prd.md > 4.5 Filters`

#### Collection Home

- PRD-017 | `critical` | Library grouped by status: Active, Excited, Interested, Other | `showbiz_prd.md > 7.1 Collection Home`
- PRD-018 | `detail` | Active tiles prominent/larger; Other statuses collapsed | `showbiz_prd.md > 7.1 Collection Home`
- PRD-019 | `detail` | Tiles show poster, title, and My Data badges | `showbiz_prd.md > 7.1 Collection Home`
- PRD-020 | `detail` | Empty states: no collection prompts Search/Ask; empty filter shows message | `showbiz_prd.md > 7.1 Collection Home`

#### Search & Catalog Integration

- PRD-021 | `critical` | Text search by title/keywords in external catalog | `showbiz_prd.md > 7.2 Search`
- PRD-022 | `important` | Results in poster grid with in-collection markers | `showbiz_prd.md > 7.2 Search`
- PRD-023 | `detail` | Search auto-opens on launch if "Search on Launch" enabled | `showbiz_prd.md > 7.2 Search`
- PRD-024 | `important` | External catalog to Show field mapping rules | `storage-schema.md > External catalog > Show mapping`
- PRD-025 | `important` | Merge: non-my fields use selectFirstNonEmpty; my fields use timestamps | `storage-schema.md > Merge / overwrite policy`

#### AI Chat (Ask)

- PRD-026 | `critical` | Chat UI with user/assistant turns for conversational discovery | `showbiz_prd.md > 7.3 Ask`
- PRD-027 | `important` | Mentioned shows in horizontal strip, selectable | `showbiz_prd.md > 7.3 Ask`
- PRD-028 | `important` | Structured output with commentary + showList delimiter format | `ai_prompting_context.md > 3.2 Ask with Mentions`
- PRD-029 | `detail` | Welcome view: 6 random starter prompts, refreshable | `showbiz_prd.md > 7.3 Ask`
- PRD-030 | `important` | Context retained; older turns summarised after ~10 messages | `showbiz_prd.md > 7.3 Ask`
- PRD-031 | `important` | Ask About a Show from Detail with seeded show context | `showbiz_prd.md > 7.3 Ask`

#### AI Scoop

- PRD-032 | `important` | Generated on demand from Detail, spoiler-safe default | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-033 | `important` | 4-hour cache freshness, regenerates on demand | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-034 | `important` | Persisted only if show is in user's collection | `showbiz_prd.md > 4.9 AI Scoop`
- PRD-035 | `important` | Structured as mini blog post (take, stack-up, Scoop centerpiece, fit, verdict) | `ai_prompting_context.md > 3.3 Scoop`
- PRD-036 | `detail` | Streams progressively; "Generating..." not blank wait | `detail_page_experience.md > 3.4 Scoop UX`
- PRD-037 | `detail` | Toggle copy: "Give me the scoop!" / "Show the scoop" / "The Scoop" | `detail_page_experience.md > 3.4 Scoop UX`
- PRD-038 | `detail` | Scoop length target 150-350 words | `ai_voice_personality.md > 4.1 Scoop`

#### Alchemy

- PRD-039 | `critical` | Select 2+ starting shows from library + global catalog | `showbiz_prd.md > 4.7 Alchemy`
- PRD-040 | `critical` | AI extracts shared concept catalysts from selected shows | `showbiz_prd.md > 4.7 Alchemy`
- PRD-041 | `important` | User selects 1-8 concepts | `showbiz_prd.md > 4.7 Alchemy`
- PRD-042 | `important` | AI returns 6 recommended shows with short reasons | `showbiz_prd.md > 4.7 Alchemy`
- PRD-043 | `important` | Chain another round using results as new inputs | `showbiz_prd.md > 4.7 Alchemy`
- PRD-044 | `important` | Backtracking: changing shows clears concepts/results | `showbiz_prd.md > 7.4 Alchemy`
- PRD-045 | `detail` | Step clarity with cards/sections | `showbiz_prd.md > 7.4 Alchemy`
- PRD-046 | `detail` | Alchemy results and reasons are session-only (not persisted) | `showbiz_prd.md > 5.7 AI Data Persistence`

#### Explore Similar & Concepts

- PRD-047 | `important` | Get Concepts extracts concepts for a single show | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-048 | `important` | User selects concepts then Explore Shows for AI recs | `showbiz_prd.md > 4.8 Explore Similar`
- PRD-049 | `important` | Explore Similar returns 5 recommendations per round | `concept_system.md > 6. Concepts > Recommendations`
- PRD-050 | `important` | Generate 8 concepts by default | `discovery_quality_bar.md > 2.3 Concepts`
- PRD-051 | `important` | Concepts: 1-3 words, evocative, bullet list, no spoilers | `concept_system.md > 4. Generation Rules`
- PRD-052 | `important` | Multi-show concepts must be shared across all inputs | `concept_system.md > 4. Generation Rules`
- PRD-053 | `detail` | Concept quality: specificity, diversity, ordered by strength | `concept_system.md > 4. Generation Rules`
- PRD-054 | `detail` | Selecting/unselecting concepts clears downstream results | `concept_system.md > 5. Selection UX Rules`
- PRD-055 | `detail` | UI hint: "pick the ingredients you want more of" | `concept_system.md > 5. Selection UX Rules`

#### Show Detail Page

- PRD-056 | `critical` | Narrative hierarchy preserved (12 sections in specified order) | `detail_page_experience.md > 3. Narrative Hierarchy`
- PRD-057 | `important` | Header media carousel with graceful fallback | `showbiz_prd.md > 7.5 Show Detail`
- PRD-058 | `important` | Core facts row + community score visible quickly | `showbiz_prd.md > 7.5 Show Detail`
- PRD-059 | `important` | Status/interest chips in sticky toolbar (not scroll body) | `detail_page_experience.md > 3.3 My Relationship Controls`
- PRD-060 | `important` | Traditional recommendations strand of similar shows | `showbiz_prd.md > 7.5 Show Detail`
- PRD-061 | `important` | Streaming availability section | `showbiz_prd.md > 7.5 Show Detail`
- PRD-062 | `important` | Cast & Crew horizontal strands linking to Person Detail | `showbiz_prd.md > 7.5 Show Detail`
- PRD-063 | `detail` | Seasons section (TV only) | `showbiz_prd.md > 7.5 Show Detail`
- PRD-064 | `detail` | Budget vs Revenue (movies when available) | `showbiz_prd.md > 7.5 Show Detail`
- PRD-065 | `detail` | Graceful fallback for missing media (poster/logo layout) | `detail_page_experience.md > 5. Critical States`
- PRD-066 | `detail` | TV vs Movie runtime/episode counts handled gracefully | `detail_page_experience.md > 5. Critical States`

#### Person Detail

- PRD-067 | `important` | Person profiles: image gallery, name, bio | `showbiz_prd.md > 7.6 Person Detail`
- PRD-068 | `important` | Filmography grouped by year | `showbiz_prd.md > 7.6 Person Detail`
- PRD-069 | `detail` | Analytics charts (ratings, genres, projects-by-year) | `showbiz_prd.md > 7.6 Person Detail`
- PRD-070 | `important` | Selecting a credit opens Show Detail | `showbiz_prd.md > 7.6 Person Detail`

#### Settings & User Data

- PRD-071 | `detail` | Font size / readability setting | `showbiz_prd.md > 7.7 Settings`
- PRD-072 | `detail` | Search on launch setting | `showbiz_prd.md > 7.7 Settings`
- PRD-073 | `detail` | Username setting (synced if enabled) | `showbiz_prd.md > 7.7 Settings`
- PRD-074 | `important` | AI provider API key management (env in benchmark) | `showbiz_prd.md > 7.7 Settings`
- PRD-075 | `detail` | AI model selection setting (synced if enabled) | `showbiz_prd.md > 7.7 Settings`
- PRD-076 | `detail` | Content catalog provider API key setting | `showbiz_prd.md > 7.7 Settings`
- PRD-077 | `important` | Export My Data: .zip with JSON backup, ISO-8601 dates | `showbiz_prd.md > 7.7 Settings`

#### AI Voice & Personality

- PRD-078 | `important` | All AI surfaces feel like one consistent persona | `ai_voice_personality.md > 1. Persona Summary`
- PRD-079 | `detail` | Search has no AI voice (straightforward catalog) | `ai_voice_personality.md > 1. Persona Summary`
- PRD-080 | `important` | Opinionated honesty; acknowledge mixed reception | `ai_voice_personality.md > 2. Voice Pillars`
- PRD-081 | `detail` | Specific not generic reasoning (concrete flavour) | `ai_voice_personality.md > 2. Voice Pillars`
- PRD-082 | `important` | All AI surfaces stay within TV/movies domain | `ai_prompting_context.md > 1. Shared Rules`
- PRD-083 | `important` | Taste-aware AI: uses library + My Data + session context | `showbiz_prd.md > 8. Cross-Cutting Rules`
- PRD-084 | `important` | AI recommendations map to real selectable shows | `showbiz_prd.md > 5.8 AI Recommendations`
- PRD-085 | `detail` | Unresolvable recs shown non-interactive or handed to Search | `showbiz_prd.md > 5.8 AI Recommendations`
- PRD-086 | `detail` | Structured parsing: retry once then fallback to unstructured | `ai_prompting_context.md > 5. Guardrails`
- PRD-087 | `detail` | Conversation summaries preserve persona tone | `ai_prompting_context.md > 4. Summarization`
- PRD-088 | `detail` | Recommendation reasons reflect selected concepts | `ai_prompting_context.md > 3.5 Concept-Based Recs`
- PRD-089 | `detail` | Don't list a show without a reason | `ai_voice_personality.md > 6. Do/Don't`

#### Data Persistence & Integrity

- PRD-090 | `critical` | All user fields track modification timestamps | `showbiz_prd.md > 5.6 Timestamps`
- PRD-091 | `important` | Timestamps used for sorting, conflict resolution, cache freshness | `showbiz_prd.md > 5.6 Timestamps`
- PRD-092 | `important` | Tile indicators: in-collection badge + user rating badge | `showbiz_prd.md > 5.9 Tile Indicators`
- PRD-093 | `important` | Preserve user libraries across updates (data continuity) | `showbiz_prd.md > 5.11 Data Continuity`
- PRD-094 | `critical` | Backend is source of truth; clearing client storage safe | `showbiz_prd.md > 8. Cross-Cutting Rules`
- PRD-095 | `detail` | Transient fields (cast, crew, etc.) not persisted, re-pullable | `storage-schema.md > Stored entities`
- PRD-096 | `detail` | ProviderData stores provider IDs by region | `storage-schema.md > ProviderData`
- PRD-097 | `detail` | AppMetadata tracks dataModelVersion for migrations | `storage-schema.md > AppMetadata`
- PRD-098 | `detail` | UI state persistence: filter memory, removal confirmation counter | `storage-schema.md > UI state`

#### Navigation & App Structure

- PRD-099 | `important` | Filters/navigation panel in layout | `showbiz_prd.md > 6. App Structure`
- PRD-100 | `important` | Persistent Find/Discover entry point in primary navigation | `showbiz_prd.md > 6. App Structure`
- PRD-101 | `important` | Persistent Settings entry point in primary navigation | `showbiz_prd.md > 6. App Structure`
- PRD-102 | `important` | Find/Discover hub: Search, Ask, Alchemy with mode switcher | `showbiz_prd.md > 6. App Structure`

#### Infrastructure & Isolation

- PRD-103 | `critical` | Must use Next.js (latest stable) | `showbiz_infra_rider_prd.md > 2. Benchmark Baseline`
- PRD-104 | `critical` | Must use Supabase as persistence layer | `showbiz_infra_rider_prd.md > 2. Benchmark Baseline`
- PRD-105 | `critical` | .env.example with all required variables | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-106 | `important` | .gitignore excludes .env* secrets (except .env.example) | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-107 | `important` | Build configurable via env vars without code edits | `showbiz_infra_rider_prd.md > 3.1 Environment variable interface`
- PRD-108 | `critical` | Secrets must not be committed to repo | `showbiz_infra_rider_prd.md > 3.1 Credential handling`
- PRD-109 | `important` | Browser/client uses anon key; elevated keys server-only | `showbiz_infra_rider_prd.md > 3.1 Credential handling`
- PRD-110 | `important` | Script: start app | `showbiz_infra_rider_prd.md > 3.2 One-command DX`
- PRD-111 | `important` | Script: run tests | `showbiz_infra_rider_prd.md > 3.2 One-command DX`
- PRD-112 | `important` | Script: reset test data for namespace | `showbiz_infra_rider_prd.md > 3.2 One-command DX`
- PRD-113 | `important` | Repeatable schema definition; fresh state deterministic | `showbiz_infra_rider_prd.md > 3.3 Database evolution`
- PRD-114 | `critical` | Namespace isolation: no cross-namespace data leaks | `showbiz_infra_rider_prd.md > 4.1 Build/run namespace`
- PRD-115 | `critical` | All user-owned records associated with user_id | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-116 | `important` | System behaves as if multiple users could exist | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-117 | `important` | user_id as opaque string (no provider encoding) | `showbiz_infra_rider_prd.md > 4.2 User identity`
- PRD-118 | `important` | Dev identity injection: documented, production-gated | `showbiz_infra_rider_prd.md > 5. Authentication`
- PRD-119 | `important` | OAuth migration requires config only, not schema redesign | `showbiz_infra_rider_prd.md > 5.2 Migration`
- PRD-120 | `important` | Persisted user data stored server-side | `showbiz_infra_rider_prd.md > 6.1 Source of truth`
- PRD-121 | `important` | Client cache is disposable (clearing safe) | `showbiz_infra_rider_prd.md > 6.2 Cache`
- PRD-122 | `important` | Destructive testing without global database teardown | `showbiz_infra_rider_prd.md > 7. Destructive Testing`
- PRD-123 | `important` | Docker must not be required to run benchmark | `showbiz_infra_rider_prd.md > 8. Cloud Agent Compatibility`

```
Total: 123 requirements (19 critical, 69 important, 35 detail) across 14 functional areas
```

---

## 2. Coverage Table

| PRD-ID | Requirement | Severity | Coverage | Evidence | Gap |
| ------ | ----------- | -------- | -------- | -------- | --- |
| PRD-001 | Show has user overlay "My Data" fields | `critical` | `full` | Phase 1.3 schema (my_status, my_interest, my_tags, my_score, ai_scoop); Phase 2.1 type definitions | |
| PRD-002 | User-overlaid version displayed everywhere show appears | `critical` | `partial` | Phase 4.5 Show Detail shows My Data; Phase 4.1 Home shows status | Plan does not mandate user overlay in search results, AI outputs, or recommendation tiles universally |
| PRD-003 | Collection membership defined by assigned status | `critical` | `full` | Phase 4.1 LibrarySection groups by status; Phase 2.3 getByStatus | |
| PRD-004 | Four saving triggers (status, interest, rating, tag) | `critical` | `full` | Phase 4.5 toolbar: status chips save, rating auto-saves Done, tag auto-saves Later + Interested | |
| PRD-005 | Default save without explicit status: Later + Interested | `important` | `partial` | Phase 4.5: "adding tag to unsaved auto-saves as Later + Interested" | Only tag-save default specified; general default policy not defined |
| PRD-006 | First save via rating defaults status to Done | `important` | `full` | Phase 4.5: "Rating bar (auto-saves as Done)" | |
| PRD-007 | Clearing status removes show and clears all My Data | `important` | `partial` | Phase 4.5: "Reselecting triggers removal confirmation" | Confirmation mentioned but "clears all My Data" not stated explicitly |
| PRD-008 | Removal confirmation suppressible after repeated removals | `detail` | `missing` | none | Suppression option not addressed |
| PRD-009 | Re-adding preserves My Data and refreshes public metadata | `important` | `partial` | Phase 2.2: merge logic with timestamp conflict resolution | Merge logic handles data, but re-adding flow and UX not explicit |
| PRD-010 | Core statuses: Active, Later, Wait, Done, Quit | `critical` | `full` | Phase 4.5: "Status chips: Active, Later, Excited, Wait, Done, Quit" | |
| PRD-011 | Interested/Excited chips map to Later + interest level | `important` | `partial` | Phase 4.5 lists Excited as a status chip | Plan does not explain that Interested/Excited set status=Later + interest level |
| PRD-012 | Interest only applies when status is Later | `important` | `missing` | none | Interest-status coupling rule not addressed |
| PRD-013 | Free-form tags forming implicit personal tag library | `important` | `partial` | Phase 4.5: "My Tags: tag display + picker" | Tags exist but implicit tag library concept not specified |
| PRD-014 | Sidebar tag filters: one per tag plus "No tags" | `important` | `missing` | none | Only status filtering mentioned; tag sidebar filters absent |
| PRD-015 | Data filters: genre, decade, community score ranges | `important` | `missing` | none | Data filters entirely absent from plan |
| PRD-016 | Media-type toggle: All / Movies / TV on any filter | `important` | `missing` | none | Media-type toggle not mentioned |
| PRD-017 | Library grouped by status: Active, Excited, Interested, Other | `critical` | `full` | Phase 4.1: "Active, Excited, Interested, Other groups" | |
| PRD-018 | Active tiles prominent/larger; Other statuses collapsed | `detail` | `missing` | none | Tile sizing and collapse behaviour not specified |
| PRD-019 | Tiles show poster, title, and My Data badges | `detail` | `partial` | Phase 4.1: "Show cards with poster, title, status chip" | Status chip only; in-collection and rating badges not specified |
| PRD-020 | Empty states for no collection and empty filter results | `detail` | `missing` | none | Empty state copy and behaviour not addressed |
| PRD-021 | Text search by title/keywords in external catalog | `critical` | `full` | Phase 4.2: "Search input with debounced query"; Phase 2.4: "POST /api/shows/search" | |
| PRD-022 | Results in poster grid with in-collection markers | `important` | `partial` | Phase 4.2: "Results grid with pagination" | Grid mentioned but not poster-specific layout; in-collection markers absent |
| PRD-023 | Search auto-opens on launch if enabled | `detail` | `partial` | Phase 4.6: autoSearch setting exists | Setting exists but auto-open launch behaviour not connected |
| PRD-024 | External catalog to Show field mapping rules | `important` | `partial` | Phase 2.2: "mergeCatalogIntoShow() function" | Function planned but no field mapping rules detailed |
| PRD-025 | Merge: non-my selectFirstNonEmpty; my fields timestamps | `important` | `full` | Phase 2.2: "Implement selectFirstNonEmpty() for catalog fields" + "timestamp-based conflict resolution for user fields" | |
| PRD-026 | Chat UI with conversational discovery turns | `critical` | `full` | Phase 4.3: "Chat interface with message list", "Input field for user queries" | |
| PRD-027 | Mentioned shows in horizontal strip, selectable | `important` | `full` | Phase 4.3: "'Mentioned shows' row (parsed from AI response)" | |
| PRD-028 | Structured output: commentary + showList delimiter format | `important` | `partial` | Phase 4.3: "parseMentionedShows"; Phase 3.2: "Parse structured output" | Parsing planned but delimiter-based contract (Title::externalId::mediaType;;) not specified |
| PRD-029 | Welcome view: 6 random starter prompts, refreshable | `detail` | `missing` | none | Starter prompts not mentioned |
| PRD-030 | Context retained; older turns summarised after ~10 messages | `important` | `partial` | Phase 3.2: "Maintain conversation history (summarize old turns)" | Summarisation mentioned but ~10 message threshold not specified |
| PRD-031 | Ask About a Show from Detail with seeded context | `important` | `partial` | Phase 4.5 point 5: "'Ask about this show' CTA" | CTA listed but seeding conversation with show context not described |
| PRD-032 | Generated on demand from Detail, spoiler-safe default | `important` | `partial` | Phase 3.2: "POST /api/ai/scoop"; Phase 3.1: "spoiler-safe" in persona | On-demand and spoiler-safe each partially covered across separate sections |
| PRD-033 | 4-hour cache freshness, regenerates on demand | `important` | `full` | Phase 3.2: "Cache: 4 hours"; Phase 4.5: "Freshness: regenerate after 4 hours" | |
| PRD-034 | Persisted only if show is in collection | `important` | `full` | Phase 3.2: "only persist if show in collection" | |
| PRD-035 | Structured as mini blog post (take, stack-up, centerpiece, fit, verdict) | `important` | `partial` | Phase 3.1: "mini blog-post of taste" | Label used but required subsections (take, stack-up, centerpiece, fit, verdict) not enumerated |
| PRD-036 | Streams progressively; "Generating..." not blank wait | `detail` | `full` | Phase 3.2: "Stream response if supported"; Phase 4.5: "Streams progressively" | |
| PRD-037 | Toggle copy: three distinct label states | `detail` | `partial` | Phase 4.5: "'Give me the scoop!' / 'Show the scoop'" | Two of three states listed; "The Scoop" open-state title missing |
| PRD-038 | Scoop length target 150-350 words | `detail` | `full` | Phase 3.1: "150-350 words" | |
| PRD-039 | Select 2+ starting shows from library + global catalog | `critical` | `full` | Phase 4.4: "Step 1: Select 2+ shows from library or search" | |
| PRD-040 | AI extracts shared concept catalysts | `critical` | `full` | Phase 4.4: "Step 2: Generate concepts from selected shows"; Phase 3.2: "shared concepts" | |
| PRD-041 | User selects 1-8 concepts | `important` | `full` | Phase 4.4: "Step 3: Select up to 8 concepts" | |
| PRD-042 | AI returns 6 recommended shows with reasons | `important` | `full` | Phase 3.2: "6 recs (Alchemy)"; Phase 4.4: "Recommendation cards with reasons" | |
| PRD-043 | Chain another round using results as new inputs | `important` | `missing` | none | Alchemy chaining ("More Alchemy!") entirely absent |
| PRD-044 | Backtracking: changing shows clears concepts/results | `important` | `missing` | none | Backtracking behaviour not addressed |
| PRD-045 | Step clarity with cards/sections | `detail` | `partial` | Phase 4.4 describes 4 steps sequentially | Steps listed but cards/sections UX not specified |
| PRD-046 | Alchemy results session-only (not persisted) | `detail` | `missing` | none | Session-only data lifecycle not stated |
| PRD-047 | Get Concepts extracts concepts for single show | `important` | `partial` | Phase 4.5 point 8: "Explore Similar (concepts > recs)"; Phase 3.2: "Single-show" | Referenced at high level but Get Concepts flow not detailed |
| PRD-048 | User selects concepts then Explore Shows for AI recs | `important` | `partial` | Phase 4.5: "Explore Similar (concepts > recs)" | Abbreviated; selection flow not described |
| PRD-049 | Explore Similar returns 5 recs per round | `important` | `full` | Phase 3.2: "5 recs (Explore Similar)" | |
| PRD-050 | Generate 8 concepts by default | `important` | `full` | Phase 3.2: "Return 8 concepts by default" | |
| PRD-051 | Concepts: 1-3 words, evocative, bullet list, no spoilers | `important` | `full` | Phase 3.1: "Concepts: Bullet list, 1-3 words, evocative" | |
| PRD-052 | Multi-show concepts must be shared across all inputs | `important` | `full` | Phase 3.2: "Single-show or multi-show (shared concepts)" | |
| PRD-053 | Concept quality: specificity, diversity, ordered by strength | `detail` | `missing` | none | Quality constraints not mentioned |
| PRD-054 | Selecting/unselecting concepts clears downstream results | `detail` | `missing` | none | Downstream clearing behaviour not addressed |
| PRD-055 | UI hint: "pick the ingredients you want more of" | `detail` | `missing` | none | Concept selection copy guidance absent |
| PRD-056 | Narrative hierarchy (12 sections in order) | `critical` | `full` | Phase 4.5 lists all 12 sections in correct order | |
| PRD-057 | Header media carousel with graceful fallback | `important` | `partial` | Phase 4.5 point 1: "Header carousel (backdrop/poster/logo/trailer)" | Media listed but graceful fallback not specified |
| PRD-058 | Core facts row + community score visible quickly | `important` | `full` | Phase 4.5 point 2: "Core facts row (year/runtime + community score)" | |
| PRD-059 | Status/interest chips in sticky toolbar | `important` | `full` | Phase 4.5: "Toolbar (sticky): Status chips" | |
| PRD-060 | Traditional recommendations strand | `important` | `full` | Phase 4.5 point 7: "Traditional recommendations strand" | |
| PRD-061 | Streaming availability section | `important` | `full` | Phase 4.5 point 9: "Providers ('Stream It')" | |
| PRD-062 | Cast & Crew strands linking to Person Detail | `important` | `partial` | Phase 4.5 point 10: "Cast, Crew" | Cast/Crew listed but no link to Person Detail (page missing from plan) |
| PRD-063 | Seasons section (TV only) | `detail` | `full` | Phase 4.5 point 11: "Seasons (TV only)" | |
| PRD-064 | Budget vs Revenue (movies when available) | `detail` | `full` | Phase 4.5 point 12: "Budget/Revenue (movies)" | |
| PRD-065 | Graceful fallback for missing media (poster/logo) | `detail` | `missing` | none | Fallback states for missing media not addressed |
| PRD-066 | TV vs Movie runtime/episodes handled gracefully | `detail` | `missing` | none | Conditional display logic not specified |
| PRD-067 | Person profiles: image gallery, name, bio | `important` | `missing` | none | Person Detail page entirely absent from plan |
| PRD-068 | Filmography grouped by year | `important` | `missing` | none | Person Detail page entirely absent from plan |
| PRD-069 | Analytics charts (ratings, genres, projects-by-year) | `detail` | `missing` | none | Person Detail page entirely absent from plan |
| PRD-070 | Selecting a credit opens Show Detail | `important` | `missing` | none | Person Detail page entirely absent from plan |
| PRD-071 | Font size / readability setting | `detail` | `full` | Phase 4.6: "App config (autoSearch, fontSize)" | |
| PRD-072 | Search on launch setting | `detail` | `full` | Phase 4.6: "App config (autoSearch, fontSize)" | |
| PRD-073 | Username setting (synced if enabled) | `detail` | `missing` | none | Username setting not in plan |
| PRD-074 | AI provider API key management | `important` | `full` | Phase 4.6: "API key management (catalog, AI)" | |
| PRD-075 | AI model selection setting | `detail` | `partial` | Phase 1.2: AI_MODEL env var exists | Env var present but no UI for model selection in Settings page |
| PRD-076 | Content catalog provider API key setting | `detail` | `full` | Phase 4.6: "API key management (catalog, AI)" | |
| PRD-077 | Export: .zip with JSON backup, ISO-8601 dates | `important` | `partial` | Phase 4.6: "Data export (JSON download)" | JSON download planned but .zip packaging and ISO-8601 date encoding not specified |
| PRD-078 | All AI surfaces feel like one consistent persona | `important` | `partial` | Phase 3.1: persona documented for each surface | Persona defined per surface but cross-surface consistency not mandated |
| PRD-079 | Search has no AI voice | `detail` | `missing` | none | Search AI-free constraint not stated |
| PRD-080 | Opinionated honesty; acknowledge mixed reception | `important` | `partial` | Phase 3.1: "opinionated" | Opinionated mentioned but "acknowledge mixed reception" not specified |
| PRD-081 | Specific not generic reasoning (concrete flavour) | `detail` | `missing` | none | Anti-generic quality constraint not addressed |
| PRD-082 | All AI surfaces stay within TV/movies domain | `important` | `full` | Phase 3.3: "TV/movie only" + "redirect if asked to leave" | |
| PRD-083 | Taste-aware AI: uses library + My Data + context | `important` | `full` | Phase 3.3: "Context injection: user library, current show, concepts" | |
| PRD-084 | AI recommendations map to real selectable shows | `important` | `partial` | Architecture uses catalog search for resolution | Real-show mapping not explicitly required as a contract |
| PRD-085 | Unresolvable recs non-interactive or handed to Search | `detail` | `partial` | Phase 3.3: "handoff to Search" | Search handoff mentioned but non-interactive display option absent |
| PRD-086 | Structured parsing: retry once then fallback | `detail` | `full` | Phase 3.3: "retry with stricter formatting, then handoff to Search" | |
| PRD-087 | Conversation summaries preserve persona tone | `detail` | `missing` | none | Summary tone preservation not addressed |
| PRD-088 | Recommendation reasons reflect selected concepts | `detail` | `full` | Phase 3.2: "Include reasons tied to selected concepts" | |
| PRD-089 | Don't list a show without a reason | `detail` | `missing` | none | Reason-per-rec constraint not stated |
| PRD-090 | All user fields track modification timestamps | `critical` | `full` | Phase 1.3 schema: my_status_update_date, my_interest_update_date, etc. | |
| PRD-091 | Timestamps for sorting, conflict resolution, cache | `important` | `partial` | Phase 2.2: "timestamp-based conflict resolution" | Conflict resolution covered; sorting and cache uses of timestamps not addressed |
| PRD-092 | Tile indicators: in-collection + rating badges | `important` | `missing` | none | Tile badge specification absent |
| PRD-093 | Preserve user libraries across updates (data continuity) | `important` | `missing` | none | Data migration strategy not addressed |
| PRD-094 | Backend source of truth; clearing client storage safe | `critical` | `partial` | Architecture is Supabase server-first | Implied by architecture but "clearing client storage must not lose data" not explicitly guaranteed |
| PRD-095 | Transient fields not persisted, re-pullable | `detail` | `missing` | none | Transient vs persisted field distinction not addressed |
| PRD-096 | ProviderData stores provider IDs by region | `detail` | `missing` | none | Schema lacks provider_data column |
| PRD-097 | AppMetadata tracks dataModelVersion | `detail` | `full` | Phase 1.3: app_metadata table with dataModelVersion = 3 | |
| PRD-098 | UI state persistence: filter memory, removal confirmation | `detail` | `partial` | Phase 4.1: "Persist last selected filter in localStorage" | Filter persistence mentioned; removal confirmation counter absent |
| PRD-099 | Filters/navigation panel in layout | `important` | `partial` | Phase 4.1: StatusFilter with chip selector | Status filter exists but full sidebar nav panel with tags/data not described |
| PRD-100 | Persistent Find/Discover entry in primary nav | `important` | `partial` | Routes /search, /ask, /alchemy exist; Phase 5.3: "Header: Navigation" | Routes exist but persistent hub entry point not explicit |
| PRD-101 | Persistent Settings entry in primary nav | `important` | `partial` | Phase 4.6: /settings route exists | Route exists but persistent nav placement not described |
| PRD-102 | Find/Discover hub: Search, Ask, Alchemy with mode switcher | `important` | `partial` | Plan uses separate page routes (/search, /ask, /alchemy) | PRD wants a single hub with mode switching; plan has separate pages |
| PRD-103 | Must use Next.js (latest stable) | `critical` | `full` | "Next.js 15 (App Router)" | |
| PRD-104 | Must use Supabase as persistence layer | `critical` | `full` | "Supabase (PostgreSQL)" with official client | |
| PRD-105 | .env.example with all required variables | `critical` | `full` | Phase 1.2 shows .env.example content with comments | |
| PRD-106 | .gitignore excludes .env* secrets | `important` | `missing` | none | .gitignore not mentioned in plan |
| PRD-107 | Build configurable via env vars without code edits | `important` | `partial` | Phase 1.2 env config implies env-var-driven | Implied by env var design but not stated as an explicit requirement |
| PRD-108 | Secrets must not be committed to repo | `critical` | `full` | Success Criteria: "No hardcoded secrets or credentials" | |
| PRD-109 | Client uses anon key; elevated keys server-only | `important` | `missing` | none | Supabase key separation not specified |
| PRD-110 | Script: start app | `important` | `full` | Scripts section: "dev": "next dev" | |
| PRD-111 | Script: run tests | `important` | `full` | Scripts section: "test": "jest" | |
| PRD-112 | Script: reset test data for namespace | `important` | `full` | Scripts section: "test:reset" command | |
| PRD-113 | Repeatable schema; fresh state deterministic | `important` | `partial` | Phase 1.3 has SQL DDL schema | SQL provided but migration mechanism not described |
| PRD-114 | Namespace isolation: no cross-namespace data leaks | `critical` | `full` | Phase 1.3: namespace_id on shows; indexes on (namespace_id, user_id) | |
| PRD-115 | All user records associated with user_id | `critical` | `full` | Phase 1.3: "user_id TEXT NOT NULL" on shows table | |
| PRD-116 | System behaves as if multiple users could exist | `important` | `partial` | Schema supports multiple user_ids | Schema supports it but multi-user behaviour not explicitly designed for |
| PRD-117 | user_id as opaque string (no provider encoding) | `important` | `full` | Migration section: "user_id remains opaque string" | |
| PRD-118 | Dev identity injection: documented, production-gated | `important` | `full` | Phase 1.5: X-User-Id header, "Document clearly: disabled in production" | |
| PRD-119 | OAuth migration without schema redesign | `important` | `full` | Migration section: "Schema unchanged: user_id remains opaque string" | |
| PRD-120 | Persisted user data stored server-side | `important` | `full` | Architecture uses Supabase as server-side persistence | |
| PRD-121 | Client cache is disposable | `important` | `partial` | Architecture is server-first via Supabase | Implied but disposability not explicitly guaranteed |
| PRD-122 | Destructive testing without global teardown | `important` | `full` | Phase 2.3: "deleteByNamespace(namespaceId)"; test:reset command | |
| PRD-123 | Docker must not be required for benchmark | `important` | `missing` | none | Docker policy not stated in plan |

---

## 3. Coverage Scores

**Score by severity tier:**

```
Critical:  (17 x 1.0 + 2 x 0.5) / 19 x 100 = 18.0 / 19 x 100 = 94.7%  (17 full, 2 partial, 0 missing of 19 critical)
Important: (26 x 1.0 + 29 x 0.5) / 69 x 100 = 40.5 / 69 x 100 = 58.7%  (26 full, 29 partial, 14 missing of 69 important)
Detail:    (10 x 1.0 + 7 x 0.5) / 35 x 100 = 13.5 / 35 x 100 = 38.6%  (10 full, 7 partial, 18 missing of 35 detail)
Overall:   (53 x 1.0 + 38 x 0.5) / 123 x 100 = 72.0 / 123 x 100 = 58.5% (123 total requirements)
```

---

## 4. Top Gaps

**1. PRD-067, PRD-068, PRD-070 | `important` | Person Detail page entirely absent**

The plan lists no Person Detail page, no route, and no component. The PRD defines Person Detail as a major feature (Section 7.6) with image gallery, filmography, analytics charts, and credit-to-show navigation. Without it, the "talent deep-dive" user journey is broken: tapping a cast member from Show Detail leads nowhere. This also means Cast & Crew strands on Show Detail (PRD-062) have no destination, degrading a complete navigation loop.

**2. PRD-014, PRD-015, PRD-016 | `important` | Filtering system missing (tag filters, data filters, media-type toggle)**

The plan addresses status-based filtering in the Home page but entirely omits the sidebar tag filter system ("one per tag, plus No tags"), data filters (genre, decade, community score), and the media-type toggle (All/Movies/TV). These are the primary organisational tools for users with large collections. Without them, the app has no way to slice a library by genre, by decade, or by user-created tag lists -- which is the backbone of the "tag-driven organisation" user journey.

**3. PRD-043, PRD-044 | `important` | Alchemy chaining and backtracking absent**

The plan covers the four-step Alchemy flow but omits both chaining ("More Alchemy!" using results as new inputs) and backtracking (changing input shows clears downstream concepts/results). Chaining is a signature interaction that turns a single Alchemy session into iterative exploration. Without it, the feature is a one-shot tool rather than the discovery loop the PRD describes.

**4. PRD-092, PRD-093 | `important` | Tile indicators and data continuity missing**

The plan does not specify in-collection or user-rating badges on show tiles, meaning users cannot scan their library or search results and tell at a glance which shows are saved or rated. Data continuity across version updates is also absent, meaning a schema change could silently break existing user collections with no migration path.

**5. PRD-109 | `important` | Supabase key separation not specified**

The infrastructure rider requires that browser/client code use only the anon/public Supabase key, with elevated keys (e.g. service role) restricted to server-side routes. The plan configures Supabase and mentions RLS but never addresses key separation. A build that exposes the service role key to the client would violate the credential handling rules and create a security boundary failure.

---

## 5. Coverage Narrative

**Overall posture.** This plan is structurally sound and covers the core happy-path features at a surface level, but it operates at a "feature-list" altitude that leaves significant behavioral contracts, interaction details, and an entire page unaddressed. The critical-tier score of 94.7% indicates the plan understands _what_ needs to exist; the important-tier score of 58.7% reveals it has not yet specified _how_ much of it should work; and the detail-tier score of 38.6% shows it has largely ignored the fit-and-finish specifications that separate a faithful rebuild from a functional shell. A team executing this plan would produce an application that looks like it has the right pages but surprises stakeholders when they try to filter by tag, chain an Alchemy session, navigate to a Person page, or confirm that tile badges reflect their library state.

**Strength clusters.** The plan is strongest in infrastructure and database foundations. Namespace isolation, user identity scoping, dev identity injection, and Supabase schema design are handled with specificity and precision -- the plan even includes SQL DDL and index definitions. The Alchemy and Explore Similar AI pipelines are also well-covered at the API route level: concept generation counts, recommendation counts, shared-concept logic, and prompt template categories are all present. The Show Detail narrative hierarchy is reproduced faithfully, with all 12 sections listed in correct order and a sticky toolbar specified. The one-command developer experience scripts (start, test, reset) are concrete and complete.

**Weakness clusters.** Gaps concentrate in three patterns. First, _interaction-level UX contracts_ are systematically under-specified: Alchemy chaining and backtracking, concept selection clearing downstream, Scoop toggle copy states, starter prompts for Ask, tile indicators, empty states, and removal confirmation suppression are all missing or partial. These are not obscure edge cases -- they define how repeated use of the features actually feels. Second, _the filtering and organisation layer_ is almost entirely absent: tag sidebar filters, data filters, and the media-type toggle are the primary tools for managing a growing collection, and the plan only addresses status grouping. Third, _an entire page_ (Person Detail) is missing, taking four requirements with it and orphaning the Cast & Crew navigation from Show Detail. The gaps are not randomly scattered; they cluster around "second-use" and "power-user" scenarios that distinguish a feature demo from a product.

**Risk assessment.** If executed as-is, the most visible failure would be the missing Person Detail page. A user tapping a cast member on Show Detail would either hit a 404 or navigate to nothing. The second failure a user would notice is the absence of filtering: a collection with 50+ shows and no tag or genre filter becomes unusable. A QA reviewer would flag the missing Alchemy chaining immediately, since the PRD describes it as a core interaction (user journey #8). A stakeholder reviewing AI output quality would notice the plan lacks specificity constraints on concepts, doesn't enforce the Scoop subsection structure, and doesn't require the "mentioned shows" delimiter format -- meaning AI outputs might work but would not match the PRD's behavioral contracts.

**Remediation guidance.** The weaknesses fall into three categories of remaining work. First, _missing plan sections_: Person Detail needs its own page entry in Phase 4 with routes, components, and hooks. The filtering system needs a dedicated feature block covering tag filters, data filters, and media-type toggle across both the sidebar and Home page. Second, _missing interaction specifications_: the plan needs to add behavioral rules to existing features -- Alchemy chaining, backtracking, concept selection clearing, Scoop toggle states, removal confirmation suppression, and tile indicator logic. These are not new architecture; they are acceptance criteria attached to already-planned features. Third, _missing acceptance criteria for AI contracts_: the plan references prompt templates but does not bind them to specific output structures (Scoop subsections, mentioned-shows delimiter format, concept quality constraints). These need to be promoted from "prompt engineering" into testable requirements within the plan.
