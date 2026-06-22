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
