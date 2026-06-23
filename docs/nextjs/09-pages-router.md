# Next.js — Pages Router

> **IDs:** NEXT-050, NEXT-075, NEXT-082 – NEXT-101
> **Source:** MRH-NJS (mrhrifat/nextjs-interview-questions)

---

## NEXT-050

### What is the `<Head>` component (`next/head`) in Next.js?

🟢 Beginner

`next/head` exports a `<Head>` component used exclusively in the **Pages Router** to inject elements into the HTML `<head>` — such as `<title>`, `<meta>`, `<link>`, and `<script>` tags — on a per-page basis.

```tsx
import Head from 'next/head';

export default function AboutPage() {
  return (
    <>
      <Head>
        <title>About Us</title>
        <meta name="description" content="Learn more about our company" />
        <link rel="canonical" href="https://example.com/about" />
      </Head>
      <main>...</main>
    </>
  );
}
```

**Key points:**
- Only available in the Pages Router — in the App Router use the `metadata` export instead
- Multiple `<Head>` components across the tree are merged; duplicates are deduplicated by `key` prop
- Cannot inject arbitrary HTML outside of valid `<head>` child elements

**Related:** NEXT-029 — metadata in App Router | NEXT-082 — Pages Router routes

**Source:** MRH-NJS-C32

---

## NEXT-075

### What is the difference between the `pages` and `components` directories in Next.js?

🟢 Beginner

| Directory | Purpose |
|---|---|
| `pages/` | **Routing** — every file becomes a publicly accessible URL route. Special files (`_app.js`, `_document.js`, `_error.js`) control app-wide behaviour. |
| `components/` | **Reusable UI** — React components shared across pages. Not route-mapped; no convention enforced by Next.js. |

The `pages/` directory is a Next.js convention with real routing semantics. The `components/` directory is a community convention — you could name it `ui/`, `shared/`, or anything else; Next.js does not care.

**Related:** NEXT-004 — file-based routing | NEXT-082 — creating pages routes

**Source:** MRH-NJS-C69

---

## NEXT-082

### How do you create a route in the Pages Router?

🟢 Beginner

In the Pages Router every file inside the `pages/` directory automatically becomes a route. The file name maps directly to the URL path.

```
pages/index.tsx        →  /
pages/about.tsx        →  /about
pages/blog/index.tsx   →  /blog
pages/blog/[slug].tsx  →  /blog/:slug  (dynamic)
```

Each file must export a default React component:

```tsx
// pages/about.tsx
export default function AboutPage() {
  return <h1>About Us</h1>;
}
```

**Related:** NEXT-004 — App Router file-based routing | NEXT-083 — catch-all segments | NEXT-084 — _app.js

**Source:** MRH-NJS-P02

---

## NEXT-083

### What is a catch-all segment in Next.js?

🟡 Intermediate

A catch-all segment matches **any number of path segments** after a given prefix. In the Pages Router it uses the `[...param]` syntax; in the App Router it uses the same `[...param]` folder name.

```
pages/docs/[...slug].tsx
```

This matches `/docs/intro`, `/docs/api/auth`, `/docs/a/b/c`, etc. The `slug` param is always an **array** of strings.

```tsx
import { useRouter } from 'next/router';

export default function DocsPage() {
  const { slug } = useRouter().query; // ['api', 'auth'] for /docs/api/auth
  return <div>{slug?.join(' / ')}</div>;
}
```

**Optional catch-all** (`[[...slug]]`) also matches the route with no segments at all (`/docs`).

**Related:** NEXT-006 — dynamic routes | NEXT-082 — Pages Router routes

**Source:** MRH-NJS-P04

---

## NEXT-084

### What is the `_app.js` file in Next.js?

🟢 Beginner

`pages/_app.js` (or `_app.tsx`) is the **global app wrapper** in the Pages Router. Next.js wraps every page component with the default export from `_app.js`. It is the right place to:

- Add global CSS imports
- Wrap all pages with a shared layout (header, footer, sidebar)
- Provide a global context (theme, auth, Redux store)
- Persist state between page navigations

