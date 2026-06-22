# Next.js — Infrastructure: Proxy, Env, Auth & Deployment

> **IDs:** NEXT-032 – NEXT-035
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-032

### What is Proxy (formerly Middleware) in Next.js and what is it used for?

🟡 Intermediate

Proxy (officially renamed from Middleware in recent Next.js versions) is code that runs **before a request is processed** — before routing, before Server Components render, and before any cached response is served. You define it in a `proxy.ts` (or `middleware.ts`) file at the root of your project.

Because it runs at the edge (between the user and your server, on Vercel's edge network or a compatible runtime), it is extremely fast and runs on every matching request.

**Common uses:**
- **Authentication guards** — redirect unauthenticated users before the page renders
- **Geolocation-based routing** — redirect users to locale-specific routes based on IP
- **A/B testing** — randomly assign users to variants without client-side flicker
- **Request rewriting** — map one URL to a different internal path
- **Rate limiting / bot protection** — block or challenge suspicious traffic at the edge

```ts
// middleware.ts (or proxy.ts)
import { NextRequest, NextResponse } from 'next/server';
import { getToken } from 'next-auth/jwt';

export async function middleware(req: NextRequest) {
  const token = await getToken({ req });

  const isProtected = req.nextUrl.pathname.startsWith('/dashboard');

  if (isProtected && !token) {
    return NextResponse.redirect(new URL('/login', req.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*', '/settings/:path*'],
};
```

**Constraints:** Proxy runs in the Edge Runtime, not Node.js. It cannot use Node.js-only APIs (`fs`, `crypto`, most ORMs). Use it for fast, lightweight logic only.

**Related:** NEXT-034 — Authentication | NEXT-025 — redirect()

**Source:** GFE-NJS-032

---

## NEXT-033

### How do environment variables work in Next.js and what is NEXT_PUBLIC_?

🟢 Beginner

Next.js loads environment variables from `.env` files and makes them available to your code. The key distinction is **which code can access which variables**.

**By default, all env vars are server-only.** They are available in Server Components, Route Handlers, Server Actions, and `next.config.ts` — but NOT in Client Components or the browser.

**To expose a variable to the browser**, prefix it with `NEXT_PUBLIC_`. Next.js inlines the value into the client bundle at build time. This means:
1. Anyone can see it in browser DevTools — never put secrets in `NEXT_PUBLIC_` vars
2. The value is baked in at build time — changing it requires a rebuild

```bash
# .env.local
DATABASE_URL=postgres://user:pass@host/db   # server-only, secret
STRIPE_SECRET_KEY=sk_live_...               # server-only, secret
NEXT_PUBLIC_API_BASE=https://api.example.com  # exposed to browser, public
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX              # exposed to browser, public
```

```tsx
// Server Component — can read server-only vars
const db = createClient(process.env.DATABASE_URL); // ✅

// Client Component — can only read NEXT_PUBLIC_ vars
"use client";
const apiBase = process.env.NEXT_PUBLIC_API_BASE; // ✅
const secret = process.env.STRIPE_SECRET_KEY;     // ❌ undefined in browser
```

**File loading order** (later files override earlier):
`.env` → `.env.local` → `.env.[NODE_ENV]` → `.env.[NODE_ENV].local`

**Related:** NEXT-034 — Authentication | NEXT-032 — Proxy

**Source:** GFE-NJS-033

---

## NEXT-034

### How do you handle authentication in a Next.js app?

🟡 Intermediate

Authentication in Next.js typically involves three layers working together: verifying identity (login), persisting the session, and protecting routes.

**Recommended approach using Auth.js (NextAuth v5):**

```tsx
// 1. Configure Auth.js
// auth.ts
import NextAuth from 'next-auth';
import GitHub from 'next-auth/providers/github';

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [GitHub],
});

// 2. Mount the Route Handler
// app/api/auth/[...nextauth]/route.ts
import { handlers } from '@/auth';
export const { GET, POST } = handlers;

// 3. Protect routes in Proxy (Middleware) — runs on every request
// middleware.ts
import { auth } from '@/auth';
export default auth((req) => {
  if (!req.auth && req.nextUrl.pathname.startsWith('/dashboard')) {
    return Response.redirect(new URL('/login', re