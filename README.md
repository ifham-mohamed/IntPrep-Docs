# IntPrep-Docs

> React & Next.js interview preparation knowledge base. **284 React + 134 Next.js canonical questions**, organized by topic, with original answers, cross-links, and full source traceability.

---

## Quick Navigation

| | |
|---|---|
| 📖 [React Master Index](docs/react/master-index.md) | All 284 React questions — tech-level overview |
| 📖 [Next.js Master Index](docs/nextjs/README.md) | All 134 Next.js questions — NEXT-001 to NEXT-134 |
| 📖 [Reference Master Index](docs/reference/00-master-index.md) | GFE-001 sourced questions (REACT-001 to REACT-110) |
| ⚛️ [React Knowledge Base](#react-knowledge-base) | 11 topic files, REACT-001 to REACT-284 |
| ▲ [Next.js Knowledge Base](#nextjs-knowledge-base) | 18 topic files, NEXT-001 to NEXT-134 |
| 🗂️ [Question Registry](docs/reference/01-question-registry.md) | Full metadata for every React question |
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

| File | IDs | Topics |
|---|---|---|
| [01-fundamentals.md](docs/nextjs/01-fundamentals.md) | NEXT-001–010, 036, 079 | What is Next.js, App Router vs Pages Router, file-based routing, dynamic routes, loading/error/not-found, create project |
| [02-server-client.md](docs/nextjs/02-server-client.md) | NEXT-011–014 | Server Components, Client Components, `"use client"` guidance, composition rules |
| [03-rendering.md](docs/nextjs/03-rendering.md) | NEXT-015–018, 048 | SSR, SSG, CSR, ISR, static vs dynamic rendering, hydration, pre-rendering |
| [04-data-fetching.md](docs/nextjs/04-data-fetching.md) | NEXT-019–022 | fetch in Server Components, ISR, caching layers, `"use cache"` APIs |
| [05-navigation.md](docs/nextjs/05-navigation.md) | NEXT-023–025, 037, 061 | next/link, useRouter, redirect(), push vs replace, singleton router |
| [06-api-actions.md](docs/nextjs/06-api-actions.md) | NEXT-026–028, 112–114, 122, 124–127 | Route Handlers, Server Actions, benefits/drawbacks/alternatives, API best practices |
| [07-seo-assets.md](docs/nextjs/07-seo-assets.md) | NEXT-029–031 | generateMetadata, next/image, next/font |
| [08-infra.md](docs/nextjs/08-infra.md) | NEXT-032–035, 134 | Middleware, env vars, auth, deployment, Edge vs Node runtime |
| [09-pages-router.md](docs/nextjs/09-pages-router.md) | NEXT-050, 075, 082–101 | Pages Router, `_app`, `_document`, getStaticProps, getServerSideProps, getStaticPaths |
| [10-config-tooling.md](docs/nextjs/10-config-tooling.md) | NEXT-038–039, 041–044, 049, 051–058, 060, 063 | TypeScript, public folder, port config, Fast Refresh, next.config.js, Webpack, polyfills |
| [11-styling.md](docs/nextjs/11-styling.md) | NEXT-045–047 | CSS Modules, global CSS, Tailwind CSS |
| [12-scripts-seo.md](docs/nextjs/12-scripts-seo.md) | NEXT-062, 065, 068–069, 074 | next/script, next-seo, Google Analytics, sitemaps, AMP |
| [13-security.md](docs/nextjs/13-security.md) | NEXT-070–072, 080–081, 104, 111, 123, 130 | CORS, cookies, security practices, JWT, auth tokens, authorization, input validation |
| [14-app-router-advanced.md](docs/nextjs/14-app-router-advanced.md) | NEXT-040, 067, 102–103, 105–110, 115–120, 131, 133 | next/dynamic, ssr:false, form handling, Auth.js, route groups, parallel/intercepting routes, streaming |
| [15-i18n.md](docs/nextjs/15-i18n.md) | NEXT-059, 066, 073, 121 | i18n routing, next-i18next, useTranslation, App Router i18n with next-intl |
| [16-integrations.md](docs/nextjs/16-integrations.md) | NEXT-064, 128–129 | GraphQL, RTK Query, Redux Toolkit with App Router |
| [16-performance.md](docs/nextjs/16-performance.md) | NEXT-076–078 | Performance optimization, limitations, large-scale suitability |
| [17-testing.md](docs/nextjs/17-testing.md) | NEXT-132 | Testing Server Components, Client Components, Route Handlers, Playwright |

---

## Sources

Sources are organized by technology → source type → source name.

| Technology | Type | Source ID | Source | Questions | Archive |
|---|---|---|---|---|---|
| React | Website | GFE-001 | [GreatFrontEnd — 100 React Interview Questions (2026)](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers) | 110 | [sources/react/website/greatfrontend/](sources/react/website/greatfrontend/react-100-questions.md) |
| React | GitHub | GFE-002 | [GreatFrontEnd — Top React Interview Questions](https://github.com/greatfrontend/top-reactjs-interview-questions) | 50 | [sources/react/github/greatfrontend-top-reactjs-interview-questions/](sources/react/github/greatfrontend-top-reactjs-interview-questions/question-map.md) |
| React | GitHub | SDJ-001 | [SudheerJ — reactjs-interview-questions](https://github.com/sudheerj/reactjs-interview-questions) | 408 | [sources/react/github/sudheerj-reactjs-interview-questions/](sources/react/github/sudheerj-reactjs-interview-questions/question-map.md) |
| Next.js | Website | GFE-NJS | [GreatFrontEnd — Next.js Interview Questions for Freshers](https://www.greatfrontend.com/blog/next-js-interview-questions-for-freshers) | 35 | [sources/nextjs/website/greatfrontend/](sources/nextjs/website/greatfrontend/question-map.md) |
| Next.js | GitHub | MRH-NJS | [mrhrifat — nextjs-interview-questions](https://github.com/mrhrifat/nextjs-interview-questions) | 150 | [sources/nextjs/github/mrhrifat/](sources/nextjs/github/mrhrifat/question-map.md) |
| React | YouTube | — | Interview series | Planned | — |

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

### Next.js Full-Stack Track
1. App Router fundamentals — NEXT-001 to NEXT-014
2. Rendering models — NEXT-015 to NEXT-022
3. Data fetching + caching — NEXT-019 to NEXT-022
4. Server Actions + Route Handlers — NEXT-026 to NEXT-028, NEXT-112 to NEXT-127
5. Security + Auth — NEXT-070 to NEXT-081, NEXT-103 to NEXT-111
6. Advanced patterns — NEXT-116 to NEXT-120, NEXT-131 to NEXT-134

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
│   │   ├── README.md                  Next.js master index (134 questions)
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
│       ├── 00-master-index.md         Reference navigation (GFE-001 sourced)
│       ├── 01-question-registry.md    React IDs + metadata
│       ├── 02-source-library.md       source attribution (5 active sources)
│       └── 03-visual-assets.md        diagram references
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
    └── perplexity-source-addition-prompts.md
```

---

## Question ID Scheme

- React: `REACT-NNN` (current range: REACT-001 to REACT-284, next: REACT-285)
- Next.js: `NEXT-NNN` (current range: NEXT-001 to NEXT-134, next: NEXT-135)

Every question entry includes: answer, code example, related question links, and source attribution.

---

## Contribution Guidelines

**Adding a question from a new source:**
1. Check the relevant master index — does this question already exist?
2. If yes → add the new source to the existing entry's `Source` section.
3. If no → assign the next available ID, write a canonical answer, update master index + registry + source library.

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

*Last updated: June 2026 | React: 284 questions, 3 sources | Next.js: 134 questions, 2 sources*
