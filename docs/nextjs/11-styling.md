# Next.js — Styling

> **IDs:** NEXT-045 – NEXT-047
> **Source:** MRH-NJS (mrhrifat/nextjs-interview-questions)

---

## NEXT-045

### How do you add component-level (scoped) CSS in Next.js?

🟢 Beginner

Next.js has built-in support for **CSS Modules** — any file named `[name].module.css` is scoped to the component that imports it. Class names are automatically transformed into unique identifiers, preventing style collisions.

```css
/* Button.module.css */
.button {
  background: #0070f3;
  color: white;
  padding: 8px 16px;
  border-radius: 4px;
}

.button:hover {
  background: #0051a8;
}
```

```tsx
import styles from './Button.module.css';

export default function Button({ children }) {
  return <button className={styles.button}>{children}</button>;
}
```

The generated class name is something like `Button_button__3ZkMb` — unique to this module.

**Related:** NEXT-046 — global CSS | NEXT-047 — Tailwind CSS

**Source:** MRH-NJS-C21

---

## NEXT-046

### How do you add global CSS in Next.js?

🟢 Beginner

Global CSS files must be imported in a single location:

- **App Router:** import in `app/layout.tsx` (the root layout)
- **Pages Router:** import in `pages/_app.tsx`

Importing global CSS anywhere else will cause an error in development.

```tsx
// app/layout.tsx (App Router)
import './globals.css'; // ✅ correct

export default function RootLayout({ children }) {
  return <html><body>{children}</body></html>;
}
```

```tsx
// pages/_app.tsx (Pages Router)
import '../styles/globals.css'; // ✅ correct

export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

**Note:** you can import as many global CSS files as you want, but all imports must go through the root layout or `_app.tsx`.

**Related:** NEXT-045 — CSS Modules | NEXT-047 — Tailwind CSS | NEXT-084 — _app.js

**Source:** MRH-NJS-C22

---

## NEXT-047

### How do you use Tailwind CSS in Next.js?

🟢 Beginner

`create-next-app` offers Tailwind setup during scaffolding. To add it to an existing project:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p  # creates tailwind.config.js and postcss.config.js
```

Configure the content paths:
```js
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: { extend: {} },
  plugins: [],
};
```

Add the Tailwind directives to your global CSS file:
```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Import `globals.css` in `app/layout.tsx` (or `pages/_app.tsx`), and you can use Tailwind classes in any component.

**Related:** NEXT-046 — global CSS | NEXT-045 — CSS Modules

**Source:** MRH-NJS-C23

---

*← [10-config-tooling](./10-config-tooling.md) | [12-scripts-seo →](./12-scripts-seo.md)*
