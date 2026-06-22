# Core Learning Path

> Recommended study order for React interview preparation. Follow this path from start to finish for a structured progression from beginner to job-ready.

---

## Who This Is For

Engineers with basic JavaScript knowledge preparing for a React frontend interview at a mid-level or senior position. Estimated time: 3–4 weeks at 1–2 hours/day.

---

## Phase 1 — React Fundamentals (Week 1)

**Goal:** Understand what React is and how its core building blocks work.

**Questions to master:**

| ID | Topic | Priority |
|---|---|---|
| [REACT-001](../docs/react/01-fundamentals.md#react-001) | What is React? Main features | ⭐ Must-know |
| [REACT-002](../docs/react/01-fundamentals.md#react-002) | JSX — what it is and how it compiles | ⭐ Must-know |
| [REACT-003](../docs/react/01-fundamentals.md#react-003) | Virtual DOM — how it works | ⭐ Must-know |
| [REACT-008](../docs/react/01-fundamentals.md#react-008) | Props vs state | ⭐ Must-know |
| [REACT-009](../docs/react/01-fundamentals.md#react-009) | Class vs functional components | ⭐ Must-know |
| [REACT-005](../docs/react/01-fundamentals.md#react-005) | React Fragments | High |
| [REACT-006](../docs/react/01-fundamentals.md#react-006) | The key prop | ⭐ Must-know |
| [REACT-007](../docs/react/01-fundamentals.md#react-007) | Array indices as keys — why bad | High |
| [REACT-012](../docs/react/01-fundamentals.md#react-012) | Reconciliation | High |
| [REACT-014](../docs/react/01-fundamentals.md#react-014) | Controlled vs Uncontrolled components | High |
| [REACT-015](../docs/react/01-fundamentals.md#react-015) | Lifting state up | ⭐ Must-know |
| [REACT-016](../docs/react/01-fundamentals.md#react-016) | Pure Components / React.memo | High |
| [REACT-018](../docs/react/01-fundamentals.md#react-018) | PropTypes | Medium |
| [REACT-019](../docs/react/01-fundamentals.md#react-019) | Stateless components | Medium |
| [REACT-020](../docs/react/01-fundamentals.md#react-020) | Stateful components | Medium |
| [REACT-022](../docs/react/01-fundamentals.md#react-022) | Why not mutate state | ⭐ Must-know |
| [REACT-023](../docs/react/01-fundamentals.md#react-023) | Benefits of hooks | ⭐ Must-know |

**Phase 1 checklist:**
- [ ] Can explain the Virtual DOM diffing process without notes
- [ ] Can articulate the difference between props and state clearly
- [ ] Can write a controlled form input from scratch
- [ ] Can explain why key stability matters in lists

---

## Phase 2 — React Hooks (Week 1–2)

**Goal:** Be fluent with all built-in hooks and able to create custom ones.

| ID | Topic | Priority |
|---|---|---|
| [REACT-024](../docs/react/02-hooks.md#react-024) | Rules of hooks | ⭐ Must-know |
| [REACT-025](../docs/react/02-hooks.md#react-025) | useEffect vs useLayoutEffect | ⭐ Must-know |
| [REACT-026](../docs/react/02-hooks.md#react-026) | Dependency array of useEffect | ⭐ Must-know |
| [REACT-027](../docs/react/02-hooks.md#react-027) | useRef — two use cases | High |
| [REACT-029](../docs/react/02-hooks.md#react-029) | useCallback | ⭐ Must-know |
| [REACT-030](../docs/react/02-hooks.md#react-030) | useMemo | ⭐ Must-know |
| [REACT-031](../docs/react/02-hooks.md#react-031) | useReducer | High |
| [REACT-033](../docs/react/02-hooks.md#react-033) | useContext + Context API | ⭐ Must-know |
| [REACT-034](../docs/react/02-hooks.md#react-034) | Custom hooks | ⭐ Must-know |
| [REACT-028](../docs/react/02-hooks.md#react-028) | setState callback format | Medium |
| [REACT-032](../docs/react/02-hooks.md#react-032) | useId | Medium |

**Phase 2 checklist:**
- [ ] Can write a custom hook (`useFetch`, `useWindowSize`) from memory
- [ ] Can explain when to use `useCallback` vs `useMemo` and why
- [ ] Can explain what happens when a dependency is missing from useEffect
- [ ] Can implement a simple reducer pattern with `useReducer`

---

## Phase 3 — Advanced Concepts (Week 2–3)

**Goal:** Understand patterns, performance, and architectural concepts.

**Core advanced concepts:**

| ID | Topic | Priority |
|---|---|---|
| [REACT-035](../docs/react/03-advanced.md#react-035) | What triggers re-renders? | ⭐ Must-know |
| [REACT-037](../docs/react/03-advanced.md#react-037) | Error boundaries | High |
| [REACT-038](../docs/react/03-advanced.md#react-038) | React Suspense | High |
| [REACT-040](../docs/react/03-advanced.md#react-040) | React Portals | High |
| [REACT-041](../docs/react/03-advanced.md#react-041) | Strict Mode | Medium |
| [REACT-042](../docs/react/03-advanced.md#react-042) | Code splitting | High |
| [REACT-045](../docs/react/03-advanced.md#react-045) | One-way data flow | ⭐ Must-know |
| [REACT-047](../docs/react/03-advanced.md#react-047) | React anti-patterns | High |
| [REACT-048](../docs/react/03-advanced.md#react-048) | When to use state vs context vs external | ⭐ Must-know |
| [REACT-049](../docs/react/03-advanced.md#react-049) | What happens when setState is called? | High |
| [REACT-050](../docs/react/03-advanced.md#react-050) | Prop drilling | ⭐ Must-know |
| [REACT-051](../docs/react/03-advanced.md#react-051) | Lazy loading | High |
| [REACT-052](../docs/react/03-advanced.md#react-052) | Synthetic events | Medium |
| [REACT-053](../docs/react/03-advanced.md#react-053) | Lifecycle methods | Medium |
| [REACT-059](../docs/react/03-advanced.md#react-059) | HOCs | Medium |
| [REACT-062](../docs/react/03-advanced.md#react-062) | Composition pattern | High |
| [REACT-064](../docs/react/03-advanced.md#react-064) | Async data loading patterns | ⭐ Must-know |
| [REACT-065](../docs/react/03-advanced.md#react-065) | Data fetching pitfalls | High |

**Phase 3 checklist:**
- [ ] Can implement an error boundary from memory
- [ ] Can explain the difference between code splitting and lazy loading
- [ ] Can describe 3+ React anti-patterns with examples
- [ ] Can implement race condition protection in data fetching

---

## Phase 4 — React Router (Week 3)

**Goal:** Be able to implement routing, guards, and navigation for a real app.

| ID | Topic | Priority |
|---|---|---|
| [REACT-066](../docs/react/04-routing.md#react-066) | React Router fundamentals | ⭐ Must-know |
| [REACT-067](../docs/react/04-routing.md#react-067) | Dynamic routing with useParams | ⭐ Must-know |
| [REACT-068](../docs/react/04-routing.md#react-068) | Nested routes + Outlet | High |
| [REACT-069](../docs/react/04-routing.md#react-069) | BrowserRouter vs HashRouter | High |
| [REACT-073](../docs/react/04-routing.md#react-073) | Programmatic navigation | ⭐ Must-know |
| [REACT-074](../docs/react/04-routing.md#react-074) | Private routes / route guards | ⭐ Must-know |
| [REACT-075](../docs/react/04-routing.md#react-075) | Active link state with NavLink | High |
| [REACT-076](../docs/react/04-routing.md#react-076) | 404 handling | High |
| [REACT-077](../docs/react/04-routing.md#react-077) | Query parameters | High |
| [REACT-078](../docs/react/04-routing.md#react-078) | Redirect after login | High |

**Phase 4 checklist:**
- [ ] Can implement a full authenticated routing setup from scratch
- [ ] Can implement nested layouts with `<Outlet>`
- [ ] Can explain when to use `replace` vs `push` for history

---

## Phase 5 — Testing (Week 3–4)

**Goal:** Know how to write meaningful tests for React components and hooks.

| ID | Topic | Priority |
|---|---|---|
| [REACT-087](../docs/react/06-testing.md#react-087) | Testing strategy overview | ⭐ Must-know |
| [REACT-088](../docs/react/06-testing.md#react-088) | Jest basics | High |
| [REACT-089](../docs/react/06-testing.md#react-089) | React Testing Library | ⭐ Must-know |
| [REACT-090](../docs/react/06-testing.md#react-090) | Writing component tests with RTL | ⭐ Must-know |
| [REACT-091](../docs/react/06-testing.md#react-091) | Async testing with findBy / waitFor | High |
| [REACT-092](../docs/react/06-testing.md#react-092) | Mocking API calls (MSW) | High |
| [REACT-094](../docs/react/06-testing.md#react-094) | Testing custom hooks with renderHook | High |
| [REACT-097](../docs/react/06-testing.md#react-097) | Testing with context providers | High |

**Phase 5 checklist:**
- [ ] Can write a component test using `getByRole` and `userEvent`
- [ ] Can write an async test using `findByText`
- [ ] Can set up MSW for API mocking in tests
- [ ] Can test a custom hook with `renderHook`

---

## Recommended Study Sessions

| Session | Topics | Time |
|---|---|---|
| Session 1 | REACT-001 to REACT-010 | 90 min |
| Session 2 | REACT-011 to REACT-023 | 90 min |
| Session 3 | REACT-024 to REACT-034 (hooks) | 90 min |
| Session 4 | REACT-035 to REACT-052 (advanced pt. 1) | 90 min |
| Session 5 | REACT-053 to REACT-065 (advanced pt. 2) | 90 min |
| Session 6 | REACT-066 to REACT-079 (routing) | 75 min |
| Session 7 | REACT-087 to REACT-100 (testing) | 90 min |
| Session 8 | Review + mock interview (beginner questions) | 60 min |

---

*← [Repository Root](../README.md) | Advanced Path →: [advanced-learning-path.md](./advanced-learning-path.md)*
