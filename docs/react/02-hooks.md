# React Hooks

> **Canonical Knowledge Base** | Category: React Hooks | IDs: REACT-024 – REACT-034
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-024 — What are the rules of React hooks?](#react-024)
- [REACT-025 — What is the difference between `useEffect` and `useLayoutEffect`?](#react-025)
- [REACT-026 — What does the dependency array of `useEffect` affect?](#react-026)
- [REACT-027 — What is the `useRef` hook and when should it be used?](#react-027)
- [REACT-028 — What is the purpose of the callback format of `setState()` in class components?](#react-028)
- [REACT-029 — What is `useCallback` and when should it be used?](#react-029)
- [REACT-030 — What is `useMemo` and when should it be used?](#react-030)
- [REACT-031 — What is `useReducer` and when should it be used?](#react-031)
- [REACT-032 — What is `useId` and when should it be used?](#react-032)
- [REACT-033 — What is `useContext` and how does it relate to the Context API?](#react-033)
- [REACT-034 — How do you create and use custom hooks?](#react-034)

---

## REACT-024

### What are the rules of React hooks?

React enforces two core rules for hooks (linted by `eslint-plugin-react-hooks`):

**Rule 1 — Only call hooks at the top level**
Never call hooks inside loops, conditions, or nested functions. React relies on the call order being the same on every render to correctly associate hook state with the right variable.

```jsx
// BAD
function MyComponent({ isLoggedIn }) {
  if (isLoggedIn) {
    const [user, setUser] = useState(null); // breaks hook order
  }
}

// GOOD — condition goes inside the hook body or the component logic
function MyComponent({ isLoggedIn }) {
  const [user, setUser] = useState(null);
  // use isLoggedIn to decide what to do with `user`
}
```

**Rule 2 — Only call hooks from React functions**
Call hooks only from:
- React function components
- Custom hooks (functions whose names start with `use`)

Never call hooks from plain JS utility functions, class components, or event handlers outside components.

**Why these rules exist:** React tracks hook state by call index per component. If the number or order of hook calls varies between renders, React will pair state with the wrong hook.

**Related:** [REACT-023 Benefits of Hooks](#react-023) | [REACT-034 Custom Hooks](#react-034)

**Source:** GreatFrontEnd Q25

---

## REACT-025

### What is the difference between `useEffect` and `useLayoutEffect`?

Both run after render, but at different points in the browser's paint cycle:

| | `useEffect` | `useLayoutEffect` |
|---|---|---|
| **When it runs** | After the browser has painted | After DOM mutations, before the browser paints |
| **Blocking?** | Non-blocking (async) | Blocks paint (sync) |
| **Use case** | Data fetching, subscriptions, logging | DOM measurements, preventing visual flicker |
| **SSR** | Works (effect skipped on server) | Logs a warning on server (DOM unavailable) |

```
Render → React updates DOM → [useLayoutEffect fires] → Browser paints → [useEffect fires]
```

**`useLayoutEffect` example — avoid flicker when reading layout:**
```jsx
function Tooltip({ targetRef, content }) {
  const tooltipRef = useRef(null);
  const [pos, setPos] = useState({ top: 0, left: 0 });

  useLayoutEffect(() => {
    // Must read DOM before browser paints, or user sees tooltip in wrong position
    const rect = targetRef.current.getBoundingClientRect();
    setPos({ top: rect.bottom, left: rect.left });
  }, []);

  return <div ref={tooltipRef} style={pos}>{content}</div>;
}
```

**Rule of thumb:** Start with `useEffect`. Switch to `useLayoutEffect` only when you see a visual flicker caused by measuring or adjusting the DOM.

**Related:** [REACT-026 Dependency array](#react-026)

**Source:** GreatFrontEnd Q26

---

## REACT-026

### What does the dependency array of `useEffect` affect?

The dependency array is the second argument to `useEffect`. It controls **when the effect re-runs**.

| Dependency array | Effect runs |
|---|---|
| Omitted | After every render |
| `[]` (empty array) | Once after the initial render only |
| `[a, b]` | After initial render + whenever `a` or `b` changes |

```jsx
// Runs after every render
useEffect(() => { document.title = 'App'; });

// Runs once on mount, cleanup on unmount
useEffect(() => {
  const id = setInterval(tick, 1000);
  return () => clearInterval(id); // cleanup
}, []);

// Runs when `userId` changes
useEffect(() => {
  fetchUser(userId).then(setUser);
}, [userId]);
```

**Cleanup function:** The function returned from `useEffect` runs before the next effect execution and on unmount — used to cancel subscriptions, clear timers, abort fetch requests.

**Common pitfalls:**
- Missing a dependency → stale closure (effect reads outdated variable values).
- Including an unstable object/function reference → infinite loop. Stabilize with `useMemo` / `useCallback`.
- Avoid `async` directly in the effect callback — wrap it in an inner async function instead.

```jsx
// Correct async pattern
useEffect(() => {
  let cancelled = false;
  async function load() {
    const data = await fetchData(id);
    if (!cancelled) setData(data);
  }
  load();
  return () => { cancelled = true; };
}, [id]);
```

**Related:** [REACT-025 useEffect vs useLayoutEffect](#react-025) | [REACT-029 useCallback](#react-029)

**Source:** GreatFrontEnd Q27

---

## REACT-027

### What is the `useRef` hook and when should it be used?

`useRef` returns a mutable object `{ current: initialValue }` that **persists across renders** without causing re-renders when changed.

**Two main use cases:**

**1. Accessing DOM nodes directly**
```jsx
function FocusInput() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus(); // direct DOM access
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus</button>
    </>
  );
}
```

**2. Storing mutable values that shouldn't trigger re-renders**
```jsx
function Timer() {
  const intervalRef = useRef(null);

  function start() {
    intervalRef.current = setInterval(tick, 1000);
  }

  function stop() {
    clearInterval(intervalRef.current); // no re-render needed
  }

  return <><button onClick={start}>Start</button><button onClick={stop}>Stop</button></>;
}
```

**Other use cases:**
- Storing the previous value of a prop or state.
- Holding a flag (e.g., `isMounted`) to guard async state updates.
- Caching expensive values that don't affect render output.

**`useRef` vs `useState`:** Both persist across renders, but `useState` triggers re-render on change; `useRef` does not.

**Related:** [REACT-014 Uncontrolled Components](#react-014) | [REACT-036 forwardRef](#react-036)

**Source:** GreatFrontEnd Q28

---

## REACT-028

### What is the purpose of the callback format of `setState()` in class components and when should it be used?

In class components, `setState` can accept either an object or a **callback function** `(prevState, prevProps) => newState`. The callback form should be used when the new state depends on the previous state.

**Why:** `setState` is asynchronous and may be batched. If you call `setState` multiple times in quick succession using the object form, all calls may read the same stale `this.state` value.

```jsx
// BAD — may use stale state if called multiple times in one event
this.setState({ count: this.state.count + 1 });
this.setState({ count: this.state.count + 1 }); // both read same value!

// GOOD — each call receives the most up-to-date state
this.setState(prev => ({ count: prev.count + 1 }));
this.setState(prev => ({ count: prev.count + 1 })); // correctly increments twice
```

**Equivalent in function components:**
The `useState` setter also accepts a callback for the same reason:
```jsx
setCount(prev => prev + 1); // safe when called in loops or async contexts
```

**Related:** [REACT-022 Immutable State](#react-022) | [REACT-031 useReducer](#react-031)

**Source:** GreatFrontEnd Q29

---

## REACT-029

### What is `useCallback` and when should it be used?

`useCallback(fn, deps)` returns a **memoized version of a callback function** that only changes when one of its dependencies changes. It is the function-equivalent of `useMemo`.

**Why it matters:** In React, functions are recreated on every render. If you pass a callback as a prop to a memoized child (`React.memo`), a new function reference on every render will break memoization and cause unnecessary re-renders.

```jsx
// Without useCallback — new `handleClick` reference every render → MyButton always re-renders
function Parent() {
  const [count, setCount] = useState(0);
  const handleClick = () => setCount(c => c + 1); // new ref each render
  return <MyButton onClick={handleClick} />;
}

// With useCallback — stable reference → MyButton skips re-render when count changes
function Parent() {
  const [count, setCount] = useState(0);
  const handleClick = useCallback(() => setCount(c => c + 1), []); // stable
  return <MyButton onClick={handleClick} />;
}

const MyButton = React.memo(function MyButton({ onClick }) {
  return <button onClick={onClick}>Click</button>;
});
```

**When to use:**
- Passing callbacks to deeply memoized children.
- Callbacks listed in `useEffect` dependency arrays that would otherwise cause infinite loops.

**When NOT to use:** Don't apply `useCallback` everywhere by default — it adds overhead. Measure first.

**Related:** [REACT-030 useMemo](#react-030) | [REACT-016 Pure Components](#react-016) | [REACT-026 useEffect deps](#react-026)

**Source:** GreatFrontEnd Q30

---

## REACT-030

### What is `useMemo` and when should it be used?

`useMemo(fn, deps)` returns a **memoized value** — it runs `fn` on the first render and only re-runs it when a dependency changes. It caches the result between renders.

```jsx
function ProductList({ products, filterText }) {
  // Without useMemo: filters on every render even if products/filterText didn't change
  // With useMemo: only recomputes when products or filterText changes
  const filtered = useMemo(
    () => products.filter(p => p.name.includes(filterText)),
    [products, filterText]
  );

  return <ul>{filtered.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

**Use cases:**
- Expensive computations (sorting, filtering large datasets, complex transformations).
- Stabilizing object/array references passed as props to memoized children.
- Avoiding recomputation in `useEffect` dependencies.

**`useMemo` vs `useCallback`:**
```jsx
useMemo(() => computeValue(a, b), [a, b])   // memoizes a VALUE
useCallback(() => doSomething(a), [a])        // memoizes a FUNCTION
// useCallback(fn, deps) ≡ useMemo(() => fn, deps)
```

**When NOT to use:** Don't memoize everything. The comparison overhead and memory cost aren't worth it for fast computations. Use it when profiling reveals a real bottleneck.

**Related:** [REACT-029 useCallback](#react-029) | [REACT-016 Pure Components](#react-016)

**Source:** GreatFrontEnd Q31

---

## REACT-031

### What is `useReducer` and when should it be used?

`useReducer(reducer, initialState)` is an alternative to `useState` for managing more complex state. It follows the Redux pattern: you dispatch an **action**, and a **reducer function** calculates the next state.

```jsx
const initialState = { count: 0, step: 1 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { ...state, count: state.count + state.step };
    case 'decrement': return { ...state, count: state.count - state.step };
    case 'setStep':   return { ...state, step: action.payload };
    case 'reset':     return initialState;
    default: throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```

**Prefer `useReducer` over `useState` when:**
- State has multiple sub-values that change together.
- Next state depends on the previous state in complex ways.
- You have many `setState` calls that need to be dispatched from child components.
- You want the reducer logic to be independently testable.

**Related:** [REACT-022 Immutable State](#react-022) | [REACT-028 setState callback](#react-028)

**Source:** GreatFrontEnd Q32

---

## REACT-032

### What is `useId` and when should it be used?

`useId()` (React 18+) generates a **stable, unique ID** that is consistent between server and client renders — making it safe for SSR/hydration.

**Primary use case: linking form labels to inputs**

```jsx
function FormField({ label, type = 'text' }) {
  const id = useId();
  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} type={type} />
    </div>
  );
}

// Multiple instances each get unique IDs automatically
<FormField label="Email" type="email" />   // id="r:0:"
<FormField label="Password" type="password" /> // id="r:1:"
```

**Why not use `Math.random()` or a counter?**
- `Math.random()` differs between server and client → hydration mismatch.
- A module-level counter resets on the server for every request.
- `useId` is stable, incremental, and scoped to the component tree.

**Important:** `useId` is for accessibility attributes (`id`, `aria-labelledby`, etc.) — **not** for list `key` props. Use database IDs or stable business data for keys.

**Related:** [REACT-006 key prop](#react-006) | [REACT-101 React 19 features](#react-101)

**Source:** GreatFrontEnd Q33

---

## REACT-033

### What is `useContext` and how does it relate to the Context API?

`useContext(MyContext)` is the hook API for consuming a React context. It replaces the older `<MyContext.Consumer>` render-prop pattern.

**Setting up Context:**
```jsx
// 1. Create context
const ThemeContext = createContext('light'); // default value

// 2. Provide it high in the tree
function App() {
  const [theme, setTheme] = useState('dark');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Page />
    </ThemeContext.Provider>
  );
}

// 3. Consume anywhere in the subtree
function ThemedButton() {
  const { theme, setTheme } = useContext(ThemeContext);
  return (
    <button
      className={theme}
      onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}
    >
      Toggle Theme
    </button>
  );
}
```

**Key behavior:** A component using `useContext` re-renders whenever the context value changes — even if the part of the value it uses didn't change. This is a common performance pitfall.

**Optimization strategies:**
- Split large contexts into smaller, more focused contexts.
- Memoize the context value: `useMemo(() => ({ theme, setTheme }), [theme])`.
- Use a state management library (Zustand, Jotai) for frequent fine-grained updates.

**Related:** [REACT-043 Optimizing Context](#react-043) | [REACT-046 Context Pitfalls](#react-046) | [REACT-015 Lifting State Up](#react-015)

**Source:** GFE — referenced throughout

---

## REACT-034

### How do you create and use custom hooks?

A **custom hook** is a JavaScript function whose name starts with `use` that calls other hooks. It lets you extract and reuse stateful logic across multiple components.

**Example — `useWindowSize`:**
```jsx
import { useState, useEffect } from 'react';

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
  }, []);

  return size;
}

// Usage in any component
function ResponsiveLayout() {
  const { width } = useWindowSize();
  return <div>{width > 768 ? <DesktopNav /> : <MobileNav />}</div>;
}
```

**Example — `useFetch`:**
```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(data => { if (!cancelled) { setData(data); setLoading(false); } })
      .catch(err => { if (!cancelled) { setError(err); setLoading(false); } });
    return () => { cancelled = true; };
  }, [url]);

  return { data, loading, error };
}
```

**Rules for custom hooks:**
- Must start with `use` (required for the linter to apply hooks rules).
- Can call any other hooks — built-in or custom.
- State inside a custom hook is **not shared** between components — each call gets its own isolated state.

**Benefits:** Replaces higher-order components and render props for logic reuse, resulting in flatter component trees.

**Related:** [REACT-023 Hook benefits](#react-023) | [REACT-024 Rules of Hooks](#react-024) | [REACT-058 HOCs](#react-058)

**Source:** GreatFrontEnd Q34

---

*← [React Fundamentals](./01-fundamentals.md) | [Master Index](../reference/00-master-index.md) | [Advanced Concepts →](./03-advanced.md)*

---

## REACT-196

### How to ensure hooks follow the rules in your project?

Install and configure `eslint-plugin-react-hooks`:

```bash
npm install --save-dev eslint-plugin-react-hooks
```

```json
// .eslintrc.json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

`rules-of-hooks` catches: hooks called inside conditions, loops, nested functions, event handlers, or non-React functions.

`exhaustive-deps` catches: missing or extra dependencies in `useEffect`, `useCallback`, and `useMemo` arrays.

This plugin is maintained by the React team and ships with Vite's React template and CRA by default. It's considered essential — missing `exhaustive-deps` is one of the most common sources of stale closure bugs.

**Related:** [REACT-024 — Rules of hooks](./02-hooks.md#react-024) | [REACT-177 — Linters](../react/01-fundamentals.md#react-177)

**Source:** [SudheerJ SDJ-170, SDJ-233](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-197

### What is an updater function in useState? When should you use it?

An updater function is a callback passed to `setState` that receives the previous state and returns the next state:

```jsx
// Direct value — uses the state at the time of scheduling
setCount(count + 1);

// Updater function — receives the latest committed state
setCount((prev) => prev + 1);
```

**Why it matters:** React batches state updates. If you call `setCount(count + 1)` three times in a row, all three read the same `count` — you end up incrementing only once. With an updater function, each call receives the result of the previous:

```jsx
// ✗ count increments only once — all reads use the same stale `count`
handleClick() {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1);
}

// ✓ count increments three times
handleClick() {
  setCount((n) => n + 1);
  setCount((n) => n + 1);
  setCount((n) => n + 1);
}
```

Use the updater form whenever the new state depends on the previous state.

**Related:** [REACT-028 — setState callback](./02-hooks.md#react-028) | [REACT-049 — setState mechanics](../react/03-advanced.md#react-049)

**Source:** [SudheerJ SDJ-273](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-198

### Can useState take a function as an initial value?

Yes — passing a function (lazy initializer) delays expensive initial computation to the first render only:

```jsx
// ✗ Runs on every render — JSON.parse is called each time
const [data, setData] = useState(JSON.parse(localStorage.getItem('data')));

// ✓ Lazy initializer — function is called only on the first render
const [data, setData] = useState(() => JSON.parse(localStorage.getItem('data')));
```

The function is called with no arguments and its return value becomes the initial state. React ignores it on subsequent renders.

Use lazy initialization for:
- Reading from `localStorage` or `sessionStorage`
- Complex initial computations (filtering, sorting a large array)
- Creating initial objects that are expensive to instantiate

**Related:** [REACT-199 — useState value types](#react-199) | [REACT-197 — Updater function](#react-197)

**Source:** [SudheerJ SDJ-274](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-199

### What types of values can useState hold?

`useState` can hold any JavaScript value:

```jsx
useState(0)              // number
useState('hello')        // string
useState(true)           // boolean
useState(null)           // null
useState(undefined)      // undefined
useState([1, 2, 3])      // array
useState({ x: 0, y: 0}) // object
useState(() => 42)       // function — careful: this calls the initializer!
```

**Storing a function in state** requires a wrapper because React treats a function passed to `setState` as an updater:

```jsx
// ✗ React calls fn() as an updater — stores its return value, not fn itself
setCallback(fn);

// ✓ Wrap in another function to store fn as a value
setCallback(() => fn);
```

State values should be **serializable** for time-travel debugging (Redux DevTools / React DevTools) and persistence. Avoid storing class instances, DOM nodes, or circular references in state.

**Related:** [REACT-008 — Props vs state](../react/01-fundamentals.md#react-008) | [REACT-198 — Lazy initializer](#react-198)

**Source:** [SudheerJ SDJ-275](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-200

### What happens if you call useState conditionally?

React tracks hooks in order of their calls on each render. Calling a hook conditionally breaks this order and causes React to associate the wrong state with the wrong hook — leading to bugs and the "Rendered more hooks than during the previous render" error.

```jsx
// ✗ Violates Rules of Hooks — React error
function Form({ showExtra }) {
  const [name, setName] = useState('');
  if (showExtra) {
    const [note, setNote] = useState(''); // conditional hook — invalid
  }
}

// ✓ Always call all hooks; conditionally use the values
function Form({ showExtra }) {
  const [name, setName] = useState('');
  const [note, setNote] = useState(''); // always declared

  return (
    <>
      <input value={name} onChange={e => setName(e.target.value)} />
      {showExtra && <input value={note} onChange={e => setNote(e.target.value)} />}
    </>
  );
}
```

**Related:** [REACT-024 — Rules of hooks](./02-hooks.md#react-024)

**Source:** [SudheerJ SDJ-276](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-201

### Is useState synchronous or asynchronous?

`setState` from `useState` is **asynchronous** — the state value is not updated immediately after calling it. React batches state updates and applies them after the current event handler completes.

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    console.log(count); // Still logs old value — state hasn't updated yet
  };
}
```

To read the updated value after a setState, use `useEffect` with the state variable as a dependency:

```jsx
useEffect(() => {
  console.log('Updated count:', count);
}, [count]);
```

Or use the updater function pattern to derive the next value without reading the stale state:

```jsx
setCount((prev) => {
  const next = prev + 1;
  doSomethingWith(next);  // access next value here
  return next;
});
```

**Related:** [REACT-197 — Updater function](#react-197) | [REACT-049 — setState mechanics](../react/03-advanced.md#react-049)

**Source:** [SudheerJ SDJ-277](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-202

### How does useState work internally?

React maintains a linked list of "hooks" for each component fiber. On the first render, `useState` appends a new cell containing the initial value. On re-renders, it reads the existing cell in order.

```
Fiber for MyComponent:
  hooks: [ {state: 0, queue: []},   // useState(0)
            {state: '', queue: []},  // useState('')
            {effect: fn, deps: []} ] // useEffect
```

When you call `setState`:
1. React creates an "update" object and adds it to the hook's queue.
2. React schedules a re-render for the component.
3. During the re-render, React processes the queue, applies the updates, and the hook cell gets the new state value.

This is why:
- Hooks must be called in the same order every render (the fiber reads cells positionally).
- State is not updated synchronously (it goes through the queue).
- Calling `setState` with the same value (by reference) skips re-rendering (`Object.is` comparison).

**Related:** [REACT-024 — Rules of hooks](./02-hooks.md#react-024) | [REACT-011 — Fiber](../react/01-fundamentals.md#react-011)

**Source:** [SudheerJ SDJ-278](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-203

### Can you combine useReducer with useContext?

Yes — this is a common pattern for global state management without Redux. `useReducer` manages state logic; `useContext` provides access anywhere in the tree:

```jsx
// store.jsx
const CounterContext = createContext(null);
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
}

export function CounterProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
}

export const useCounter = () => useContext(CounterContext);

// Usage anywhere in the tree
function Counter() {
  const { state, dispatch } = useCounter();
  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
}
```

This scales well for medium-complexity apps. For fine-grained performance, split the context into a state context and a dispatch context — dispatch never changes, so components that only dispatch don't re-render when state changes.

**Related:** [REACT-031 — useReducer](./02-hooks.md#react-031) | [REACT-033 — useContext](./02-hooks.md#react-033) | [REACT-118 — Context vs Redux](../react/08-redux.md#react-118)

**Source:** [SudheerJ SDJ-281](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-204

### Can you dispatch multiple actions in a row with useReducer?

Yes — multiple dispatches in the same event handler are batched in React 18+ and applied in sequence, with only one re-render:

```jsx
function handleReset(dispatch) {
  dispatch({ type: 'RESET_FORM' });
  dispatch({ type: 'CLEAR_ERRORS' });
  dispatch({ type: 'SET_STATUS', payload: 'idle' });
  // React 18: one re-render applies all three updates
}
```

Each dispatch creates a new update in the queue. The reducer runs once per dispatch in sequence — the output of the first becomes the input for the second:

```
state → RESET_FORM → state1 → CLEAR_ERRORS → state2 → SET_STATUS → state3
```

The component re-renders once with `state3`. In React 17 and below (outside event handlers), each dispatch triggered a separate render — React 18's automatic batching resolved this.

**Related:** [REACT-031 — useReducer](./02-hooks.md#react-031) | [REACT-049 — setState batching](../react/03-advanced.md#react-049)

**Source:** [SudheerJ SDJ-282](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-205

### Is dispatch from useReducer asynchronous? Does it update state immediately?

Like `setState`, dispatch is **asynchronous** — the state is not immediately updated when you call `dispatch`. React queues the update and re-renders the component.

```jsx
const [state, dispatch] = useReducer(reducer, { count: 0 });

const handleClick = () => {
  dispatch({ type: 'increment' });
  console.log(state.count); // Still 0 — state not updated yet
};
```

However, dispatch itself is **stable** (same reference across renders — you don't need to include it in `useCallback` or `useEffect` dependency arrays).

To react to state changes after dispatch, use `useEffect`:

```jsx
useEffect(() => {
  if (state.status === 'submitted') {
    navigate('/success');
  }
}, [state.status]);
```

**Related:** [REACT-031 — useReducer](./02-hooks.md#react-031) | [REACT-201 — useState async](#react-201)

**Source:** [SudheerJ SDJ-283](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-206

### Can you use multiple contexts in one component?

Yes — call `useContext` multiple times with different context objects:

```jsx
const theme = useContext(ThemeContext);
const user = useContext(UserContext);
const locale = useContext(LocaleContext);
```

Each `useContext` call subscribes to one context independently. The component re-renders when any of those context values change.

**Avoiding excess re-renders from multiple contexts:**

Split large context objects into smaller ones so components only subscribe to what they need:

```jsx
// ✗ One big context — all consumers re-render on any change
const AppContext = createContext({ user, theme, cart, notifications });

// ✓ Separate contexts — components subscribe to only what they use
const UserContext = createContext(user);
const ThemeContext = createContext(theme);
const CartContext = createContext(cart);
```

**Related:** [REACT-033 — useContext](./02-hooks.md#react-033) | [REACT-043 — Context performance](../react/03-advanced.md#react-043)

**Source:** [SudheerJ SDJ-285](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-207

### What is a common pitfall when using useContext with objects?

Passing an object literal directly to `value` creates a new object reference on every render, causing all consumers to re-render even if the data didn't change:

```jsx
// ✗ New object on every render — all consumers re-render unnecessarily
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}

// ✓ Memoize the value object
function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const value = useMemo(() => ({ user, setUser }), [user]);
  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}
```

The fix is `useMemo` on the value so the object reference only changes when `user` actually changes. An even better pattern: split into separate `UserContext` (value, changes often) and `DispatchContext` (`setUser`, never changes).

**Related:** [REACT-033 — useContext](./02-hooks.md#react-033) | [REACT-043 — Context performance](../react/03-advanced.md#react-043) | [REACT-206 — Multiple contexts](#react-206)

**Source:** [SudheerJ SDJ-286](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-208

### What is the context value when there is no matching Provider?

When a component calls `useContext(MyContext)` but there is no `<MyContext.Provider>` anywhere above it in the tree, it receives the **default value** passed to `createContext`:

```jsx
const ThemeContext = createContext('light'); // 'light' is the default

function App() {
  // No Provider — all consumers get 'light'
  return <ThemedButton />;
}

function ThemedButton() {
  const theme = useContext(ThemeContext); // 'light'
  return <button className={theme}>Click</button>;
}
```

The default value is useful for:
- Components that can function without a Provider (e.g., testing in isolation)
- Providing sensible defaults so the app doesn't crash if a Provider is accidentally omitted

If the default is `undefined` or `null` and a consumer tries to destructure it (`const { user } = useContext(AuthContext)`), you'll get a runtime error — consider making the default a valid "empty" object matching your context shape.

**Related:** [REACT-033 — useContext](./02-hooks.md#react-033)

**Source:** [SudheerJ SDJ-287](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-209

### What happens if you return a Promise from useEffect?

React ignores it — and it can cause subtle bugs. `useEffect` expects either nothing or a **cleanup function** (a plain function, not a Promise) as its return value.

```jsx
// ✗ Returning a Promise — React ignores it, no cleanup runs
useEffect(async () => {
  const data = await fetchData();
  setData(data);
}, []);

// ✓ Define an async function inside and call it
useEffect(() => {
  let cancelled = false;

  async function load() {
    const data = await fetchData();
    if (!cancelled) setData(data);
  }
  load();

  return () => { cancelled = true; }; // cleanup function — not a Promise
}, []);
```

The reason: React needs a synchronous cleanup function to cancel subscriptions, abort fetches, or clear timers when the component unmounts or dependencies change. An `async` function always returns a Promise — it can never return a cleanup function synchronously.

**Related:** [REACT-026 — useEffect dependency](./02-hooks.md#react-026) | [REACT-065 — Race conditions](../react/03-advanced.md#react-065)

**Source:** [SudheerJ SDJ-290](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-210

### Can you have multiple useEffect hooks in a single component?

Yes — and it's recommended. Split unrelated side effects into separate `useEffect` calls rather than combining everything into one:

```jsx
function UserProfile({ userId }) {
  // Effect 1: fetch user data
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);

  // Effect 2: update page title
  useEffect(() => {
    document.title = `Profile — ${userId}`;
    return () => { document.title = 'App'; };
  }, [userId]);

  // Effect 3: analytics
  useEffect(() => {
    analytics.track('profile_viewed', { userId });
  }, [userId]);
}
```

Benefits of splitting:
- Each effect is independently readable and testable
- React runs cleanup and re-execution per effect based on its own dependencies — no spurious re-runs
- Easier to remove one behavior without touching the others

**Related:** [REACT-026 — useEffect dependency](./02-hooks.md#react-026)

**Source:** [SudheerJ SDJ-290 (duplicate)](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-211

### How to prevent infinite loops in useEffect?

Infinite loops in `useEffect` are caused by the effect updating something that's in its dependency array, triggering another run, which updates again, and so on.

**Common causes and fixes:**

```jsx
// ✗ Infinite loop — data is set on every run, [] is wrong dep
useEffect(() => {
  setData(processData(rawData)); // state update triggers re-render → re-runs effect
}, [rawData, data]); // data is both produced and consumed

// ✓ Fix — compute derived value outside effect or use useMemo
const processedData = useMemo(() => processData(rawData), [rawData]);

// ✗ Object dependency — new object reference on every render
useEffect(() => { fetch(config.url); }, [config]); // config is {}

// ✓ Fix — destructure primitive values
useEffect(() => { fetch(url); }, [url]);

// ✗ Function dependency without useCallback
useEffect(() => { fetchData(); }, [fetchData]); // fetchData recreated each render

// ✓ Fix — memoize the function
const fetchData = useCallback(() => { ... }, [id]);
```

The `eslint-plugin-react-hooks` `exhaustive-deps` rule helps catch these patterns before they cause bugs.

**Related:** [REACT-026 — useEffect dependency](./02-hooks.md#react-026) | [REACT-029 — useCallback](./02-hooks.md#react-029)

**Source:** [SudheerJ SDJ-291](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-212

### How does useLayoutEffect behave during server-side rendering?

`useLayoutEffect` does not run on the server. React logs a warning:

> Warning: useLayoutEffect does nothing on the server because its effect cannot be encoded into the server renderer's output format.

This is because `useLayoutEffect` runs synchronously after DOM mutations — there is no DOM on the server.

**Solutions:**

```jsx
// Option 1: use useEffect instead (runs client-side only, no warning)
// Acceptable when you don't need layout measurement on server render

// Option 2: suppress the warning with a custom hook
function useIsomorphicLayoutEffect(effect, deps) {
  const isServer = typeof window === 'undefined';
  return isServer ? useEffect(effect, deps) : useLayoutEffect(effect, deps);
}

// Option 3: guard with a mounted check
const [mounted, setMounted] = useState(false);
useEffect(() => setMounted(true), []);
useLayoutEffect(() => {
  if (!mounted) return;
  // safe to run on client only
}, [mounted]);
```

**Related:** [REACT-025 — useEffect vs useLayoutEffect](./02-hooks.md#react-025)

**Source:** [SudheerJ SDJ-293](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-213

### What happens if you use useLayoutEffect for non-layout logic?

`useLayoutEffect` blocks the browser from painting until it finishes. Using it for logic that doesn't need to run before paint (network requests, subscriptions, analytics) unnecessarily delays the user from seeing the page.

```jsx
// ✗ Network request in useLayoutEffect — blocks painting for no reason
useLayoutEffect(() => {
  fetch('/api/data').then(setData); // async, doesn't affect layout
}, []);

// ✓ Network requests belong in useEffect
useEffect(() => {
  fetch('/api/data').then(setData);
}, []);
```

Reserve `useLayoutEffect` strictly for:
- Measuring DOM node sizes/positions
- Synchronously updating the DOM before the user sees it (to prevent flash)
- Integrating with non-React DOM libraries that require synchronous DOM access

For everything else, `useEffect` is correct and avoids the blocking penalty.

**Related:** [REACT-025 — useEffect vs useLayoutEffect](./02-hooks.md#react-025)

**Source:** [SudheerJ SDJ-294](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-214

### How does useLayoutEffect cause layout thrashing?

Layout thrashing occurs when you repeatedly read and write DOM geometry in a tight loop — each write invalidates the browser's layout cache and forces a synchronous reflow on the next read.

```jsx
// ✗ Layout thrashing inside useLayoutEffect
useLayoutEffect(() => {
  items.forEach((el) => {
    const height = el.getBoundingClientRect().height; // Forces layout
    el.style.height = `${height + 10}px`;            // Writes, invalidates cache
    // Next getBoundingClientRect() on the loop forces ANOTHER layout
  });
});
```

**Fix — batch reads first, then writes:**

```jsx
useLayoutEffect(() => {
  // 1. All reads (one layout)
  const heights = items.map((el) => el.getBoundingClientRect().height);
  // 2. All writes (one reflow)
  items.forEach((el, i) => { el.style.height = `${heights[i] + 10}px`; });
});
```

Using `requestAnimationFrame` or libraries like `FastDOM` can also help batch DOM reads and writes. In most React code, avoid this pattern entirely — manage layout with CSS and state rather than imperative DOM measurement.

**Related:** [REACT-025 — useEffect vs useLayoutEffect](./02-hooks.md#react-025) | [REACT-213 — useLayoutEffect non-layout](#react-213)

**Source:** [SudheerJ SDJ-295](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-215

### Can useRef store previous values?

Yes — a common pattern is a "previous value" ref:

```jsx
function usePrevious(value) {
  const prevRef = useRef(undefined);

  useEffect(() => {
    prevRef.current = value; // update AFTER render
  });

  return prevRef.current; // returns the value from the previous render
}

function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);

  return (
    <p>
      Now: {count}, Before: {prevCount ?? 'none'}
    </p>
  );
}
```

The trick: `prevRef.current` is read during render (returns old value), then `useEffect` updates it after the render completes — so the next render sees the previous render's value.

**Related:** [REACT-027 — useRef](./02-hooks.md#react-027)

**Source:** [SudheerJ SDJ-298](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-216

### Can you access a ref in the render method?

During the current render, the ref `.current` reflects its state from the previous render — it's not guaranteed to hold the element being rendered right now. React attaches refs after the component mounts/updates.

```jsx
function Example() {
  const divRef = useRef(null);

  // During render: divRef.current is null (first render) or the previous DOM node
  console.log(divRef.current); // null on first render

  useEffect(() => {
    // After render: ref is attached, safe to read
    console.log(divRef.current); // <div> DOM element
  }, []);

  return <div ref={divRef}>Hello</div>;
}
```

Never conditionally render something that includes a ref attachment in a way that depends on the ref itself — the ref isn't populated until after the render cycle.

For class components, `this.refs.name` (string refs) is deprecated. Use `createRef()` or callback refs instead.

**Related:** [REACT-027 — useRef](./02-hooks.md#react-027)

**Source:** [SudheerJ SDJ-299](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-217

### What is useImperativeHandle?

`useImperativeHandle` customizes the value exposed when a parent accesses a child component via a `ref`. Without it, the parent gets the raw DOM node; with it, the parent gets a custom object with only the methods you choose to expose.

```jsx
import { useRef, useImperativeHandle, forwardRef } from 'react';

const FancyInput = forwardRef(function FancyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => ({
    focus() { inputRef.current.focus(); },
    clear() { inputRef.current.value = ''; },
    // Not exposing: blur, select, the full DOM node, etc.
  }));

  return <input ref={inputRef} {...props} />;
});

// Parent
function Form() {
  const inputRef = useRef(null);
  return (
    <>
      <FancyInput ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
      <button onClick={() => inputRef.current.clear()}>Clear</button>
    </>
  );
}
```

Use `useImperativeHandle` sparingly — it's an escape hatch for imperative control. Most UI interactions should be driven by state and props.

**Related:** [REACT-027 — useRef](./02-hooks.md#react-027) | [REACT-036 — forwardRef](../react/03-advanced.md#react-036)

**Source:** [SudheerJ SDJ-301](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-218

### When should you use useImperativeHandle?

Use it when a parent component needs to trigger an action inside a child component imperatively, and that action can't cleanly be expressed as a prop change.

**Good use cases:**
- `focus()` / `blur()` / `scrollIntoView()` on a custom input wrapper
- Resetting a child's internal state from a parent
- Exposing `play()` / `pause()` on a custom media player component
- Triggering animations defined inside the child

```jsx
// Custom animation component that exposes play/pause to parent
const AnimatedBox = forwardRef(function AnimatedBox(_, ref) {
  const [playing, setPlaying] = useState(false);

  useImperativeHandle(ref, () => ({
    play: () => setPlaying(true),
    pause: () => setPlaying(false),
  }));

  return <div className={playing ? 'animate' : ''} />;
});
```

**Bad use cases:** Managing data flow (use props/callbacks instead), rendering decisions (use conditional rendering).

**Related:** [REACT-217 — useImperativeHandle](#react-217)

**Source:** [SudheerJ SDJ-302](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-219

### Is it possible to use useImperativeHandle without forwardRef?

In React 18 and below, `useImperativeHandle` requires `forwardRef` because refs were passed as a separate second argument and couldn't be received as a prop.

In **React 19**, `forwardRef` is no longer needed — `ref` is now a regular prop:

```jsx
// React 19 — ref as a prop, no forwardRef needed
function FancyInput({ ref, ...props }) {
  const inputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
  }));
  return <input ref={inputRef} {...props} />;
}

// Parent
<FancyInput ref={myRef} />
```

In React 18 and below, you must use `forwardRef` — `useImperativeHandle` without it simply won't receive the external ref.

**Related:** [REACT-217 — useImperativeHandle](#react-217) | [REACT-036 — forwardRef](../react/03-advanced.md#react-036)

**Source:** [SudheerJ SDJ-303](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-220

### Does useMemo prevent re-rendering of child components?

Not directly. `useMemo` memoizes a computed value — it doesn't prevent a component from rendering. To prevent a **child component** from re-rendering, use `React.memo` on the child:

```jsx
// useMemo — memoizes a value (doesn't affect child renders)
const sortedList = useMemo(() => [...items].sort(), [items]);

// React.memo — memoizes the component's render output
const SortedList = React.memo(function SortedList({ items }) {
  return <ul>{items.map(i => <li key={i}>{i}</li>)}</ul>;
});
```

However, `useMemo` can **enable** `React.memo` to work effectively: if a parent passes a derived object/array as a prop without memoization, the child gets a new reference every render and `React.memo` can't help. Wrapping the derived value in `useMemo` keeps the reference stable:

```jsx
// Without useMemo: new array reference every render → React.memo bypassed
const sorted = [...items].sort(); // new ref each render

// With useMemo: stable reference → React.memo works
const sorted = useMemo(() => [...items].sort(), [items]);
```

**Related:** [REACT-030 — useMemo](./02-hooks.md#react-030) | [REACT-016 — React.memo](../react/01-fundamentals.md#react-016) | [REACT-035 — Re-renders](../react/03-advanced.md#react-035)

**Source:** [SudheerJ SDJ-305](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-221

### Can hooks be used in class components?

No. Hooks are exclusively for function components. They rely on React's internal fiber state linked to function call ordering — class components use a different instance-based mechanism.

```jsx
// ✗ Hooks in class components — runtime error
class MyComponent extends React.Component {
  render() {
    const [count, setCount] = useState(0); // Invalid — hooks can't be called here
    return <div>{count}</div>;
  }
}
```

**Options for using hooks with class components:**
1. Convert the class component to a function component (recommended)
2. Wrap the class component with a function component "adapter" that uses hooks and passes values as props

```jsx
// Adapter pattern — wraps class with a function component
function ClassComponentAdapter(props) {
  const theme = useContext(ThemeContext);
  return <ClassComponent {...props} theme={theme} />;
}
```

**Related:** [REACT-009 — Class vs functional](../react/01-fundamentals.md#react-009) | [REACT-023 — Benefits of hooks](../react/01-fundamentals.md#react-023)

**Source:** [SudheerJ SDJ-272](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-222

### What is useSyncExternalStore?

`useSyncExternalStore` is a hook for subscribing React components to external stores (browser APIs, non-React state managers, Zustand, Valtio, etc.) in a way that is compatible with concurrent React.

```jsx
import { useSyncExternalStore } from 'react';

// Subscribe to browser's online/offline status
function useOnlineStatus() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener('online', callback);
      window.addEventListener('offline', callback);
      return () => {
        window.removeEventListener('online', callback);
        window.removeEventListener('offline', callback);
      };
    },
    () => navigator.onLine,          // getSnapshot (client)
    () => true,                      // getServerSnapshot (SSR)
  );
}

function StatusBar() {
  const isOnline = useOnlineStatus();
  return <p>{isOnline ? '✅ Online' : '❌ Offline'}</p>;
}
```

**Why it exists:** In concurrent mode, React can render a component multiple times before committing. Using a `useEffect` + `useState` subscription can read stale external state mid-render. `useSyncExternalStore` guarantees the snapshot is consistent.

**Related:** [REACT-054 — Concurrent rendering](../react/03-advanced.md#react-054) | [REACT-034 — Custom hooks](./02-hooks.md#react-034)

**Source:** [SudheerJ SDJ-313](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-223

### What is useInsertionEffect?

`useInsertionEffect` is a low-level hook designed for **CSS-in-JS libraries** to inject `<style>` tags before any DOM mutations. It runs before `useLayoutEffect`, which runs before `useEffect`.

```
Render phase → useInsertionEffect → useLayoutEffect → Browser paint → useEffect
```

```jsx
// Typical usage in a CSS-in-JS library
function useCSS(rule) {
  useInsertionEffect(() => {
    if (!document.getElementById(rule.id)) {
      const style = document.createElement('style');
      style.id = rule.id;
      style.textContent = rule.cssText;
      document.head.appendChild(style);
    }
  });
  return rule.className;
}
```

**Not for application code.** This hook exists so CSS-in-JS libraries (Emotion, Styled Components) can inject styles before `useLayoutEffect` reads layout — avoiding a race condition where a `useLayoutEffect` measures DOM geometry before the injected styles have been applied.

Application developers should use `useEffect` or `useLayoutEffect` — not `useInsertionEffect`.

**Related:** [REACT-025 — useEffect vs useLayoutEffect](./02-hooks.md#react-025)

**Source:** [SudheerJ SDJ-314](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-224

### What is useDebugValue?

`useDebugValue` adds a label to custom hooks that appears in React DevTools when you inspect a component using that hook. It has no effect in production.

```jsx
function useFriendStatus(friendId) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    subscribeToFriendStatus(friendId, setIsOnline);
    return () => unsubscribeFromFriendStatus(friendId);
  }, [friendId]);

  // DevTools shows: "FriendStatus: Online" or "FriendStatus: Offline"
  useDebugValue(isOnline ? 'Online' : 'Offline');

  return isOnline;
}

