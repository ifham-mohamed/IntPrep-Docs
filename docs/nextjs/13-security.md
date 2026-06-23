# Next.js Security

This file covers CORS, cookies, authentication patterns, JWT, input validation, authorization, and security best practices for Next.js apps.

---

## NEXT-070

### How do you handle CORS in Next.js Route Handlers?

Route Handlers are same-origin by default. To allow cross-origin requests, set CORS headers explicitly:

```ts
// app/api/data/route.ts
import { NextRequest, NextResponse } from 'next/server';

const ALLOWED_ORIGINS = ['https://app.example.com', 'https://admin.example.com'];

function corsHeaders(origin: string | null) {
  const allowed = origin && ALLOWED_ORIGINS.includes(origin) ? origin : ALLOWED_ORIGINS[0];
  return {
    'Access-Control-Allow-Origin': allowed,
    'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type, Authorization',
  };
}

export async function OPTIONS(req: NextRequest) {
  return new Response(null, { status: 204, headers: corsHeaders(req.headers.get('origin')) });
}

export async function GET(req: NextRequest) {
  const data = await fetchData();
  return NextResponse.json(data, { headers: corsHeaders(req.headers.get('origin')) });
}
```

For global CORS, configure it in `next.config.js` under `headers()`. For public APIs with no origin restriction, use `'Access-Control-Allow-Origin': '*'`.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-032 — Middleware](./08-infra.md#next-032)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-60](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-071

### How do you manage cookies in Next.js?

Next.js provides `cookies()` from `next/headers` for server-side cookie access, and `document.cookie` / libraries like `js-cookie` on the client.

```ts
// Server Component / Server Action / Route Handler
import { cookies } from 'next/headers';

// Read
const cookieStore = await cookies();
const token = cookieStore.get('auth-token')?.value;

// Set (only in Server Actions or Route Handlers, not Server Components)
cookieStore.set('auth-token', value, {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'lax',
  maxAge: 60 * 60 * 24 * 7, // 1 week
  path: '/',
});

// Delete
cookieStore.delete('auth-token');
```

```ts
// Route Handler — set cookie in response
export async function POST(req: Request) {
  const res = Response.json({ ok: true });
  res.headers.set('Set-Cookie', 'auth-token=abc; HttpOnly; Secure; SameSite=Lax; Path=/');
  return res;
}
```

Always use `HttpOnly` + `Secure` + `SameSite=Lax` for session/auth cookies to protect against XSS and CSRF.

**Related:** [NEXT-130 — Reading/setting cookies in App Router](./13-security.md#next-130) | [NEXT-034 — Authentication](./08-infra.md#next-034)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-61](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-072

### How do you secure a Next.js App Router application?

Security in the App Router spans multiple layers:

**1. Authentication & Authorization**
- Use NextAuth.js (Auth.js) or a dedicated auth provider (Clerk, Auth0).
- Check sessions in Middleware for route-level protection.
- Re-validate auth inside Server Components / Route Handlers — don't trust Middleware alone.

**2. Input validation**
- Validate all inputs (forms, query params, API bodies) with Zod or Yup.
- Sanitize HTML if rendering user content (DOMPurify).

**3. Cookie security**
- Always use `HttpOnly`, `Secure`, `SameSite=Lax` for auth cookies.

**4. Environment variables**
- Keep secrets in server-only env vars (no `NEXT_PUBLIC_` prefix).

**5. CORS**
- Restrict origins on Route Handlers that handle sensitive data.

**6. Content Security Policy**
- Add CSP headers via `next.config.js` `headers()` to prevent XSS.

**7. Rate limiting**
- Use Middleware + Upstash Redis to rate-limit API routes.

**8. Dependency audits**
- Run `npm audit` regularly; keep Next.js and dependencies updated.

**Related:** [NEXT-034 — Authentication](./08-infra.md#next-034) | [NEXT-032 — Middleware](./08-infra.md#next-032) | [NEXT-123 — Input validation](./13-security.md#next-123)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-63](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-080

### How do you prevent Next.js API routes from being accessed by the client directly?

Route Handlers are HTTP endpoints — anyone can call them. To restrict access:

**1. Check authentication**
```ts
import { getSession } from '@/lib/auth';
export async function GET(req: Request) {
  const session = await getSession(req);
  if (!session) return Response.json({ error: 'Unauthorized' }, { status: 401 });
  // ...
}
```

**2. Verify request origin (CSRF protection)**
Check the `Origin` or `Referer` header, or use CSRF tokens for state-mutating endpoints.

**3. Use secret tokens for internal routes**
```ts
const INTERNAL_SECRET = process.env.INTERNAL_API_SECRET;
export async function POST(req: Request) {
  if (req.headers.get('x-internal-secret') !== INTERNAL_SECRET)
    return Response.json({ error: 'Forbidden' }, { status: 403 });
}
```

**4. Move logic to Server Actions**
If a route is only called from your own UI (not third parties), a Server Action is less exposed because it requires a special `Next-Action` header that browsers don't send arbitrarily.

**5. Rate limiting** via Middleware blocks brute-force access even if auth is bypassed.

**Related:** [NEXT-026 — Route Handlers](./06-api-actions.md#next-026) | [NEXT-072 — Security practices](./13-security.md#next-072)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-76](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-081

### How do you use JWT tokens in Next.js?

JWT (JSON Web Tokens) are a common stateless authentication mechanism. In Next.js, they're typically issued on login and stored in `HttpOnly` cookies:

```ts
// app/api/login/route.ts
import jwt from 'jsonwebtoken';

export async function POST(req: Request) {
  const { email, password } = await req.json();
  const user = await validateCredentials(email, password);
  if (!user) return Response.json({ error: 'Invalid credentials' }, { status: 401 });

  const token = jwt.sign({ userId: user.id, role: user.role }, process.env.JWT_SECRET!, { expiresIn: '7d' });

  const res = Response.json({ ok: true });
  res.headers.set('Set-Cookie', `auth=${token}; HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=${60*60*24*7}`);
  return res;
}
```

```ts
// Verify in middleware or Server Component
import jwt from 'jsonwebtoken';
import { cookies } from 'next/headers';

async function getUser() {
  const token = (await cookies()).get('auth')?.value;
  if (!token) return null;
  try {
    return jwt.verify(token, process.env.JWT_SECRET!) as { userId: string; role: string };
  } catch {
    return null;
  }
}
```

For most apps, using Auth.js (NextAuth) is preferable to managing JWTs manually — it handles token rotation, session invalidation, and OAuth providers.

**Related:** [NEXT-034 — Authentication](./08-infra.md#next-034) | [NEXT-103 — Auth.js setup](./14-app-router-advanced.md#next-103)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-77](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-104

### How do you handle authentication tokens in Next.js?

Authentication tokens (JWTs or opaque session tokens) must be stored, transmitted, and verified securely:

**Storage:**
- Store in `HttpOnly; Secure; SameSite=Lax` cookies — inaccessible to JavaScript, prevents XSS theft.
- Never store in `localStorage` or `sessionStorage`.

**Transmission:**
- Cookies send automatically with same-origin requests — no extra code needed.
- For cross-origin API calls, include credentials: `fetch(url, { credentials: 'include' })`.

**Verification:**
```ts
// middleware.ts — protect all /dashboard routes
import { NextRequest, NextResponse } from 'next/server';
import { jwtVerify } from 'jose';

export async function middleware(req: NextRequest) {
  const token = req.cookies.get('auth')?.value;
  if (!token) return NextResponse.redirect(new URL('/login', req.url));
  try {
    await jwtVerify(token, new TextEncoder().encode(process.env.JWT_SECRET));
    return NextResponse.next();
  } catch {
    return NextResponse.redirect(new URL('/login', req.url));
  }
}
export const config = { matcher: ['/dashboard/:path*'] };
```

**Rotation:** Refresh tokens before they expire (sliding sessions). Revoke on logout by deleting the cookie server-side.

**Related:** [NEXT-081 — JWT tokens](./13-security.md#next-081) | [NEXT-032 — Middleware](./08-infra.md#next-032)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-09](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-111

### How do you handle authorization in Next.js middleware and route handlers?

Authorization (what an authenticated user can do) operates at multiple layers:

**Middleware — coarse-grained (route-level)**
```ts
// middleware.ts
export async function middleware(req: NextRequest) {
  const session = await getSession(req); // decode JWT or session cookie
  if (!session) return NextResponse.redirect(new URL('/login', req.url));
  if (req.nextUrl.pathname.startsWith('/admin') && session.role !== 'admin')
    return NextResponse.redirect(new URL('/forbidden', req.url));
  return NextResponse.next();
}
```

**Server Components — fine-grained (data-level)**
```tsx
// app/admin/users/page.tsx
import { getSession } from '@/lib/auth';
import { redirect } from 'next/navigation';

export default async function AdminUsers() {
  const session = await getSession();
  if (session?.role !== 'admin') redirect('/forbidden');
  const users = await db.user.findMany();
  return <UserList users={users} />;
}
```

**Route Handlers — API-level**
Check roles before processing mutations. Never rely solely on UI hiding to enforce authorization.

**Key principle:** Defense in depth — enforce authorization at every layer (Middleware + Server Component + Route Handler), not just one.

**Related:** [NEXT-032 — Middleware](./08-infra.md#next-032) | [NEXT-034 — Authentication](./08-infra.md#next-034)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS A-22](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-123

### How do you validate and sanitize input data in Next.js?

Always validate inputs at the server boundary — never trust the client:

```ts
// Validation with Zod in a Server Action
'use server';
import { z } from 'zod';

const CreatePostSchema = z.object({
  title: z.string().min(3).max(100),
  content: z.string().min(10),
  tags: z.array(z.string()).max(5),
});

export async function createPost(formData: FormData) {
  const raw = {
    title: formData.get('title'),
    content: formData.get('content'),
    tags: formData.getAll('tags'),
  };

  const result = CreatePostSchema.safeParse(raw);
  if (!result.success) {
    return { errors: result.error.flatten().fieldErrors };
  }

  await db.post.create({ data: result.data });
}
```

**Sanitization for HTML content:**
```ts
import DOMPurify from 'isomorphic-dompurify';
const cleanHtml = DOMPurify.sanitize(userHtml, { ALLOWED_TAGS: ['b', 'i', 'a', 'p'] });
```

- **Validate** structure, types, and constraints (Zod, Yup, Valibot).
- **Sanitize** HTML to remove XSS vectors (DOMPurify).
- **Escape** SQL/NoSQL queries — use your ORM (Prisma, Drizzle) instead of raw strings.

**Related:** [NEXT-122 — API route best practices](./06-api-actions.md#next-122) | [NEXT-072 — Security practices](./13-security.md#next-072)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-06](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-130

### How do you read and set cookies in the App Router?

The App Router uses `cookies()` from `next/headers` for server-side access:

```ts
// Server Component — READ ONLY
import { cookies } from 'next/headers';

export default async function Page() {
  const cookieStore = await cookies();
  const theme = cookieStore.get('theme')?.value ?? 'light';
  return <div data-theme={theme}>...</div>;
}
```

```ts
// Server Action or Route Handler — READ AND WRITE
'use server';
import { cookies } from 'next/headers';

export async function setTheme(theme: string) {
  const cookieStore = await cookies();
  cookieStore.set('theme', theme, {
    httpOnly: false,   // theme is not sensitive — readable by JS is fine
    secure: true,
    sameSite: 'lax',
    maxAge: 60 * 60 * 24 * 365, // 1 year
  });
}
```

You cannot set cookies from a Server Component (read-only there). Setting cookies requires a Server Action or Route Handler response. For client-side cookies (non-sensitive), use `document.cookie` or `js-cookie` in a Client Component.

**Related:** [NEXT-071 — Cookie management](./13-security.md#next-071) | [NEXT-027 — Server Actions](./06-api-actions.md#next-027)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-13](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [12-scripts-seo.md](./12-scripts-seo.md) | [14-app-router-advanced.md →](./14-app-router-advanced.md)*
