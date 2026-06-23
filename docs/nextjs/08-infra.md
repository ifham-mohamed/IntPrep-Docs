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
DATABASE_URL=postgres://user:pass@host/db      # server-only, secret
STRIPE_SECRET_KEY=sk_live_...                  # server-only, secret
NEXT_PUBLIC_API_BASE=https://api.example.com   # exposed to browser, public
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX                 # exposed to browser, public
```

```tsx
// Server Component — can read server-only vars
const db = createClient(process.env.DATABASE_URL); // ✅

// Client Component — can only read NEXT_PUBLIC_ vars
"use client";
const apiBase = process.env.NEXT_PUBLIC_API_BASE; // ✅
const secret  = process.env.STRIPE_SECRET_KEY;    // ❌ undefined in browser
```

**File loading order** (later files override earlier):
`.env` → `.env.local` → `.env.[NODE_ENV]` → `.env.[NODE_ENV].local`

**Related:** NEXT-034 — Authentication | NEXT-032 — Proxy

**Source:** GFE-NJS-033

---

## NEXT-034

### How do you handle authentication in a Next.js app?

🟡 Intermediate

Authentication in Next.js typically involves three layers working together: verifying identity (login), persisting the session, and protecting routes. The recommended approach is **Auth.js (NextAuth v5)**, which integrates natively with the App Router.

```tsx
// 1. Configure Auth.js — auth.ts (project root)
import NextAuth from 'next-auth';
import GitHub from 'next-auth/providers/github';

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [GitHub],
});

// 2. Mount the Route Handler (OAuth callback endpoint)
// app/api/auth/[...nextauth]/route.ts
import { handlers } from '@/auth';
export const { GET, POST } = handlers;

// 3. Protect routes in Proxy — runs before every request, at the edge
// middleware.ts
import { auth } from '@/auth';
export default auth((req) => {
  const isProtected = req.nextUrl.pathname.startsWith('/dashboard');
  if (!req.auth && isProtected) {
    return Response.redirect(new URL('/login', req.url));
  }
});

export const config = { matcher: ['/dashboard/:path*'] };

// 4. Read session in a Server Component
// app/dashboard/page.tsx
import { auth } from '@/auth';
export default async function Dashboard() {
  const session = await auth();
  if (!session) return null; // Proxy already redirected, but defensive check
  return <h1>Welcome, {session.user?.name}</h1>;
}
```

**The three-layer model:**
- **Proxy / Middleware** — fast edge-level redirect for unauthenticated access
- **Server Components** — secondary check, access session data for UI personalization
- **Server Actions / Route Handlers** — always re-verify session before mutating data

> **Scenario:** A user navigates directly to `/dashboard`. Proxy runs first, calls `auth()`, finds no session, and redirects to `/login` before Next.js even starts rendering the page — no flash, no leaked HTML.

**Related:** NEXT-032 — Proxy | NEXT-025 — redirect() | NEXT-033 — Environment variables

**Source:** GFE-NJS-034

---

## NEXT-035

### How do you deploy a Next.js app and what features require a runtime?

🟢 Beginner

Next.js has two deployment modes: **static export** (a folder of HTML/CSS/JS files) and **server deployment** (a Node.js process or edge runtime that handles requests). Which you need depends on which Next.js features you use.

**Static export (`output: 'export'` in `next.config.ts`)**
Pre-renders all routes to HTML at build time. Can be hosted on any CDN (GitHub Pages, Netlify, S3 + CloudFront) with no server.

Limitations: no SSR, no Route Handlers, no Server Actions, no ISR, no Proxy.

**Server deployment (standard `next build` + `next start`)**
Requires a Node.js server. Supports all Next.js features. You can self-host on a VPS (DigitalOcean, EC2, Fly.io) or use Vercel.

**Vercel (recommended for full feature support)**
Vercel is the platform built by the Next.js team. It handles build, deployment, CDN, edge functions, ISR regeneration, and image optimization automatically with zero config.

```bash
# Standard build — requires Node.js runtime
next build   # builds the app
next start   # starts the production server on port 3000

# Static export — no server needed
# next.config.ts
export default { output: 'export' };

