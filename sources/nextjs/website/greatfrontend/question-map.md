# Source Archive — GreatFrontEnd: Next.js Interview Questions for Freshers

> **Source:** https://www.greatfrontend.com/blog/next-js-interview-questions-for-freshers
> **Author:** GreatFrontEnd Team
> **Published:** Jun 9, 2026
> **Questions extracted:** 35
> **Source type:** Website article
> **Technology:** Next.js
> **Source code prefix:** GFE-NJS

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | 0 |
| New canonical created | 35 |
| **Total** | **35** |

> All 35 questions are net-new. The `docs/nextjs/` knowledge base had zero canonical entries prior to this source.

---

## Section 1 — Framework & App Router Fundamentals (10 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 1 | What is Next.js and what does it add on top of React? | NEXT-001 | New | docs/nextjs/01-fundamentals.md |
| 2 | How is Next.js different from plain React (e.g. a Vite app)? | NEXT-002 | New | docs/nextjs/01-fundamentals.md |
| 3 | What is the App Router and how does it differ from the Pages Router? | NEXT-003 | New | docs/nextjs/01-fundamentals.md |
| 4 | What is file-based routing in Next.js? | NEXT-004 | New | docs/nextjs/01-fundamentals.md |
| 5 | What is the difference between page.tsx, layout.tsx, and template.tsx? | NEXT-005 | New | docs/nextjs/01-fundamentals.md |
| 6 | What are dynamic routes and how do you define them in the App Router? | NEXT-006 | New | docs/nextjs/01-fundamentals.md |
| 7 | What is generateStaticParams() and what does it replace from the Pages Router? | NEXT-007 | New | docs/nextjs/01-fundamentals.md |
| 8 | What is loading.tsx and how does it relate to streaming? | NEXT-008 | New | docs/nextjs/01-fundamentals.md |
| 9 | What is error.tsx and why must it be a Client Component? | NEXT-009 | New | docs/nextjs/01-fundamentals.md |
| 10 | What is not-found.tsx and how do you trigger it? | NEXT-010 | New | docs/nextjs/01-fundamentals.md |

---

## Section 2 — Server Components & Client Components (4 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 11 | What are Server Components in Next.js and what can't they do? | NEXT-011 | New | docs/nextjs/02-server-client.md |
| 12 | What are Client Components and when do you use "use client"? | NEXT-012 | New | docs/nextjs/02-server-client.md |
| 13 | When should you use "use client" and what are the downsides of overusing it? | NEXT-013 | New | docs/nextjs/02-server-client.md |
| 14 | Can a Server Component import a Client Component, and vice versa? | NEXT-014 | New | docs/nextjs/02-server-client.md |

---

## Section 3 — Rendering Models (4 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 15 | What is the difference between SSR, SSG, CSR, and ISR? | NEXT-015 | New | docs/nextjs/03-rendering.md |
| 16 | What is the difference between static rendering and dynamic rendering? | NEXT-016 | New | docs/nextjs/03-rendering.md |
| 17 | What is hydration in Next.js? | NEXT-017 | New | docs/nextjs/03-rendering.md |
| 18 | What causes hydration errors and how do you fix them? | NEXT-018 | New | docs/nextjs/03-rendering.md |

---

## Section 4 — Data Fetching & Caching (4 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 19 | How do you fetch data in the App Router? | NEXT-019 | New | docs/nextjs/04-data-fetching.md |
| 20 | What is ISR (Incremental Static Regeneration) in Next.js? | NEXT-020 | New | docs/nextjs/04-data-fetching.md |
| 21 | How does caching work in Next.js at a high level? | NEXT-021 | New | docs/nextjs/04-data-fetching.md |
| 22 | What are the new Cache Component APIs ("use cache", cacheLife, cacheTag)? | NEXT-022 | New | docs/nextjs/04-data-fetching.md |

---

## Section 5 — Navigation (3 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 23 | What is next/link used for and when should you use a plain <a> tag? | NEXT-023 | New | docs/nextjs/05-navigation.md |
| 24 | What is useRouter() used for in the App Router? | NEXT-024 | New | docs/nextjs/05-navigation.md |
| 25 | What is the difference between redirect() and client-side navigation? | NEXT-025 | New | docs/nextjs/05-navigation.md |

---

## Section 6 — Route Handlers & Server Actions (3 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 26 | What are Route Handlers in the App Router? | NEXT-026 | New | docs/nextjs/06-api-actions.md |
| 27 | What are Server Functions and Server Actions? | NEXT-027 | New | docs/nextjs/06-api-actions.md |
| 28 | What is the difference between a Route Handler and a Server Action? | NEXT-028 | New | docs/nextjs/06-api-actions.md |

---

## Section 7 — SEO, Assets & Metadata (3 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 29 | How do you add metadata for SEO in the App Router? | NEXT-029 | New | docs/nextjs/07-seo-assets.md |
| 30 | What is next/image and why does it matter for performance? | NEXT-030 | New | docs/nextjs/07-seo-assets.md |
| 31 | What is next/font and why use it over manual font link tags? | NEXT-031 | New | docs/nextjs/07-seo-assets.md |

---

## Section 8 — Infrastructure: Proxy, Env, Auth & Deployment (4 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| 32 | What is Proxy (formerly Middleware) in Next.js and what is it used for? | NEXT-032 | New | docs/nextjs/08-infra.md |
| 33 | How do environment variables work in Next.js and what is NEXT_PUBLIC_? | NEXT-033 | New | docs/nextjs/08-infra.md |
| 34 | How do you handle authentication in a Next.js app? | NEXT-034 | New | docs/nextjs/08-infra.md |
| 35 | How do you deploy a Next.js app and what features need a runtime? | NEXT-035 | New | docs/nextjs/08-infra.md |

---

## Scenario Questions (embedded in canonical entries as extended examples)

The following 5 scenario questions from the source are NOT standalone canonicals — they are embedded as **Scenario** blocks inside the relevant canonical entries listed below.

| Scenario | Topic | Embedded in |
|---|---|---|
| Product detail page with stale price | Caching + rendering strategy | NEXT-021 |
| Cart button on a Server Component page | Server/Client boundary | NEXT-014 |
| Search page with query params — static or dynamic? | Static vs dynamic rendering | NEXT-016 |
| Auth-protected dashboard redirect | Auth + redirect | NEXT-034 |
| Hydration mismatch from localStorage theme | Hydration error causes | NEXT-018 |

---

## ID Range Summary

| Section | IDs | Target File |
|---|---|---|
| Fundamentals & App Router | NEXT-001 – NEXT-010 | docs/nextjs/01-fundamentals.md |
| Server & Client Components | NEXT-011 – NEXT-014 | docs/nextjs/02-server-client.md |
| Rendering Models | NEXT-015 – NEXT-018 | docs/nextjs/03-rendering.md |
| Data Fetching & Caching | NEXT-019 – NEXT-022 | docs/nextjs/04-data-fetching.md |
| Navigation | NEXT-023 – NEXT-025 | docs/nextjs/05-navigation.md |
| Route Handlers & Server Actions | NEXT-026 – NEXT-028 | docs/nextjs/06-api-actions.md |
| SEO, Assets & Metadata | NEXT-029 – NEXT-031 | docs/nextjs/07-seo-assets.md |
| Infrastructure | NEXT-032 – NEXT-035 | docs/nextjs/08-infra.md |
| **Total** | **NEXT-001 – NEXT-035** | **8 files** |

---

*Phase 2 complete. Phase 3 will write original canonical entries for all 35 questions into their respective target files.*
