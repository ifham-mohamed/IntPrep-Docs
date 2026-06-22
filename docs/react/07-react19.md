# React 19 and Modern React

> **Canonical Knowledge Base** | Category: React 19 | IDs: REACT-101 – REACT-110
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-101 — What's new in React 19?](#react-101)
- [REACT-102 — What are Actions in React 19?](#react-102)
- [REACT-103 — What does the `useActionState` hook do?](#react-103)
- [REACT-104 — What does `useOptimistic` do?](#react-104)
- [REACT-105 — What is the `use` hook and how is it different from `useEffect` + fetch?](#react-105)
- [REACT-106 — What are React Server Components?](#react-106)
- [REACT-107 — What's the difference between Server Components and Client Components?](#react-107)
- [REACT-108 — What is the React Compiler?](#react-108)
- [REACT-109 — What's the difference between `useTransition` and `useDeferredValue`?](#react-109)
- [REACT-110 — How does the new form `action` prop work in React 19?](#react-110)

---

## REACT-101

### What's new in React 19?

React 19 (released December 2024) is the biggest React release in years, focusing on three themes: **Actions** for async mutations, **Server Components** for full-stack React, and the **React Compiler** for automatic memoization.

**Key additions:**

**New hooks:**
- `useActionState` — manages async form actions (loading, error, result state).
- `useOptimistic` — temporarily applies an optimistic update while an async action is pending.
- `use(resource)` — reads a Promise or Context mid-render; works with Suspense.
- `useFormStatus` — reads the pending state of a surrounding `<form>` action.

**Actions:**
- `<form action={asyncFn}>` — forms can now accept an async function as the `action` prop.
- `<button formAction={asyncFn}>` — per-button action overrides.
- Transitions automatically wrap form actions.

**Server Components (stable):**
- `async` server components — components that `await` data on the server.
- `'use client'` and `'use server'` directives.

**Breaking / deprecated removals:**
- `react-test-renderer` removed.
- `forwardRef` no longer needed — `ref` is now a regular prop.
- `React.memo` still available but React Compiler may make it redundant.
- PropTypes moved to a separate package.
- Deprecated lifecycle warnings removed (`componentWillMount` etc. finally gone).

**Other improvements:**
- `ref` as a prop (no `forwardRef` needed).
- `<Context>` as a provider instead of `<Context.Provider>`.
- `onCaughtError` / `onUncaughtError` on `createRoot` for global error handling.
- `preload`, `preinit`, `prefetchDNS` Resource Hints API.
- Document metadata (`<title>`, `<meta>`, `<link>`) hoisted to `<head>` automatically.

**Related:** [REACT-102 Actions](#react-102) | [REACT-106 Server Components](#react-106) | [REACT-108 React Compiler](#react-108)

**Source:** GreatFrontEnd Q101

---

## REACT-102

### What are Actions in React 19?

**Actions** are async functions passed directly to form elements (or transitions) that React manages for you — automatically handling pending state, errors, and sequential execution.

**Before React 19 — manual async state management:**
```jsx
function OldForm() {
  const [pending, setPending] = useState(false);
  const [error, setError] = useState(null);

  async function handleSubmit(e) {
    e.preventDefault();
    setPending(true);
    try {
      await submitForm(new FormData(e.target));
    } catch (err) {
      setError(err.message);
    } finally {
      setPending(false);
    }
  }

  return (
    <form onSubmit={handleSubmit}>
      <button disabled={pending}>{pending ? 'Saving...' : 'Save'}</button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

**React 19 — Actions with `useActionState`:**
```jsx
import { useActionState } from 'react';

async function submitFormAction(prevState, formData) {
  try {
    await saveProfile(Object.fromEntries(formData));
    return { success: true, error: null };
  } catch (err) {
    return { success: false, error: err.message };
  }
}

function NewForm() {
  const [state, formAction, isPending] = useActionState(submitFormAction, {
    success: false,
    error: null,
  });

  return (
    <form action={formAction}>
      <input name="name" />
      <button disabled={isPending}>{isPending ? 'Saving...' : 'Save'}</button>
      {state.error && <p>{state.error}</p>}
      {state.success && <p>Saved!</p>}
    </form>
  );
}
```

**Key properties of Actions:**
- React manages pending state automatically.
- Multiple submissions are queued — React won't start a new submission until the previous one completes.
- Errors are caught and exposed via state.
- Work with both client and server (Server Actions in Next.js).

**Related:** [REACT-103 useActionState](#react-103) | [REACT-110 form action prop](#react-110)

**Source:** GreatFrontEnd Q102

---

## REACT-103

### What does the `useActionState` hook do?

`useActionState(action, initialState, permalink?)` wraps an async action function and returns `[state, formAction, isPending]` — managing the loading/error/result lifecycle automatically.

```jsx
import { useActionState } from 'react';

// The action receives (previousState, formData) and returns new state
async function createUser(prevState, formData) {
  const name = formData.get('name');
  const email = formData.get('email');

  if (!email.includes('@')) {
    return { error: 'Invalid email', user: null };
  }

  try {
    const user = await api.createUser({ name, email });
    return { error: null, user };
  } catch (err) {
    return { error: err.message, user: null };
  }
}

function CreateUserForm() {
  const [state, formAction, isPending] = useActionState(createUser, {
    error: null,
    user: null,
  });

  if (state.user) {
    return <p>Created user: {state.user.name}</p>;
  }

  return (
    <form action={formAction}>
      <label>
        Name: <input name="name" required />
      </label>
      <label>
        Email: <input name="email" type="email" required />
      </label>

      {state.error && <p role="alert" style={{ color: 'red' }}>{state.error}</p>}

      <button type="submit" disabled={isPending}>
        {isPending ? 'Creating...' : 'Create User'}
      </button>
    </form>
  );
}
```

**Return values:**
- `state` — the current state (starts as `initialState`, updated by each action return).
- `formAction` — pass this to `<form action={formAction}>` or `<button formAction={formAction}>`.
- `isPending` — `true` while the action is executing.

**With Server Actions (Next.js):**
```jsx
// app/actions.ts — 'use server' marks this as a server action
'use server';
export async function createUser(prevState, formData) {
  // Runs on the server — can access database directly
  const user = await db.user.create({ data: Object.fromEntries(formData) });
  revalidatePath('/users');
  return { user, error: null };
}
```

**Related:** [REACT-102 Actions](#react-102) | [REACT-104 useOptimistic](#react-104) | [REACT-110 form action prop](#react-110)

**Source:** GreatFrontEnd Q103

---

## REACT-104

### What does `useOptimistic` do?

`useOptimistic(state, updateFn)` lets you display a **temporary optimistic value** immediately while an async action is pending — without waiting for the server response. When the action completes (or fails), the optimistic value is replaced with the real state.

```jsx
import { useOptimistic, useActionState } from 'react';

function TodoList({ todos: serverTodos }) {
  const [optimisticTodos, addOptimisticTodo] = useOptimistic(
    serverTodos,
    (currentTodos, newTodo) => [...currentTodos, { ...newTodo, pending: true }]
  );

  async function addTodo(prevState, formData) {
    const title = formData.get('title');
    const tempTodo = { id: crypto.randomUUID(), title, completed: false };

    // Immediately show the optimistic todo
    addOptimisticTodo(tempTodo);

    // Server call — optimistic entry shown while this is in flight
    try {
      const savedTodo = await api.createTodo({ title });
      return [...prevState, savedTodo]; // real state replaces optimistic
    } catch (err) {
      // On error, React reverts to `serverTodos` automatically
      throw err;
    }
  }

  const [todos, formAction, isPending] = useActionState(addTodo, serverTodos);

  return (
    <div>
      <ul>
        {optimisticTodos.map(todo => (
          <li key={todo.id} style={{ opacity: todo.pending ? 0.5 : 1 }}>
            {todo.title} {todo.pending && '(saving...)'}
          </li>
        ))}
      </ul>
      <form action={formAction}>
        <input name="title" placeholder="New todo" />
        <button type="submit" disabled={isPending}>Add</button>
      </form>
    </div>
  );
}
```

**How it works:**
1. User submits a form.
2. `addOptimisticTodo` is called immediately — UI updates instantly.
3. The async action runs in the background.
4. On success → real state from server replaces the optimistic value.
5. On failure → React reverts `optimisticTodos` back to `serverTodos`.

**Use cases:** Like buttons, comment submission, cart operations, todo creation — anywhere UX benefits from instant feedback.

**Related:** [REACT-102 Actions](#react-102) | [REACT-103 useActionState](#react-103)

**Source:** GreatFrontEnd Q104

---

## REACT-105

### What is the `use` hook and how is it different from `useEffect` + fetch?

`use(resource)` is a new React 19 hook that reads the value of a **Promise** or **Context** during render — suspending the component until the Promise resolves (integrates with Suspense).

**Reading a Promise:**
```jsx
import { use, Suspense } from 'react';

// The Promise is created outside the component (e.g., in a parent or cache)
function UserCard({ userPromise }) {
  const user = use(userPromise); // suspends until resolved
  return <div>{user.name}</div>;
}

function App() {
  const userPromise = fetchUser(userId); // stable reference — cached
  return (
    <Suspense fallback={<Skeleton />}>
      <UserCard userPromise={userPromise} />
    </Suspense>
  );
}
```

**Reading Context with `use` (can be called conditionally unlike `useContext`):**
```jsx
function ThemeButton({ darkMode }) {
  // use() can be called inside conditions — unlike useContext
  if (darkMode) {
    const theme = use(ThemeContext);
    return <button className={theme.button}>Click</button>;
  }
  return <button>Click</button>;
}
```

**`use` vs `useEffect` + fetch:**

| | `useEffect` + fetch | `use(promise)` |
|---|---|---|
| **When data loads** | After first render (waterfall) | During render (before paint) |
| **Suspense integration** | Manual (show spinner in component) | Automatic (parent Suspense shows fallback) |
| **Error handling** | Manual try/catch + error state | ErrorBoundary catches rejections |
| **Cancellation** | Manual cleanup / AbortController | Managed by framework/cache layer |
| **Conditional use** | Must follow hook rules (top level) | Can be called conditionally |
| **Best for** | Side effects with subscriptions | Read-only data access in render |

**Important:** The Promise passed to `use` should be **stable** (not recreated on every render). Create it outside the component or cache it with a library like React Query or Next.js's `fetch` caching.

**Related:** [REACT-038 Suspense](#react-038) | [REACT-064 Async data loading](#react-064) | [REACT-024 Rules of hooks](#react-024)

**Source:** GreatFrontEnd Q105

---

## REACT-106

### What are React Server Components?

**React Server Components (RSC)** are components that run exclusively on the server — they can directly access databases, file systems, and secrets, and they send their rendered output (not JS) to the client. They have **zero client-side JavaScript bundle impact**.

**What RSCs can do:**
- `await` async operations directly in the component body.
- Import server-only modules (database clients, ORMs, secrets).
- Read from the file system, environment variables, etc.

**What RSCs cannot do:**
- Use state (`useState`, `useReducer`).
- Use lifecycle effects (`useEffect`).
- Use browser APIs (`window`, `localStorage`, DOM events).
- Use event handlers (`onClick`, `onChange`).

```jsx
// A Server Component (no 'use client' directive = server by default in Next.js App Router)
async function ProductPage({ params }) {
  // Direct database query — no API route needed, no client JS
  const product = await db.product.findUnique({ where: { id: params.id } });
  const reviews = await db.review.findMany({ where: { productId: params.id } });

  return (
    <div>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      {/* Client Component nested inside Server Component */}
      <AddToCartButton productId={product.id} />
      <ReviewList reviews={reviews} />
    </div>
  );
}
```

**RSC rendering model:**
```
Server: RSC renders → React tree (RSC payload format)
         ↓
Client: React reconstructs tree → merges with Client Components → hydrates
```

**Framework support:** RSC requires framework integration (Next.js App Router, Remix 2+, Waku). You cannot use RSC in a plain Vite/CRA setup.

**Related:** [REACT-107 Server vs Client Components](#react-107) | [REACT-057 SSR](#react-057) | [REACT-039 Hydration](#react-039)

**Source:** GreatFrontEnd Q106

---

## REACT-107

### What's the difference between Server Components and Client Components?

| | Server Components | Client Components |
|---|---|---|
| **Where they run** | Server only | Server (initial render) + Client (hydration + updates) |
| **Directive** | None (default in Next.js App Router) | `'use client'` at top of file |
| **State / hooks** | ❌ Not allowed | ✅ Full hooks support |
| **Event handlers** | ❌ Not allowed | ✅ `onClick`, `onChange`, etc. |
| **Bundle impact** | Zero — not shipped to client | Included in client JS bundle |
| **Data access** | Direct (DB, FS, secrets) | API calls via fetch |
| **Async in component** | ✅ `await` directly | ❌ Must use `useEffect` or `use()` |
| **Context** | Can provide, cannot consume most hooks | Full context support |

**Composition rules:**
- ✅ Server Component can import Client Components.
- ❌ Client Component cannot import Server Components directly.
- ✅ Server Components can be passed as `children` or props to Client Components.

```jsx
// Server Component (no directive)
async function Layout({ children }) {
  const user = await getCurrentUser(); // server-only
  return (
    <div>
      <Header user={user} />     {/* Server Component */}
      <Sidebar />                 {/* Server Component */}
      <main>{children}</main>
    </div>
  );
}

// Client Component
'use client';
import { useState } from 'react';

function Sidebar() {
  const [collapsed, setCollapsed] = useState(false);
  return (
    <aside>
      <button onClick={() => setCollapsed(c => !c)}>Toggle</button>
      {!collapsed && <NavLinks />}  {/* Server Component passed as prop works */}
    </aside>
  );
}
```

**Decision guide:** Make a component a Client Component only when it needs interactivity, browser APIs, or state. Keep the rest as Server Components to minimize bundle size.

**Related:** [REACT-106 Server Components](#react-106) | [REACT-057 SSR](#react-057)

**Source:** GreatFrontEnd Q107

---

## REACT-108

### What is the React Compiler?

The **React Compiler** (previously "React Forget") is an experimental build-time compiler that **automatically memoizes** React components and hooks — eliminating the need to manually write `useMemo`, `useCallback`, and `React.memo`.

**What it does:**
The compiler analyzes your component code and inserts memoization automatically wherever it determines a value is stable and doesn't need to be recomputed.

```jsx
// You write this (no manual memoization):
function TodoList({ todos, filter }) {
  const filtered = todos.filter(t => t.status === filter);
  return <ul>{filtered.map(t => <li key={t.id}>{t.text}</li>)}</ul>;
}

// The compiler transforms it to something equivalent to:
const TodoList = React.memo(function TodoList({ todos, filter }) {
  const filtered = useMemo(
    () => todos.filter(t => t.status === filter),
    [todos, filter]
  );
  return <ul>{filtered.map(t => <li key={t.id}>{t.text}</li>)}</ul>;
});
```

**Requirements for the compiler to work:**
- Code must follow React's rules (pure renders, no direct mutations, no hook rule violations).
- Uses the new React 19 runtime.
- Opt-in via compiler config — not automatic.

**Status:** Available as `babel-plugin-react-compiler` / `eslint-plugin-react-compiler`. Adopted by Meta's production apps. Stable in React 19.

**Does it replace `useMemo`/`useCallback`?**
Gradually, yes — if your code follows React's rules, the compiler handles memoization for you. `useMemo` and `useCallback` are still available for cases the compiler can't analyze.

**Tooling:**
```bash
npm install babel-plugin-react-compiler eslint-plugin-react-compiler
```

```json
// babel.config.json
{
  "plugins": ["babel-plugin-react-compiler"]
}
```

**Related:** [REACT-029 useCallback](#react-029) | [REACT-030 useMemo](#react-030) | [REACT-016 Pure Components](#react-016)

**Source:** GreatFrontEnd Q108

---

## REACT-109

### What's the difference between `useTransition` and `useDeferredValue`?

Both are concurrent React tools for deferring non-urgent updates, but they work differently:

| | `useTransition` | `useDeferredValue` |
|---|---|---|
| **What you control** | Marks a state update as non-urgent | Defers re-rendering of a specific value |
| **Usage** | Wraps a state setter call | Wraps a value |
| **Pending signal** | Returns `isPending` boolean | No built-in pending signal (use `value !== deferredValue`) |
| **Control level** | You decide which update is deferred | React decides when to apply the deferred value |
| **When to use** | You own the state update being deferred | You receive a prop/value you can't control |

**`useTransition` — wrap the update:**
```jsx
const [isPending, startTransition] = useTransition();

function handleInput(e) {
  setSearchQuery(e.target.value); // urgent — update input immediately

  startTransition(() => {
    setSearchResults(search(e.target.value)); // non-urgent — can be interrupted
  });
}

return (
  <>
    <input value={searchQuery} onChange={handleInput} />
    {isPending ? <Spinner /> : <Results data={searchResults} />}
  </>
);
```

**`useDeferredValue` — wrap the value:**
```jsx
function SearchPage({ query }) {
  // query prop comes from parent — you can't control when it updates
  const deferredQuery = useDeferredValue(query);

  // HeavyResults re-renders with deferredQuery while deferredQuery lags behind query
  const isStale = query !== deferredQuery;

  return (
    <div style={{ opacity: isStale ? 0.7 : 1 }}>
      <HeavyResults query={deferredQuery} />
    </div>
  );
}
```

**Rule of thumb:**
- You control the state update → use `useTransition`.
- You receive a value from props/context you can't control → use `useDeferredValue`.

**Related:** [REACT-054 Concurrent features](#react-054) | [REACT-055 Update prioritization](#react-055)

**Source:** GreatFrontEnd Q109

---

## REACT-110

### How does the new form `action` prop work in React 19?

In React 19, the `<form>` element's `action` prop accepts an **async function** (not just a URL string). When the form is submitted, React calls the function with the form's `FormData` and manages pending state.

**Basic usage:**
```jsx
async function handleSubmit(formData) {
  const name = formData.get('name');
  const email = formData.get('email');
  await saveToDatabase({ name, email });
}

function ContactForm() {
  return (
    <form action={handleSubmit}>
      <input name="name" placeholder="Name" required />
      <input name="email" type="email" placeholder="Email" required />
      <button type="submit">Send</button>
    </form>
  );
}
```

**With `useActionState` for full control:**
```jsx
import { useActionState } from 'react';
import { useFormStatus } from 'react-dom';

async function submitAction(prevState, formData) {
  const data = Object.fromEntries(formData);
  try {
    await api.submit(data);
    return { success: true, error: null };
  } catch (err) {
    return { success: false, error: err.message };
  }
}

// Submit button that reads pending state from the surrounding form
function SubmitButton() {
  const { pending } = useFormStatus();
  return <button type="submit" disabled={pending}>{pending ? 'Submitting...' : 'Submit'}</button>;
}

function MyForm() {
  const [state, formAction, isPending] = useActionState(submitAction, { success: false, error: null });

  return (
    <form action={formAction}>
      <input name="title" />
      <SubmitButton />
      {state.error && <p>{state.error}</p>}
    </form>
  );
}
```

**Per-button action (`formAction` prop):**
```jsx
<form>
  <input name="item" />
  <button formAction={saveAsDraft}>Save Draft</button>
  <button formAction={publishAction}>Publish</button>
</form>
```

**Behavior differences from `onSubmit`:**
- No need for `e.preventDefault()` — React handles it.
- After a successful action, form inputs are **automatically reset** to empty (like native HTML form behavior).
- Errors thrown inside the action are caught by the nearest ErrorBoundary.
- Pending state is automatically tracked — accessible via `useFormStatus()` in child components.

**Related:** [REACT-102 Actions](#react-102) | [REACT-103 useActionState](#react-103) | [REACT-104 useOptimistic](#react-104)

**Source:** GreatFrontEnd Q110

---

*← [React Testing](./06-testing.md) | [Master Index](../reference/00-master-index.md)*
