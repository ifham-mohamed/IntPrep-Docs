# Next.js — SEO, Assets & Metadata

> **IDs:** NEXT-029 – NEXT-031
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-029

### How do you add metadata for SEO in the App Router?

🟢 Beginner

The App Router has a first-class Metadata API. Instead of placing `<meta>` tags manually in HTML, you export a `metadata` object or a `generateMetadata()` function from any `page.tsx` or `layout.tsx`. Next.js merges these up the layout tree and injects the correct `<head>` tags automatically.

**Static metadata** — for pages where metadata is known at build time:

```tsx
// app/about/page.tsx
import { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'About Us — Acme Corp',
  description: 'Learn about Acme Corp and our mission.',
  openGraph: {
    title: 'About Us',
    description: 'Learn about Acme Corp.',
    images: [{ url: '/og-about.png', width: 1200, height: 630 }],
  },
};

export default function AboutPage() {
  return <main>About content</main>;
}
```

**Dynamic metadata** — for pages where metadata depends on fetched data:

```tsx
// app/blog/[slug]/page.tsx
import { Metadata } from 'next';

interface Props { params: Promise<{ slug: string }> }

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { slug } = await params;
  const post = await fetchPost(slug);
  return {
    title: `${post.title} — My Blog`,
    description: post.excerpt,
    openGraph: { images: [{ url: post.coverImage }] },
  };
}

export default async function BlogPost({ params }: Props) {
  const { slug } = await params;
  const post = await fetchPost(slug);
  return <article><h1>{post.title}</h1></article>;
}
```

Next.js also supports a `title.template` in the root layout to avoid repeating the site name in every page title.

**Related:** NEXT-030 — next/image | NEXT-031 — next/font

**Source:** GFE-NJS-029

---

## NEXT-030

### What is next/image and why does it matter for performance?

🟡 Intermediate

`next/image` is Next.js's built-in `<Image>` component that optimizes images automatically. Using a plain `<img>` tag in Next.js works, but you lose a significant set of performance features.

**What `<Image>` does automatically:**
- **Format conversion** — serves WebP or AVIF instead of JPEG/PNG when the browser supports it (typically 25–40% smaller files)
- **Responsive sizing** — generates multiple sizes via `srcset` so mobile devices download smaller images
- **Lazy loading** — images below the fold are not loaded until the user scrolls near them
- **Prevents Cumulative Layout Shift (CLS)** — requires `width` and `height` (or `fill` layout) so the browser reserves space before the image loads
- **Priority loading** — set `priority` on above-the-fold hero images to eagerly load and improve LCP
- **Built-in blur placeholder** — set `placeholder="blur"` for a low-quality preview while the full image loads

```tsx
import Image from 'next/image';

export default function Hero() {
  return (
    <Image
      src="/hero.jpg"
      alt="Our product"
      width={1200}
      height={630}
      priority           // Load eagerly — above the fold
      placeholder="blur" // Show blurred preview while loading
    />
  );
}

// Fill mode — image fills its positioned parent
<div style={{ position: 'relative', width: '100%', height: '400px' }}>
  <Image src="/banner.jpg" alt="Banner" fill style={{ objectFit: 'cover' }} />
</div>
```

**Related:** NEXT-031 — next/font | NEXT-029 — Metadata for SEO

**Source:** GFE-NJS-030

---

## NEXT-031

### What is next/font and why use it over a manual font link tag?

🟡 Intermediate

`next/font` is Next.js's font optimization system. It solves the two biggest font performance problems: Cumulative Layout Shift (CLS) caused by font loading, and the privacy/performance cost of loading fonts from an external CDN like Google Fonts.

**What `next/font` does:**
- **Self-hosts fonts at build time** — downloads fonts from Google Fonts (or local files) once at build time and serves them from your domain. Zero third-party requests at runtime.
- **Eliminates CLS** — automatically uses `font-display: swap` and injects correct `size-adjust` values so fallback and custom fonts render at the same size, preventing layout jump
- **Zero layout shift** — the font is available immediately on first paint because it is colocated with your app
- **Generates CSS variables** — you can use the font as a CSS custom property across your entire stylesheet

```tsx
// app/layout.tsx
import { Inter, Playfair_Display } from 'next/font/google';

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
});

const playfair = Playfair_Display({
  subsets: ['latin'],
  variable: '--font-playfair',
  weight: ['400', '700'],
});

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={`${inter.variable} ${playfair.variable}`}>
      <body>{children}</body>
    </html>
  );
}

// globals.css
body { font-family: var(--font-inter), sans-serif; }
h1   { font-family: var(--font-playfair), serif; }
```

For local fonts, use `next/font/local` with the `src` pointing to font files in your project.

**Related:** NEXT-030 — next/image | NEXT-029 — SEO and metadata

**Source:** GFE-NJS-031
