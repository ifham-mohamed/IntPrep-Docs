# Source Archive — GFE-NJS

## Source Metadata

| Field | Value |
|---|---|
| **Source ID** | GFE-NJS |
| **Title** | Next.js Interview Questions for Freshers: 35 Questions with Answers |
| **Publisher** | GreatFrontEnd |
| **URL** | https://www.greatfrontend.com/blog/next-js-interview-questions-for-freshers |
| **Published** | Jun 9, 2026 |
| **Technology** | Next.js |
| **Source type** | Website article |
| **Total questions in source** | 35 Q&As + 5 embedded scenario deep-dives |
| **New canonical IDs created** | NEXT-001 – NEXT-035 |
| **Overlap with existing canonicals** | 0 (Next.js domain was empty at time of ingestion) |
| **Ingested** | Jun 2026 |

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | 0 |
| New canonical entries created | 35 |
| **Total** | **35** |

> All 35 questions are net-new. The `docs/nextjs/` knowledge base had zero canonical entries prior to this source.

---

## Difficulty Distribution

| Level | Count | % |
|---|---|---|
| 🟢 Beginner | 14 | 40% |
| 🟡 Intermediate | 15 | 43% |
| 🔴 Advanced | 6 | 17% |

---

## Section 1 — Framework & App Router Fundamentals (10 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 1 | What is Next.js and what does it add on top of React? | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | New |
| 2 | How is Next.js different from plain React (e.g. a Vite app)? | [NEXT-002](../../../../docs/nextjs/01-fundamentals.md#next-002) | New |
| 3 | What is the App Router and how does it differ from the Pages Router? | [NEXT-003](../../../../docs/nextjs/01-fundamentals.md#next-003) | New |
| 4 | What is file-based routing in Next.js? | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | New |
| 5 | What is the difference between page.tsx, layout.tsx, and template.tsx? | [NEXT-005](../../../../docs/nextjs/01-fundamentals.md#next-005) | New |
| 6 | What are dynamic routes and how do you define them in the App Router? | [NEXT-006](../../../../docs/nextjs/01-fundamentals.md#next-006) | New |
| 7 | What is generateStaticParams() and what does it replace from the Pages Router? | [NEXT-007](../../../../docs/nextjs/01-fundamentals.md#next-007) | New |
| 8 | What is loading.tsx and how does it relate to streaming? | [NEXT-008](../../../../docs/nextjs/01-fundamentals.md#next-008) | New |
| 9 | What is error.tsx and why must it be a Client Component? | [NEXT-009](../../../../docs/nextjs/01-fundamentals.md#next-009) | New |
| 10 | What is not-found.tsx and how do you trigger it? | [NEXT-010](../../../../docs/nextjs/01-fundamentals.md#next-010) | New |

---

## Section 2 — Server Components & Client Components (4 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 11 | What are Server Components in Next.js and what can’t they do? | [NEXT-011](../../../../docs/nextjs/02-server-client.md#next-011) | New |
| 12 | What are Client Components and when do you use "use client"? | [NEXT-012](../../../../docs/nextjs/02-server-client.md#next-012) | New |
| 13 | When should you use "use client" and what are the downsides of overusing it? | [NEXT-013](../../../../docs/nextjs/02-server-client.md#next-013) | New |
| 14 | Can a Server Component import a Client Component, and vice versa? | [NEXT-014](../../../../docs/nextjs/02-server-client.md#next-014) | New |

---

## Section 3 — Rendering Models (4 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 15 | What is the difference between SSR, SSG, CSR, and ISR? | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | New |
| 16 | What is the difference between static rendering and dynamic rendering? | [NEXT-016](../../../../docs/nextjs/03-rendering.md#next-016) | New |
| 17 | What is hydration in Next.js? | [NEXT-017](../../../../docs/nextjs/03-rendering.md#next-017) | New |
| 18 | What causes hydration errors and how do you fix them? | [NEXT-018](../../../../docs/nextjs/03-rendering.md#next-018) | New |

---

## Section 4 — Data Fetching & Caching (4 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 19 | How do you fetch data in the App Router? | [NEXT-019](../../../../docs/nextjs/04-data-fetching.md#next-019) | New |
| 20 | What is ISR (Incremental Static Regeneration) in Next.js? | [NEXT-020](../../../../docs/nextjs/04-data-fetching.md#next-020) | New |
| 21 | How does caching work in Next.js at a high level? | [NEXT-021](../../../../docs/nextjs/04-data-fetching.md#next-021) | New |
| 22 | What are the new Cache Component APIs ("use cache", cacheLife, cacheTag)? | [NEXT-022](../../../../docs/nextjs/04-data-fetching.md#next-022) | New |

---

## Section 5 — Navigation (3 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 23 | What is next/link used for and when should you use a plain `<a>` tag? | [NEXT-023](../../../../docs/nextjs/05-navigation.md#next-023) | New |
| 24 | What is useRouter() used for in the App Router? | [NEXT-024](../../../../docs/nextjs/05-navigation.md#next-024) | New |
| 25 | What is the difference between redirect() and client-side navigation? | [NEXT-025](../../../../docs/nextjs/05-navigation.md#next-025) | New |

---

## Section 6 — Route Handlers & Server Actions (3 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 26 | What are Route Handlers in the App Router? | [NEXT-026](../../../../docs/nextjs/06-api-actions.md#next-026) | New |
| 27 | What are Server Functions and Server Actions? | [NEXT-027](../../../../docs/nextjs/06-api-actions.md#next-027) | New |
| 28 | What is the difference between a Route Handler and a Server Action? | [NEXT-028](../../../../docs/nextjs/06-api-actions.md#next-028) | New |

---

## Section 7 — SEO, Assets & Metadata (3 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 29 | How do you add metadata for SEO in the App Router? | [NEXT-029](../../../../docs/nextjs/07-seo-assets.md#next-029) | New |
| 30 | What is next/image and why does it matter for performance? | [NEXT-030](../../../../docs/nextjs/07-seo-assets.md#next-030) | New |
| 31 | What is next/font and why use it over manual font link tags? | [NEXT-031](../../../../docs/nextjs/07-seo-assets.md#next-031) | New |

---

## Section 8 — Infrastructure: Proxy, Env, Auth & Deployment (4 questions)

| # | Question (paraphrased) | Canonical ID | Status |
|---|---|---|---|
| 32 | What is Proxy (formerly Middleware) in Next.js and what is it used for? | [NEXT-032](../../../../docs/nextjs/08-infra.md#next-032) | New |
| 33 | How do environment variables work in Next.js and what is NEXT_PUBLIC_? | [NEXT-033](../../../../docs/nextjs/08-infra.md#next-033) | New |
| 34 | How do you handle authentication in a Next.js app? | [NEXT-034](../../../../docs/nextjs/08-infra.md#next-034) | New |
| 35 | How do you deploy a Next.js app and what features need a runtime? | [NEXT-035](../../../../docs/nextjs/08-infra.md#next-035) | New |

---

## Scenario Questions (embedded in canonical entries)

The following 5 scenario questions are NOT standalone canonicals — they are embedded as **Scenario** blocks inside the relevant canonical entries.

| Scenario | Topic | Embedded in |
|---|---|---|
| Product detail page with stale price | Caching + rendering strategy | [NEXT-021](../../../../docs/nextjs/04-data-fetching.md#next-021) |
| Cart button on a Server Component page | Server/Client boundary | [NEXT-014](../../../../docs/nextjs/02-server-client.md#next-014) |
| Search page with query params — static or dynamic? | Static vs dynamic rendering | [NEXT-016](../../../../docs/nextjs/03-rendering.md#next-016) |
| Auth-protected dashboard redirect | Auth + redirect | [NEXT-034](../../../../docs/nextjs/08-infra.md#next-034) |
| Hydration mismatch from localStorage theme | Hydration error causes | [NEXT-018](../../../../docs/nextjs/03-rendering.md#next-018) |

---

## ID Range Summary

| Section | IDs | Target File |
|---|---|---|
| Fundamentals & App Router | NEXT-001 – NEXT-010 | [docs/nextjs/01-fundamentals.md](../../../../docs/nextjs/01-fundamentals.md) |
| Server & Client Components | NEXT-011 – NEXT-014 | [docs/nextjs/02-server-client.md](../../../../docs/nextjs/02-server-client.md) |
| Rendering Models | NEXT-015 – NEXT-018 | [docs/nextjs/03-rendering.md](../../../../docs/nextjs/03-rendering.md) |
| Data Fetching & Caching | NEXT-019 – NEXT-022 | [docs/nextjs/04-data-fetching.md](../../../../docs/nextjs/04-data-fetching.md) |
| Navigation | NEXT-023 – NEXT-025 | [docs/nextjs/05-navigation.md](../../../../docs/nextjs/05-navigation.md) |
| Route Handlers & Server Actions | NEXT-026 – NEXT-028 | [docs/nextjs/06-api-actions.md](../../../../docs/nextjs/06-api-actions.md) |
| SEO, Assets & Metadata | NEXT-029 – NEXT-031 | [docs/nextjs/07-seo-assets.md](../../../../docs/nextjs/07-seo-assets.md) |
| Infrastructure | NEXT-032 – NEXT-035 | [docs/nextjs/08-infra.md](../../../../docs/nextjs/08-infra.md) |
| **Total** | **NEXT-001 – NEXT-035** | **8 files** |

---

*← [Source Library](../../../../docs/reference/02-source-library.md)*
