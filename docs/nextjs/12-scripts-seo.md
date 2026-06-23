# Next.js Scripts & SEO

This file covers `next/script`, third-party script loading strategies, SEO tooling, sitemaps, AMP, and analytics integration.

---

## NEXT-062

### What is `next/script` used for?

`next/script` is Next.js's enhanced `<script>` component that gives you fine-grained control over when third-party scripts load, preventing them from blocking the critical rendering path.

```tsx
import Script from 'next/script';

export default function Layout({ children }) {
  return (
    <>
      {children}
      {/* Loads after the page becomes interactive */}
      <Script src="https://analytics.example.com/script.js" strategy="afterInteractive" />
      {/* Loads after all hydration is done — lowest priority */}
      <Script src="https://chat-widget.example.com/widget.js" strategy="lazyOnload" />
      {/* Inline script with afterInteractive */}
      <Script id="gtm-init" strategy="afterInteractive">
        {`window.dataLayer = window.dataLayer || [];`}
      </Script>
    </>
  );
}
```

**Loading strategies:**
- `beforeInteractive` — runs before hydration (use for critical polyfills only)
- `afterInteractive` (default) — runs after hydration (analytics, tag managers)
- `lazyOnload` — runs during browser idle time (chat widgets, low-priority tools)
- `worker` — experimental: runs in a Web Worker (Partytown)

**Related:** [NEXT-029 — generateMetadata for SEO](./07-seo-assets.md#next-029) | [NEXT-065 — next-seo](./12-scripts-seo.md#next-065)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-47](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-065

### What is `next-seo` used for?

`next-seo` is a community library that simplifies managing SEO metadata — Open Graph, Twitter Cards, JSON-LD structured data, and canonical URLs — across a Next.js app without duplicating boilerplate in every page.

```tsx
// Pages Router usage
import { NextSeo } from 'next-seo';

export default function BlogPost({ post }) {
  return (
    <>
      <NextSeo
        title={post.title}
        description={post.excerpt}
        canonical={`https://example.com/blog/${post.slug}`}
        openGraph={{
          url: `https://example.com/blog/${post.slug}`,
          title: post.title,
          description: post.excerpt,
          images: [{ url: post.coverImage }],
        }}
      />
      <article>{post.content}</article>
    </>
  );
}
```

In the App Router, the built-in `generateMetadata()` function covers most use cases without the library. `next-seo` is more relevant in the Pages Router where `generateMetadata` is not available and you need a convenient component-based approach.

**Related:** [NEXT-029 — generateMetadata](./07-seo-assets.md#next-029) | [NEXT-062 — next/script](./12-scripts-seo.md#next-062)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-53](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-068

### How do you add Google Analytics to a Next.js project?

The recommended approach uses `next/script` in the root layout with the `afterInteractive` strategy:

```tsx
// app/layout.tsx
import Script from 'next/script';

const GA_ID = process.env.NEXT_PUBLIC_GA_ID;

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        {children}
        <Script
          src={`https://www.googletagmanager.com/gtag/js?id=${GA_ID}`}
          strategy="afterInteractive"
        />
        <Script id="ga-init" strategy="afterInteractive">
          {`
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());
            gtag('config', '${GA_ID}');
          `}
        </Script>
      </body>
    </html>
  );
}
```

For page-view tracking in the App Router (SPA-style navigation), listen to pathname changes with `usePathname()` in a Client Component and fire `gtag('event', 'page_view', ...)` on each change.

**Related:** [NEXT-062 — next/script](./12-scripts-seo.md#next-062) | [NEXT-033 — Environment variables](../nextjs/08-infra.md#next-033)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-57](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-069

### How do you add a sitemap to a Next.js app?

In the App Router, create a `sitemap.ts` file in the `app/` directory that returns a `MetadataRoute.Sitemap` array:

```ts
// app/sitemap.ts
import { MetadataRoute } from 'next';

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await fetchPosts(); // your data source
  return [
    { url: 'https://example.com', lastModified: new Date(), changeFrequency: 'yearly', priority: 1 },
    { url: 'https://example.com/about', lastModified: new Date(), changeFrequency: 'monthly', priority: 0.8 },
    ...posts.map(post => ({
      url: `https://example.com/blog/${post.slug}`,
      lastModified: post.updatedAt,
      changeFrequency: 'weekly' as const,
      priority: 0.6,
    })),
  ];
}
```

Next.js serves this as `/sitemap.xml` automatically. For the Pages Router, use the `next-sitemap` package or generate a static XML file during the build via a custom script in `postbuild`.

**Related:** [NEXT-029 — generateMetadata](./07-seo-assets.md#next-029) | [NEXT-065 — next-seo](./12-scripts-seo.md#next-065)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-59](../../sources/nextjs/github/mrhrifat/question-map.md)

---

## NEXT-074

### What is AMP in Next.js and how do you enable it?

AMP (Accelerated Mobile Pages) is a Google-backed web standard for creating fast-loading mobile pages. Next.js Pages Router has built-in AMP support, but it is **deprecated** and not supported in the App Router.

```jsx
// Pages Router only — pages/blog/[slug].js
export const config = { amp: true }; // full AMP page

// Or hybrid AMP (renders both AMP and regular version)
export const config = { amp: 'hybrid' };

import { useAmp } from 'next/amp';

export default function Post({ title }) {
  const isAmp = useAmp();
  return (
    <article>
      <h1>{title}</h1>
      {isAmp ? <amp-img src="/img.jpg" width="400" height="300" layout="responsive" /> : <img src="/img.jpg" />}
    </article>
  );
}
```

**Current recommendation:** AMP adoption has declined significantly. Google now uses Core Web Vitals for mobile ranking rather than AMP-only signals. For new projects, focus on performance optimization (next/image, next/font, streaming) instead of AMP.

**Related:** [NEXT-030 — next/image](./07-seo-assets.md#next-030) | [NEXT-029 — SEO metadata](./07-seo-assets.md#next-029)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS C-65/C-66](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [07-seo-assets.md](./07-seo-assets.md) | [13-security.md →](./13-security.md)*
