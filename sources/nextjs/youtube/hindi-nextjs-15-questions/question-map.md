# Source Archive — Hindi: 15 Next.js Interview Questions & Answers (YouTube)

> **Source:** https://www.youtube.com/watch?v=Qyw1Q8BqGmM
> **Channel:** (Hindi tech channel)
> **Technology:** Next.js
> **Source type:** YouTube
> **Language:** Hindi
> **Format:** 15 Next.js interview questions with detailed answers, diagrams for CSR/SSR/RSC timing
> **Source ID:** YTH-NJS

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | 14 |
| New canonical created | 1 |
| Source-only | 0 |
| **Total** | **15** |

> 14 questions map to existing NEXT-001–NEXT-093 entries. 1 new entry: NEXT-135 (SSR vs React Server Components — performance comparison with timing diagram).

---

## Question Map

| YTH # | Question | Timestamp | Canonical ID | Status |
|---|---|---|---|---|
| 1 | What is Next.js? (React framework for the web, built on React library) | [00:00:26](https://youtu.be/Qyw1Q8BqGmM?t=26) | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped |
| 2 | What is Server Side Rendering (SSR)? (server creates ready HTML, sends to browser, JS bundle hydrates) | [00:01:00](https://youtu.be/Qyw1Q8BqGmM?t=60) | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped |
| 3 | Benefits of using Next.js (hybrid rendering, image opt, font opt, built-in routing, API endpoints, i18n) | [00:02:31](https://youtu.be/Qyw1Q8BqGmM?t=151) | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped |
| 4 | How is the App Router different from Pages Router? (no getServerSideProps/getStaticProps, RSC by default, layout.tsx, streaming) | [00:06:29](https://youtu.be/Qyw1Q8BqGmM?t=389) | [NEXT-003](../../../../docs/nextjs/01-fundamentals.md#next-003) | Mapped |
| 5 | Disadvantages of Next.js (learning curve, hydration issues, recurring security vulnerabilities) | [00:10:00](https://youtu.be/Qyw1Q8BqGmM?t=600) | [NEXT-077](../../../../docs/nextjs/16-performance.md#next-077) | Mapped |
| 6 | What is Static Site Generation (SSG)? (`generateStaticParams`, build-time generation, `revalidate` for ISR) | [00:12:15](https://youtu.be/Qyw1Q8BqGmM?t=735) | [NEXT-020](../../../../docs/nextjs/04-data-fetching.md#next-020) | Mapped |
| 7 | How to implement dynamic routing in Next.js? (`[param]` folder syntax, bracket notation) | [00:15:13](https://youtu.be/Qyw1Q8BqGmM?t=913) | [NEXT-006](../../../../docs/nextjs/01-fundamentals.md#next-006) | Mapped |
| 8 | What are getStaticProps, getServerSideProps, getStaticPaths? (Pages Router data fetching functions) | [00:15:36](https://youtu.be/Qyw1Q8BqGmM?t=936) | [NEXT-090](../../../../docs/nextjs/09-pages-router.md#next-090) | Mapped |
| 9 | Image optimization in Next.js (`next/image`, WebP conversion, lazy loading, CDN, responsive sizing) | [00:17:32](https://youtu.be/Qyw1Q8BqGmM?t=1052) | [NEXT-030](../../../../docs/nextjs/07-seo-assets.md#next-030) | Mapped |
| 10 | How to write a 404 page in Next.js (`not-found.tsx` in App Router, `404.tsx` in Pages Router) | [00:19:00](https://youtu.be/Qyw1Q8BqGmM?t=1140) | [NEXT-010](../../../../docs/nextjs/01-fundamentals.md#next-010) | Mapped |
| 11 | What is Incremental Static Regeneration (ISR)? (`revalidate` seconds, background re-generation without full rebuild) | [00:19:36](https://youtu.be/Qyw1Q8BqGmM?t=1176) | [NEXT-020](../../../../docs/nextjs/04-data-fetching.md#next-020) | Mapped (merged) |
| 12 | Common performance optimization techniques in Next.js (image opt, SSG/ISR, dynamic imports, lazy load, prefetch with Link) | [00:21:07](https://youtu.be/Qyw1Q8BqGmM?t=1267) | [NEXT-076](../../../../docs/nextjs/16-performance.md#next-076) | Mapped |
| 13 | What is hydration? Hydration issues in Next.js (mismatch HTML, slow hydration, over-hydration, useEffect failures) | [00:22:59](https://youtu.be/Qyw1Q8BqGmM?t=1379) | [NEXT-017](../../../../docs/nextjs/03-rendering.md#next-017) | Mapped |
| 14 | Difference between CSR and SSR (blank page vs loading state, FCP timing, LCP timing comparison) | [00:26:21](https://youtu.be/Qyw1Q8BqGmM?t=1581) | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped |
| 15 | Difference between SSR (Pages Router) and React Server Components (App Router) — JS bundle size, TTI comparison | [00:25:24](https://youtu.be/Qyw1Q8BqGmM?t=1524) | [NEXT-135](../../../../docs/nextjs/03-rendering.md#next-135) | New |

---

## Notable Teaching Points

- **Timing diagram** (00:27:47): CSR → blank page → loading → FCP late → LCP late; Pages Router SSR → loading immediate → FCP early → LCP+TTI late (full JS bundle); App Router RSC → loading immediate → FCP early → smaller client JS → TTI earlier
- **ISR clarification** (00:20:05): ISR ≠ full rebuild — only the specific page segment is regenerated in the background
- **Locale routing** (00:04:55): `next.config.js` i18n config + `[locale]` folder handles multi-language without duplicating page files

---

*Source folder: `sources/nextjs/youtube/hindi-nextjs-15-questions/`*
