# Next.js — Route Handlers & Server Actions

> **IDs:** NEXT-026 – NEXT-028
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-026

### What are Route Handlers in the App Router?

🟢 Beginner

Route Handlers are the App Router's way to create API endpoints inside your Next.js project. You create a `route.ts` file inside `app/` at any path, and that path becomes an HTTP endpoint. They replace `pages/api/` from the Pages Router.

You export named functions corresponding to HTTP methods (`GET`, `POST`, `PUT`, `DELETE`, `PATCH`).

```tsx
// app/api/users/route.ts
import { NextRequest } from 'next/server';

export async function GET() {
  const users = await db.users.findMany();
  return Response.json(users);
}

export async function POST(req: NextRequest) {
  const body = await req.json();
  const user = await db.users.create({ data: body });
  return Response.json(user, { status: 201 });
}
```

Route Handlers support full Web API `Request` and `Response` objects. You can also use `NextRequest` and `NextResponse` for Next.js-specific utilities like cookies and headers.

Common use cases:
- Webhook receivers (e.g., from Stripe, GitHub)
- Auth callbacks (OAuth redirect handlers)
- API routes consumed by external services
- Proxying third-party APIs to hide credentials

**Note:** Route Handlers and `page.tsx` cannot coexist at the same path — they are mutually exclusive.

**Related:** NEXT-027 — Server Actions | NEXT-028 — Route Handlers vs Server Actions

**Source:** GFE-NJS-026

---

## NEXT-027

### What are Server Functions and Server Actions?

🟡 Intermediate

**Server Functions** are async functions defined with the `"use server"` directive that run exclusively on the server but can be called from Client Components. They are the mechanism behind Server Actions.

**Server Actions** are Server Functions used specifically for form submissions and data mutations. They integrate directly with HTML `<form>` `action` attributes and React hooks like `useActionState` and `useOptimistic`.

You mark a file or individual function with `"use server"` to create one:

```tsx
// actions/create-post.ts
"use server";
import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';

export async function createPost(formData: FormData) {
  const title = formData.get('title') as string;
  const content = formData.get('content') as string;

  // Direct DB call — runs on server, never exposed to client
  await db.posts.create({ data: { title, content } });

  revalidatePath('/blog');   // Invalidate the blog page cache
  redirect('/blog');         // Redirect after mutation
}

// app/blog/new/page.tsx — Server Component with a form
import { createPost } from '@/actions/create-post';

export default function NewPostPage() {
  return (
    <form action={createPost}>
      <input name="title" placeholder="Title" />
      <textarea name="content" placeholder="Content" />
      <button type="submit">Publish</button>
    </form>
  );
}
```

Server Actions work even without JavaScript (progressive enhancement) because they use native HTML form submission as the transport.

**Related:** NEXT-026 — Route Handlers | NEXT-028 — Route Handlers vs Server Actions

**Source:** GFE-NJS-027

---

## NEXT-028

### What is the difference between a Route Handler and a Server Action?

🟡 Intermediate

Both run on the server, but they solve different problems and have different interfaces.

| Dimension | Route Handler | Server Action |
|---|---|---|
| **Definition** | `route.ts` file with exported HTTP method functions | Async function with `"use server"` directive |
| **Invoked by** | HTTP requests (external services, fetch calls) | React (forms, Client Component calls, `useTransition`) |
| **URL** | Has an explicit URL (`/api/webhook`) | No public URL — called via React's action protocol |
| **Primary use** | Webhooks, OAuth, external API consumers | Form mutations, data writes from the UI |
| **Response format** | `Response.json()` — standard HTTP | Return value passed back to React (or void) |
| **Progressive enhancement** | No | Yes — forms work without JS |
| **Caching** | GET handlers can be cached | Not cached — always fresh |

**Rule of thumb:**
- If an **external service** needs to call your endpoint → Route Handler
- If your **own UI** is triggering a mutation → Server Action

```tsx
// Route Handler — Stripe webhook (external service calls this)
// app/api/stripe/webhook/route.ts
export async function POST(req: Request) {
  const event = await verifyStripeWebhook(req);
  await handleStripeEvent(event);
  return Response.json({ received: true });
}

// Server Action — user clicking "Delete post" in the UI
// actions/delete-post.ts
"use server";
export async function deletePost(postId: string) {
  await db.posts.delete({ where: { id: postId } });
  revalidatePath('/blog');
}
```

**Related:** NEXT-026 — Route Handlers | NEXT-027 — Server Actions

