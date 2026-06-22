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

---

## REACT-145

### What is the history behind React's evolution?

React was created by Jordan Walke at Facebook and first used in Facebook's News Feed in 2011, then Instagram in 2012. It was open-sourced at JSConf US in May 2013.

Key milestones:

- **2013** — Initial open-source release. Class components, `React.createClass`, virtual DOM.
- **2015** — React Native released. React 0.14 split `react` and `react-dom` into separate packages.
- **2016** — React 15: functional stateless components gain traction. Fiber rewrite begins.
- **2017** — React 16 (Fiber): error boundaries, portals, returning arrays/strings from render, improved SSR.
- **2018** — React 16.3: new Context API, `createRef`, lifecycle deprecations. React 16.6: `React.memo`, `React.lazy`, Suspense.
- **2019** — React 16.8: **Hooks** — the most significant API addition since React's release.
- **2020** — React 17: no new features; focused on gradual upgrades, new JSX transform (no more `import React`).
- **2022** — React 18: concurrent features (`useTransition`, `useDeferredValue`), automatic batching, Suspense SSR, `startTransition`.
- **2024** — React 19: Server Components stable, Actions, `useActionState`, `useOptimistic`, `use()`, React Compiler.

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001) | [REACT-011 — Fiber](./01-fundamentals.md#react-011)

**Source:** [SudheerJ SDJ-002](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-146

### How do you create components in React?

Two ways: **function components** (modern, preferred) and **class components** (legacy).

**Function component:**

```jsx
// Simple — returns JSX
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Arrow function variant
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;
```

**Class component:**

```jsx
import { Component } from 'react';

class Greeting extends Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

**Usage:**

```jsx
<Greeting name="Akar" />
```

Component names must start with a capital letter — lowercase tags are treated as HTML elements by the JSX transform. For new code, always use function components with hooks.

**Related:** [REACT-009 — Class vs functional](./01-fundamentals.md#react-009) | [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-006](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-147

### What are inline conditional expressions in React?

Inline conditionals are patterns for rendering content conditionally within JSX without extracting logic into an external `if` statement.

**Short-circuit (`&&`)** — renders the right side only if the left is truthy:

```jsx
{isLoggedIn && <Dashboard />}
{count > 0 && <p>You have {count} messages.</p>}
```

⚠️ Avoid `{count && <p>...</p>}` when `count` could be `0` — React renders `0` as text. Use `{count > 0 && ...}` instead.

**Ternary (`? :`)** — choose between two outputs:

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

**Nullish / extraction** — for complex conditions, extract to a variable or function:

```jsx
const content = (() => {
  if (loading) return <Spinner />;
  if (error) return <ErrorMsg />;
  return <DataView data={data} />;
})();

return <div>{content}</div>;
```

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-155 — Conditional rendering](./01-fundamentals.md#react-155)

**Source:** [SudheerJ SDJ-014](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-148

### What is the children prop?

`children` is a special React prop that holds whatever JSX or values are placed between a component's opening and closing tags.

```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">{children}</div>
    </div>
  );
}

// Usage — anything between <Card> tags becomes children
<Card title="Welcome">
  <p>Hello world!</p>
  <button>Click me</button>
</Card>
```

`children` can be anything: a string, a single element, an array of elements, a render function (render props pattern), or `null`. Use `React.Children` utilities to inspect or iterate over children safely.

```jsx
// Count and map children safely
React.Children.count(children);
React.Children.map(children, (child) => cloneElement(child, { active: true }));
```

**Related:** [REACT-061 — Render props](./03-advanced.md#react-061) | [REACT-062 — Composition](./03-advanced.md#react-062)

**Source:** [SudheerJ SDJ-026](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-149

### How to write comments in React / JSX?

Inside JSX, JavaScript-style comments must be wrapped in `{}`:

```jsx
function App() {
  return (
    <div>
      {/* This is a JSX comment — renders nothing */}
      <h1>Hello</h1>
      {/* Multi-line
          JSX comment */}
    </div>
  );
}
```

Outside JSX (in JS logic), standard JS comments work normally:

```jsx
function App() {
  // This is a regular JS comment
  const title = 'Hello'; /* Also valid */
  return <h1>{title}</h1>;
}
```

`<!-- HTML comments -->` do not work in JSX and will cause a parse error.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-027](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-150

### Does React.lazy support named exports?

No — `React.lazy` only works with **default exports**. If the component you want to lazy-load is a named export, you must re-export it as default in an intermediate module.

```jsx
// components/HeavyChart.jsx
export const HeavyChart = () => <div>Chart</div>; // named export

// HeavyChart.lazy.js — re-export as default
export { HeavyChart as default } from './HeavyChart';

// Usage — now lazy works
const HeavyChart = React.lazy(() => import('./HeavyChart.lazy'));
```

Alternatively, convert the component to a default export if you control the source.

**Related:** [REACT-051 — Lazy loading](./03-advanced.md#react-051) | [REACT-042 — Code splitting](./03-advanced.md#react-042)

**Source:** [SudheerJ SDJ-029](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-151

### What are the limitations of React?

React is a library (not a full framework), which means:

**Scope is narrow.** React handles the view layer only. Routing (React Router), state management (Redux, Zustand), and data fetching (RTK Query, TanStack Query) are left to third-party libraries.

**JSX learning curve.** JSX is not standard HTML or JS, which can be surprising for beginners. Build tooling (Babel/SWC/Vite) is required.

**Frequent ecosystem churn.** The React ecosystem evolves rapidly. Patterns that were standard (class components, HOCs, `redux-form`) become obsolete. Staying current requires ongoing learning.

**Performance pitfalls.** Misusing context, missing memoization, or placing state too high can cause cascading re-renders. Optimization requires deliberate effort.

**Opinionated about nothing.** The freedom to choose your own stack is also a burden — teams must make and maintain many architectural decisions.

**Not a complete solution for SEO/SSR.** A plain React SPA is not SEO-friendly. Server-side rendering requires Next.js, Remix, or custom setup.

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001) | [REACT-145 — React history](./01-fundamentals.md#react-145)

**Source:** [SudheerJ SDJ-038](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-152

### What is the react-dom package?

`react-dom` is the package that connects React to the browser DOM. It is separate from `react` because React's core (component model, reconciliation) is renderer-agnostic — the same core works with `react-native`, `react-three-fiber`, `react-pdf`, etc.

Key APIs:

```jsx
import { createRoot } from 'react-dom/client';
import { createPortal } from 'react-dom';
import { flushSync } from 'react-dom';

