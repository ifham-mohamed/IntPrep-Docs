# Next.js — Rendering Models

> **IDs:** NEXT-015 – NEXT-018
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-015

### What is the difference between SSR, SSG, CSR, and ISR?

🟡 Intermediate

These four rendering strategies describe **when and where** HTML is generated.

| Strategy | When HTML is generated | Best for |
|---|---|---|
| **SSR** (Server-Side Rendering) | On every request, on the server | Personalized pages, live data |
| **SSG** (Static Site Generation) | At build time, once | Blogs, docs, marketing pages |
| **CSR** (Client-Side Rendering) | In the browser after JS loads | Dashboards behind auth, rich apps |
| **ISR** (Incremental Static Regeneration) | At build time + on-demand revalidation | E-commerce, news, content that changes occasionally |

In the App Router, the strategy is not declared explicitly — it falls out of how you fetch data:
- `fetch()` with no options → cached (SSG-like, static)
- `fetch()` with `{ cache: 'no-store' }` → dynamic (SSR-like, per-request)
- `fetch()` with `{ next: { revalidate: 60 } }` → ISR (cached, revalidated every 60 s)
- `export const dynamic = 'force-dynamic'` → always dynamic

```tsx
// SSG-like: cached at build time
const data = await fetch('https://api.example.com/posts');

// SSR-like: fresh data on every request
const data = await fetch('https://api.example.com/feed', { cache: 'no-store' });

// ISR: cached, but refreshed every 30 seconds
const data = await fetch('https://api.example.com/prices', {
  next: { revalidate: 30 },
});
```

**Related:** NEXT-016 — Static vs dynamic rendering | NEXT-020 — ISR in depth | NEXT-021 — Caching overview

**Source:** GFE-NJS-015

---

## NEXT-016

### What is the difference between static rendering and dynamic rendering in the App Router?

🟡 Intermediate

In the App Router, **static rendering** means a route is built at compile time and the same HTML is served to every user from a CDN. **Dynamic rendering** means the HTML is generated fresh on the server for each incoming request.

Next.js decides automatically:
- A route is **static** by default if all its data fetches are cacheable and it uses no dynamic APIs
- A route becomes **dynamic** when it uses `cookies()`, `headers()`, `searchParams`, or `{ cache: 'no-store' }` fetches — things that differ per request

You can force the mode with route segment config:
```tsx
export const dynamic = 'force-static';  // always static
export const dynamic = 'force-dynamic'; // always dynamic
```

**Trade-offs:**
- Static: faster (CDN-served), cheaper (no server compute), but stale until revalidated
- Dynamic: always fresh, but slower (server must render per request) and more expensive

```tsx
// This route is static — no dynamic APIs, fetch is cached
export default async function BlogPage() {
  const posts = await fetch('https://api.example.com/posts').then(r => r.json());
  return <PostList posts={posts} />;
}

// This route is dynamic — uses searchParams (per-request)
export default async function SearchPage({
  searchParams,
}: {
  searchParams: Promise<{ q: string }>;
}) {
  const { q } = await searchParams;
  const results = await search(q);
  return <SearchResults results={results} />;
}
```

> **Scenario:** A product search page with query params (`/search?q=shoes`) is always dynamic because `searchParams` differs per request. A product listing page showing all products is static — it can be pre-built and served from a CDN.

**Related:** NEXT-015 — SSR vs SSG vs ISR | NEXT-021 — Caching | NEXT-019 — Data fetching

**Source:** GFE-NJS-016

---

## NEXT-017

### What is hydration in Next.js?

🟢 Beginner

Hydration is the process by which React takes server-rendered HTML and "attaches" JavaScript event listeners to it so the page becomes interactive. The server sends fully formed HTML that the browser displays immediately (fast First Contentful Paint). Then React loads, compares its virtual DOM tree with the existing HTML, and hooks up interactivity — this comparison and attachment phase is hydration.

