# Perplexity Prompts — Adding New Sources to the Knowledge Base

> Use these prompts phase by phase when adding a new source to the IntPrep-Docs repository. Each prompt is self-contained — paste it into Perplexity with your specific values substituted for `[PLACEHOLDERS]`.

---

## Overview of the Process

Adding a new source takes 5 phases:

1. **Discovery** — identify the source's questions and assess overlap with existing canonicals
2. **Mapping** — map each source question to a canonical ID or flag as new
3. **New canonicals** — write original canonical entries for genuinely new questions
4. **File updates** — update master index, source library, README
5. **Cross-linking** — add source attribution to existing canonical entries

---

## Phase 1 — Source Discovery

**Use this when:** You have found a new interview article, repo, or video and want to assess whether to add it.

```
I'm building an interview preparation knowledge base for React.js.

I have a source I'm considering adding: [SOURCE URL OR DESCRIPTION].

My existing canonical question set covers REACT-001 through REACT-144 across these topics:
- Fundamentals (JSX, Virtual DOM, props/state, reconciliation, Fiber)
- Hooks (useEffect, useRef, useCallback, useMemo, useReducer, useContext, custom hooks)
- Advanced (re-rendering, error boundaries, Suspense, Portals, SSR, SSG, HOCs, composition, concurrent features)
- React Router v6 (dynamic routes, nested routes, navigation, guards, query params)
- i18n (react-intl, FormatJS)
- Testing (Jest, React Testing Library, MSW, renderHook, snapshots)
- React 19 (Actions, useActionState, useOptimistic, use(), Server Components, React Compiler)
- Redux (core principles, Thunk, Saga, selectors, DevTools, middleware)
- React Native (basics, testing, logging, debugging)
- Libraries (Reselect, Flow, Styled Components, React DevTools)

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

My canonical ID system:
- REACT-001 to REACT-023: Fundamentals
- REACT-024 to REACT-034: Hooks
- REACT-035 to REACT-065: Advanced (re-rendering, SSR/SSG, patterns, Portals, Suspense, concurrent features)
- REACT-066 to REACT-079: React Router v6
- REACT-080 to REACT-086: i18n (react-intl)
- REACT-087 to REACT-100: Testing (Jest, RTL, MSW)
- REACT-101 to REACT-110: React 19 (Server Components, Actions, React Compiler)
- REACT-111 to REACT-135: Redux (core, Thunk, Saga, selectors)
- REACT-136 to REACT-139: React Native
- REACT-140 to REACT-144: Libraries (Reselect, Flow, Styled Components)
- Next available: REACT-145

For this source, please:
1. List all questions in the source (paraphrased — do not copy verbatim).
2. For each, identify the best matching canonical ID from my system above, OR mark it as "New" if no match exists.
3. For "New" questions, group them by topic area.
4. Estimate how many new canonical entries I'll need to create.

Format the output as a markdown table:
| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
```

---

## Phase 3 — Writing New Canonical Entries

**Use this when:** Phase 2 has identified questions that don't map to any existing canonical. Use this per topic cluster.

```
I'm writing canonical interview preparation entries for a React knowledge base.

Topic cluster: [TOPIC NAME, e.g. "React Performance Optimization"]
Questions to cover:
- [Question 1]
- [Question 2]
- [Question 3]

Starting canonical ID: REACT-[NNN]

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

**Source:** [Source attribution]

---

Rules:
- All prose must be original — not copied from any source.
- Code examples should be practical, not toy-level.
- Use React 18+ APIs by default (hooks, function components).
- Related links should reference real questions in the set above.
- Difficulty: mark each as Beginner 🟢 / Intermediate 🟡 / Advanced 🔴 at the end.

Generate entries for all [N] questions above.
```

---

## Phase 4 — Updating Index Files

**Use this when:** You've written the new canonical entries and need to update the master index tables.

```
I've added new canonical React interview questions to my knowledge base. I need to update two index tables.

New entries added:
[Paste your list of new IDs, titles, difficulties, and source names here. Example:
- REACT-145 | What is React Query? | Intermediate | SDJ
- REACT-146 | React Query vs SWR | Intermediate | SDJ
- REACT-147 | Infinite queries with React Query | Advanced | SDJ
]

File: docs/react/master-index.md
This file has a section table for each topic (e.g., ## Libraries). 
Please generate the new table rows to add for these questions, in this format:
| [REACT-NNN](./NN-filename.md#react-nnn) | [Title] | 🟡 Intermediate | [Source codes] |

Also generate:
1. Updated count line: "X questions, REACT-001 to REACT-NNN"
2. Updated difficulty summary counts (how many beginner/intermediate/advanced to add to totals)
3. The new row to add to README.md's React Knowledge Base table if a new file was created

Keep it in copy-pasteable markdown blocks.
```

---

## Phase 5 — Cross-Linking Existing Entries

**Use this when:** New entries should be referenced in existing canonical entries as "Related" links.

```
I've added these new canonical React interview entries:
[List new IDs and titles]

I need to add cross-references to these new entries in my existing canonical files.

For each new entry, identify which EXISTING canonicals (REACT-001 through REACT-144) should link to it via their "Related:" section.

My existing topic areas:
- Fundamentals: REACT-001 – 023 (01-fundamentals.md)
- Hooks: REACT-024 – 034 (02-hooks.md)
- Advanced: REACT-035 – 065 (03-advanced.md)
- Routing: REACT-066 – 079 (04-routing.md)
- i18n: REACT-080 – 086 (05-i18n.md)
- Testing: REACT-087 – 100 (06-testing.md)
- React 19: REACT-101 – 110 (07-react19.md)
- Redux: REACT-111 – 135 (08-redux.md)
- React Native: REACT-136 – 139 (09-react-native.md)
- Libraries: REACT-140 – 144 (10-libraries.md)

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
[Table: Status | Count]

## Section [Name] ([N] questions)
[Table: Source # | Question | Canonical ID | Status]

...repeat for each section...

---

The mappings are:
[Paste your full question-to-canonical mapping from Phase 2 here]

Please format the full question-map.md file using the template above.
Note: Questions marked "New" should show `[REACT-NNN](path/to/file.md#react-nnn)` with the canonical ID I assigned.
Questions mapped to existing IDs should show the existing ID and link.
```

---

## Quick Reference — Current ID Ranges

| Section | IDs | File |
|---|---|---|
| Fundamentals | REACT-001 – 023 | docs/react/01-fundamentals.md |
| Hooks | REACT-024 – 034 | docs/react/02-hooks.md |
| Advanced | REACT-035 – 065 | docs/react/03-advanced.md |
| Routing | REACT-066 – 079 | docs/react/04-routing.md |
| i18n | REACT-080 – 086 | docs/react/05-i18n.md |
| Testing | REACT-087 – 100 | docs/react/06-testing.md |
| React 19 | REACT-101 – 110 | docs/react/07-react19.md |
| Redux | REACT-111 – 135 | docs/react/08-redux.md |
| React Native | REACT-136 – 139 | docs/react/09-react-native.md |
| Libraries | REACT-140 – 144 | docs/react/10-libraries.md |
| **Next available** | **REACT-145** | New file or extend existing |

---

*← [Advanced Learning Path](./advanced-learning-path.md) | [Source Library →](../docs/reference/02-source-library.md)*
