# Perplexity Prompts — Adding New Sources to the Knowledge Base

> Use these prompts phase by phase when adding a new source to the IntPrep-Docs repository. Each prompt is self-contained — paste it into Perplexity with your specific values substituted for `[PLACEHOLDERS]`.

---

## Core Principle — Every Question Gets a Canonical Entry

**The rule:** When you add a source, EVERY question in that source becomes (or links to) a canonical entry in the correct `.md` file.

- If the question is already covered by an existing canonical: update that canonical's `Source:` line to credit the new source.
- If the question is new / not covered: write a NEW canonical entry in the correct category file.

There is no "source-only" bucket. The question map (`question-map.md`) is a mapping layer — the actual knowledge lives in the canonical files.

---

## Repository Structure (Current)

```
IntPrep-Docs/
├── docs/
│   ├── react/
│   │   ├── master-index.md            (284 questions)
│   │   ├── 01-fundamentals.md         REACT-001–023, 145–195
│   │   ├── 02-hooks.md                REACT-024–034, 196–225
│   │   ├── 03-advanced.md             REACT-035–065, 226–255, 284
│   │   ├── 04-routing.md              REACT-066–079, 256–257
│   │   ├── 05-i18n.md                 REACT-080–086
│   │   ├── 06-testing.md              REACT-087–100, 258
│   │   ├── 07-react19.md              REACT-101–110
│   │   ├── 08-redux.md                REACT-111–135, 259–261
│   │   ├── 09-react-native.md         REACT-136–139
│   │   ├── 10-libraries.md            REACT-140–144, 262–267, 283
│   │   └── 11-legacy-class.md         REACT-268–282
│   ├── nextjs/
│   │   ├── README.md                  (134 questions, NEXT-001–NEXT-134)
│   │   ├── 01-fundamentals.md         NEXT-001–010, 036, 079
│   │   ├── 02-server-client.md        NEXT-011–014
│   │   ├── 03-rendering.md            NEXT-015–018, 048
│   │   ├── 04-data-fetching.md        NEXT-019–022
│   │   ├── 05-navigation.md           NEXT-023–025, 037, 061
│   │   ├── 06-api-actions.md          NEXT-026–028, 112–114, 122, 124–127
│   │   ├── 07-seo-assets.md           NEXT-029–031
│   │   ├── 08-infra.md                NEXT-032–035, 134
│   │   ├── 09-pages-router.md         NEXT-050, 075, 082–101
│   │   ├── 10-config-tooling.md       NEXT-038–039, 041–044, 049, 051–058, 060, 063
│   │   ├── 11-styling.md              NEXT-045–047
│   │   ├── 12-scripts-seo.md          NEXT-062, 065, 068–069, 074
│   │   ├── 13-security.md             NEXT-070–072, 080–081, 104, 111, 123, 130
│   │   ├── 14-app-router-advanced.md  NEXT-040, 067, 102–103, 105–120, 131, 133
│   │   ├── 15-i18n.md                 NEXT-059, 066, 073, 121
│   │   ├── 16-integrations.md         NEXT-064, 128–129
│   │   ├── 16-performance.md          NEXT-076–078
│   │   └── 17-testing.md              NEXT-132
│   └── reference/
│       ├── 00-master-index.md         (React + Next.js combined navigation)
│       ├── 01-question-registry.md    (418 total: 284 React + 134 Next.js)
│       ├── 02-source-library.md
│       └── 03-visual-assets.md
├── sources/
│   ├── react/
│   │   ├── github/
│   │   │   ├── greatfrontend-top-reactjs-interview-questions/
│   │   │   │   └── question-map.md    (50 questions, GFE-002)
│   │   │   └── sudheerj-reactjs-interview-questions/
│   │   │       └── question-map.md    (408 questions, SDJ-001)
│   │   └── website/
│   │       └── greatfrontend/
│   │           └── react-100-questions.md  (110 questions, GFE-001)
│   └── nextjs/
│       ├── github/
│       │   └── mrhrifat/
│       │       └── question-map.md    (150 questions, MRH-NJS)
│       └── website/
│           └── greatfrontend/
│               └── question-map.md    (35 questions, GFE-NJS)
└── playbooks/
    ├── core-learning-path.md
    ├── advanced-learning-path.md
    └── perplexity-source-addition-prompts.md  ← this file
```