**Source:** GFE-NJS-028

---

## NEXT-112

### What are the benefits of Server Actions?

Server Actions (`"use server"`) are async functions that run on the server and can be invoked directly from Client Components or HTML forms without writing a separate API endpoint.

Key benefits:
- **No API boilerplate** — no `fetch('/api/...')`, no route file, no request/response serialization.
- **Progressively enhanced forms** — work even without JavaScript; the form posts to the server action automatically.
- **Type-safe end-to-end** — TypeScript types flow from action signature to call site.
- **Automatic revalidation** — call `revalidatePath()` or `revalidateTag()` inside the action to refresh cached data.
- **Co-location** — actions live alongside the UI code that uses them.

```ts
// app/actions.ts
'use server';
import { revalidatePath } from 'next/cache';

export async function createPost(formData: FormData) {
  await db.post.create({ data: { title: formData.get('title') } });
  revalidatePath('/posts');
}
```

**Related:** [NEXT-027 — Server Functions and Server Actions](./06-api-actions.md#next-027) | [NEXT-113 — Drawbacks of Server Actions](./06-api-actions.md#next-113)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-25](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-113

### What are the drawbacks of Server Actions?

Despite their convenience, Server Actions have real limitations:

- **Limited error handling UX** — throwing inside an action propagates to the nearest `error.tsx`, not to an inline form error state (you must use `useActionState` to catch errors gracefully).
- **No streaming** — a Server Action runs synchronously from the client's perspective; you can't stream partial results back (use a Route Handler for streaming).
- **Harder to test** — actions depend on Next.js server context; unit-testing them requires mocking request internals.
- **Tight coupling** — mixing data-mutation logic inside UI component files can hurt separation of concerns at scale.
- **No request deduplication** — multiple concurrent invocations all hit the server; there is no built-in debounce or queuing.
- **Cannot return non-serializable values** — return values must be JSON-serializable.

For complex workflows (webhooks, streaming, third-party callbacks), a Route Handler is still the right tool.

**Related:** [NEXT-112 — Benefits of Server Actions](./06-api-actions.md#next-112) | [NEXT-114 — Alternatives to Server Actions](./06-api-actions.md#next-114) | [NEXT-026 — Route Handlers](./06-api-actions.md#next-026)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-26](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-114

### What are the alternatives to Server Actions?

If Server Actions don't fit your use case, several alternatives exist:

| Alternative | Best for |
|---|---|
| **Route Handlers** (`app/api/route.ts`) | Webhooks, streaming, binary responses, third-party callbacks |
| **tRPC** | Fully type-safe client-server RPC without code generation |
| **GraphQL** (Apollo, Pothos) | Complex relational data, multiple consumers |
| **REST API (external)** | Microservices that Next.js consumes via fetch |
| **Edge Functions** | Ultra-low-latency, geographically distributed logic |

```ts
// Route Handler as alternative — supports streaming, binary, webhooks
// app/api/export/route.ts
export async function GET() {
  const stream = new ReadableStream({ /* ... */ });
  return new Response(stream, { headers: { 'Content-Type': 'text/csv' } });
}
```

For simple CRUD that doesn't need streaming, Server Actions are usually the simplest path. For anything requiring fine-grained HTTP control, a Route Handler is the correct choice.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-027 — Server Actions](./06-api-actions.md#next-027) | [NEXT-028 — Route Handler vs Server Action](./06-api-actions.md#next-028)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-27](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-122

### What are best practices for Next.js API routes?

Whether using Route Handlers (App Router) or API Routes (Pages Router):

**1. Validate input immediately**
```ts
import { z } from 'zod';
const schema = z.object({ email: z.string().email() });
const result = schema.safeParse(await req.json());
if (!result.success) return Response.json({ error: result.error }, { status: 400 });
```

**2. Use correct HTTP methods** — GET for reads, POST for creates, PUT/PATCH for updates, DELETE for deletes. Return 405 for unsupported methods.

**3. Handle errors explicitly** — wrap logic in try/catch and return structured error responses with appropriate status codes (400, 401, 403, 404, 500).

**4. Authenticate and authorize** — check sessions/tokens before processing. Never trust client-provided user IDs.

**5. Keep routes thin** — move business logic into service modules, not inline in the route file.

**6. Rate-limit sensitive endpoints** — use middleware or an upstash/redis-based rate limiter.

**7. Set response headers** — CORS, Cache-Control, Content-Type.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-123 — Input validation](../nextjs/13-security.md#next-123)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-05](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-124

### How do you use proper HTTP methods in Next.js Route Handlers?

Each Route Handler file exports named functions matching the HTTP verb:

```ts
// app/api/posts/route.ts
import { NextRequest } from 'next/server';

export async function GET(req: NextRequest) {
  const posts = await db.post.findMany();
  return Response.json(posts);
}

export async function POST(req: NextRequest) {
  const body = await req.json();
  const post = await db.post.create({ data: body });
  return Response.json(post, { status: 201 });
}

// app/api/posts/[id]/route.ts
export async function PATCH(req: NextRequest, { params }: { params: { id: string } }) {
  const body = await req.json();
  const post = await db.post.update({ where: { id: params.id }, data: body });
  return Response.json(post);
}

export async function DELETE(_: NextRequest, { params }: { params: { id: string } }) {
  await db.post.delete({ where: { id: params.id } });
  return new Response(null, { status: 204 });
}
```

Only exported method names are handled — Next.js returns 405 automatically for any method you don't export.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-122 — API route best practices](./06-api-actions.md#next-122)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-07](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-125

### How do you handle errors in Next.js API routes?

Wrap route logic in try/catch and return structured JSON error responses:

```ts
// app/api/users/[id]/route.ts
export async function GET(_: Request, { params }: { params: { id: string } }) {
  try {
    const user = await db.user.findUniqueOrThrow({ where: { id: params.id } });
    return Response.json(user);
  } catch (error) {
    if (error instanceof Prisma.PrismaClientKnownRequestError && error.code === 'P2025') {
      return Response.json({ error: 'User not found' }, { status: 404 });
    }
    console.error('[GET /api/users/:id]', error);
    return Response.json({ error: 'Internal server error' }, { status: 500 });
  }
}
```

Key principles:
- Map domain errors to appropriate HTTP status codes (400, 401, 403, 404, 409, 500).
- Never expose internal stack traces to clients.
- Log server errors with enough context to debug.
- Validate input before reaching business logic to return 400 early.

**Related:** [NEXT-122 — API route best practices](./06-api-actions.md#next-122) | [NEXT-009 — error.tsx](../nextjs/01-fundamentals.md#next-009)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-08](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-126

### How do you use middleware within Next.js API routes?

In the App Router, middleware runs globally at the edge (`middleware.ts` at the project root). For route-specific logic, compose helper functions:

```ts
// lib/withAuth.ts
import { NextRequest, NextResponse } from 'next/server';
import { getSession } from '@/lib/auth';

type Handler = (req: NextRequest, ctx: any) => Promise<Response>;

export function withAuth(handler: Handler): Handler {
  return async (req, ctx) => {
    const session = await getSession(req);
    if (!session) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    return handler(req, ctx);
  };
}

// app/api/profile/route.ts
import { withAuth } from '@/lib/withAuth';

export const GET = withAuth(async (req) => {
  const user = await db.user.findUnique({ where: { id: req.headers.get('x-user-id')! } });
  return Response.json(user);
});
```

For cross-cutting concerns (auth, CORS, rate-limiting) that apply to many routes, the global `middleware.ts` is the right place. Route-specific logic belongs in composable wrappers.

**Related:** [NEXT-032 — Middleware](../nextjs/08-infra.md#next-032) | [NEXT-122 — API route best practices](./06-api-actions.md#next-122)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-09](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-127

### How do you keep Next.js API routes modular and organized?

Structure API routes by domain, not by HTTP method:

```
app/
  api/
    users/
      route.ts          # GET /api/users, POST /api/users
      [id]/
        route.ts        # GET/PATCH/DELETE /api/users/:id
    posts/
      route.ts
      [slug]/
        route.ts
lib/
  services/
    userService.ts      # business logic
  repositories/
    userRepository.ts   # DB queries
  middleware/
    withAuth.ts
    withRateLimit.ts
```

Rules:
- **Thin route handlers** — validate input, call a service, return a response.
- **Services** — hold business logic (compute, validate domain rules).
- **Repositories** — hold data access (Prisma queries, Redis calls).
- **Shared middleware** — composable wrappers for auth, logging, rate-limiting.

This pattern makes routes testable (mock the service) and prevents route files from becoming monolithic.

**Related:** [NEXT-122 — API route best practices](./06-api-actions.md#next-122) | [NEXT-026 — Route Handlers](./06-api-actions.md#next-026)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-10](../../sources/nextjs/github/mrhrifat/question-map.md)
