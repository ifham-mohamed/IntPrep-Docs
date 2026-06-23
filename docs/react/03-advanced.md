# React Advanced Concepts

> **Canonical Knowledge Base** | Category: Advanced Concepts | IDs: REACT-035 – REACT-065
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-035 — What does re-rendering mean in React?](#react-035)
- [REACT-036 — What is `forwardRef()` in React used for?](#react-036)
- [REACT-037 — What are error boundaries in React?](#react-037)
- [REACT-038 — What is React Suspense?](#react-038)
- [REACT-039 — What is React hydration?](#react-039)
- [REACT-040 — What are React Portals used for?](#react-040)
- [REACT-041 — What is React Strict Mode and what are its benefits?](#react-041)
- [REACT-042 — What is code splitting in a React application?](#react-042)
- [REACT-043 — How do you optimize React context performance to reduce re-renders?](#react-043)
- [REACT-044 — What is the Flux pattern?](#react-044)
- [REACT-045 — Explain one-way data flow in React](#react-045)
- [REACT-046 — What are pitfalls of using context in React?](#react-046)
- [REACT-047 — What are some React anti-patterns?](#react-047)
- [REACT-048 — How do you decide between state, context, and external state managers?](#react-048)
- [REACT-049 — What happens when `setState` is called in React?](#react-049)
- [REACT-050 — What is prop drilling?](#react-050)
- [REACT-051 — What is lazy loading in React?](#react-051)
- [REACT-052 — What are synthetic events in React?](#react-052)
- [REACT-053 — What are the React component lifecycle methods in class components?](#react-053)
- [REACT-054 — What are concurrent features in React?](#react-054)
- [REACT-055 — How does React prioritize concurrent updates?](#react-055)
- [REACT-056 — How do you handle expensive computations without blocking the UI?](#react-056)
- [REACT-057 — What is server-side rendering (SSR) and what are its benefits?](#react-057)
- [REACT-058 — What is static site generation (SSG) in React?](#react-058)
- [REACT-059 — What are higher-order components (HOCs)?](#react-059)
- [REACT-060 — What is the presentational vs container component pattern?](#react-060)
- [REACT-061 — What are render props?](#react-061)
- [REACT-062 — What is the composition pattern in React?](#react-062)
- [REACT-063 — How do you re-render the view when the browser is resized?](#react-063)
- [REACT-064 — How do you handle async data loading in React?](#react-064)
- [REACT-065 — What are common pitfalls when data fetching in React?](#react-065)

---

## REACT-035

### What does re-rendering mean in React?

A **re-render** occurs when React re-executes a component's function body to produce a new VDOM tree, then reconciles that tree against the previous one to update the real DOM.

**When does a component re-render?**
1. Its own state changes (`useState` / `useReducer` setter called).
2. Its parent re-renders (unless the component is wrapped in `React.memo`).
3. A context it consumes changes value.
4. A hook it calls (e.g., `useContext`, custom hook) triggers a state change.

**Re-render ≠ DOM update.** React may re-render a component but produce exactly the same VDOM — in that case, the real DOM is not touched. Re-renders are relatively cheap; real DOM mutations are expensive.

**How to reduce unnecessary re-renders:**
- `React.memo` — skip re-render if props haven't changed (shallow compare).
- `useMemo` / `useCallback` — stabilize values and function references.
- Split large components — a small section that changes often shouldn't force the whole page to re-render.
- Keyed lists — ensure stable keys so React can reuse elements.

**Related:** [REACT-016 Pure Components / memo](#react-016) | [REACT-029 useCallback](#react-029) | [REACT-030 useMemo](#react-030)

**Source:** GreatFrontEnd Q35

---

## REACT-036

### What is `forwardRef()` in React used for?

`forwardRef` lets a parent component pass a `ref` through to a DOM node or component instance **inside** a child component. By default, refs don't pass through components automatically.

**Why it's needed:** If you create a custom `<Input>` component and a parent wants to call `.focus()` on the underlying `<input>` DOM node, the ref must be forwarded explicitly.

```jsx
// Without forwardRef — ref attaches to the <Input> component instance, not the <input> DOM node
// With forwardRef:
const Input = React.forwardRef(function Input({ placeholder, ...props }, ref) {
  return <input ref={ref} placeholder={placeholder} {...props} />;
});

// Parent can now access the DOM node directly
function Form() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus(); // works!
  }, []);

  return <Input ref={inputRef} placeholder="Search..." />;
}
```

**`useImperativeHandle`** — used together with `forwardRef` to expose only specific methods rather than the raw DOM node:

```jsx
const FancyInput = forwardRef(function FancyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => { inputRef.current.value = ''; },
  }));

  return <input ref={inputRef} />;
});
```

**Note:** In React 19, `ref` is a regular prop — `forwardRef` is no longer required.

**Related:** [REACT-027 useRef](#react-027) | [REACT-101 React 19 changes](#react-101)

**Source:** GreatFrontEnd Q36

---

## REACT-037

### What are error boundaries in React?

**Error boundaries** are class components that catch JavaScript errors anywhere in their child component tree, log them, and display a fallback UI instead of crashing the entire app.

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false, error: null };

  static getDerivedStateFromError(error) {
    // Update state so the next render shows the fallback UI
    return { hasError: true, error };
  }

  componentDidCatch(error, info) {
    // Log to an error reporting service
    reportError(error, info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <h2>Something went wrong.</h2>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary fallback={<ErrorPage />}>
  <UserDashboard />
</ErrorBoundary>
```

**What error boundaries DON'T catch:**
- Errors inside event handlers (use try/catch there).
- Async code (`setTimeout`, `fetch` callbacks).
- Errors in the error boundary itself.
- Server-side rendering errors.

**Popular alternative:** The [`react-error-boundary`](https://github.com/bvaughn/react-error-boundary) library wraps the pattern in a reusable component with a `resetKeys` prop.

**React 19:** Introduces `onCaughtError` and `onUncaughtError` options on `createRoot` for global error handling without a class component.

**Related:** [REACT-010 Class components](#react-010) | [REACT-038 Suspense](#react-038)

**Source:** GreatFrontEnd Q37

---

## REACT-038

### What is React Suspense?

**React Suspense** is a mechanism for declaratively handling async operations (data loading, code splitting) in the component tree. It lets you specify a fallback UI to show while children are "waiting" for something.

**Code splitting (original use case):**
```jsx
const LazyProfile = React.lazy(() => import('./Profile'));

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <LazyProfile />
    </Suspense>
  );
}
```

**Data fetching with Suspense (React 18+ / RSC / frameworks):**
In frameworks like Next.js App Router, async Server Components automatically integrate with Suspense:
```jsx
// Wrapping an async component
<Suspense fallback={<PostsSkeleton />}>
  <Posts /> {/* async component that fetches data */}
</Suspense>
```

**The `use` hook (React 19):** Lets you `use(promise)` inside a component — if the promise isn't resolved, Suspense catches it and shows the fallback.

**Suspense + error boundaries:** Combine both for a complete loading/error UX:
```jsx
<ErrorBoundary fallback={<Error />}>
  <Suspense fallback={<Loading />}>
    <DataComponent />
  </Suspense>
</ErrorBoundary>
```

**Related:** [REACT-037 Error Boundaries](#react-037) | [REACT-051 Lazy Loading](#react-051) | [REACT-105 use hook](#react-105)

**Source:** GreatFrontEnd Q38

---

## REACT-039

### What is React hydration?

**Hydration** is the process by which React attaches event listeners and initializes component state to server-rendered HTML, making the page interactive without re-rendering from scratch.

**SSR + Hydration flow:**
```
1. Server renders React tree → HTML string → sent to browser
2. Browser displays HTML immediately (fast First Contentful Paint)
3. React JS bundle loads
4. React "hydrates" the existing DOM — attaches handlers, restores state
5. App becomes fully interactive
```

```jsx
// Server (Node.js)
import { renderToString } from 'react-dom/server';
const html = renderToString(<App />);

// Client
import { hydrateRoot } from 'react-dom/client';
hydrateRoot(document.getElementById('root'), <App />);
```

**Hydration mismatch:** If the server-rendered HTML differs from what React expects on the client (e.g., different timestamps, random IDs), React logs a hydration error and re-renders the mismatched subtree. This defeats the performance benefit.

**Common mismatch causes:**
- Using `Math.random()` or `Date.now()` in render.
- Rendering browser-only APIs (`window`, `localStorage`) without guards.
- Different data between server and client renders.

**Selective hydration (React 18):** With streaming SSR and `<Suspense>`, React can hydrate parts of the page independently and prioritize hydrating components the user is interacting with.

**Related:** [REACT-057 SSR](#react-057) | [REACT-032 useId](#react-032) | [REACT-106 Server Components](#react-106)

**Source:** GreatFrontEnd Q39

---

## REACT-040

### What are React Portals used for?

**Portals** let you render a component's children into a different DOM node than the component's parent — while keeping them in the same React component tree (with full event bubbling and context access).

**Without a portal:** A modal rendered inside a deeply nested component inherits CSS `overflow: hidden` or `z-index` stacking from ancestors, causing clipping or layering bugs.

**With a portal:** The modal's DOM node is attached to `document.body` directly, escaping ancestor CSS constraints.

```jsx
import { createPortal } from 'react-dom';

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.body // render target — outside the component's DOM parent
  );
}
```

**Key behaviors:**
- Events bubble up through the **React tree**, not the DOM tree — so clicking inside the portal still triggers handlers in parent React components.
- Context works normally — a portal child still has access to its React ancestors' context.

**Common use cases:** Modals, tooltips, dropdowns, toasts, drawers — anything that needs to visually escape its container.

**Related:** [REACT-038 Suspense](#react-038)

**Source:** GreatFrontEnd Q40

---

## REACT-041

### What is React Strict Mode and what are its benefits?

`React.StrictMode` is a development-only tool that helps identify potential problems by running extra checks and intentionally invoking certain functions twice.

```jsx
// Wrap your app (or a subtree) in StrictMode
import { StrictMode } from 'react';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

**What Strict Mode does (development only):**
- **Double-invokes** render functions, state initializers, and reducer functions to expose side effects in "pure" code.
- **Double-fires** `useEffect` setup+cleanup to expose missing cleanup logic.
- Warns about deprecated APIs (`findDOMNode`, legacy Context, string refs).
- Warns about unexpected side effects in render.

**Practical benefit — catching effect cleanup bugs:**
```jsx
useEffect(() => {
  const sub = subscribe(callback);
  // If you forget the cleanup, StrictMode's double-fire reveals the leak
  return () => sub.unsubscribe(); // cleanup required
}, []);
```

**Strict Mode does NOT:**
- Run in production.
- Break valid code — if your app works correctly, double-invocation won't break it.
- Affect performance of production builds.

**Related:** [REACT-026 useEffect cleanup](#react-026) | [REACT-022 Immutable State](#react-022)

**Source:** GreatFrontEnd Q41

---

## REACT-042

### What is code splitting in a React application?

**Code splitting** is the practice of breaking a JavaScript bundle into smaller chunks that are loaded on demand, rather than shipping one large bundle. This reduces initial load time because the browser only downloads code for the current page/route.

**React's built-in tools:**

**`React.lazy` + `Suspense`:**
```jsx
// The Profile component's JS is only loaded when this component renders
const Profile = React.lazy(() => import('./pages/Profile'));
const Settings = React.lazy(() => import('./pages/Settings'));

function App() {
  return (
    <Suspense fallback={<PageSpinner />}>
      <Routes>
        <Route path="/profile" element={<Profile />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

**Route-based splitting** (most common): each route becomes a separate chunk loaded on navigation.

**Component-based splitting**: split heavy components (rich text editors, chart libraries, maps) that aren't needed immediately.

**Dynamic import** (non-React):
```js
button.addEventListener('click', async () => {
  const { default: heavyLib } = await import('./heavyLib');
  heavyLib.run();
});
```

**Bundler integration:** Webpack, Vite, and Rollup all support code splitting automatically when they see dynamic `import()`. Next.js does route-level splitting by default.

**Related:** [REACT-038 Suspense](#react-038) | [REACT-051 Lazy Loading](#react-051)

**Source:** GreatFrontEnd Q42

---

## REACT-043

### How do you optimize React context performance to reduce re-renders?

Every component that calls `useContext(MyContext)` re-renders whenever the context value changes — even if the component only uses one field of a large context object.

**Strategy 1 — Split contexts by concern:**
```jsx
// BAD — any change to either user or theme re-renders all consumers
const AppContext = createContext({ user, theme, setTheme, setUser });

// GOOD — consumers only subscribe to what they need
const UserContext = createContext({ user, setUser });
const ThemeContext = createContext({ theme, setTheme });
```

**Strategy 2 — Memoize the context value:**
```jsx
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  // Without useMemo, a new object is created on every Provider render
  const value = useMemo(() => ({ theme, setTheme }), [theme]);

  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
}
```

**Strategy 3 — Use `useContextSelector` (via `use-context-selector` library):**
Lets consumers subscribe to only a slice of context, similar to Redux `useSelector`.

**Strategy 4 — Separate read and write contexts:**
```jsx
// Components that only dispatch actions don't need to re-render on state changes
const StateContext = createContext(state);
const DispatchContext = createContext(dispatch);
```

**Strategy 5 — Consider external state management** (Zustand, Jotai) for frequent high-frequency updates where context's re-render behavior is too broad.

**Related:** [REACT-033 useContext](#react-033) | [REACT-046 Context Pitfalls](#react-046) | [REACT-048 State vs Context vs External](#react-048)

**Source:** GreatFrontEnd Q43

---

## REACT-044

### What is the Flux pattern?

**Flux** is an application architecture pattern introduced by Facebook alongside React to manage unidirectional data flow in client-side apps.

**Core concepts:**
```
Action → Dispatcher → Store → View → (user interaction) → Action
```

- **Action:** A plain object describing what happened (`{ type: 'ADD_ITEM', payload: item }`).
- **Dispatcher:** A central hub that broadcasts every action to all registered stores.
- **Store:** Holds application state and business logic. Listens to actions and updates state accordingly. Emits a change event.
- **View:** React components. They listen to store changes and re-render. User interaction creates new Actions.

**Why Flux matters:**
Before Flux, two-way data binding (common in Angular/Backbone) made it hard to reason about what changed and why. Flux enforced a single, predictable direction for data changes.

**Flux → Redux:**
Redux simplified Flux by replacing multiple stores with a single store and replacing the Dispatcher with reducer functions. Redux is the dominant Flux-inspired library today.

**Related:** [REACT-045 One-way data flow](#react-045) | [REACT-048 State management decisions](#react-048)

**Source:** GreatFrontEnd Q44

---

## REACT-045

### Explain one-way data flow in React

In React, data flows in **one direction: from parent to child via props**. A child component cannot modify its parent's state directly — it must call a callback function the parent passed down.

```
Parent state
    ↓ (props)
Child Component
    ↓ (callback prop — e.g., onChange)
Parent state update
    ↓ (re-render with new props)
Child Component (updated view)
```

**Example:**
```jsx
function Parent() {
  const [value, setValue] = useState('');

  // Data flows DOWN as props; events flow UP via callbacks
  return <Input value={value} onChange={setValue} />;
}

function Input({ value, onChange }) {
  // Can't set parent state directly — must call the callback
  return <input value={value} onChange={e => onChange(e.target.value)} />;
}
```

**Benefits of one-way flow:**
- Easier to debug — follow data from root down to find its source.
- Predictable renders — components are a pure function of their props.
- Avoids cascading updates common in two-way binding frameworks.

**Related:** [REACT-044 Flux](#react-044) | [REACT-050 Prop Drilling](#react-050) | [REACT-015 Lifting State Up](#react-015)

**Source:** GreatFrontEnd Q45

---

## REACT-046

### What are pitfalls of using context in React?

**1. Unnecessary re-renders:** All consumers re-render when the context value changes — even if they only use a part of the value. See [REACT-043](#react-043) for optimization strategies.

**2. Tight coupling:** Components that consume context are harder to reuse in isolation — they depend on an ancestor providing the right context.

**3. Testing difficulty:** Components with context dependencies need the context provider in tests, adding boilerplate.

**4. Not designed for high-frequency updates:** Context works well for slow-changing values (theme, locale, auth user). For state that updates many times per second (e.g., mouse position, real-time data), use a dedicated state manager.

**5. Context does not replace state management:** Context is a dependency injection mechanism, not a state manager. It has no built-in selectors, middleware, or devtools.

**6. Implicit dependencies:** It's not always obvious which components consume a context — the dependency is invisible in the JSX.

**7. Provider nesting hell:** Multiple contexts at the app root can create deep nesting:
```jsx
<AuthContext.Provider>
  <ThemeContext.Provider>
    <LocaleContext.Provider>
      <RouterContext.Provider>
        <App />
      </RouterContext.Provider>
    </LocaleContext.Provider>
  </ThemeContext.Provider>
</AuthContext.Provider>
```
Mitigate by composing providers or using a combiner utility.

**Related:** [REACT-043 Context performance](#react-043) | [REACT-048 When to use what](#react-048)

**Source:** GreatFrontEnd Q46

---

## REACT-047

### What are some React anti-patterns?

**1. Deriving state from props without `useMemo`:**
```jsx
// BAD — stale if props change after initial render
const [filteredItems, setFilteredItems] = useState(props.items.filter(active));

// GOOD — derived value, not state
const filteredItems = useMemo(() => props.items.filter(active), [props.items, active]);
```

**2. Using array index as `key` in dynamic lists** — see [REACT-007](#react-007).

**3. Putting everything in a single giant component** — breaks reusability and makes testing hard.

**4. Directly mutating state** — see [REACT-022](#react-022).

**5. Unnecessary `useEffect` for derived state:**
```jsx
// BAD — using effect to compute a derived value
const [fullName, setFullName] = useState('');
useEffect(() => { setFullName(`${first} ${last}`); }, [first, last]);

// GOOD — just compute it inline
const fullName = `${first} ${last}`;
```

**6. Missing cleanup in `useEffect`** — leads to memory leaks and state updates on unmounted components.

**7. Prop drilling excessively** — see [REACT-050](#react-050).

**8. Overusing `useContext` for frequently changing state** — causes too many re-renders.

**9. Not memoizing expensive child components** — parent re-renders unnecessarily cascade to all children.

**10. Forgetting `key` on `React.Fragment` in lists:**
```jsx
// Wrong — can't put key on shorthand fragments
items.map(i => <><dt>{i.term}</dt><dd>{i.def}</dd></>)

// Correct
items.map(i => <Fragment key={i.id}><dt>{i.term}</dt><dd>{i.def}</dd></Fragment>)
```

**Related:** [REACT-006 keys](#react-006) | [REACT-022 Immutability](#react-022) | [REACT-026 useEffect](#react-026)

**Source:** GreatFrontEnd Q47

---

## REACT-048

### How do you decide between React state, context, and external state managers?

| Scenario | Best tool |
|---|---|
| UI state local to one component (open/closed, input value) | `useState` / `useReducer` |
| State shared between a few nearby components | Lift state up to common ancestor |
| App-wide slow-changing values (theme, locale, auth user) | Context API |
| Complex state with many consumers, frequent updates | External state manager |
| Server state (async data, caching, invalidation) | React Query / SWR / Apollo |

**Decision tree:**
1. Is the state needed by only one component? → `useState`.
2. Is it needed by a handful of siblings? → Lift to parent.
3. Is it needed far away in the tree, changes infrequently? → Context.
4. Does it change often, have many consumers, or need middleware/devtools? → Zustand / Redux / Jotai.
5. Is it remote server data? → React Query / SWR.

**External state managers compared:**

| Library | Best for |
|---|---|
| **Redux Toolkit** | Large teams, complex state, time-travel debugging |
| **Zustand** | Simple global state, small/medium apps, minimal boilerplate |
| **Jotai** | Atomic state, fine-grained subscriptions |
| **React Query / SWR** | Server state — caching, deduplication, refetching |

**Related:** [REACT-043 Context performance](#react-043) | [REACT-046 Context pitfalls](#react-046)

**Source:** GreatFrontEnd Q48

---

## REACT-049

### What happens when `setState` is called in React?

When you call a state setter (e.g., `setState` in a class component, or a `useState` setter in a function component), React does not update state synchronously. Instead:

1. **State update is enqueued** in React's internal update queue.
2. **React schedules a re-render** (batched with other updates in the same event).
3. **Batching:** Multiple `setState` calls within the same event handler are batched into a single re-render (React 18 batches all updates, including those in `setTimeout` and Promises).
4. **Re-render phase:** React re-executes the component function with the new state value.
5. **Reconciliation:** New VDOM is diffed against old.
6. **Commit phase:** Only the changed DOM nodes are updated.

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1); // enqueued
    setCount(count + 1); // enqueued — same value, reads same stale `count`
    // Result: count increments by 1, NOT 2

    // To increment twice, use the callback form:
    // setCount(c => c + 1);
    // setCount(c => c + 1); // result: count increments by 2
  }
}
```

**React 18 automatic batching:** Before React 18, `setState` inside `setTimeout` or Promises triggered an immediate re-render. React 18 batches these too, improving performance.

**Related:** [REACT-028 setState callback](#react-028) | [REACT-022 Immutable state](#react-022) | [REACT-054 Concurrent features](#react-054)

**Source:** GreatFrontEnd Q49

---

## REACT-050

### What is prop drilling?

**Prop drilling** is the pattern of passing props through multiple intermediate components that don't use the data themselves — solely to get it to a deeply nested component that does.

```jsx
// Theme needs to reach DeepButton but passes through 3 components that don't use it
function App() {
  const [theme, setTheme] = useState('dark');
  return <Page theme={theme} />;
}
function Page({ theme }) {
  return <Section theme={theme} />;  // doesn't use theme itself
}
function Section({ theme }) {
  return <DeepButton theme={theme} />; // doesn't use theme itself
}
function DeepButton({ theme }) {
  return <button className={theme}>Click</button>; // finally uses it
}
```

**Problems with prop drilling:**
- Intermediate components become unnecessarily coupled to data they don't need.
- Refactoring is painful — adding or removing a layer requires updating every component in the chain.
- Makes components harder to reuse independently.

**Solutions:**
1. **Context API** — for slow-changing, widely needed values.
2. **Component composition** — pass JSX children or render props instead of data.
3. **External state manager** (Zustand, Redux) — access state directly from any component.

```jsx
// Composition solution — avoids drilling by passing JSX elements
function App() {
  const [theme] = useState('dark');
  return <Page content={<DeepButton theme={theme} />} />;
}
function Page({ content }) { return <Section content={content} />; }
function Section({ content }) { return <div>{content}</div>; }
```

**Related:** [REACT-015 Lifting State Up](#react-015) | [REACT-033 useContext](#react-033) | [REACT-048 State management](#react-048)

**Source:** GreatFrontEnd Q50

---

## REACT-051

### What is lazy loading in React?

**Lazy loading** means deferring the load of a component's JavaScript bundle until the component is actually needed — typically at route navigation or when it becomes visible.

**`React.lazy` with `Suspense`:**
```jsx
// The import() is only executed when <HeavyChart /> first renders
const HeavyChart = React.lazy(() => import('./HeavyChart'));

function Dashboard() {
  const [showChart, setShowChart] = useState(false);

  return (
    <>
      <button onClick={() => setShowChart(true)}>Show Chart</button>
      {showChart && (
        <Suspense fallback={<div>Loading chart...</div>}>
          <HeavyChart />
        </Suspense>
      )}
    </>
  );
}
```

**Route-level lazy loading (React Router v6):**
```jsx
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));

<Suspense fallback={<PageLoader />}>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/about" element={<About />} />
  </Routes>
</Suspense>
```

**Intersection Observer — lazy load on scroll:**
```jsx
function LazySection() {
  const ref = useRef(null);
  const [visible, setVisible] = useState(false);
  const HeavyComp = lazy(() => import('./HeavyComp'));

  useEffect(() => {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) { setVisible(true); observer.disconnect(); }
    });
    observer.observe(ref.current);
    return () => observer.disconnect();
  }, []);

  return (
    <div ref={ref}>
      {visible && <Suspense fallback={<Skeleton />}><HeavyComp /></Suspense>}
    </div>
  );
}
```

**Related:** [REACT-038 Suspense](#react-038) | [REACT-042 Code Splitting](#react-042)

**Source:** GreatFrontEnd Q51

---

## REACT-052

### What are synthetic events in React?

**Synthetic events** are React's cross-browser wrapper around native browser events. They implement the same interface as native events (`preventDefault`, `stopPropagation`, `target`, etc.) but normalize browser inconsistencies.

```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault(); // same API as native event
    console.log(e.target.elements.email.value);
  }

  function handleChange(e) {
    console.log(e.target.value); // works the same across browsers
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" onChange={handleChange} />
    </form>
  );
}
```

**Event delegation:** React does not attach event listeners to individual DOM nodes. Instead, it attaches a single listener to the **root container** (`document` in React 17-, `root` in React 17+). Events bubble up to that listener and React routes them to the correct handler. This is more memory-efficient than per-element listeners.

**React 17 change:** Event delegation moved from `document` to the React root container, fixing issues with embedding multiple React apps or integrating with non-React code.

**Accessing native event:** `e.nativeEvent` gives you the underlying browser event.

**Note:** In React 17+, synthetic events are no longer pooled — you can access the event asynchronously without calling `e.persist()`.

**Related:** [REACT-009 Function components](#react-009)

**Source:** GreatFrontEnd Q52

---

## REACT-053

### What are the React component lifecycle methods in class components?

**Mounting phase (component added to DOM):**
- `constructor(props)` — initialize state, bind methods.
- `static getDerivedStateFromProps(props, state)` — sync state from props before render (rare).
- `render()` — return JSX.
- `componentDidMount()` — DOM is ready; good place for data fetching, subscriptions.

**Updating phase (props or state change):**
- `static getDerivedStateFromProps` — runs again.
- `shouldComponentUpdate(nextProps, nextState)` — return `false` to skip re-render (optimization).
- `render()`.
- `getSnapshotBeforeUpdate(prevProps, prevState)` — capture scroll position etc. before DOM update.
- `componentDidUpdate(prevProps, prevState, snapshot)` — DOM updated; run side effects, compare prev/next props.

**Unmounting phase (component removed from DOM):**
- `componentWillUnmount()` — cancel subscriptions, clear timers.

**Error handling:**
- `static getDerivedStateFromError(error)` — render fallback UI.
- `componentDidCatch(error, info)` — log the error.

**Hook equivalents:**
```
componentDidMount    ≈ useEffect(() => { ... }, [])
componentDidUpdate   ≈ useEffect(() => { ... }, [deps])
componentWillUnmount ≈ useEffect(() => { return () => cleanup(); }, [])
shouldComponentUpdate ≈ React.memo / useMemo
```

**Related:** [REACT-025 useEffect](#react-025) | [REACT-037 Error Boundaries](#react-037) | [REACT-009 Class vs Function](#react-009)

**Source:** GreatFrontEnd Q53

---

## REACT-054

### What are concurrent features in React, and how do they improve rendering?

**Concurrent React** (React 18) allows React to prepare multiple versions of the UI simultaneously, interrupt work-in-progress renders, and prioritize urgent updates over non-urgent ones.

**Key concurrent features:**

**`useTransition`** — mark a state update as non-urgent, keeping the UI responsive:
```jsx
const [isPending, startTransition] = useTransition();

function handleSearch(query) {
  setInput(query); // urgent — update input immediately
  startTransition(() => {
    setResults(filterItems(query)); // non-urgent — can be interrupted
  });
}

return (
  <>
    <input value={input} onChange={e => handleSearch(e.target.value)} />
    {isPending ? <Spinner /> : <Results data={results} />}
  </>
);
```

**`useDeferredValue`** — defer re-rendering of a specific value:
```jsx
const deferredQuery = useDeferredValue(query);
// HeavyList only re-renders when React has idle time
return <HeavyList filter={deferredQuery} />;
```

**`Suspense` with streaming SSR** — stream HTML from the server progressively, hydrating high-priority parts first.

**`createRoot`** — the new API that enables concurrent features (replaces `ReactDOM.render`).

**Why it matters:** Before Concurrent React, a large render would block the main thread, causing dropped frames. Concurrent features let React yield to the browser between chunks of render work.

**Related:** [REACT-011 Fiber](#react-011) | [REACT-055 Prioritization](#react-055) | [REACT-109 useTransition vs useDeferredValue](#react-109)

**Source:** GreatFrontEnd Q54

---

## REACT-055

### How does React handle concurrent rendering with multiple updates and prioritize them?

React assigns a **priority lane** to each update. Higher-priority updates interrupt and preempt lower-priority ones.

**Priority lanes (simplified):**

| Priority | Examples | Behavior |
|---|---|---|
| **Synchronous** | Controlled inputs, `flushSync` | Processed immediately |
| **User-blocking** | Click, keypress event handlers | Processed before paint |
| **Normal** | `useState` in async callbacks | Batched, processed soon |
| **Transition** | `startTransition` wrapped updates | Can be deferred/interrupted |
| **Idle** | Background prefetch | Processed when browser is idle |

**Example — input stays responsive during expensive filter:**
```jsx
function Search() {
  const [input, setInput] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    setInput(e.target.value);          // high priority — sync
    startTransition(() => {
      setResults(filter(e.target.value)); // low priority — can be interrupted
    });
  }

  return (
    <>
      <input value={input} onChange={handleChange} />
      {isPending && <Spinner />}
      <ResultList items={results} />
    </>
  );
}
```

If the user types while the `results` update is in progress, React **abandons the in-progress render** and immediately handles the new input keystroke, then restarts the deferred work.

**Related:** [REACT-054 Concurrent Features](#react-054) | [REACT-011 Fiber](#react-011) | [REACT-056 Expensive computations](#react-056)

**Source:** GreatFrontEnd Q55

---

## REACT-056

### How do you handle long-running tasks or expensive computations in React without blocking the UI?

**1. `useTransition` — defer non-urgent renders:**
```jsx
startTransition(() => {
  setFilteredList(heavyFilter(data, query));
});
```

**2. `useDeferredValue` — defer re-rendering a slow subtree:**
```jsx
const deferredQuery = useDeferredValue(query);
const filtered = useMemo(() => heavyFilter(data, deferredQuery), [data, deferredQuery]);
```

**3. `useMemo` — cache expensive computation results:**
```jsx
const sortedData = useMemo(() => expensiveSort(data), [data]);
```

**4. Web Workers — offload CPU work off the main thread:**
```jsx
useEffect(() => {
  const worker = new Worker(new URL('./heavy.worker.js', import.meta.url));
  worker.postMessage({ data });
  worker.onmessage = (e) => setResult(e.data);
  return () => worker.terminate();
}, [data]);
```

**5. Virtualization — render only visible list items:**
```jsx
import { FixedSizeList } from 'react-window';

<FixedSizeList height={600} itemCount={100000} itemSize={35}>
  {({ index, style }) => <Row key={index} style={style} item={items[index]} />}
</FixedSizeList>
```

**6. Chunking with `requestIdleCallback` / `scheduler`:**
Process large datasets in small chunks, yielding control between chunks so the browser can handle user input.

**Related:** [REACT-054 Concurrent features](#react-054) | [REACT-030 useMemo](#react-030) | [REACT-055 Prioritization](#react-055)

**Source:** GreatFrontEnd Q56

---

## REACT-057

### What is server-side rendering (SSR) and what are its benefits?

**SSR** means generating the initial HTML for a React app on the **server** (Node.js), sending that HTML to the browser, and then hydrating it on the client.

```
Client request → Server runs React → HTML string → Browser displays HTML → JS loads → Hydration
```

**Benefits:**
- **Faster First Contentful Paint (FCP)** — users see content before JS downloads/parses.
- **SEO** — search engines receive pre-rendered HTML, not an empty `<div id="root">`.
- **Social media previews** — meta tags are present in the initial HTML.
- **Better performance on low-powered devices** — less work for the client.

**Drawbacks:**
- **Time To Interactive (TTI) lag** — page looks ready but isn't interactive until hydration completes.
- **Server load** — rendering on every request consumes server CPU.
- **Hydration mismatch** bugs if server and client render different output.
- **Increased complexity** — code must be "isomorphic" (run on both server and browser).

**React APIs:**
```jsx
// Server
import { renderToString } from 'react-dom/server';
const html = renderToString(<App />);

// React 18 — streaming SSR
import { renderToPipeableStream } from 'react-dom/server';
const { pipe } = renderToPipeableStream(<App />, { onShellReady() { pipe(res); } });

// Client
import { hydrateRoot } from 'react-dom/client';
hydrateRoot(document.getElementById('root'), <App />);
```

**Frameworks:** Next.js (Pages Router `getServerSideProps`, App Router Server Components), Remix.

**Related:** [REACT-039 Hydration](#react-039) | [REACT-058 SSG](#react-058) | [REACT-106 Server Components](#react-106)

**Source:** GreatFrontEnd Q57

---

## REACT-058

### What is static site generation (SSG) in React?

**SSG** generates HTML at **build time**, not at request time. The HTML files are pre-built and served as static assets (CDN-friendly, no server needed per request).

**SSG vs SSR vs CSR:**

| | SSG | SSR | CSR |
|---|---|---|---|
| **When HTML is generated** | Build time | Per request | In browser |
| **Server needed at runtime?** | No | Yes | No |
| **Data freshness** | Stale until rebuild | Fresh on every request | Fresh on every load |
| **Best for** | Blogs, docs, marketing | Dashboards, personalized pages | Highly interactive SPAs |

**Next.js example (Pages Router):**
```jsx
// getStaticProps runs at build time
export async function getStaticProps() {
  const posts = await fetchAllPosts();
  return { props: { posts }, revalidate: 60 }; // ISR: revalidate every 60 seconds
}

export default function Blog({ posts }) {
  return <ul>{posts.map(p => <li key={p.id}>{p.title}</li>)}</ul>;
}
```

**Incremental Static Regeneration (ISR):** Next.js feature that regenerates individual pages in the background after a revalidation interval — combines the speed of static with the freshness of SSR.

**When to choose SSG:**
- Content doesn't change per user (blog posts, documentation, product pages).
- Maximum performance and CDN caching are priorities.
- You can tolerate slightly stale data between rebuilds.

**Related:** [REACT-057 SSR](#react-057) | [REACT-039 Hydration](#react-039)

**Source:** GreatFrontEnd Q58

---

## REACT-059

### What are higher-order components (HOCs)?

A **Higher-Order Component (HOC)** is a function that takes a component and returns a new, enhanced component. It's a pattern for reusing component logic.

```jsx
// HOC that adds a loading state
function withLoader(WrappedComponent, selectData) {
  return function WithLoader(props) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
      selectData(props).then(d => { setData(d); setLoading(false); });
    }, []);

    if (loading) return <Spinner />;
    return <WrappedComponent {...props} data={data} />;
  };
}

// Usage
const UserListWithLoader = withLoader(UserList, props => fetchUsers(props.teamId));
```

**HOC conventions:**
- Name the returned component for DevTools: `WithLoader.displayName = \`withLoader(\${WrappedComponent.displayName})\``.
- Pass through all props: `{...props}`.
- Don't mutate the original component.

**HOC pitfalls:**
- Wrapper hell — multiple HOCs create deeply nested component trees.
- Prop name collisions.
- Difficult to compose multiple HOCs.

**Modern alternative:** Custom hooks solve most HOC use cases with less complexity. HOCs are still useful when you need to wrap JSX or handle ref forwarding at the component level.

**Related:** [REACT-034 Custom Hooks](#react-034) | [REACT-061 Render Props](#react-061)

**Source:** GreatFrontEnd Q59

---

## REACT-060

### What is the presentational vs container component pattern?

This pattern (popularized by Dan Abramov) separates components by responsibility:

**Presentational components ("dumb"):**
- Concerned with *how things look*.
- Receive all data via props.
- Contain no business logic or data fetching.
- Are highly reusable.

```jsx
// Pure presentational — no state, no data fetching
function UserCard({ name, avatar, role, onFollow }) {
  return (
    <div className="card">
      <img src={avatar} alt={name} />
      <h2>{name}</h2>
      <p>{role}</p>
      <button onClick={onFollow}>Follow</button>
    </div>
  );
}
```

**Container components ("smart"):**
- Concerned with *how things work*.
- Fetch data, manage state, handle events.
- Pass data down to presentational components.

```jsx
function UserCardContainer({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => { fetchUser(userId).then(setUser); }, [userId]);

  if (!user) return <Skeleton />;
  return <UserCard {...user} onFollow={() => followUser(userId)} />;
}
```

**Modern note:** This pattern is less rigidly followed today. Custom hooks extract container logic cleanly, making the explicit container wrapper often unnecessary. The principle of separating concerns remains valid, but the implementation is flexible.

**Related:** [REACT-019 Stateless](#react-019) | [REACT-020 Stateful](#react-020) | [REACT-034 Custom Hooks](#react-034)

**Source:** GreatFrontEnd Q60

---

## REACT-061

### What are render props?

A **render prop** is a prop whose value is a function that returns JSX. It lets a component share stateful logic by calling that function to render its output.

```jsx
// MouseTracker shares mouse position via a render prop
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });

  function handleMove(e) {
    setPos({ x: e.clientX, y: e.clientY });
  }

  return <div onMouseMove={handleMove}>{render(pos)}</div>;
}

// Usage
<MouseTracker render={({ x, y }) => (
  <p>Mouse is at ({x}, {y})</p>
)} />
```

**Children as a function** (common render prop pattern):
```jsx
<MouseTracker>
  {({ x, y }) => <Cursor x={x} y={y} />}
</MouseTracker>
```

**When render props are still useful:**
- When you need to share rendering logic (not just state logic) — e.g., a `<Transition>` component that controls animation state while delegating what to render.
- Library APIs (React Router's `<Route render>`, Formik's `<Field>`, Downshift).

**Modern alternative:** Custom hooks have largely replaced render props for logic sharing, as they're cleaner and don't add component nesting.

**Related:** [REACT-034 Custom Hooks](#react-034) | [REACT-059 HOCs](#react-059) | [REACT-062 Composition](#react-062)

**Source:** GreatFrontEnd Q61

---

## REACT-062

### What is the composition pattern in React?

**Composition** means building complex UIs by combining smaller, focused components — using `children`, slots, and rendering components inside other components — rather than inheritance or HOC chains.

**Children composition:**
```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      {title && <h2 className="card-title">{title}</h2>}
      <div className="card-body">{children}</div>
    </div>
  );
}

// Compose flexibly
<Card title="User Profile">
  <Avatar src={user.avatar} />
  <UserDetails user={user} />
</Card>
```

**Named slots (multiple children areas):**
```jsx
function Layout({ sidebar, main, footer }) {
  return (
    <div className="layout">
      <aside>{sidebar}</aside>
      <main>{main}</main>
      <footer>{footer}</footer>
    </div>
  );
}

<Layout
  sidebar={<Nav />}
  main={<Dashboard />}
  footer={<Footer />}
/>
```

**Why composition over inheritance:**
- React components are functions — there's no natural class hierarchy to extend.
- Composition is more flexible: you can swap, rearrange, and add behaviors at the call site.
- Avoids tightly coupling parent and child implementation.
- React itself recommends "use composition instead of inheritance to reuse code between components."

**Related:** [REACT-059 HOCs](#react-059) | [REACT-061 Render Props](#react-061) | [REACT-050 Prop Drilling](#react-050)

**Source:** GreatFrontEnd Q62

---

## REACT-063

### How do you re-render the view when the browser is resized?

Listen to the `resize` event via `useEffect` and store the window dimensions in state. Clean up the listener on unmount.

```jsx
function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });

  useEffect(() => {
    function handleResize() {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    }

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []); // empty deps — add listener once, remove on unmount

  return size;
}

function ResponsiveComponent() {
  const { width } = useWindowSize();

  return (
    <div>
      {width < 768 ? <MobileView /> : <DesktopView />}
    </div>
  );
}
```

**Optimization — debounce to avoid excessive state updates:**
```jsx
useEffect(() => {
  let timeoutId;
  function handleResize() {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      setSize({ width: window.innerWidth, height: window.innerHeight });
    }, 150);
  }
  window.addEventListener('resize', handleResize);
  return () => { window.removeEventListener('resize', handleResize); clearTimeout(timeoutId); };
}, []);
```

**SSR note:** `window` is not defined on the server — guard with `typeof window !== 'undefined'` or initialize inside `useEffect`.

**Related:** [REACT-034 Custom Hooks](#react-034) | [REACT-026 useEffect cleanup](#react-026)

**Source:** GreatFrontEnd Q63

---

## REACT-064

### How do you handle asynchronous data loading in React applications?

**Pattern 1 — `useEffect` + local state (manual):**
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;
    setLoading(true);
    setError(null);
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => { if (!cancelled) { setUser(data); setLoading(false); } })
      .catch(err => { if (!cancelled) { setError(err); setLoading(false); } });
    return () => { cancelled = true; };
  }, [userId]);

  if (loading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;
  return <UserCard user={user} />;
}
```

**Pattern 2 — React Query (recommended for most apps):**
```jsx
import { useQuery } from '@tanstack/react-query';

function UserProfile({ userId }) {
  const { data: user, isLoading, error } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetch(`/api/users/${userId}`).then(res => res.json()),
  });

  if (isLoading) return <Spinner />;
  if (error) return <ErrorMessage error={error} />;
  return <UserCard user={user} />;
}
```

React Query handles caching, deduplication, background refetch, stale-while-revalidate, and pagination out of the box.

**Pattern 3 — React 19 `use` hook with Suspense:**
```jsx
function UserProfile({ userId }) {
  const userPromise = fetchUser(userId); // cached promise
  return (
    <Suspense fallback={<Spinner />}>
      <UserData promise={userPromise} />
    </Suspense>
  );
}

function UserData({ promise }) {
  const user = use(promise); // suspends until resolved
  return <UserCard user={user} />;
}
```

**Related:** [REACT-065 Data fetching pitfalls](#react-065) | [REACT-026 useEffect](#react-026) | [REACT-038 Suspense](#react-038)

**Source:** GreatFrontEnd Q64

---

## REACT-065

### What are common pitfalls when data fetching in React?

**1. Race conditions — stale responses overwriting newer data:**
```jsx
// BUG: if userId changes quickly, an older response may arrive after a newer one
useEffect(() => {
  fetch(`/api/users/${userId}`)
    .then(res => res.json())
    .then(setUser); // may set user from a stale request
}, [userId]);

// FIX: cancel flag or AbortController
useEffect(() => {
  const controller = new AbortController();
  fetch(`/api/users/${userId}`, { signal: controller.signal })
    .then(res => res.json())
    .then(setUser)
    .catch(err => { if (err.name !== 'AbortError') setError(err); });
  return () => controller.abort();
}, [userId]);
```

**2. Missing loading / error states** — assuming data is always available.

**3. Fetching in deeply nested components** — causes waterfall requests (parent fetches, renders child, child fetches). Lift data fetching up or use parallel queries.

**4. Not cleaning up async operations** — setting state on an unmounted component (can cause memory leaks and React warnings in React 17-).

**5. No caching** — re-fetching on every component mount when data hasn't changed. Use React Query, SWR, or a global cache.

**6. `useEffect` with missing dependencies** — stale closure reads outdated values. Use the ESLint exhaustive-deps rule.

**7. Fetching inside event handlers without feedback** — no loading indicator, no error handling.

**8. Sequential waterfall fetches** — fetching B only after A resolves when both could be parallel:
```jsx
// BAD: sequential
const user = await fetchUser(id);
const posts = await fetchPosts(user.id);

// GOOD: parallel
const [user, posts] = await Promise.all([fetchUser(id), fetchPosts(id)]);
```

**Related:** [REACT-064 Async data loading](#react-064) | [REACT-026 useEffect deps](#react-026) | [REACT-038 Suspense](#react-038)

**Source:** GreatFrontEnd Q65

---

*← [React Hooks](./02-hooks.md) | [Master Index](../reference/00-master-index.md) | [React Router →](./04-routing.md)*

---

## REACT-226

### What is render hijacking in React?

Render hijacking refers to a Higher-Order Component (HOC) intercepting and altering what another component renders — "hijacking" its output. It is one of the primary use cases for HOCs.

```jsx
function withLoadingState(WrappedComponent) {
  return function WithLoadingState({ isLoading, ...props }) {
    // Hijack the render — show Spinner instead of WrappedComponent
    if (isLoading) return <Spinner />;
    // Or alter props before passing them through
    return <WrappedComponent {...props} className="loaded" />;
  };
}

const DataTable = withLoadingState(RawDataTable);
<DataTable isLoading={true} data={[]} />  // Renders Spinner
<DataTable isLoading={false} data={rows} /> // Renders RawDataTable
```

Other render hijacking patterns:
- Wrapping the output in extra DOM/context
- Conditionally rendering based on auth/permissions
- Injecting additional props or overriding existing ones

Modern React prefers custom hooks for shared logic — HOC render hijacking is still valid for third-party library wrapping or render-level concerns.

**Related:** [REACT-059 — HOCs](./03-advanced.md#react-059) | [REACT-061 — Render props](./03-advanced.md#react-061)

**Source:** [SudheerJ SDJ-160](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-227

### How to prevent unnecessary setState updates?

`useState`'s `setState` (and class component's `this.setState`) bails out of re-rendering if the new value is the same as the current one (`Object.is` equality).

```jsx
const [count, setCount] = useState(0);

// No re-render if count is already 5
setCount(5); // if count === 5, React skips the re-render

// For objects, reference must change — Object.is compares references
const [obj, setObj] = useState({ x: 0 });
setObj(obj); // Same reference — no re-render
setObj({ ...obj }); // New reference — re-render even if values are identical
```

**To avoid unnecessary updates:**

```jsx
// Check before setting
function increment() {
  setCount((prev) => {
    const next = prev + 1;
    return next === prev ? prev : next; // no-op if same
  });
}

// For objects/arrays — only update when values actually change
function updateName(name) {
  if (name === user.name) return; // bail out before dispatch
  setUser((prev) => ({ ...prev, name }));
}
```

**Related:** [REACT-049 — setState mechanics](./03-advanced.md#react-049) | [REACT-035 — Re-rendering](./03-advanced.md#react-035)

**Source:** [SudheerJ SDJ-166](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-228

### What are the differences between Flux and Redux?

| Aspect | Flux | Redux |
|---|---|---|
| Stores | Multiple stores | Single store |
| Dispatcher | Central dispatcher object | No dispatcher — reducers are pure functions |
| Mutability | Stores can mutate state | State is immutable — reducers return new state |
| Setup | More boilerplate | Less boilerplate (especially with RTK) |
| DevTools | Limited | Redux DevTools with time-travel |
| Middleware | Not built-in | Middleware system (thunk, saga) |
| Async | Handled per-store | Handled via middleware |

Redux distilled the best ideas from Flux and added the constraint that there is only one store and reducers must be pure. This makes state transitions fully predictable and enables features like time-travel debugging.

**Related:** [REACT-044 — Flux](./03-advanced.md#react-044) | [REACT-111 — What is Redux](../react/08-redux.md#react-111)

**Source:** [SudheerJ SDJ-171](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-229

### What happens to uncaught errors in React 16 and later?

React 16 changed error behavior significantly: **any unhandled error during rendering causes the entire React component tree to unmount**.

Before React 16, a rendering error would leave the component in a broken state — potentially showing corrupt UI. React 16's behavior is more drastic but safer: users see nothing rather than incorrect data.

**Error boundary strategy:**

```jsx
// Without error boundaries — the whole app unmounts on any render error

// With error boundaries — only the subtree with the error unmounts
<AppShell>
  <ErrorBoundary fallback={<CrashedWidget />}>
    <UserWidget />       {/* If this crashes, only UserWidget is unmounted */}
  </ErrorBoundary>
  <ErrorBoundary fallback={<CrashedFeed />}>
    <NewsFeed />         {/* This is unaffected */}
  </ErrorBoundary>
</AppShell>
```

Best practice: place error boundaries at meaningful UI boundaries — each major section, not every component. Too granular and you hide real bugs; too coarse and the user loses too much UI on a minor error.

**Related:** [REACT-037 — Error boundaries](./03-advanced.md#react-037)

**Source:** [SudheerJ SDJ-175](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-230

### What are the scenarios where error boundaries don't catch errors?

Error boundaries catch errors during rendering, lifecycle methods, and constructors of components in the tree below them. They do **not** catch:

1. **Event handlers** — `onClick`, `onChange`, etc. Use try/catch inside handlers instead.
2. **Asynchronous code** — `setTimeout`, `Promise.then`, `fetch` callbacks, async/await. Use try/catch or `.catch()`.
3. **Server-side rendering** — error boundaries are client-side only.
4. **Errors in the error boundary itself** — an error thrown inside the boundary component is not caught by that same boundary.

```jsx
// ✗ Error boundary DOES NOT catch this
class MyComponent extends Component {
  handleClick = () => {
    throw new Error('Event handler error'); // Not caught by boundary
  };
  render() { return <button onClick={this.handleClick}>Click</button>; }
}

// ✓ Use try/catch in event handlers
handleClick = () => {
  try {
    riskyOperation();
  } catch (err) {
    this.setState({ error: err });
  }
};
```

**Related:** [REACT-037 — Error boundaries](./03-advanced.md#react-037) | [REACT-229 — Uncaught errors](#react-229)

**Source:** [SudheerJ SDJ-174](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-231

### What is the proper placement for error boundaries?

Placement determines how much UI is preserved when something breaks:

**Too coarse (single top-level boundary)** — the user loses the entire app on any error:

```jsx
<ErrorBoundary><App /></ErrorBoundary> // Too broad
```

**Too fine (every component)** — hides bugs and adds boilerplate:

```jsx
<ErrorBoundary><Button /></ErrorBoundary> // Overkill
```

**Recommended — route-level and feature-level:**

```jsx
<Layout>
  <ErrorBoundary fallback={<PageError />}>
    <Route path="/dashboard" element={<Dashboard />} />
  </ErrorBoundary>

  <Sidebar>
    <ErrorBoundary fallback={<WidgetError />}>
      <RecommendationsWidget />
    </ErrorBoundary>
  </Sidebar>
</Layout>
```

Rule of thumb: wrap each independently usable feature that can crash without taking down the rest of the page. The user should be able to continue using unaffected parts of the app.

**Related:** [REACT-037 — Error boundaries](./03-advanced.md#react-037) | [REACT-230 — Error boundary limits](#react-230)

**Source:** [SudheerJ SDJ-176](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-232

### What is MobX?

MobX is a reactive state management library. Unlike Redux's single store + actions + reducers model, MobX uses **observable state** — any property marked as observable is automatically tracked, and components re-render only when their specific observables change.

```jsx
import { makeAutoObservable } from 'mobx';
import { observer } from 'mobx-react-lite';

class CounterStore {
  count = 0;

  constructor() { makeAutoObservable(this); }

  increment() { this.count++; }
  decrement() { this.count--; }
}

const store = new CounterStore();

const Counter = observer(() => (
  <div>
    <p>{store.count}</p>
    <button onClick={() => store.increment()}>+</button>
  </div>
));
```

MobX mutations are direct (no dispatch, no action creators) — it wraps mutations in transactions automatically. The `observer` HOC subscribes the component to exactly the observables it reads during render.

**Related:** [REACT-111 — Redux](../react/08-redux.md#react-111) | [REACT-233 — MobX vs Redux](#react-233)

**Source:** [SudheerJ SDJ-227](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-233

### What are the differences between Redux and MobX?

| Aspect | Redux | MobX |
|---|---|---|
| Mental model | Unidirectional data flow, immutable | Reactive, observable state |
| Mutation | Immutable — pure reducers return new state | Mutable — direct mutations in actions |
| Boilerplate | More (reducers, actions, selectors) | Less (auto-observable, no action types) |
| Learning curve | Steeper | Gentler |
| Debugging | Excellent — DevTools, time-travel | Good — MobX DevTools |
| Performance | Opt-in memoization (selectors, memo) | Automatic — fine-grained subscriptions |
| Predictability | Very high — strict data flow | Moderate — mutation anywhere is easier to misuse |
| Testability | Very easy — pure functions | Requires setup for observable state |

**When to choose:**
- Redux: large teams, complex state, need strict architecture and excellent DevTools
- MobX: smaller teams, rapid development, mutable OOP-style preferred

**Related:** [REACT-232 — MobX](#react-232) | [REACT-111 — Redux](../react/08-redux.md#react-111)

**Source:** [SudheerJ SDJ-228](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-234

### What is the difference between async mode and concurrent mode?

"Async mode" was an earlier name used during React 16's experimental phase. "Concurrent mode" is the final, stable term used in React 18+.

The concept is the same: React can pause, interrupt, and resume rendering work, rather than blocking the main thread with a synchronous render.

| Term | React version | Status |
|---|---|---|
| Async mode | React 16 experimental | Renamed / deprecated terminology |
| Concurrent mode | React 18 opt-in (Experimental API) | Also superseded |
| Concurrent features | React 18 stable | `createRoot` enables by default |

In React 18, concurrent features (`useTransition`, `useDeferredValue`, `Suspense` for data fetching) are available by default when you use `createRoot`. There's no longer a separate "mode" to opt into — it's the default behavior.

**Related:** [REACT-054 — Concurrent rendering](./03-advanced.md#react-054) | [REACT-109 — useTransition vs useDeferredValue](../react/07-react19.md#react-109)

**Source:** [SudheerJ SDJ-231](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-235

### Can you use JavaScript URLs in React?

`javascript:` URLs (e.g., `href="javascript:void(0)"`) are a security risk — they can execute arbitrary JavaScript in the page context. React 16.9 added a deprecation warning when `javascript:` URLs are used in `href`, `src`, or `action` attributes.

```jsx
// ✗ Security risk — deprecated in React 16.9+
<a href="javascript:void(0)" onClick={handleClick}>Click</a>

// ✓ Use a button for click-only behavior
<button onClick={handleClick} type="button">Click</button>

// ✓ Use '#' with preventDefault if you must use an anchor
<a href="#" onClick={(e) => { e.preventDefault(); handleClick(); }}>Click</a>
```

The correct semantic choice for actions is `<button>`, not `<a>`. Anchor tags are for navigation; buttons are for actions.

**Related:** [REACT-187 — JSX injection prevention](../react/01-fundamentals.md#react-187)

**Source:** [SudheerJ SDJ-232](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-236

### What are the benefits of using TypeScript with React?

**Compile-time prop validation.** TypeScript catches incorrect prop types before the browser runs the code, unlike PropTypes which only warn at runtime:

```tsx
type ButtonProps = { label: string; disabled?: boolean; onClick: () => void };

function Button({ label, disabled = false, onClick }: ButtonProps) { ... }

<Button label={42} /> // TypeScript error at compile time
```

**Autocomplete and IDE support.** TypeScript enables IntelliSense for props, hook return values, and event types — faster development with fewer lookups.

**Safer refactoring.** Renaming a prop type propagates the error everywhere it's used — the compiler tells you what to fix.

**Self-documenting APIs.** Type signatures describe what a component expects without a separate docs page.

**Hook typing.** Generics make hook return types explicit:

```tsx
const [count, setCount] = useState<number>(0);
const data = useRef<HTMLDivElement>(null);
```

**Drawback:** Added build complexity, steeper learning curve, and verbose types for complex generics. Most teams consider the trade-off well worth it for projects beyond prototype scale.

**Related:** [REACT-021 — Static type checking](../react/01-fundamentals.md#react-021) | [REACT-141 — Flow](../react/10-libraries.md#react-141)

**Source:** [SudheerJ SDJ-235](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-237

### How to keep a user authenticated on page refresh when using Context?

Context state is in-memory and is lost on page refresh. To persist authentication across refreshes, initialize Context from a persistent store (localStorage, sessionStorage, or a cookie):

```jsx
function AuthProvider({ children }) {
  const [user, setUser] = useState(() => {
    // Lazy initializer — reads from localStorage on first render only
    try {
      const stored = localStorage.getItem('auth_user');
      return stored ? JSON.parse(stored) : null;
    } catch {
      return null;
    }
  });

  const login = (userData) => {
    setUser(userData);
    localStorage.setItem('auth_user', JSON.stringify(userData));
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('auth_user');
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

For security-sensitive auth, prefer **HttpOnly cookies** (sent automatically with requests, not accessible to JavaScript — protects against XSS). The pattern above is suitable for non-sensitive user preferences but not for auth tokens that protect API access.

**Related:** [REACT-033 — useContext](../react/02-hooks.md#react-033) | [REACT-074 — Private routes](../react/04-routing.md#react-074)

**Source:** [SudheerJ SDJ-236](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-238

### What is a wrapper component?

A wrapper component (also called a layout component or container component) wraps other components to provide shared structure, styling, or context — without dictating the inner content.

```jsx
// Card wrapper — provides consistent styling and layout
function Card({ children, title, className = '' }) {
  return (
    <div className={`card ${className}`}>
      {title && <div className="card-header"><h3>{title}</h3></div>}
      <div className="card-body">{children}</div>
    </div>
  );
}

// Page layout wrapper
function PageLayout({ sidebar, children }) {
  return (
    <div className="page">
      <aside>{sidebar}</aside>
      <main>{children}</main>
    </div>
  );
}

// Usage
<Card title="User Info">
  <UserForm />
</Card>
```

Wrapper components use the `children` prop (or named slot props like `sidebar`) to accept any content. This is the **composition pattern** — preferred over inheritance for sharing structure in React.

**Related:** [REACT-148 — Children prop](../react/01-fundamentals.md#react-148) | [REACT-062 — Composition pattern](./03-advanced.md#react-062)

**Source:** [SudheerJ SDJ-242](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-239

### Why does Strict Mode render twice in development?

React Strict Mode intentionally double-invokes certain functions in development to help detect side effects:

- Component `render` functions / function component bodies
- `useState`, `useMemo`, `useReducer` initializer functions
- Functions passed to `setState`

The double invocation surfaces unintentional side effects — if your render function has a side effect (mutating a ref, writing to localStorage, etc.) you'll see it fire twice in dev and notice the bug.

```jsx
function Counter() {
  console.log('render'); // logs twice in Strict Mode dev
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

This only happens in **development** — production builds never double-invoke. React also discards the second render's output; the UI renders normally.

React 18 extended this to also unmount and remount components once after initial mount (to verify effects are written correctly for concurrent mode).

**Related:** [REACT-041 — Strict Mode](./03-advanced.md#react-041)

**Source:** [SudheerJ SDJ-247](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-240

### What is the windowing (virtualization) technique?

Windowing (also called virtual scrolling or list virtualization) is a technique that renders only the visible portion of a large list rather than all items. For a list of 10,000 rows, only the ~20 visible rows are in the DOM at any time.

```jsx
import { FixedSizeList } from 'react-window';

function Row({ index, style }) {
  return <div style={style}>Row #{index}</div>;
}

function VirtualList() {
  return (
    <FixedSizeList
      height={400}          // visible window height
      itemCount={10000}     // total items
      itemSize={35}         // each row height in px
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

**Libraries:** `react-window` (lightweight), `react-virtual` (TanStack Virtual, flexible), `react-virtualized` (feature-rich, heavier).

**When to use:** Lists/tables with hundreds or thousands of items that cause performance issues. For a list of 50 items, virtualization adds complexity without meaningful benefit.

**Related:** [REACT-056 — Expensive computations](./03-advanced.md#react-056)

**Source:** [SudheerJ SDJ-207](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-241

### Can you use web components in React?

Yes. React renders web components (custom elements) in JSX like any HTML tag. There are some friction points:

```jsx
// Using a web component in React
function App() {
  return (
    <div>
      <my-button onClick={handleClick}>Click me</my-button>
    </div>
  );
}
```

**Friction points:**
- **Events:** Web component events don't bubble through React's synthetic event system. Add event listeners via `useRef`:

```jsx
const ref = useRef();
useEffect(() => {
  ref.current.addEventListener('my-custom-event', handler);
  return () => ref.current.removeEventListener('my-custom-event', handler);
}, []);
<my-component ref={ref} />
```

- **Props:** Web components receive attributes (strings) not React props. Complex data must be set imperatively via the ref.
- **React 19** significantly improves web component support — props are passed as properties, events are handled more naturally.

**Related:** [REACT-001 — What is React](../react/01-fundamentals.md#react-001)

**Source:** [SudheerJ SDJ-196](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-242

### What are loadable components?

`@loadable/component` is a code-splitting library (an alternative to `React.lazy`) that supports server-side rendering — which `React.lazy` does not natively support:

```jsx
import loadable from '@loadable/component';

// With @loadable/component — works with SSR
const HeavyChart = loadable(() => import('./HeavyChart'), {
  fallback: <Spinner />,
});

// Named exports work too (unlike React.lazy)
const Chart = loadable(() => import('./charts'), {
  resolveComponent: (components) => components.BarChart,
});
```

**vs React.lazy:**
| Feature | React.lazy | @loadable/component |
|---|---|---|
| SSR support | ❌ (client only) | ✅ |
| Named exports | ❌ | ✅ |
| Prefetching | ❌ | ✅ |
| Built-in | ✅ | Requires install |

For Next.js, use `next/dynamic` which has its own SSR-compatible lazy loading.

**Related:** [REACT-042 — Code splitting](./03-advanced.md#react-042) | [REACT-150 — React.lazy named exports](../react/01-fundamentals.md#react-150)

**Source:** [SudheerJ SDJ-198](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-243

### What is the purpose of the default context value?

The default value passed to `createContext(defaultValue)` is used when a component consumes the context but is **not** wrapped in a matching Provider:

```jsx
// Default value used when no Provider is in the tree
const ThemeContext = createContext({ mode: 'light', primary: '#6200ea' });

// This component works without a Provider — uses the default
function ThemedButton() {
  const { mode } = useContext(ThemeContext);
  return <button className={mode}>Click</button>;
}
```

**When it matters:**
1. **Testing in isolation** — render a component without its Provider in unit tests
2. **Component library distribution** — ship components that work standalone without requiring consumers to set up Providers
3. **Documentation examples** — code samples in docs don't need Provider boilerplate

If the default is `null` or `undefined` and consumers try to destructure it, you'll get a runtime error. Provide a valid empty/default shape matching your context structure.

**Related:** [REACT-033 — useContext](../react/02-hooks.md#react-033) | [REACT-208 — No matching provider](../react/02-hooks.md#react-208)

**Source:** [SudheerJ SDJ-201](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-244

### What are the rules covered by the diffing algorithm?

React's reconciliation uses two heuristics to make diffing O(n) rather than the O(n³) of a generic tree diff:

**Rule 1 — Different type, full rebuild.** If the root element changes type (e.g., `<div>` → `<span>`, or `<ComponentA>` → `<ComponentB>`), React tears down the entire subtree and mounts a new one:

```jsx
// Old: <div><Counter /></div>
// New: <span><Counter /></span>
// React unmounts Counter, mounts a fresh Counter — state is lost
```

**Rule 2 — Same type, update in place.** If the type is the same, React updates the existing DOM node's attributes and recurses into children:

```jsx
// Old: <div className="a" />
// New: <div className="b" />
// React updates just className — no unmount
```

**Rule 3 — Keys for lists.** Without keys, React diffs children positionally. With keys, React matches children by key across reorders — enabling efficient reordering without destroying state.

**Related:** [REACT-012 — Reconciliation](../react/01-fundamentals.md#react-012) | [REACT-006 — key prop](../react/01-fundamentals.md#react-006)

**Source:** [SudheerJ SDJ-203](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-245

### Does the render prop have to be named "render"?

No — the render prop pattern refers to any prop that accepts a function to be called during render. The prop can have any name:

```jsx
// Classic "render" prop name
<DataFetcher render={(data) => <Table data={data} />} />

// "children" as a function (most common pattern)
<DataFetcher>
  {(data) => <Table data={data} />}
</DataFetcher>

// Any custom name works
<Slider formatLabel={(value) => `${value}%`} />
<List renderItem={(item) => <Row item={item} />} />
<Modal trigger={(open) => <Button onClick={open}>Open</Button>} />
```

The `children` as function pattern is particularly clean because you don't need a custom prop name — the content between the tags is naturally the children. The key characteristic of render props is that **a function is passed as a prop and called within the component's render**, regardless of what that prop is named.

**Related:** [REACT-061 — Render props](./03-advanced.md#react-061) | [REACT-148 — Children prop](../react/01-fundamentals.md#react-148)

**Source:** [SudheerJ SDJ-205](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-246

### What are the problems of using render props with Pure Components?

When a render prop is defined as an inline function in JSX, it creates a new function reference on every parent render. A `PureComponent` or `React.memo` child that receives this prop will always see a "changed" prop and re-render — defeating the optimization:

```jsx
// ✗ New function reference every render — PureComponent re-renders always
class Parent extends Component {
  render() {
    return (
      <PureChildComponent
        render={() => <ExpensiveView />}  // New arrow function each render
      />
    );
  }
}

// ✓ Define the render prop as a class method — stable reference
class Parent extends Component {
  renderContent = () => <ExpensiveView />;
  render() {
    return <PureChildComponent render={this.renderContent} />;
  }
}

// ✓ Modern equivalent — useCallback for stable reference
function Parent() {
  const renderContent = useCallback(() => <ExpensiveView />, []);
  return <MemoizedChild render={renderContent} />;
}
```

**Related:** [REACT-061 — Render props](./03-advanced.md#react-061) | [REACT-016 — React.memo](../react/01-fundamentals.md#react-016) | [REACT-029 — useCallback](../react/02-hooks.md#react-029)

**Source:** [SudheerJ SDJ-206](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-247

### How to prevent automatic batching in React 18?

React 18 introduced automatic batching — all state updates in any context (event handlers, `setTimeout`, Promises) are batched into a single re-render. In rare cases you may want to force a synchronous flush:

```jsx
import { flushSync } from 'react-dom';

function handleClick() {
  // ✓ Forces an immediate synchronous re-render after each setState
  flushSync(() => {
    setCount(c => c + 1);
  });
  // DOM is updated here, before the next line
  flushSync(() => {
    setFlag(f => !f);
  });
}
```

`flushSync` is rare. It's useful when:
- You need to read the DOM immediately after a state update (e.g., scroll position after adding an item)
- Third-party code or testing utilities require synchronous updates

**Related:** [REACT-049 — setState mechanics](./03-advanced.md#react-049)

**Source:** [SudheerJ SDJ-254](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-248

### How to update objects inside state?

Always spread the existing state and override only the changed fields — don't mutate the original object:

```jsx
const [user, setUser] = useState({ name: 'Akar', role: 'engineer', age: 28 });

// ✓ Spread and override the changed field
setUser((prev) => ({ ...prev, role: 'senior engineer' }));

// ✗ Never mutate state directly
user.role = 'senior'; // mutation — React won't detect the change, no re-render
setUser(user);        // same reference — React bails out
```

For nested objects:

```jsx
const [config, setConfig] = useState({ server: { host: 'localhost', port: 3000 } });

// Spread at every level
setConfig((prev) => ({
  ...prev,
  server: { ...prev.server, port: 8080 },
}));
```

For deeply nested state, consider using `immer` (via RTK's `createSlice` or directly) which lets you write mutating syntax safely:

```jsx
import { produce } from 'immer';
setConfig(produce(draft => { draft.server.port = 8080; }));
```

**Related:** [REACT-022 — Immutable state](../react/01-fundamentals.md#react-022) | [REACT-249 — Update arrays in state](#react-249)

**Source:** [SudheerJ SDJ-256, SDJ-257](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-249

### How to update arrays inside state?

Arrays in state must be replaced with new arrays — never mutate the original. Use non-mutating array operations:

```jsx
const [items, setItems] = useState([1, 2, 3]);

// Add
setItems((prev) => [...prev, 4]);

// Remove
setItems((prev) => prev.filter((item) => item !== 2));

// Update specific item
setItems((prev) => prev.map((item) => item === 2 ? 99 : item));

// Insert at position
setItems((prev) => [...prev.slice(0, 2), 10, ...prev.slice(2)]);

// Reorder / sort
setItems((prev) => [...prev].sort((a, b) => a - b)); // copy first, then sort
```

**Avoid these mutating methods in state updates:**
`push`, `pop`, `shift`, `unshift`, `splice`, `reverse`, `sort` (without copying first).

**Related:** [REACT-248 — Update objects in state](#react-248) | [REACT-022 — Immutable state](../react/01-fundamentals.md#react-022)

**Source:** [SudheerJ SDJ-258, SDJ-261](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-250

### What are the benefits of Immer for state updates?

Immer lets you write state mutations in a natural "mutable" style while producing a new immutable state under the hood:

```jsx
import { produce } from 'immer';

const [state, setState] = useState({ user: { name: 'Akar', scores: [90, 85] } });

// Without Immer — verbose spreads for nested state
setState((prev) => ({
  ...prev,
  user: {
    ...prev.user,
    scores: [...prev.user.scores, 95],
  },
}));

// With Immer — write as if mutating directly
setState(produce((draft) => {
  draft.user.scores.push(95);  // Immer converts this to an immutable update
}));
```

Immer uses ES6 Proxies to intercept mutations on the `draft` object and produce a new immutable result. Redux Toolkit's `createSlice` uses Immer by default — that's why you can write `state.count++` in RTK reducers.

**Related:** [REACT-248 — Update objects](#react-248) | [REACT-249 — Update arrays](#react-249) | [REACT-112 — Redux principles](../react/08-redux.md#react-112)

**Source:** [SudheerJ SDJ-259](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-251

### What happens when you define nested function components?

Defining a component inside another component is an anti-pattern. On every render of the outer component, the inner component is redefined — giving it a new identity. React sees a different component type and remounts it from scratch, resetting all its state and running all effects again.

```jsx
// ✗ Anti-pattern — InnerForm is redefined on every AppForm render
function AppForm() {
  // This is a new component type every render
  function InnerForm({ label }) {
    const [value, setValue] = useState('');
    return <input value={value} onChange={e => setValue(e.target.value)} />;
  }
  return <InnerForm label="Name" />;
}
// Problem: every time AppForm re-renders, InnerForm's state is lost

// ✓ Define at module level
function InnerForm({ label }) {
  const [value, setValue] = useState('');
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

function AppForm() {
  return <InnerForm label="Name" />;
}
```

The fix: always define components at the module's top level. If the inner component needs access to outer variables, pass them as props.

**Related:** [REACT-035 — Re-rendering](./03-advanced.md#react-035) | [REACT-047 — Anti-patterns](./03-advanced.md#react-047)

**Source:** [SudheerJ SDJ-262](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-252

### What are capture phase events in React?

DOM events go through three phases: **capture** (root → target), **target**, **bubble** (target → root). React's standard event handlers fire in the bubble phase. Appending `Capture` to any React event name fires in the capture phase:

```jsx
function EventDemo() {
  return (
    <div
      onClickCapture={() => console.log('div capture')} // fires first
      onClick={() => console.log('div bubble')}          // fires last
    >
      <button
        onClickCapture={() => console.log('button capture')}
        onClick={() => console.log('button click')}
      >
        Click me
      </button>
    </div>
  );
}
// Click order: div capture → button capture → button click → div bubble
```

All React events have capture variants: `onFocusCapture`, `onMouseDownCapture`, `onKeyDownCapture`, etc.

**Use cases:** Implementing a global click-outside detector, intercepting events before they reach child handlers, implementing modal/overlay dismiss behavior.

**Related:** [REACT-052 — Synthetic events](./03-advanced.md#react-052)

**Source:** [SudheerJ SDJ-251](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-253

### What is the popular choice for form handling in React?

Two libraries dominate modern React form handling:

**React Hook Form (most popular):**

```jsx
import { useForm } from 'react-hook-form';

function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('email', { required: true })} />
      {errors.email && <span>Email is required</span>}
      <button type="submit">Login</button>
    </form>
  );
}
```

Uncontrolled inputs — minimal re-renders, tiny bundle (~9KB).

**Formik:**

```jsx
import { Formik, Field, Form } from 'formik';

<Formik initialValues={{ email: '' }} onSubmit={values => console.log(values)}>
  <Form>
    <Field name="email" type="email" />
    <button type="submit">Login</button>
  </Form>
</Formik>
```

Controlled inputs — more re-renders, larger bundle, more explicit API.

**Comparison:** React Hook Form is generally preferred for its performance (uncontrolled inputs), smaller bundle, and better TypeScript support. Formik is more explicit and easier to reason about for complex nested forms.

**Related:** [REACT-131 — Redux Form](../react/08-redux.md#react-131) | [REACT-014 — Controlled components](../react/01-fundamentals.md#react-014)

**Source:** [SudheerJ SDJ-193, SDJ-223](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-254

### Why is inheritance not used in React?

React's component model is built on composition, not inheritance. The React team explicitly recommends against inheriting from custom base components:

```jsx
// ✗ Inheritance — React discourages this
class FancyButton extends Button {
  render() { ... }
}

// ✓ Composition — wrap, don't extend
function FancyButton({ children, ...props }) {
  return <Button {...props} className="fancy">{children}</Button>;
}
```

**Why:**
- Inheritance creates tight coupling between parent and child component implementations
- Multiple inheritance is not possible in JavaScript
- Props and composition already give you all the flexibility inheritance provides
- The `children` prop and render props allow full customization without inheritance

The React team's guidance: "If you want to reuse non-UI functionality between components, extract it into a separate JavaScript module. Components can import it and use that function, object, or class, without extending it."

**Related:** [REACT-062 — Composition](./03-advanced.md#react-062) | [REACT-148 — Children prop](../react/01-fundamentals.md#react-148)

**Source:** [SudheerJ SDJ-195](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-255

### Do I need to rewrite all class components as hooks/function components?

No. React has no plans to remove class components. The React team has stated that class components will continue to be supported.

**When to convert:**
- When actively working in the file and the class component needs significant changes
- When you want to adopt concurrent features (some features like `useTransition` are hooks-only)
- When adding new functionality that would be significantly cleaner as hooks

**When NOT to convert:**
- Stable components that work correctly and don't need new features
- Large, complex class components where conversion risk outweighs benefit
- When the team doesn't have time for the testing overhead

```jsx
// Both are valid in modern React
class StableWidget extends React.Component { /* works, no need to change */ }
function NewFeature() { return <div>...</div>; } // new code uses hooks
```

The best approach: write all **new** components as function components with hooks. Convert existing class components opportunistically when you're already editing them.

**Related:** [REACT-009 — Class vs functional](../react/01-fundamentals.md#react-009) | [REACT-023 — Benefits of hooks](../react/01-fundamentals.md#react-023)

**Source:** [SudheerJ SDJ-216, SDJ-218](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)


---

## REACT-284

### How do you debug React applications?

React debugging combines browser DevTools, React-specific tooling, and code-level techniques to locate and fix issues in components, state, effects, and rendering.

**1. React Developer Tools (browser extension)**
- Inspect the component tree, view props/state at any node.
- Use the Profiler tab to record renders and spot expensive re-renders.
- "Highlight updates when components render" shows which components are re-rendering.

**2. Console logging with useEffect**
```jsx
function MyComponent({ data }) {
  useEffect(() => {
    console.log('[MyComponent] data changed:', data);
  }, [data]);

  // Log during render (careful — runs on every render)
  console.log('[MyComponent] render, state:', internalState);
}
```

**3. React Error Boundaries**
Wrap sections of the tree to catch render-time errors and display fallback UI with error details.

**4. Strict Mode double-invoke**
Wrap the app in `<React.StrictMode>` to surface effects that run unexpectedly or state mutations — React intentionally double-invokes renders in development.

**5. Breakpoints in browser DevTools**
Set breakpoints in component functions or event handlers directly from the Sources tab. Source maps make this readable when using Create React App / Vite.

**6. Why Did You Render library**
```bash
npm install @welldone-software/why-did-you-render
```
Patches React to log when a component re-renders with identical props — pinpoints unnecessary renders.

**7. Network tab**
Inspect API calls made from `useEffect` or event handlers — check payloads, status codes, timing.

**Related:** [REACT-035 — What does re-rendering mean](./03-advanced.md#react-035) | [REACT-037 — Error Boundaries](./03-advanced.md#react-037) | [REACT-143 — React DevTools](./10-libraries.md#react-143)

**Source:** [GreatFrontEnd top-reactjs-interview-questions](../../sources/react/github/greatfrontend-top-reactjs-interview-questions/question-map.md)
