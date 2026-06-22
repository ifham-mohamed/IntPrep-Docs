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