Hydration is specific to Client Components. Server Components are never hydrated because they ship no JavaScript to the browser — their output is purely HTML.

The sequence for a Next.js page:
1. Server renders HTML (including Client Components' initial HTML)
2. Browser receives and displays the HTML — page is **visible but not interactive**
3. Browser downloads the JavaScript bundle
4. React hydrates — event listeners are attached — page is now **fully interactive**

```
Server → HTML string → Browser paints → React.hydrateRoot() → Interactive
         (fast)         (fast)            (JS download needed)
```

The gap between step 2 and step 4 is why you should keep Client Components small and push as much as possible to Server Components — it minimizes hydration work and Time to Interactive.

**Related:** NEXT-018 — Hydration errors | NEXT-011 — Server Components | NEXT-012 — Client Components

**Source:** GFE-NJS-017

---

## NEXT-018

### What causes hydration errors and how do you fix them?

🟡 Intermediate

A hydration error occurs when the HTML React renders on the server doesn't match what React renders on the client during hydration. React expects them to be identical. When they differ, React either throws an error (in development) or silently corrects the mismatch (in production), potentially causing flickering or incorrect UI.

**Common causes:**

1. **Browser-only APIs read during render** — `localStorage`, `window`, `navigator` don't exist on the server
2. **Date/time rendered during SSR** — `new Date()` on the server differs from the client
3. **Random values** — `Math.random()` produces different outputs server and client
4. **Browser extensions** — extensions that modify the DOM before hydration
5. **Improper HTML nesting** — e.g., `<p>` inside `<p>` is invalid and browsers auto-correct it differently

**Fixes:**

```tsx
// ❌ Causes hydration error — localStorage doesn't exist on server
export default function ThemeButton() {
  const theme = localStorage.getItem('theme'); // ReferenceError on server
  return <button>{theme}</button>;
}

// ✅ Fix 1 — read browser APIs in useEffect (client only)
"use client";
export default function ThemeButton() {
  const [theme, setTheme] = useState('light');
  useEffect(() => {
    setTheme(localStorage.getItem('theme') ?? 'light');
  }, []);
  return <button>{theme}</button>;
}

// ✅ Fix 2 — suppress for truly unavoidable mismatches
<div suppressHydrationWarning>{new Date().toLocaleTimeString()}</div>
// Use sparingly — it hides the warning but not the mismatch.
```

> **Scenario:** A navbar reads `localStorage` for a saved theme on mount. Without `useEffect`, the server renders `"light"` (default) but the browser immediately tries to render `"dark"` (from storage), causing a hydration mismatch and a visible flash.

**Related:** NEXT-017 — Hydration | NEXT-012 — Client Components | NEXT-011 — Server Components

**Source:** GFE-NJS-018

---

## NEXT-048

### What is pre-rendering in Next.js?

Pre-rendering means generating HTML for a page in advance, rather than relying entirely on client-side JavaScript. Every Next.js page is pre-rendered by default, which improves initial load performance and SEO because the browser receives real HTML instead of a blank page that waits for JS to hydrate.

Two forms of pre-rendering exist:

| Form | When HTML is generated | Data freshness |
|---|---|---|
| Static Generation (SSG) | At build time | Fixed until next build / ISR revalidation |
| Server-Side Rendering (SSR) | On every request | Always current |

In the App Router, the distinction maps to **static rendering** (default — no dynamic data) vs **dynamic rendering** (triggered by `cookies()`, `headers()`, `searchParams`, or `{ cache: 'no-store' }` fetches). Pre-rendered output can be cached at the CDN level, giving near-instant responses.

Client-Side Rendering (CSR), by contrast, sends minimal HTML and defers all content generation to the browser — no pre-rendering occurs.

**Related:** [NEXT-015 — SSR vs SSG vs CSR vs ISR](./03-rendering.md#next-015) | [NEXT-016 — Static vs dynamic rendering](./03-rendering.md#next-016)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-27](../../sources/nextjs/github/mrhrifat/question-map.md)
