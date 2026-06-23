# Next.js App Router — Advanced Patterns

This file covers advanced App Router concepts: dynamic imports, form handling, Auth.js, Server Action patterns, route groups, parallel routes, intercepting routes, streaming, nested layouts, and optimistic UI.

---

## NEXT-040

### What is `next/dynamic` and when do you use it?

`next/dynamic` is Next.js's code-splitting wrapper that lets you lazy-load components. It defers the JS bundle for a component until it's actually needed, reducing initial bundle size.

```tsx
import dynamic from 'next/dynamic';

// Lazy-load a heavy component — only loads when rendered
const HeavyChart = dynamic(() => import('@/components/HeavyChart'), {
  loading: () => <p>Loading chart...</p>,
  ssr: false, // don't render on server (useful for browser-only libs)
});

export default function Dashboard() {
  return <HeavyChart data={chartData} />;
}
```

**Use cases:**
- Components that use browser-only APIs (`window`, `document`) — set `ssr: false`
- Large third-party libraries (chart libraries, map widgets, rich-text editors)
- Components below the fold or behind a tab/modal

In the App Router, you can also use React's native `lazy()` + `Suspense` — `next/dynamic` is still convenient for the `ssr: false` option and the `loading` prop sugar.

**Related:** [NEXT-067 — ssr:false in dynamic import](./14-app-router-advanced.md#next-067) | [NEXT-008 — loading.tsx and streaming](../nextjs/01-fundamentals.md#next-008)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-16](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-067

### What does `ssr: false` mean in `next/dynamic`?

`ssr: false` tells Next.js to skip server-side rendering for that dynamically imported component entirely — it only renders in the browser.

```tsx
const MapComponent = dynamic(() => import('@/components/Map'), {
  ssr: false,
  loading: () => <div className="map-skeleton" />,
});
```

**When to use `ssr: false`:**
- The component directly accesses `window`, `document`, `navigator`, or `localStorage` — these don't exist on the server and would throw during SSR.
- Third-party libraries that don't support SSR (some canvas/WebGL libraries, Leaflet maps, certain animation libraries).
- Components that rely on browser-specific APIs like `IntersectionObserver` or `ResizeObserver`.

**Without `ssr: false`:** Next.js attempts to render the component on the server. If the component uses browser globals, it throws a ReferenceError.

In the App Router, an alternative is to mark the component with `"use client"` and wrap with `<ClientOnly>`, but `dynamic({ ssr: false })` is the cleanest pattern.

**Related:** [NEXT-040 — next/dynamic](./14-app-router-advanced.md#next-040) | [NEXT-011 — Server Components](../nextjs/02-server-client.md#next-011)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-56](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-102

### How do you handle form submissions in the App Router?

The App Router integrates Server Actions with HTML forms for progressive enhancement:

```tsx
// app/contact/page.tsx
import { submitContact } from './actions';

export default function ContactPage() {
  return (
    <form action={submitContact}>
      <input name="email" type="email" required />
      <textarea name="message" required />
      <button type="submit">Send</button>
    </form>
  );
}
```

```ts
// app/contact/actions.ts
'use server';
import { redirect } from 'next/navigation';

export async function submitContact(formData: FormData) {
  const email = formData.get('email') as string;
  const message = formData.get('message') as string;
  await sendEmail({ email, message });
  redirect('/contact/success');
}
```

For a better UX with loading state and inline errors, use `useActionState` (React 19):

```tsx
'use client';
import { useActionState } from 'react';
import { submitContact } from './actions';

const [state, action, isPending] = useActionState(submitContact, null);
return (
  <form action={action}>
    {state?.error && <p className="error">{state.error}</p>}
    <button disabled={isPending}>{isPending ? 'Sending...' : 'Send'}</button>
  </form>
);
```

**Related:** [NEXT-027 — Server Actions](../nextjs/06-api-actions.md#next-027) | [NEXT-112 — Server Action benefits](../nextjs/06-api-actions.md#next-112)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-05](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-103

### How do you add Auth.js (NextAuth) to the Next.js App Router?

Auth.js (formerly NextAuth.js) is the standard auth library for Next.js. App Router setup:

```bash
npm install next-auth@beta
```

```ts
// auth.ts (project root)
import NextAuth from 'next-auth';
import GitHub from 'next-auth/providers/github';

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [GitHub],
  callbacks: {
    authorized({ auth, request }) {
      return !!auth?.user; // protect all routes requiring auth
    },
  },
});
```

```ts
// app/api/auth/[...nextauth]/route.ts
import { handlers } from '@/auth';
export const { GET, POST } = handlers;
```

```ts
// middleware.ts — protect routes
import { auth } from '@/auth';
export default auth;
export const config = { matcher: ['/dashboard/:path*', '/profile/:path*'] };
```

```tsx
// Read session in a Server Component
import { auth } from '@/auth';
export default async function Page() {
  const session = await auth();
  if (!session) redirect('/login');
  return <div>Hello, {session.user?.name}</div>;
}
```

**Related:** [NEXT-034 — Authentication](../nextjs/08-infra.md#next-034) | [NEXT-081 — JWT tokens](./13-security.md#next-081)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-08](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-105

### How do you add a credentials provider with Auth.js?

The credentials provider lets you authenticate with a username/password form against your own database:

```ts
// auth.ts
import NextAuth from 'next-auth';
import Credentials from 'next-auth/providers/credentials';
import { z } from 'zod';
import bcrypt from 'bcryptjs';

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [
    Credentials({
      async authorize(credentials) {
        const parsed = z.object({ email: z.string().email(), password: z.string().min(6) })
          .safeParse(credentials);
        if (!parsed.success) return null;

        const user = await db.user.findUnique({ where: { email: parsed.data.email } });
        if (!user || !user.passwordHash) return null;

        const isValid = await bcrypt.compare(parsed.data.password, user.passwordHash);
        return isValid ? { id: user.id, email: user.email, name: user.name } : null;
      },
    }),
  ],
  pages: { signIn: '/login' },
});
```

**Security notes:** The credentials provider disables CSRF protection by design — always implement rate limiting. Prefer OAuth providers (GitHub, Google) where possible, as they avoid storing password hashes.

**Related:** [NEXT-103 — Auth.js setup](./14-app-router-advanced.md#next-103) | [NEXT-081 — JWT tokens](./13-security.md#next-081)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-10](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-106

### What is the difference between using and not using `"use server"`?

`"use server"` marks a function (or entire file) as a Server Action — it runs exclusively on the server and can be called from Client Components or forms.

| | With `"use server"` | Without `"use server"` |
|---|---|---|
| Execution | Always server-side | Context-dependent (Server Component = server; Client Component = browser) |
| Access to secrets | Yes (env vars, DB) | Only if in a Server Component |
| Can be called from form `action` | Yes | No |
| Can access `cookies()`, `headers()` | Yes | Only in Server Components |
| Sends data over network? | Yes (from Client Components) | No (compiled into the bundle or rendered inline) |

```ts
// Server Action — only runs on server, callable from client
'use server';
export async function deletePost(id: string) {
  await db.post.delete({ where: { id } });
}

// Regular async function in a Server Component — also runs on server,
// but cannot be passed to Client Components as a prop action
async function fetchPost(id: string) {
  return db.post.findUnique({ where: { id } });
}
```

**Related:** [NEXT-027 — Server Actions](../nextjs/06-api-actions.md#next-027) | [NEXT-011 — Server Components](../nextjs/02-server-client.md#next-011)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-12](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-107

### How do you handle file uploads in Next.js?

File uploads require a Route Handler (not a Server Action, which can't reliably handle multipart form data with large files):

```ts
// app/api/upload/route.ts
export async function POST(req: Request) {
  const formData = await req.formData();
  const file = formData.get('file') as File;

  if (!file) return Response.json({ error: 'No file' }, { status: 400 });
  if (file.size > 5 * 1024 * 1024) return Response.json({ error: 'File too large' }, { status: 413 });

  const buffer = Buffer.from(await file.arrayBuffer());
  const filename = `${Date.now()}-${file.name}`;

  // Option 1: Save to local filesystem (development only)
  await fs.writeFile(`public/uploads/${filename}`, buffer);

  // Option 2: Upload to S3
  await s3.send(new PutObjectCommand({ Bucket: 'my-bucket', Key: filename, Body: buffer }));

  return Response.json({ url: `/uploads/${filename}` });
}
```

```tsx
// Client Component form
'use client';
async function handleUpload(e: React.ChangeEvent<HTMLInputElement>) {
  const file = e.target.files?.[0];
  if (!file) return;
  const form = new FormData();
  form.append('file', file);
  const res = await fetch('/api/upload', { method: 'POST', body: form });
  const { url } = await res.json();
}
```

For production, upload directly to S3/Cloudflare R2 using presigned URLs to avoid routing binary data through your Next.js server.

**Related:** [NEXT-026 — Route Handlers](../nextjs/06-api-actions.md#next-026) | [NEXT-030 — next/image](../nextjs/07-seo-assets.md#next-030)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-16](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-108

### What are common use cases for the Next.js App Router?

The App Router is best suited for:

- **Content-heavy sites** — blogs, documentation, marketing pages (static rendering + ISR)
- **Full-stack web apps** — dashboards, SaaS apps (Server Components + Server Actions + Route Handlers)
- **E-commerce** — product pages (SSG/ISR), cart/checkout (dynamic + Server Actions)
- **Authentication-gated apps** — Middleware handles route protection globally
- **Streaming UIs** — Suspense boundaries progressively reveal content as data loads
- **SEO-critical pages** — Server Components pre-render HTML for crawlers

The App Router is **not ideal** for:
- Pure static sites with no server logic (use plain static site generators instead)
- Real-time collaborative apps that need WebSockets throughout (Next.js has limited WebSocket support)
- Projects requiring deep customization of the HTTP server layer

**Related:** [NEXT-003 — App Router overview](../nextjs/01-fundamentals.md#next-003) | [NEXT-110 — App Router limitations](./14-app-router-advanced.md#next-110)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-17](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-109

### Is it possible to use both the App Router and Pages Router together?

Yes — Next.js supports coexistence of both routers in the same project. Routes in `app/` use the App Router; routes in `pages/` use the Pages Router. They can be navigated between seamlessly.

```
app/
  layout.tsx      ← App Router
  dashboard/
    page.tsx
pages/
  index.tsx       ← Pages Router (legacy)
  about.tsx
```

**Rules during coexistence:**
- `app/` takes precedence if both routers define the same route.
- You cannot use `app/` features (Server Components, `next/headers`) inside `pages/` files.
- Shared components can live in `components/` — just be mindful of `"use client"` vs Server Component boundaries.
- `_app.tsx` and `_document.tsx` only apply to Pages Router routes.

This is useful for **incremental migration**: keep existing Pages Router routes working while gradually moving to the App Router. The recommended migration strategy is to move leaf pages first, then shared layouts.

**Related:** [NEXT-003 — App Router vs Pages Router](../nextjs/01-fundamentals.md#next-003) | [NEXT-101 — When to choose each router](../nextjs/09-pages-router.md#next-101)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-20](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-110

### What are the limitations of the Next.js App Router?

The App Router is powerful but has real constraints to know:

- **Complexity** — the mental model (Server vs Client, streaming, caching layers) is harder to learn than the Pages Router.
- **Caching is opaque** — Next.js caches aggressively by default; cache invalidation bugs are common and hard to debug.
- **Third-party library compatibility** — many React libraries assume a client-side environment and require `"use client"` wrappers or dynamic imports with `ssr: false`.
- **No WebSocket support** — App Router Route Handlers don't support long-lived connections; you still need a separate WebSocket server.
- **Testing is harder** — Server Components and Server Actions have limited testing support; most tools target Client Components.
- **Build output size** — App Router bundles can be larger due to streaming and React Server Component protocol overhead.
- **Ecosystem maturity** — some patterns (Server Action composition, typed route params) are still evolving.

**Related:** [NEXT-108 — Use cases](./14-app-router-advanced.md#next-108) | [NEXT-101 — When to choose App vs Pages Router](../nextjs/09-pages-router.md#next-101)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-21](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-115

### How do you handle global state management in the Next.js App Router?

Server Components can't use React Context or hooks, so global state strategy depends on where the state lives:

**Server state (data from DB/API):**
- Fetch in Server Components and pass as props.
- Use `revalidatePath()` / `revalidateTag()` in Server Actions to refresh.
- React Query / SWR in Client Components for client-side caching + refetching.

**Client UI state:**
- React Context (in a `"use client"` Provider wrapping Client Component subtrees).
- Zustand, Jotai — lightweight, work well in App Router.
- Redux Toolkit — valid but heavier; use RTK Query for server data.

```tsx
// app/providers.tsx
'use client';
import { createContext, useContext, useState } from 'react';

const ThemeContext = createContext<{ theme: string; toggle: () => void } | null>(null);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, toggle: () => setTheme(t => t === 'light' ? 'dark' : 'light') }}>
      {children}
    </ThemeContext.Provider>
  );
}
export const useTheme = () => useContext(ThemeContext)!;
```

Keep Context Providers as **leaf-level** as possible — every component inside the provider re-renders on context change.

**Related:** [NEXT-014 — Server/Client component composition](../nextjs/02-server-client.md#next-014) | [NEXT-128 — RTK Query with App Router](./16-integrations.md#next-128)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-32](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-116

### What are route groups in the App Router?

Route groups let you organize routes into folders without affecting the URL path. Wrap the folder name in parentheses:

```
app/
  (marketing)/
    page.tsx          → /
    about/
      page.tsx        → /about
    layout.tsx        ← shared layout for marketing pages only
  (app)/
    dashboard/
      page.tsx        → /dashboard
    settings/
      page.tsx        → /settings
    layout.tsx        ← shared layout (with sidebar) for app pages only
```

`(marketing)` and `(app)` don't appear in the URL. This lets you apply different layouts to different route sections (e.g., a marketing layout without a sidebar, and an app layout with one) — all within the same Next.js project.

Route groups also let you create multiple root layouts (useful for different shells at the top level).

**Related:** [NEXT-005 — layout.tsx](../nextjs/01-fundamentals.md#next-005) | [NEXT-117 — Parallel routes](./14-app-router-advanced.md#next-117)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-34](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-117

### What are parallel routes in Next.js?

Parallel routes let you render multiple independent pages simultaneously in the same layout — each with its own loading and error states. Define them with `@slot` directories:

```
app/
  layout.tsx
  @team/
    page.tsx       ← rendered as the "team" slot
  @analytics/
    page.tsx       ← rendered as the "analytics" slot
  page.tsx
```

```tsx
// app/layout.tsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode;
  team: React.ReactNode;
  analytics: React.ReactNode;
}) {
  return (
    <>
      {children}
      <div className="sidebar">
        {team}
        {analytics}
      </div>
    </>
  );
}
```

Each slot can independently stream, show loading states, and handle errors. Common use cases: dashboard panels, modals that co-exist with the page, split-view UIs.

**Related:** [NEXT-116 — Route groups](./14-app-router-advanced.md#next-116) | [NEXT-118 — Intercepting routes](./14-app-router-advanced.md#next-118)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-35](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-118

### What are intercepting routes in Next.js?

Intercepting routes let you load a route within the current layout context instead of performing a full page navigation — perfect for modals that show detail content while keeping the background page visible.

Use the `(.)` convention to intercept at the same level, `(..)` for one level up:

```
app/
  feed/
    page.tsx           → /feed (shows photo grid)
    (..)photo/
      [id]/
        page.tsx       → intercepts /photo/:id when navigated from /feed
  photo/
    [id]/
      page.tsx         → /photo/:id (full page, shown on direct URL or refresh)
```

When a user clicks a photo in the feed, the `(..)photo/[id]` route intercepts and renders the photo in a modal. When they share the URL and open it fresh, `/photo/[id]` renders the full page instead.

This pattern (popularized by Instagram/Twitter) maintains context without losing shareability.

**Related:** [NEXT-117 — Parallel routes](./14-app-router-advanced.md#next-117) | [NEXT-116 — Route groups](./14-app-router-advanced.md#next-116)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-36](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-119

### How do you implement nested layouts in the App Router?

Every `layout.tsx` file wraps all routes at its level and below. Nest layouts by creating `layout.tsx` files at each directory level:

```
app/
  layout.tsx              ← root layout (html, body, global nav)
  dashboard/
    layout.tsx            ← dashboard layout (sidebar + header)
    page.tsx              → /dashboard
    settings/
      layout.tsx          ← settings layout (secondary nav tabs)
      page.tsx            → /dashboard/settings
      profile/
        page.tsx          → /dashboard/settings/profile
```

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="dashboard">
      <Sidebar />
      <main>{children}</main>
    </div>
  );
}
```

Layouts are persistent — when navigating between routes that share a layout, the layout doesn't re-render (only the `children` swap). This enables smooth navigation without flickers and allows layout-level data fetching that doesn't re-run on navigation.

**Related:** [NEXT-005 — page.tsx vs layout.tsx vs template.tsx](../nextjs/01-fundamentals.md#next-005) | [NEXT-116 — Route groups](./14-app-router-advanced.md#next-116)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-40](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-120

### How do you handle streaming and Suspense in the App Router?

The App Router supports React streaming via `<Suspense>` — content streams to the browser as it becomes ready instead of waiting for all data:

```tsx
// app/dashboard/page.tsx (Server Component)
import { Suspense } from 'react';
import { UserStats } from './UserStats';  // fast
import { SlowReport } from './SlowReport'; // slow DB query

export default function Dashboard() {
  return (
    <section>
      <UserStats />       {/* renders immediately */}
      <Suspense fallback={<ReportSkeleton />}>
        <SlowReport />    {/* streams in when ready */}
      </Suspense>
    </section>
  );
}
```

`loading.tsx` is a route-level Suspense boundary that wraps the entire page automatically.

For granular streaming, wrap individual slow Server Components in `<Suspense>` with skeleton fallbacks. This way the user sees the page shell instantly while slower sections load progressively.

Streaming works because Next.js uses HTTP chunked transfer encoding to send HTML in pieces. Each Suspense boundary is flushed to the browser as soon as its data is resolved.

**Related:** [NEXT-008 — loading.tsx and streaming](../nextjs/01-fundamentals.md#next-008) | [NEXT-011 — Server Components](../nextjs/02-server-client.md#next-011)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-42](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-131

### How do you stream responses from a Route Handler?

Route Handlers can return a streaming `Response` using the Web Streams API, useful for LLM output, CSV exports, or SSE:

```ts
// app/api/stream/route.ts
export async function GET() {
  const encoder = new TextEncoder();

  const stream = new ReadableStream({
    async start(controller) {
      const items = await fetchLargeDataset(); // generator or paginated source
      for (const item of items) {
        controller.enqueue(encoder.encode(JSON.stringify(item) + '\n'));
        await new Promise(r => setTimeout(r, 10)); // simulate delay
      }
      controller.close();
    },
  });

  return new Response(stream, {
    headers: {
      'Content-Type': 'application/x-ndjson',
      'Transfer-Encoding': 'chunked',
    },
  });
}
```

For **Server-Sent Events (SSE)**:
```ts
return new Response(stream, {
  headers: { 'Content-Type': 'text/event-stream', 'Cache-Control': 'no-cache' },
});
// Each chunk: `data: ${JSON.stringify(payload)}\n\n`
```

For AI streaming (LLM responses), the Vercel AI SDK provides `streamText()` and `toDataStreamResponse()` utilities that handle SSE formatting automatically.

**Related:** [NEXT-120 — Streaming with Suspense](./14-app-router-advanced.md#next-120) | [NEXT-026 — Route Handlers](../nextjs/06-api-actions.md#next-026)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-14](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-133

### How do you implement optimistic UI with the App Router?

Optimistic UI shows the expected result of an action immediately before the server confirms it, making the app feel faster. React 19's `useOptimistic` hook integrates naturally with Server Actions:

```tsx
'use client';
import { useOptimistic, useTransition } from 'react';
import { toggleLike } from './actions';

export function LikeButton({ postId, initialLiked, initialCount }: Props) {
  const [optimisticState, setOptimistic] = useOptimistic(
    { liked: initialLiked, count: initialCount },
    (current, liked: boolean) => ({ liked, count: current.count + (liked ? 1 : -1) })
  );
  const [isPending, startTransition] = useTransition();

  const handleToggle = () => {
    startTransition(async () => {
      setOptimistic(!optimisticState.liked); // immediate UI update
      await toggleLike(postId);             // server action — runs async
    });
  };

  return (
    <button onClick={handleToggle} disabled={isPending}>
      {optimisticState.liked ? '❤️' : '🤍'} {optimisticState.count}
    </button>
  );
}
```

If the Server Action throws, React automatically reverts the optimistic state. No manual rollback needed.

**Related:** [NEXT-027 — Server Actions](../nextjs/06-api-actions.md#next-027) | [NEXT-102 — Form submissions](./14-app-router-advanced.md#next-102)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-16](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [13-security.md](./13-security.md) | [15-i18n.md →](./15-i18n.md)*
