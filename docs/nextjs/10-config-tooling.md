# Next.js — Configuration & Tooling

> **IDs:** NEXT-038 – NEXT-049, NEXT-051 – NEXT-058, NEXT-060, NEXT-063
> **Source:** MRH-NJS (mrhrifat/nextjs-interview-questions)

---

## NEXT-038

### How do you enable TypeScript in a Next.js project?

🟢 Beginner

Next.js has **built-in TypeScript support** — no extra packages required. You can enable it in two ways:

**Option A — New project:** pass `--typescript` to the scaffolding command:
```bash
npx create-next-app@latest my-app --typescript
```

**Option B — Existing project:** create an empty `tsconfig.json` in the root, then run `next dev`. Next.js will auto-fill a default `tsconfig.json` and install `typescript`, `@types/react`, and `@types/node`.

```bash
touch tsconfig.json
npm run dev   # Next.js detects the file and bootstraps TypeScript
```

Rename `.js` files to `.tsx` / `.ts` incrementally — TypeScript and JavaScript files can coexist.

**Related:** NEXT-055 — next-env.d.ts

**Source:** MRH-NJS-C12

---

## NEXT-039

### What is the `public` folder in Next.js? How do you serve static files?

🟢 Beginner

The `public/` directory at the project root is served as **static assets** at the root URL (`/`). Files inside it are accessible directly without any import or URL manipulation.

```
public/
  logo.png      →  https://example.com/logo.png
  favicon.ico   →  https://example.com/favicon.ico
  robots.txt    →  https://example.com/robots.txt
```

```tsx
// Reference in JSX — use the path from root, no import needed
<img src="/logo.png" alt="Logo" />
```

**Rules:**
- Only the `public/` folder is exposed — files in `src/`, `pages/`, `app/` etc. are **not** served statically
- Do not put sensitive files (env, keys) here — it is fully public
- For images that need optimization use `next/image` instead of raw `<img>`

**Related:** NEXT-030 — next/image | NEXT-044 — next.config.js

**Source:** MRH-NJS-C15

---

## NEXT-041

### What is the default port for a Next.js app? How do you change it?

🟢 Beginner

Next.js runs on **port 3000** by default.

To change it, use the `-p` flag when starting the dev server:

```bash
next dev -p 4000
# or with npm scripts
npm run dev -- -p 4000
```

For production:
```bash
next start -p 8080
```

Or set the `PORT` environment variable:
```bash
PORT=4000 next dev
```

**Source:** MRH-NJS-C17 / C18

---

## NEXT-043

### What is Fast Refresh in Next.js?

🟢 Beginner

Fast Refresh is Next.js's **hot-reloading system** built on React's Fast Refresh. When you save a file during development, only the changed component re-renders — without losing component state.

**Behaviour:**
- Edit inside a function component → component re-renders, state is preserved
- Edit a file that exports a class component → full reload
- Edit a file with a syntax error → error overlay shown; fixes automatically on save
- Edit a non-component file (util, hook) → components that use it refresh

Fast Refresh is enabled automatically in `next dev` — no configuration needed. It is replaced by the Turbopack HMR in Next.js 15+ when using `--turbopack`.

**Source:** MRH-NJS-C19

---

## NEXT-044

### What is `next.config.js`?

🟢 Beginner

`next.config.js` (or `next.config.ts`) is the **main configuration file** for a Next.js project. It sits at the project root and is loaded by the Next.js server at startup.

```js
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    remotePatterns: [{ hostname: 'cdn.example.com' }],
  },
  async redirects() {
    return [
      { source: '/old', destination: '/new', permanent: true },
    ];
  },
  async headers() {
    return [
      { source: '/(.*)', headers: [{ key: 'X-Frame-Options', value: 'DENY' }] },
    ];
  },
};

module.exports = nextConfig;
```

**Common options:** `reactStrictMode`, `images`, `redirects`, `rewrites`, `headers`, `env`, `webpack`, `experimental`, `i18n`.

**Related:** NEXT-049 — redirects | NEXT-051 — custom headers | NEXT-053 — webpack config

**Source:** MRH-NJS-C20

---

## NEXT-049

### How do you handle redirects in Next.js?

🟡 Intermediate

Next.js supports three types of redirects:

**1. Config-level redirects** (`next.config.js`) — evaluated at the routing layer before any page/component code:
```js
async redirects() {
  return [
    { source: '/old-page', destination: '/new-page', permanent: true },  // 308
    { source: '/blog/:slug', destination: '/posts/:slug', permanent: false }, // 307
  ];
},
```