```tsx
// pages/_app.tsx
import type { AppProps } from 'next/app';
import '../styles/globals.css';
import { ThemeProvider } from '@/context/theme';

export default function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ThemeProvider>
      <Component {...pageProps} />
    </ThemeProvider>
  );
}
```

**Related:** NEXT-085 — _document.js | NEXT-086 — _app vs _document

**Source:** MRH-NJS-P05

---

## NEXT-085

### What is the `_document.js` file in Next.js?

🟡 Intermediate

`pages/_document.js` customises the **HTML document structure** rendered by the server — the `<html>`, `<head>`, and `<body>` tags that wrap every page. It is rendered **only on the server** and is not re-run on client-side navigation.

Use `_document.js` to:
- Set the `lang` attribute on `<html>`
- Add custom fonts via `<link>` in `<head>`
- Inject third-party scripts that must appear before the body
- Integrate CSS-in-JS libraries (e.g., styled-components, emotion) that require server-side style flushing

```tsx
// pages/_document.tsx
import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() {
  return (
    <Html lang="en">
      <Head>
        <link rel="preconnect" href="https://fonts.googleapis.com" />
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

**Related:** NEXT-084 — _app.js | NEXT-086 — _app vs _document

**Source:** MRH-NJS-P06

---

## NEXT-086

### What is the difference between `_app.js` and `_document.js`?

🟡 Intermediate

| | `_app.js` | `_document.js` |
|---|---|---|
| **Runs on** | Server + Client | Server only |
| **Purpose** | Wrap pages with global layout / context | Customise the HTML `<html>/<head>/<body>` shell |
| **Re-runs on navigation** | Yes — re-renders on every client route change | No — static server render only |
| **Global CSS** | ✅ Yes (only place allowed) | ❌ No |
| **Context providers** | ✅ Yes | ❌ No |
| **Custom `<html lang>`** | ❌ No | ✅ Yes |
| **CSS-in-JS style flushing** | ❌ No | ✅ Yes |

**Rule of thumb:** if it runs every page render → `_app.js`. If it's a one-time HTML shell customisation → `_document.js`.

**Related:** NEXT-084 — _app.js | NEXT-085 — _document.js

**Source:** MRH-NJS-P07

---

## NEXT-087

### What is the `_error.js` file in Next.js? How do you create custom error pages?

🟡 Intermediate

`pages/_error.js` is a custom error page that handles **both client-side and server-side errors** that are not caught by page-level components. It receives a `statusCode` prop from the server.

```tsx
// pages/_error.tsx
function Error({ statusCode }: { statusCode: number }) {
  return (
    <p>
      {statusCode
        ? `Server error ${statusCode}`
        : 'A client-side error occurred'}
    </p>
  );
}

Error.getInitialProps = ({ res, err }: any) => {
  const statusCode = res ? res.statusCode : err ? err.statusCode : 404;
  return { statusCode };
};

export default Error;
```

**Specialised error pages:**
- `pages/404.tsx` — custom 404 (statically generated, preferred over `_error` for 404s)
- `pages/500.tsx` — custom 500 server error page

**Related:** NEXT-088 — 404 page | NEXT-009 — App Router error.js

**Source:** MRH-NJS-P08

---

## NEXT-088

### How do you create a 404 page in the Pages Router?

🟢 Beginner

Create `pages/404.tsx`. Next.js statically generates this file at build time and serves it automatically for any unmatched route.

```tsx
// pages/404.tsx
export default function Custom404() {
  return (
    <div>
      <h1>404 — Page Not Found</h1>
      <p>The page you are looking for does not exist.</p>
      <a href="/">Go home</a>
    </div>
  );
}
```

**Key points:**
- Cannot use `getServerSideProps` in `pages/404.tsx` — it is always static
- For a custom 500 page use `pages/500.tsx`
- `_error.js` is a fallback for other status codes and dynamic error scenarios

**Related:** NEXT-087 — _error.js | NEXT-010 — not-found in App Router

**Source:** MRH-NJS-P09

---

## NEXT-089

### How do you fetch data in a Pages Router page?

🟡 Intermediate

The Pages Router provides three data-fetching functions that you export alongside the page component:

| Function | Runs on | When to use |
|---|---|---|
| `getStaticProps` | Build time (server) | Static content that rarely changes |
| `getServerSideProps` | Every request (server) | Per-request dynamic data |
| `getStaticPaths` + `getStaticProps` | Build time (server) | Dynamic routes with static generation |

```tsx
// Static generation
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data }, revalidate: 60 };
}

