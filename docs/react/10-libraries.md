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

---

## REACT-262

### How to use Font Awesome icons in React?

Install the official React Font Awesome packages:

```bash
npm install @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome
```

```jsx
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faCoffee, faHome, faUser } from '@fortawesome/free-solid-svg-icons';

function App() {
  return (
    <div>
      <FontAwesomeIcon icon={faCoffee} />
      <FontAwesomeIcon icon={faHome} size="2x" color="blue" />
      <FontAwesomeIcon icon={faUser} spin />
      <FontAwesomeIcon icon={faCoffee} className="icon-custom" />
    </div>
  );
}
```

Font Awesome renders SVGs, not icon fonts — better accessibility and no network request for a font file. Individual icon imports enable tree-shaking so only used icons are bundled.

**Related:** [REACT-001 — React overview](../react/01-fundamentals.md#react-001)

**Source:** [SudheerJ SDJ-143](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-263

### Why does React DevTools not load in Chrome for local files?

Chrome restricts extension access to `file://` URLs by default. The React DevTools extension can't inspect pages opened as local files (`file:///path/to/index.html`).

**Fix — enable file access:**
1. Go to `chrome://extensions/`
2. Find "React Developer Tools"
3. Click "Details"
4. Enable "Allow access to file URLs"

**Alternative fixes:**
- Serve the file through a local HTTP server instead of opening directly: `npx serve .` or `python3 -m http.server`
- Use Vite's dev server (`npm run dev`) — runs at `localhost`, always accessible

For production debugging, React DevTools requires the development build of React. Production builds don't expose the hook React DevTools uses.

**Related:** [REACT-143 — React DevTools](./10-libraries.md#react-143)

**Source:** [SudheerJ SDJ-145](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-264

### Why does the React tab not show up in DevTools?

Common causes:

1. **Production build** — React DevTools only works with development builds. Production builds don't expose the DevTools hook. Run `npm run dev` or check your build config.

2. **Stale cache / reload needed** — open DevTools before the page loads, or hard-refresh (`Cmd+Shift+R`).

3. **Extension not updated** — ensure React DevTools extension is up to date. Older versions don't support React 18/19 APIs.

4. **iframe or cross-origin content** — DevTools may not detect React inside cross-origin iframes.

5. **Custom renderer** — React DevTools detects React via a global hook (`__REACT_DEVTOOLS_GLOBAL_HOOK__`). Custom renderers or unusual setups may not expose this.

6. **Electron / non-Chrome environment** — use the standalone `react-devtools` npm package: `npx react-devtools`.

**Related:** [REACT-143 — React DevTools](./10-libraries.md#react-143) | [REACT-263 — DevTools local files](#react-263)

**Source:** [SudheerJ SDJ-149](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-265

### What are the advantages of React over Vue.js?

This comparison depends on context, but commonly cited React advantages:

**Ecosystem size.** React has a larger ecosystem — more libraries, more tutorials, more job market demand, and broader community support.

**JavaScript-centric.** React uses JSX and plain JavaScript. Vue uses `.vue` single-file components with its own template syntax, directives, and options API — more to learn.

**Flexibility.** React is a library; you choose your own router, state manager, and form library. Vue is more opinionated but also provides more out of the box (Vue Router, Pinia are official).

**TypeScript integration.** React + TypeScript is extremely well-supported and widely adopted. Vue's TypeScript support has improved but JSX + TS is more mature in React.

**Server Components.** React 19's RSC model has no Vue equivalent and enables new full-stack patterns.

**Vue advantages:** Simpler learning curve, official integration of routing/state, cleaner template syntax for HTML-oriented developers, smaller initial bundle.

Neither is objectively better — both are excellent choices.

**Source:** [SudheerJ SDJ-147](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-266

### What is the difference between React and Angular?

| Aspect | React | Angular |
|---|---|---|
| Type | UI library | Full framework |
| Language | JavaScript + JSX | TypeScript (required) |
| Data binding | One-way | Two-way (ngModel) |
| DOM | Virtual DOM | Real DOM with change detection |
| Architecture | Flexible — you choose | Opinionated — routing, HTTP, forms built-in |
| Learning curve | Moderate | Steep (decorators, DI, modules, zones) |
| Bundle size | Smaller | Larger |
| State management | Third-party (Redux, Zustand) | Services + NgRx (optional) |
| Updates | Hooks-based | Zone.js + signals (v17+) |
| Mobile | React Native | Ionic (different ecosystem) |

React is a library that focuses on the view layer. Angular is a complete framework that includes routing, HTTP client, forms, animations, DI container, and testing infrastructure.

React is more flexible and has a larger ecosystem. Angular provides more structure, which some teams prefer for large enterprise projects with many developers.

**Source:** [SudheerJ SDJ-148](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-267

### What is Formik?

Formik is a form management library for React that handles form state, validation, and submission in a structured, controlled way.

```jsx
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';

const schema = Yup.object({ email: Yup.string().email().required() });

function LoginForm() {
  return (
    <Formik
      initialValues={{ email: '' }}
      validationSchema={schema}
      onSubmit={(values, { setSubmitting }) => {
        submitLogin(values).finally(() => setSubmitting(false));
      }}
    >
      {({ isSubmitting }) => (
        <Form>
          <Field type="email" name="email" placeholder="Email" />
          <ErrorMessage name="email" component="p" />
          <button type="submit" disabled={isSubmitting}>Login</button>
        </Form>
      )}
    </Formik>
  );
}
```

Formik manages: field values, touched state, validation errors (via Yup or custom), and submission loading state. It uses controlled inputs and re-renders on every change.

**vs React Hook Form:** Formik is more verbose but explicit; React Hook Form uses uncontrolled inputs and re-renders far less. React Hook Form is generally preferred for performance-sensitive forms.

**Related:** [REACT-253 — Popular form libraries](./03-advanced.md#react-253) | [REACT-131 — Redux Form](./08-redux.md#react-131)

**Source:** [SudheerJ SDJ-223, SDJ-194](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)


---

## REACT-283

### How to use Polymer in React?

Polymer was a web components library developed by Google (now largely superseded by Lit). Integrating Polymer elements into a React application requires wrapping them in a React component because React does not natively understand web component event semantics.

```jsx
import React, { useRef, useEffect } from 'react';
// Assume the Polymer element is imported via its package
// import '@polymer/paper-button/paper-button.js';

function PolymerButton({ label, onClick }) {
  const ref = useRef(null);

  useEffect(() => {
    const el = ref.current;
    // Polymer elements fire custom DOM events — React synthetic
    // events won't catch them, so attach natively
    el.addEventListener('tap', onClick);
    return () => el.removeEventListener('tap', onClick);
  }, [onClick]);

  return <paper-button ref={ref}>{label}</paper-button>;
}
```

Key points:
- Polymer elements are custom HTML elements — React renders them as unknown DOM elements, which works in modern browsers.
- Custom events (`tap`, `iron-select`, etc.) must be attached imperatively with `addEventListener` inside `useEffect`, not via JSX props.
- Properties (not attributes) must also be set via refs when dealing with complex objects.
- **Recommendation:** Polymer is deprecated in favour of [Lit](https://lit.dev/). For new projects, prefer Lit or native web components + React wrapper patterns. React 19 improves web component interop natively.

**Related:** [REACT-265 — React vs Vue.js](./10-libraries.md#react-265) | [REACT-266 — React vs Angular](./10-libraries.md#react-266)

**Source:** [SudheerJ SDJ-146](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-285

**Q: What is Immer and how do you use it for state updates in React?**

**Immer** is a library that lets you write mutating-style code for state updates while producing a correct immutable result under the hood. You pass it a `produce(baseState, recipe)` function where `recipe` receives a `draft` you can mutate directly.

```jsx
import { produce } from 'immer';
import { useState } from 'react';

const initialItems = [
  { id: 1, label: 'Apple', checked: false },
  { id: 2, label: 'Banana', checked: false },
];

export default function ShoppingList() {
  const [items, setItems] = useState(initialItems);

  // Toggle one item — no spread gymnastics needed
  const toggleItem = (id) => {
    setItems(produce(items, (draft) => {
      const item = draft.find((i) => i.id === id);
      if (item) item.checked = !item.checked;
    }));
  };

  // Select all / deselect all
  const toggleAll = (checked) => {
    setItems(produce(items, (draft) => {
      draft.forEach((item) => { item.checked = checked; });
    }));
  };

  const allChecked = items.every((i) => i.checked);

  return (
    <ul>
      <li>
        <input type="checkbox" checked={allChecked} onChange={(e) => toggleAll(e.target.checked)} />
        <strong> Select All</strong>
      </li>
      {items.map((item) => (
        <li key={item.id}>
          <input type="checkbox" checked={item.checked} onChange={() => toggleItem(item.id)} />
          {item.label}
        </li>
      ))}
    </ul>
  );
}
```

**Why Immer over manual spread:**

| Scenario | Manual spread | Immer |
|---|---|---|
| Nested object update | `{ ...state, user: { ...state.user, name: 'x' } }` | `draft.user.name = 'x'` |
| Array item toggle | `items.map(i => i.id === id ? {...i, checked: !i.checked} : i)` | `item.checked = !item.checked` |
| Readability at depth | ❌ Very verbose | ✅ Reads like mutation |
| Immutability guarantee | ✅ Manual discipline | ✅ Enforced by Immer |

**Immer with `useReducer`** (canonical pattern for complex state):

```jsx
import { produce } from 'immer';

const reducer = produce((draft, action) => {
  switch (action.type) {
    case 'TOGGLE':
      const item = draft.find((i) => i.id === action.id);
      if (item) item.checked = !item.checked;
      break;
    case 'SET_ALL':
      draft.forEach((i) => { i.checked = action.checked; });
      break;
  }
});

// Pass produce-wrapped reducer directly to useReducer
const [items, dispatch] = useReducer(reducer, initialItems);
```

**Install:** `npm install immer`

**Related:** [REACT-022 — Why not mutate state directly](./01-fundamentals.md#react-022) | [REACT-116 — useReducer](./02-hooks.md#react-116) | [REACT-144 — Styled Components](./10-libraries.md#react-144)

**Source:** [CodeDam 10 React Problems CDM-001 Q2/Q3](../../sources/react/youtube/codedam-10-react-problems/question-map.md)
