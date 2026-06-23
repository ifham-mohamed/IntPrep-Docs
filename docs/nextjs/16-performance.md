# Next.js Performance & Scalability

This file covers performance optimization, limitations, and large-scale architectural considerations for Next.js.

---

## NEXT-076

### What are common performance optimization techniques in Next.js?

**1. Image optimization** — use `next/image` for automatic WebP conversion, lazy loading, and sizing.

**2. Font optimization** — use `next/font` to self-host fonts and eliminate layout shift.

**3. Script loading** — use `next/script` with `strategy="afterInteractive"` or `"lazyOnload"` for third-party scripts.

**4. Static rendering** — keep pages static whenever possible (no `cookies()`, `headers()`, or uncached fetches). Static pages are CDN-cached globally.

**5. Streaming** — wrap slow data with `<Suspense>` to stream content progressively instead of blocking the entire page.

**6. Code splitting** — `next/dynamic` for large components (maps, editors, chart libraries) loaded on demand.

**7. Caching** — use `fetch` with `next: { revalidate: N }` or `unstable_cache()` to memoize expensive computations and DB queries.

**8. Bundle analysis** — run `ANALYZE=true next build` (with `@next/bundle-analyzer`) to identify bloated dependencies.

**9. React Server Components** — move data-heavy components to RSC to remove their JS from the client bundle entirely.

**10. Middleware at the edge** — route decisions made at Vercel/Cloudflare edge nodes run in <1ms without cold starts.

**Related:** [NEXT-030 — next/image](./07-seo-assets.md#next-030) | [NEXT-031 — next/font](./07-seo-assets.md#next-031) | [NEXT-021 — Caching](./04-data-fetching.md#next-021)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-71](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-077

### What are the limitations of Next.js?

Next.js is powerful but has trade-offs worth knowing:

- **Vendor lock-in risk** — advanced features like ISR, Edge Middleware, and Image Optimization work best (or only) on Vercel; self-hosting requires extra configuration.
- **Complex caching model** — the App Router has multiple cache layers (Request Memoization, Data Cache, Full Route Cache, Router Cache) that are easy to misconfigure and hard to debug.
- **Cold starts** — Serverless deployment means the first request after inactivity can be slow; Vercel's Edge runtime mitigates this but limits Node.js APIs.
- **Learning curve** — App Router's Server Component mental model, streaming, and "use client"/"use server" directives take significant time to understand.
- **Third-party library compatibility** — libraries built for CSR environments often need `dynamic({ ssr: false })` or `"use client"` wrappers.
- **No native WebSockets** — long-lived real-time connections require a separate server (Pusher, Ably, custom Socket.io server).
- **Build times** — large projects can have slow `next build`; Turbopack (now stable) significantly improves dev rebuild times but production builds still use Webpack.

**Related:** [NEXT-110 — App Router limitations](./14-app-router-advanced.md#next-110) | [NEXT-076 — Performance optimizations](./16-performance.md#next-076)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-73](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-078

### Is Next.js suitable for large-scale applications?

Yes — with the right architecture, Next.js scales well. Many large production apps (Vercel, Loom, Notion, The Washington Post) run on Next.js. Key considerations:

**What scales well:**
- Static and ISR pages serve from CDN globally with no server overhead.
- App Router's granular caching reduces repeated DB queries.
- Server Components move rendering work to the server, sending smaller HTML + less JS.
- Route handlers can be deployed as Edge Functions for low-latency global APIs.

**What requires care at scale:**
- **Monorepo structure** — use a monorepo (Turborepo, Nx) to share types and components across multiple Next.js apps.
- **Database connections** — serverless deployments open a new DB connection per invocation; use a connection pooler (PgBouncer, Prisma Accelerate, Neon).
- **Cache invalidation** — `revalidatePath()` / `revalidateTag()` at scale can trigger many re-renders; design tag granularity carefully.
- **Bundle size discipline** — enforce barrel-file avoidance and use `@next/bundle-analyzer` to prevent bundle creep.
- **Testing** — invest in component testing (Playwright, Vitest) early; Server Component testing patterns are still maturing.

The App Router's Server Components model is specifically designed for large-scale data-heavy applications where shipping less JavaScript is a priority.

**Related:** [NEXT-077 — Next.js limitations](./16-performance.md#next-077) | [NEXT-021 — Caching](./04-data-fetching.md#next-021)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-74](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [15-i18n.md](./15-i18n.md) | [17-testing.md →](./17-testing.md)*