// Server-side rendering
export async function getServerSideProps(context) {
  const data = await fetchData(context.params.id);
  return { props: { data } };
}

export default function Page({ data }) {
  return <div>{JSON.stringify(data)}</div>;
}
```

For client-side fetching use `useEffect` + `fetch` or SWR/React Query.

**Related:** NEXT-090 — getStaticProps | NEXT-091 — getServerSideProps | NEXT-019 — client-side fetching

**Source:** MRH-NJS-P10

---

## NEXT-090

### What is `getStaticProps`?

🟡 Intermediate

`getStaticProps` is a Pages Router function that runs at **build time on the server** to fetch data and pass it as props to the page component. The page is then pre-rendered as static HTML.

```tsx
export async function getStaticProps(context) {
  const { params, locale, preview } = context;
  const post = await getPost(params.slug);

  if (!post) return { notFound: true }; // renders 404

  return {
    props: { post },
    revalidate: 60, // ISR: re-generate after 60 seconds
  };
}

export default function BlogPost({ post }) {
  return <article>{post.title}</article>;
}
```

**Return shape options:**
- `{ props }` — pass data to the component
- `{ notFound: true }` — render 404
- `{ redirect: { destination } }` — redirect
- `revalidate` — enable ISR (seconds until re-generation)

**Related:** NEXT-091 — getServerSideProps | NEXT-092 — comparison | NEXT-093 — getStaticPaths

**Source:** MRH-NJS-P11

---

## NEXT-091

### What is `getServerSideProps`?

🟡 Intermediate

`getServerSideProps` runs on the **server on every request**. It is the Pages Router equivalent of a Server Component with no caching — the page is server-rendered fresh for each visitor.

```tsx
export async function getServerSideProps(context) {
  const { req, res, params, query } = context;
  const session = await getSession(req); // auth check

  if (!session) {
    return { redirect: { destination: '/login', permanent: false } };
  }

  const data = await fetchUserData(session.userId);
  return { props: { data } };
}

export default function Dashboard({ data }) {
  return <div>{data.name}</div>;
}
```

**When to use:** data that must be fresh on every request (logged-in user data, real-time prices, per-request auth).

**Cost:** every page load hits your server — slower TTFB than static pages.

**Related:** NEXT-090 — getStaticProps | NEXT-092 — comparison

**Source:** MRH-NJS-P12

---

## NEXT-092

### What is the difference between `getStaticProps` and `getServerSideProps`?

🟡 Intermediate

| | `getStaticProps` | `getServerSideProps` |
|---|---|---|
| **Runs** | Build time (+ ISR re-generation) | Every HTTP request |
| **Output** | Static HTML — served from CDN | Dynamically rendered HTML per request |
| **Performance** | Fastest (CDN-cached) | Slower (server computation per request) |
| **Data freshness** | Stale until rebuild / ISR interval | Always fresh |
| **Access to request** | ❌ No `req`/`res` | ✅ Full `req`/`res`/cookies |
| **Use cases** | Blog posts, docs, marketing pages | Dashboards, auth-gated pages, real-time data |

**Rule of thumb:** prefer `getStaticProps` + `revalidate` (ISR) whenever possible; fall back to `getServerSideProps` only when you genuinely need per-request server access.

**Related:** NEXT-090 — getStaticProps | NEXT-091 — getServerSideProps | NEXT-020 — ISR

**Source:** MRH-NJS-P13

---

## NEXT-093

### What is `getStaticPaths`?

🟡 Intermediate

`getStaticPaths` is used alongside `getStaticProps` for **dynamic routes** in the Pages Router. It tells Next.js which dynamic path values to pre-render at build time.

```tsx
// pages/blog/[slug].tsx
export async function getStaticPaths() {
  const posts = await getAllPosts();
  return {
    paths: posts.map((p) => ({ params: { slug: p.slug } })),
    fallback: 'blocking', // or false | true
  };
}

