# Source Archive — Next.js Interview Questions (English, YouTube)

> **Source:** https://www.youtube.com/watch?v=BYTh9UAVHdY
> **Technology:** Next.js
> **Source type:** YouTube
> **Format:** Three categories — Freshers (Q1–15), Experienced (Q16–29), Most Frequently Asked (Q30–39)
> **Source ID:** YTE-NJS

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | 27 |
| New canonical created | 9 |
| Source-only | 0 |
| **Total** | **39** |

> 27 questions map to existing canonicals. 9 new entries: NEXT-136–NEXT-144.

---

## Freshers Questions (Q1–15)

| YTE # | Question | Timestamp | Canonical ID | Status |
|---|---|---|---|---|
| 1 | Most popular website types for Next.js (desktop, static, SSR apps, SEO-friendly, PWAs) | [00:01:55](https://youtu.be/BYTh9UAVHdY?t=115) | [NEXT-108](../../../../docs/nextjs/14-app-router-advanced.md#next-108) | Mapped |
| 2 | Can you use Next.js with Redux? | [00:02:11](https://youtu.be/BYTh9UAVHdY?t=131) | [NEXT-129](../../../../docs/nextjs/16-integrations.md#next-129) | Mapped |
| 3 | How to create a custom error page in Next.js (`error.js` in pages folder, import custom vs next/error) | [00:02:29](https://youtu.be/BYTh9UAVHdY?t=149) | [NEXT-009](../../../../docs/nextjs/01-fundamentals.md#next-009) | Mapped |
| 4 | Benefits of implementing a serverless model (splits app into lambdas, scalability, pay-per-use) | [00:03:02](https://youtu.be/BYTh9UAVHdY?t=182) | [NEXT-035](../../../../docs/nextjs/08-infra.md#next-035) | Mapped |
| 5 | What is SSR? (server-side rendering, faster initial load) | [00:03:25](https://youtu.be/BYTh9UAVHdY?t=205) | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped |
| 6 | How to implement serverless mode (`target: 'serverless'` in next.config.js) | [00:03:35](https://youtu.be/BYTh9UAVHdY?t=215) | [NEXT-035](../../../../docs/nextjs/08-infra.md#next-035) | Mapped (merged) |
| 7 | What is styled-jsx in Next.js? (CSS-in-JS for scoped/encapsulated styles, no class conflicts) | [00:03:53](https://youtu.be/BYTh9UAVHdY?t=233) | [NEXT-136](../../../../docs/nextjs/11-styling.md#next-136) | New |
| 8 | What is automatic code splitting? (each page bundles only its imports, no unnecessary code) | [00:04:14](https://youtu.be/BYTh9UAVHdY?t=254) | [NEXT-076](../../../../docs/nextjs/16-performance.md#next-076) | Mapped |
| 9 | Is Next.js backend, frontend, or full stack? (full stack — client + server + API routes) | [00:04:46](https://youtu.be/BYTh9UAVHdY?t=286) | [NEXT-079](../../../../docs/nextjs/01-fundamentals.md#next-079) | Mapped |
| 10 | What is the DOM? (Document Object Model, tree structure, bridges code and UI) | [00:05:01](https://youtu.be/BYTh9UAVHdY?t=301) | [REACT-003](../../../../docs/react/01-fundamentals.md#react-003) | Mapped (React) |
| 11 | Imperative vs declarative programming (specify steps vs describe end result; React is declarative) | [00:05:21](https://youtu.be/BYTh9UAVHdY?t=321) | [REACT-001](../../../../docs/react/01-fundamentals.md#react-001) | Mapped (React) |
| 12 | Types of pre-rendering in Next.js (SSR and static generation) | [00:05:41](https://youtu.be/BYTh9UAVHdY?t=341) | [NEXT-048](../../../../docs/nextjs/03-rendering.md#next-048) | Mapped |
| 13 | Difference between pre-rendering types (static = build time; SSR = each request, uses getServerSideProps) | [00:05:55](https://youtu.be/BYTh9UAVHdY?t=355) | [NEXT-015](../../../../docs/nextjs/03-rendering.md#next-015) | Mapped |
| 14 | Default pre-render in Next.js (static generation by default when no data fetching) | [00:06:26](https://youtu.be/BYTh9UAVHdY?t=386) | [NEXT-016](../../../../docs/nextjs/03-rendering.md#next-016) | Mapped |
| 15 | Main scripts in Next.js package.json (dev, build, start, lint) | [00:06:59](https://youtu.be/BYTh9UAVHdY?t=419) | [NEXT-137](../../../../docs/nextjs/10-config-tooling.md#next-137) | New |

---

## Experienced Questions (Q16–29)

| YTE # | Question | Timestamp | Canonical ID | Status |
|---|---|---|---|---|
| 16 | Which method does Next.js recommend for fetching data? (`getInitialProps`, async, retrieves data from anywhere) | [00:07:36](https://youtu.be/BYTh9UAVHdY?t=456) | [NEXT-089](../../../../docs/nextjs/09-pages-router.md#next-089) | Mapped |
| 17 | Properties in context object with `getInitialProps` (pathname, asPath, query, req, res, err) | [00:08:09](https://youtu.be/BYTh9UAVHdY?t=489) | [NEXT-138](../../../../docs/nextjs/09-pages-router.md#next-138) | New |
| 18 | Difference between Next.js and Create React App (CRA = boilerplate, no SSR/routing built-in; Next.js = full stack, inbuilt routing/SSR/API) | [00:08:43](https://youtu.be/BYTh9UAVHdY?t=523) | [NEXT-139](../../../../docs/nextjs/01-fundamentals.md#next-139) | New |
| 19 | Features introduced in Next.js 12.1 (Rust-based compiler, faster image opt, on-demand ISR beta, React 18 support) | [00:09:50](https://youtu.be/BYTh9UAVHdY?t=590) | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped |
| 20 | SEO features of Next.js (Jamstack compatibility, automatic static optimization, fast static sites, responsiveness) | [00:10:27](https://youtu.be/BYTh9UAVHdY?t=627) | [NEXT-029](../../../../docs/nextjs/07-seo-assets.md#next-029) | Mapped |
| 21 | How to configure build ID in Next.js (`generateBuildId` async function in next.config.js) | [00:11:03](https://youtu.be/BYTh9UAVHdY?t=663) | [NEXT-140](../../../../docs/nextjs/10-config-tooling.md#next-140) | New |
| 22 | Importance of code splitting (splits into bundles loaded on demand, controls resource prioritization, reduces load time) | [00:11:40](https://youtu.be/BYTh9UAVHdY?t=700) | [NEXT-076](../../../../docs/nextjs/16-performance.md#next-076) | Mapped |
| 23 | Three main ways to split code (entry points, prevent duplication via SplitChunks, dynamic imports) | [00:12:13](https://youtu.be/BYTh9UAVHdY?t=733) | [NEXT-040](../../../../docs/nextjs/14-app-router-advanced.md#next-040) | Mapped |
| 24 | How to create a page directory inside a project (pages/index.js, function HomePage, export default) | [00:12:45](https://youtu.be/BYTh9UAVHdY?t=765) | [NEXT-036](../../../../docs/nextjs/01-fundamentals.md#next-036) | Mapped |
| 25 | AMP-first pages method to enable AMP in Next.js (`import { withAmp } from 'next/amp'`, export with config) | [00:13:19](https://youtu.be/BYTh9UAVHdY?t=799) | [NEXT-074](../../../../docs/nextjs/12-scripts-seo.md#next-074) | Mapped |
| 26 | Hybrid AMP pages in Next.js (coexisting AMP + regular page, search engines show AMP on mobile) | [00:13:55](https://youtu.be/BYTh9UAVHdY?t=835) | [NEXT-074](../../../../docs/nextjs/12-scripts-seo.md#next-074) | Mapped (merged) |
| 27 | CDN setup in Next.js (`assetPrefix` setting, configure CDN origin to resolve Next.js domain, `isProd` guard) | [00:14:29](https://youtu.be/BYTh9UAVHdY?t=869) | [NEXT-141](../../../../docs/nextjs/10-config-tooling.md#next-141) | New |
| 28 | CDN on separate domain (`crossOrigin: 'anonymous'` in next.config.js exports) | [00:15:04](https://youtu.be/BYTh9UAVHdY?t=904) | [NEXT-141](../../../../docs/nextjs/10-config-tooling.md#next-141) | Mapped (merged) |
| 29 | Can Next.js be hosted in nginx? (requires Node.js app server, nginx as reverse proxy only) | [00:15:36](https://youtu.be/BYTh9UAVHdY?t=936) | [NEXT-142](../../../../docs/nextjs/08-infra.md#next-142) | New |

---

## Most Frequently Asked (Q30–39)

| YTE # | Question | Timestamp | Canonical ID | Status |
|---|---|---|---|---|
| 30 | Major companies using Next.js (TikTok, Nike, Netflix, GitHub Copilot, Target, Twitch, Hulu) | [00:15:50](https://youtu.be/BYTh9UAVHdY?t=950) | [NEXT-078](../../../../docs/nextjs/16-performance.md#next-078) | Mapped |
| 31 | Competitors and alternatives to Next.js (Gatsby, CRA, Hexo, Angular Universal, React Router) | [00:16:10](https://youtu.be/BYTh9UAVHdY?t=970) | [NEXT-143](../../../../docs/nextjs/01-fundamentals.md#next-143) | New |
| 32 | Important features of Next.js (static export, server rendering, code splitting, Babel/webpack control, HMR) | [00:16:31](https://youtu.be/BYTh9UAVHdY?t=991) | [NEXT-001](../../../../docs/nextjs/01-fundamentals.md#next-001) | Mapped |
| 33 | Digital products that can be built with Next.js (Jamstack, SPAs, MVP, SaaS, dashboards, e-commerce) | [00:17:05](https://youtu.be/BYTh9UAVHdY?t=1025) | [NEXT-108](../../../../docs/nextjs/14-app-router-advanced.md#next-108) | Mapped |
| 34 | In which language is Next.js written? (JavaScript, Rust, TypeScript, React) | [00:17:33](https://youtu.be/BYTh9UAVHdY?t=1053) | [NEXT-144](../../../../docs/nextjs/01-fundamentals.md#next-144) | New |
| 35 | Why is there a built-in router in Next.js? (file-system based, no config needed, shadow routing, lazy loadable) | [00:17:52](https://youtu.be/BYTh9UAVHdY?t=1072) | [NEXT-004](../../../../docs/nextjs/01-fundamentals.md#next-004) | Mapped |
| 36 | How to fetch data in Next.js (getServerSideProps, SWR/useEffect CSR, getStaticProps SSR, revalidate ISR) | [00:18:22](https://youtu.be/BYTh9UAVHdY?t=1102) | [NEXT-019](../../../../docs/nextjs/04-data-fetching.md#next-019) | Mapped |
| 37 | Requirements for building a React web app from scratch (bundler like Webpack, compiler like Babel, code splitting, SSR, server-side code for data store) | [00:18:52](https://youtu.be/BYTh9UAVHdY?t=1132) | [NEXT-002](../../../../docs/nextjs/01-fundamentals.md#next-002) | Mapped |
| 38 | Why is Next.js preferred by major companies? (no setup, production-ready, SSR enabled) | [00:18:58](https://youtu.be/BYTh9UAVHdY?t=1138) | [NEXT-078](../../../../docs/nextjs/16-performance.md#next-078) | Mapped |
| 39 | How to install Next.js (Node.js prerequisite, mkdir, npm init -y, npm install react react-dom next, update package.json scripts) | [00:19:30](https://youtu.be/BYTh9UAVHdY?t=1170) | [NEXT-036](../../../../docs/nextjs/01-fundamentals.md#next-036) | Mapped |

---

*Source folder: `sources/nextjs/youtube/nextjs-interview-english/`*
