# Next.js — Server Components & Client Components

> **IDs:** NEXT-011 – NEXT-014
> **Source:** GFE-NJS (GreatFrontEnd Next.js Freshers, Jun 2026)

---

## NEXT-011

### What are Server Components in Next.js and what can't they do?

🟢 Beginner

Server Components are React components that render exclusively on the server and never ship their component code to the browser. In the App Router, **every component is a Server Component by default** unless you mark it with `"use client"`.

Because they run only on the server, Server Components can:
- `await` data directly — no `useEffect`, no loading state boilerplate
- Access databases, file systems, and secrets without exposing them to the browser
- Import large server-only libraries without adding to the client bundle
- Reduce the JavaScript sent to the browser (component code itself is not included)

What Server Components **cannot** do:
- Use React hooks (`useState`, `useEffect`, `useRef`, etc.)
- Attach browser event listeners (`onClick`, `onChange`, etc.)
- Use browser-only APIs (`window`, `localStorage`, `document`)
- Use React Context directly

```tsx
// app/products/page.tsx — Server Component (default)
// No "use client" needed. This runs on the server.
export default async function ProductsPage() {
  // Direct DB or API call — never exposed to browser
  const products = await db.query('SELECT * FROM products LIMIT 20');

  return (
    <ul>
      {products.map(p => (
        <li key={p.id}>{p.name} — ${p.price}</li>
      ))}
    </ul>
  );
}
// Zero JS for this component is shipped to the client.
```

**Related:** NEXT-012 — Client Components | NEXT-013 — When to use "use client" | NEXT-014 — Composition boundary

**Source:** GFE-NJS-011

---

## NEXT-012

### What are Client Components and when do you use "use client"?

🟢 Beginner

Client Components are React components that run in the browser. They are the same components you write in a plain React app. In Next.js, you opt into client-side rendering by adding `"use client"` as the very first line of a file. Every component imported into that file also becomes a Client Component.

Use `"use client"` when your component needs:
- **React hooks** — `useState`, `useEffect`, `useReducer`, `useRef`
- **Browser event handlers** — `onClick`, `onSubmit`, `onChange`
- **Browser APIs** — `localStorage`, `window`, `navigator`, `IntersectionObserver`
- **Client-only libraries** — animation libraries, drag-and-drop, maps

```tsx
"use client";

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>Increment</button>
    </div>
  );
}
```

Client Components are still **server-rendered for their initial HTML** (Next.js pre-renders them on the server during SSR), but their JavaScript is also shipped to the browser so interactivity works after hydration.

**Related:** NEXT-011 — Server Components | NEXT-013 — When to use "use client" | NEXT-017 — Hydration

**Source:** GFE-NJS-012

---

## NEXT-013

### When should you use "use client" and what are the downsides of overusing it?

🟡 Intermediate

The guiding principle is: **add `"use client"` as deep in the component tree as possible, and as rarely as necessary.** Server Components are better for performance because they don't ship component code to the browser. Every `"use client"` boundary adds JavaScript to the client bundle.

**Use `"use client"` only when the component genuinely needs:**
- Interactivity (event handlers, controlled inputs)
- State or effects (`useState`, `useEffect`)
- Browser APIs not available on the server
- Third-party libraries that use browser APIs internally

**The downside of overusing it:**
1. **Larger JS bundle** — more components ship their code to the browser
2. **Lost server benefits** — async data fetching must move to `useEffect` or a parent
3. **Marks the entire subtree** — once you add `"use client"`, all imports in that file are also treated as client-side, even if they could have run on the server

**Best practice — push the boundary down:**

```tsx
// ❌ Bad — entire page becomes client-side just for one button
"use client";
export default function ProductPage({ product }) {
  const [added, setAdded] = useState(false);
  return (
    <div>
      <h1>{product.name}</h1> {/* This could be a Server Component */}
      <button onClick={() => setAdded(true)}>Add to cart</button>
    </div>
  );
}

// ✅ Good — only the interactive part is a Client Component
// ProductPage is a Server Component
export default async function ProductPage({ params }) {
  const product = await fetchProduct(params.id);
  return (
    <div>
      <h1>{product.name}</h1>       {/* Stays on server */}
      <AddToCartButton id={product.id} /> {/* Client Component */}
    </div>
  );
}
```

**Related:** NEXT-011 — Server Components | NEXT-012 — Client Components | NEXT-014 — Composition boundary

**Source:** GFE-NJS-013

---

## NEXT-014

### Can a Server Component import a Client Component, and vice versa?

🟡 Intermediate

The composition rules are asymmetric:

**Server Component → Client Component: ✅ Yes**
A Server Component can import and render a Client Component. This is the normal pattern — Server Components form the tree's backbone, and Client Components are leaves for interactive parts.

**Client Component → Server Component via direct import: ❌ No**
A Client Component cannot `import` a Server Component because the imported file would be pulled into the client bundle, making it a Client Component too.

**Client Component → Server Component via `children` prop: ✅ Yes (the workaround)**
You can pass a Server Component as `children` (or any prop) to a Client Component. The Server Component is rendered on the server and its output (HTML) is passed in — the client never gets the Server Component's source code.

```tsx
// ✅ Server Component passes a Server Component as children to a Client Component

// ClientShell.tsx (Client Component)
"use client";
export function ClientShell({ children }: { children: React.ReactNode }) {
  const [open, setOpen] = useState(true);
  return open ? <div>{children}</div> : null;
}

// app/page.tsx (Server Component)
import { ClientShell } from './ClientShell';
import { ServerContent } from './ServerContent'; // Server Component

export default function Page() {
  return (
    <ClientShell>
      <ServerContent /> {/* Rendered on server, output passed as children */}
    </ClientShell>
  );
}
```

> **Scenario:** You have a `<ProductPage>` Server Component that fetches product data, and a `<AddToCartButton>` Client Component that needs interactivity. Render `<AddToCartButton>` as a child of the Server Component — not the other way round.

**Related:** NEXT-011 — Server Components | NEXT-012 — Client Components | NEXT-013 — When to use "use client"

**Source:** GFE-NJS-014
