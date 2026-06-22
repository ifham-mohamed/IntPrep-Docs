# Advanced Learning Path

> Deep-dive track for engineers targeting senior/staff roles or companies with rigorous system design + React architecture rounds.

---

## Who This Is For

Engineers who have completed the [Core Learning Path](./core-learning-path.md) and want to go deeper into React internals, performance, modern patterns, and architecture-level thinking.

---

## Phase A — React Internals

**Goal:** Understand how React actually works under the hood — not just how to use it.

| ID | Topic | Why It Matters |
|---|---|---|
| [REACT-011](../docs/react/01-fundamentals.md#react-011) | React Fiber architecture | Explains concurrent features, scheduling |
| [REACT-012](../docs/react/01-fundamentals.md#react-012) | Reconciliation algorithm | Performance reasoning, key decisions |
| [REACT-003](../docs/react/01-fundamentals.md#react-003) | Virtual DOM deep dive | Trade-offs vs fine-grained reactivity |
| [REACT-049](../docs/react/03-advanced.md#react-049) | What happens when setState is called? | Batching, rendering pipeline |
| [REACT-039](../docs/react/03-advanced.md#react-039) | Hydration internals | SSR/RSC correctness |
| [REACT-052](../docs/react/03-advanced.md#react-052) | Synthetic events + event delegation | Performance, integration with non-React |

---

## Phase B — Performance Mastery

**Goal:** Identify and fix performance bottlenecks confidently.

| ID | Topic | Key Technique |
|---|---|---|
| [REACT-035](../docs/react/03-advanced.md#react-035) | Re-rendering — causes and prevention | React DevTools Profiler |
| [REACT-016](../docs/react/01-fundamentals.md#react-016) | React.memo + Pure Components | Shallow comparison trade-offs |
| [REACT-029](../docs/react/02-hooks.md#react-029) | useCallback — when it actually helps | Reference stability |
| [REACT-030](../docs/react/02-hooks.md#react-030) | useMemo — when it actually helps | Expensive computations |
| [REACT-043](../docs/react/03-advanced.md#react-043) | Context performance optimization | Split contexts, memoize values |
| [REACT-054](../docs/react/03-advanced.md#react-054) | Concurrent features | useTransition, Suspense |
| [REACT-055](../docs/react/03-advanced.md#react-055) | Priority lanes in concurrent React | Scheduling mental model |
| [REACT-056](../docs/react/03-advanced.md#react-056) | Expensive computations — 5 strategies | Web Workers, virtualization |
| [REACT-042](../docs/react/03-advanced.md#react-042) | Code splitting strategies | Route-level vs component-level |
| [REACT-108](../docs/react/07-react19.md#react-108) | React Compiler | Automatic memoization |

**Performance debugging toolkit:**
- React DevTools Profiler — identify slow renders and their causes
- `why-did-you-render` library — log unnecessary re-renders
- Chrome Performance tab — measure FPS and long tasks
- Lighthouse — Core Web Vitals baseline

---

## Phase C — Architecture Patterns

**Goal:** Know how to structure large React applications.

| ID | Topic | Pattern |
|---|---|---|
| [REACT-044](../docs/react/03-advanced.md#react-044) | Flux pattern | Unidirectional state flow |
| [REACT-048](../docs/react/03-advanced.md#react-048) | State management decision framework | When to use what |
| [REACT-059](../docs/react/03-advanced.md#react-059) | Higher-order components | Cross-cutting concerns |
| [REACT-060](../docs/react/03-advanced.md#react-060) | Presentational vs container | Separation of concerns |
| [REACT-061](../docs/react/03-advanced.md#react-061) | Render props | Sharing render logic |
| [REACT-062](../docs/react/03-advanced.md#react-062) | Composition pattern | Preferred over inheritance |
| [REACT-034](../docs/react/02-hooks.md#react-034) | Custom hooks as the modern pattern | Replaces HOC + render props |
| [REACT-046](../docs/react/03-advanced.md#react-046) | Context pitfalls at scale | When to reach for external state |
| [REACT-050](../docs/react/03-advanced.md#react-050) | Prop drilling solutions | Context, composition, state manager |

---

## Phase D — Full-Stack React (SSR, SSG, RSC)

**Goal:** Understand React's role in full-stack architectures.

| ID | Topic | Framework Context |
|---|---|---|
| [REACT-057](../docs/react/03-advanced.md#react-057) | Server-side rendering | Next.js `getServerSideProps`, Remix |
| [REACT-058](../docs/react/03-advanced.md#react-058) | Static site generation | Next.js `getStaticProps`, ISR |
| [REACT-039](../docs/react/03-advanced.md#react-039) | Hydration — mechanics and pitfalls | All SSR frameworks |
| [REACT-106](../docs/react/07-react19.md#react-106) | React Server Components | Next.js App Router |
| [REACT-107](../docs/react/07-react19.md#react-107) | Server vs Client Components | Composition boundaries |
| [REACT-102](../docs/react/07-react19.md#react-102) | Actions in React 19 | Server Actions |
| [REACT-103](../docs/react/07-react19.md#react-103) | useActionState | Full-stack forms |
| [REACT-104](../docs/react/07-react19.md#react-104) | useOptimistic | Instant UX with server data |
| [REACT-105](../docs/react/07-react19.md#react-105) | use() hook | Streaming data in render |

---

## Phase E — Modern React 19

**Goal:** Be current on the latest APIs that interviewers at leading companies will ask about.

| ID | Topic |
|---|---|
| [REACT-101](../docs/react/07-react19.md#react-101) | React 19 overview |
| [REACT-108](../docs/react/07-react19.md#react-108) | React Compiler |
| [REACT-109](../docs/react/07-react19.md#react-109) | useTransition vs useDeferredValue |
| [REACT-110](../docs/react/07-react19.md#react-110) | Form action prop |
| [REACT-036](../docs/react/03-advanced.md#react-036) | forwardRef → now just a prop in React 19 |

---

## Senior Interview Topics to Prepare

Beyond question-and-answer, senior interviews often include:

**Architecture discussions:**
- "How would you design the state management layer for a large e-commerce app?"
- "Walk me through how you'd implement authentication and protected routes."
- "How would you ensure your React app stays performant at 100k users?"

**Live coding:**
- Build a paginated list with search and filters
- Implement a custom hook (`useDebounce`, `useIntersectionObserver`)
- Debug a performance issue using React DevTools

**System design with React:**
- How would you approach infinite scroll?
- How would you implement real-time updates (WebSocket) in React?
- How would you handle offline-first functionality?

---

*← [Core Learning Path](./core-learning-path.md) | [Master Index](../docs/reference/00-master-index.md)*
