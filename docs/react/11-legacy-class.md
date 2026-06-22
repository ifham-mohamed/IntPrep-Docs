# React Legacy — Class Components & Old Patterns

> Canonical answers for REACT-268 to REACT-282.
> These cover class component patterns from React's pre-hooks era (pre-2019). Still asked in interviews when maintaining legacy codebases.
> Sources: [SudheerJ Old Q&A](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-268

### What are the different phases of a React component's lifecycle?

React components go through three broad phases:

**1. Mounting** — the component is created and inserted into the DOM:
- `constructor(props)`
- `static getDerivedStateFromProps(props, state)`
- `render()`
- `componentDidMount()` ← side effects, subscriptions, data fetching

**2. Updating** — the component re-renders due to props or state changes:
- `static getDerivedStateFromProps(props, state)`
- `shouldComponentUpdate(nextProps, nextState)`
- `render()`
- `getSnapshotBeforeUpdate(prevProps, prevState)` ← capture scroll position before DOM update
- `componentDidUpdate(prevProps, prevState, snapshot)`

**3. Unmounting** — the component is removed from the DOM:
- `componentWillUnmount()` ← cleanup: clear timers, cancel subscriptions

**Error handling (React 16+):**
- `static getDerivedStateFromError(error)` ← update state to show fallback
- `componentDidCatch(error, info)` ← log error to service

In function components, `useEffect` replaces all three phases: mount (empty deps `[]`), update (with deps), unmount (return cleanup).

**Related:** [REACT-009 — Class vs function](./01-fundamentals.md#react-009) | [REACT-026 — useEffect](./02-hooks.md#react-026)

**Source:** [SudheerJ Old Q&A SDJ-010](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-269

### How to bind methods or event handlers in class components?

Class methods don't automatically bind `this` to the component instance. Several approaches:

**1. Arrow function class field (recommended for new class code):**

```jsx
class Button extends Component {
  // Arrow function auto-binds to the instance
  handleClick = () => {
    console.log(this.state.count);
  };
  render() { return <button onClick={this.handleClick}>Click</button>; }
}
```

**2. Bind in constructor:**

```jsx
constructor(props) {
  super(props);
  this.handleClick = this.handleClick.bind(this);
}
```

**3. Arrow function inline in JSX (creates new function on every render):**

```jsx
<button onClick={() => this.handleClick()}>Click</button>
```

Arrow class fields are the standard approach — bind in constructor is verbose, inline arrow functions have the re-render overhead downside.

**Related:** [REACT-009 — Class vs functional](./01-fundamentals.md#react-009)

**Source:** [SudheerJ Old Q&A SDJ-003](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-270

### What is the purpose of the callback function argument in setState()?

In class components, `this.setState()` is asynchronous. The optional second argument is a callback that fires after the state has been applied and the component has re-rendered:

```jsx
this.setState(
  { count: this.state.count + 1 },
  () => {
    // Guaranteed to run after state is updated and DOM is committed
    console.log('New count:', this.state.count);
    this.props.onCountChange(this.state.count);
  }
);
```

**In function components:** there is no equivalent direct callback to `useState`'s setter. Use `useEffect` with the state variable as a dependency instead:

```jsx
useEffect(() => {
  // Runs after count changes and DOM is updated
  onCountChange(count);
}, [count]);
```

**Related:** [REACT-028 — setState callback pattern](./02-hooks.md#react-028)

**Source:** [SudheerJ Old Q&A SDJ-002](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-271

### What is the difference between super() and super(props) in React class components?

In a class constructor, `super()` calls the parent class constructor (`React.Component`). Passing `props` makes `this.props` available inside the constructor itself:

```jsx
class MyComponent extends Component {
  constructor(props) {
    super();           // props NOT passed — this.props is undefined here
    console.log(this.props); // undefined ← bug

    super(props);      // props passed — this.props is available
    console.log(this.props); // { name: 'Akar' } ✓
  }
}
```

**Outside the constructor**, `this.props` is always available (React sets it after construction). So the bug only manifests inside the constructor body.

Best practice: always pass `props` to `super` — `super(props)` — to avoid the confusing edge case where `this.props` is `undefined` during construction.

**Related:** [REACT-009 — Class vs functional](./01-fundamentals.md#react-009)

**Source:** [SudheerJ Old Q&A SDJ-035](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-272

### What is the purpose of getDerivedStateFromProps()?

`static getDerivedStateFromProps(props, state)` is called before every render (both on mount and updates). It returns an object to merge into state, or `null` to change nothing:

```jsx
class EmailInput extends Component {
  static getDerivedStateFromProps(props, state) {
    // Sync internal state when the external email prop changes
    if (props.email !== state.prevEmail) {
      return { email: props.email, prevEmail: props.email };
    }
    return null; // no change
  }

  state = { email: this.props.email, prevEmail: this.props.email };
  // ...
}
```

**Rarely needed.** Most use cases that seem to require `getDerivedStateFromProps` are better solved by:
- Fully controlled components (parent owns state)
- Fully uncontrolled components with a reset `key`
- Computing derived values in `render()` or `useMemo`

The React docs describe this method as a "last resort" because it makes component behavior hard to reason about.

**Related:** [REACT-009 — Class vs functional](./01-fundamentals.md#react-009) | [REACT-189 — Key to reset](./01-fundamentals.md#react-189)

**Source:** [SudheerJ Old Q&A SDJ-027](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-273

### What is the purpose of getSnapshotBeforeUpdate()?

`getSnapshotBeforeUpdate(prevProps, prevState)` is called right before the DOM is updated. Its return value is passed as the third argument (`snapshot`) to `componentDidUpdate`. Used to capture DOM information (e.g., scroll position) before React changes the DOM:

```jsx
class ChatThread extends Component {
  getSnapshotBeforeUpdate(prevProps) {
    // Before new messages are added, capture scroll position
    if (prevProps.messages.length < this.props.messages.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // Maintain scroll position after new messages are inserted
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }
}
```

In function components, use `useLayoutEffect` to read DOM layout before paint — the timing is equivalent.

**Related:** [REACT-025 — useLayoutEffect](./02-hooks.md#react-025)

**Source:** [SudheerJ Old Q&A SDJ-028](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-274

### Why are String Refs legacy in React?

String refs (`ref="myRef"`) were the original ref API and are now removed (React 19):

```jsx
// ✗ String ref — legacy, removed in React 19
class OldInput extends Component {
  handleFocus() {
    this.refs.input.focus(); // Accessed via this.refs
  }
  render() { return <input ref="input" />; }
}
```

Problems with string refs:
- They require the component to have a `this` context — can't be used in functional contexts
- `this.refs` is populated by React's internal bookkeeping — creates performance overhead
- No good composition story — you can't pass the ref around
- Didn't support function refs or `createRef` patterns

**Modern replacements:**

```jsx
// createRef (class components)
this.inputRef = React.createRef();
<input ref={this.inputRef} />
this.inputRef.current.focus();

// Callback ref
<input ref={(el) => { this.input = el; }} />

// useRef (function components)
const inputRef = useRef(null);
<input ref={inputRef} />
```

**Related:** [REACT-027 — useRef](./02-hooks.md#react-027)

**Source:** [SudheerJ Old Q&A SDJ-009](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-275

### Can you force a React component to re-render without calling setState?

In class components, `this.forceUpdate()` forces a re-render, bypassing `shouldComponentUpdate`:

```jsx
class Clock extends Component {
  componentDidMount() {
    this.timer = setInterval(() => this.forceUpdate(), 1000);
  }
  componentWillUnmount() {
    clearInterval(this.timer);
  }
  render() {
    return <p>{new Date().toLocaleTimeString()}</p>;
  }
}
```

**Avoid `forceUpdate`** — it's a sign that state or props should drive the render. Reading from external mutable sources (like `new Date()`) should be done via state updates.

**In function components:** there's no `forceUpdate` equivalent. The standard pattern is a dummy state counter:

```jsx
const [, rerender] = useState(0);
const forceUpdate = () => rerender(n => n + 1);
```

Or better — use `useSyncExternalStore` for external state that's not managed by React.

**Related:** [REACT-222 — useSyncExternalStore](./02-hooks.md#react-222) | [REACT-035 — Re-rendering](./03-advanced.md#react-035)

**Source:** [SudheerJ Old Q&A SDJ-034](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-276

### What is the difference between setState() and replaceState()?

`replaceState()` was a method on `React.createClass` components (pre-ES6). It replaced the entire state object rather than merging:

```jsx
// setState — merges into existing state
this.setState({ name: 'Akar' }); // { count: 0, name: 'Akar' }

// replaceState — replaces entire state (old React.createClass only)
this.replaceState({ name: 'Akar' }); // { name: 'Akar' } — count is gone
```

`replaceState` was removed when ES6 class components were introduced. Class components and hooks both use `setState` / `useState` with merging semantics.

**Note:** Class component `this.setState()` does shallow merging. Function component `useState`'s setter does NOT merge — it replaces. For object state in function components, spread manually:

```jsx
setUser(prev => ({ ...prev, name: 'Akar' })); // spread to merge
```

**Related:** [REACT-009 — Class vs functional](./01-fundamentals.md#react-009)

**Source:** [SudheerJ Old Q&A SDJ-036](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-277

### How to create props proxy for an HOC?

A props proxy HOC intercepts the wrapped component's props — it can add, remove, rename, or transform them:

```jsx
function withExtraProps(WrappedComponent) {
  return function WithExtraProps(props) {
    const extraProps = {
      theme: 'dark',
      onError: (err) => logError(err),
    };

    // Filter out HOC-specific props, pass the rest + extras
    const { sensitiveData, ...cleanProps } = props;
    return <WrappedComponent {...cleanProps} {...extraProps} />;
  };
}

// Usage
const EnhancedCard = withExtraProps(Card);
<EnhancedCard title="Hello" sensitiveData="secret" />
// Card receives: { title: 'Hello', theme: 'dark', onError: fn }
```

Props proxy is one of two HOC implementation strategies. The other is **inheritance inversion** (the HOC extends the wrapped component to control its render — see render hijacking [REACT-226]).

**Related:** [REACT-059 — HOCs](./03-advanced.md#react-059) | [REACT-226 — Render hijacking](./03-advanced.md#react-226)

**Source:** [SudheerJ Old Q&A SDJ-012](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-278

### What is the difference between try/catch and error boundaries?

| Aspect | try/catch | Error boundaries |
|---|---|---|
| Catches render errors | ✗ (can't wrap JSX) | ✅ |
| Catches event handler errors | ✅ | ✗ |
| Catches async errors | ✅ (in async functions) | ✗ |
| React lifecycle errors | ✗ | ✅ |
| UI fallback on error | Manual | Declarative via `fallback` prop |
| Scope | Specific code block | Entire component subtree |

```jsx
// try/catch — works for event handlers, async code
handleSubmit = async () => {
  try {
    await submitForm(this.state);
  } catch (err) {
    this.setState({ error: err.message });
  }
};

// Error boundary — catches render/lifecycle errors
<ErrorBoundary fallback={<ErrorPage />}>
  <ComplexForm />
</ErrorBoundary>
```

Use both: error boundaries for render-phase errors, `try/catch` for event handlers and async logic.

**Related:** [REACT-037 — Error boundaries](./03-advanced.md#react-037) | [REACT-230 — Error boundary limits](./03-advanced.md#react-230)

**Source:** [SudheerJ Old Q&A SDJ-059](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-279

### What are the lifecycle methods that were deprecated in React?

Three lifecycle methods were deprecated in React 16.3 and prefixed with `UNSAFE_` because they were frequently misused, especially with concurrent mode:

| Deprecated | UNSAFE_ replacement | Why deprecated |
|---|---|---|
| `componentWillMount` | `UNSAFE_componentWillMount` | Called multiple times in concurrent mode; use constructor or componentDidMount |
| `componentWillReceiveProps` | `UNSAFE_componentWillReceiveProps` | Use `getDerivedStateFromProps` or `componentDidUpdate` |
| `componentWillUpdate` | `UNSAFE_componentWillUpdate` | Use `getSnapshotBeforeUpdate` |

```jsx
// ✗ Deprecated — will be removed in a future major version
componentWillMount() { ... }
componentWillReceiveProps(nextProps) { ... }

// ✓ Use these instead
componentDidMount() { ... }
static getDerivedStateFromProps(props, state) { ... }
componentDidUpdate(prevProps) { ... }
```

These methods are still in React but the `UNSAFE_` prefix signals intent to remove them. In concurrent React, these methods can be called more than once — making them dangerous for side effects like API calls or subscriptions.

**Related:** [REACT-268 — Lifecycle phases](#react-268)

**Source:** [SudheerJ Old Q&A SDJ-026](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-280

### What is the recommended ordering of methods in a class component?

The React community convention (from Airbnb's style guide, widely adopted):

```jsx
class MyComponent extends React.Component {
  // 1. Static methods and properties
  static propTypes = { ... };
  static defaultProps = { ... };
  static getDerivedStateFromProps() { ... }

  // 2. Instance properties (state, refs, bound methods)
  state = { ... };
  myRef = React.createRef();

  // 3. Lifecycle methods (in order of execution)
  constructor(props) { ... }
  componentDidMount() { ... }
  shouldComponentUpdate() { ... }
  getSnapshotBeforeUpdate() { ... }
  componentDidUpdate() { ... }
  componentWillUnmount() { ... }
  componentDidCatch() { ... }

  // 4. Custom methods (event handlers, utilities)
  handleClick = () => { ... };
  fetchData = () => { ... };

  // 5. render() — always last
  render() { return <div>...</div>; }
}
```

The `render()` method last is the most important rule — it makes the component easy to scan since the output is always at the bottom.

**Related:** [REACT-268 — Lifecycle phases](#react-268)

**Source:** [SudheerJ Old Q&A SDJ-030](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-281

### What is context in the old React API (pre-16.3)?

The pre-16.3 Context API used `childContextTypes`, `getChildContext`, and `contextTypes` — a pattern that was difficult to type and caused issues with `shouldComponentUpdate`:

```jsx
// Legacy context provider
class ThemeProvider extends Component {
  static childContextTypes = {
    theme: PropTypes.string,
  };
  getChildContext() {
    return { theme: 'dark' };
  }
  render() { return this.props.children; }
}

// Legacy context consumer
class ThemedButton extends Component {
  static contextTypes = {
    theme: PropTypes.string,
  };
  render() {
    return <button className={this.context.theme}>Click</button>;
  }
}
```

**Why it was replaced (React 16.3):** The old API broke if any component in the tree had `shouldComponentUpdate` returning `false` — context updates wouldn't propagate through optimized components.

The new API (`createContext`, `Provider`, `useContext`) fixes this — context always propagates regardless of `shouldComponentUpdate` or `React.memo`.

**Related:** [REACT-033 — useContext](./02-hooks.md#react-033)

**Source:** [SudheerJ Old Q&A SDJ-013, SDJ-075, SDJ-076, SDJ-077](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-282

### How to set state with a dynamic key name in React?

Use computed property names (ES6) in `setState`:

```jsx
// Class component — dynamic key via computed property
handleChange = (fieldName, value) => {
  this.setState({ [fieldName]: value });
};

// Usage
this.handleChange('email', 'akar@example.com');
this.handleChange('password', 'secret123');

// Function component equivalent
const [form, setForm] = useState({ email: '', password: '' });

const handleChange = (fieldName, value) => {
  setForm(prev => ({ ...prev, [fieldName]: value }));
};

// Generic input handler using event.target.name
const handleInput = (e) => {
  const { name, value } = e.target;
  setForm(prev => ({ ...prev, [name]: value }));
};
```

The `[fieldName]` syntax creates an object with a dynamic key — the value of `fieldName` is evaluated at runtime. This is the standard pattern for building generic form handlers that work for any field.

**Related:** [REACT-248 — Update objects in state](./03-advanced.md#react-248)

**Source:** [SudheerJ Old Q&A SDJ-015](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

*← [10-libraries.md](./10-libraries.md) | [Master Index →](../../docs/reference/00-master-index.md)*
