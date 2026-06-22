# Perplexity Prompts — Adding New Sources to the Knowledge Base

> Use these prompts phase by phase when adding a new source to the IntPrep-Docs repository. Each prompt is self-contained — paste it into Perplexity with your specific values substituted for `[PLACEHOLDERS]`.

---

## Core Principle — Every Question Gets a Canonical Entry

**The rule:** When you add a source, EVERY question in that source becomes (or links to) a canonical entry in the correct `.md` file.

- If the question is already covered by an existing canonical: update that canonical's `Source:` line to credit the new source.
- If the question is new / not covered: write a NEW canonical entry in the correct category file.

There is no "source-only" bucket. The question map (`question-map.md`) is a mapping layer — the actual knowledge lives in the canonical files.

---

## Overview of the Process

Adding a new source takes 5 phases:

1. **Discovery** — identify the source's questions and assess overlap with existing canonicals
2. **Mapping** — map each source question to a canonical ID or assign a new ID
3. **New canonicals** — write original canonical entries for every unmapped question
4. **File updates** — update master index, source library, README
5. **Cross-linking** — add source attribution to existing canonical entries

---

## Phase 1 — Source Discovery

**Use this when:** You have found a new interview article, repo, or video and want to assess whether to add it.

```
I'm building an interview preparation knowledge base for React.js.

I have a source I'm considering adding: [SOURCE URL OR DESCRIPTION].

My existing canonical question set covers REACT-001 through REACT-282 across these topics:
- Fundamentals (JSX, Virtual DOM, props/state, reconciliation, Fiber) — REACT-001 to REACT-195
- Hooks (useEffect, useRef, useCallback, useMemo, useReducer, useContext, custom hooks, advanced hooks) — REACT-024 to REACT-225
- Advanced (re-rendering, error boundaries, Suspense, Portals, SSR, SSG, HOCs, composition, concurrent features, patterns) — REACT-035 to REACT-255
- React Router v6 (dynamic routes, nested routes, navigation, guards, query params) — REACT-066 to REACT-257
- i18n (react-intl, FormatJS) — REACT-080 to REACT-086
- Testing (Jest, React Testing Library, MSW, renderHook, snapshots) — REACT-087 to REACT-258
- React 19 (Actions, useActionState, useOptimistic, use(), Server Components, React Compiler) — REACT-101 to REACT-110
- Redux (core principles, Thunk, Saga, selectors, DevTools, middleware, RTK) — REACT-111 to REACT-261
- React Native (basics, testing, logging, debugging) — REACT-136 to REACT-139
- Libraries (Reselect, Flow, Styled Components, React DevTools, Formik, Font Awesome, React vs Vue/Angular) — REACT-140 to REACT-267
- Legacy Class Components (lifecycle, binding, getDerivedStateFromProps, string refs, etc.) — REACT-268 to REACT-282

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

I need to create a question-map.md file that maps each source question to a canonical ID.

CRITICAL RULE: Every question in the source must either:
(a) Map to an existing canonical ID (if the topic is already covered)
(b) Be assigned a new canonical ID starting from REACT-283 (if it's a new topic)

There is no "skip" or "ignore" option — every question gets a canonical home.

My canonical ID system:
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
- Next available: REACT-283

For this source, please:
1. List all questions in the source (paraphrased — do not copy verbatim).
2. For each, identify the best matching canonical ID from my system above.
   - If a matching canonical exists: assign that ID and mark status as "Mapped"
   - If no matching canonical exists: assign a new ID starting from REACT-283 and mark status as "New"
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

[Original answer in 150-300 words. Do NOT copy from any source. Write from first principles. Cover: what it is, why it matters, when to use it, key trade-offs.]

```[language]
// Code example showing the concept
// Should be 10-25 lines, runnable, practical
```

**Related:** [REACT-XXX — Related concept title](./relevant-file.md#react-xxx) | [REACT-YYY — Another related concept](./relevant-file.md#react-yyy)

**Source:** [Source Name SDJ-NNN or GFE-NNN](../../sources/react/...)

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

## Phase 4 — Updating Index Files

**Use this when:** You've written the new canonical entries and need to update the master index and README.

```
I've added new canonical React interview questions to my knowledge base. I need to update the index files.

New entries added:
[Paste your list of new IDs, titles, difficulties, and source names here. Example:
- REACT-283 | What is React Query? | Intermediate | NewSource
- REACT-284 | React Query vs SWR | Intermediate | NewSource
- REACT-285 | Infinite queries with React Query | Advanced | NewSource
]

Also updated existing canonicals (source credit added):
[List any existing IDs that received a new Source attribution]

I need:

1. New rows for docs/react/master-index.md — the relevant section table:
   Format: | [REACT-NNN](./NN-filename.md#react-nnn) | [Title] | 🟡 | [Source codes] |

2. Updated header for docs/react/master-index.md:
   "X entries, REACT-001 to REACT-NNN"

3. If a new section file was created (e.g., 12-react-query.md):
   - New row for the Quick Navigation table
   - New row for README.md's React Knowledge Base table

4. Update sources/react/[tech]/[type]/[name]/question-map.md:
   - Change any rows that were "New" to "Mapped" with the assigned canonical ID

5. Update docs/reference/02-source-library.md:
   - Updated question count for the source

Keep all output in copy-pasteable markdown blocks.
```

---

## Phase 5 — Cross-Linking Existing Entries

**Use this when:** New entries should be referenced in existing canonical entries as "Related" links.

```
I've added these new canonical React interview entries:
[List new IDs and titles]

I need to add cross-references to these new entries in my existing canonical files.

For each new entry, identify which EXISTING canonicals should link to it via their "Related:" section.

My existing topic areas:
- Fundamentals: REACT-001–023, REACT-145–195 (01-fundamentals.md)
- Hooks: REACT-024–034, REACT-196–225 (02-hooks.md)
- Advanced: REACT-035–065, REACT-226–255 (03-advanced.md)
- Routing: REACT-066–079, REACT-256–257 (04-routing.md)
- i18n: REACT-080–086 (05-i18n.md)
- Testing: REACT-087–100, REACT-258 (06-testing.md)
- React 19: REACT-101–110 (07-react19.md)
- Redux: REACT-111–135, REACT-259–261 (08-redux.md)
- React Native: REACT-136–139 (09-react-native.md)
- Libraries: REACT-140–144, REACT-262–267 (10-libraries.md)
- Legacy Class: REACT-268–282 (11-legacy-class.md)

For each relationship you identify, output:
- File to edit: [filename]
- Entry to edit: REACT-XXX
- Add to Related: `[REACT-NNN — New entry title](./new-file.md#react-nnn)`

Format as a markdown list grouped by file. Only include relationships where the conceptual connection is strong and useful to a reader.
```

---

## Bonus — Generating Source Archive (question-map.md)

**Use this when:** You want to generate the full `question-map.md` for a new source, combining Phase 2 output.

```
I'm creating a source archive file (question-map.md) for a new source in my React interview knowledge base.

Source details:
- Name: [SOURCE NAME]
- Author/Publisher: [AUTHOR]
- URL: [URL]
- Type: [github / website / youtube]
- Technology: React

The file should follow this format:

# Source Archive — [Source Name]

> **Source:** [URL]
> **Author:** [AUTHOR]
> **Questions extracted:** [N]
> **Source type:** [type]

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | X |
| New canonical created | Y |
| **Total** | N |

## Section [Name] ([N] questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 1 | [question] | [REACT-NNN](path/to/file.md#react-nnn) | Mapped |
| 2 | [question] | [REACT-NNN](path/to/file.md#react-nnn) | New |

...repeat for each section...

---

The mappings are:
[Paste your full question-to-canonical mapping from Phase 2 here]

Please format the full question-map.md file using the template above.
Note: ALL questions must appear — whether Mapped or New. No question is skipped.
```

---

## Quick Reference — Current ID Ranges

| Section | IDs | File |
|---|---|---|
| Fundamentals (core) | REACT-001 – 023 | docs/react/01-fundamentals.md |
| Hooks (core) | REACT-024 – 034 | docs/react/02-hooks.md |
| Advanced (core) | REACT-035 – 065 | docs/react/03-advanced.md |
| Routing (core) | REACT-066 – 079 | docs/react/04-routing.md |
| i18n | REACT-080 – 086 | docs/react/05-i18n.md |
| Testing (core) | REACT-087 – 100 | docs/react/06-testing.md |
| React 19 | REACT-101 – 110 | docs/react/07-react19.md |
| Redux (core) | REACT-111 – 135 | docs/react/08-redux.md |
| React Native | REACT-136 – 139 | docs/react/09-react-native.md |
| Libraries (core) | REACT-140 – 144 | docs/react/10-libraries.md |
| Fundamentals (SDJ extended) | REACT-145 – 195 | docs/react/01-fundamentals.md |
| Hooks (SDJ extended) | REACT-196 – 225 | docs/react/02-hooks.md |
| Advanced (SDJ extended) | REACT-226 – 255 | docs/react/03-advanced.md |
| Routing (SDJ extended) | REACT-256 – 257 | docs/react/04-routing.md |
| Testing (SDJ extended) | REACT-258 | docs/react/06-testing.md |
| Redux (SDJ extended) | REACT-259 – 261 | docs/react/08-redux.md |
| Libraries (SDJ extended) | REACT-262 – 267 | docs/react/10-libraries.md |
| Legacy Class Components | REACT-268 – 282 | docs/react/11-legacy-class.md |
| **Next available** | **REACT-283** | New section or extend existing |

---

*← [Advanced Learning Path](./advanced-learning-path.md) | [Source Library →](../docs/reference/02-source-library.md)*