// Mount the app
const root = createRoot(document.getElementById('root'));
root.render(<App />);

// Render outside component tree (used in Portals)
createPortal(<Modal />, document.body);

// Force synchronous state flush (rare, used in tests/event handlers)
flushSync(() => setState(newValue));
```

`react-dom/server` contains SSR APIs (`renderToString`, `renderToPipeableStream`).

**Related:** [REACT-040 — Portals](./03-advanced.md#react-040) | [REACT-039 — Hydration](./03-advanced.md#react-039)

**Source:** [SudheerJ SDJ-040, SDJ-061, SDJ-062](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-153

### How to use innerHTML in React? (dangerouslySetInnerHTML)

React does not expose `innerHTML` directly. Instead it provides `dangerouslySetInnerHTML`, a prop that intentionally looks dangerous to discourage casual use — it bypasses React's XSS protection.

```jsx
function BlogPost({ htmlContent }) {
  return (
    <div dangerouslySetInnerHTML={{ __html: htmlContent }} />
  );
}
```

The double-brace syntax (`{{ __html: ... }}`) is intentional: the outer `{}` is JSX interpolation, the inner `{}` is the object React expects with a `__html` key.

**⚠️ Security warning:** Never pass unsanitized user input. Always sanitize HTML first using a library like `DOMPurify`:

```jsx
import DOMPurify from 'dompurify';

<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userInput) }} />
```

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-188 — JSX injection prevention](./01-fundamentals.md#react-188)

**Source:** [SudheerJ SDJ-042](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-154

### How to apply styles in React?

React supports multiple styling approaches:

**1. Inline styles** — JavaScript object, camelCase properties, no units for zero values:

```jsx
<div style={{ backgroundColor: '#f0f0f0', fontSize: '16px', margin: 0 }}>
  Inline styled
</div>
```

**2. CSS Modules** — locally scoped class names, no conflicts:

```jsx
import styles from './Button.module.css';
<button className={styles.primary}>Click</button>
```

**3. External CSS** — import a stylesheet:

```jsx
import './Button.css';
<button className="btn-primary">Click</button>
```

**4. CSS-in-JS** (Styled Components, Emotion):

```jsx
const Button = styled.button`background: blue; color: white;`;
```

**5. Utility-first** (Tailwind CSS):

```jsx
<button className="bg-blue-500 text-white px-4 py-2 rounded">Click</button>
```

Inline styles don't support pseudo-selectors or media queries. Use CSS Modules or CSS-in-JS when those are needed.

**Related:** [REACT-144 — Styled Components](./10-libraries.md#react-144) | [REACT-176 — CSS Modules](./01-fundamentals.md#react-176)

**Source:** [SudheerJ SDJ-043](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-155

### How to conditionally render components?

See also [REACT-147 — Inline conditional expressions](#react-147) for the shorthand patterns. For full conditional rendering approaches:

```jsx
// 1. if/else before return
function Alert({ type, message }) {
  if (!message) return null; // renders nothing

  if (type === 'error') {
    return <div className="error">{message}</div>;
  }
  return <div className="info">{message}</div>;
}

// 2. Ternary inline
{isLoading ? <Spinner /> : <Content />}

// 3. Short-circuit
{hasPermission && <AdminPanel />}

// 4. Object map (switch-style)
const views = { home: <Home />, about: <About />, contact: <Contact /> };
return views[currentPage] ?? <NotFound />;
```

Returning `null` from a component renders nothing and is the standard way to "hide" a component without unmounting its parent.

**Related:** [REACT-147 — Inline expressions](#react-147) | [REACT-158 — Switching component](#react-158)

**Source:** [SudheerJ SDJ-046](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-156

### Why be careful when spreading props onto DOM elements?

Spreading all props onto a DOM element can pass unknown HTML attributes, which either produces browser warnings or silently adds invalid attributes.

```jsx
// ⚠️ Dangerous — isActive is not a valid HTML attribute
function Button({ isActive, ...props }) {
  return <button {...props} />; // if props includes isActive, it leaks to DOM
}

// ✓ Safe — destructure component-specific props, spread only the rest
function Button({ isActive, onClick, children, ...domProps }) {
  return (
    <button
      {...domProps}   // only real DOM attributes
      onClick={onClick}
      className={isActive ? 'active' : ''}
    >
      {children}
    </button>
  );
}
```

In React 16+ unknown attributes are passed to the DOM (previously they were silently stripped), so this produces browser console warnings. React 19 is stricter about this.

**Related:** [REACT-008 — Props](./01-fundamentals.md#react-008) | [REACT-161 — Custom DOM attributes](#react-161)

**Source:** [SudheerJ SDJ-047](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-157

### How to enable production mode in React?

React's development build includes extra warnings, propType checks, and DevTools hooks. These have a real performance cost. For production, you need the minified production build.

**With Vite (recommended):**

```bash
vite build   # automatically sets NODE_ENV=production
```

**With Create React App:**

```bash
npm run build   # outputs optimized production build in /build
```

**With Webpack manually:**

```js
// webpack.config.js
const webpack = require('webpack');
module.exports = {
  mode: 'production',   // sets NODE_ENV=production, enables tree-shaking, minification
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production'),
    }),
  ],
};
```

React checks `process.env.NODE_ENV` at runtime. When it equals `'production'`, all development-only code paths are stripped by the bundler via dead-code elimination.

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001)

**Source:** [SudheerJ SDJ-050](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-158

### What is a switching component?

A switching component renders one of several child components based on a prop (typically called `page` or `route`). It is a simple manual router pattern useful when a full routing library is overkill.

```jsx
import Home from './Home';
import About from './About';
import Contact from './Contact';
import NotFound from './NotFound';