**Active sources — React:**
- `GFE-001` — GreatFrontEnd blog article (110 questions, `sources/react/website/greatfrontend/`)
- `GFE-002` — GreatFrontEnd GitHub repo (50 questions, `sources/react/github/greatfrontend-top-reactjs-interview-questions/`)
- `SDJ-001` — SudheerJ GitHub repo (408 questions, `sources/react/github/sudheerj-reactjs-interview-questions/`)

**Active sources — Next.js:**
- `GFE-NJS` — GreatFrontEnd blog article (35 questions, `sources/nextjs/website/greatfrontend/`)
- `MRH-NJS` — mrhrifat GitHub repo (150 questions, `sources/nextjs/github/mrhrifat/`)

---

## Overview of the Process

Adding a new source takes 5 phases:

1. **Discovery** — identify the source's questions and assess overlap with existing canonicals
2. **Mapping** — map each source question to a canonical ID or assign a new ID
3. **New canonicals** — write original canonical entries for every unmapped question
4. **File updates** — update master index, source library, README, question registry
5. **Cross-linking** — add source attribution to existing canonical entries

---

## Phase 1 — Source Discovery

**Use this when:** You have found a new interview article, repo, or video and want to assess whether to add it.

```
I'm building an interview preparation knowledge base for React.js.

I have a source I'm considering adding: [SOURCE URL OR DESCRIPTION].

My existing canonical question set covers REACT-001 through REACT-284 across these topics:
- Fundamentals (JSX, Virtual DOM, props/state, reconciliation, Fiber, inline conditions, comments,
  switching components, mixins, pointer events, custom DOM attrs, vendor prefixes, folder structures,
  CSS modules, linters, ES6 import/export) — REACT-001–023, 145–195
- Hooks (useEffect, useRef, useCallback, useMemo, useReducer, useContext, custom hooks,
  advanced hooks internals, hook rules, hook patterns) — REACT-024–034, 196–225
- Advanced (re-rendering, error boundaries, Suspense, Portals, SSR, SSG, HOCs, composition,
  concurrent features, patterns, MobX, windowing, debugging) — REACT-035–065, 226–255, 284
- React Router v6 (dynamic routes, nested routes, navigation, guards, query params,
  Google Analytics, legacy warnings) — REACT-066–079, 256–257
- i18n (react-intl, FormatJS) — REACT-080–086
- Testing (Jest, React Testing Library, MSW, renderHook, snapshots, ReactTestUtils) — REACT-087–100, 258
- React 19 (Actions, useActionState, useOptimistic, use(), Server Components, React Compiler) — REACT-101–110
- Redux (core principles, Thunk, Saga, selectors, DevTools, middleware, RTK, RxJS comparison) — REACT-111–135, 259–261
- React Native (basics, testing, logging, debugging) — REACT-136–139
- Libraries (Reselect, Flow, Styled Components, React DevTools, Formik, Font Awesome,
  React vs Vue/Angular, Polymer) — REACT-140–144, 262–267, 283
- Legacy Class Components (lifecycle, binding, getDerivedStateFromProps, string refs,
  legacy context, forceUpdate, replaceState) — REACT-268–282

Please analyze [SOURCE URL OR DESCRIPTION] and tell me:
1. How many total questions does it contain?
2. What topics does it cover that I DON'T already have (or covers very shallowly)?
3. What's the estimated overlap with my existing set?
4. Is this source worth adding, and what canonical sections would benefit most?
5. What is the approximate difficulty distribution (beginner / intermediate / advanced)?

Give me a concise assessment — 200-300 words.
```

---

## Phase 2 — Question Extraction and Mapping

**Use this when:** You've decided to add the source and need to build the question map.

