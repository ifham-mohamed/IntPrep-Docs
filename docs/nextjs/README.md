# Next.js Knowledge Base

> **Status:** Active — 144 canonical entries, NEXT-001 to NEXT-144.
> **Sources:** GFE-NJS (35 entries), MRH-NJS (150 questions mapped, 99 new entries), YTH-NJS (15 questions, 1 new entry), YTE-NJS (39 questions, 9 new entries)

---

## Quick Navigation

| File | IDs | Topics |
|---|---|---|
| [01-fundamentals.md](./01-fundamentals.md) | NEXT-001–010, 036, 079, 139, 143, 144 | What is Next.js, App Router vs Pages Router, file-based routing, dynamic routes, loading/error/not-found, create project, full-stack, Next.js vs CRA, alternatives, language |
| [02-server-client.md](./02-server-client.md) | NEXT-011–014 | Server Components, Client Components, `"use client"` guidance, composition boundary rules |
| [03-rendering.md](./03-rendering.md) | NEXT-015–018, 048, 135 | SSR, SSG, CSR, ISR, static vs dynamic rendering, hydration, hydration errors, pre-rendering, SSR vs RSC comparison |
| [04-data-fetching.md](./04-data-fetching.md) | NEXT-019–022 | `async/await` in Server Components, ISR, caching layers, `"use cache"` / `cacheLife` / `cacheTag` |
| [05-navigation.md](./05-navigation.md) | NEXT-023–025, 037, 061 | `next/link`, `useRouter`, `redirect()`, push vs replace, singleton router |
| [06-api-actions.md](./06-api-actions.md) | NEXT-026–028, 112–114, 122, 124–127 | Route Handlers, Server Actions, Server Action benefits/drawbacks, alternatives, API best practices |
| [07-seo-assets.md](./07-seo-assets.md) | NEXT-029–031 | `generateMetadata`, `next/image`, `next/font` |
| [08-infra.md](./08-infra.md) | NEXT-032–035, 134, 142 | Middleware, environment variables, authentication, deployment, Edge vs Node runtime, nginx hosting |
| [09-pages-router.md](./09-pages-router.md) | NEXT-050, 075, 082–101, 138 | Pages Router specifics, `_app`, `_document`, getStaticProps, getServerSideProps, getStaticPaths, context object |
| [10-config-tooling.md](./10-config-tooling.md) | NEXT-038–039, 041–044, 049, 051–058, 060, 063, 137, 140, 141 | TypeScript, public folder, port, Fast Refresh, next.config.js, redirects, headers, Webpack, polyfills, scripts, build ID, CDN |
| [11-styling.md](./11-styling.md) | NEXT-045–047, 136 | CSS Modules, global CSS, Tailwind CSS, styled-jsx |
| [12-scripts-seo.md](./12-scripts-seo.md) | NEXT-062, 065, 068–069, 074 | `next/script` strategies, next-seo, Google Analytics, sitemaps, AMP |
| [13-security.md](./13-security.md) | NEXT-070–072, 080–081, 104, 111, 123, 130 | CORS, cookies, security practices, JWT, auth tokens, authorization, input validation |
| [14-app-router-advanced.md](./14-app-router-advanced.md) | NEXT-040, 067, 102–103, 105–110, 115–120, 131, 133 | next/dynamic, ssr:false, form handling, Auth.js, route groups, parallel/intercepting routes, streaming, optimistic UI |
| [15-i18n.md](./15-i18n.md) | NEXT-059, 066, 073, 121 | i18n routing, next-i18next, useTranslation, App Router i18n with next-intl |
| [16-integrations.md](./16-integrations.md) | NEXT-064, 128–129 | GraphQL, RTK Query, Redux Toolkit with App Router |
| [16-performance.md](./16-performance.md) | NEXT-076–078 | Performance optimization techniques, limitations, large-scale suitability |
| [17-testing.md](./17-testing.md) | NEXT-132 | Testing Server Components, Client Components, Route Handlers, Playwright E2E |

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
| NEXT-011 | What are Server Components in Next.js and what can't they do? | 🟢 | GFE-NJS |
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
| NEXT-022 | What are the new Cache Component APIs ("use cache", cacheLife, cacheTag)? | 🔴 | GFE-NJS |
| NEXT-023 | What is next/link used for and when should you use a plain `<a>` tag? | 🟢 | GFE-NJS |
| NEXT-024 | What is useRouter() used for in the App Router? | 🟡 | GFE-NJS |
| NEXT-025 | What is the difference between redirect() and client-side navigation? | 🟡 | GFE-NJS |
| NEXT-026 | What are Route Handlers in the App Router? | 🟢 | GFE-NJS |
| NEXT-027 | What are Server Functions and Server Actions? | 🟡 | GFE-NJS |
| NEXT-028 | What is the difference between a Route Handler and a Server Action? | 🟡 | GFE-NJS |
| NEXT-029 | How do you add metadata for SEO in the App Router? | 🟢 | GFE-NJS |
| NEXT-030 | What is next/image and why does it matter for performance? | 🟡 | GFE-NJS |
| NEXT-031 | What is next/font and why use it over a manual font link tag? | 🟡 | GFE-NJS |
| NEXT-032 | What is Middleware in Next.js and what is it used for? | 🟡 | GFE-NJS |
| NEXT-033 | How do environment variables work in Next.js and what is NEXT_PUBLIC_? | 🟢 | GFE-NJS |
| NEXT-034 | How do you handle authentication in a Next.js app? | 🟡 | GFE-NJS |
| NEXT-035 | How do you deploy a Next.js app and what features require a runtime? | 🟢 | GFE-NJS |
| NEXT-036 | How do you create a new Next.js project? | 🟢 | MRH-NJS |
| NEXT-037 | What is the difference between push and replace in useRouter? | 🟢 | MRH-NJS |
| NEXT-038 | How do you enable TypeScript in a Next.js project? | 🟢 | MRH-NJS |
| NEXT-039 | What is the public folder in Next.js? | 🟢 | MRH-NJS |
| NEXT-040 | What is next/dynamic and when do you use it? | 🟡 | MRH-NJS |
| NEXT-041 | What is the default port for a Next.js app? | 🟢 | MRH-NJS |
| NEXT-042 | How do you change the default port for a Next.js app? | 🟢 | MRH-NJS |
| NEXT-043 | What is Fast Refresh in Next.js? | 🟢 | MRH-NJS |
| NEXT-044 | What is next.config.js? | 🟢 | MRH-NJS |
| NEXT-045 | How do you add component-level CSS in Next.js? | 🟢 | MRH-NJS |
| NEXT-046 | How do you add global CSS in Next.js? | 🟢 | MRH-NJS |
| NEXT-047 | How do you use Tailwind CSS in Next.js? | 🟢 | MRH-NJS |
| NEXT-048 | What is pre-rendering in Next.js? | 🟡 | MRH-NJS |
| NEXT-049 | How do you handle redirects in Next.js? | 🟢 | MRH-NJS |
| NEXT-050 | What is the Head component (next/head) in Next.js? | 🟢 | MRH-NJS |
| NEXT-051 | How do you add custom headers in Next.js? | 🟡 | MRH-NJS |
| NEXT-052 | What is the use of the next export command? | 🟡 | MRH-NJS |
| NEXT-053 | How do you configure Webpack in Next.js? | 🟡 | MRH-NJS |
| NEXT-054 | How do you configure a custom Babel setup in Next.js? | 🟡 | MRH-NJS |
| NEXT-055 | What is the purpose of next-env.d.ts? | 🟢 | MRH-NJS |
| NEXT-056 | What is the purpose of next-compose-plugins? | 🟡 | MRH-NJS |
| NEXT-057 | How do you add polyfills in Next.js? | 🟡 | MRH-NJS |
| NEXT-058 | What is static optimization in Next.js? | 🟡 | MRH-NJS |
| NEXT-059 | How do you handle internationalization (i18n) in Next.js? | 🟡 | MRH-NJS |
| NEXT-060 | What is React Strict Mode in Next.js? | 🟢 | MRH-NJS |
| NEXT-061 | What is a singleton router in Next.js? | 🟡 | MRH-NJS |
| NEXT-062 | What is next/script used for? | 🟡 | MRH-NJS |
| NEXT-063 | What is a custom server in Next.js? | 🔴 | MRH-NJS |
| NEXT-064 | How do you set up GraphQL in Next.js? | 🟡 | MRH-NJS |
| NEXT-065 | What is the use of next-seo? | 🟡 | MRH-NJS |
| NEXT-066 | How do you configure next-i18next in Next.js? | 🟡 | MRH-NJS |
| NEXT-067 | What does ssr:false mean in next/dynamic? | 🟡 | MRH-NJS |
| NEXT-068 | How do you add Google Analytics to a Next.js project? | 🟡 | MRH-NJS |
| NEXT-069 | How do you add a sitemap in a Next.js app? | 🟡 | MRH-NJS |
| NEXT-070 | How do you handle CORS in Next.js Route Handlers? | 🟡 | MRH-NJS |
| NEXT-071 | How do you manage cookies in Next.js? | 🟡 | MRH-NJS |
| NEXT-072 | How do you secure a Next.js App Router application? | 🟡 | MRH-NJS |
| NEXT-073 | What is the useTranslation hook in Next.js? | 🟢 | MRH-NJS |
| NEXT-074 | What is AMP in Next.js and how do you enable it? | 🟡 | MRH-NJS |
| NEXT-075 | What is the difference between pages and components directories? | 🟢 | MRH-NJS |
| NEXT-076 | Common performance optimization techniques in Next.js | 🟡 | MRH-NJS |
| NEXT-077 | What are the limitations of Next.js? | 🟡 | MRH-NJS |
| NEXT-078 | Is Next.js suitable for large-scale applications? | 🟡 | MRH-NJS |
| NEXT-079 | How is Next.js a full-stack framework? | 🟢 | MRH-NJS |
| NEXT-080 | How do you prevent API routes from being accessed by the client? | 🟡 | MRH-NJS |
| NEXT-081 | How do you use JWT tokens in Next.js? | 🟡 | MRH-NJS |
| NEXT-082 | How do you create a route in the Pages Router? | 🟢 | MRH-NJS |
| NEXT-083 | What is a catch-all segment in Next.js? | 🟡 | MRH-NJS |
| NEXT-084 | What is the _app.js file in Next.js? | 🟢 | MRH-NJS |
| NEXT-085 | What is the _document.js file in Next.js? | 🟡 | MRH-NJS |
| NEXT-086 | What is the difference between _app.js and _document.js? | 🟡 | MRH-NJS |
| NEXT-087 | What is the _error.js file in Next.js? | 🟡 | MRH-NJS |
| NEXT-088 | How do you create a 404 page in the Pages Router? | 🟢 | MRH-NJS |
| NEXT-089 | How do you fetch data in a Pages Router page? | 🟡 | MRH-NJS |
| NEXT-090 | What is getStaticProps? | 🟡 | MRH-NJS |
| NEXT-091 | What is getServerSideProps? | 🟡 | MRH-NJS |
| NEXT-092 | What is the difference between getStaticProps and getServerSideProps? | 🟡 | MRH-NJS |
| NEXT-093 | What is getStaticPaths? | 🟡 | MRH-NJS |
| NEXT-094 | What is fallback in getStaticPaths? | 🟡 | MRH-NJS |
| NEXT-095 | How do you handle API routes in the Pages Router? | 🟢 | MRH-NJS |
| NEXT-096 | What are the limitations of the Pages Router? | 🟡 | MRH-NJS |
| NEXT-097 | How do you handle form submissions in the Pages Router? | 🟡 | MRH-NJS |
| NEXT-098 | Performance optimizations in the Pages Router? | 🟡 | MRH-NJS |
| NEXT-099 | How do you handle i18n in the Pages Router? | 🟡 | MRH-NJS |
| NEXT-100 | How does caching work in the Pages Router? | 🟡 | MRH-NJS |
| NEXT-101 | When to choose Pages Router vs App Router? | 🟡 | MRH-NJS |
| NEXT-102 | How do you handle form submissions in the App Router? | 🟡 | MRH-NJS |
| NEXT-103 | How do you add Auth.js to the Next.js App Router? | 🟡 | MRH-NJS |
| NEXT-104 | How do you handle authentication tokens in Next.js? | 🟡 | MRH-NJS |
| NEXT-105 | How do you add a credentials provider with Auth.js? | 🟡 | MRH-NJS |
| NEXT-106 | Difference between using and not using "use server" | 🟡 | MRH-NJS |
| NEXT-107 | How do you handle file uploads in Next.js? | 🟡 | MRH-NJS |
| NEXT-108 | Common use cases for the Next.js App Router | 🟢 | MRH-NJS |
| NEXT-109 | Can you use both App Router and Pages Router together? | 🟡 | MRH-NJS |
| NEXT-110 | What are the limitations of the App Router? | 🟡 | MRH-NJS |
| NEXT-111 | How do you handle authorization in middleware and route handlers? | 🔴 | MRH-NJS |
| NEXT-112 | What are the benefits of Server Actions? | 🟡 | MRH-NJS |
| NEXT-113 | What are the drawbacks of Server Actions? | 🟡 | MRH-NJS |
| NEXT-114 | What are the alternatives to Server Actions? | 🟡 | MRH-NJS |
| NEXT-115 | How do you handle global state management in the App Router? | 🟡 | MRH-NJS |
| NEXT-116 | What are route groups in the App Router? | 🟡 | MRH-NJS |
| NEXT-117 | What are parallel routes in Next.js? | 🔴 | MRH-NJS |
| NEXT-118 | What are intercepting routes in Next.js? | 🔴 | MRH-NJS |
| NEXT-119 | How do you implement nested layouts in the App Router? | 🟡 | MRH-NJS |
| NEXT-120 | How do you handle streaming and Suspense in the App Router? | 🟡 | MRH-NJS |
| NEXT-121 | How do you handle i18n in the Next.js App Router? | 🟡 | MRH-NJS |
| NEXT-122 | Best practices for Next.js API routes | 🟡 | MRH-NJS |
| NEXT-123 | How do you validate and sanitize input data in Next.js? | 🟡 | MRH-NJS |
| NEXT-124 | How do you use proper HTTP methods in Route Handlers? | 🟢 | MRH-NJS |
| NEXT-125 | How do you handle errors in Next.js API routes? | 🟡 | MRH-NJS |
| NEXT-126 | How do you use middleware within Next.js API routes? | 🟡 | MRH-NJS |
| NEXT-127 | How do you keep Next.js API routes modular and organized? | 🟡 | MRH-NJS |
| NEXT-128 | How do you use RTK Query with the Next.js App Router? | 🔴 | MRH-NJS |
| NEXT-129 | How do you use Redux Toolkit with the Next.js App Router? | 🔴 | MRH-NJS |
| NEXT-130 | How do you read and set cookies in the App Router? | 🟡 | MRH-NJS |
| NEXT-131 | How do you stream responses from a Route Handler? | 🔴 | MRH-NJS |
| NEXT-132 | How do you test Next.js App Router components? | 🟡 | MRH-NJS |
| NEXT-133 | How do you implement optimistic UI with the App Router? | 🔴 | MRH-NJS |
| NEXT-134 | How do you choose between Edge and Node.js runtimes? | 🔴 | MRH-NJS |
| NEXT-135 | What is the difference between SSR (Pages Router) and React Server Components? | 🔴 | YTH-NJS |
| NEXT-136 | What is styled-jsx in Next.js? | 🟢 | YTE-NJS |
| NEXT-137 | What are the main scripts in a Next.js package.json? | 🟢 | YTE-NJS |
| NEXT-138 | What properties does the context object have in getInitialProps? | 🟡 | YTE-NJS |
| NEXT-139 | What is the difference between Next.js and Create React App? | 🟢 | YTE-NJS |
| NEXT-140 | How do you configure the build ID in Next.js? | 🟡 | YTE-NJS |
| NEXT-141 | How do you configure a CDN in Next.js? | 🟡 | YTE-NJS |
| NEXT-142 | Can Next.js be hosted on nginx or traditional web servers? | 🟡 | YTE-NJS |
| NEXT-143 | What are the competitors and alternatives to Next.js? | 🟢 | YTE-NJS |
| NEXT-144 | In what language is Next.js written? | 🟢 | YTE-NJS |

**144 entries — NEXT-001 to NEXT-144 — Next available: NEXT-145**

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
- **Streaming / Suspense** → [REACT-038](../react/03-advanced.md#react-038), [REACT-108](../react/07-react19.md#react-108)

---

*← [Master Index](../reference/00-master-index.md) | [Source Library →](../reference/02-source-library.md)*