const PAGES = { home: Home, about: About, contact: Contact };

function Switch({ page }) {
  const Page = PAGES[page] ?? NotFound;
  return <Page />;
}

// Usage
<Switch page="about" />
```

This pattern is essentially a component-level `switch` statement. In practice, React Router's `<Routes>` / `<Route>` handles this declaratively and with URL binding. Use the manual pattern for in-page tab switching or wizard steps that don't need URL state.

**Related:** [REACT-066 — React Router](./04-routing.md#react-066) | [REACT-155 — Conditional rendering](#react-155)

**Source:** [SudheerJ SDJ-052](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-159

### What are React Mixins?

Mixins were a React pattern (from `React.createClass` era, pre-ES6) for sharing behavior between components by merging methods and lifecycle hooks. They are **deprecated** and not supported in modern React.

```js
// Legacy — createClass + mixin (no longer works in modern React)
const MouseMixin = {
  getInitialState() { return { x: 0, y: 0 }; },
  componentDidMount() {
    window.addEventListener('mousemove', this.handleMouseMove);
  },
  handleMouseMove(e) { this.setState({ x: e.clientX, y: e.clientY }); },
};

const MyComponent = React.createClass({
  mixins: [MouseMixin],
  render() { return <div>Mouse: {this.state.x}, {this.state.y}</div>; },
});
```

**Why they were removed:** Mixins create implicit dependencies, cause name collisions when multiple mixins define the same key, and make the data flow hard to trace.

**Modern replacements:** Custom hooks (preferred), Higher-Order Components, Render Props.

**Related:** [REACT-034 — Custom hooks](./02-hooks.md#react-034) | [REACT-059 — HOCs](./03-advanced.md#react-059)

**Source:** [SudheerJ SDJ-053](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-160

### What are Pointer Events in React?

Pointer Events are a W3C API that unifies mouse, touch, and stylus input into a single event model. React supports them as synthetic events on any element.

```jsx
function Canvas() {
  return (
    <div
      onPointerDown={(e) => console.log('pointer down', e.pointerId)}
      onPointerMove={(e) => console.log('pointer move', e.clientX, e.clientY)}
      onPointerUp={(e) => console.log('pointer up')}
      onPointerCancel={(e) => console.log('cancelled')}
      onPointerEnter={(e) => console.log('entered')}
      onPointerLeave={(e) => console.log('left')}
      onPointerOver={(e) => console.log('over')}
      onPointerOut={(e) => console.log('out')}
      onGotPointerCapture={(e) => console.log('captured')}
      onLostPointerCapture={(e) => console.log('lost capture')}
    />
  );
}
```

`e.pointerType` tells you whether the input is `'mouse'`, `'touch'`, or `'pen'`. Pointer Events replace the need to handle both `onMouseDown` + `onTouchStart` separately.

**Related:** [REACT-052 — Synthetic events](./03-advanced.md#react-052)

**Source:** [SudheerJ SDJ-054](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-161

### Are custom DOM attributes supported in React?

Yes, since React 16. Before React 16, unknown HTML attributes were silently stripped. Now React passes unknown attributes through to the DOM, but logs a warning if the attribute is not a known HTML attribute.

```jsx
// React 16+ — passes through to DOM
<div mycustomattr="value" />
// Renders: <div mycustomattr="value"></div>

// For data attributes — always supported, no warnings
<div data-testid="submit-btn" data-user-id="42" />

// For ARIA attributes — always supported
<div aria-label="Close dialog" aria-hidden="false" />
```

Custom attributes should use `data-*` convention for semantic correctness. Avoid inventing arbitrary attribute names — use `data-*` or `aria-*` for non-standard HTML attributes.

**Related:** [REACT-156 — Spreading props onto DOM](#react-156)

**Source:** [SudheerJ SDJ-056](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-162

### How to loop (iterate) inside JSX?

Use `Array.prototype.map()` — it returns an array of JSX elements which React renders in order.

```jsx
const fruits = ['Apple', 'Banana', 'Cherry'];

function FruitList() {
  return (
    <ul>
      {fruits.map((fruit) => (
        <li key={fruit}>{fruit}</li>
      ))}
    </ul>
  );
}
```

For filtering + mapping:

```jsx
{items
  .filter((item) => item.active)
  .map((item) => <Item key={item.id} {...item} />)
}
```

For index-based loops when there is no natural unique key (use sparingly — see [REACT-007]):

```jsx
{Array.from({ length: 5 }, (_, i) => (
  <Skeleton key={i} />
))}
```

Always include a stable `key` prop on the outermost element returned from `map`.

**Related:** [REACT-006 — key prop](./01-fundamentals.md#react-006) | [REACT-007 — Index as key](./01-fundamentals.md#react-007)

**Source:** [SudheerJ SDJ-057](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-163

### How to access props in JSX attribute quotes?

You can't use quotes to interpolate a prop — use curly braces `{}`:

```jsx
// ✗ Wrong — this is a literal string, not the prop value
<Avatar src="user.avatarUrl" />

// ✓ Correct — curly braces interpolate the prop
<Avatar src={user.avatarUrl} />

// ✓ Mix of string and prop
<Avatar alt={`Avatar of ${user.name}`} />

// ✓ Boolean props (either form is equivalent)
<Button disabled />
<Button disabled={true} />
```

JSX attribute values are either string literals (in double quotes) or JavaScript expressions (in curly braces). You cannot embed curly braces inside quotes.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-058](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-164

### How to conditionally apply CSS class names?

Use template literals or the `clsx` / `classnames` library:

```jsx
// Template literal (simple cases)
<div className={`btn ${isActive ? 'btn--active' : ''} ${isLarge ? 'btn--large' : ''}`} />