```
I'm adding a new source to my React interview knowledge base. The source is:
- Name: [SOURCE NAME]
- URL: [SOURCE URL]
- Type: [github repo / website article / YouTube video]
- Source ID to assign: [e.g., YT-001 / RD-001 / GH-001]

I need to create a question-map.md file that maps each source question to a canonical ID.

CRITICAL RULE: Every question in the source must either:
(a) Map to an existing canonical ID (if the topic is already covered)
(b) Be assigned a new canonical ID starting from REACT-285 (if it's a new topic)

There is no "skip" or "ignore" option — every question gets a canonical home.

My canonical ID system (current as of June 2026):
- REACT-001 to REACT-023: Fundamentals (core)
- REACT-024 to REACT-034: Hooks (core)
- REACT-035 to REACT-065: Advanced patterns
- REACT-066 to REACT-079: Routing
- REACT-080 to REACT-086: i18n
- REACT-087 to REACT-100: Testing
- REACT-101 to REACT-110: React 19
- REACT-111 to REACT-135: Redux
- REACT-136 to REACT-139: React Native
- REACT-140 to REACT-144: Libraries (core)
- REACT-145 to REACT-195: Fundamentals (extended, SDJ)
- REACT-196 to REACT-225: Hooks (extended, SDJ)
- REACT-226 to REACT-255: Advanced (extended, SDJ)
- REACT-256 to REACT-257: Routing (extended, SDJ)
- REACT-258: Testing (extended, SDJ)
- REACT-259 to REACT-261: Redux (extended, SDJ)
- REACT-262 to REACT-267: Libraries (extended, SDJ)
- REACT-268 to REACT-282: Legacy class components
- REACT-283: Libraries (Polymer, SDJ)
- REACT-284: Advanced (Debugging, GFE-002)
- Next available: REACT-285

For this source, please:
1. List all questions in the source (paraphrased — do not copy verbatim).
2. For each, identify the best matching canonical ID from my system above.
   - If a matching canonical exists: assign that ID and mark status as "Mapped"
   - If no matching canonical exists: assign a new ID starting from REACT-285 and mark status as "New"
3. For "New" questions, group them by which category file they should go into.
4. Tell me the total count: how many Mapped vs New.

Format the output as a markdown table:
| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
```

---

## Phase 3 — Writing New Canonical Entries

**Use this when:** Phase 2 has identified questions that need new canonical entries.

```
I'm writing canonical interview preparation entries for a React knowledge base.

Topic cluster: [TOPIC NAME, e.g. "React Performance Optimization"]
Target file: [e.g. docs/react/03-advanced.md]
Source: [Source ID and name, e.g. "GFE-002 — GreatFrontEnd top-reactjs-interview-questions"]
Questions to cover:
- [Question 1]
- [Question 2]
- [Question 3]

Starting canonical ID: REACT-[NNN]

IMPORTANT: Every question in the list above must become a canonical entry. No skipping.

For each question, write an original canonical entry following this exact format:

---

## REACT-[NNN]

### [Question as a heading]

[Original answer in 150-300 words. Do NOT copy from any source. Write from first principles.
Cover: what it is, why it matters, when to use it, key trade-offs.]

```[language]
// Code example showing the concept
// Should be 10-25 lines, runnable, practical
```

**Related:** [REACT-XXX — Related concept title](./relevant-file.md#react-xxx) | [REACT-YYY — Another related concept](./relevant-file.md#react-yyy)

**Source:** [Source Name — Source-ID-NNN](../../sources/react/{type}/{source-name}/question-map.md)

---

Rules:
- All prose must be original — not copied from any source.
- Code examples should be practical, not toy-level.
- Use React 18+ APIs by default (hooks, function components).
- Related links should reference real questions in the set.
- Difficulty: note each as Beginner 🟢 / Intermediate 🟡 / Advanced 🔴.

Generate entries for ALL [N] questions above.
```

---

## Phase 4 — Updating Index and Reference Files

**Use this when:** You've written the new canonical entries and need to update all the index files.

```
I've added new canonical React interview questions to my knowledge base. I need to update the index files.

New entries added:
[Paste your list, e.g.:
- REACT-285 | What is React Query? | Intermediate | GFE-003
- REACT-286 | React Query vs SWR | Intermediate | GFE-003
]

Also updated existing canonicals (source credit added):
[List any existing IDs that received a new Source attribution]

New source details:
- Source ID: [e.g. GFE-003]
- Source name: [e.g. GreatFrontEnd — React Query Deep Dive]
- URL: [URL]
- Type: [github / website / youtube]
- Folder path: sources/react/{type}/{source-name}/
- Total questions: [N]
- Mapped: [X] | New: [Y]

I need you to generate the content for these file updates:

1. **docs/react/master-index.md** — new rows for the relevant section table:
   Format: | [REACT-NNN](./NN-filename.md#react-nnn) | [Title] | 🟡 | [Source ID] |
   Also update the header: "X entries, REACT-001 to REACT-NNN"

2. **docs/reference/00-master-index.md** — new rows in the full table + Quick Navigation update + header count

3. **docs/reference/01-question-registry.md** — new rows:
   Format: | REACT-NNN | [Title] | [Category] | 🟡 | [Source ID] | [Related IDs] |
   Also update the header count.

4. **docs/reference/02-source-library.md** — new source entry block:
   Include: Source ID, title, author/publisher, URL, type, question count, canonical IDs, archive path,
   and a coverage map table. Update "Next available" in the Adding a New Source section.

5. **README.md** — new row in the Sources table, updated question count in header, updated
   Knowledge Base table IDs, updated Repository Structure tree, updated footer line.

6. **sources/react/{type}/{source-name}/question-map.md** — full file content using the standard template
   (Overlap Summary + section tables with Mapped/New status for each question).

Keep all output in copy-pasteable markdown blocks, one per file.
```

