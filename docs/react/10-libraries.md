# React Libraries & Integration

> Canonical answers for REACT-140 to REACT-144.
> Sources: [SudheerJ](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-140

### What is Reselect and how does it work?

Reselect is a selector library for Redux (now bundled into Redux Toolkit as `createSelector`). It creates memoized selectors that recompute only when their input selectors return new values.

**Problem without Reselect:** every `useSelector` call with a derived value (filter, sort, map) runs on every store update, even if the relevant data didn't change:

```js
// Recalculates on every render / store update
const expensiveItems = useSelector((state) =>
  state.cart.items.filter((item) => item.price > 100)
);
```

**With Reselect:**

```js
import { createSelector } from '@reduxjs/toolkit';

const selectItems = (state) => state.cart.items;

// Memoized — only recalculates when selectItems returns a different reference
export const selectExpensiveItems = createSelector(
  [selectItems],
  (items) => items.filter((item) => item.price > 100)
);

// In component
const expensiveItems = useSelector(selectExpensiveItems);
```

**How memoization works:** `createSelector` stores the last arguments and last result. If the inputs haven't changed (by reference equality), it returns the cached result — preventing the array from being re-created and triggering an unnecessary re-render.

**Composing selectors:**

```js
const selectCartTotal = createSelector(
  [selectExpensiveItems, selectDiscount],
  (items, discount) =>
    items.reduce((sum, i) => sum + i.price, 0) * (1 - discount)
);
```

**Related:** [REACT-130 — Redux selectors](./08-redux.md#react-130) | [REACT-030 — useMemo](./02-hooks.md#react-030)

**Source:** [SudheerJ SDJ-140](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-141

### What is Flow?

Flow is a static type checker for JavaScript developed by Facebook (Meta). It adds type annotations to plain JavaScript and checks them at compile time, catching bugs before they reach runtime.

```js
// @flow
function greet(name: string): string {
  return `Hello, ${name}`;
}

greet(42); // Flow error: number is incompatible with string
```

Flow integrates with React via typed props and state:

```js
// @flow
type Props = { name: string, age: number };
type State = { count: number };

class UserCard extends React.Component<Props, State> { /* ... */ }
```

Flow runs incrementally — you opt in file by file with `// @flow`. It was React's original type system (class components had `Props` and `State` type parameters designed for Flow).

**Today:** TypeScript has become the dominant choice for new projects. Flow's ecosystem and tooling have not kept pace, and most popular libraries now ship TypeScript declarations. Meta itself has migrated many codebases to TypeScript.

**Related:** [REACT-142 — Flow vs PropTypes](#react-142) | [REACT-021 — Static type checking](./01-fundamentals.md#react-021)

**Source:** [SudheerJ SDJ-141](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-142

### What is the difference between Flow and PropTypes?

Both catch type-related bugs in React, but at different phases and with different capabilities:

| Aspect | Flow | PropTypes |
|---|---|---|
| When it runs | Compile time (static analysis) | Runtime (development only) |
| Scope | Entire JS codebase | React component props only |
| Types | Full type system (generics, unions, intersections) | Limited type checkers (`PropTypes.string`, `arrayOf`, etc.) |
| Error surfacing | Editor / CI | Browser console warning |
| Performance | No runtime cost | Small runtime overhead (stripped in prod) |
| Depth | Can type state, functions, modules, APIs | Props only |

PropTypes are simpler to adopt and have zero setup — `import PropTypes from 'prop-types'` and go. They're helpful for quick validation and documentation, especially in JavaScript-only codebases.

Flow and TypeScript offer deeper safety guarantees because they check types before the code runs. PropTypes are a weaker substitute — they only fire when a component actually renders with bad data.

**Modern consensus:** Use TypeScript (not Flow) for full type safety, and drop PropTypes in TS projects since TypeScript's component prop types provide equivalent guarantees at compile time.

**Related:** [REACT-141 — Flow](#react-141) | [REACT-018 — PropTypes](./01-fundamentals.md#react-018) | [REACT-021 — Static type checking](./01-fundamentals.md#react-021)

**Source:** [SudheerJ SDJ-142](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-143

### What is React DevTools?

React DevTools is a browser extension (Chrome, Firefox, Edge) and standalone app that provides introspection into React component trees. It is an essential debugging tool for React development.

**Core features:**

**Components tab:** Browse the component tree, inspect props and state for any component, edit state and props live to see how the UI responds.

**Profiler tab:** Record rendering sessions and view a flamegraph showing which components rendered, how long each took, and why they rendered. Identify performance bottlenecks.

**Highlight updates:** Flash a colored border around components when they re-render — a quick visual to spot unnecessary renders.

**Hooks inspection:** See all hooks for a function component (useState, useEffect, custom hooks) and their current values.

**Filter components:** Search/filter the component tree by name or type.

```bash
# Install as standalone (no browser needed — for React Native)
npm install -g react-devtools
react-devtools
```

In production builds, DevTools shows a message: "This page is using the production build of React. React DevTools is only available in development mode." Enabling DevTools in production requires a custom build.

**Related:** [REACT-035 — Re-rendering](./03-advanced.md#react-035) | [REACT-129 — Redux DevTools](./08-redux.md#react-129)

**Source:** [SudheerJ SDJ-144](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-144

### What are Styled Components?

Styled Components is a CSS-in-JS library that lets you write actual CSS syntax inside JavaScript using tagged template literals. Each styled component is a React component with styles scoped to it automatically.

```jsx
import styled from 'styled-components';

const Button = styled.button`
  background: ${(props) => (props.$primary ? '#6200ea' : 'white')};
  color: ${(props) => (props.$primary ? 'white' : '#6200ea')};
  padding: 0.5rem 1.5rem;
  border: 2px solid #6200ea;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;

  &:hover {
    opacity: 0.9;
  }
`;

function App() {
  return (
    <>
      <Button>Secondary</Button>
      <Button $primary>Primary</Button>
    </>
  );
}
```

**Key benefits:**
- **Scoped styles** — class names are auto-generated hashes; no conflicts between components
- **Dynamic styles** — access component props directly in CSS
- **No dead CSS** — styles are removed with the component
- **Theming** — `ThemeProvider` passes a theme object to all styled components in the tree

**Trade-offs:**
- Runtime overhead (CSS is generated and injected at render time)
- SSR requires setup for critical CSS extraction
- Style debugging can be harder (auto-generated class names)

Alternatives in the same space: Emotion, vanilla-extract, CSS Modules, Tailwind CSS.

**Related:** [REACT-043 — Context performance](./03-advanced.md#react-043)

**Source:** [SudheerJ SDJ-150, SDJ-151](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

*← [09-react-native.md](./09-react-native.md) | [Master Index →](../../docs/reference/00-master-index.md)*