export async function getStaticProps({ params }) {
  const post = await getPost(params.slug);
  return { props: { post } };
}

export default function BlogPost({ post }) {
  return <h1>{post.title}</h1>;
}
```

**Related:** NEXT-090 — getStaticProps | NEXT-094 — fallback option

**Source:** MRH-NJS-P14

---

## NEXT-094

### What is the `fallback` option in `getStaticPaths`?

🟡 Intermediate

The `fallback` key in the return value of `getStaticPaths` controls what happens when a visitor requests a path that was **not** pre-rendered at build time.

| Value | Behaviour |
|---|---|
| `false` | Any path not in `paths` returns a 404 page |
| `true` | Next.js serves a fallback page instantly; generates the full page in the background; future requests get the generated page |
| `'blocking'` | Next.js renders the page on the server on first request (no fallback UI); caches it for subsequent requests |

```tsx
return {
  paths: [{ params: { slug: 'hello-world' } }],
  fallback: 'blocking',
};
```

**Use `'blocking'`** when you have too many dynamic pages to pre-render all at build time but still want SSR-quality SEO on first load.

**Related:** NEXT-093 — getStaticPaths | NEXT-020 — ISR

**Source:** MRH-NJS-P15

---

## NEXT-095

### How do you handle API routes in the Pages Router?

🟡 Intermediate

In the Pages Router, API routes live in `pages/api/`. Each file exports a default handler function that receives `req` and `res` objects (Node.js `IncomingMessage` / `ServerResponse`).

```tsx
// pages/api/users.ts
import type { NextApiRequest, NextApiResponse } from 'next';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'GET') {
    return res.status(200).json({ users: [] });
  }
  if (req.method === 'POST') {
    const { name } = req.body;
    // create user...
    return res.status(201).json({ name });
  }
  res.setHeader('Allow', ['GET', 'POST']);
  res.status(405).end(`Method ${req.method} Not Allowed`);
}
```

**Key points:**
- API routes are never included in the client bundle
- Supports dynamic routes: `pages/api/users/[id].ts`
- In App Router the equivalent is Route Handlers (`app/api/route.ts`)

**Related:** NEXT-026 — API Routes overview | NEXT-028 — Route Handlers (App Router)

**Source:** MRH-NJS-P16

---

## NEXT-096

### Are there any limitations of the Pages Router?

🟡 Intermediate

- **No React Server Components** — all data fetching uses special functions (`getStaticProps`, `getServerSideProps`), not native async components
- **No nested layouts** — sharing layouts requires manual wrapping in `_app.js`; cannot co-locate layout with routes
- **No co-located loading / error UI** — no `loading.js` or `error.js` per route segment
- **No streaming** — pages are fully rendered before being sent; no Suspense-driven partial streaming
- **`_app.js` bottleneck** — all global providers must funnel through a single file
- **Larger client JS** — data-fetching code ships as props, not direct server reads
- **`getServerSideProps` couples UX to server latency** — every request waits for data before HTML is sent

**Related:** NEXT-003 — Pages Router vs App Router | NEXT-101 — when to choose each

**Source:** MRH-NJS-P18

---

## NEXT-097

### How do you handle form submissions in the Pages Router?

🟡 Intermediate

In the Pages Router, forms are handled client-side with a fetch/axios call to a `pages/api/` endpoint, or server-side with a native HTML form `action`.

```tsx
// pages/api/contact.ts
export default function handler(req, res) {
  if (req.method !== 'POST') return res.status(405).end();
  const { name, email } = req.body;
  // save to DB...
  res.status(200).json({ ok: true });
}

