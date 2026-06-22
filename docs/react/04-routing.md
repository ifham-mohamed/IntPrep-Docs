# React Router

> **Canonical Knowledge Base** | Category: React Router | IDs: REACT-066 – REACT-079
>
> Sources: [GreatFrontEnd – 100 React Interview Questions](https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers)

---

## Table of Contents

- [REACT-066 — What is React Router?](#react-066)
- [REACT-067 — How does React Router work, and how do you implement dynamic routing?](#react-067)
- [REACT-068 — How do you handle nested routes and route parameters?](#react-068)
- [REACT-069 — What is the difference between BrowserRouter and HashRouter?](#react-069)
- [REACT-070 — How is React Router different from the history library?](#react-070)
- [REACT-071 — What are the `<Router>` components in React Router v6?](#react-071)
- [REACT-072 — What is the purpose of push and replace in history?](#react-072)
- [REACT-073 — How do you navigate programmatically in React Router?](#react-073)
- [REACT-074 — How would you implement route guards or private routes?](#react-074)
- [REACT-075 — How do you manage active route state in a multi-page app?](#react-075)
- [REACT-076 — How do you handle 404 / page not found in React Router?](#react-076)
- [REACT-077 — How do you get query parameters in React Router?](#react-077)
- [REACT-078 — How do you perform an automatic redirect after login?](#react-078)
- [REACT-079 — How do you pass props to a route component in React Router?](#react-079)

---

## REACT-066

### What is React Router?

**React Router** is the standard routing library for React applications. It maps URL paths to components, enabling navigation between views without full page reloads (SPA behaviour).

**Key capabilities:**
- Declarative route definitions with `<Route>` components.
- URL parameter extraction (`:id`, `:slug`).
- Nested routes and layouts.
- Programmatic navigation via the `useNavigate` hook.
- Query string parsing via `useSearchParams`.
- Route guards and redirects.
- Lazy-loaded route components with `React.lazy`.
- History management (browser history, hash history, memory history).

**Install:**
```bash
npm install react-router-dom
```

**Minimal setup (v6):**
```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users/:id" element={<UserProfile />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Related:** [REACT-067 Dynamic routing](#react-067) | [REACT-042 Code splitting](#react-042)

**Source:** GreatFrontEnd Q66

---

## REACT-067

### How does React Router work, and how do you implement dynamic routing?

**How it works:**
React Router uses the browser's History API (`pushState`, `replaceState`) to update the URL without a page reload. It listens for URL changes and re-renders the component tree to match the current path against registered `<Route>` elements.

**Dynamic routing** means route parameters whose values change at runtime, defined with a colon prefix (`:paramName`).

```jsx
// Route definition
<Route path="/products/:productId" element={<ProductDetail />} />

// Accessing the param inside the component
import { useParams } from 'react-router-dom';

function ProductDetail() {
  const { productId } = useParams();

  const [product, setProduct] = useState(null);
  useEffect(() => {
    fetchProduct(productId).then(setProduct);
  }, [productId]);

  if (!product) return <Spinner />;
  return <h1>{product.name}</h1>;
}
```

**Multiple params:**
```jsx
<Route path="/teams/:teamId/members/:memberId" element={<MemberDetail />} />

function MemberDetail() {
  const { teamId, memberId } = useParams();
  // ...
}
```

**Optional segments (v6.3+):**
```jsx
<Route path="/docs/:lang?/:version?" element={<Docs />} />
```

**Related:** [REACT-068 Nested routes](#react-068) | [REACT-077 Query params](#react-077)

**Source:** GreatFrontEnd Q67

---

## REACT-068

### How do you handle nested routes and route parameters in React Router?

**Nested routes** let a parent route render a shared layout while child routes swap in their content via `<Outlet>`.

```jsx
// Route tree
<Routes>
  <Route path="/dashboard" element={<DashboardLayout />}>
    <Route index element={<DashboardHome />} />          {/* /dashboard */}
    <Route path="analytics" element={<Analytics />} />  {/* /dashboard/analytics */}
    <Route path="settings" element={<Settings />} />    {/* /dashboard/settings */}
    <Route path="users/:userId" element={<UserDetail />} /> {/* /dashboard/users/42 */}
  </Route>
</Routes>

// DashboardLayout renders the shared shell + <Outlet> where child routes appear
function DashboardLayout() {
  return (
    <div className="dashboard">
      <Sidebar />
      <main>
        <Outlet /> {/* child route content renders here */}
      </main>
    </div>
  );
}

// Accessing nested param in child
function UserDetail() {
  const { userId } = useParams();
  return <div>User #{userId}</div>;
}
```

**`index` route:** Renders at the parent's exact path (`/dashboard`) — no extra path segment.

**Relative links in nested routes:**
```jsx
// Inside a /dashboard/* child, use relative paths
<Link to="analytics">Go to Analytics</Link>  // → /dashboard/analytics
<Link to="..">Go up</Link>                   // → /dashboard (parent)
```

**Related:** [REACT-066 React Router basics](#react-066) | [REACT-067 Dynamic routing](#react-067) | [REACT-073 Programmatic navigation](#react-073)

**Source:** GreatFrontEnd Q68

---

## REACT-069

### What is the difference between BrowserRouter and HashRouter?

| | `BrowserRouter` | `HashRouter` |
|---|---|---|
| **URL format** | `example.com/about` | `example.com/#/about` |
| **History API** | HTML5 `pushState` / `replaceState` | URL hash (`window.location.hash`) |
| **Server config needed?** | Yes — server must return `index.html` for all routes | No — `#` part never sent to server |
| **SEO** | Better — full paths are crawlable | Worse — hash fragment ignored by search engines |
| **Browser support** | IE10+ (History API) | All browsers |
| **Use case** | Production web apps with a proper server | Static file hosting with no server-side routing (e.g., GitHub Pages without config) |

**`BrowserRouter` server requirement:** Without server config, navigating to `example.com/about` directly returns a 404 (server doesn't know the route). You must configure the server to serve `index.html` for all paths (nginx: `try_files`, Apache: `.htaccess` with `mod_rewrite`).

```jsx
// BrowserRouter — clean URLs
import { BrowserRouter } from 'react-router-dom';
<BrowserRouter><App /></BrowserRouter>

// HashRouter — hash-based, no server config
import { HashRouter } from 'react-router-dom';
<HashRouter><App /></HashRouter>
```

**`MemoryRouter`** — a third option that keeps history in memory (no URL change). Used in tests and React Native.

**Related:** [REACT-070 history library](#react-070) | [REACT-071 Router components](#react-071)

**Source:** GreatFrontEnd Q69

---

## REACT-070

### How is React Router different from the history library?

The **`history` library** is a low-level abstraction over the browser's History API. React Router uses it internally but exposes a higher-level declarative API.

| | `history` library | React Router |
|---|---|---|
| **Level** | Low-level — raw API | High-level — React components and hooks |
| **API** | `history.push('/path')`, `history.listen()` | `<Route>`, `useNavigate`, `<Link>` |
| **React integration** | None — plain JS | Built for React — components, context, hooks |
| **Routing** | No concept of routes | Full route matching, nested routes, params |
| **Use alone?** | Yes (any JS app) | No (React only) |

React Router v6 ships its own `@remix-run/router` package (built on `history`) and exposes the router instance via `useRouter` internally. You rarely need to use `history` directly in a React project when using React Router v6.

**When `history` directly matters:** If you need to access the navigation history outside a React component (e.g., in an Axios interceptor to redirect to `/login` on 401), you can create a shared `history` instance.

```js
// history.js
import { createBrowserHistory } from 'history';
export const history = createBrowserHistory();

// axios interceptor
axios.interceptors.response.use(null, error => {
  if (error.response?.status === 401) history.push('/login');
  return Promise.reject(error);
});
```

**Related:** [REACT-069 BrowserRouter vs HashRouter](#react-069) | [REACT-072 push vs replace](#react-072)

**Source:** GreatFrontEnd Q70

---

## REACT-071

### What are the `<Router>` components in React Router v6?

React Router v6 ships several router implementations for different environments:

| Router | When to use |
|---|---|
| `<BrowserRouter>` | Standard web apps — HTML5 History API, clean URLs |
| `<HashRouter>` | Static file hosting without server routing config |
| `<MemoryRouter>` | Tests, non-browser environments (React Native) |
| `<StaticRouter>` | Server-side rendering — matches a URL string, no history |
| `<RouterProvider>` | Data router API (v6.4+) — enables loaders, actions, `defer` |
| `<NativeRouter>` | React Native apps |

**v6.4 Data Router (recommended for new apps):**
```jsx
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'users/:id',
        element: <UserProfile />,
        loader: ({ params }) => fetchUser(params.id), // data loading
      },
    ],
  },
]);

function App() {
  return <RouterProvider router={router} />;
}
```

The data router pattern co-locates data loading with routes and enables `useLoaderData` in route components, eliminating waterfall `useEffect` fetches.

**Related:** [REACT-069 BrowserRouter vs HashRouter](#react-069) | [REACT-064 Async data loading](#react-064)

**Source:** GreatFrontEnd Q71

---

## REACT-072

### What is the purpose of push and replace in history?

Both methods navigate to a new URL, but they differ in how they interact with the browser's back/forward stack:

| Method | History stack | Back button behaviour |
|---|---|---|
| `push('/new')` | Adds a new entry | Back button returns to previous page |
| `replace('/new')` | Replaces current entry | Back button skips this page — returns to the page before it |

**When to use `replace`:**
- After a successful login — you don't want the back button to return to the login page.
- Redirect from an old URL to a new one — avoids back-button loops.
- Pagination — replacing current page entry so back doesn't cycle through every page change.
- Wizard steps where you don't want the user to go "back" to a completed step.

**In React Router v6:**
```jsx
const navigate = useNavigate();

// Push — new history entry
navigate('/dashboard');

// Replace — overwrite current entry
navigate('/dashboard', { replace: true });

// Programmatic back/forward
navigate(-1); // go back
navigate(1);  // go forward
```

**Related:** [REACT-073 Programmatic navigation](#react-073) | [REACT-078 Redirect after login](#react-078)

**Source:** GreatFrontEnd Q72

---

## REACT-073

### How do you navigate programmatically in React Router?

Use the `useNavigate` hook (React Router v6) to navigate without a `<Link>` click — e.g., after form submission or API call.

```jsx
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const navigate = useNavigate();
  const [error, setError] = useState(null);

  async function handleSubmit(e) {
    e.preventDefault();
    try {
      await loginUser(formData);
      navigate('/dashboard', { replace: true }); // navigate + replace history entry
    } catch (err) {
      setError(err.message);
    }
  }

  return <form onSubmit={handleSubmit}>...</form>;
}
```

**With state:**
```jsx
// Pass state along with navigation
navigate('/order-confirmation', { state: { orderId: '12345' } });

// Access state in destination component
const location = useLocation();
const { orderId } = location.state;
```

**Navigating with search params:**
```jsx
navigate(`/search?q=${encodeURIComponent(query)}&page=1`);
```

**Conditional back navigation:**
```jsx
function handleCancel() {
  // Go back if there's history, otherwise go home
  if (window.history.length > 1) {
    navigate(-1);
  } else {
    navigate('/');
  }
}
```

**Related:** [REACT-072 push vs replace](#react-072) | [REACT-078 Redirect after login](#react-078) | [REACT-077 Query params](#react-077)

**Source:** GreatFrontEnd Q73

---

## REACT-074

### How would you implement route guards or private routes in React?

A **private route** wraps a route and redirects unauthenticated users to a login page.

**Pattern 1 — Wrapper component (most common):**
```jsx
import { Navigate, useLocation } from 'react-router-dom';

function PrivateRoute({ children }) {
  const { isAuthenticated } = useAuth(); // custom hook reading auth state
  const location = useLocation();

  if (!isAuthenticated) {
    // Redirect to login, saving the intended path in state
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  return children;
}

// Usage
<Routes>
  <Route path="/login" element={<Login />} />
  <Route
    path="/dashboard"
    element={
      <PrivateRoute>
        <Dashboard />
      </PrivateRoute>
    }
  />
</Routes>
```

**Pattern 2 — Layout route (cleaner for many protected routes):**
```jsx
function ProtectedLayout() {
  const { isAuthenticated } = useAuth();
  const location = useLocation();

  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }

  return <Outlet />; // render child routes
}

<Routes>
  <Route path="/login" element={<Login />} />
  <Route element={<ProtectedLayout />}>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/profile" element={<Profile />} />
    <Route path="/settings" element={<Settings />} />
  </Route>
</Routes>
```

**Role-based guards:**
```jsx
function RoleRoute({ children, requiredRole }) {
  const { user } = useAuth();
  if (!user) return <Navigate to="/login" replace />;
  if (!user.roles.includes(requiredRole)) return <Navigate to="/forbidden" replace />;
  return children;
}
```

**Related:** [REACT-073 Programmatic navigation](#react-073) | [REACT-078 Redirect after login](#react-078)

**Source:** GreatFrontEnd Q74

---

## REACT-075

### How do you manage active route state in a multi-page React application?

React Router provides the `<NavLink>` component which automatically applies an `active` class/style when its `to` prop matches the current URL.

**`<NavLink>` basic usage:**
```jsx
import { NavLink } from 'react-router-dom';

function Nav() {
  return (
    <nav>
      <NavLink to="/" end>Home</NavLink>
      <NavLink to="/about">About</NavLink>
      <NavLink to="/contact">Contact</NavLink>
    </nav>
  );
}

/* CSS */
/* .active { font-weight: bold; color: blue; } */
```

**Custom active styling with a function:**
```jsx
<NavLink
  to="/about"
  className={({ isActive, isPending }) =>
    isActive ? 'nav-link active' : isPending ? 'nav-link pending' : 'nav-link'
  }
  style={({ isActive }) => ({ color: isActive ? 'blue' : 'inherit' })}
>
  About
</NavLink>
```

**`end` prop:** Without `end`, a NavLink with `to="/"` matches every URL (since all URLs start with `/`). The `end` prop makes it only match exactly `/`.

**Programmatic check with `useMatch`:**
```jsx
import { useMatch } from 'react-router-dom';

function CustomNavItem({ to, label }) {
  const match = useMatch({ path: to, end: true });
  return (
    <li className={match ? 'active' : ''}>
      <Link to={to}>{label}</Link>
    </li>
  );
}
```

**Related:** [REACT-073 Programmatic navigation](#react-073) | [REACT-066 React Router](#react-066)

**Source:** GreatFrontEnd Q75

---

## REACT-076

### How do you handle 404 errors or page not found in React Router?

Use a catch-all route with `path="*"` as the last route in your `<Routes>`. React Router matches routes in order and falls through to `*` if nothing else matches.

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/users/:id" element={<UserProfile />} />
  <Route path="*" element={<NotFound />} /> {/* must be last */}
</Routes>

function NotFound() {
  return (
    <div>
      <h1>404 — Page Not Found</h1>
      <p>The page you're looking for doesn't exist.</p>
      <Link to="/">Return Home</Link>
    </div>
  );
}
```

**Nested 404 — within a layout:**
```jsx
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route index element={<DashboardHome />} />
  <Route path="analytics" element={<Analytics />} />
  <Route path="*" element={<DashboardNotFound />} /> {/* 404 within /dashboard/* */}
</Route>
```

**Redirect unknown routes to home:**
```jsx
<Route path="*" element={<Navigate to="/" replace />} />
```

**With error handling (v6.4 data router):**
```jsx
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <ErrorPage />, // handles 404 + thrown errors
    children: [...],
  },
]);

// ErrorPage — check error type
function ErrorPage() {
  const error = useRouteError();
  if (error.status === 404) return <NotFound />;
  return <GenericError error={error} />;
}
```

**Related:** [REACT-066 React Router](#react-066) | [REACT-037 Error Boundaries](#react-037)

**Source:** GreatFrontEnd Q76

---

## REACT-077

### How do you get query parameters in React Router?

Use the `useSearchParams` hook (React Router v6), which returns a `URLSearchParams` instance and a setter function — similar to `useState` for the URL query string.

```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();

  const query = searchParams.get('q') ?? '';
  const page = Number(searchParams.get('page') ?? 1);
  const sort = searchParams.get('sort') ?? 'relevance';

  function handleQueryChange(e) {
    // Update query string without losing other params
    setSearchParams(prev => {
      prev.set('q', e.target.value);
      prev.set('page', '1'); // reset page on new query
      return prev;
    });
  }

  return (
    <div>
      <input value={query} onChange={handleQueryChange} />
      <p>Showing page {page}, sorted by {sort}</p>
    </div>
  );
}
```

**Reading from `useLocation` (manual approach):**
```jsx
import { useLocation } from 'react-router-dom';

function Component() {
  const { search } = useLocation();
  const params = new URLSearchParams(search);
  const tab = params.get('tab');
  // ...
}
```

**Setting multiple params at once:**
```jsx
setSearchParams({ q: 'react', page: '2', sort: 'date' });
// → URL becomes: /search?q=react&page=2&sort=date
```

**Related:** [REACT-067 Dynamic routing](#react-067) | [REACT-073 Programmatic navigation](#react-073)

**Source:** GreatFrontEnd Q77

---

## REACT-078

### How do you perform an automatic redirect after login in React Router?

Preserve the user's intended destination in `location.state` when redirecting to login, then navigate there after successful authentication.

```jsx
// Step 1 — Private route saves the intended path in location state
function PrivateRoute({ children }) {
  const { isAuthenticated } = useAuth();
  const location = useLocation();

  if (!isAuthenticated) {
    return (
      <Navigate
        to="/login"
        state={{ from: location }}  // save intended destination
        replace
      />
    );
  }

  return children;
}

// Step 2 — Login form reads location.state and redirects after login
function LoginPage() {
  const { login } = useAuth();
  const navigate = useNavigate();
  const location = useLocation();

  const from = location.state?.from?.pathname ?? '/dashboard';

  async function handleSubmit(credentials) {
    await login(credentials);
    navigate(from, { replace: true }); // redirect to intended page
  }

  return (
    <form onSubmit={handleSubmit}>
      {/* login form fields */}
    </form>
  );
}
```

**Why `replace: true`?** After login, replacing the history entry means the user can't press Back to return to the login page.

**Related:** [REACT-074 Route guards](#react-074) | [REACT-073 Programmatic navigation](#react-073) | [REACT-072 push vs replace](#react-072)

**Source:** GreatFrontEnd Q78

---

## REACT-079

### How do you pass props to a route component in React Router?

In React Router v6, pass props directly in the `element` prop — the element is JSX, so you can pass any props you want.

**Direct prop passing:**
```jsx
<Route
  path="/users/:id"
  element={<UserProfile showAvatar={true} theme="dark" />}
/>
```

**With URL params (from `useParams` inside the component):**
```jsx
function UserProfile({ showAvatar, theme }) {
  const { id } = useParams(); // URL param — not passed as prop
  // ...
}
```

**Wrapper function for complex prop logic:**
```jsx
<Route
  path="/admin"
  element={<AdminPanel user={currentUser} permissions={userPermissions} />}
/>
```

**Using route `loader` data (v6.4+) instead of props:**
```jsx
const router = createBrowserRouter([{
  path: '/users/:id',
  element: <UserProfile />,
  loader: async ({ params }) => fetchUser(params.id),
}]);

function UserProfile() {
  const user = useLoaderData(); // data is available without prop drilling
  return <div>{user.name}</div>;
}
```

**Context as an alternative for deeply shared data:**
If every route needs access to the same data (e.g., current user), provide it via Context rather than threading it through route props.

**Related:** [REACT-066 React Router](#react-066) | [REACT-050 Prop Drilling](#react-050) | [REACT-033 useContext](#react-033)

**Source:** GreatFrontEnd Q79

---

*← [Advanced Concepts](./03-advanced.md) | [Master Index](../reference/00-master-index.md) | [Internationalization →](./05-i18n.md)*
