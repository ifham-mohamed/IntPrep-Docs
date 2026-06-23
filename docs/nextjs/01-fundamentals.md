# Next.js — Fundamentals & App Router

> **IDs:** NEXT-001 – NEXT-010
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-001

### What is Next.js and what does it add on top of React?

🟢 Beginner

Next.js is a production-grade React framework built by Vercel. Plain React is a UI library — it renders components but leaves routing, data fetching, bundling, and rendering strategy entirely to the developer. Next.js provides all of that out of the box.

Key additions over plain React:
- **File-based routing** — folder structure in `app/` defines URL structure automatically
- **Multiple rendering strategies** — SSR, SSG, ISR, and CSR per route, not globally
- **Server Components** — components that run exclusively on the server, reducing JS sent to the browser
- **Built-in optimizations** — image optimization (`next/image`), font loading (`next/font`), code splitting
- **Full-stack capability** — Route Handlers and Server Actions let you write backend logic in the same project
- **Streaming** — progressive HTML delivery via React Suspense boundaries

Next.js does not replace React — it is React, plus a conventions layer that makes production apps faster to build and better by default.

```tsx
// Plain React — you wire everything yourself
// ReactDOM.createRoot(document.getElementById('root')).render(<App />)

// Next.js — create app/page.tsx and the route / is ready
export default function HomePage() {
  return <h1>Hello from Next.js</h1>;
}
// No router setup, no build config, no server setup needed.
```

**Related:** NEXT-002 — Next.js vs plain React | NEXT-003 — App Router overview

**Source:** GFE-NJS-001

---

## NEXT-002

### How is Next.js different from a plain React app (e.g. a Vite app)?

🟢 Beginner

A Vite + React app is a Single-Page Application (SPA) by default — the server sends a minimal HTML shell and JavaScript builds the entire UI in the browser. Next.js is a hybrid framework that can render pages on the server, at build time, or in the browser depending on what each route needs.

| Dimension | Vite + React (SPA) | Next.js |
|---|---|---|
| Rendering | Client-side only | SSR / SSG / ISR / CSR per route |
| Routing | Manual (React Router) | File-based, built-in |
| SEO | Poor (empty HTML shell) | Excellent (server-rendered HTML) |
| Data fetching | `useEffect` / React Query | `async` Server Components, Server Actions |
| API layer | Separate backend needed | Route Handlers in same project |
| Bundle size | All JS up front | Automatic code splitting per route |

The practical impact: a Vite SPA shows a blank page until JS loads and executes. A Next.js page delivers fully rendered HTML immediately, improving First Contentful Paint and search engine indexability.