**2. Server-side redirect** (App Router — `redirect()` from `next/navigation`):
```tsx
import { redirect } from 'next/navigation';
export default async function Page() {
  const session = await getSession();
  if (!session) redirect('/login');
}
```

**3. Pages Router** — return from `getStaticProps` / `getServerSideProps`:
```tsx
export async function getServerSideProps() {
  return { redirect: { destination: '/login', permanent: false } };
}
```

**Related:** NEXT-025 — redirect() vs client navigation | NEXT-044 — next.config.js

**Source:** MRH-NJS-C31

---

## NEXT-051

### How do you add custom HTTP headers in Next.js?

🟡 Intermediate

Define custom headers in `next.config.js` using the `headers()` async function. Headers are applied at the routing layer before any page code runs.

```js
// next.config.js
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',  // apply to all routes
        headers: [
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'",
          },
        ],
      },
      {
        source: '/api/(.*)',
        headers: [{ key: 'Cache-Control', value: 'no-store' }],
      },
    ];
  },
};
```

**Related:** NEXT-044 — next.config.js | NEXT-072 — security practices

**Source:** MRH-NJS-C34

---

## NEXT-052

### What is the `next export` command?

🟡 Intermediate

`next export` generates a **fully static HTML export** of your Next.js app — no Node.js server required. Every page is pre-rendered to a static `.html` file that can be hosted on any static file host (GitHub Pages, S3, Netlify, etc.).

```bash
next build && next export
# Output: out/ directory of static files
```

**Limitations — features that do NOT work with `next export`:**
- `getServerSideProps` (requires a server)
- API Routes (require a server)
- ISR (requires a server for revalidation)
- Image Optimization (requires a server; use a cloud provider or `unoptimized: true`)
- Middleware (requires an edge runtime)

> **Note:** In Next.js 13+ (App Router) `next export` is replaced by `output: 'export'` in `next.config.js`.

```js
// next.config.js (Next.js 13+)
module.exports = { output: 'export' };
```

**Related:** NEXT-044 — next.config.js | NEXT-035 — Vercel deployment

**Source:** MRH-NJS-C35

---

## NEXT-053

### How do you configure Webpack in Next.js?

🟡 Intermediate

Next.js exposes the Webpack config via the `webpack` function in `next.config.js`. The function receives the current config object and an options object, and must return the modified config.

```js
// next.config.js
module.exports = {
  webpack(config, { buildId, dev, isServer, defaultLoaders, webpack }) {
    // Add a custom loader
    config.module.rules.push({
      test: /\.svg$/,
      use: ['@svgr/webpack'],
    });

    // Add a plugin
    config.plugins.push(new webpack.DefinePlugin({ VERSION: JSON.stringify('1.0.0') }));

    return config; // always return the config
  },
};
```

**Important:** always return the modified `config`. The `isServer` flag lets you apply rules only to the server or client bundle.

**Related:** NEXT-044 — next.config.js | NEXT-054 — Babel config

**Source:** MRH-NJS-C38

---

## NEXT-054

### How do you configure a custom Babel setup in Next.js?

🟡 Intermediate

Next.js uses SWC as the default compiler (much faster than Babel). If you need custom Babel configuration, create a `.babelrc` or `babel.config.js` in the root — Next.js will detect it and use Babel instead of SWC.

```json
// .babelrc
{
  "presets": ["next/babel"],
  "plugins": [
    ["@babel/plugin-proposal-decorators", { "legacy": true }],
    "babel-plugin-styled-components"
  ]
}
```

**Important:** always include `"next/babel"` as a preset — it includes all the defaults Next.js needs. Adding a custom `.babelrc` **disables SWC** and will slow down your build.

> In Next.js 13+, prefer SWC transforms (`next.config.js` `compiler` options) over Babel where possible.

**Related:** NEXT-053 — Webpack config

**Source:** MRH-NJS-C39

---

## NEXT-055

### What is the purpose of `next-env.d.ts`?

🟢 Beginner

`next-env.d.ts` is an auto-generated TypeScript declaration file that tells the TypeScript compiler about Next.js-specific types. It is created automatically when you run `next dev` or `next build` in a TypeScript project.

```ts
/// <reference types="next" />
/// <reference types="next/image-types/global" />
```

**Rules:**
- Do **not** edit or delete this file — Next.js regenerates it on every build
- Do **not** commit it in some setups — check your `.gitignore` preference (Next.js recommends keeping it in git so CI works without running dev first)
- It should be included in your `tsconfig.json` via the `include` array or default glob

