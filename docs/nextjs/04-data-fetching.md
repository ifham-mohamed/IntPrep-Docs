# Next.js — Data Fetching & Caching

> **IDs:** NEXT-019 – NEXT-022
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-019

### How do you fetch data in the App Router?

🟢 Beginner

In the App Router, the primary data fetching pattern is `async/await` directly inside Server Components. Because Server Components run on the server, they can `await` any async operation — database queries, third-party APIs, file reads — without useEffect or client-side loading state.

**Three main approaches:**

```tsx
// 1. fetch() in a Server Component — simplest, with built-in caching
export default async function PostsPage() {
  const posts = await fetch('https://api.example.com/posts').then(r => r.json());
  return <PostList posts={posts} />;
}

// 2. ORM / database call directly in a Server Component
import { db } from '@/lib/db';
export default async function UsersPage() {
  const users = await db.select().from(usersTable);
  return <UserList users={users} />;
}

// 3. Parallel fetching with Promise.all — avoids request waterfalls
export default async function Dashboard() {
  const [user, stats, feed] = await Promise.all([
    fetchUser(),
    fetchStats(),
    fetchFeed(),
  ]);
  return <DashboardUI user={user} stats={stats} feed={feed} />;
}
```

For **client-side data fetching** (inside Client Components), standard React patterns apply: React Query, SWR, or `useEffect` with `fetch`. Next.js does not replace these for client-side needs.

**Related:** NEXT-020 — ISR | NEXT-021 — Caching | NEXT-015 — SSR vs SSG vs ISR

**Source:** GFE-NJS-019

---

## NEXT-020

### What is Incremental Static Regeneration (ISR) in Next.js?

🟡 Intermediate

ISR lets you build static pages at build time and then automatically regenerate them in the background after a specified time interval — without redeploying the app. It is the best of both worlds between SSG (fast, CDN-served) and SSR (always fresh).

In the App Router, ISR is controlled by the `revalidate` option on `fetch()` or by the `revalidate` route segment config:

```tsx
// Time-based ISR — page is rebuilt at most once every 60 seconds
export const revalidate = 60; // route segment config

export default async function PricingPage() {
  const plans = await fetch('https://api.example.com/plans').then(r => r.json());
  return <PricingTable plans={plans} />;
}

// Or per-fetch:
const plans = await fetch('https://api.example.com/plans', {
  next: { revalidate: 60 },
});
```

**On-demand ISR** lets you revalidate immediately when data changes — no waiting for the interval:

```tsx
// app/api/revalidate/route.ts — called by a CMS webhook
import { revalidatePath, revalidateTag } from 'next/cache';

export async function POST(req: Request) {
  const { path } = await req.json();
  revalidatePath(path);       // revalidate a specific URL path
  // or: revalidateTag('posts'); // revalidate all fetches tagged 'posts'
  return Response.json({ revalidated: true });
}
```

**Related:** NEXT-015 — SSR vs SSG vs ISR | NEXT-021 — Caching overview | NEXT-022 — Cache APIs

**Source:** GFE-NJS-020

---

## NEXT-021

### How does caching work in Next.js at a high level?

🔴 Advanced

Next.js has a multi-layered caching system. Understanding it prevents cache-related bugs and helps you optimize performance deliberately.

**The four cache layers (Next.js 15+):**

| Layer | What it caches | Storage | Invalidation |
|---|---|---|---|
| **Request Memoization** | `fetch()` deduplication within a single render pass | In-memory (per request) | Automatic — cleared after each request |
| **Data Cache** | `fetch()` responses across requests | Persistent (server disk/CDN) | `revalidatePath()`, `revalidateTag()`, or time-based `revalidate` |
| **Full Route Cache** | Pre-rendered HTML + RSC payload for static routes | Server disk | On redeploy, or on-demand revalidation |
| **Router Cache** | RSC payloads for visited routes | Browser memory | On navigation, `router.refresh()`, or expiry |

**In practice — the most important rules:**
- In Next.js 15, `fetch()` is **NOT cached by default** (changed from Next.js 14). You must opt into caching explicitly with `{ next: { revalidate: N } }` or `"use cache"`.
- Use `revalidateTag()` to invalidate cache by semantic group (e.g., all fetches tagged `'product'`).
- Use `revalidatePath()` to invalidate a specific URL.

```tsx
// Tag a fetch for targeted invalidation
const products = await fetch('https://api.example.com/products', {
  next: { tags: ['products'] },
});

// Later, in a Server Action or Route Handler:
import { revalidateTag } from 'next/cache';
revalidateTag('products'); // All fetches with this tag are now stale
```

> **Scenario:** An e-commerce product page shows a price fetched from an API. You tag the fetch with `'product-price'`. When the pricing team updates a price via the CMS, a webhook hits your Route Handler which calls `revalidateTag('product-price')`. The next visitor gets fresh data — without a full redeploy.

**Related:** NEXT-020 — ISR | NEXT-022 — Cache component APIs | NEXT-019 — Data fetching

**Source:** GFE-NJS-021

---

## NEXT-022

### What are the new Cache Component APIs — "use cache", cacheLife, and cacheTag?

🔴 Advanced

Next.js 15 introduced a new, more expressive caching API as an alternative to the `fetch()`-level cache options. These APIs are still experimental/canary but represent the direction Next.js is moving.

**`"use cache"`** is a directive (like `"use client"`) that you add to the top of a file or function to mark it as a cacheable computation. Everything returned by that function is cached.

**`cacheLife()`** sets the cache lifetime — how long the cached value stays fresh and how long stale-while-revalidate applies.

**`cacheTag()`** tags the cache entry so you can invalidate it on-demand with `revalidateTag()`.

```tsx
import { unstable_cacheLife as cacheLife, unstable_cacheTag as cacheTag } from 'next/cache';

// Cache an entire async function's result
async function getProducts() {
  'use cache';
  cacheLife('hours');        // Cache for hours (built-in profile)
  cacheTag('products');      // Tag for on-demand invalidation

  const res = await fetch('https://api.example.com/products');
  return res.json();
}

// Or cache at the component level
export async function ProductList() {
  'use cache';
  cacheLife({ stale: 30, revalidate: 60, expire: 3600 }); // custom profile
  const products = await db.products.findMany();
  return <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

These APIs give you cache control at the function/component granularity rather than at the fetch call level, making it easier to cache complex logic that isn't just a single `fetch()`.

**Related:** NEXT-021 — Caching overview | NEXT-020 — ISR | NEXT-019 — Data fetching

**Source:** GFE-NJS-022