// pages/contact.tsx (client-side submission)
"use client" // not needed in pages router — shown for contrast
export default function ContactPage() {
  async function handleSubmit(e) {
    e.preventDefault();
    const data = Object.fromEntries(new FormData(e.target));
    await fetch('/api/contact', { method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    });
  }
  return <form onSubmit={handleSubmit}>...</form>;
}
```

**Related:** NEXT-095 — API routes | NEXT-102 — form submissions in App Router

**Source:** MRH-NJS-P21

---

## NEXT-098

### What performance optimizations are available in the Pages Router?

🟡 Intermediate

- **`getStaticProps` + ISR** — serve pages from CDN, regenerate only when stale
- **`next/image`** — automatic WebP conversion, lazy loading, `srcset`
- **`next/link` prefetching** — prefetch linked pages in the viewport
- **`next/dynamic`** — lazy-load heavy components (`ssr: false` for client-only)
- **Automatic code splitting** — each page gets its own JS bundle
- **`next/script` with `strategy`** — defer or lazy-load third-party scripts
- **`next/font`** — self-host Google Fonts with zero layout shift
- **Bundle analyzer** — `@next/bundle-analyzer` to identify large modules

**Related:** NEXT-076 — general performance techniques | NEXT-030 — image optimization

**Source:** MRH-NJS-P22

---

## NEXT-099

### How do you handle internationalization (i18n) in the Pages Router?

🟡 Intermediate

The Pages Router has **built-in i18n routing** configured in `next.config.js`:

```js
// next.config.js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
    localeDetection: true,
  },
};
```

Next.js automatically prefixes routes with the locale (`/fr/about`, `/de/about`). The current locale is available via `useRouter().locale` or in `getStaticProps` / `getServerSideProps` context.

```tsx
import { useRouter } from 'next/router';
export default function Page() {
  const { locale, locales, defaultLocale } = useRouter();
  return <p>Current locale: {locale}</p>;
}
```

For translated strings, pair with `next-i18next` or `react-intl`.

**Related:** NEXT-059 — i18n general | NEXT-066 — next-i18next | NEXT-073 — useTranslation

**Source:** MRH-NJS-P23

---

## NEXT-100

### How does caching and revalidation work in the Pages Router?

🟡 Intermediate

**Static pages (`getStaticProps`):**
- Cached at the CDN / file system after build
- **ISR revalidation** — add `revalidate: N` to regenerate the page after `N` seconds on next request

```tsx
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data }, revalidate: 60 }; // stale after 60s
}
```

**On-demand revalidation** — call `res.revalidate('/path')` from an API route to force immediate regeneration (e.g., triggered by a CMS webhook).

**Server-side pages (`getServerSideProps`):**
- Not cached by default — rendered fresh per request
- Add `Cache-Control` headers manually via `res.setHeader` to enable CDN caching

**Client-side fetching:**
- Use SWR's `revalidateOnFocus`, `refreshInterval`, or React Query's `staleTime` / `cacheTime`

**Related:** NEXT-020 — ISR | NEXT-021 — cache tags in App Router

**Source:** MRH-NJS-P26

---

## NEXT-101

### When should you choose Pages Router vs App Router?

🟡 Intermediate

| Choose **Pages Router** when… | Choose **App Router** when… |
|---|---|
| You are maintaining a legacy codebase that already uses it | Starting a new project (App Router is the recommended default) |
| Your team is more familiar with `getStaticProps` / `getServerSideProps` patterns | You need React Server Components, streaming, or nested layouts |
| You rely on third-party libraries not yet compatible with RSC | You want co-located `loading.js`, `error.js`, and `layout.js` per route |
| You use class-based patterns or older React patterns | You want to use Server Actions to replace API routes |
| You need `getInitialProps` for SSR with access to `req` | You want the best performance with the latest Next.js features |

**Both routers can coexist** in the same project during incremental migration.

**Related:** NEXT-003 — App Router overview | NEXT-096 — Pages Router limitations | NEXT-110 — App Router limitations

**Source:** MRH-NJS-P29

---

*← [08-infra](./08-infra.md) | [10-config-tooling →](./10-config-tooling.md)*
