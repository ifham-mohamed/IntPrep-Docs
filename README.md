# IntPrep-Docs

> React & Next.js interview preparation knowledge base. **284 canonical questions across 11 sections**, organized by topic, with original answers, cross-links, and full source traceability.

---

## Quick Navigation

| | |
|---|---|
| 📖 [React Master Index](docs/react/master-index.md) | All 284 React questions — tech-level overview |
| 📖 [Reference Master Index](docs/reference/00-master-index.md) | Original GFE-sourced questions (REACT-001 to 110) |
| ⚛️ [React Knowledge Base](#react-knowledge-base) | 11 topic files, REACT-001 to REACT-284 |
| 🗂️ [Question Registry](docs/reference/01-question-registry.md) | Full metadata for every question |
| 📚 [Source Library](docs/reference/02-source-library.md) | Attribution and source tracking |
| 🗺️ [Core Learning Path](playbooks/core-learning-path.md) | Structured 3–4 week study plan |
| 🚀 [Advanced Learning Path](playbooks/advanced-learning-path.md) | Senior/staff depth track |

---

## React Knowledge Base

| File | IDs | Topics |
|---|---|---|
| [01-fundamentals.md](docs/react/01-fundamentals.md) | REACT-001–023, 145–195 | JSX, Virtual DOM, Props, State, Reconciliation, Pure Components, JSX deep dives |
| [02-hooks.md](docs/react/02-hooks.md) | REACT-024–034, 196–225 | useEffect, useRef, useMemo, useCallback, useReducer, custom hooks, advanced hooks internals |
| [03-advanced.md](docs/react/03-advanced.md) | REACT-035–065, 226–255, 284 | Re-rendering, Portals, Suspense, SSR, HOCs, Composition, Data Fetching, MobX, windowing, debugging |
| [04-routing.md](docs/react/04-routing.md) | REACT-066–079, 256–257 | React Router v6, dynamic routes, guards, navigation, analytics, legacy warnings |
| [05-i18n.md](docs/react/05-i18n.md) | REACT-080–086 | react-intl, FormatJS, localization, date formatting |
| [06-testing.md](docs/react/06-testing.md) | REACT-087–100, 258 | Jest, RTL, async testing, MSW, hooks testing, snapshots, ReactTestUtils |
| [07-react19.md](docs/react/07-react19.md) | REACT-101–110 | Actions, useActionState, useOptimistic, use(), Server Components, React Compiler |
| [08-redux.md](docs/react/08-redux.md) | REACT-111–135, 259–261 | Redux core, Thunk, Saga, selectors, DevTools, middleware, RxJS comparison |
| [09-react-native.md](docs/react/09-react-native.md) | REACT-136–139 | React Native vs React, testing, logging, debugging |
| [10-libraries.md](docs/react/10-libraries.md) | REACT-140–144, 262–267, 283 | Reselect, Flow, DevTools, Styled Components, Formik, Font Awesome, React vs Vue/Angular, Polymer |
| [11-legacy-class.md](docs/react/11-legacy-class.md) | REACT-268–282 | Lifecycle methods, binding, getDerivedStateFromProps, string refs, legacy context |

---

## Next.js Knowledge Base

[docs/nextjs/README.md](docs/nextjs/README.md) — planned, in progress.

Covers: App Router, data fetching, Server Actions, middleware, image optimization, deployment.

---

## Sources

Sources are organized by technology → source type → source name.

| Technology | Type | Source ID | Source | Questions | Archive |
|---|---|---|---|---|---|
| React | Website | GFE-001 | [GreatFrontEnd — 100 React Interview Questions (2026)](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers) | 110 | [sources/react/website/greatfrontend/](sources/react/website/greatfrontend/react-100-questions.md) |
| React | GitHub | GFE-002 | [GreatFrontEnd — Top React Interview Questions](https://github.com/greatfrontend/top-reactjs-interview-questions) | 50 | [sources/react/github/greatfrontend-top-reactjs-interview-questions/](sources/react/github/greatfrontend-top-reactjs-interview-questions/question-map.md) |
| React | GitHub | SDJ-001 | [SudheerJ — reactjs-interview-questions](https://github.com/sudheerj/reactjs-interview-questions) | 408 | [sources/react/github/sudheerj-reactjs-interview-questions/](sources/react/github/sudheerj-reactjs-interview-questions/question-map.md) |
| React | YouTube | — | Interview series | Planned | — |
| Next.js | Website | — | Official docs + GFE | Planned | — |

---

## Learning Paths

### Beginner → Interview Ready (3–4 weeks)
Follow the **[Core Learning Path](playbooks/core-learning-path.md)**:
1. React Fundamentals — REACT-001 to REACT-023
2. React Hooks — REACT-024 to REACT-034
3. Advanced Concepts — REACT-035 to REACT-065
4. React Router — REACT-066 to REACT-079
5. Testing — REACT-087 to REACT-100

### Senior / Staff Depth
Follow the **[Advanced Learning Path](playbooks/advanced-learning-path.md)**:
- React internals (Fiber, reconciliation, scheduling)
- Performance mastery (profiling, memoization, concurrent features)
- Architecture patterns (HOCs, composition, state management)
- Full-stack React (SSR, SSG, RSC, Server Actions)
- React 19 modern APIs

### Redux / State Management Deep Dive
Study REACT-111 to REACT-135 (`docs/react/08-redux.md`) — covers:
- Core concepts (store, actions, reducers, principles)
- Async: Redux Thunk, redux-saga, RTK Query
- Tooling: Redux DevTools, selectors (Reselect)
- Architecture decisions: Context vs Redux, when to use local state

---

## Repository Structure

```
IntPrep-Docs/
├── README.md                          ← you are here
├── docs/
│   ├── react/
│   │   ├── master-index.md            React tech-level index (284 questions)
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
│   │   └── README.md                  (planned)
│   └── reference/
│       ├── 00-master-index.md         Reference navigation (GFE-001 sourced)
│       ├── 01-question-registry.md    all IDs + metadata
│       ├── 02-source-library.md       source attribution (3 active sources)
│       └── 03-visual-assets.md        diagram references
├── sources/
│   └── react/
│       ├── github/
│       │   ├── greatfrontend-top-reactjs-interview-questions/
│       │   │   └── question-map.md    50 questions → canonical IDs (GFE-002)
│       │   └── sudheerj-reactjs-interview-questions/
│       │       └── question-map.md    408 questions → canonical IDs (SDJ-001)
│       └── website/
│           └── greatfrontend/
│               └── react-100-questions.md  110 questions (GFE-001)
└── playbooks/
    ├── core-learning-path.md          3–4 week plan
    ├── advanced-learning-path.md      senior depth track
    └── perplexity-source-addition-prompts.md  prompts for adding new sources
```

---

## Question ID Scheme

All canonical questions follow the format `REACT-NNN` (React) and `NEXT-NNN` (Next.js, planned). The scheme scales to 999 questions per domain.

Every question entry includes: answer, code example, related question links, and source attribution.

---

## Contribution Guidelines

**Adding a question from a new source:**
1. Check the [React Master Index](docs/react/master-index.md) — does this question already exist?
2. If yes → add the new source to the existing entry's `Source` section.
3. If no → assign the next available ID (REACT-285), write a canonical answer in the appropriate file, update the master index, question registry, and source library.

**Adding a new source entirely:**
1. Create `sources/{technology}/{source-type}/{source-name}/` directory.
2. Create a `question-map.md` mapping EVERY source question to a canonical ID (existing or new).
3. Add the source to [docs/reference/02-source-library.md](docs/reference/02-source-library.md).
4. Update this README's Sources table and counts.

**Formatting rules:**
- Answers are original — not copied verbatim from sources.
- Every entry ends with `Related:` and `Source:` sections.
- Code examples use fenced code blocks with language tags.
- Cross-links use relative paths with heading anchors.

---

*Last updated: June 2026 | Questions: 284 | Sources: 3 (GFE-001, GFE-002, SDJ-001)*