// With a formatter function — runs lazily (only when DevTools is open)
useDebugValue(date, (d) => d.toDateString());
```

The optional second argument is a formatter function that's only called when DevTools is open. Use it for expensive formatting operations so they don't run in every render.

**Related:** [REACT-034 — Custom hooks](./02-hooks.md#react-034)

**Source:** [SudheerJ SDJ-316](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-225

### What are best practices for using React Hooks?

**1. Follow the Rules of Hooks** — call only at the top level, only in function components or custom hooks. Use `eslint-plugin-react-hooks`.

**2. Keep effects focused** — one concern per `useEffect`. Split unrelated effects rather than combining them.

**3. Clean up subscriptions** — always return a cleanup function from effects that add event listeners, timers, or subscriptions.

**4. Use the updater function for derived state:**

```jsx
setCount((prev) => prev + 1); // not setCount(count + 1)
```

**5. Don't over-memoize** — `useMemo` and `useCallback` have overhead. Only memoize when you've measured a real performance problem.

**6. Custom hooks for shared logic** — extract any hook combination used in multiple components into a custom hook.

**7. Exhaustive deps** — always include all referenced values in `useEffect` / `useCallback` / `useMemo` dependency arrays. Let the ESLint rule guide you.

**8. Initialize state lazily** — pass a function to `useState` for expensive initial computation.

**9. Avoid storing derived state** — compute derived values with `useMemo` rather than `useState` + `useEffect` to keep them in sync.

**10. Prefer specific hooks for specific jobs** — `useRef` for DOM access, `useState` for user-visible state, `useReducer` for complex state machines.

**Related:** [REACT-024 — Rules of hooks](./02-hooks.md#react-024) | [REACT-034 — Custom hooks](./02-hooks.md#react-034)

**Source:** [SudheerJ SDJ-318](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