```tsx
// Vite SPA data fetching — runs in browser, not SEO-friendly
useEffect(() => {
  fetch('/api/products').then(r => r.json()).then(setProducts);
}, []);

// Next.js Server Component — runs on server, HTML arrives pre-populated
export default async function ProductsPage() {
  const products = await fetch('https://api.example.com/products').then(r => r.json());
  return <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

**Related:** NEXT-001 — What is Next.js | NEXT-011 — Server Components | NEXT-015 — SSR vs SSG vs CSR vs ISR

**Source:** GFE-NJS-002

---

## NEXT-003

### What is the App Router and how does it differ from the Pages Router?

🟢 Beginner

Next.js has had two routing systems. The **Pages Router** (pre-Next.js 13) used the `pages/` directory where every file became a route, and data fetching happened via special exports like `getServerSideProps` and `getStaticProps`. The **App Router** (introduced in Next.js 13, stable in 14) uses the `app/` directory and is built entirely on React Server Components.

Key differences:

| Feature | Pages Router | App Router |
|---|---|---|
| Directory | `pages/` | `app/` |
| Default component type | Client Component | Server Component |
| Data fetching | `getServerSideProps`, `getStaticProps` | `async/await` in Server Components |
| Layouts | Manual `_app.tsx` | Nested `layout.tsx` files |
| Streaming | Not supported | Native via Suspense |
| Loading states | Manual | `loading.tsx` convention |
| Error boundaries | Manual | `error.tsx` convention |

The App Router is the current standard. The Pages Router is still supported but is in maintenance mode. New projects should always use the App Router.

```
app/
├── layout.tsx        ← root layout (wraps everything)
├── page.tsx          ← route: /
├── about/
│   └── page.tsx      ← route: /about
└── blog/
    ├── layout.tsx    ← nested layout for /blog/*
    └── [slug]/
        └── page.tsx  ← route: /blog/:slug
```

**Related:** NEXT-004 — File-based routing | NEXT-005 — page.tsx vs layout.tsx vs template.tsx

**Source:** GFE-NJS-003

---

## NEXT-004

### What is file-based routing in Next.js?

🟢 Beginner

File-based routing means the URL structure of your application is determined by the folder and file structure inside the `app/` directory — not by any configuration file or imperative router setup. You never call `router.addRoute()` or write a routes array. You create a folder, put a `page.tsx` inside it, and that path is live.

Conventions:
- A folder represents a URL segment: `app/dashboard/` → `/dashboard`
- `page.tsx` inside a folder makes that route publicly accessible
- Folders without a `page.tsx` do not create a route (they can hold layouts, components, etc.)
- `[param]` folders create dynamic segments: `app/blog/[slug]/page.tsx` → `/blog/:slug`
- `(group)` folders create route groups that don't affect the URL
- `_folder` prefixes opt a folder out of routing entirely

```
app/
├── page.tsx              → /
├── about/page.tsx        → /about
├── blog/
│   ├── page.tsx          → /blog
│   └── [slug]/page.tsx   → /blog/my-post
├── (marketing)/          → no URL segment added
│   └── pricing/page.tsx  → /pricing
└── _components/          → not a route, shared components
    └── Card.tsx
```

**Related:** NEXT-003 — App Router | NEXT-006 — Dynamic routes

**Source:** GFE-NJS-004

---

## NEXT-005

### What is the difference between page.tsx, layout.tsx, and template.tsx?

🟡 Intermediate

These are three special file conventions in the App Router that serve different purposes within the same route segment.

**`page.tsx`** is the unique content for a route. It is what makes a folder publicly accessible as a URL. Only `page.tsx` is shown to the user as the route's content.

**`layout.tsx`** is a persistent shell that wraps `page.tsx` and all child routes. Crucially, layouts **do not remount** when you navigate between child routes — their state and DOM are preserved. Use layouts for sidebars, headers, and authenticated shells that should survive navigation.

**`template.tsx`** is similar to a layout but **remounts on every navigation**. It creates a fresh instance each time the route changes. Use templates when you need per-navigation effects — e.g., page-enter animations, resetting form state, or firing analytics on each visit.

```tsx
// layout.tsx — persists across navigations, does NOT remount
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="dashboard">
      <Sidebar />      {/* Sidebar state survives route changes */}
      <main>{children}</main>
    </div>
  );
}

// template.tsx — remounts on every navigation
export default function PageTemplate({ children }: { children: React.ReactNode }) {
  // useEffect here fires on every route change within this segment
  return <div className="page-wrapper">{children}</div>;
}
```

**Related:** NEXT-003 — App Router | NEXT-008 — loading.tsx | NEXT-009 — error.tsx

**Source:** GFE-NJS-005

---

## NEXT-006

### What are dynamic routes and how do you define them in the App Router?

🟢 Beginner

Dynamic routes let a single file handle an infinite number of URL patterns where one or more segments are variable. You define them by wrapping a folder name in square brackets.

**Types of dynamic segments:**

| Syntax | Matches | Example |
|---|---|---|
| `[slug]` | Single segment | `/blog/my-post` |
| `[...slug]` | One or more segments | `/docs/a/b/c` |
| `[[...slug]]` | Zero or more segments | `/docs` and `/docs/a/b` |

The matched value is available in the `params` prop of the `page.tsx`.

```tsx
// app/blog/[slug]/page.tsx
interface Props {
  params: Promise<{ slug: string }>;
}

export default async function BlogPost({ params }: Props) {
  const { slug } = await params;  // params is a Promise in Next.js 15+
  const post = await fetchPost(slug);

  return (
    <article>
      <h1>{post.title}</h1>
      <div>{post.content}</div>
    </article>
  );
}
```

For statically generated dynamic routes, pair with `generateStaticParams()` (see NEXT-007).

**Related:** NEXT-004 — File-based routing | NEXT-007 — generateStaticParams

**Source:** GFE-NJS-006

---

## NEXT-007

### What is generateStaticParams() and what does it replace from the Pages Router?

🟡 Intermediate

`generateStaticParams()` is an async function you export from a dynamic route's `page.tsx` to tell Next.js which param values to pre-render at build time. It is the App Router equivalent of `getStaticPaths()` from the Pages Router.

At build time, Next.js calls `generateStaticParams()`, gets the list of params, and statically generates an HTML page for each one. This gives you the performance of a static site with the flexibility of dynamic routing.

```tsx
// app/blog/[slug]/page.tsx

