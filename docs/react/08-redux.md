# React Redux

> Canonical answers for REACT-111 to REACT-135.
> Sources: [SudheerJ](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-111

### What is Redux?

Redux is a predictable state container for JavaScript applications. It centralizes application state in a single store and enforces a strict unidirectional data flow: UI dispatches an **action** → a **reducer** computes a new state → subscribers re-render.

Redux was inspired by Flux but simplifies it to a single store and pure reducer functions. It is framework-agnostic, though it is most commonly used with React via the `react-redux` library.

```js
import { createSlice, configureStore } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
  },
});

const store = configureStore({ reducer: { counter: counterSlice.reducer } });

store.dispatch(counterSlice.actions.increment());
console.log(store.getState()); // { counter: { value: 1 } }
```

**Related:** [REACT-044 — Flux](./03-advanced.md#react-044) | [REACT-112 — Redux principles](#react-112) | [REACT-118 — Context vs Redux](#react-118)

**Source:** [SudheerJ SDJ-103](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-112

### What are the core principles of Redux?

Redux is built on three fundamental principles:

**1. Single source of truth.** The entire application state lives in one object tree inside a single store. This makes debugging and state inspection straightforward.

**2. State is read-only.** The only way to change state is by dispatching an action — a plain object describing what happened (`{ type: 'counter/increment' }`). This ensures all mutations are intentional and traceable.

**3. Changes are made with pure functions.** Reducers take the previous state and an action and return a new state without mutating the original. Pure functions make state changes predictable and testable.

```js
// Reducer — pure function, no side effects
function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'increment': return { value: state.value + 1 };
    case 'decrement': return { value: state.value - 1 };
    default: return state;
  }
}
```

**Related:** [REACT-111 — What is Redux](#react-111) | [REACT-119 — Why "reducers"](#react-119) | [REACT-135 — Actions](#react-135)

**Source:** [SudheerJ SDJ-104](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-113

### What are the downsides of Redux compared to Flux?

Redux simplifies Flux but introduces its own trade-offs:

**Boilerplate.** Traditional Redux required manually writing action type constants, action creators, and reducer switch statements for every feature. Redux Toolkit (RTK) greatly reduces this, but older codebases can be verbose.

**Steeper learning curve.** Concepts like middleware, thunks, selectors, and normalizing state are non-trivial for newcomers.

**Overkill for small apps.** For apps with simple or localized state, React's built-in `useState` + `useContext` is sufficient. Reaching for Redux adds complexity without proportional benefit.

**Immutability discipline required.** Reducers must return new objects rather than mutating state. Forgetting this causes subtle bugs. RTK's `createSlice` uses Immer internally, which helps.

**No built-in async.** Redux itself is synchronous. Async logic requires middleware like `redux-thunk` (bundled in RTK) or `redux-saga`.

**Related:** [REACT-111 — What is Redux](#react-111) | [REACT-121 — When to use Redux](#react-121) | [REACT-125 — redux-saga](#react-125)

**Source:** [SudheerJ SDJ-105, SDJ-109](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-114

### What is the difference between mapStateToProps() and mapDispatchToProps()?

These are arguments to the `connect()` function from `react-redux` (legacy API before hooks).

**`mapStateToProps(state, ownProps?)`** reads from the store and passes selected state slices as props. It re-runs whenever the store updates.

**`mapDispatchToProps(dispatch, ownProps?)`** provides functions that dispatch actions. It can be an object (action creators auto-wrapped with `dispatch`) or a function returning an object.

```js
// Legacy connect() pattern
const mapStateToProps = (state) => ({
  count: state.counter.value,
});

const mapDispatchToProps = {
  increment: counterSlice.actions.increment,
  decrement: counterSlice.actions.decrement,
};

export default connect(mapStateToProps, mapDispatchToProps)(CounterComponent);
```

Modern hooks replace both: `useSelector` replaces `mapStateToProps`; `useDispatch` replaces `mapDispatchToProps`.

```js
// Modern hooks pattern
const count = useSelector((state) => state.counter.value);
const dispatch = useDispatch();
```

**Related:** [REACT-111 — Redux](#react-111) | [REACT-122 — Component vs container](#react-122)

**Source:** [SudheerJ SDJ-106, SDJ-119, SDJ-120](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-115

### Can I dispatch an action in a reducer?

No — and you shouldn't. Reducers must be pure functions: given the same input they return the same output, with no side effects. Dispatching an action inside a reducer is a side effect and violates this contract. It can also cause infinite loops.

If you need to dispatch multiple actions in response to one event, dispatch them sequentially from the component or middleware:

```js
// In a thunk — dispatch multiple actions safely
export const resetAndLoad = () => async (dispatch) => {
  dispatch(reset());       // action 1
  const data = await fetchData();
  dispatch(setData(data)); // action 2
};
```

Redux Toolkit's `createListenerMiddleware` is another option for reacting to one action by dispatching others.

**Related:** [REACT-112 — Redux principles](#react-112) | [REACT-127 — Redux Thunk](#react-127) | [REACT-125 — redux-saga](#react-125)

**Source:** [SudheerJ SDJ-107](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-116

### How to access the Redux store outside a component?

The recommended approach is to export the store instance and import it where needed:

```js
// store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
  reducer: { counter: counterReducer },
});

// Anywhere else — plain JS file, utility, service
import { store } from './store';

const state = store.getState();
store.dispatch({ type: 'counter/increment' });
store.subscribe(() => console.log(store.getState()));
```

This pattern is common in utilities like API clients, analytics helpers, or legacy code that sits outside the React component tree. Inside components, always prefer `useSelector` and `useDispatch`.

**Related:** [REACT-111 — Redux](#react-111) | [REACT-114 — mapStateToProps](#react-114)

**Source:** [SudheerJ SDJ-108, SDJ-116](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-117

### How to reset Redux state?

**Option 1 — Handle a reset action in every reducer:**

```js
const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: { /* ... */ },
  extraReducers: (builder) => {
    builder.addCase(resetAll, () => initialState);
  },
});
```

**Option 2 — Root reducer that resets the entire state tree:**

```js
const rootReducer = (state, action) => {
  if (action.type === 'RESET_ALL') {
    state = undefined; // Return undefined → all reducers use their initialState
  }
  return appReducer(state, action);
};
```

Option 2 is useful for logout flows where you want to wipe all user data in one dispatch.

**Related:** [REACT-112 — Redux principles](#react-112) | [REACT-133 — Initial state](#react-133)

**Source:** [SudheerJ SDJ-111](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-118

### What is the difference between React Context and React Redux?

| Aspect | React Context | Redux |
|---|---|---|
| Primary purpose | Avoid prop drilling; pass data down | Centralized global state management |
| Performance | Re-renders all consumers on any context change | Granular subscriptions via `useSelector` |
| Dev tools | None built-in | Redux DevTools with time-travel debugging |
| Async support | Manual (useEffect + fetch) | Middleware (thunk, saga) |
| Middleware / plugins | None | Rich ecosystem |
| Best for | Theme, auth, locale (low-frequency updates) | Complex domain state, shared mutations |

Context is not a Redux replacement — it's a prop-drilling solution. For frequent state updates across many components, Context can cause cascading re-renders; Redux's selector system avoids this.

**Related:** [REACT-033 — useContext](./02-hooks.md#react-033) | [REACT-048 — State management decisions](./03-advanced.md#react-048) | [REACT-111 — Redux](#react-111)

**Source:** [SudheerJ SDJ-112](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-119

### Why are Redux state functions called reducers?

The name comes from the `Array.prototype.reduce` method. A reducer is a function that takes an accumulator and the current value and returns a new accumulator:

```js
[1, 2, 3].reduce((acc, val) => acc + val, 0); // 6
```

Redux reducers follow the same pattern: they take the current state (accumulator) and an action (current value) and return the next state:

```js
function reducer(state = initialState, action) {
  // state = accumulator, action = current value
  switch (action.type) {
    case 'increment': return { ...state, count: state.count + 1 };
    default: return state;
  }
}
```

The `createStore` function essentially calls `reduce` over all dispatched actions to arrive at the current state.

**Related:** [REACT-112 — Redux principles](#react-112) | [REACT-135 — Actions](#react-135)

**Source:** [SudheerJ SDJ-113](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-120

### How to make AJAX requests in Redux?

Redux itself is synchronous. Async logic (network requests) lives in **middleware**:

**Redux Thunk** (bundled in Redux Toolkit — simplest):

```js
// Async action creator (thunk)
export const fetchUser = (id) => async (dispatch) => {
  dispatch(fetchStart());
  try {
    const user = await api.getUser(id);
    dispatch(fetchSuccess(user));
  } catch (err) {
    dispatch(fetchError(err.message));
  }
};

// In component
dispatch(fetchUser(42));
```

**RTK Query** (built into Redux Toolkit — full data-fetching layer):

```js
export const userApi = createApi({
  reducerPath: 'userApi',
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  endpoints: (builder) => ({
    getUser: builder.query({ query: (id) => `/users/${id}` }),
  }),
});
```

RTK Query handles caching, loading states, and cache invalidation automatically.

**Related:** [REACT-125 — redux-saga](#react-125) | [REACT-127 — Redux Thunk](#react-127) | [REACT-132 — Middleware](#react-132)

**Source:** [SudheerJ SDJ-114](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-121

### Should I keep all component state in the Redux store?

No. A good heuristic: put state in Redux only if it needs to be shared across components that don't have a natural parent-child relationship, or if you need time-travel debugging / persistence.

**Keep in local state (`useState`):**
- Form input values (controlled inputs)
- UI-only state: modal open/closed, accordion expanded, hover state
- Async loading state that belongs to one component

**Put in Redux:**
- Authenticated user data consumed across the app
- Shopping cart / order state
- Shared filters or search queries that drive multiple views
- Data that needs to be persisted to localStorage or synced with a server

A useful test: if you re-mounted this component from scratch, would you need to restore this state? If yes → Redux (or another global store). If no → local state.

**Related:** [REACT-048 — State management decisions](./03-advanced.md#react-048) | [REACT-118 — Context vs Redux](#react-118)

**Source:** [SudheerJ SDJ-115](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-122

### What is the difference between a component and a container in Redux?

This distinction comes from the "presentational vs container" pattern, which predates hooks:

**Presentational component:** concerned only with how things look. Receives data and callbacks via props. Has no Redux awareness.

**Container component:** concerned with how things work. Connected to Redux via `connect()` or hooks. Extracts data from the store and dispatches actions.

```jsx
// Presentational — pure UI
function UserCard({ name, onFollow }) {
  return <button onClick={onFollow}>{name}</button>;
}

// Container — wired to Redux
function UserCardContainer({ userId }) {
  const user = useSelector(selectUser(userId));
  const dispatch = useDispatch();
  return <UserCard name={user.name} onFollow={() => dispatch(follow(userId))} />;
}
```

With hooks, the distinction has blurred — many components can now read from the store directly without a separate container. The pattern is still useful conceptually for separating UI from data concerns.

**Related:** [REACT-060 — Presentational vs container](./03-advanced.md#react-060) | [REACT-114 — mapStateToProps](#react-114)

**Source:** [SudheerJ SDJ-117](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-123

### What is the purpose of constants in Redux?

Action type constants prevent typos and enable IDE autocomplete. A misspelled string in a `switch` case fails silently — the action is dispatched but the reducer does nothing.

```js
// Without constants — typo-prone
dispatch({ type: 'USER_LOGED_IN' }); // Silent failure if case says 'USER_LOGGED_IN'

// With constants
export const USER_LOGGED_IN = 'USER_LOGGED_IN';
dispatch({ type: USER_LOGGED_IN }); // Reference error if typo — caught at dev time
```

Redux Toolkit's `createSlice` auto-generates action type strings (`counter/increment`) from the slice name and reducer key, eliminating the need to manually define constants while still providing autocomplete and type safety.

**Related:** [REACT-135 — Actions](#react-135) | [REACT-112 — Redux principles](#react-112)

**Source:** [SudheerJ SDJ-118](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-124

### How to structure Redux top-level directories?

Two common approaches:

**Feature-based (recommended by RTK team):**

```
src/
├── features/
│   ├── counter/
│   │   ├── counterSlice.js    reducer + actions
│   │   ├── Counter.jsx        component
│   │   └── counterSelectors.js
│   └── auth/
│       ├── authSlice.js
│       └── Login.jsx
└── app/
    └── store.js               combines all reducers
```

**Domain-based (older Ducks / "Rails" style):**

```
src/
├── actions/
│   ├── counterActions.js
│   └── authActions.js
├── reducers/
│   ├── counterReducer.js
│   └── authReducer.js
└── store.js
```

Feature-based is preferred because all code for a feature lives together — easier to delete a feature without hunting across folders.

**Related:** [REACT-111 — Redux](#react-111)

**Source:** [SudheerJ SDJ-121](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-125

### What is redux-saga?

`redux-saga` is a Redux middleware that handles side effects (async calls, background tasks) using ES6 generator functions. It models side effects as "sagas" — long-running processes that can be paused, cancelled, and composed.

```js
import { call, put, takeLatest } from 'redux-saga/effects';

function* fetchUserSaga(action) {
  try {
    const user = yield call(api.getUser, action.payload.id); // pause, wait for API
    yield put(fetchSuccess(user));                            // dispatch action
  } catch (err) {
    yield put(fetchError(err.message));
  }
}

function* watchFetchUser() {
  yield takeLatest('users/fetchRequest', fetchUserSaga); // cancel previous if new one arrives
}
```

Sagas are easier to test than thunks (you assert on yielded effects, not mock async calls) and support complex flows like cancellation, debouncing, and race conditions. The trade-off is a steeper learning curve.

**Related:** [REACT-126 — call() vs put()](#react-126) | [REACT-127 — Redux Thunk](#react-127) | [REACT-128 — saga vs thunk](#react-128)

**Source:** [SudheerJ SDJ-122, SDJ-123](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-126

### What are the differences between call() and put() in redux-saga?

Both are saga effect creators, but they do different things:

| Effect | Purpose | Behavior |
|---|---|---|
| `call(fn, ...args)` | Invoke a function (usually async) | Pauses the saga until the promise resolves |
| `put(action)` | Dispatch a Redux action | Triggers the reducer and any other watchers |

```js
function* mySaga() {
  const data = yield call(fetchApi, '/users'); // Pauses here until fetchApi resolves
  yield put(setUsers(data));                   // Dispatches action to store
}
```

`call` is the saga equivalent of `await`. `put` is the saga equivalent of `dispatch`. Keeping them separate makes sagas easy to test — you can assert the yielded effect objects without running actual API calls.

**Related:** [REACT-125 — redux-saga](#react-125)

**Source:** [SudheerJ SDJ-124](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-127

### What is Redux Thunk?

Redux Thunk is a middleware that lets action creators return functions instead of plain objects. The middleware intercepts functions and calls them with `dispatch` and `getState`, enabling async logic inside action creators.

Redux Toolkit includes `redux-thunk` by default via `configureStore`.

```js
// Thunk action creator
export const fetchUser = (id) => async (dispatch, getState) => {
  const { loading } = getState().users;
  if (loading) return; // avoid duplicate requests

  dispatch(fetchStart());
  const user = await api.getUser(id);
  dispatch(fetchSuccess(user));
};

// Usage
dispatch(fetchUser(42)); // Middleware intercepts, calls the function
```

RTK also provides `createAsyncThunk` which auto-generates `pending`, `fulfilled`, and `rejected` action types:

```js
export const fetchUser = createAsyncThunk('users/fetch', async (id) => {
  return await api.getUser(id);
});
```

**Related:** [REACT-125 — redux-saga](#react-125) | [REACT-128 — saga vs thunk](#react-128) | [REACT-120 — AJAX in Redux](#react-120)

**Source:** [SudheerJ SDJ-125](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-128

### What are the differences between redux-saga and redux-thunk?

| Aspect | redux-thunk | redux-saga |
|---|---|---|
| Syntax | `async/await` or Promise chains | Generator functions (`function*`, `yield`) |
| Learning curve | Low — familiar async patterns | High — generators, effect model |
| Testability | Requires mocking async functions | Assert on yielded effects (no mocks needed) |
| Complex flows | Manual (race conditions, cancellation are verbose) | Built-in: `race()`, `cancel()`, `takeLatest()` |
| Bundle size | ~1KB | ~15KB |
| Best for | Simple async CRUD operations | Complex orchestration, long-running processes |

Thunks are a natural default for most apps. Reach for sagas when you need cancellation, debounce at the effect level, polling, or WebSocket handling.

**Related:** [REACT-125 — redux-saga](#react-125) | [REACT-127 — Redux Thunk](#react-127)

**Source:** [SudheerJ SDJ-126](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-129

### What is Redux DevTools?

Redux DevTools is a browser extension that provides deep visibility into Redux state changes. It connects to any Redux store configured with the DevTools extension.

**Key features:**

- **Action log:** see every dispatched action with its payload in real time
- **State diff:** inspect how state changed for each action
- **Time-travel debugging:** jump to any previous state by clicking an action in the log
- **Action replay:** replay a sequence of actions to reproduce bugs
- **Import/export state:** save and share exact state snapshots
- **Skip actions:** remove an action from the log and see what state would be without it

**Enabling in RTK:**

```js
// configureStore enables Redux DevTools automatically in development
const store = configureStore({
  reducer: rootReducer,
  devTools: process.env.NODE_ENV !== 'production',
});
```

**Related:** [REACT-111 — Redux](#react-111) | [REACT-129 — DevTools](#react-129)

**Source:** [SudheerJ SDJ-127, SDJ-128](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-130

### What are Redux selectors and why use them?

A selector is a function that extracts a piece of data from the Redux state tree. Selectors encapsulate state shape, making components independent of how state is structured internally.

**Without selectors:**

```js
// Component knows about full state shape
const user = useSelector((state) => state.auth.user.profile.name);
```

**With selectors:**

```js
// authSelectors.js
export const selectUserName = (state) => state.auth.user.profile.name;

// Component only knows about the selector
const userName = useSelector(selectUserName);
```

**Memoized selectors with Reselect:**

```js
import { createSelector } from '@reduxjs/toolkit';

const selectItems = (state) => state.cart.items;
const selectDiscount = (state) => state.promo.discount;

export const selectTotal = createSelector(
  [selectItems, selectDiscount],
  (items, discount) => {
    const subtotal = items.reduce((sum, item) => sum + item.price, 0);
    return subtotal * (1 - discount);
  }
);
```

`createSelector` memoizes results — `selectTotal` recalculates only when `items` or `discount` actually change, preventing unnecessary re-renders.

**Related:** [REACT-140 — Reselect](./10-libraries.md#react-140) | [REACT-114 — mapStateToProps](#react-114)

**Source:** [SudheerJ SDJ-129](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-131

### What is Redux Form?

Redux Form (now largely superseded by React Hook Form and Formik) was a library that stored form state in the Redux store, enabling time-travel debugging of form interactions and cross-component form state.

**Key features:**
- Tracks field values, validation errors, touched/pristine state, and submission status in Redux
- Provides `<Field>` component to connect individual inputs to the store
- Supports sync and async validation

```jsx
// Legacy redux-form pattern
import { Field, reduxForm } from 'redux-form';

function LoginForm({ handleSubmit }) {
  return (
    <form onSubmit={handleSubmit}>
      <Field name="email" component="input" type="email" />
      <Field name="password" component="input" type="password" />
      <button type="submit">Login</button>
    </form>
  );
}

export default reduxForm({ form: 'login' })(LoginForm);
```

**Today:** Most teams use [React Hook Form](https://react-hook-form.com) (uncontrolled, tiny bundle) or [Formik](https://formik.org). Redux Form is rarely chosen for new projects because keeping form state in Redux adds unnecessary overhead.

**Related:** [REACT-014 — Controlled components](./01-fundamentals.md#react-014) | [REACT-111 — Redux](#react-111)

**Source:** [SudheerJ SDJ-130, SDJ-131](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-132

### How to add multiple middlewares to Redux?

With `configureStore` from Redux Toolkit, pass an array via `getDefaultMiddleware().concat(...)`:

```js
import { configureStore } from '@reduxjs/toolkit';
import logger from 'redux-logger';
import { sagaMiddleware } from './sagas';

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(sagaMiddleware, logger),
});
```

`getDefaultMiddleware()` includes `redux-thunk`, `serializability-check`, and `immutability-check` by default. Concatenating preserves those while adding yours.

With the legacy `createStore`:

```js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(thunk, logger, sagaMiddleware))
);
```

**Related:** [REACT-127 — Redux Thunk](#react-127) | [REACT-125 — redux-saga](#react-125)

**Source:** [SudheerJ SDJ-132](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-133

### How to set initial state in Redux?

**In the reducer / slice:**

```js
const initialState = { count: 0, user: null, loading: false };

const counterSlice = createSlice({
  name: 'counter',
  initialState,   // ← declared here
  reducers: { /* ... */ },
});
```

**Preloaded state from `configureStore`** (for hydrating from localStorage or server):

```js
const preloadedState = {
  counter: { count: JSON.parse(localStorage.getItem('count')) || 0 },
};

const store = configureStore({
  reducer: rootReducer,
  preloadedState,
});
```

`preloadedState` values override the reducer's `initialState` only for the matching slice keys. Reducers still receive their own `initialState` for any key not in `preloadedState`.

**Related:** [REACT-117 — Resetting state](#react-117) | [REACT-112 — Redux principles](#react-112)

**Source:** [SudheerJ SDJ-133](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-134

### How is Relay different from Redux?

Both Relay and Redux are data management tools, but they operate at different layers and solve different problems:

| Aspect | Redux | Relay |
|---|---|---|
| Data source | Any (API, local, derived) | GraphQL only |
| Coupling | Decoupled from data fetching | Tightly coupled to GraphQL schema |
| Data fetching | Manual (thunks, sagas, RTK Query) | Declarative — components declare data needs via fragments |
| Caching | Manual (you design the cache) | Automatic normalized object store |
| Colocation | Data logic lives in Redux files | Data declarations live next to the component |
| Best for | General application state | GraphQL-heavy apps with complex data graphs |

Relay is a Facebook-internal solution optimized for large GraphQL APIs. Most React apps use Redux or RTK Query for data fetching and don't need Relay.

**Related:** [REACT-111 — Redux](#react-111) | [REACT-120 — AJAX in Redux](#react-120)

**Source:** [SudheerJ SDJ-134, SDJ-152](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-135

### What is an action in Redux?

An action is a plain JavaScript object that describes an intent to change state. It must have a `type` property (a string identifier) and can carry any additional data in a `payload` (by convention).

```js
// Plain action objects
{ type: 'counter/increment' }
{ type: 'users/fetchSuccess', payload: { id: 1, name: 'Akar' } }
{ type: 'auth/logout' }
```

**Action creators** are functions that return action objects, making it easier to dispatch them correctly:

```js
const increment = () => ({ type: 'counter/increment' });
const fetchSuccess = (user) => ({ type: 'users/fetchSuccess', payload: user });

dispatch(increment());
dispatch(fetchSuccess({ id: 1, name: 'Akar' }));
```

Redux Toolkit's `createSlice` auto-generates action creators from your reducer definitions, so you rarely write them manually:

```js
const { increment, decrement } = counterSlice.actions;
dispatch(increment()); // Calls the auto-generated action creator
```

**Related:** [REACT-119 — Why "reducers"](#react-119) | [REACT-123 — Action type constants](#react-123) | [REACT-112 — Redux principles](#react-112)

**Source:** [SudheerJ SDJ-135](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

*← [07-react19.md](./07-react19.md) | [09-react-native.md →](./09-react-native.md)*
*[Master Index](../../docs/reference/00-master-index.md)*

---

## REACT-259

### Are there similarities between Redux and RxJS?

Both Redux and RxJS involve streams of events/actions that produce new states, but they operate at different abstraction levels:

| Aspect | Redux | RxJS |
|---|---|---|
| Focus | Application state management | Reactive programming / async streams |
| Core unit | Action (plain object) | Observable (event stream) |
| Composition | Reducers combine state slices | Operators compose streams |
| Subscriptions | `store.subscribe()` | `observable.subscribe()` |
| Middleware | Custom middleware (thunk, saga) | Operators (map, filter, debounce, mergeMap) |

**Similarities:**
- Both model state changes as sequences of events
- Both provide a way to react to changes (subscriptions)
- `redux-observable` is a middleware that brings RxJS into Redux — actions become Observables that can be composed with RxJS operators (called "Epics")

RxJS is a general-purpose reactive library; Redux is specifically for UI state management. They complement each other rather than compete.

**Related:** [REACT-111 — Redux](./08-redux.md#react-111) | [REACT-125 — redux-saga](./08-redux.md#react-125)

**Source:** [SudheerJ SDJ-110](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-260

### Can Redux be used without React?

Yes — Redux is a standalone JavaScript library with no dependency on React. It can be used with any UI framework or with vanilla JavaScript:

```js
// Pure JavaScript — no React
import { createStore } from 'redux';

function counterReducer(state = 0, action) {
  switch (action.type) {
    case 'increment': return state + 1;
    default: return state;
  }
}

const store = createStore(counterReducer);

store.subscribe(() => {
  document.getElementById('count').textContent = store.getState();
});

document.getElementById('btn').addEventListener('click', () => {
  store.dispatch({ type: 'increment' });
});
```

Redux is used with Vue (via `pinia` typically, but Redux works), Angular, Svelte, or any framework. The `react-redux` package is the React binding — it's optional.

**Related:** [REACT-111 — What is Redux](./08-redux.md#react-111)

**Source:** [SudheerJ SDJ-155](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-261

### Do you need a specific build tool to use Redux?

No. Redux is just a JavaScript package — it works in any environment that can run JavaScript and import npm packages:

```html
<!-- Browser via CDN — no build tool -->
<script src="https://unpkg.com/redux/dist/redux.min.js"></script>
<script>
  const store = Redux.createStore(reducer);
</script>
```

```js
// Node.js — no bundler needed
const { createStore } = require('redux');
```

In practice, React + Redux projects use Vite, webpack, or Create React App because:
- JSX requires transpilation (Babel/SWC)
- ES modules need bundling for broad browser support
- Tree-shaking removes unused Redux code

But Redux itself has zero build requirements.

**Related:** [REACT-111 — Redux](./08-redux.md#react-111) | [REACT-260 — Redux without React](#react-260)

**Source:** [SudheerJ SDJ-156](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

