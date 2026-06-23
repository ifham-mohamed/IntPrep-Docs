# Next.js Knowledge Base

> **Status:** Active — 35 canonical entries, NEXT-001 to NEXT-035.
> **Source:** GFE-NJS — GreatFrontEnd Next.js Interview Questions for Freshers (Jun 2026)

---

## Quick Navigation

| File | IDs | Topics |
|---|---|---|
| [01-fundamentals.md](./01-fundamentals.md) | NEXT-001 – 010 | What is Next.js, App Router vs Pages Router, file-based routing, dynamic routes, `generateStaticParams`, `loading.tsx`, `error.tsx`, `not-found.tsx` |
| [02-server-client.md](./02-server-client.md) | NEXT-011 – 014 | Server Components, Client Components, `"use client"` guidance, composition boundary rules |
| [03-rendering.md](./03-rendering.md) | NEXT-015 – 018 | SSR, SSG, CSR, ISR, static vs dynamic rendering, hydration, hydration errors |
| [04-data-fetching.md](./04-data-fetching.md) | NEXT-019 – 022 | `async/await` in Server Components, ISR, caching layers, `"use cache"` / `cacheLife` / `cacheTag` |
| [05-navigation.md](./05-navigation.md) | NEXT-023 – 025 | `next/link`, `useRouter`, `redirect()` vs client navigation |
| [06-api-actions.md](./06-api-actions.md) | NEXT-026 – 028 | Route Handlers, Server Functions, Server Actions, Route Handler vs Server Action |
| [07-seo-assets.md](./07-seo-assets.md) | NEXT-029 – 031 | `generateMetadata`, `next/image`, `next/font` |
| [08-infra.md](./08-infra.md) | NEXT-032 – 035 | Proxy (Middleware), environment variables, authentication, deployment |

---

## Next.js Master Index

| ID | Title | Difficulty | Source |
|---|---|---|---|
| NEXT-001 | What is Next.js and what does it add on top of React? | 🟢 | GFE-NJS |
| NEXT-002 | How is Next.js different from a plain React (Vite) app? | 🟢 | GFE-NJS |
| NEXT-003 | What is the App Router and how does it differ from the Pages Router? | 🟢 | GFE-NJS |
| NEXT-004 | What is file-based routing in Next.js? | 🟢 | GFE-NJS |
| NEXT-005 | What is the difference between page.tsx, layout.tsx, and template.tsx? | 🟡 | GFE-NJS |
| NEXT-006 | What are dynamic routes and how do you define them in the App Router? | 🟢 | GFE-NJS |
| NEXT-007 | What is generateStaticParams() and what does it replace from the Pages Router? | 🟡 | GFE-NJS |
| NEXT-008 | What is loading.tsx and how does it relate to streaming? | 🟡 | GFE-NJS |
| NEXT-009 | What is error.tsx and why must it be a Client Component? | 🟡 | GFE-NJS |
| NEXT-010 | What is not-found.tsx and how do you trigger it? | 🟢 | GFE-NJS |
| NEXT-011 | What are Server Components in Next.js and what can’t they do? | 🟢 | GFE-NJS |
| NEXT-012 | What are Client Components and when do you use "use client"? | 🟢 | GFE-NJS |
| NEXT-013 | When should you use "use client" and what are the downsides of overusing it? | 🟡 | GFE-NJS |
| NEXT-014 | Can a Server Component import a Client Component, and vice versa? | 🟡 | GFE-NJS |
| NEXT-015 | What is the difference between SSR, SSG, CSR, and ISR? | 🟡 | GFE-NJS |
| NEXT-016 | What is the difference between static rendering and dynamic rendering? | 🟡 | GFE-NJS |
| NEXT-017 | What is hydration in Next.js? | 🟢 | GFE-NJS |
| NEXT-018 | What causes hydration errors and how do you fix them? | 🟡 | GFE-NJS |
| NEXT-019 | How do you fetch data in the App Router? | 🟢 | GFE-NJS |
| NEXT-020 | What is Incremental Static Regeneration (ISR) in Next.js? | 🟡 | GFE-NJS |
| NEXT-021 | How does caching work in Next.js at a high level? | 🔴 | GFE-NJS |
| NEXT-022 | What are the new Cache Component APIs (“use cache”, cacheLife, cacheTag)? | 🔴 | GFE-NJS |
| NEXT-023 | What is next/link used for and when should you use a plain `<a>` tag? | 🟢 | GFE-NJS |
| NEXT-024 | What is useRouter() used for in the App Router? | 🟡 | GFE-NJS |
| NEXT-025 | What is the difference between redirect() and client-side navigation? | 🟡 | GFE-NJS |
| NEXT-026 | What are Route Handlers in the App Router? | 🟢 | GFE-NJS |
| NEXT-027 | What are Server Functions and Server Actions? | 🟡 | GFE-NJS |
| NEXT-028 | What is the difference between a Route Handler and a Server Action? | 🟡 | GFE-NJS |
| NEXT-029 | How do you add metadata for SEO in the App Router? | 🟢 | GFE-NJS |
| NEXT-030 | What is next/image and why does it matter for performance? | 🟡 | GFE-NJS |
| NEXT-031 | What is next/font and why use it over a manual font link tag? | 🟡 | GFE-NJS |
| NEXT-032 | What is Proxy (formerly Middleware) in Next.js and what is it used for? | 🟡 | GFE-NJS |
| NEXT-033 | How do environment variables work in Next.js and what is NEXT_PUBLIC_? | 🟢 | GFE-NJS |
| NEXT-034 | How do you handle authentication in a Next.js app? | 🟡 | GFE-NJS |
| NEXT-035 | How do you deploy a Next.js app and what features require a runtime? | 🟢 | GFE-NJS |

**35 entries — NEXT-001 to NEXT-035 — Next available: NEXT-036**

---

## Cross-References to React Knowledge Base

Many Next.js concepts build directly on React:

- **Server Components** → [REACT-106](../react/07-react19.md#react-106), [REACT-107](../react/07-react19.md#react-107)
- **Hydration** → [REACT-039](../react/03-advanced.md#react-039)
- **SSR** → [REACT-057](../react/03-advanced.md#react-057)
- **SSG** → [REACT-058](../react/03-advanced.md#react-058)
- **Actions / useActionState** → [REACT-102](../react/07-react19.md#react-102), [REACT-103](../react/07-react19.md#react-103)
- **useTransition** → [REACT-054](../react/03-advanced.md#react-054), [REACT-109](../react/07-react19.md#react-109)
- **useOptimistic** → [REACT-104](../react/07-react19.md#react-104)

---

*← [Master Index](../reference/00-master-index.md) | [Source Library →](../reference/02-source-library.md)*