next build   # generates /out directory of static files
# Deploy /out to any static host
```

**Feature → Runtime requirement:**

| Feature | Static export | Node.js server | Vercel edge |
|---|---|---|---|
| SSG pages | ✅ | ✅ | ✅ |
| SSR / dynamic routes | ❌ | ✅ | ✅ |
| Server Actions | ❌ | ✅ | ✅ |
| Route Handlers | ❌ | ✅ | ✅ |
| ISR | ❌ | ✅ | ✅ |
| Proxy (Middleware) | ❌ | ✅ | ✅ |
| Image optimization | ❌ | ✅ | ✅ |

**Related:** NEXT-032 — Proxy | NEXT-033 — Environment variables

**Source:** GFE-NJS-035

---

## NEXT-134

### How do you choose between Edge and Node.js runtimes for Route Handlers?

Next.js Route Handlers (and Middleware) can run on either the **Node.js runtime** (default) or the **Edge runtime**:

```ts
// app/api/fast/route.ts — Edge runtime
export const runtime = 'edge';

export async function GET() {
  return Response.json({ message: 'Served from edge' });
}
```

| | Node.js Runtime | Edge Runtime |
|---|---|---|
| **Cold start** | Slower (~100–500ms) | Near-zero (<5ms) |
| **Location** | Single region (or serverless) | 100+ edge locations globally |
| **Node APIs** | Full (fs, crypto, Buffer, etc.) | Limited subset only |
| **Memory** | Up to 1GB+ | 128MB |
| **NPM packages** | All packages | Edge-compatible only (no native bindings) |
| **Use case** | DB queries, file I/O, heavy computation | Auth checks, redirects, geo-routing, A/B tests |

**Choose Edge when:**
- Low latency is critical (auth middleware, redirects, feature flags)
- You don't need Node.js-specific APIs
- Global user base needs geographically close responses

**Choose Node.js when:**
- Connecting to a database (most DB clients require Node.js)
- Using libraries with native bindings (bcrypt, sharp)
- Processing uploads or large payloads

Middleware (`middleware.ts`) defaults to the Edge runtime and should stay there — it runs on every request, so cold starts would be unacceptable.

**Related:** [NEXT-032 — Middleware](./08-infra.md#next-032) | [NEXT-035 — Deployment](./08-infra.md#next-035)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-17](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-142

**Q: Can Next.js be hosted on nginx (traditional web servers)?**

Next.js requires a **Node.js runtime** to serve SSR pages, API routes, and Server Actions. nginx cannot serve a Next.js application on its own — it must be used as a **reverse proxy** in front of the Node.js process.

**Typical self-hosted setup with nginx:**

```
Internet → nginx (port 80/443) → Node.js / Next.js server (port 3000)
```

**nginx reverse proxy config:**

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass         http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection 'upgrade';
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

**What nginx handles in this setup:**

- SSL/TLS termination (HTTPS)
- Static file caching (you can configure nginx to cache `/_next/static/` directly)
- Rate limiting and DDoS protection
- Load balancing across multiple Node.js instances
- Compression (gzip / brotli)

**What nginx cannot replace:**

- Running `next start` — the Node.js server must be running separately (PM2, systemd, or Docker recommended)
- SSR page generation — nginx has no understanding of React
- API route handling — all dynamic logic requires Node.js

**Static-only alternative:** If you run `next export` (static export — only works if no SSR/API routes are used), the output is plain HTML/CSS/JS files that nginx can serve directly without Node.js.

```bash
# next.config.js: output: 'export'
npm run build  # generates /out directory
# nginx serves /out as static files — no Node.js needed
```

**Recommended self-hosted process managers:**

```bash
# PM2 — keeps the Node.js process alive, restarts on crash
pm2 start npm --name "nextjs" -- start

# systemd service (Linux)
[Unit]
Description=Next.js app
[Service]
ExecStart=/usr/bin/npm start
WorkingDirectory=/var/www/myapp
Restart=always
[Install]
WantedBy=multi-user.target
```

**Related:** [NEXT-035 — Deployment options](./08-infra.md#next-035) | [NEXT-134 — Edge vs Node.js runtime](./08-infra.md#next-134) | [NEXT-141 — CDN setup](../nextjs/10-config-tooling.md#next-141)

**Source:** [Next.js Interview Questions English YTE-NJS Q29](../../sources/nextjs/youtube/nextjs-interview-english/question-map.md)