// Tell Next.js which slugs to pre-build
export async function generateStaticParams() {
  const posts = await fetch('https://api.example.com/posts').then(r => r.json());

  return posts.map((post: { slug: string }) => ({
    slug: post.slug,
  }));
  // Returns: [{ slug: 'hello-world' }, { slug: 'react-tips' }, ...]
}

export default async function BlogPost({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;
  const post = await fetchPost(slug);
  return <article><h1>{post.title}</h1></article>;
}
```

If a user visits a slug NOT in the pre-built list, Next.js either returns 404 (default) or renders on-demand depending on your `dynamicParams` setting (`true` by default = render on-demand and cache).

**Related:** NEXT-006 — Dynamic routes | NEXT-015 — SSR vs SSG vs ISR

**Source:** GFE-NJS-007

---

## NEXT-008

### What is loading.tsx and how does it relate to streaming?

🟡 Intermediate

`loading.tsx` is a special file in the App Router that automatically wraps a route segment's `page.tsx` in a React Suspense boundary. When the page is loading (e.g., awaiting a slow data fetch), Next.js immediately streams the `loading.tsx` UI to the browser, then replaces it with the full page once it resolves.

This is **streaming** — instead of waiting for ALL data before sending ANY HTML, Next.js sends the layout and loading skeleton instantly, then progressively streams in content chunks as they become ready.

```tsx
// app/dashboard/loading.tsx
// This renders instantly while page.tsx is awaiting data
export default function DashboardSkeleton() {
  return (
    <div className="skeleton">
      <div className="skeleton-header" />
      <div className="skeleton-body" />
    </div>
  );
}

// app/dashboard/page.tsx
export default async function Dashboard() {
  const data = await fetchSlowData(); // loading.tsx shown during this await
  return <DashboardContent data={data} />;
}
```

You can also manually place `<Suspense>` boundaries inside a page to stream individual sections independently — giving you granular control over which parts load first.

**Related:** NEXT-005 — layout vs template | NEXT-009 — error.tsx | NEXT-017 — Hydration

**Source:** GFE-NJS-008

---

## NEXT-009

### What is error.tsx and why must it be a Client Component?

🟡 Intermediate

`error.tsx` is a special file that defines the error boundary UI for a route segment. When any component in that segment throws an error during rendering or data fetching, Next.js catches it and renders `error.tsx` instead of crashing the whole page. This keeps unaffected parts of the layout visible while isolating the failure.

`error.tsx` **must be a Client Component** (marked with `"use client"`) because React error boundaries are implemented using the class lifecycle method `componentDidCatch`, which only exists on the client. Error boundaries cannot be Server Components.

```tsx
// app/dashboard/error.tsx
"use client";

import { useEffect } from 'react';

interface ErrorProps {
  error: Error & { digest?: string };
  reset: () => void;
}

export default function DashboardError({ error, reset }: ErrorProps) {
  useEffect(() => {
    // Log to an error reporting service
    console.error('Dashboard error:', error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong loading the dashboard.</h2>
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

The `reset` function re-renders the segment, giving users a recovery path without a full page reload.

**Related:** NEXT-005 — layout vs template | NEXT-008 — loading.tsx | NEXT-012 — Client Components

**Source:** GFE-NJS-009

---

## NEXT-010

### What is not-found.tsx and how do you trigger it?

🟢 Beginner

`not-found.tsx` is the special file that renders when a route cannot be matched or when you explicitly call the `notFound()` function from `next/navigation`. It is the App Router's replacement for a custom 404 page.

There are two ways it gets triggered:

1. **Automatic** — a user navigates to a URL that matches no file in `app/`. Next.js renders the nearest `not-found.tsx` up the segment tree (or the root `app/not-found.tsx` as a fallback).
2. **Manual** — you call `notFound()` inside a Server Component when a resource isn't found in your data source (e.g., a blog post slug that doesn't exist in the database).

```tsx
// app/blog/[slug]/page.tsx
import { notFound } from 'next/navigation';

export default async function BlogPost({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;
  const post = await fetchPost(slug);

  // Explicitly trigger 404 if the post doesn't exist in the DB
  if (!post) notFound();

  return <article><h1>{post.title}</h1></article>;
}

// app/not-found.tsx — rendered for all unmatched routes
export default function NotFound() {
  return (
    <div>
      <h1>404 — Page Not Found</h1>
      <a href="/">Go home</a>
    </div>
  );
}
```

**Related:** NEXT-004 — File-based routing | NEXT-009 — error.tsx

**Source:** GFE-NJS-010

---

## NEXT-036

### How do you create a new Next.js project?

The recommended way is `create-next-app`, the official scaffolding CLI:

```bash
npx create-next-app@latest my-app
# Prompts: TypeScript? ESLint? Tailwind? src/ dir? App Router?
cd my-app
npm run dev   # starts dev server on http://localhost:3000
```

`create-next-app` sets up the full project structure: `app/` (or `pages/`) directory, `next.config.js`, `tsconfig.json`, ESLint config, and a basic `layout.tsx` + `page.tsx`. Choosing the App Router (default since Next.js 13) gives you Server Components, layouts, and streaming out of the box. You can also start from scratch by installing `next react react-dom` manually and adding scripts to `package.json`, but the CLI is strongly preferred for correctness.

**Related:** [NEXT-003 — App Router vs Pages Router](./01-fundamentals.md#next-003) | [NEXT-044 — next.config.js](./10-config-tooling.md#next-044)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-02](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-079

### How is Next.js a full-stack framework?

Next.js bridges the traditional frontend/backend split by letting you write both in the same project:

- **Route Handlers** (`app/api/route.ts`) replace standalone Express/Fastify API servers for most use cases.
- **Server Actions** (`"use server"`) let you call server-side logic (DB queries, auth checks) directly from form actions or event handlers — no manual fetch required.
- **Server Components** run on the server and can query databases, read the file system, or call internal services without any client-side exposure.
- **Middleware** runs at the edge before requests reach your app, enabling auth guards, redirects, and A/B testing.

```ts
// app/api/users/route.ts — a full API endpoint
export async function GET() {
  const users = await db.user.findMany();
  return Response.json(users);
}

// app/actions.ts — a Server Action
'use server';
export async function createUser(formData: FormData) {
  await db.user.create({ data: { name: formData.get('name') } });
}
```

This means a single Next.js project can replace a React frontend + a separate REST/GraphQL API layer for many use cases.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-027 — Server Actions](./06-api-actions.md#next-027)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-75](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-139

**Q: What is the difference between Next.js and Create React App (CRA)?**

Create React App (CRA) is a boilerplate tool that generates a client-side-only React application. Next.js is a full-stack framework built on React that adds server-side rendering, file-system routing, API routes, and production optimizations out of the box.

| Feature | Next.js | Create React App |
|---|---|---|
| **Rendering** | SSR, SSG, ISR, CSR — choose per page | CSR only — all rendering in the browser |
| **Routing** | File-system based (`pages/` or `app/`) | None built-in — must install React Router |
| **API layer** | Built-in API routes / Route Handlers + Server Actions | None — requires a separate backend |
| **SEO** | Strong — server-rendered HTML for crawlers | Poor — blank HTML until JS loads |
| **Performance** | Automatic code splitting, image opt, font opt | Basic — relies on the app to optimize |
| **Build output** | Node.js server (or static export) | Static files (HTML/JS/CSS) only |
| **Initial setup** | Production-ready from day 1 | Good for prototypes; needs config for production |
| **TypeScript** | First-class support | Supported via template |
| **Maintenance** | Actively developed by Vercel | Officially deprecated (succeeded by Vite + React) |

**When CRA was appropriate (historically):**

- Simple client-side SPAs where SEO doesn't matter (dashboards, internal tools behind auth)
- Learning React without framework complexity
- Projects where a separate API server already exists

**Why Next.js is preferred today:**

- CRA was officially deprecated in 2023; Vite is now the community replacement for client-side-only React apps
- Next.js handles the full stack, eliminating the need for a separate backend for many use cases
- Server-side rendering significantly improves Core Web Vitals and SEO ranking
- Incremental adoption is possible — you can mix static and dynamic pages in one project

> **In interviews:** Frame CRA as "a tool for learning or simple SPAs; Next.js adds SSR, routing, and API capabilities that CRA lacks entirely."

**Related:** [NEXT-001 — What is Next.js](./01-fundamentals.md#next-001) | [NEXT-002 — Why Next.js](./01-fundamentals.md#next-002) | [NEXT-143 — Competitors and alternatives](./01-fundamentals.md#next-143)

**Source:** [Next.js Interview Questions English YTE-NJS Q18](../../sources/nextjs/youtube/nextjs-interview-english/question-map.md)

---

## NEXT-143

**Q: What are the main competitors and alternatives to Next.js?**

Next.js competes with other React frameworks, meta-frameworks for other libraries, and static-site generators.

**React-based alternatives:**

| Alternative | Type | Key differentiator vs Next.js |
|---|---|---|
| **Remix** | Full-stack React framework | Emphasizes web standards (forms, fetch API); nested layouts via loaders/actions |
| **Gatsby** | Static-site generator | Optimized for content-heavy static sites; GraphQL data layer |
| **Create React App / Vite** | CSR boilerplate | No SSR, no routing built-in; simpler for pure SPAs |
| **Astro** | Multi-framework SSG/SSR | Island architecture; any UI framework (React, Vue, Svelte); minimal JS shipped |

**Non-React meta-frameworks:**

| Alternative | Framework | Notes |
|---|---|---|
| **Nuxt.js** | Vue.js | Next.js equivalent for Vue; very similar API surface |
| **SvelteKit** | Svelte | File-based routing, SSR/SSG; smaller bundle due to Svelte compiler |
| **Angular Universal** | Angular | SSR for Angular apps |
| **SolidStart** | SolidJS | SSR framework for SolidJS, similar to Remix in philosophy |

**When you might choose an alternative:**

- **Remix** — when you want to embrace web platform APIs (forms as progressive enhancements, native fetch)
- **Gatsby** — when you have a large static content site and want rich plugin ecosystem
- **Astro** — when shipping minimal JS is the priority (blogs, marketing, documentation)
- **SvelteKit** — when you prefer Svelte's reactivity model over React
- **Nuxt.js** — when the team prefers Vue.js

**Next.js strengths vs all alternatives:**

- Largest ecosystem, most production deployments
- Backed by Vercel with first-class deployment integration
- Most feature-complete (App Router, RSC, Server Actions, Turbopack, image/font/script optimization)
- Most common in enterprise React adoption

**Related:** [NEXT-001 — What is Next.js](./01-fundamentals.md#next-001) | [NEXT-002 — Why Next.js](./01-fundamentals.md#next-002) | [NEXT-139 — Next.js vs CRA](./01-fundamentals.md#next-139)

**Source:** [Next.js Interview Questions English YTE-NJS Q31](../../sources/nextjs/youtube/nextjs-interview-english/question-map.md)

---

## NEXT-144

**Q: In what language is Next.js written?**

Next.js is written primarily in **TypeScript** and **JavaScript**, with performance-critical internal tooling (the Turbopack bundler and the SWC compiler) written in **Rust**.

| Layer | Language | Purpose |
|---|---|---|
| Framework core | TypeScript / JavaScript | Routing, rendering pipeline, data fetching, React integration |
| **SWC compiler** | **Rust** | Replaces Babel; transpiles JSX and TypeScript at native speed |
| **Turbopack bundler** | **Rust** | Replaces Webpack; incremental bundler for dev and production builds |
| CLI (`create-next-app`) | JavaScript / Node.js | Project scaffolding |

**SWC vs Babel:**

Next.js replaced Babel with SWC as the default compiler in Next.js 12 (2021). SWC is ~17× faster than Babel for compilation because it runs in native Rust rather than JavaScript.

**Turbopack (Next.js 13+):**

Turbopack is a Rust-based bundler that Next.js uses to replace Webpack. It performs incremental compilation — only re-bundling the modules that changed — making `next dev` significantly faster on large codebases. As of Next.js 15, Turbopack is stable for development (`next dev --turbopack` or enabled by default in some configurations).

**What this means for developers:**

- You write your application in **JavaScript or TypeScript** (React JSX)
- The Rust tooling is an implementation detail — it improves build performance but requires no Rust knowledge
- Next.js fully supports TypeScript first-class (auto-detects `tsconfig.json`, provides type-safe APIs)

**Related:** [NEXT-038 — next.config.js](./10-config-tooling.md#next-038) | [NEXT-001 — What is Next.js](./01-fundamentals.md#next-001)

**Source:** [Next.js Interview Questions English YTE-NJS Q34](../../sources/nextjs/youtube/nextjs-interview-english/question-map.md)
