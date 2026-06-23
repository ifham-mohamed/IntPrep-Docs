# React Knowledge Base — Master Index

> All canonical React questions across all sources. **284 entries, REACT-001 to REACT-284.**

---

## Quick Navigation

| Section | IDs | File | Count |
|---|---|---|---|
| [Fundamentals](#fundamentals) | REACT-001 – 195 | [01-fundamentals.md](./01-fundamentals.md) | 74 |
| [Hooks](#hooks) | REACT-024 – 225 | [02-hooks.md](./02-hooks.md) | 41 |
| [Advanced](#advanced) | REACT-035 – 255 | [03-advanced.md](./03-advanced.md) | 61 |
| [Routing](#routing) | REACT-066 – 257 | [04-routing.md](./04-routing.md) | 16 |
| [i18n](#i18n) | REACT-080 – 086 | [05-i18n.md](./05-i18n.md) | 7 |
| [Testing](#testing) | REACT-087 – 258 | [06-testing.md](./06-testing.md) | 15 |
| [React 19](#react-19) | REACT-101 – 110 | [07-react19.md](./07-react19.md) | 10 |
| [Redux](#redux) | REACT-111 – 261 | [08-redux.md](./08-redux.md) | 28 |
| [React Native](#react-native) | REACT-136 – 139 | [09-react-native.md](./09-react-native.md) | 4 |
| [Libraries](#libraries) | REACT-140 – 267 | [10-libraries.md](./10-libraries.md) | 11 |
| [Legacy Class](#legacy-class) | REACT-268 – 282 | [11-legacy-class.md](./11-legacy-class.md) | 15 |

**Sources:** [GFE](../../sources/react/website/greatfrontend/react-100-questions.md) • [SudheerJ](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## Fundamentals

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-001](./01-fundamentals.md#react-001) | What is React? | 🟢 | GFE, SDJ |
| [REACT-002](./01-fundamentals.md#react-002) | What is JSX? | 🟢 | GFE, SDJ |
| [REACT-003](./01-fundamentals.md#react-003) | Virtual DOM | 🟡 | GFE, SDJ |
| [REACT-004](./01-fundamentals.md#react-004) | React Node, Element, Component differences | 🟡 | GFE, SDJ |
| [REACT-005](./01-fundamentals.md#react-005) | React Fragments | 🟢 | GFE, SDJ |
| [REACT-006](./01-fundamentals.md#react-006) | key prop | 🟡 | GFE, SDJ |
| [REACT-007](./01-fundamentals.md#react-007) | Array indices as keys — why bad | 🟡 | GFE, SDJ |
| [REACT-008](./01-fundamentals.md#react-008) | Props vs state | 🟢 | GFE, SDJ |
| [REACT-009](./01-fundamentals.md#react-009) | Class vs functional components | 🟢 | GFE, SDJ |
| [REACT-010](./01-fundamentals.md#react-010) | When to use class over function | 🟡 | GFE, SDJ |
| [REACT-011](./01-fundamentals.md#react-011) | React Fiber architecture | 🔴 | GFE, SDJ |
| [REACT-012](./01-fundamentals.md#react-012) | Reconciliation algorithm | 🟡 | GFE, SDJ |
| [REACT-013](./01-fundamentals.md#react-013) | Shadow DOM vs Virtual DOM | 🟡 | GFE, SDJ |
| [REACT-014](./01-fundamentals.md#react-014) | Controlled vs uncontrolled components | 🟡 | GFE, SDJ |
| [REACT-015](./01-fundamentals.md#react-015) | Lifting state up | 🟢 | GFE, SDJ |
| [REACT-016](./01-fundamentals.md#react-016) | Pure Components and React.memo | 🟡 | GFE, SDJ |
| [REACT-017](./01-fundamentals.md#react-017) | createElement vs cloneElement | 🟡 | GFE, SDJ |
| [REACT-018](./01-fundamentals.md#react-018) | PropTypes for runtime validation | 🟢 | GFE, SDJ |
| [REACT-019](./01-fundamentals.md#react-019) | Stateless components | 🟢 | GFE, SDJ |
| [REACT-020](./01-fundamentals.md#react-020) | Stateful components | 🟢 | GFE, SDJ |
| [REACT-021](./01-fundamentals.md#react-021) | Static type checking (TypeScript, Flow) | 🟡 | GFE, SDJ |
| [REACT-022](./01-fundamentals.md#react-022) | Why not mutate state | 🟢 | GFE |
| [REACT-023](./01-fundamentals.md#react-023) | Benefits of React Hooks | 🟢 | GFE, SDJ |
| [REACT-145](./01-fundamentals.md#react-145) | History of React | 🟢 | SDJ |
| [REACT-146](./01-fundamentals.md#react-146) | How to create components | 🟢 | SDJ |
| [REACT-147](./01-fundamentals.md#react-147) | Inline conditional expressions | 🟢 | SDJ |
| [REACT-148](./01-fundamentals.md#react-148) | children prop | 🟢 | SDJ |
| [REACT-149](./01-fundamentals.md#react-149) | How to write comments in React | 🟢 | SDJ |
| [REACT-150](./01-fundamentals.md#react-150) | React.lazy — named exports | 🟡 | SDJ |
| [REACT-151](./01-fundamentals.md#react-151) | Limitations of React | 🟢 | SDJ |
| [REACT-152](./01-fundamentals.md#react-152) | react-dom package | 🟢 | SDJ |
| [REACT-153](./01-fundamentals.md#react-153) | dangerouslySetInnerHTML | 🟡 | SDJ |
| [REACT-154](./01-fundamentals.md#react-154) | Styles in React | 🟢 | SDJ |
| [REACT-155](./01-fundamentals.md#react-155) | Conditional rendering | 🟢 | SDJ |
| [REACT-156](./01-fundamentals.md#react-156) | Spreading props onto DOM elements | 🟡 | SDJ |
| [REACT-157](./01-fundamentals.md#react-157) | Enable production mode | 🟢 | SDJ |
| [REACT-158](./01-fundamentals.md#react-158) | Switching component | 🟢 | SDJ |
| [REACT-159](./01-fundamentals.md#react-159) | React Mixins (legacy) | 🟢 | SDJ |
| [REACT-160](./01-fundamentals.md#react-160) | Pointer Events | 🟡 | SDJ |
| [REACT-161](./01-fundamentals.md#react-161) | Custom DOM attributes | 🟢 | SDJ |
| [REACT-162](./01-fundamentals.md#react-162) | Looping inside JSX | 🟢 | SDJ |
| [REACT-163](./01-fundamentals.md#react-163) | Props in JSX attribute quotes | 🟢 | SDJ |
| [REACT-164](./01-fundamentals.md#react-164) | Conditional CSS class names | 🟢 | SDJ |
| [REACT-165](./01-fundamentals.md#react-165) | React vs ReactDOM | 🟢 | SDJ |
| [REACT-166](./01-fundamentals.md#react-166) | React label element (htmlFor) | 🟢 | SDJ |
| [REACT-167](./01-fundamentals.md#react-167) | Combine inline style objects | 🟢 | SDJ |
| [REACT-168](./01-fundamentals.md#react-168) | Pretty-print JSON | 🟢 | SDJ |
| [REACT-169](./01-fundamentals.md#react-169) | Detect React version at runtime | 🟢 | SDJ |
| [REACT-170](./01-fundamentals.md#react-170) | Vendor prefixes in inline styles | 🟢 | SDJ |
| [REACT-171](./01-fundamentals.md#react-171) | ES6 import/export of components | 🟢 | SDJ |
| [REACT-172](./01-fundamentals.md#react-172) | Component naming exceptions | 🟡 | SDJ |
| [REACT-173](./01-fundamentals.md#react-173) | Async/await in React components | 🟡 | SDJ |
| [REACT-174](./01-fundamentals.md#react-174) | Folder structures for React | 🟢 | SDJ |
| [REACT-175](./01-fundamentals.md#react-175) | Animation libraries | 🟢 | SDJ |
| [REACT-176](./01-fundamentals.md#react-176) | CSS Modules benefit | 🟢 | SDJ |
| [REACT-177](./01-fundamentals.md#react-177) | React-specific linters | 🟢 | SDJ |
| [REACT-178](./01-fundamentals.md#react-178) | Rules of JSX | 🟢 | SDJ |
| [REACT-179](./01-fundamentals.md#react-179) | Why JSX tags must be wrapped | 🟢 | SDJ |
| [REACT-180](./01-fundamentals.md#react-180) | Imperative vs declarative in React | 🟡 | SDJ |
| [REACT-181](./01-fundamentals.md#react-181) | New JSX transform benefits | 🟢 | SDJ |
| [REACT-182](./01-fundamentals.md#react-182) | Default props | 🟢 | SDJ |
| [REACT-183](./01-fundamentals.md#react-183) | displayName property | 🟡 | SDJ |
| [REACT-184](./01-fundamentals.md#react-184) | Keyed Fragments | 🟡 | SDJ |
| [REACT-185](./01-fundamentals.md#react-185) | React HTML attribute support | 🟢 | SDJ |
| [REACT-186](./01-fundamentals.md#react-186) | Props defaulting to true | 🟢 | SDJ |
| [REACT-187](./01-fundamentals.md#react-187) | JSX prevents injection attacks | 🟡 | SDJ |
| [REACT-188](./01-fundamentals.md#react-188) | Keys — globally unique? | 🟢 | SDJ |
| [REACT-189](./01-fundamentals.md#react-189) | Keys on non-list items | 🟡 | SDJ |
| [REACT-190](./01-fundamentals.md#react-190) | Pass numbers to components | 🟢 | SDJ |
| [REACT-191](./01-fundamentals.md#react-191) | Pass event handler to component | 🟢 | SDJ |
| [REACT-192](./01-fundamentals.md#react-192) | Prevent function multiple calls | 🟡 | SDJ |
| [REACT-193](./01-fundamentals.md#react-193) | Print falsy values in JSX | 🟡 | SDJ |
| [REACT-194](./01-fundamentals.md#react-194) | Import SVG as React component | 🟢 | SDJ |
| [REACT-195](./01-fundamentals.md#react-195) | How React updates the screen | 🟡 | SDJ |

---

## Hooks

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-024](./02-hooks.md#react-024) | Rules of Hooks | 🟢 | GFE, SDJ |
| [REACT-025](./02-hooks.md#react-025) | useEffect vs useLayoutEffect | 🟡 | GFE, SDJ |
| [REACT-026](./02-hooks.md#react-026) | useEffect dependency array | 🟡 | GFE, SDJ |
| [REACT-027](./02-hooks.md#react-027) | useRef — DOM access and mutable values | 🟡 | GFE, SDJ |
| [REACT-028](./02-hooks.md#react-028) | setState callback format | 🟡 | GFE |
| [REACT-029](./02-hooks.md#react-029) | useCallback | 🟡 | GFE, SDJ |
| [REACT-030](./02-hooks.md#react-030) | useMemo | 🟡 | GFE, SDJ |
| [REACT-031](./02-hooks.md#react-031) | useReducer | 🟡 | GFE, SDJ |
| [REACT-032](./02-hooks.md#react-032) | useId | 🟢 | GFE, SDJ |
| [REACT-033](./02-hooks.md#react-033) | useContext + Context API | 🟡 | GFE, SDJ |
| [REACT-034](./02-hooks.md#react-034) | Custom hooks | 🟡 | GFE, SDJ |
| [REACT-196](./02-hooks.md#react-196) | Ensure hooks follow rules (ESLint) | 🟢 | SDJ |
| [REACT-197](./02-hooks.md#react-197) | useState updater function | 🟡 | SDJ |
| [REACT-198](./02-hooks.md#react-198) | useState lazy initializer | 🟡 | SDJ |
| [REACT-199](./02-hooks.md#react-199) | useState value types | 🟢 | SDJ |
| [REACT-200](./02-hooks.md#react-200) | useState called conditionally | 🟡 | SDJ |
| [REACT-201](./02-hooks.md#react-201) | useState — synchronous or async? | 🟡 | SDJ |
| [REACT-202](./02-hooks.md#react-202) | useState internal workings | 🔴 | SDJ |
| [REACT-203](./02-hooks.md#react-203) | useReducer + useContext pattern | 🟡 | SDJ |
| [REACT-204](./02-hooks.md#react-204) | Dispatch multiple actions in useReducer | 🟡 | SDJ |
| [REACT-205](./02-hooks.md#react-205) | Is dispatch asynchronous? | 🟡 | SDJ |
| [REACT-206](./02-hooks.md#react-206) | Multiple contexts in one component | 🟡 | SDJ |
| [REACT-207](./02-hooks.md#react-207) | useContext pitfall with object values | 🔴 | SDJ |
| [REACT-208](./02-hooks.md#react-208) | Context value without Provider | 🟢 | SDJ |
| [REACT-209](./02-hooks.md#react-209) | Return Promise from useEffect | 🟡 | SDJ |
| [REACT-210](./02-hooks.md#react-210) | Multiple useEffect hooks | 🟢 | SDJ |
| [REACT-211](./02-hooks.md#react-211) | Prevent infinite loops in useEffect | 🔴 | SDJ |
| [REACT-212](./02-hooks.md#react-212) | useLayoutEffect during SSR | 🔴 | SDJ |
| [REACT-213](./02-hooks.md#react-213) | useLayoutEffect for non-layout logic | 🟡 | SDJ |
| [REACT-214](./02-hooks.md#react-214) | useLayoutEffect layout thrashing | 🔴 | SDJ |
| [REACT-215](./02-hooks.md#react-215) | useRef to store previous values | 🟡 | SDJ |
| [REACT-216](./02-hooks.md#react-216) | Access ref in render method | 🟡 | SDJ |
| [REACT-217](./02-hooks.md#react-217) | useImperativeHandle | 🟡 | SDJ |
| [REACT-218](./02-hooks.md#react-218) | When to use useImperativeHandle | 🟡 | SDJ |
| [REACT-219](./02-hooks.md#react-219) | useImperativeHandle without forwardRef | 🟡 | SDJ |
| [REACT-220](./02-hooks.md#react-220) | useMemo prevent child re-rendering | 🔴 | SDJ |
| [REACT-221](./02-hooks.md#react-221) | Hooks in class components? | 🟢 | SDJ |
| [REACT-222](./02-hooks.md#react-222) | useSyncExternalStore | 🔴 | SDJ |
| [REACT-223](./02-hooks.md#react-223) | useInsertionEffect | 🔴 | SDJ |
| [REACT-224](./02-hooks.md#react-224) | useDebugValue | 🟡 | SDJ |
| [REACT-225](./02-hooks.md#react-225) | Hooks best practices | 🟢 | SDJ |

---

## Advanced

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-035](./03-advanced.md#react-035) | What causes re-renders? | 🟡 | GFE |
| [REACT-036](./03-advanced.md#react-036) | forwardRef | 🟡 | GFE |
| [REACT-037](./03-advanced.md#react-037) | Error boundaries | 🟡 | GFE, SDJ |
| [REACT-038](./03-advanced.md#react-038) | React Suspense | 🟡 | GFE, SDJ |
| [REACT-039](./03-advanced.md#react-039) | Hydration | 🔴 | GFE, SDJ |
| [REACT-040](./03-advanced.md#react-040) | React Portals | 🟡 | GFE, SDJ |
| [REACT-041](./03-advanced.md#react-041) | Strict Mode | 🟢 | GFE, SDJ |
| [REACT-042](./03-advanced.md#react-042) | Code splitting | 🟡 | GFE, SDJ |
| [REACT-043](./03-advanced.md#react-043) | Context performance optimization | 🔴 | GFE |
| [REACT-044](./03-advanced.md#react-044) | Flux pattern | 🟡 | GFE, SDJ |
| [REACT-045](./03-advanced.md#react-045) | One-way data flow | 🟢 | GFE |
| [REACT-046](./03-advanced.md#react-046) | Context pitfalls at scale | 🔴 | GFE |
| [REACT-047](./03-advanced.md#react-047) | React anti-patterns | 🟡 | GFE |
| [REACT-048](./03-advanced.md#react-048) | State management decisions | 🔴 | GFE |
| [REACT-049](./03-advanced.md#react-049) | What happens when setState is called? | 🔴 | GFE |
| [REACT-050](./03-advanced.md#react-050) | Prop drilling | 🟢 | GFE, SDJ |
| [REACT-051](./03-advanced.md#react-051) | Lazy loading | 🟡 | GFE |
| [REACT-052](./03-advanced.md#react-052) | Synthetic events | 🟡 | GFE, SDJ |
| [REACT-053](./03-advanced.md#react-053) | Lifecycle methods | 🟡 | GFE |
| [REACT-054](./03-advanced.md#react-054) | Concurrent rendering — useTransition | 🔴 | GFE, SDJ |
| [REACT-055](./03-advanced.md#react-055) | Priority lanes in Fiber | 🔴 | GFE |
| [REACT-056](./03-advanced.md#react-056) | Expensive computations — 5 strategies | 🔴 | GFE |
| [REACT-057](./03-advanced.md#react-057) | Server-side rendering | 🟡 | GFE, SDJ |
| [REACT-058](./03-advanced.md#react-058) | Static site generation | 🟡 | GFE |
| [REACT-059](./03-advanced.md#react-059) | Higher-order components | 🟡 | GFE, SDJ |
| [REACT-060](./03-advanced.md#react-060) | Presentational vs container | 🟡 | GFE |
| [REACT-061](./03-advanced.md#react-061) | Render props | 🟡 | GFE, SDJ |
| [REACT-062](./03-advanced.md#react-062) | Composition pattern | 🟡 | GFE |
| [REACT-063](./03-advanced.md#react-063) | Resize event handler | 🟢 | GFE, SDJ |
| [REACT-064](./03-advanced.md#react-064) | Async data loading patterns | 🟡 | GFE |
| [REACT-065](./03-advanced.md#react-065) | Data fetching pitfalls / race conditions | 🔴 | GFE |
| [REACT-226](./03-advanced.md#react-226) | Render hijacking | 🟡 | SDJ |
| [REACT-227](./03-advanced.md#react-227) | Prevent unnecessary setState | 🟡 | SDJ |
| [REACT-228](./03-advanced.md#react-228) | Flux vs Redux differences | 🟡 | SDJ |
| [REACT-229](./03-advanced.md#react-229) | Uncaught errors in React 16+ | 🟡 | SDJ |
| [REACT-230](./03-advanced.md#react-230) | Error boundary limits | 🟡 | SDJ |
| [REACT-231](./03-advanced.md#react-231) | Error boundary placement | 🟡 | SDJ |
| [REACT-232](./03-advanced.md#react-232) | MobX | 🟡 | SDJ |
| [REACT-233](./03-advanced.md#react-233) | Redux vs MobX | 🟡 | SDJ |
| [REACT-234](./03-advanced.md#react-234) | Async mode vs concurrent mode | 🟡 | SDJ |
| [REACT-235](./03-advanced.md#react-235) | JavaScript URLs in React | 🟡 | SDJ |
| [REACT-236](./03-advanced.md#react-236) | TypeScript with React | 🟡 | SDJ |
| [REACT-237](./03-advanced.md#react-237) | Auth persistence with Context | 🔴 | SDJ |
| [REACT-238](./03-advanced.md#react-238) | Wrapper component pattern | 🟢 | SDJ |
| [REACT-239](./03-advanced.md#react-239) | Strict Mode renders twice | 🟡 | SDJ |
| [REACT-240](./03-advanced.md#react-240) | Windowing / virtualization | 🟡 | SDJ |
| [REACT-241](./03-advanced.md#react-241) | Web components in React | 🟡 | SDJ |
| [REACT-242](./03-advanced.md#react-242) | Loadable components | 🟡 | SDJ |
| [REACT-243](./03-advanced.md#react-243) | Default context value purpose | 🟡 | SDJ |
| [REACT-244](./03-advanced.md#react-244) | Diffing algorithm rules | 🔴 | SDJ |
| [REACT-245](./03-advanced.md#react-245) | Render prop — must be named "render"? | 🟡 | SDJ |
| [REACT-246](./03-advanced.md#react-246) | Render props with pure components | 🔴 | SDJ |
| [REACT-247](./03-advanced.md#react-247) | Prevent automatic batching | 🔴 | SDJ |
| [REACT-248](./03-advanced.md#react-248) | Update objects in state | 🟡 | SDJ |
| [REACT-249](./03-advanced.md#react-249) | Update arrays in state | 🟡 | SDJ |
| [REACT-250](./03-advanced.md#react-250) | Immer for state updates | 🟡 | SDJ |
| [REACT-251](./03-advanced.md#react-251) | Nested function components — anti-pattern | 🔴 | SDJ |
| [REACT-252](./03-advanced.md#react-252) | Capture phase events | 🟡 | SDJ |
| [REACT-253](./03-advanced.md#react-253) | Popular form libraries | 🟢 | SDJ |
| [REACT-254](./03-advanced.md#react-254) | Why not inheritance in React | 🟡 | SDJ |
| [REACT-255](./03-advanced.md#react-255) | Rewrite class components to hooks? | 🟢 | SDJ |

---

## Routing

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-066](./04-routing.md#react-066) | React Router fundamentals | 🟢 | GFE, SDJ |
| [REACT-067](./04-routing.md#react-067) | Dynamic routing with useParams | 🟢 | GFE |
| [REACT-068](./04-routing.md#react-068) | Nested routes and Outlet | 🟡 | GFE |
| [REACT-069](./04-routing.md#react-069) | BrowserRouter vs HashRouter | 🟡 | GFE, SDJ |
| [REACT-070](./04-routing.md#react-070) | history library | 🟡 | GFE, SDJ |
| [REACT-071](./04-routing.md#react-071) | Router components in v6 | 🟡 | GFE, SDJ |
| [REACT-072](./04-routing.md#react-072) | push vs replace in history | 🟢 | GFE, SDJ |
| [REACT-073](./04-routing.md#react-073) | Programmatic navigation — useNavigate | 🟢 | GFE, SDJ |
| [REACT-074](./04-routing.md#react-074) | Private routes / ProtectedLayout | 🟡 | GFE |
| [REACT-075](./04-routing.md#react-075) | NavLink active state | 🟢 | GFE |
| [REACT-076](./04-routing.md#react-076) | 404 / NotFound handling | 🟢 | GFE, SDJ |
| [REACT-077](./04-routing.md#react-077) | Query parameters — useSearchParams | 🟢 | GFE, SDJ |
| [REACT-078](./04-routing.md#react-078) | Redirect after login | 🟡 | GFE, SDJ |
| [REACT-079](./04-routing.md#react-079) | Passing props to routes | 🟢 | GFE |
| [REACT-256](./04-routing.md#react-256) | Google Analytics with React Router | 🟡 | SDJ |
| [REACT-257](./04-routing.md#react-257) | "Router may have only one child" warning | 🟢 | SDJ |

---

## i18n

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-080](./05-i18n.md#react-080) | Localization strategy | 🟡 | GFE |
| [REACT-081](./05-i18n.md#react-081) | React Intl / FormatJS | 🟢 | GFE, SDJ |
| [REACT-082](./05-i18n.md#react-082) | FormatJS features | 🟡 | GFE, SDJ |
| [REACT-083](./05-i18n.md#react-083) | Component vs imperative formatting | 🟡 | GFE, SDJ |
| [REACT-084](./05-i18n.md#react-084) | FormattedMessage as placeholder | 🟢 | GFE, SDJ |
| [REACT-085](./05-i18n.md#react-085) | Accessing current locale | 🟢 | GFE, SDJ |
| [REACT-086](./05-i18n.md#react-086) | Date formatting with locale | 🟢 | GFE, SDJ |

---

## Testing

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-087](./06-testing.md#react-087) | Testing strategy (pyramid) | 🟢 | GFE |
| [REACT-088](./06-testing.md#react-088) | Jest basics | 🟢 | GFE, SDJ |
| [REACT-089](./06-testing.md#react-089) | React Testing Library | 🟢 | GFE |
| [REACT-090](./06-testing.md#react-090) | Writing component tests | 🟡 | GFE |
| [REACT-091](./06-testing.md#react-091) | Async testing with findBy / waitFor | 🟡 | GFE |
| [REACT-092](./06-testing.md#react-092) | Mocking API calls | 🟡 | GFE |
| [REACT-093](./06-testing.md#react-093) | MSW (Mock Service Worker) | 🔴 | GFE |
| [REACT-094](./06-testing.md#react-094) | Testing custom hooks — renderHook | 🟡 | GFE |
| [REACT-095](./06-testing.md#react-095) | Shallow Renderer (deprecated) | 🟡 | GFE, SDJ |
| [REACT-096](./06-testing.md#react-096) | Snapshot testing | 🟡 | GFE |
| [REACT-097](./06-testing.md#react-097) | Testing with context providers | 🟡 | GFE |
| [REACT-098](./06-testing.md#react-098) | Testing Redux in components | 🟡 | GFE |
| [REACT-099](./06-testing.md#react-099) | Shallow vs full DOM rendering | 🟡 | GFE |
| [REACT-100](./06-testing.md#react-100) | TestRenderer (deprecated React 19) | 🟡 | GFE, SDJ |
| [REACT-258](./06-testing.md#react-258) | ReactTestUtils package | 🟡 | SDJ |

---

## React 19

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-101](./07-react19.md#react-101) | React 19 overview | 🟡 | GFE |
| [REACT-102](./07-react19.md#react-102) | Actions concept | 🟡 | GFE |
| [REACT-103](./07-react19.md#react-103) | useActionState | 🟡 | GFE |
| [REACT-104](./07-react19.md#react-104) | useOptimistic | 🔴 | GFE |
| [REACT-105](./07-react19.md#react-105) | use() hook | 🔴 | GFE |
| [REACT-106](./07-react19.md#react-106) | React Server Components | 🔴 | GFE, SDJ |
| [REACT-107](./07-react19.md#react-107) | Server vs Client Components | 🔴 | GFE |
| [REACT-108](./07-react19.md#react-108) | React Compiler | 🔴 | GFE |
| [REACT-109](./07-react19.md#react-109) | useTransition vs useDeferredValue | 🔴 | GFE, SDJ |
| [REACT-110](./07-react19.md#react-110) | Form action prop | 🟡 | GFE |

---

## Redux

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-111](./08-redux.md#react-111) | What is Redux? | 🟢 | SDJ |
| [REACT-112](./08-redux.md#react-112) | Core principles of Redux | 🟢 | SDJ |
| [REACT-113](./08-redux.md#react-113) | Downsides of Redux vs Flux | 🟡 | SDJ |
| [REACT-114](./08-redux.md#react-114) | mapStateToProps vs mapDispatchToProps | 🟡 | SDJ |
| [REACT-115](./08-redux.md#react-115) | Dispatch action in reducer? | 🟡 | SDJ |
| [REACT-116](./08-redux.md#react-116) | Access Redux store outside a component | 🟡 | SDJ |
| [REACT-117](./08-redux.md#react-117) | Reset Redux state | 🟡 | SDJ |
| [REACT-118](./08-redux.md#react-118) | Context vs Redux | 🟡 | SDJ |
| [REACT-119](./08-redux.md#react-119) | Why "reducers"? | 🟢 | SDJ |
| [REACT-120](./08-redux.md#react-120) | AJAX in Redux | 🟡 | SDJ |
| [REACT-121](./08-redux.md#react-121) | Should all state live in Redux? | 🟡 | SDJ |
| [REACT-122](./08-redux.md#react-122) | Component vs container in Redux | 🟡 | SDJ |
| [REACT-123](./08-redux.md#react-123) | Action type constants | 🟢 | SDJ |
| [REACT-124](./08-redux.md#react-124) | Redux directory structure | 🟡 | SDJ |
| [REACT-125](./08-redux.md#react-125) | redux-saga | 🔴 | SDJ |
| [REACT-126](./08-redux.md#react-126) | call() vs put() in redux-saga | 🔴 | SDJ |
| [REACT-127](./08-redux.md#react-127) | Redux Thunk | 🟡 | SDJ |
| [REACT-128](./08-redux.md#react-128) | redux-saga vs redux-thunk | 🔴 | SDJ |
| [REACT-129](./08-redux.md#react-129) | Redux DevTools | 🟢 | SDJ |
| [REACT-130](./08-redux.md#react-130) | Redux selectors | 🟡 | SDJ |
| [REACT-131](./08-redux.md#react-131) | Redux Form | 🟡 | SDJ |
| [REACT-132](./08-redux.md#react-132) | Multiple middlewares | 🟡 | SDJ |
| [REACT-133](./08-redux.md#react-133) | Initial state in Redux | 🟢 | SDJ |
| [REACT-134](./08-redux.md#react-134) | Relay vs Redux | 🔴 | SDJ |
| [REACT-135](./08-redux.md#react-135) | What is an action? | 🟢 | SDJ |
| [REACT-259](./08-redux.md#react-259) | Redux vs RxJS similarities | 🔴 | SDJ |
| [REACT-260](./08-redux.md#react-260) | Redux without React | 🟢 | SDJ |
| [REACT-261](./08-redux.md#react-261) | Build tool required for Redux? | 🟢 | SDJ |

---

## React Native

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-136](./09-react-native.md#react-136) | React Native vs React | 🟢 | SDJ |
| [REACT-137](./09-react-native.md#react-137) | Testing React Native | 🟡 | SDJ |
| [REACT-138](./09-react-native.md#react-138) | Logging in React Native | 🟢 | SDJ |
| [REACT-139](./09-react-native.md#react-139) | Debugging React Native | 🟡 | SDJ |

---

## Libraries

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-140](./10-libraries.md#react-140) | Reselect | 🟡 | SDJ |
| [REACT-141](./10-libraries.md#react-141) | Flow type checker | 🟡 | SDJ |
| [REACT-142](./10-libraries.md#react-142) | Flow vs PropTypes | 🟡 | SDJ |
| [REACT-143](./10-libraries.md#react-143) | React DevTools | 🟢 | SDJ |
| [REACT-144](./10-libraries.md#react-144) | Styled Components | 🟡 | SDJ |
| [REACT-262](./10-libraries.md#react-262) | Font Awesome in React | 🟢 | SDJ |
| [REACT-263](./10-libraries.md#react-263) | DevTools not loading on local files | 🟢 | SDJ |
| [REACT-264](./10-libraries.md#react-264) | React tab not showing in DevTools | 🟢 | SDJ |
| [REACT-265](./10-libraries.md#react-265) | React vs Vue.js | 🟡 | SDJ |
| [REACT-266](./10-libraries.md#react-266) | React vs Angular | 🟡 | SDJ |
| [REACT-267](./10-libraries.md#react-267) | Formik | 🟡 | SDJ |
| [REACT-283](./10-libraries.md#react-283) | Polymer in React | 🟡 | SDJ |

---

## Legacy Class

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-268](./11-legacy-class.md#react-268) | Component lifecycle phases | 🟡 | SDJ |
| [REACT-269](./11-legacy-class.md#react-269) | Binding methods in class components | 🟡 | SDJ |
| [REACT-270](./11-legacy-class.md#react-270) | setState callback argument | 🟡 | SDJ |
| [REACT-271](./11-legacy-class.md#react-271) | super() vs super(props) | 🟡 | SDJ |
| [REACT-272](./11-legacy-class.md#react-272) | getDerivedStateFromProps | 🔴 | SDJ |
| [REACT-273](./11-legacy-class.md#react-273) | getSnapshotBeforeUpdate | 🔴 | SDJ |
| [REACT-274](./11-legacy-class.md#react-274) | String refs — why legacy | 🟡 | SDJ |
| [REACT-275](./11-legacy-class.md#react-275) | Force re-render without setState | 🟡 | SDJ |
| [REACT-276](./11-legacy-class.md#react-276) | setState vs replaceState | 🟡 | SDJ |
| [REACT-277](./11-legacy-class.md#react-277) | Props proxy for HOC | 🟡 | SDJ |
| [REACT-278](./11-legacy-class.md#react-278) | try/catch vs error boundaries | 🟡 | SDJ |
| [REACT-279](./11-legacy-class.md#react-279) | Deprecated lifecycle methods | 🟡 | SDJ |
| [REACT-280](./11-legacy-class.md#react-280) | Class component method ordering | 🟢 | SDJ |
| [REACT-281](./11-legacy-class.md#react-281) | Old Context API (pre-16.3) | 🟡 | SDJ |
| [REACT-282](./11-legacy-class.md#react-282) | Dynamic key in setState | 🟢 | SDJ |

---

## Advanced (Debug / GFE-002)

| ID | Title | Difficulty | Sources |
|---|---|---|---|
| [REACT-284](./03-advanced.md#react-284) | How to debug React applications | 🟡 | GFE-002 |

---

## Difficulty Summary

🟢 **Beginner** — REACT-001,002,005,008,009,015,018,019,020,022,023,032,041,045,050,063,066,067,072,073,075,076,077,079,081,084,085,086,087,088,089,111,112,119,123,129,133,135,136,138,143,145,146,147,148,149,151,152,154,155,157,158,159,161,162,163,164,165,166,167,168,169,170,171,174,175,176,177,178,179,181,182,185,186,188,190,191,194,196,199,208,210,221,225,238,253,255,257,260,261,262,263,264,280,282

🟡 **Intermediate** — Most remaining questions

🔴 **Advanced** — REACT-011,039,043,046,048,049,054,055,056,065,093,104,105,106,107,108,109,125,126,128,134,202,207,211,212,214,220,222,223,237,244,246,247,251,259,272,273

---

*← [Repository Root](../../README.md) | [Reference Master Index →](../reference/00-master-index.md)*