---

## Phase 5 — Cross-Linking Existing Entries

**Use this when:** New entries should be referenced in existing canonical entries as "Related" links.

```
I've added these new canonical React interview entries:
[List new IDs and titles]

I need to add cross-references to these new entries in my existing canonical files.

For each new entry, identify which EXISTING canonicals should link to it via their "Related:" section.

My existing topic files (current as of June 2026):
- Fundamentals: REACT-001–023, 145–195 (01-fundamentals.md)
- Hooks: REACT-024–034, 196–225 (02-hooks.md)
- Advanced: REACT-035–065, 226–255, 284 (03-advanced.md)
- Routing: REACT-066–079, 256–257 (04-routing.md)
- i18n: REACT-080–086 (05-i18n.md)
- Testing: REACT-087–100, 258 (06-testing.md)
- React 19: REACT-101–110 (07-react19.md)
- Redux: REACT-111–135, 259–261 (08-redux.md)
- React Native: REACT-136–139 (09-react-native.md)
- Libraries: REACT-140–144, 262–267, 283 (10-libraries.md)
- Legacy Class: REACT-268–282 (11-legacy-class.md)

For each relationship you identify, output:
- File to edit: [filename]
- Entry to edit: REACT-XXX
- Add to Related: `[REACT-NNN — New entry title](./new-file.md#react-nnn)`

Format as a markdown list grouped by file. Only include relationships where the conceptual connection is strong and useful to a reader.
```

---

## Bonus — Generating the question-map.md File

**Use this when:** You want to generate the full `question-map.md` for a new source.

```
I'm creating a question-map.md (source archive file) for a new source in my React interview knowledge base.

Source details:
- Source ID: [e.g. GFE-003]
- Name: [SOURCE NAME]
- Author/Publisher: [AUTHOR]
- URL: [URL]
- Type: [github / website / youtube]
- Technology: React
- Folder: sources/react/{type}/{source-name}/

The file should follow this template exactly:

# Source Archive — [Source Name]

> **Source:** [URL]
> **Author/Publisher:** [AUTHOR]
> **Technology:** React
> **Source type:** [type]
> **Questions extracted:** [N]
> **Source ID:** [SOURCE-ID]

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | X |
| New canonical created | Y |
| Source-only | 0 |
| **Total** | **N** |

> [One sentence describing what was mapped/created.]

---

## Section: [Section Name] ([N] questions)

| [Source abbreviation] # | Question | Canonical ID | Status |
|---|---|---|---|
| 1 | [question paraphrased] | [REACT-NNN](../../../../docs/react/NN-filename.md#react-nnn) | Mapped |
| 2 | [question paraphrased] | [REACT-NNN](../../../../docs/react/NN-filename.md#react-nnn) | New |

...repeat for each section...

---

*Source folder: `sources/react/{type}/{source-name}/`*

Important rules:
- ALL questions must appear — no question is skipped.
- Status is either "Mapped", "Mapped (merged)" (two source Qs map to same canonical), or "New".
- Links use relative path `../../../../docs/react/` from inside the source folder.
- Source-only count must always be 0.

The mappings from Phase 2 are:
[Paste your full question-to-canonical mapping table here]

Please format the complete question-map.md file.
```

---

## Quick Reference — Current ID Ranges

### React

| Section | IDs | File |
|---|---|---|
| Fundamentals (core) | REACT-001–023 | docs/react/01-fundamentals.md |
| Hooks (core) | REACT-024–034 | docs/react/02-hooks.md |
| Advanced (core) | REACT-035–065 | docs/react/03-advanced.md |
| Routing (core) | REACT-066–079 | docs/react/04-routing.md |
| i18n | REACT-080–086 | docs/react/05-i18n.md |
| Testing (core) | REACT-087–100 | docs/react/06-testing.md |
| React 19 | REACT-101–110 | docs/react/07-react19.md |
| Redux (core) | REACT-111–135 | docs/react/08-redux.md |
| React Native | REACT-136–139 | docs/react/09-react-native.md |
| Libraries (core) | REACT-140–144 | docs/react/10-libraries.md |
| Fundamentals (SDJ extended) | REACT-145–195 | docs/react/01-fundamentals.md |
| Hooks (SDJ extended) | REACT-196–225 | docs/react/02-hooks.md |
| Advanced (SDJ extended) | REACT-226–255 | docs/react/03-advanced.md |
| Routing (SDJ extended) | REACT-256–257 | docs/react/04-routing.md |
| Testing (SDJ extended) | REACT-258 | docs/react/06-testing.md |
| Redux (SDJ extended) | REACT-259–261 | docs/react/08-redux.md |
| Libraries (SDJ extended) | REACT-262–267 | docs/react/10-libraries.md |
| Legacy Class Components | REACT-268–282 | docs/react/11-legacy-class.md |
| Libraries (Polymer — SDJ) | REACT-283 | docs/react/10-libraries.md |
| Advanced (Debugging — GFE-002) | REACT-284 | docs/react/03-advanced.md |
| **Next available** | **REACT-285** | New section or extend existing |

### Next.js

| Section | IDs | File |
|---|---|---|
| Fundamentals | NEXT-001–010, 036, 079 | docs/nextjs/01-fundamentals.md |
| Server/Client Components | NEXT-011–014 | docs/nextjs/02-server-client.md |
| Rendering | NEXT-015–018, 048 | docs/nextjs/03-rendering.md |
| Data Fetching & Caching | NEXT-019–022 | docs/nextjs/04-data-fetching.md |
| Navigation | NEXT-023–025, 037, 061 | docs/nextjs/05-navigation.md |
| API Routes & Server Actions | NEXT-026–028, 112–114, 122, 124–127 | docs/nextjs/06-api-actions.md |
| SEO & Assets | NEXT-029–031 | docs/nextjs/07-seo-assets.md |
| Infrastructure | NEXT-032–035, 134 | docs/nextjs/08-infra.md |
| Pages Router | NEXT-050, 075, 082–101 | docs/nextjs/09-pages-router.md |
| Config & Tooling | NEXT-038–039, 041–044, 049, 051–058, 060, 063 | docs/nextjs/10-config-tooling.md |
| Styling | NEXT-045–047 | docs/nextjs/11-styling.md |
| Scripts, SEO & Analytics | NEXT-062, 065, 068–069, 074 | docs/nextjs/12-scripts-seo.md |
| Security | NEXT-070–072, 080–081, 104, 111, 123, 130 | docs/nextjs/13-security.md |
| App Router Advanced | NEXT-040, 067, 102–103, 105–120, 131, 133 | docs/nextjs/14-app-router-advanced.md |
| i18n | NEXT-059, 066, 073, 121 | docs/nextjs/15-i18n.md |
| Integrations | NEXT-064, 128–129 | docs/nextjs/16-integrations.md |
| Performance & Scale | NEXT-076–078 | docs/nextjs/16-performance.md |
| Testing | NEXT-132 | docs/nextjs/17-testing.md |
| **Next available** | **NEXT-135** | New section or extend existing |

---

## Source Folder Naming Convention

New sources always follow: `sources/{technology}/{source-type}/{source-name}/`

| Technology | Source type | Example source-name | Source ID |
|---|---|---|---|
| react | website | `greatfrontend` | GFE-001 |
| react | github | `greatfrontend-top-reactjs-interview-questions` | GFE-002 |
| react | github | `sudheerj-reactjs-interview-questions` | SDJ-001 |
| nextjs | website | `greatfrontend` | GFE-NJS |
| nextjs | github | `mrhrifat` | MRH-NJS |
| react | youtube | `fireship-react-series` | (planned) |
| nextjs | website | `nextjs-official-docs` | (planned) |

The source-name is the GitHub repo name (for GitHub sources) or a kebab-case slug of the site/title.

---

*← [Advanced Learning Path](./advanced-learning-path.md) | [Source Library →](../docs/reference/02-source-library.md)*
