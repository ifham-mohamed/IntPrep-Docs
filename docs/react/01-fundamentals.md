# React Fundamentals

> **Canonical Knowledge Base** | Category: React Fundamentals | IDs: REACT-001 – REACT-023
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-001 — What is React and what are its main features?](#react-001)
- [REACT-002 — What is JSX and how does it work?](#react-002)
- [REACT-003 — What is the Virtual DOM and how does it work?](#react-003)
- [REACT-004 — What is the difference between React Node, React Element, and React Component?](#react-004)
- [REACT-005 — What are React Fragments used for?](#react-005)
- [REACT-006 — What is the purpose of the `key` prop in React?](#react-006)
- [REACT-007 — What is the consequence of using array indices as keys?](#react-007)
- [REACT-008 — What are props in React? How are they different from state?](#react-008)
- [REACT-009 — What is the difference between class components and functional components?](#react-009)
- [REACT-010 — When should you use a class component over a function component?](#react-010)
- [REACT-011 — What is React Fiber?](#react-011)
- [REACT-012 — What is reconciliation in React?](#react-012)
- [REACT-013 — What is the difference between Shadow DOM and Virtual DOM?](#react-013)
- [REACT-014 — What is the difference between Controlled and Uncontrolled components?](#react-014)
- [REACT-015 — How would you lift state up in React, and why?](#react-015)
- [REACT-016 — What are Pure Components?](#react-016)
- [REACT-017 — What is the difference between `createElement` and `cloneElement`?](#react-017)
- [REACT-018 — What is the role of PropTypes in React?](#react-018)
- [REACT-019 — What are stateless components?](#react-019)
- [REACT-020 — What are stateful components?](#react-020)
- [REACT-021 — What are the recommended ways to type-check React component props?](#react-021)
- [REACT-022 — Why does React recommend against mutating state?](#react-022)
- [REACT-023 — What are the benefits of using hooks in React?](#react-023)

---

## REACT-001

### What is React and what are its main features?

React is an open-source JavaScript **library** (not a framework) developed by Facebook/Meta for building user interfaces. It focuses exclusively on the view layer and is typically combined with other libraries for routing, state management, etc.

**Core features:**

- **Component-based architecture** — UIs are built as a tree of reusable, composable components, each managing its own logic and rendering.
- **Declarative UI** — you describe *what* the UI should look like for a given state; React handles the DOM updates.
- **Virtual DOM** — React maintains an in-memory representation of the DOM and performs minimal real-DOM updates via diffing.
- **Unidirectional data flow** — data flows top-down from parent to child via props, making apps easier to reason about.
- **JSX** — a syntax extension that lets you write HTML-like markup directly in JavaScript.
- **Hooks** — functions like `useState` and `useEffect` that let function components use state and lifecycle features.
- **Rich ecosystem** — large community, React DevTools, concurrent features, Server Components, etc.

**Related:** [REACT-002 JSX](#react-002) | [REACT-003 Virtual DOM](#react-003) | [REACT-011 Fiber](#react-011)

**Source:** GreatFrontEnd Q1

---

## REACT-002

### What is JSX and how does it work?

**JSX** (JavaScript XML) is a syntax extension for JavaScript that lets you write markup that looks like HTML directly inside `.js`/`.tsx` files. It is **not** valid JavaScript — a transpiler (typically Babel or the TypeScript compiler) converts it into `React.createElement()` calls before the browser sees it.

**Example:**

```jsx
// JSX
const el = <h1 className="title">Hello</h1>;

// What Babel compiles it to (React 17- classic runtime)
const el = React.createElement('h1', { className: 'title' }, 'Hello');

// React 17+ automatic runtime — no React import needed
// import { jsx as _jsx } from 'react/jsx-runtime';
const el = _jsx('h1', { className: 'title', children: 'Hello' });
```

**Key rules:**
- Must return a single root element (or a Fragment).
- Use `className` instead of `class`, `htmlFor` instead of `for`.
- JavaScript expressions are embedded in `{}`.
- Self-closing tags must be explicitly closed: `<img />`.

**Why it matters:** JSX keeps markup and logic co-located, making components easier to read and refactor than string-based templates.

**Related:** [REACT-001 React](#react-001) | [REACT-004 React Node/Element/Component](#react-004)

**Source:** GreatFrontEnd Q2

---

## REACT-003

### What is the Virtual DOM and how does it work? What are its benefits and downsides?

The **Virtual DOM (VDOM)** is a lightweight, in-memory JavaScript object tree that mirrors the structure of the real DOM. React uses it to batch and minimize expensive real-DOM mutations.

**How it works (reconciliation loop):**

1. When state or props change, React creates a **new VDOM tree**.
2. React **diffs** the new tree against the previous one using its reconciliation algorithm (see [REACT-012](#react-012)).
3. React calculates the minimal set of changes (a "patch").
4. Only those changes are applied to the **real DOM**.

```
State change
    ↓
New VDOM tree created
    ↓
Diff: new VDOM vs old VDOM
    ↓
Patch: apply minimal changes to real DOM
```

**Benefits:**
- Avoids unnecessary real-DOM operations, which are slow.
- Makes UI updates declarative — you don't touch the DOM directly.
- Enables server-side rendering (VDOM can be serialized to HTML).

**Downsides:**
- Additional memory overhead (maintaining two trees).
- Diffing itself has a cost; in highly dynamic UIs it may not always outperform fine-grained DOM updates.
- Introduces an abstraction layer that can obscure what's actually happening.

**Related:** [REACT-012 Reconciliation](#react-012) | [REACT-013 Shadow DOM vs Virtual DOM](#react-013) | [REACT-011 Fiber](#react-011)

**Source:** GreatFrontEnd Q3, Q4

---

## REACT-004

### What is the difference between React Node, React Element, and React Component?

| Term | Description | Example |
|---|---|---|
| **React Node** | Anything React can render: element, string, number, boolean, `null`, array, Fragment | `"Hello"`, `42`, `null`, `<div />` |
| **React Element** | An immutable plain object describing what to render — the output of `React.createElement()` or JSX | `{ type: 'div', props: {}, key: null }` |
| **React Component** | A function (or class) that accepts props and returns React Elements | `function Button(props) { return <button>{props.label}</button>; }` |

**Mental model:**
- A **Component** is the blueprint (the function/class).
- An **Element** is what you get when React calls that blueprint — a description of the UI.
- A **Node** is any value React knows how to render.

```jsx
// Component (blueprint)
function Greeting({ name }) {
  return <p>Hello, {name}</p>; // returns an Element
}

// Element (description)
const el = <Greeting name="Akar" />;
// same as: React.createElement(Greeting, { name: 'Akar' })

// Node (renderable value)
const node = "plain text"; // also a valid React Node
```

**Related:** [REACT-002 JSX](#react-002) | [REACT-019 Stateless Components](#react-019)

**Source:** GreatFrontEnd Q5

---

## REACT-005

### What are React Fragments used for?

**Fragments** let you group multiple children without adding an extra DOM node. This matters because JSX requires a single root element, but wrapping in a `<div>` can break CSS layouts (e.g., flexbox, grid, table rows).

**Two syntax forms:**

```jsx
// Explicit — supports the `key` prop (useful in lists)
import { Fragment } from 'react';
return (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
);

// Shorthand — cleaner, but cannot accept props
return (
  <>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </>
);
```

**Common use cases:**
- Returning multiple siblings from a component without a wrapper `<div>`.
- Rendering table rows (`<tr>` children cannot be wrapped in `<div>`).
- Mapping lists where each item returns multiple elements and needs a `key`.

**Related:** [REACT-006 key prop](#react-006)

**Source:** GreatFrontEnd Q6

---

## REACT-006

### What is the purpose of the `key` prop in React?

The `key` prop is a special string/number attribute that helps React **identify which items in a list have changed, been added, or removed** during reconciliation. It is not passed to the component as a prop.

**Why it matters:**

Without keys, React uses position-based matching. If you insert an item at the start of a list, React may re-render every subsequent element unnecessarily and can even lose component state.

```jsx
// Bad — React can't tell which item moved
{items.map((item, index) => <Item key={index} value={item} />)}

// Good — stable, unique identifier
{items.map((item) => <Item key={item.id} value={item} />)}
```

**Rules:**
- Keys must be **unique among siblings** (not globally unique).
- Keys should be **stable** — don't use `Math.random()`.
- Keys must be **strings or numbers**.

**Related:** [REACT-007 Array index keys](#react-007) | [REACT-005 Fragments](#react-005)

**Source:** GreatFrontEnd Q7

---

## REACT-007

### What is the consequence of using array indices as keys?

Using array indices (`key={index}`) causes problems when the list order can change:

**Problem scenarios:**
- **Reordering** — React sees the same keys in the same positions and doesn't know items moved; it patches props in place instead of moving elements, which is slower and can corrupt component state.
- **Deletion** — removing item at index 0 shifts every other item's index, causing React to update all of them unnecessarily.
- **Stateful children** — if a child component has local state (e.g., an input's value), that state is tied to position, not the item, and will appear to jump to the wrong element.

```jsx
// This breaks when items are reordered or deleted:
{todos.map((todo, index) => (
  <TodoItem key={index} todo={todo} />
))}

// Use a stable unique ID instead:
{todos.map((todo) => (
  <TodoItem key={todo.id} todo={todo} />
))}
```

**When indices are acceptable:** static lists that never reorder, filter, or change — e.g., a fixed nav menu.

**Related:** [REACT-006 key prop](#react-006)

**Source:** GreatFrontEnd Q8

---

## REACT-008

### What are props in React? How are they different from state?

| | Props | State |
|---|---|---|
| **Ownership** | Passed in by the parent | Owned and managed internally |
| **Mutability** | Read-only inside the component | Can be updated with `setState` / `useState` |
| **Who controls it** | Parent component | The component itself |
| **Triggers re-render?** | Yes, when parent re-renders with new props | Yes, on every `setState` / `useState` update |

**Props** are the component's "inputs" — like function arguments. A component must never modify its own props.

```jsx
// Parent passes props
<Button label="Submit" onClick={handleSubmit} />

// Child reads props (read-only)
function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}
```

**State** is private data managed by the component itself, typically for things that change over time due to user interaction.

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

**Related:** [REACT-015 Lifting State Up](#react-015) | [REACT-022 Immutable State](#react-022)

**Source:** GreatFrontEnd Q9

---

## REACT-009

### What is the difference between class components and functional components?

| | Class Components | Functional Components |
|---|---|---|
| **Syntax** | ES6 class extending `React.Component` | Plain JavaScript function |
| **State** | `this.state` + `this.setState()` | `useState()` hook |
| **Lifecycle** | `componentDidMount`, `componentDidUpdate`, etc. | `useEffect()` and other hooks |
| **`this` binding** | Required — can cause bugs | Not needed |
| **Verbosity** | More boilerplate | Concise |
| **Performance** | Slightly heavier | Lighter (no class instantiation) |
| **React team preference** | Legacy — still supported | Recommended since React 16.8 |

```jsx
// Class component
class Greeter extends React.Component {
  state = { name: 'World' };
  render() {
    return <h1>Hello, {this.state.name}</h1>;
  }
}

// Equivalent function component
function Greeter() {
  const [name] = useState('World');
  return <h1>Hello, {name}</h1>;
}
```

**Related:** [REACT-010 When to use class components](#react-010) | [REACT-023 Hook benefits](#react-023)

**Source:** GreatFrontEnd Q10

---

## REACT-010

### When should you use a class component over a function component?

**Short answer: rarely, and only in legacy codebases.**

Function components with hooks can do everything class components can do, plus they're simpler. The React team has not deprecated class components but recommends function components for new code.

**The one remaining use case for class components:**

**Error Boundaries** — as of React 18, error boundaries can only be implemented with class components using `componentDidCatch` and `getDerivedStateFromError`. There is no hook equivalent yet (though React 19 introduces `onCaughtError` / `onUncaughtError` options on the root).

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    logErrorToService(error, info.componentStack);
  }

  render() {
    if (this.state.hasError) return <h1>Something went wrong.</h1>;
    return this.props.children;
  }
}
```

**Related:** [REACT-009 Class vs Functional](#react-009) | [REACT-037 Error Boundaries](#react-037)

**Source:** GreatFrontEnd Q11

---

## REACT-011

### What is React Fiber?

React **Fiber** is the complete rewrite of React's reconciliation engine, introduced in React 16. The core goal was to enable **incremental rendering** — the ability to split rendering work into chunks and spread it across multiple frames.

**Why it was needed:**
The previous "stack reconciler" was synchronous. Once it started reconciling a tree, it couldn't be interrupted — causing frame drops and janky UIs on complex updates.

**What Fiber enables:**
- **Pausing and resuming** rendering work.
- **Prioritizing** updates (user input gets higher priority than data fetches).
- **Concurrent Mode** and features like `useTransition`, `useDeferredValue`, and React 19 Actions.
- **Time-slicing** — breaking large renders into smaller units of work.

**Key concept — Fiber node:**
Each React element gets a corresponding Fiber node (a plain JS object) that stores component state, effect list, and work-in-progress information. React maintains two trees: the **current** tree (what's on screen) and the **work-in-progress** tree (what's being computed).

**Related:** [REACT-012 Reconciliation](#react-012) | [REACT-054 Concurrent Features](#react-054)

**Source:** GreatFrontEnd Q12

---

## REACT-012

### What is reconciliation in React?

**Reconciliation** is the process React uses to determine what changed in the VDOM and how to efficiently update the real DOM.

**The algorithm (two key heuristics):**

1. **Different element types produce different trees.** If the root element type changes (e.g., `<div>` → `<span>`), React tears down the old tree and builds a new one from scratch.

2. **Keys help identify stable children.** In lists, React uses `key` to match old vs new children rather than relying on position.

**Diffing rules:**
- Same element type → React updates only changed attributes.
- Same component type → React updates props and recurses.
- Different type → unmount old, mount new.

```jsx
// Only className attribute changes — React updates just that attribute
// Before: <div className="old" />
// After:  <div className="new" />
// → React calls: domNode.setAttribute('class', 'new')
```

**Performance implication:** The algorithm is O(n) where n is the number of nodes, because React assumes elements at the same tree depth are the same type.

**Related:** [REACT-003 Virtual DOM](#react-003) | [REACT-011 Fiber](#react-011) | [REACT-006 key prop](#react-006)

**Source:** GreatFrontEnd Q13

---

## REACT-013

### What is the difference between Shadow DOM and Virtual DOM?

| | Virtual DOM | Shadow DOM |
|---|---|---|
| **Purpose** | Performance optimization for React rendering | Native browser feature for style/DOM encapsulation |
| **Origin** | React (library concept) | Web Components standard (browser built-in) |
| **Location** | JavaScript memory | Inside the real DOM |
| **Visibility** | Not in browser DevTools DOM tree | Visible in DevTools (toggled via "show user agent shadow DOM") |
| **Encapsulation** | None — React styles bleed globally | Styles scoped inside the shadow root |
| **Use case** | Minimizing costly DOM operations | Building encapsulated custom elements |

**Shadow DOM** creates an isolated sub-tree attached to an element. CSS inside a shadow root doesn't leak out, and global CSS doesn't leak in (unless CSS custom properties are used).

**Virtual DOM** is purely a React abstraction — a JS object tree React uses internally to calculate minimal DOM patches.

They solve different problems and can be used together (a React component can render into a Shadow DOM).

**Related:** [REACT-003 Virtual DOM](#react-003) | [REACT-012 Reconciliation](#react-012)

**Source:** GreatFrontEnd Q14

---

## REACT-014

### What is the difference between Controlled and Uncontrolled components?

**Controlled component:** React state is the single source of truth for the input's value. Every keystroke goes through a state update.

```jsx
function ControlledInput() {
  const [value, setValue] = useState('');
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

**Uncontrolled component:** The DOM itself owns the value. React reads it on demand via a `ref`.

```jsx
function UncontrolledInput() {
  const inputRef = useRef(null);

  function handleSubmit() {
    console.log(inputRef.current.value); // read on demand
  }

  return <input ref={inputRef} defaultValue="initial" />;
}
```

| | Controlled | Uncontrolled |
|---|---|---|
| **Source of truth** | React state | DOM node |
| **Value access** | Always available in state | Via `ref` on demand |
| **Validation** | Easy — run on every change | Harder — validate on submit |
| **When to use** | Forms with dynamic validation, instant feedback | Simple forms, file inputs, third-party DOM libs |

**Note:** `<input type="file">` is always uncontrolled — React cannot set its value programmatically.

**Related:** [REACT-028 useRef](#react-028) | [REACT-008 Props vs State](#react-008)

**Source:** GreatFrontEnd Q15

---

## REACT-015

### How would you lift state up in React, and why?

**Lifting state up** means moving shared state to the **nearest common ancestor** of the components that need it, then passing it down as props.

**Why:** React's data flow is unidirectional (top-down). If two sibling components need to share or sync data, neither can read the other's state directly. The solution is to own that state in their shared parent.

**Before (each component owns its own state — can't sync):**
```jsx
function TemperatureInput({ scale }) {
  const [temp, setTemp] = useState('');
  return <input value={temp} onChange={e => setTemp(e.target.value)} />;
}
```

**After (parent owns state, passes down as props):**
```jsx
function Calculator() {
  const [temp, setTemp] = useState('');

  return (
    <>
      <TemperatureInput scale="C" value={temp} onChange={setTemp} />
      <TemperatureInput scale="F" value={toFahrenheit(temp)} onChange={v => setTemp(toCelsius(v))} />
    </>
  );
}

function TemperatureInput({ scale, value, onChange }) {
  return <input value={value} onChange={e => onChange(e.target.value)} />;
}
```

**Trade-off:** Lifting state too aggressively causes "prop drilling". Solutions include [Context API](#react-046) or external state managers.

**Related:** [REACT-050 Prop Drilling](#react-050) | [REACT-046 Context Pitfalls](#react-046)

**Source:** GreatFrontEnd Q16

---

## REACT-016

### What are Pure Components?

A **Pure Component** re-renders only when its props or state actually change (shallow comparison). In contrast, a regular component re-renders whenever its parent re-renders, regardless of whether props changed.

**Class-based:**
```jsx
// React.PureComponent performs shallow prop/state comparison
class MyList extends React.PureComponent {
  render() {
    return <ul>{this.props.items.map(i => <li key={i.id}>{i.name}</li>)}</ul>;
  }
}
```

**Function-based equivalent — `React.memo`:**
```jsx
const MyList = React.memo(function MyList({ items }) {
  return <ul>{items.map(i => <li key={i.id}>{i.name}</li>)}</ul>;
});
```

**Shallow comparison caveat:** If props include objects or arrays, a new reference (even with the same content) will trigger a re-render. Pair with `useMemo` / `useCallback` to stabilize references.

```jsx
// Parent: stabilize the reference
const stableItems = useMemo(() => computeItems(data), [data]);
return <MyList items={stableItems} />;
```

**Related:** [REACT-030 useMemo](#react-030) | [REACT-029 useCallback](#react-029) | [REACT-035 Re-rendering](#react-035)

**Source:** GreatFrontEnd Q17

---

## REACT-017

### What is the difference between `createElement` and `cloneElement`?

| | `React.createElement` | `React.cloneElement` |
|---|---|---|
| **Purpose** | Create a new element from scratch | Clone an existing element, optionally overriding props |
| **First arg** | Element type (string or component) | Existing React element |
| **Use case** | Normal JSX compilation | Higher-order components, render props, injecting props |

```jsx
// createElement — creates a fresh element
const el = React.createElement('button', { onClick: handler }, 'Click me');
// Same as: <button onClick={handler}>Click me</button>

// cloneElement — takes an existing element and merges new props
function WithExtraClass({ children }) {
  return React.cloneElement(children, { className: 'highlighted' });
}
// Usage: <WithExtraClass><Button /></WithExtraClass>
// Result: <Button className="highlighted" />
```

**`cloneElement` is useful for:**
- Injecting additional props into child components without wrapping them.
- Implementing compound components.
- Passing refs to children you don't own.

**Note:** `cloneElement` merges new props shallowly — new props override old ones, but unmentioned props are preserved.

**Related:** [REACT-059 Render Props](#react-059) | [REACT-058 Higher-Order Components](#react-058)

**Source:** GreatFrontEnd Q18

---

## REACT-018

### What is the role of PropTypes in React?

`PropTypes` is a built-in React library for **runtime type-checking of component props** during development. It logs a warning to the console if a prop has the wrong type or a required prop is missing.

```jsx
import PropTypes from 'prop-types';

function UserCard({ name, age, email }) {
  return <div>{name} ({age}) — {email}</div>;
}

UserCard.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  email: PropTypes.string.isRequired,
};

UserCard.defaultProps = {
  age: 0,
};
```

**Limitations:**
- Only runs in development (stripped in production builds).
- Checks happen at runtime, not during editing or compilation.
- Limited compared to TypeScript — no generics, no union types across files.

**Modern alternative:** **TypeScript** provides static type-checking at compile time, catches errors earlier, and integrates with editor tooling. Most new React projects use TypeScript instead of PropTypes.

**Related:** [REACT-021 Type-checking strategies](#react-021)

**Source:** GreatFrontEnd Q19

---

## REACT-019

### What are stateless components?

A **stateless component** is a component that has no internal state — it only renders based on the props it receives. It is a pure function of its inputs.

```jsx
// Stateless — output is entirely determined by props
function Avatar({ src, alt, size = 40 }) {
  return <img src={src} alt={alt} width={size} height={size} />;
}
```

**Characteristics:**
- Always produces the same output for the same props.
- No side effects, no subscriptions, no timers.
- Easy to test and reason about.
- Often called "presentational" or "dumb" components.

**Note:** A function component *can* be stateless even if it uses hooks — for example, a component that only uses `useMemo` or `useContext` but not `useState` / `useReducer` is still considered stateless in spirit.

**Related:** [REACT-020 Stateful Components](#react-020) | [REACT-060 Presentational vs Container](#react-060)

**Source:** GreatFrontEnd Q20

---

## REACT-020

### What are stateful components?

A **stateful component** manages its own internal state and can change its output over time in response to user interaction, async operations, or other events.

```jsx
function SearchBox() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  useEffect(() => {
    if (query) fetchResults(query).then(setResults);
  }, [query]);

  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <ResultsList items={results} />
    </>
  );
}
```

**Characteristics:**
- Has local state via `useState` / `useReducer` (or `this.state` in class components).
- May have side effects.
- Output can change over time without new props.
- Often called "container" or "smart" components.

**Stateful vs Stateless trade-off:** Stateless components are more reusable and testable. Prefer making components stateless and lifting state up to the fewest necessary places.

**Related:** [REACT-019 Stateless](#react-019) | [REACT-015 Lifting State Up](#react-015) | [REACT-060 Presentational vs Container](#react-060)

**Source:** GreatFrontEnd Q21

---

## REACT-021

### What are the recommended ways to type-check React component props?

**1. TypeScript (recommended for new projects)**
Static type-checking at compile time. Catches errors in the editor before you run the code.

```tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

function Button({ label, onClick, variant = 'primary', disabled = false }: ButtonProps) {
  return (
    <button className={variant} onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
}
```

**2. PropTypes (legacy / JS projects)**
Runtime type-checking in development only. See [REACT-018 PropTypes](#react-018).

**3. JSDoc (lightweight JS projects)**
Type annotations in comments — picked up by editors like VS Code without a TS compiler.

```jsx
/**
 * @param {{ label: string, onClick: () => void }} props
 */
function Button({ label, onClick }) { ... }
```

**Recommendation:** Use **TypeScript** for all new projects. Use PropTypes only in existing JavaScript codebases where migrating to TypeScript is not feasible.

**Related:** [REACT-018 PropTypes](#react-018)

**Source:** GreatFrontEnd Q22

---

## REACT-022

### Why does React recommend against mutating state?

React's rendering model depends on **reference equality** to detect changes. If you mutate an object in place, the reference stays the same, and React won't know a change occurred — leading to stale UI.

**Mutation causes bugs:**

```jsx
// BAD — mutates state in place
function addItem(item) {
  state.items.push(item); // same array reference
  setState(state);        // React sees same object → no re-render
}

// GOOD — returns a new reference
function addItem(item) {
  setState(prev => ({ ...prev, items: [...prev.items, item] }));
}
```

**Why immutability matters:**
- Enables React to bail out of renders when references haven't changed (memoization, `React.memo`, `useMemo`).
- Makes state history traceable (important for undo/redo, Redux DevTools time-travel).
- Prevents hard-to-debug issues where shared mutable objects cause one component to silently affect another.
- Required for React concurrent features — React may prepare multiple versions of the UI simultaneously.

**Helpers:** Use spread syntax, `Array.from()`, `[...arr]`, `Object.assign()`, or libraries like Immer for complex nested updates.

**Related:** [REACT-008 Props vs State](#react-008) | [REACT-031 useReducer](#react-031)

**Source:** GreatFrontEnd Q23

---

## REACT-023

### What are the benefits of using hooks in React?

Hooks (introduced in React 16.8) let function components use state, lifecycle, context, and other React features — eliminating the need for class components in most cases.

**Key benefits:**

**1. Reusable stateful logic** — Custom hooks let you extract and share logic between components without HOCs or render props.
```jsx
// Reusable hook — share across any component
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => localStorage.getItem(key) ?? initialValue);
  useEffect(() => { localStorage.setItem(key, value); }, [key, value]);
  return [value, setValue];
}
```

**2. Simpler code** — Logic related to the same concern is co-located in one hook, instead of split across `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

**3. No `this` binding issues** — Class component bugs from forgetting to bind event handlers disappear entirely.

**4. Better composition** — Hooks compose naturally by calling one inside another, unlike HOC wrapper hell.

**5. Smaller bundle** — Function components with hooks typically produce smaller minified output than equivalent class components.

**6. Testing** — Hooks and their logic can be tested in isolation with `renderHook` from React Testing Library.

**Related:** [REACT-024 Rules of Hooks](#react-024) | [REACT-009 Class vs Function](#react-009) | [REACT-034 Custom Hooks](#react-034)

**Source:** GreatFrontEnd Q24

---

*← Back to [Master Index](../reference/00-master-index.md) | Next: [React Hooks →](./02-hooks.md)*
