# React Testing

> **Canonical Knowledge Base** | Category: Testing | IDs: REACT-087 – REACT-100
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-087 — How do you test React applications?](#react-087)
- [REACT-088 — What is Jest and how is it used for testing React?](#react-088)
- [REACT-089 — What is React Testing Library?](#react-089)
- [REACT-090 — How do you test React components with RTL?](#react-090)
- [REACT-091 — How do you test asynchronous code in React components?](#react-091)
- [REACT-092 — How do you mock API calls in React tests?](#react-092)
- [REACT-093 — How do you test React hooks in functional components?](#react-093)
- [REACT-094 — How do you test custom hooks?](#react-094)
- [REACT-095 — What is Shallow Renderer in React testing?](#react-095)
- [REACT-096 — What is Snapshot Testing in React?](#react-096)
- [REACT-097 — How do you test components that use context?](#react-097)
- [REACT-098 — How do you test components that use Redux?](#react-098)
- [REACT-099 — What are the key differences between shallow and full DOM rendering?](#react-099)
- [REACT-100 — What is the TestRenderer package in React?](#react-100)

---

## REACT-087

### How do you test React applications?

React testing follows the principle: **test behaviour, not implementation details**. Users don't care which functions are called — they care that clicking a button does what it should.

**Testing pyramid for React:**

```
        [E2E tests] — Playwright, Cypress
           (few, slow, high confidence)

      [Integration tests] — RTL + Jest
        (components working together)

    [Unit tests] — Jest, RTL renderHook
      (individual components, hooks, utils)
```

**Standard toolchain:**
- **Jest** — test runner, assertion library, mocking.
- **React Testing Library (RTL)** — renders components and queries the DOM like a user would.
- **MSW (Mock Service Worker)** — intercepts HTTP requests at the network level for realistic API mocking.
- **Vitest** — fast Jest-compatible alternative, popular with Vite projects.
- **Playwright / Cypress** — full browser E2E testing.

**What to test:**
- Component renders correctly with given props.
- User interactions (click, type, submit) trigger correct behaviour.
- Async operations display loading, success, and error states.
- Custom hooks return correct values and update correctly.
- Edge cases — empty state, long text, missing optional props.

**What NOT to test:**
- Internal implementation details (which functions were called, internal state values).
- Styling (use visual regression tools instead).
- Third-party library behaviour.

**Related:** [REACT-088 Jest](#react-088) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q87

---

## REACT-088

### What is Jest and how is it used for testing React applications?

**Jest** is a JavaScript testing framework developed by Meta. It provides a test runner, assertion library, and built-in mocking — making it the standard for React unit and integration tests.

**Key features:**
- `describe` / `it` / `test` for test organization.
- `expect` with a rich matcher API (`toBe`, `toEqual`, `toContain`, `toBeInTheDocument`, etc.).
- Automatic mocking of modules.
- `jest.fn()` for spy/mock functions.
- Snapshot testing.
- Code coverage reports.
- jsdom environment for simulating a browser DOM.

**Setup (Create React App / Vite):** Jest is pre-configured in CRA; Vite projects often use Vitest.

**Basic example:**
```jsx
// utils/formatPrice.js
export function formatPrice(amount, currency = 'USD') {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency }).format(amount);
}

// utils/formatPrice.test.js
import { formatPrice } from './formatPrice';

describe('formatPrice', () => {
  it('formats USD by default', () => {
    expect(formatPrice(9.99)).toBe('$9.99');
  });

  it('formats EUR when specified', () => {
    expect(formatPrice(9.99, 'EUR')).toBe('€9.99');
  });

  it('handles zero', () => {
    expect(formatPrice(0)).toBe('$0.00');
  });
});
```

**Component test with RTL:**
```jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Counter from './Counter';

test('increments count on button click', async () => {
  render(<Counter />);
  const button = screen.getByRole('button', { name: /increment/i });
  await userEvent.click(button);
  expect(screen.getByText('1')).toBeInTheDocument();
});
```

**Related:** [REACT-089 RTL](#react-089) | [REACT-092 Mocking APIs](#react-092)

**Source:** GreatFrontEnd Q88

---

## REACT-089

### What is React Testing Library and how is it used?

**React Testing Library (RTL)** is a testing utility that renders React components into a real DOM (jsdom) and provides queries that reflect how users find elements — by accessible role, label, text, and placeholder — rather than by CSS selectors or component internals.

**Core philosophy:** "The more your tests resemble the way your software is used, the more confidence they can give you." — Kent C. Dodds

**Install:**
```bash
npm install --save-dev @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

**Query priority (use in this order):**
1. `getByRole` — most accessible, finds by ARIA role.
2. `getByLabelText` — finds form fields by label.
3. `getByPlaceholderText`
4. `getByText`
5. `getByDisplayValue`
6. `getByAltText`, `getByTitle`
7. `getByTestId` — last resort; use `data-testid`.

**Query variants:**
- `getBy*` — throws if not found (sync).
- `queryBy*` — returns `null` if not found (use for asserting absence).
- `findBy*` — returns a Promise, waits for element to appear (async).

**Complete component test:**
```jsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LoginForm from './LoginForm';

describe('LoginForm', () => {
  it('shows error when submitting empty email', async () => {
    render(<LoginForm onSubmit={jest.fn()} />);

    await userEvent.click(screen.getByRole('button', { name: /sign in/i }));

    expect(screen.getByText(/email is required/i)).toBeInTheDocument();
  });

  it('calls onSubmit with email and password', async () => {
    const onSubmit = jest.fn();
    render(<LoginForm onSubmit={onSubmit} />);

    await userEvent.type(screen.getByLabelText(/email/i), 'user@example.com');
    await userEvent.type(screen.getByLabelText(/password/i), 'secret123');
    await userEvent.click(screen.getByRole('button', { name: /sign in/i }));

    expect(onSubmit).toHaveBeenCalledWith({
      email: 'user@example.com',
      password: 'secret123',
    });
  });
});
```

**Related:** [REACT-088 Jest](#react-088) | [REACT-090 Testing components](#react-090) | [REACT-094 Testing hooks](#react-094)

**Source:** GreatFrontEnd Q89

---

## REACT-090

### How do you test React components using React Testing Library?

**Pattern: Arrange → Act → Assert**

**Testing a list component:**
```jsx
import { render, screen } from '@testing-library/react';
import ProductList from './ProductList';

const mockProducts = [
  { id: 1, name: 'Widget', price: 9.99 },
  { id: 2, name: 'Gadget', price: 24.99 },
];

describe('ProductList', () => {
  it('renders all products', () => {
    render(<ProductList products={mockProducts} />);

    expect(screen.getByText('Widget')).toBeInTheDocument();
    expect(screen.getByText('Gadget')).toBeInTheDocument();
    expect(screen.getAllByRole('listitem')).toHaveLength(2);
  });

  it('shows empty state when no products', () => {
    render(<ProductList products={[]} />);
    expect(screen.getByText(/no products available/i)).toBeInTheDocument();
  });
});
```

**Testing conditional rendering:**
```jsx
it('shows edit button only for owners', () => {
  render(<Post post={post} currentUserId={post.authorId} />);
  expect(screen.getByRole('button', { name: /edit/i })).toBeInTheDocument();
});

it('hides edit button for non-owners', () => {
  render(<Post post={post} currentUserId="other-user" />);
  expect(screen.queryByRole('button', { name: /edit/i })).not.toBeInTheDocument();
});
```

**Testing form submission:**
```jsx
it('submits form data correctly', async () => {
  const handleSubmit = jest.fn();
  render(<ContactForm onSubmit={handleSubmit} />);

  await userEvent.type(screen.getByLabelText(/name/i), 'Akar');
  await userEvent.type(screen.getByLabelText(/message/i), 'Hello!');
  await userEvent.click(screen.getByRole('button', { name: /send/i }));

  expect(handleSubmit).toHaveBeenCalledTimes(1);
  expect(handleSubmit).toHaveBeenCalledWith({ name: 'Akar', message: 'Hello!' });
});
```

**Testing with cleanup:** RTL automatically calls `cleanup()` after each test (unmounts and clears the DOM) when using Jest.

**Related:** [REACT-089 RTL](#react-089) | [REACT-091 Async tests](#react-091) | [REACT-097 Context in tests](#react-097)

**Source:** GreatFrontEnd Q90

---

## REACT-091

### How do you test asynchronous code in React components?

Use `findBy*` queries (return Promises) and `waitFor` to handle async state updates.

**Testing a component that fetches data:**
```jsx
import { render, screen } from '@testing-library/react';
import UserProfile from './UserProfile';

// Mock the fetch module
jest.mock('../api/users');
import { fetchUser } from '../api/users';

describe('UserProfile', () => {
  it('displays user data after loading', async () => {
    fetchUser.mockResolvedValueOnce({ name: 'Akar', role: 'Engineer' });

    render(<UserProfile userId="123" />);

    // Loading state
    expect(screen.getByText(/loading/i)).toBeInTheDocument();

    // Wait for async update — findByText waits up to 1000ms
    expect(await screen.findByText('Akar')).toBeInTheDocument();
    expect(screen.getByText('Engineer')).toBeInTheDocument();
    expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
  });

  it('shows error state on failure', async () => {
    fetchUser.mockRejectedValueOnce(new Error('Network error'));

    render(<UserProfile userId="123" />);

    expect(await screen.findByText(/something went wrong/i)).toBeInTheDocument();
  });
});
```

**`waitFor` — polling until assertion passes:**
```jsx
import { waitFor } from '@testing-library/react';

it('updates count after debounced input', async () => {
  render(<DebouncedSearch />);

  await userEvent.type(screen.getByRole('searchbox'), 'react');

  await waitFor(() => {
    expect(screen.getByText(/5 results/i)).toBeInTheDocument();
  }, { timeout: 2000 });
});
```

**Testing with MSW (Mock Service Worker) — most realistic:**
```jsx
import { setupServer } from 'msw/node';
import { http, HttpResponse } from 'msw';

const server = setupServer(
  http.get('/api/users/:id', ({ params }) => {
    return HttpResponse.json({ id: params.id, name: 'Akar' });
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

it('loads and displays user', async () => {
  render(<UserProfile userId="1" />);
  expect(await screen.findByText('Akar')).toBeInTheDocument();
});
```

**Related:** [REACT-092 Mocking APIs](#react-092) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q91

---

## REACT-092

### How do you mock API calls in React component tests?

**Option 1 — `jest.fn()` / `jest.mock()` (simplest):**
```jsx
// Mock the entire api module
jest.mock('../services/api');
import * as api from '../services/api';

test('displays fetched data', async () => {
  api.getUsers.mockResolvedValueOnce([{ id: 1, name: 'Alice' }]);

  render(<UserList />);

  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(api.getUsers).toHaveBeenCalledTimes(1);
});
```

**Option 2 — Mock global `fetch`:**
```jsx
global.fetch = jest.fn(() =>
  Promise.resolve({
    ok: true,
    json: () => Promise.resolve({ name: 'Alice' }),
  })
);

afterEach(() => fetch.mockClear());
```

**Option 3 — MSW (Mock Service Worker) — recommended for integration tests:**

MSW intercepts actual HTTP requests at the network level — your component's real fetch/axios calls go through, and MSW returns mocked responses. This is the most realistic option.

```jsx
// msw/handlers.js
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.get('/api/users', () => {
    return HttpResponse.json([{ id: 1, name: 'Alice' }, { id: 2, name: 'Bob' }]);
  }),

  http.post('/api/users', async ({ request }) => {
    const body = await request.json();
    return HttpResponse.json({ id: 3, ...body }, { status: 201 });
  }),

  // Simulate error
  http.delete('/api/users/:id', () => {
    return new HttpResponse(null, { status: 500 });
  }),
];

// test setup (jest.setup.js)
import { setupServer } from 'msw/node';
import { handlers } from './msw/handlers';
const server = setupServer(...handlers);
beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

**MSW advantages:**
- Works with fetch, axios, or any HTTP library.
- No need to mock module internals.
- Same handlers can be reused in browser dev mode (msw/browser).

**Related:** [REACT-091 Async testing](#react-091) | [REACT-088 Jest](#react-088)

**Source:** GreatFrontEnd Q92

---

## REACT-093

### How do you test React hooks in functional components?

The simplest approach is to **test the hook through a component** that uses it:

```jsx
// Hook
function useCounter(initialCount = 0) {
  const [count, setCount] = useState(initialCount);
  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);
  const reset = () => setCount(initialCount);
  return { count, increment, decrement, reset };
}

// Test via a component
function CounterDisplay() {
  const { count, increment, decrement } = useCounter(5);
  return (
    <div>
      <span data-testid="count">{count}</span>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}

test('useCounter starts at initial value', () => {
  render(<CounterDisplay />);
  expect(screen.getByTestId('count')).toHaveTextContent('5');
});

test('increment increases count', async () => {
  render(<CounterDisplay />);
  await userEvent.click(screen.getByRole('button', { name: '+' }));
  expect(screen.getByTestId('count')).toHaveTextContent('6');
});
```

**Using `renderHook` for isolated hook testing:**
See [REACT-094](#react-094) — `renderHook` lets you test a hook's return value and state updates without building a wrapper component.

**Related:** [REACT-094 Custom hook testing](#react-094) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q93

---

## REACT-094

### How do you test custom hooks in React?

Use **`renderHook`** from `@testing-library/react` to mount a hook in isolation and interact with its return values.

```jsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('initializes with default count', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });

  it('initializes with custom count', () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });

  it('increments count', () => {
    const { result } = renderHook(() => useCounter());
    act(() => { result.current.increment(); });
    expect(result.current.count).toBe(1);
  });

  it('resets to initial value', () => {
    const { result } = renderHook(() => useCounter(5));
    act(() => { result.current.increment(); });
    act(() => { result.current.reset(); });
    expect(result.current.count).toBe(5);
  });
});
```

**Testing hooks that need context:**
```jsx
import { renderHook } from '@testing-library/react';
import { useTheme } from './useTheme';
import { ThemeProvider } from './ThemeProvider';

it('returns current theme from context', () => {
  const wrapper = ({ children }) => (
    <ThemeProvider defaultTheme="dark">{children}</ThemeProvider>
  );

  const { result } = renderHook(() => useTheme(), { wrapper });
  expect(result.current.theme).toBe('dark');
});
```

**Testing async hooks:**
```jsx
import { renderHook, waitFor } from '@testing-library/react';

it('fetches and returns data', async () => {
  jest.spyOn(global, 'fetch').mockResolvedValueOnce({
    json: async () => ({ name: 'Akar' }),
  });

  const { result } = renderHook(() => useFetch('/api/user'));

  expect(result.current.loading).toBe(true);
  await waitFor(() => expect(result.current.loading).toBe(false));
  expect(result.current.data).toEqual({ name: 'Akar' });
});
```

**`act()`** wraps state-updating actions to ensure React processes them synchronously in tests.

**Related:** [REACT-093 Testing hooks in components](#react-093) | [REACT-034 Custom Hooks](#react-034)

**Source:** GreatFrontEnd Q94

---

## REACT-095

### What is Shallow Renderer in React testing?

**Shallow rendering** renders a component **one level deep** — it renders the component itself but replaces child components with their names instead of recursively rendering them. It was the primary approach with Enzyme's `shallow()`.

```jsx
// Component under test
function ParentComponent({ title }) {
  return (
    <div>
      <h1>{title}</h1>
      <ChildComponent data={someData} />  {/* ← NOT rendered in shallow mode */}
    </div>
  );
}

// With Enzyme shallow (legacy):
const wrapper = shallow(<ParentComponent title="Hello" />);
wrapper.find('h1').text(); // → "Hello"
wrapper.find('ChildComponent').props(); // → { data: someData } — not rendered
```

**React's official `react-dom/test-utils` also has `ShallowRenderer`:**
```jsx
import ShallowRenderer from 'react-shallow-renderer';

const renderer = new ShallowRenderer();
renderer.render(<MyComponent foo="bar" />);
const output = renderer.getRenderOutput();
```

**Current status:** The React team deprecated `ShallowRenderer` and it was removed from React core in React 19. Enzyme's shallow rendering is no longer recommended. The React Testing Library philosophy discourages shallow rendering — it leads to tests tied to implementation details.

**When shallow rendering was used:** Testing that a component renders the right child components and passes the right props to them — without caring what those children actually render.

**Modern alternative:** Use full rendering with RTL. Mock heavy child components with `jest.mock()` if needed to keep tests fast.

**Related:** [REACT-099 Shallow vs full DOM](#react-099) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q95

---

## REACT-096

### What is Snapshot Testing in React?

**Snapshot testing** captures the rendered output of a component as a serialized string (a "snapshot"), saves it to a `.snap` file, and on subsequent test runs, compares the current output to the saved snapshot. The test fails if they differ.

```jsx
import { render } from '@testing-library/react';
import renderer from 'react-test-renderer';
import Button from './Button';

test('Button renders correctly', () => {
  const tree = renderer.create(<Button label="Submit" variant="primary" />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

**Generated snapshot file (`Button.test.js.snap`):**
```
exports[`Button renders correctly 1`] = `
<button
  className="btn btn-primary"
>
  Submit
</button>
`;
```

**Updating snapshots:** When a UI change is intentional, run `jest --updateSnapshot` (or `-u`) to regenerate the snapshot files.

**Benefits:**
- Catches unintended UI regressions.
- Zero-effort baseline for new components.

**Pitfalls:**
- Developers habitually update snapshots without reviewing what changed → defeats the purpose.
- Large snapshots are hard to review in code review.
- Test passes even when the component has bugs, as long as the output is consistent.
- Tightly coupled to implementation — any internal refactor breaks snapshots.

**Recommendation:** Use snapshots sparingly — for leaf, stable UI components. Prefer explicit assertions for behaviour and integration tests.

**Related:** [REACT-100 TestRenderer](#react-100) | [REACT-088 Jest](#react-088)

**Source:** GreatFrontEnd Q96

---

## REACT-097

### How do you test React components that use context?

Wrap the component under test in the required context provider. Create a reusable `renderWithProviders` utility.

**Simple inline provider:**
```jsx
import { render, screen } from '@testing-library/react';
import { ThemeContext } from '../contexts/ThemeContext';
import ThemedButton from './ThemedButton';

test('applies dark theme class', () => {
  render(
    <ThemeContext.Provider value={{ theme: 'dark', setTheme: jest.fn() }}>
      <ThemedButton />
    </ThemeContext.Provider>
  );
  expect(screen.getByRole('button')).toHaveClass('dark');
});
```

**Reusable `renderWithProviders` wrapper:**
```jsx
// test-utils.jsx
import { render } from '@testing-library/react';
import { ThemeContext } from '../contexts/ThemeContext';
import { AuthContext } from '../contexts/AuthContext';
import { IntlProvider } from 'react-intl';

const defaultTheme = { theme: 'light', setTheme: jest.fn() };
const defaultUser = { id: '1', name: 'Test User', role: 'user' };

function AllProviders({ children, themeValue, authValue }) {
  return (
    <IntlProvider locale="en" messages={{}}>
      <AuthContext.Provider value={authValue ?? defaultUser}>
        <ThemeContext.Provider value={themeValue ?? defaultTheme}>
          {children}
        </ThemeContext.Provider>
      </AuthContext.Provider>
    </IntlProvider>
  );
}

export function renderWithProviders(ui, options = {}) {
  const { themeValue, authValue, ...renderOptions } = options;
  const wrapper = ({ children }) => (
    <AllProviders themeValue={themeValue} authValue={authValue}>
      {children}
    </AllProviders>
  );
  return render(ui, { wrapper, ...renderOptions });
}

// Usage in tests
test('admin sees delete button', () => {
  renderWithProviders(<PostActions />, {
    authValue: { id: '1', name: 'Admin', role: 'admin' },
  });
  expect(screen.getByRole('button', { name: /delete/i })).toBeInTheDocument();
});
```

**Related:** [REACT-033 useContext](#react-033) | [REACT-090 Testing components](#react-090)

**Source:** GreatFrontEnd Q97

---

## REACT-098

### How do you test React components that use Redux?

Wrap the component under test in a real or mock Redux `<Provider>` with a test store. RTL integrates cleanly with Redux.

**Using a real store (recommended):**
```jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';
import CartButton from './CartButton';
import cartReducer from '../features/cart/cartSlice';

function renderWithStore(ui, { preloadedState = {}, ...options } = {}) {
  const store = configureStore({
    reducer: { cart: cartReducer },
    preloadedState,
  });

  function Wrapper({ children }) {
    return <Provider store={store}>{children}</Provider>;
  }

  return { store, ...render(ui, { wrapper: Wrapper, ...options }) };
}

test('shows item count from store', () => {
  renderWithStore(<CartButton />, {
    preloadedState: { cart: { items: [{ id: 1 }, { id: 2 }] } },
  });
  expect(screen.getByText('2')).toBeInTheDocument();
});

test('dispatches add action on click', async () => {
  const { store } = renderWithStore(<CartButton productId="abc" />, {
    preloadedState: { cart: { items: [] } },
  });

  await userEvent.click(screen.getByRole('button', { name: /add to cart/i }));

  const state = store.getState();
  expect(state.cart.items).toHaveLength(1);
  expect(state.cart.items[0].productId).toBe('abc');
});
```

**With RTK Query (React Query for Redux):**
Mock the API endpoints in test setup using RTK Query's `setupServer` or MSW.

**Redux DevTools in tests:** `configureStore` works normally in tests — you can inspect the final state via `store.getState()`.

**Related:** [REACT-097 Context in tests](#react-097) | [REACT-044 Flux pattern](#react-044)

**Source:** GreatFrontEnd Q98

---

## REACT-099

### What are the key differences between shallow rendering and full DOM rendering?

| | Shallow Rendering | Full DOM Rendering |
|---|---|---|
| **Renders children?** | No — stubs child components | Yes — full tree recursively rendered |
| **DOM access** | No real DOM | Real DOM via jsdom |
| **Lifecycle methods** | Partial | Full lifecycle (mount, update, unmount) |
| **Context** | Limited | Full context support |
| **Speed** | Faster (less work) | Slightly slower |
| **Tools** | Enzyme `shallow`, React's ShallowRenderer | RTL `render`, Enzyme `mount` |
| **Tests coupled to internals?** | Yes — tests aware of child components | No — tests against rendered output |
| **Recommended today?** | No — discouraged / deprecated | Yes — standard practice |

**Why shallow rendering fell out of favour:**
- Tests that know about child component names break on refactoring (e.g., renaming a child).
- You can't test interactions that cross component boundaries.
- Doesn't catch bugs where props are incorrectly passed to children.
- Creates false confidence — a passing shallow test doesn't mean the real UI works.

**When full DOM rendering wins:**
- Testing user interactions end-to-end across multiple components.
- Testing context, portals, refs.
- Testing components that use `useEffect` (requires real mount lifecycle).

**Related:** [REACT-095 Shallow Renderer](#react-095) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q99

---

## REACT-100

### What is the TestRenderer package in React?

**`react-test-renderer`** is an official React package that renders React components to a pure JavaScript tree — without using the DOM or a native environment. It's primarily used for **snapshot testing**.

```jsx
import renderer from 'react-test-renderer';
import Button from './Button';

test('Button snapshot', () => {
  const component = renderer.create(
    <Button label="Click me" onClick={() => {}} />
  );

  const tree = component.toJSON();
  expect(tree).toMatchSnapshot();
  /*
  Snapshot:
  <button
    className="btn"
    onClick={[Function]}
  >
    Click me
  </button>
  */
});
```

**Methods:**
- `renderer.create(<Component />)` — renders the component.
- `.toJSON()` — returns a serializable tree for snapshot matching.
- `.getInstance()` — returns the underlying class component instance (not applicable to function components).
- `.update(<Component newProp />)` — re-renders with new props.
- `act(callback)` — wraps updates to flush state changes.

**Differences from RTL:**
| | `react-test-renderer` | React Testing Library |
|---|---|---|
| **DOM** | No real DOM — pure JS tree | jsdom (simulated browser DOM) |
| **User events** | Not designed for user interaction | `userEvent` for realistic interaction |
| **Primary use** | Snapshot testing | Behaviour testing |
| **Queries** | None | `getByRole`, `getByText`, etc. |

**Current status:** `react-test-renderer` was deprecated and removed from React 19. For new projects, use RTL for behaviour tests and RTL's `render` even for snapshot tests (`screen.asFragment()`).

**Related:** [REACT-096 Snapshot testing](#react-096) | [REACT-089 RTL](#react-089)

**Source:** GreatFrontEnd Q100

---

*← [Internationalization](./05-i18n.md) | [Master Index](../reference/00-master-index.md) | [React 19 →](./07-react19.md)*