// clsx (recommended for multiple conditions)
import clsx from 'clsx';

<div className={clsx(
  'btn',
  isActive && 'btn--active',
  isLarge && 'btn--large',
  variant === 'danger' && 'btn--danger',
)} />

// Object syntax with clsx
<div className={clsx({
  'btn': true,
  'btn--active': isActive,
  'btn--disabled': disabled,
})} />
```

`clsx` is a tiny utility that filters out falsy values and joins the rest. It prevents the trailing spaces and `undefined` strings you'd get from naive template literals.

**Related:** [REACT-154 — Styling in React](#react-154)

**Source:** [SudheerJ SDJ-060](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-165

### What is the difference between React and ReactDOM?

`react` is the core library — it provides the component model, hooks, JSX transformation, and the reconciler. It has no concept of the browser or any platform.

`react-dom` is the DOM renderer — it knows how to take React's virtual tree and apply it to the browser's real DOM. It provides `createRoot`, `createPortal`, and `hydrateRoot`.

```js
import React, { useState } from 'react';        // component logic, hooks
import { createRoot } from 'react-dom/client';  // browser rendering

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

They were separated in React 0.14 to enable alternative renderers:
- `react-native` → renders to iOS/Android native views
- `react-three-fiber` → renders to WebGL via Three.js
- `react-pdf` → renders to PDF
- `react-test-renderer` → renders to JSON for testing

