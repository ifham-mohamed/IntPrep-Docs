# Next.js — Navigation

> **IDs:** NEXT-023 – NEXT-025
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-023

### What is next/link used for and when should you use a plain `<a>` tag?

🟢 Beginner

`next/link` is Next.js's `<Link>` component for client-side navigation between routes. It renders an `<a>` tag in the HTML but intercepts clicks to use the client-side router instead of triggering a full browser page load. This preserves application state, avoids re-downloading shared layouts, and makes navigation feel instant.

**What `<Link>` adds over a plain `<a>` tag:**
- **Prefetching** — when a `<Link>` enters the viewport, Next.js prefetches that route's code and data in the background (in production builds)
- **Client-side navigation** — no full page reload, shared layouts are not re-rendered
- **Scroll restoration** — automatically restores scroll position on back navigation

**When to use a plain `<a>` tag:**
- Navigating to an **external URL** (outside your Next.js app)
- Navigating to a **non-Next.js page** (e.g., a separate service)
- When you explicitly want a full browser navigation (rare)

```tsx
import Link from 'next/link';

export default function Nav() {
  return (
    <nav>
      {/* Client-side navigation — use Link */}
      <Link href="/about">About</Link>
      <Link href="/blog">Blog</Link>

      {/* External URL — use plain <a> */}
      <a href="https://github.com" target="_blank" rel="noopener noreferrer">
        GitHub
      </a>
    </nav>
  );
}
```

**Related:** NEXT-024 — useRouter | NEXT-025 — redirect() vs client navigation

**Source:** GFE-NJS-023

---

## NEXT-024

### What is useRouter() used for in the App Router?

🟡 Intermediate

`useRouter()` is a Client Component hook from `next/navigation` that gives you programmatic control over the router. You use it when navigation needs to happen in response to logic — not just a click on a link.

**Key methods:**

| Method | What it does |
|---|---|
| `router.push(href)` | Navigate to a new route, adds to history |
| `router.replace(href)` | Navigate without adding to history |
| `router.back()` | Go to the previous history entry |
| `router.forward()` | Go forward in history |
| `router.refresh()` | Re-fetch Server Components for the current route |
| `router.prefetch(href)` | Prefetch a route manually |

```tsx
"use client";
import { useRouter } from 'next/navigation';

export default function LoginForm() {
  const router = useRouter();

  async function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    const success = await login(/* ... */);
    if (success) {
      router.push('/dashboard'); // programmatic navigation after login
    }
  }

  return <form onSubmit={handleSubmit}>...</form>;
}
```

**Important:** In the App Router, import `useRouter` from `next/navigation` — NOT `next/router` (that is the Pages Router version and will not work).

**Related:** NEXT-023 — next/link | NEXT-025 — redirect()

**Source:** GFE-NJS-024

---

## NEXT-025

### What is the difference between redirect() and client-side navigation?

🟡 Intermediate

Next.js has two mechanisms for redirecting users — one server-side and one client-side — and they solve different problems.

**`redirect()` (server-side)**
- Imported from `next/navigation`
- Called inside Server Components, Server Actions, or Route Handlers
- Throws a special response that tells Next.js to send an HTTP redirect (307/308) to the browser
- The browser follows it as a new request — no JavaScript needed
- Used for: auth guards, post-form-submission redirects, moved routes

**Client-side navigation (`router.push()` / `<Link>`)**
- Runs in the browser
- Uses the client router — no new HTTP request, no full reload
- Used for: interactive navigation, UX flows, conditional navigation in event handlers

```tsx
// Server-side redirect — in a Server Component or Server Action
import { redirect } from 'next/navigation';
import { getSession } from '@/lib/auth';

export default async function DashboardPage() {
  const session = await getSession();
  if (!session) redirect('/login'); // HTTP 307 redirect
  return <Dashboard user={session.user} />;
}

// Client-side navigation — in a Client Component
"use client";
import { useRouter } from 'next/navigation';
export function LogoutButton() {
  const router = useRouter();
  return (
    <button onClick={async () => {
      await logout();
      router.push('/login'); // No page reload, no HTTP redirect
    }}>
      Log out
    </button>
  );
}
```

**Related:** NEXT-023 — next/link | NEXT-024 — useRouter | NEXT-034 — Authentication

**Source:** GFE-NJS-025
