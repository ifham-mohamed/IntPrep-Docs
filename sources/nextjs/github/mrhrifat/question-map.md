# Source Archive — mrhrifat/nextjs-interview-questions

> **Source:** https://github.com/mrhrifat/nextjs-interview-questions
> **Author:** @mrhrifat
> **Questions extracted:** 150 (77 Common + 30 Pages Router + 43 App Router + 6 bonus)
> **Source type:** GitHub repository
> **Technology:** Next.js
> **Source ID:** MRH-NJS

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical (GFE-NJS) | 44 |
| New canonical entries created | 106 |
| **Total questions canonicalized** | **150** |

All 150 questions are mapped. New canonicals span **NEXT-036 → NEXT-134**.

---

## Section A — Common (77 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| C-01 | What is Next.js? | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped | — |
| C-02 | How do you create a new Next.js project? | [NEXT-036](../../../../docs/nextjs/01-fundamentals.md#next-036) | New | 01-fundamentals.md |
| C-03 | What is the purpose of the `pages` or `app` directory? | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | Mapped | — |
| C-04 | What is file-based routing in Next.js? | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | Mapped | — |
| C-05 | What are the key features of Next.js? | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped | — |
| C-06 | What are the differences between Next.js and React.js? | [NEXT-002](../../../../docs/nextjs/01-fundamentals.md#next-002) | Mapped | — |
| C-07 | What is the difference between CSR and SSR in Next.js? | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped | — |
| C-08 | What is the Link component in Next.js? | [NEXT-023](../../../../docs/nextjs/05-navigation.md#next-023) | Mapped | — |
| C-09 | What is the `useRouter` hook in Next.js? | [NEXT-024](../../../../docs/nextjs/05-navigation.md#next-024) | Mapped | — |
| C-10 | What is the difference between `push` and `replace` in `useRouter`? | [NEXT-037](../../../../docs/nextjs/05-navigation.md#next-037) | New | 05-navigation.md |
| C-11 | How do you navigate programmatically in Next.js? | [NEXT-024](../../../../docs/nextjs/05-navigation.md#next-024) | Mapped | — |
| C-12 | How do you enable TypeScript in a Next.js project? | [NEXT-038](../../../../docs/nextjs/10-config-tooling.md#next-038) | New | 10-config-tooling.md |
| C-13 | How do you handle environment variables in Next.js? | [NEXT-033](../../../../docs/nextjs/08-infra.md#next-033) | Mapped | — |
| C-14 | What is API Routes in Next.js? | [NEXT-026](../../../../docs/nextjs/06-api-actions.md#next-026) | Mapped | — |
| C-15 | What is the public folder in Next.js? | [NEXT-039](../../../../docs/nextjs/10-config-tooling.md#next-039) | New | 10-config-tooling.md |
| C-16 | What is dynamic import in Next.js (`next/dynamic`)? | [NEXT-040](../../../../docs/nextjs/14-app-router-advanced.md#next-040) | New | 14-app-router-advanced.md |
| C-17 | What is the default port for a Next.js app? | [NEXT-041](../../../../docs/nextjs/10-config-tooling.md#next-041) | New | 10-config-tooling.md |
| C-18 | How to change the default port for a Next.js app? | [NEXT-042](../../../../docs/nextjs/10-config-tooling.md#next-042) | New | 10-config-tooling.md |
| C-19 | What is Fast Refresh in Next.js? | [NEXT-043](../../../../docs/nextjs/10-config-tooling.md#next-043) | New | 10-config-tooling.md |
| C-20 | What is `next.config.js`? | [NEXT-044](../../../../docs/nextjs/10-config-tooling.md#next-044) | New | 10-config-tooling.md |
| C-21 | How do you add component-level CSS in Next.js? | [NEXT-045](../../../../docs/nextjs/11-styling.md#next-045) | New | 11-styling.md |
| C-22 | How do you add global CSS in Next.js? | [NEXT-046](../../../../docs/nextjs/11-styling.md#next-046) | New | 11-styling.md |
| C-23 | How do you use Tailwind CSS in Next.js? | [NEXT-047](../../../../docs/nextjs/11-styling.md#next-047) | New | 11-styling.md |
| C-24 | What is server-side rendering (SSR) in Next.js? | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped | — |
| C-25 | What is static site generation (SSG) in Next.js? | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped | — |
| C-26 | What is the difference between SSG and SSR? | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped | — |
| C-27 | What is pre-rendering in Next.js? | [NEXT-048](../../../../docs/nextjs/03-rendering.md#next-048) | New | 03-rendering.md |
| C-28 | What is ISR (Incremental Static Regeneration) in Next.js? | [NEXT-020](../../../../docs/nextjs/04-data-fetching.md#next-020) | Mapped | — |
| C-29 | What is the Image component in Next.js? | [NEXT-030](../../../../docs/nextjs/07-seo-assets.md#next-030) | Mapped | — |
| C-30 | How do you deploy a Next.js app to Vercel? | [NEXT-035](../../../../docs/nextjs/08-infra.md#next-035) | Mapped | — |
| C-31 | How do you handle redirects in Next.js? | [NEXT-049](../../../../docs/nextjs/10-config-tooling.md#next-049) | New | 10-config-tooling.md |
| C-32 | What is the Head component (`next/head`) in Next.js? | [NEXT-050](../../../../docs/nextjs/09-pages-router.md#next-050) | New | 09-pages-router.md |
| C-33 | What is the `next/head` package used for? | [NEXT-050](../../../../docs/nextjs/09-pages-router.md#next-050) | Mapped (merged C-32) | — |
| C-34 | How do you add custom headers in Next.js? | [NEXT-051](../../../../docs/nextjs/10-config-tooling.md#next-051) | New | 10-config-tooling.md |
| C-35 | What is the use of the `next export` command? | [NEXT-052](../../../../docs/nextjs/10-config-tooling.md#next-052) | New | 10-config-tooling.md |
| C-36 | How do you optimize fonts in Next.js? | [NEXT-031](../../../../docs/nextjs/07-seo-assets.md#next-031) | Mapped | — |
| C-37 | How do you enable custom fonts using `next/font`? | [NEXT-031](../../../../docs/nextjs/07-seo-assets.md#next-031) | Mapped | — |
| C-38 | How do you configure Webpack in Next.js? | [NEXT-053](../../../../docs/nextjs/10-config-tooling.md#next-053) | New | 10-config-tooling.md |
| C-39 | How do you configure a custom Babel setup in Next.js? | [NEXT-054](../../../../docs/nextjs/10-config-tooling.md#next-054) | New | 10-config-tooling.md |
| C-40 | What is the purpose of `next-env.d.ts`? | [NEXT-055](../../../../docs/nextjs/10-config-tooling.md#next-055) | New | 10-config-tooling.md |
| C-41 | What is the purpose of `next-compose-plugins`? | [NEXT-056](../../../../docs/nextjs/10-config-tooling.md#next-056) | New | 10-config-tooling.md |
| C-42 | How do you add polyfills in Next.js? | [NEXT-057](../../../../docs/nextjs/10-config-tooling.md#next-057) | New | 10-config-tooling.md |
| C-43 | What is static optimization in Next.js? | [NEXT-058](../../../../docs/nextjs/03-rendering.md#next-058) | New | 03-rendering.md |
| C-44 | How do you handle internationalization (i18n) in Next.js? | [NEXT-059](../../../../docs/nextjs/15-i18n.md#next-059) | New | 15-i18n.md |
| C-45 | What is React Strict Mode in Next.js? | [NEXT-060](../../../../docs/nextjs/10-config-tooling.md#next-060) | New | 10-config-tooling.md |
| C-46 | What is a singleton router in Next.js? | [NEXT-061](../../../../docs/nextjs/05-navigation.md#next-061) | New | 05-navigation.md |
| C-47 | What is `next/script` used for? | [NEXT-062](../../../../docs/nextjs/12-scripts-seo.md#next-062) | New | 12-scripts-seo.md |
| C-48 | What is middleware in Next.js? | [NEXT-032](../../../../docs/nextjs/08-infra.md#next-032) | Mapped | — |
| C-49 | What is a custom server in Next.js? | [NEXT-063](../../../../docs/nextjs/10-config-tooling.md#next-063) | New | 10-config-tooling.md |
| C-50 | How do you perform client-side data fetching in Next.js? | [NEXT-019](../../../../docs/nextjs/04-data-fetching.md#next-019) | Mapped | — |
| C-51 | How do you set up GraphQL in Next.js? | [NEXT-064](../../../../docs/nextjs/16-integrations.md#next-064) | New | 16-integrations.md |
| C-52 | How do you create API endpoints in Next.js? | [NEXT-026](../../../../docs/nextjs/06-api-actions.md#next-026) | Mapped | — |
| C-53 | What is the use of `next-seo`? | [NEXT-065](../../../../docs/nextjs/12-scripts-seo.md#next-065) | New | 12-scripts-seo.md |
| C-54 | How do you handle routing in a Next.js app? | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | Mapped | — |
| C-55 | How do you configure `next-i18next` in Next.js? | [NEXT-066](../../../../docs/nextjs/15-i18n.md#next-066) | New | 15-i18n.md |
| C-56 | What is `ssr: false` in dynamic import? | [NEXT-067](../../../../docs/nextjs/14-app-router-advanced.md#next-067) | New | 14-app-router-advanced.md |
| C-57 | How do you add Google Analytics to a Next.js project? | [NEXT-068](../../../../docs/nextjs/12-scripts-seo.md#next-068) | New | 12-scripts-seo.md |
| C-58 | How do you add meta tags in Next.js? | [NEXT-029](../../../../docs/nextjs/07-seo-assets.md#next-029) | Mapped | — |
| C-59 | How to add a sitemap in a Next.js app? | [NEXT-069](../../../../docs/nextjs/12-scripts-seo.md#next-069) | New | 12-scripts-seo.md |
| C-60 | How do you handle CORS in Next.js API routes? | [NEXT-070](../../../../docs/nextjs/13-security.md#next-070) | New | 13-security.md |
| C-61 | How do you manage cookies in Next.js? | [NEXT-071](../../../../docs/nextjs/13-security.md#next-071) | New | 13-security.md |
| C-62 | What is the purpose of `next/dynamic`? | [NEXT-040](../../../../docs/nextjs/14-app-router-advanced.md#next-040) | Mapped (merged C-16) | — |
| C-63 | How to consider security in the Next.js App Router? | [NEXT-072](../../../../docs/nextjs/13-security.md#next-072) | New | 13-security.md |
| C-64 | What is the `useTranslation` hook in Next.js? | [NEXT-073](../../../../docs/nextjs/15-i18n.md#next-073) | New | 15-i18n.md |
| C-65 | What is AMP in Next.js? | [NEXT-074](../../../../docs/nextjs/12-scripts-seo.md#next-074) | New | 12-scripts-seo.md |
| C-66 | How do you enable AMP in Next.js? | [NEXT-074](../../../../docs/nextjs/12-scripts-seo.md#next-074) | Mapped (merged C-65) | — |
| C-67 | What is the `next/image` component used for? | [NEXT-030](../../../../docs/nextjs/07-seo-assets.md#next-030) | Mapped | — |
| C-68 | What is the `next/link` component used for? | [NEXT-023](../../../../docs/nextjs/05-navigation.md#next-023) | Mapped | — |
| C-69 | What is the difference between `pages` and `components` directories? | [NEXT-075](../../../../docs/nextjs/09-pages-router.md#next-075) | New | 09-pages-router.md |
| C-70 | How do you handle static files in Next.js? | [NEXT-039](../../../../docs/nextjs/10-config-tooling.md#next-039) | Mapped (merged C-15) | — |
| C-71 | List some common performance optimization techniques in Next.js | [NEXT-076](../../../../docs/nextjs/16-performance.md#next-076) | New | 16-performance.md |
| C-72 | Mention some common security practices in Next.js | [NEXT-072](../../../../docs/nextjs/13-security.md#next-072) | Mapped (merged C-63) | — |
| C-73 | Are there any limitations of Next.js? | [NEXT-077](../../../../docs/nextjs/16-performance.md#next-077) | New | 16-performance.md |
| C-74 | Is Next.js suitable for large-scale applications? | [NEXT-078](../../../../docs/nextjs/16-performance.md#next-078) | New | 16-performance.md |
| C-75 | How is Next.js a full-stack framework? | [NEXT-079](../../../../docs/nextjs/01-fundamentals.md#next-079) | New | 01-fundamentals.md |
| C-76 | How to prevent API routes from being accessed by the client? | [NEXT-080](../../../../docs/nextjs/13-security.md#next-080) | New | 13-security.md |
| C-77 | JWT Tokens in Next.js? | [NEXT-081](../../../../docs/nextjs/13-security.md#next-081) | New | 13-security.md |

---

## Section B — Pages Router (30 questions)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| P-01 | What is the Pages Router in Next.js? | [NEXT-003](../../../../docs/nextjs/01-fundamentals.md#next-003) | Mapped | — |
| P-02 | How do you create a route in the Pages Router? | [NEXT-082](../../../../docs/nextjs/09-pages-router.md#next-082) | New | 09-pages-router.md |
| P-03 | How do you create a dynamic route in the Pages Router? | [NEXT-006](../../../../docs/nextjs/01-fundamentals.md#next-006) | Mapped | — |
| P-04 | What is a catch-all segment in Next.js? | [NEXT-083](../../../../docs/nextjs/09-pages-router.md#next-083) | New | 09-pages-router.md |
| P-05 | What is the `_app.js` file in Next.js? | [NEXT-084](../../../../docs/nextjs/09-pages-router.md#next-084) | New | 09-pages-router.md |
| P-06 | What is the `_document.js` file in Next.js? | [NEXT-085](../../../../docs/nextjs/09-pages-router.md#next-085) | New | 09-pages-router.md |
| P-07 | What is the difference between `_app.js` and `_document.js`? | [NEXT-086](../../../../docs/nextjs/09-pages-router.md#next-086) | New | 09-pages-router.md |
| P-08 | What is the `_error.js` file in Next.js? | [NEXT-087](../../../../docs/nextjs/09-pages-router.md#next-087) | New | 09-pages-router.md |
| P-09 | How do you create a 404 page in the Pages Router? | [NEXT-088](../../../../docs/nextjs/09-pages-router.md#next-088) | New | 09-pages-router.md |
| P-10 | How do you fetch data in a Pages Router page? | [NEXT-089](../../../../docs/nextjs/09-pages-router.md#next-089) | New | 09-pages-router.md |
| P-11 | What is `getStaticProps`? | [NEXT-090](../../../../docs/nextjs/09-pages-router.md#next-090) | New | 09-pages-router.md |
| P-12 | What is `getServerSideProps`? | [NEXT-091](../../../../docs/nextjs/09-pages-router.md#next-091) | New | 09-pages-router.md |
| P-13 | What is the difference between `getStaticProps` and `getServerSideProps`? | [NEXT-092](../../../../docs/nextjs/09-pages-router.md#next-092) | New | 09-pages-router.md |
| P-14 | What is `getStaticPaths`? | [NEXT-093](../../../../docs/nextjs/09-pages-router.md#next-093) | New | 09-pages-router.md |
| P-15 | What is `fallback` in `getStaticPaths`? | [NEXT-094](../../../../docs/nextjs/09-pages-router.md#next-094) | New | 09-pages-router.md |
| P-16 | How do you handle API routes in the Pages Router? | [NEXT-095](../../../../docs/nextjs/09-pages-router.md#next-095) | New | 09-pages-router.md |
| P-17 | How do you handle custom error pages in the Pages Router? | [NEXT-087](../../../../docs/nextjs/09-pages-router.md#next-087) | Mapped (merged P-08) | — |
| P-18 | Are there any limitations of the Pages Router? | [NEXT-096](../../../../docs/nextjs/09-pages-router.md#next-096) | New | 09-pages-router.md |
| P-19 | How do you handle authentication in the Pages Router? | [NEXT-034](../../../../docs/nextjs/08-infra.md#next-034) | Mapped | — |
| P-20 | How do you handle middleware in the Pages Router? | [NEXT-032](../../../../docs/nextjs/08-infra.md#next-032) | Mapped | — |
| P-21 | How do you handle form submissions in the Pages Router? | [NEXT-097](../../../../docs/nextjs/09-pages-router.md#next-097) | New | 09-pages-router.md |
| P-22 | Are there performance optimizations in the Pages Router? | [NEXT-098](../../../../docs/nextjs/09-pages-router.md#next-098) | New | 09-pages-router.md |
| P-23 | How do you handle i18n in the Pages Router? | [NEXT-099](../../../../docs/nextjs/09-pages-router.md#next-099) | New | 09-pages-router.md |
| P-24 | How do you handle SEO in the Pages Router? | [NEXT-050](../../../../docs/nextjs/09-pages-router.md#next-050) | Mapped (merged C-32) | — |
| P-25 | How do you handle static assets in the Pages Router? | [NEXT-039](../../../../docs/nextjs/10-config-tooling.md#next-039) | Mapped (merged C-15) | — |
| P-26 | How does caching work in the Pages Router? | [NEXT-100](../../../../docs/nextjs/09-pages-router.md#next-100) | New | 09-pages-router.md |
| P-27 | Cache revalidation in the Pages Router? | [NEXT-100](../../../../docs/nextjs/09-pages-router.md#next-100) | Mapped (merged P-26) | — |
| P-28 | Optimizing images in the Pages Router? | [NEXT-030](../../../../docs/nextjs/07-seo-assets.md#next-030) | Mapped | — |
| P-29 | When to choose Pages Router over App Router? | [NEXT-101](../../../../docs/nextjs/09-pages-router.md#next-101) | New | 09-pages-router.md |
| P-30 | When to choose App Router over Pages Router? | [NEXT-101](../../../../docs/nextjs/09-pages-router.md#next-101) | Mapped (merged P-29) | — |

---

## Section C — App Router (43 questions + 6 bonus)

| # | Question (paraphrased) | Canonical ID | Status | Target File |
|---|---|---|---|---|
| A-01 | What is the App Router in Next.js? | [NEXT-003](../../../../docs/nextjs/01-fundamentals.md#next-003) | Mapped | — |
| A-02 | How do you create a route in the App Router? | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | Mapped | — |
| A-03 | How do you create a dynamic route in the App Router? | [NEXT-006](../../../../docs/nextjs/01-fundamentals.md#next-006) | Mapped | — |
| A-04 | How do you create custom error pages in the App Router? | [NEXT-009](../../../../docs/nextjs/01-fundamentals.md#next-009) | Mapped | — |
| A-05 | How do you handle form submissions in the App Router? | [NEXT-102](../../../../docs/nextjs/14-app-router-advanced.md#next-102) | New | 14-app-router-advanced.md |
| A-06 | How do you handle middleware in the App Router? | [NEXT-032](../../../../docs/nextjs/08-infra.md#next-032) | Mapped | — |
| A-07 | How do you implement authentication in the App Router? | [NEXT-034](../../../../docs/nextjs/08-infra.md#next-034) | Mapped | — |
| A-08 | How to add Auth.js in Next.js App Router? | [NEXT-103](../../../../docs/nextjs/14-app-router-advanced.md#next-103) | New | 14-app-router-advanced.md |
| A-09 | How do you handle authentication tokens in Next.js? | [NEXT-104](../../../../docs/nextjs/13-security.md#next-104) | New | 13-security.md |
| A-10 | How to add credentials provider in the App Router? | [NEXT-105](../../../../docs/nextjs/14-app-router-advanced.md#next-105) | New | 14-app-router-advanced.md |
| A-11 | What is `"use server"` in Next.js? | [NEXT-027](../../../../docs/nextjs/06-api-actions.md#next-027) | Mapped | — |
| A-12 | Difference between using and not using `"use server"` | [NEXT-106](../../../../docs/nextjs/14-app-router-advanced.md#next-106) | New | 14-app-router-advanced.md |
| A-13 | How do you handle API routes in the App Router? | [NEXT-026](../../../../docs/nextjs/06-api-actions.md#next-026) | Mapped | — |
| A-14 | How does middleware work in Next.js? | [NEXT-032](../../../../docs/nextjs/08-infra.md#next-032) | Mapped | — |
| A-15 | What is `formAction` in Next.js? | [NEXT-027](../../../../docs/nextjs/06-api-actions.md#next-027) | Mapped | — |
| A-16 | How do you handle file uploads in Next.js? | [NEXT-107](../../../../docs/nextjs/14-app-router-advanced.md#next-107) | New | 14-app-router-advanced.md |
| A-17 | Common use cases for the App Router | [NEXT-108](../../../../docs/nextjs/14-app-router-advanced.md#next-108) | New | 14-app-router-advanced.md |
| A-18 | One of the main differences between App Router and Pages Router? | [NEXT-003](../../../../docs/nextjs/01-fundamentals.md#next-003) | Mapped | — |
| A-19 | What is the use of the `"use client"` directive? | [NEXT-012](../../../../docs/nextjs/02-server-client.md#next-012) | Mapped | — |
| A-20 | Is it possible to use both App Router and Pages Router together? | [NEXT-109](../../../../docs/nextjs/14-app-router-advanced.md#next-109) | New | 14-app-router-advanced.md |
| A-21 | Are there any limitations of the App Router? | [NEXT-110](../../../../docs/nextjs/14-app-router-advanced.md#next-110) | New | 14-app-router-advanced.md |
| A-22 | Explain authorization in middleware and routes | [NEXT-111](../../../../docs/nextjs/13-security.md#next-111) | New | 13-security.md |
| A-23 | Difference between `"use server"` and `"use client"` | [NEXT-013](../../../../docs/nextjs/02-server-client.md#next-013) | Mapped | — |
| A-24 | Understand the concept of Server Actions in Next.js | [NEXT-027](../../../../docs/nextjs/06-api-actions.md#next-027) | Mapped | — |
| A-25 | What are the benefits of using Server Actions? | [NEXT-112](../../../../docs/nextjs/06-api-actions.md#next-112) | New | 06-api-actions.md |
| A-26 | What are the problems/drawbacks of Server Actions? | [NEXT-113](../../../../docs/nextjs/06-api-actions.md#next-113) | New | 06-api-actions.md |
| A-27 | Alternative options instead of Server Actions | [NEXT-114](../../../../docs/nextjs/06-api-actions.md#next-114) | New | 06-api-actions.md |
| A-28 | Alternative solutions example (not using server actions) | [NEXT-114](../../../../docs/nextjs/06-api-actions.md#next-114) | Mapped (merged A-27) | — |
| A-29 | JWT Token in Next.js App Router? | [NEXT-081](../../../../docs/nextjs/13-security.md#next-081) | Mapped (merged C-77) | — |
| A-30 | Context of JWT Token in Next.js App Router? | [NEXT-081](../../../../docs/nextjs/13-security.md#next-081) | Mapped (merged C-77) | — |
| A-31 | Is App Router better than Pages Router? | [NEXT-101](../../../../docs/nextjs/09-pages-router.md#next-101) | Mapped (merged P-29) | — |
| A-32 | How to handle global state management in the App Router? | [NEXT-115](../../../../docs/nextjs/14-app-router-advanced.md#next-115) | New | 14-app-router-advanced.md |
| A-33 | What is the fetch API in Next.js App Router? | [NEXT-019](../../../../docs/nextjs/04-data-fetching.md#next-019) | Mapped | — |
| A-34 | How do you create route groups in the App Router? | [NEXT-116](../../../../docs/nextjs/14-app-router-advanced.md#next-116) | New | 14-app-router-advanced.md |
| A-35 | What are parallel routes in Next.js App Router? | [NEXT-117](../../../../docs/nextjs/14-app-router-advanced.md#next-117) | New | 14-app-router-advanced.md |
| A-36 | How do you implement intercepting routes in the App Router? | [NEXT-118](../../../../docs/nextjs/14-app-router-advanced.md#next-118) | New | 14-app-router-advanced.md |
| A-37 | What is `loading.js` in the App Router? | [NEXT-008](../../../../docs/nextjs/01-fundamentals.md#next-008) | Mapped | — |
| A-38 | How do you handle not-found pages in the App Router? | [NEXT-010](../../../../docs/nextjs/01-fundamentals.md#next-010) | Mapped | — |
| A-39 | What is `template.js` in the App Router? | [NEXT-005](../../../../docs/nextjs/01-fundamentals.md#next-005) | Mapped | — |
| A-40 | How do you implement nested layouts in the App Router? | [NEXT-119](../../../../docs/nextjs/14-app-router-advanced.md#next-119) | New | 14-app-router-advanced.md |
| A-41 | Route handlers vs API routes in the App Router? | [NEXT-028](../../../../docs/nextjs/06-api-actions.md#next-028) | Mapped | — |
| A-42 | How do you handle streaming and Suspense in the App Router? | [NEXT-120](../../../../docs/nextjs/14-app-router-advanced.md#next-120) | New | 14-app-router-advanced.md |
| A-43 | What is React Server Components (RSC) in the App Router? | [NEXT-011](../../../../docs/nextjs/02-server-client.md#next-011) | Mapped | — |
| B-01 | How do you handle error boundaries in the App Router? | [NEXT-009](../../../../docs/nextjs/01-fundamentals.md#next-009) | Mapped | — |
| B-02 | How to differentiate server and client components? | [NEXT-014](../../../../docs/nextjs/02-server-client.md#next-014) | Mapped | — |
| B-03 | How to handle i18n in Next.js with the App Router? | [NEXT-121](../../../../docs/nextjs/15-i18n.md#next-121) | New | 15-i18n.md |
| B-04 | What is `"use server"`, why and when to use it? | [NEXT-027](../../../../docs/nextjs/06-api-actions.md#next-027) | Mapped | — |
| B-05 | Best practices for Next.js API routes | [NEXT-122](../../../../docs/nextjs/06-api-actions.md#next-122) | New | 06-api-actions.md |
| B-06 | How to validate and sanitize input data in API routes? | [NEXT-123](../../../../docs/nextjs/13-security.md#next-123) | New | 13-security.md |
| B-07 | How to use proper HTTP methods in API routes? | [NEXT-124](../../../../docs/nextjs/06-api-actions.md#next-124) | New | 06-api-actions.md |
| B-08 | How to handle errors in Next.js API routes? | [NEXT-125](../../../../docs/nextjs/06-api-actions.md#next-125) | New | 06-api-actions.md |
| B-09 | How to use middleware in Next.js API routes? | [NEXT-126](../../../../docs/nextjs/06-api-actions.md#next-126) | New | 06-api-actions.md |
| B-10 | How to keep Next.js API routes modular and organized? | [NEXT-127](../../../../docs/nextjs/06-api-actions.md#next-127) | New | 06-api-actions.md |
| B-11 | RTK Query with Next.js App Router? | [NEXT-128](../../../../docs/nextjs/16-integrations.md#next-128) | New | 16-integrations.md |
| B-12 | Redux Toolkit with Next.js App Router? | [NEXT-129](../../../../docs/nextjs/16-integrations.md#next-129) | New | 16-integrations.md |
| B-13 | How do you read and set cookies in the App Router? | [NEXT-130](../../../../docs/nextjs/13-security.md#next-130) | New | 13-security.md |
| B-14 | How can you stream responses from a route handler? | [NEXT-131](../../../../docs/nextjs/14-app-router-advanced.md#next-131) | New | 14-app-router-advanced.md |
| B-15 | How do you test App Router components and pages? | [NEXT-132](../../../../docs/nextjs/17-testing.md#next-132) | New | 17-testing.md |
| B-16 | How to implement optimistic UI with the App Router? | [NEXT-133](../../../../docs/nextjs/14-app-router-advanced.md#next-133) | New | 14-app-router-advanced.md |
| B-17 | How to choose between Edge and Node runtimes for route handlers? | [NEXT-134](../../../../docs/nextjs/08-infra.md#next-134) | New | 08-infra.md |

---

## ID Range Summary

| File | New IDs | Count |
|---|---|---|
| [docs/nextjs/01-fundamentals.md](../../../../docs/nextjs/01-fundamentals.md) | NEXT-036, NEXT-079 | 2 |
| [docs/nextjs/03-rendering.md](../../../../docs/nextjs/03-rendering.md) | NEXT-048, NEXT-058 | 2 |
| [docs/nextjs/05-navigation.md](../../../../docs/nextjs/05-navigation.md) | NEXT-037, NEXT-061 | 2 |
| [docs/nextjs/06-api-actions.md](../../../../docs/nextjs/06-api-actions.md) | NEXT-112–114, NEXT-122–127 | 9 |
| [docs/nextjs/08-infra.md](../../../../docs/nextjs/08-infra.md) | NEXT-134 | 1 |
| [docs/nextjs/09-pages-router.md](../../../../docs/nextjs/09-pages-router.md) | NEXT-050, NEXT-075, NEXT-082–101 | 22 |
| [docs/nextjs/10-config-tooling.md](../../../../docs/nextjs/10-config-tooling.md) | NEXT-038–049, NEXT-051–058, NEXT-060, NEXT-063 | 22 |
| [docs/nextjs/11-styling.md](../../../../docs/nextjs/11-styling.md) | NEXT-045–047 | 3 |
| [docs/nextjs/12-scripts-seo.md](../../../../docs/nextjs/12-scripts-seo.md) | NEXT-062, NEXT-065, NEXT-068–069, NEXT-074 | 5 |
| [docs/nextjs/13-security.md](../../../../docs/nextjs/13-security.md) | NEXT-070–072, NEXT-076, NEXT-080–081, NEXT-104, NEXT-111, NEXT-123, NEXT-130 | 10 |
| [docs/nextjs/14-app-router-advanced.md](../../../../docs/nextjs/14-app-router-advanced.md) | NEXT-040, NEXT-067, NEXT-102–103, NEXT-105–110, NEXT-115–120, NEXT-131, NEXT-133 | 17 |
| [docs/nextjs/15-i18n.md](../../../../docs/nextjs/15-i18n.md) | NEXT-059, NEXT-066, NEXT-073, NEXT-099, NEXT-121 | 5 |
| [docs/nextjs/16-integrations.md](../../../../docs/nextjs/16-integrations.md) | NEXT-064, NEXT-128–129 | 3 |
| [docs/nextjs/16-performance.md](../../../../docs/nextjs/16-performance.md) | NEXT-076–078 | 3 |
| [docs/nextjs/17-testing.md](../../../../docs/nextjs/17-testing.md) | NEXT-132 | 1 |
| **Total** | **NEXT-036 → NEXT-134** | **106** |

---

*← [GFE-NJS question-map](../../website/greatfrontend/question-map.md) | [Source Library →](../../../../docs/reference/02-source-library.md)*
