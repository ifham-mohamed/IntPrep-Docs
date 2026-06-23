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