**Related:** NEXT-038 — TypeScript setup

**Source:** MRH-NJS-C40

---

## NEXT-056

### What is `next-compose-plugins`?

🟢 Beginner

`next-compose-plugins` is a community utility that makes it easier to compose multiple Next.js plugins in `next.config.js` without deeply nesting wrapper calls.

```js
// Without next-compose-plugins (deeply nested)
const withMDX = require('@next/mdx')();
const withBundleAnalyzer = require('@next/bundle-analyzer')({ enabled: true });
module.exports = withBundleAnalyzer(withMDX({ reactStrictMode: true }));

// With next-compose-plugins (flat array)
const withPlugins = require('next-compose-plugins');
module.exports = withPlugins([withMDX, withBundleAnalyzer], { reactStrictMode: true });
```

> **Note:** `next-compose-plugins` is not an official Next.js package and is no longer actively maintained. In Next.js 13+ the recommended approach is to use the `experimental.bundlePagesExternals` and other built-in config keys, or compose plugins manually.

**Source:** MRH-NJS-C41

---

## NEXT-057

### How do you add polyfills in Next.js?

🟡 Intermediate

Next.js automatically includes polyfills for widely-used browser APIs (`fetch`, `Object.assign`, etc.). For custom polyfills:

**Option A — Custom polyfill entry file (recommended):**
Create `polyfills.js` and import it in `next.config.js` via the webpack entry:

```js
// next.config.js
module.exports = {
  webpack(config, { isServer }) {
    if (!isServer) {
      const originalEntry = config.entry;
      config.entry = async () => {
        const entries = await originalEntry();
        if (entries['main.js'] && !entries['main.js'].includes('./polyfills.js')) {
          entries['main.js'].unshift('./polyfills.js');
        }
        return entries;
      };
    }
    return config;
  },
};
```

**Option B — Import directly in `_app.js`:**
```js
// pages/_app.js
import 'intersection-observer'; // polyfill
```

**Source:** MRH-NJS-C42

---

## NEXT-058

### What is static optimization in Next.js?

🟡 Intermediate

Automatic Static Optimization is Next.js's ability to **detect at build time** whether a page can be pre-rendered as static HTML. A page is automatically optimized (statically generated) if:

- It does **not** export `getServerSideProps` or `getInitialProps`
- It only uses `getStaticProps` (or no data fetching at all)

Next.js marks such pages as static, pre-renders them to `.html` files, and serves them from the CDN — resulting in the fastest possible page loads.

**Indicator in the build output:**
- `○` = statically rendered
- `λ` = server-rendered
- `●` = statically generated with data (`getStaticProps`)

**Related:** NEXT-015 — SSR vs SSG | NEXT-090 — getStaticProps

**Source:** MRH-NJS-C43

---

## NEXT-060

### What is React Strict Mode in Next.js?

🟢 Beginner

React Strict Mode is a development-only tool that highlights potential problems in your React code by intentionally **double-invoking** certain functions (like render and lifecycle methods) and flagging deprecated APIs.

Enable it in `next.config.js`:

```js
// next.config.js
module.exports = {
  reactStrictMode: true,
};
```

**What Strict Mode catches:**
- Components with side effects in the render phase
- Use of deprecated lifecycle methods (`componentWillMount`, etc.)
- Unexpected side effects from non-idempotent renders
- Violations of the Rules of Hooks

**Important:** Strict Mode only runs in development — it has **no effect** on production builds. Next.js enables it by default in new projects created with `create-next-app`.

**Source:** MRH-NJS-C45

---

## NEXT-063

### What is a custom server in Next.js?

🔴 Advanced

A custom server replaces Next.js's built-in HTTP server with your own Node.js server (Express, Fastify, Koa, etc.). This gives you full control over request handling before Next.js processes it.

```js
// server.js
const express = require('express');
const next = require('next');

const dev = process.env.NODE_ENV !== 'production';
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  const server = express();

  server.get('/custom-route', (req, res) => {
    return app.render(req, res, '/actual-page', req.query);
  });

  server.all('*', (req, res) => handle(req, res));
  server.listen(3000, () => console.log('Ready on http://localhost:3000'));
});
```

**When to use:** complex URL rewrites, WebSocket support, multi-tenant routing. **Downside:** you lose Vercel's serverless/edge deployment optimizations — not recommended unless necessary.

**Related:** NEXT-044 — next.config.js rewrites as a simpler alternative

**Source:** MRH-NJS-C49

---

*← [09-pages-router](./09-pages-router.md) | [11-styling →](./11-styling.md)*
