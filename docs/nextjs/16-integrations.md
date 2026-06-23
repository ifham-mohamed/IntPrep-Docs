# Next.js Integrations

This file covers integrating Next.js with GraphQL, Redux Toolkit, RTK Query, and other third-party tools.

---

## NEXT-064

### How do you set up GraphQL in Next.js?

GraphQL in Next.js can be consumed as a client (fetching from an external API) or served via a Route Handler.

**Client-side with Apollo Client:**
```tsx
// app/providers.tsx
'use client';
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';

const client = new ApolloClient({
  uri: process.env.NEXT_PUBLIC_GRAPHQL_URL,
  cache: new InMemoryCache(),
});

export function ApolloWrapper({ children }: { children: React.ReactNode }) {
  return <ApolloProvider client={client}>{children}</ApolloProvider>;
}
```

**Server-side fetching in Server Components (simpler):**
```tsx
// No Apollo needed — just fetch
async function getUser(id: string) {
  const res = await fetch(process.env.GRAPHQL_URL!, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ query: `{ user(id: "${id}") { name email } }` }),
  });
  const { data } = await res.json();
  return data.user;
}
```

**Serving a GraphQL API via Route Handler:**
```ts
// app/api/graphql/route.ts
import { createYoga } from 'graphql-yoga';
import { schema } from '@/graphql/schema';
const { handleRequest } = createYoga({ schema, graphqlEndpoint: '/api/graphql' });
export { handleRequest as GET, handleRequest as POST };
```

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-019 — Data fetching in App Router](./04-data-fetching.md#next-019)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-51](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-128

### How do you use RTK Query with the Next.js App Router?

RTK Query is Redux Toolkit's built-in data-fetching layer. In the App Router, it works in Client Components:

```ts
// lib/store.ts
import { configureStore } from '@reduxjs/toolkit';
import { postsApi } from './postsApi';

export const store = configureStore({
  reducer: { [postsApi.reducerPath]: postsApi.reducer },
  middleware: (getDefault) => getDefault().concat(postsApi.middleware),
});
```

```ts
// lib/postsApi.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const postsApi = createApi({
  reducerPath: 'postsApi',
  baseQuery: fetchBaseQuery({ baseUrl: '/api' }),
  endpoints: (builder) => ({
    getPosts: builder.query<Post[], void>({ query: () => '/posts' }),
    createPost: builder.mutation<Post, Partial<Post>>({
      query: (body) => ({ url: '/posts', method: 'POST', body }),
    }),
  }),
});

export const { useGetPostsQuery, useCreatePostMutation } = postsApi;
```

```tsx
// app/providers.tsx — must be a Client Component
'use client';
import { Provider } from 'react-redux';
import { store } from '@/lib/store';
export function ReduxProvider({ children }) {
  return <Provider store={store}>{children}</Provider>;
}
```

Server Components should fetch directly (not via Redux), passing data as props to Client Components that use RTK Query for further mutations.

**Related:** [NEXT-129 — Redux Toolkit setup](./16-integrations.md#next-129) | [NEXT-115 — Global state management](./14-app-router-advanced.md#next-115)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-11](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-129

### How do you use Redux Toolkit with the Next.js App Router?

Redux Toolkit (RTK) works in the App Router but requires careful placement because the Redux store is a singleton — you must create it per request on the server, not as a module-level global.

```ts
// lib/store.ts — factory function, not singleton
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const makeStore = () => configureStore({
  reducer: { counter: counterReducer },
});

export type AppStore = ReturnType<typeof makeStore>;
export type RootState = ReturnType<AppStore['getState']>;
export type AppDispatch = AppStore['dispatch'];
```

```tsx
// app/StoreProvider.tsx
'use client';
import { useRef } from 'react';
import { Provider } from 'react-redux';
import { makeStore, AppStore } from '@/lib/store';

export default function StoreProvider({ children }: { children: React.ReactNode }) {
  const storeRef = useRef<AppStore>();
  if (!storeRef.current) storeRef.current = makeStore();
  return <Provider store={storeRef.current}>{children}</Provider>;
}
```

```tsx
// app/layout.tsx
import StoreProvider from './StoreProvider';
export default function RootLayout({ children }) {
  return <html><body><StoreProvider>{children}</StoreProvider></body></html>;
}
```

Using `useRef` ensures one store instance per StoreProvider mount, not a shared module-level store that would leak state between SSR requests.

**Related:** [NEXT-128 — RTK Query](./16-integrations.md#next-128) | [NEXT-115 — Global state management](./14-app-router-advanced.md#next-115)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-12](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [15-i18n.md](./15-i18n.md) | [17-testing.md →](./17-testing.md)*