**Related:** [REACT-152 — react-dom package](#react-152) | [REACT-136 — React Native vs React](./09-react-native.md#react-136)

**Source:** [SudheerJ SDJ-061, SDJ-062](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-166

### How to use the React label element?

In HTML, `<label for="inputId">` associates a label with an input. In JSX, `for` is a reserved JavaScript keyword, so React uses `htmlFor`:

```jsx
function LoginForm() {
  return (
    <form>
      <label htmlFor="email">Email</label>
      <input id="email" type="email" />

      <label htmlFor="password">Password</label>
      <input id="password" type="password" />
    </form>
  );
}
```

Proper label-input association is important for accessibility — clicking the label focuses the input, and screen readers announce the label when the input is focused.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-063](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-167

### How to combine multiple inline style objects?

Use the spread operator to merge style objects:

```jsx
const baseStyle = { padding: '8px 16px', borderRadius: '4px' };
const primaryStyle = { backgroundColor: '#6200ea', color: 'white' };

// Merge with spread
<button style={{ ...baseStyle, ...primaryStyle }}>Click</button>

// Dynamic merge with a conditional override
<button style={{ ...baseStyle, ...(isPrimary ? primaryStyle : {}) }}>Click</button>

// Object.assign alternative
<button style={Object.assign({}, baseStyle, primaryStyle)}>Click</button>
```

Later properties override earlier ones (same as `Object.assign`). For complex style composition, a CSS-in-JS library like Styled Components or Emotion is more maintainable.

**Related:** [REACT-154 — Styling in React](#react-154)

**Source:** [SudheerJ SDJ-064](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-168

### How to pretty-print JSON in React?

Use `JSON.stringify` with indent arguments inside a `<pre>` tag:

```jsx
function JsonDisplay({ data }) {
  return (
    <pre>
      {JSON.stringify(data, null, 2)}
    </pre>
  );
}

// Usage
<JsonDisplay data={{ name: 'Akar', role: 'engineer', skills: ['React', 'TS'] }} />
```

`JSON.stringify(value, replacer, space)`:
- `replacer` — `null` means include all keys
- `space` — `2` means 2-space indentation

`<pre>` preserves whitespace and line breaks so the formatted JSON renders correctly. Add CSS `word-break: break-all` if values can be very long.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-066](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-169

### How to detect the React version at runtime?

```jsx
import React from 'react';
console.log(React.version); // e.g. "19.0.0"

// In a component
function VersionBadge() {
  return <span>React v{React.version}</span>;
}
```

`React.version` is a string property available on the `react` package object. It reflects the version of the `react` package installed, not `react-dom` (though they should match in most setups).

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001)

**Source:** [SudheerJ SDJ-069](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-170

### How to apply vendor prefixes to inline styles in React?

React does not auto-apply vendor prefixes to inline styles. You must include them explicitly as camelCase keys:

```jsx
const style = {
  WebkitTransition: 'all 0.3s ease',  // -webkit-transition
  MozTransition: 'all 0.3s ease',     // -moz-transition
  msTransition: 'all 0.3s ease',      // -ms-transition (lowercase ms)
  transition: 'all 0.3s ease',        // standard
};

<div style={style}>Animated</div>
```

The rule: vendor prefix letters are capitalized (`Webkit`, `Moz`, `O`) except `-ms-` which stays lowercase (`ms`).

**In practice:** For new projects using Vite, webpack + PostCSS, or any modern build pipeline, autoprefixer handles vendor prefixes in CSS files automatically. Inline style vendor prefixing is rarely necessary — prefer a CSS class for anything that needs prefixes.

**Related:** [REACT-154 — Styling in React](#react-154)

**Source:** [SudheerJ SDJ-071](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-171

### How to import and export components in React with ES6?

**Named exports** — a file can have many:

```jsx
// components/Button.jsx
export function PrimaryButton({ children }) { return <button className="primary">{children}</button>; }
export function SecondaryButton({ children }) { return <button className="secondary">{children}</button>; }

// Import
import { PrimaryButton, SecondaryButton } from './components/Button';
```

**Default export** — one per file:

```jsx
// components/Card.jsx
export default function Card({ title, children }) {
  return <div className="card"><h2>{title}</h2>{children}</div>;
}

// Import — can use any name
import Card from './components/Card';
import MyCard from './components/Card'; // also valid
```

**Re-export pattern** (barrel file `index.js`):

```jsx
// components/index.js
export { PrimaryButton, SecondaryButton } from './Button';
export { default as Card } from './Card';

// Consumer
import { Card, PrimaryButton } from './components';
```

React itself has no preference — use default exports for one-component-per-file, named exports when a file exports multiple utilities.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-072](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-172

### Are there exceptions to React component naming rules?

The core rule: **component names must start with a capital letter** when used in JSX — React uses this to distinguish components from HTML elements.

Exceptions and edge cases:

```jsx
// ✓ Namespaced components — dot notation is allowed
import * as UI from './ui-library';
<UI.button />  // Treated as component even though it starts lowercase!

// ✓ Component assigned to a capitalized variable — works
const MyComp = someArray[0]; // if someArray[0] is a component
<MyComp />

// ✗ Lowercase variable — treated as HTML tag
const comp = Button;
<comp />  // React tries to render an <comp> HTML tag, not the Button component

// ✓ Capitalized variable — renders the component
const Comp = Button;
<Comp />
```

The dot-notation exception (`<UI.button>`) is the most commonly encountered — it lets you use namespaced component libraries even if the leaf name is lowercase.

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-146 — Creating components](#react-146)

**Source:** [SudheerJ SDJ-073](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-173

### Can you use async/await in React components?

You cannot make a component function itself `async` — React doesn't know how to render a Promise. But you can use async logic inside event handlers and `useEffect`:

```jsx
// ✗ Wrong — component is async
async function UserProfile({ id }) {
  const user = await fetchUser(id); // React can't handle this
  return <div>{user.name}</div>;
}

// ✓ Correct — async logic inside useEffect
function UserProfile({ id }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    async function load() {
      const data = await fetchUser(id);
      setUser(data);
    }
    load();
  }, [id]);

  return user ? <div>{user.name}</div> : <Spinner />;
}

// ✓ Async event handler — fine
function Form() {
  const handleSubmit = async (e) => {
    e.preventDefault();
    await submitForm(formData);
  };
  return <form onSubmit={handleSubmit}>...</form>;
}
```

**Exception — React Server Components (React 19):** RSCs can be async:

```jsx
async function UserProfile({ id }) {
  const user = await db.users.find(id); // runs on server only
  return <div>{user.name}</div>;
}
```

**Related:** [REACT-026 — useEffect dependency](./02-hooks.md#react-026) | [REACT-106 — Server Components](./07-react19.md#react-106)

**Source:** [SudheerJ SDJ-074](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-174

### What are common folder structures for React?

Two dominant approaches:

**Feature-based (recommended for mid-to-large apps):**

```
src/
├── features/
│   ├── auth/
│   │   ├── AuthForm.jsx
│   │   ├── authSlice.js
│   │   └── useAuth.js
│   └── dashboard/
│       ├── Dashboard.jsx
│       └── dashboardApi.js
├── shared/
│   ├── components/     Button, Modal, Input...
│   ├── hooks/          useDebounce, useFetch...
│   └── utils/          formatDate, validators...
└── app/
    ├── App.jsx
    ├── store.js
    └── router.jsx
```

**Type-based (simpler for small apps):**

```
src/
├── components/
├── hooks/
├── pages/
├── services/
├── store/
└── utils/
```

The React team recommends not over-thinking folder structure early — start flat, group by feature as the app grows. The key rule: **code that changes together should live together**.

**Related:** [REACT-124 — Redux directory structure](./08-redux.md#react-124)

**Source:** [SudheerJ SDJ-075](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-175

### What are popular animation libraries for React?

| Library | Approach | Best For |
|---|---|---|
| **Framer Motion** | Declarative, component-based | Complex animations, layout, gestures |
| **React Spring** | Physics-based spring animations | Natural, fluid motion |
| **GSAP** (with React) | Imperative, timeline-based | Complex sequenced animations |
| **React Transition Group** | Transition lifecycle hooks | Simple enter/exit animations |
| **Animate.css** (via className) | CSS classes | Quick, simple CSS animations |

```jsx
// Framer Motion example
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0, y: -20 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0 }}
  transition={{ duration: 0.3 }}
>
  Hello!
</motion.div>
```

**Framer Motion** is the most popular choice for React — declarative API, layout animations, and gesture support out of the box.

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001)

**Source:** [SudheerJ SDJ-076](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-176

### What is the benefit of CSS Modules?

CSS Modules are CSS files where class names are locally scoped by default. The build tool transforms class names into unique identifiers, preventing style collisions across components.

```css
/* Button.module.css */
.button { padding: 8px 16px; background: blue; }
.primary { background: purple; }
```

```jsx
import styles from './Button.module.css';

function Button({ primary }) {
  return (
    <button className={`${styles.button} ${primary ? styles.primary : ''}`}>
      Click
    </button>
  );
}
// Rendered: <button class="Button_button__3aK7m Button_primary__9xF2p">
```

**Benefits:**
- No global scope pollution — `.button` in one file can't conflict with `.button` in another
- Explicit imports make it clear which styles belong to which component
- Works with standard CSS (no new syntax to learn)
- Dead-code analysis tools can detect unused styles

**Related:** [REACT-154 — Styling in React](#react-154) | [REACT-144 — Styled Components](./10-libraries.md#react-144)

**Source:** [SudheerJ SDJ-077](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-177

### What are popular React-specific linters?

**ESLint** is the foundation — the most widely used JavaScript linter. For React, two key plugin sets:

**`eslint-plugin-react`** — rules for React best practices:
- `react/prop-types` — enforce PropTypes declarations
- `react/no-array-index-key` — warn when using array index as key
- `react/jsx-key` — missing key in JSX arrays
- `react/no-unused-state` — detect unused state properties

**`eslint-plugin-react-hooks`** — rules for hooks (officially maintained by React team):
- `react-hooks/rules-of-hooks` — enforces Rules of Hooks (no conditional hooks, etc.)
- `react-hooks/exhaustive-deps` — warns when useEffect/useCallback dependency arrays are incomplete

```bash
npm install --save-dev eslint eslint-plugin-react eslint-plugin-react-hooks
```

```json
// .eslintrc.json
{
  "plugins": ["react", "react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

`eslint-plugin-react-hooks` is considered essential for any hooks-based codebase.

**Related:** [REACT-024 — Rules of hooks](./02-hooks.md#react-024)

**Source:** [SudheerJ SDJ-078](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-178

### What are the rules of JSX?

JSX compiles to `React.createElement` calls and must follow these rules:

**1. Return a single root element.** Multiple sibling elements must be wrapped in one parent or a Fragment:

```jsx
// ✗ Error
return <h1>Title</h1><p>Body</p>;

// ✓ Wrap in a div or Fragment
return <><h1>Title</h1><p>Body</p></>;
```

**2. All tags must be closed.** Self-closing tags need `/>`; void elements too:

```jsx
<img src="..." alt="..." />   // ✓
<input type="text" />         // ✓
<img>                         // ✗ Error in JSX
```

**3. Use camelCase for attributes.** JSX attributes map to JavaScript properties, not HTML attributes:

```jsx
<div className="box" onClick={handler} htmlFor="id" tabIndex={0} />
```

**4. Use `{}` for JavaScript expressions:**

```jsx
<p>{isLoggedIn ? 'Welcome' : 'Sign in'}</p>
```

**5. Component names must start with a capital letter.**

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-179 — Why wrap JSX tags](#react-179)

**Source:** [SudheerJ SDJ-248](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-179

### Why must multiple JSX tags be wrapped in a single parent?

JSX compiles to a single `React.createElement(type, props, ...children)` call. A function can only `return` one value — returning two sibling `createElement` calls is a syntax error.

```jsx
// JSX source
return (
  <h1>Title</h1>
  <p>Body</p>
);

// Compiled — invalid JavaScript: two return values
return React.createElement('h1', null, 'Title')
       React.createElement('p', null, 'Body'); // Syntax error!
```

Solutions:

```jsx
// 1. Fragment — no extra DOM node
return (<><h1>Title</h1><p>Body</p></>);

// 2. div wrapper — adds a DOM node
return (<div><h1>Title</h1><p>Body</p></div>);

// 3. Keyed Fragment (for lists)
return (<React.Fragment key={id}><h1>Title</h1><p>Body</p></React.Fragment>);
```

Prefer Fragment (`<>`) when the wrapping div would affect layout or CSS (e.g., flexbox/grid children).

**Related:** [REACT-005 — Fragments](./01-fundamentals.md#react-005) | [REACT-178 — JSX rules](#react-178)

**Source:** [SudheerJ SDJ-249](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-180

### What is the difference between imperative and declarative programming in React?

**Imperative** — you describe *how* to achieve a result step by step:

```js
// Imperative: manipulate DOM directly
const btn = document.getElementById('submit');
btn.disabled = true;
btn.textContent = 'Submitting...';
btn.className = 'btn btn--loading';
```

**Declarative** — you describe *what* the UI should look like given a state:

```jsx
// Declarative: React handles the DOM updates
function SubmitButton({ loading }) {
  return (
    <button disabled={loading} className={loading ? 'btn btn--loading' : 'btn'}>
      {loading ? 'Submitting...' : 'Submit'}
    </button>
  );
}
```

React is declarative — you write the desired output as JSX and React reconciles the DOM to match. When `loading` changes, React diffs the virtual DOM and applies only the necessary DOM mutations. You don't write the step-by-step DOM operations.

The benefit: declarative code is easier to reason about because the UI is always a deterministic function of state.

**Related:** [REACT-003 — Virtual DOM](./01-fundamentals.md#react-003) | [REACT-045 — One-way data flow](./03-advanced.md#react-045)

**Source:** [SudheerJ SDJ-234](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-181

### What are the benefits of the new JSX transform?

Before React 17, using JSX required importing React in every file because JSX compiled to `React.createElement`:

```jsx
// Required before React 17
import React from 'react';
function App() { return <h1>Hello</h1>; }
```

The new JSX transform (React 17+, via Babel/SWC) compiles JSX to calls from a new `react/jsx-runtime` import that is added automatically by the compiler:

```jsx
// React 17+ — no import needed
function App() { return <h1>Hello</h1>; }
// Compiled to:
import { jsx as _jsx } from 'react/jsx-runtime';
function App() { return _jsx('h1', { children: 'Hello' }); }
```

**Benefits:**
- No more `import React from 'react'` boilerplate in every JSX file
- Smaller bundle (jsx-runtime is more efficient than createElement)
- Slightly better performance
- Easier for beginners (no mysterious required import)

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-237](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-182

### What are default props in React?

Default props provide fallback values for props that are not passed by the parent.

**Function components** — use JavaScript default parameter syntax:

```jsx
function Button({ label = 'Click me', variant = 'primary', disabled = false }) {
  return <button className={`btn btn--${variant}`} disabled={disabled}>{label}</button>;
}

// defaultProps (legacy approach — still works but not preferred)
Button.defaultProps = { label: 'Click me', variant: 'primary' };
```

**Class components** — use `static defaultProps`:

```jsx
class Button extends React.Component {
  static defaultProps = {
    label: 'Click me',
    variant: 'primary',
  };
  render() { return <button>{this.props.label}</button>; }
}
```

Default parameter syntax in function components is preferred in modern React — it's standard JavaScript, works with TypeScript inference, and doesn't require the `.defaultProps` API.

**Related:** [REACT-008 — Props vs state](./01-fundamentals.md#react-008) | [REACT-018 — PropTypes](./01-fundamentals.md#react-018)

**Source:** [SudheerJ SDJ-178](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-183

### What is the purpose of the displayName property?

`displayName` is a string property on a component that React DevTools uses as the component's label in the component tree inspector.

```jsx
// Function components automatically use their function name
function MyButton() { ... }  // DevTools shows: MyButton

// But HOCs, forwardRef, and memo wrappers lose the name
const EnhancedButton = React.memo(React.forwardRef(function Button() { ... }));
// DevTools might show: Anonymous

// Fix by setting displayName
EnhancedButton.displayName = 'EnhancedButton';

// Or with a custom HOC
function withLogger(Component) {
  function WithLogger(props) { return <Component {...props} />; }
  WithLogger.displayName = `WithLogger(${Component.displayName || Component.name})`;
  return WithLogger;
}
```

`displayName` also appears in error stack traces when an error is thrown inside the component. Well-named components make debugging significantly faster.

**Related:** [REACT-059 — HOCs](./03-advanced.md#react-059) | [REACT-036 — forwardRef](./03-advanced.md#react-036)

**Source:** [SudheerJ SDJ-179](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-184

### What are keyed Fragments?

Regular shorthand Fragments (`<>...</>`) don't accept props and can't take a `key`. Keyed Fragments use the explicit `<React.Fragment key={...}>` syntax:

```jsx
function ListWithSections({ sections }) {
  return (
    <>
      {sections.map((section) => (
        // Key is required here — use React.Fragment, not <>
        <React.Fragment key={section.id}>
          <h2>{section.title}</h2>
          <p>{section.body}</p>
        </React.Fragment>
      ))}
    </>
  );
}
```

Without a key on the Fragment, React can't efficiently reconcile the list when sections are reordered or removed — it will produce a console warning and potentially have incorrect update behavior.

**Related:** [REACT-005 — Fragments](./01-fundamentals.md#react-005) | [REACT-006 — key prop](./01-fundamentals.md#react-006)

**Source:** [SudheerJ SDJ-182](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-185

### Does React support all HTML attributes?

React supports most HTML attributes, but uses camelCase naming and some renamed attributes:

| HTML | JSX |
|---|---|
| `class` | `className` |
| `for` | `htmlFor` |
| `tabindex` | `tabIndex` |
| `autocomplete` | `autoComplete` |
| `readonly` | `readOnly` |
| `maxlength` | `maxLength` |
| `enctype` | `encType` |
| `contenteditable` | `contentEditable` |

**Not all HTML attributes are supported** — some are intentionally omitted (e.g., `align` on table elements is deprecated) and some have React-specific replacements (`style` takes an object, not a string).

Since React 16, unknown attributes are passed through to the DOM (with a console warning), rather than silently stripped as in React 15.

`data-*` and `aria-*` attributes are always supported as-is (no camelCase needed for these).

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-161 — Custom DOM attributes](#react-161)

**Source:** [SudheerJ SDJ-183](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-186

### When do component props default to true?

When you write a JSX attribute without a value, it defaults to `true`. This is identical to HTML boolean attributes:

```jsx
// These are equivalent
<Button disabled />
<Button disabled={true} />

<Input readOnly />
<Input readOnly={true} />

<MyComponent showLabel />
<MyComponent showLabel={true} />
```

This shorthand only works for boolean props. For any prop that should be a non-boolean value, you must explicitly provide it:

```jsx
// ✗ This is disabled={true}, not disabled="auto"
<Button disabled />

// ✓ String value requires explicit JSX expression
<Button variant="primary" />
```

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-008 — Props](./01-fundamentals.md#react-008)

**Source:** [SudheerJ SDJ-184](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-187

### How does JSX prevent injection attacks?

React automatically escapes all values embedded in JSX before rendering them. Any string inserted via `{expression}` is converted to its plain text representation — HTML tags and script content become literal characters, not parsed HTML.

```jsx
const userInput = '<script>alert("XSS")</script>';

// React escapes this — renders the literal string, not a script tag
<div>{userInput}</div>
// DOM output: <div>&lt;script&gt;alert("XSS")&lt;/script&gt;</div>
```

This means you can safely render any user-provided string without manual escaping.

**The only bypass** is `dangerouslySetInnerHTML` — which is why its name is intentionally alarming:

```jsx
// ⚠️ This IS vulnerable — only use with sanitized content
<div dangerouslySetInnerHTML={{ __html: userInput }} />
```

React's escaping only applies to JSX interpolation. Dynamically setting `href` to `javascript:...` URLs is also a vector — validate URLs before using them in `href` props.

**Related:** [REACT-153 — dangerouslySetInnerHTML](#react-153)

**Source:** [SudheerJ SDJ-188](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-188

### Should keys be globally unique?

No — keys only need to be **unique among siblings** within the same list. The same key value can appear in different lists without conflict.

```jsx
// ✓ 'key-1' appears in both lists — fine, they're in separate arrays
<ul>
  {listA.map((item) => <li key={item.id}>{item.name}</li>)}
</ul>
<ul>
  {listB.map((item) => <li key={item.id}>{item.name}</li>)}
</ul>

// ✗ Duplicate keys within the same list — React warns and reconciles incorrectly
{[...items, ...items].map((item) => <li key={item.id}>{item.name}</li>)}
```

If a key is duplicated within a list, React can't tell the two elements apart and may render incorrect output when the list changes.

**Related:** [REACT-006 — key prop](./01-fundamentals.md#react-006) | [REACT-007 — Index as key](./01-fundamentals.md#react-007)

**Source:** [SudheerJ SDJ-192](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-189

### Can keys be used for non-list items?

Yes — `key` can be used on any element to force a complete remount when the key changes. This is a useful technique to reset component state without prop drilling a reset function:

```jsx
// Force ProfileForm to remount (and reset its state) when userId changes
function UserDashboard({ userId }) {
  return <ProfileForm key={userId} userId={userId} />;
}
```

When `userId` changes, React sees a different `key` and unmounts the old `ProfileForm` and mounts a fresh one — all internal state is reset. Without the `key`, React would try to update the existing instance.

Use this sparingly — remounting is heavier than updating. But it's often the cleanest solution for "reset this component when this value changes".

**Related:** [REACT-006 — key prop](./01-fundamentals.md#react-006) | [REACT-188 — Keys globally unique](#react-188)

**Source:** [SudheerJ SDJ-263](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-190

### How to pass numbers to a React component?

Numbers must be passed as JavaScript expressions (inside `{}`), not as string literals:

```jsx
// ✗ Wrong — passes the string "42", not the number 42
<Slider max="100" min="0" step="1" />

// ✓ Correct — passes actual numbers
<Slider max={100} min={0} step={1} />

// Computed value
<Grid columns={isMobile ? 1 : 3} />
```

This matters when the component does arithmetic with the prop. `"100" + 1` is `"1001"` (string concatenation), while `100 + 1` is `101`.

**Related:** [REACT-008 — Props](./01-fundamentals.md#react-008)

**Source:** [SudheerJ SDJ-161](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-191

### How to pass an event handler to a component?

Pass event handlers as props using callback functions:

```jsx
// Parent passes the handler
function Parent() {
  const handleClick = (id) => console.log('clicked:', id);

  return <ChildButton itemId={42} onClick={handleClick} />;
}

// Child calls it
function ChildButton({ itemId, onClick }) {
  return (
    <button onClick={() => onClick(itemId)}>Click</button>
  );
}
```

**Common mistake:** calling the handler instead of referencing it:

```jsx
// ✗ Calls handleClick immediately during render
<button onClick={handleClick()} />

// ✓ Passes a reference — called on click
<button onClick={handleClick} />

// ✓ Wrapping when you need to pass arguments
<button onClick={() => handleClick(id)} />
```

If the handler has no arguments, pass it directly. If you need to forward arguments, wrap it in an arrow function (but be aware this creates a new function on every render — see [REACT-029] for when that matters).

**Related:** [REACT-029 — useCallback](./02-hooks.md#react-029) | [REACT-052 — Synthetic events](./03-advanced.md#react-052)

**Source:** [SudheerJ SDJ-186](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-192

### How to prevent a function from being called multiple times?

**Throttle** (limit calls per time window) or **debounce** (delay until pause):

```jsx
import { useCallback, useRef } from 'react';

// Debounce with a ref (no external library)
function useDebounce(fn, delay) {
  const timerRef = useRef(null);
  return useCallback((...args) => {
    clearTimeout(timerRef.current);
    timerRef.current = setTimeout(() => fn(...args), delay);
  }, [fn, delay]);
}

function SearchInput() {
  const handleSearch = useDebounce((query) => {
    fetchResults(query);
  }, 300);

  return <input onChange={(e) => handleSearch(e.target.value)} />;
}
```

With `lodash`:

```js
import { debounce, throttle } from 'lodash';

const handleScroll = useCallback(throttle(() => { ... }, 200), []);
const handleSearch = useCallback(debounce((q) => search(q), 300), []);
```

For click handlers that shouldn't double-fire (e.g., form submission): disable the button after the first click, or use a `useRef` flag to track in-flight requests.

**Related:** [REACT-029 — useCallback](./02-hooks.md#react-029) | [REACT-064 — Async data patterns](./03-advanced.md#react-064)

**Source:** [SudheerJ SDJ-187](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-193

### How to print falsy values in JSX?

`false`, `null`, `undefined`, and `true` render nothing in JSX. Only `0` renders — as the literal digit "0":

```jsx
// ✗ Renders "0" as text when count is 0
{count && <List items={items} />}

// ✓ Explicit boolean conversion prevents the "0" issue
{count > 0 && <List items={items} />}
{!!count && <List items={items} />}
{Boolean(count) && <List items={items} />}

// To literally render the string "false":
{'false'}        // renders: false
{String(false)}  // renders: false
{`${false}`}     // renders: false
```

If you need to display a 0 from a variable:

```jsx
{value === 0 ? '0' : value}
// or simply
{String(value)}
```

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002) | [REACT-147 — Inline conditionals](#react-147)

**Source:** [SudheerJ SDJ-208](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-194

### Can you import an SVG file as a React component?

Yes, with Vite or Create React App (which use SVGR under the hood):

```jsx
// Vite (with @vitejs/plugin-react or vite-plugin-svgr)
import { ReactComponent as Logo } from './logo.svg';
// or in Vite with svgr configured:
import Logo from './logo.svg?react';

function Header() {
  return <Logo width={80} height={80} className="logo" />;
}
```

With CRA:

```jsx
import { ReactComponent as Logo } from './logo.svg';
<Logo />
```

SVG imported as a React component gives you full control: you can style it with CSS classes, animate paths, or change fill colors via props. Alternatively, import SVG as a URL string for use in `<img src>`:

```jsx
import logoUrl from './logo.svg';
<img src={logoUrl} alt="Logo" />
```

**Related:** [REACT-002 — JSX](./01-fundamentals.md#react-002)

**Source:** [SudheerJ SDJ-159](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-195

### How does React update the screen?

React's rendering pipeline has three phases:

**1. Trigger** — something causes a render: initial render (`createRoot().render()`), a `setState` call, a context value change, or a parent re-render.

**2. Render** — React calls your component functions (recursively, starting from the changed component) to produce a new virtual DOM tree. This is pure computation — no DOM writes happen yet.

**3. Commit** — React compares the new virtual DOM to the previous one (reconciliation/diffing) and applies only the minimal DOM mutations needed. After DOM updates, it runs `useLayoutEffect` cleanups/setups, then `useEffect` cleanups/setups.

```
State change → Component re-renders (pure, no DOM)
             → React diffs old vs new virtual DOM
             → Applies minimal DOM mutations
             → Runs effects
```

In concurrent mode (React 18+), the render phase can be interrupted and resumed — React can pause a low-priority render to handle a higher-priority update, then come back to finish.

**Related:** [REACT-003 — Virtual DOM](./01-fundamentals.md#react-003) | [REACT-012 — Reconciliation](./01-fundamentals.md#react-012) | [REACT-011 — Fiber](./01-fundamentals.md#react-011)

**Source:** [SudheerJ SDJ-252](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

