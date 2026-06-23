# Next.js Testing

This file covers testing strategies for Next.js App Router — unit, integration, and end-to-end testing of Server Components, Server Actions, and Route Handlers.

---

## NEXT-132

### How do you test Next.js App Router components and pages?

Testing in the App Router requires different strategies for Server Components vs Client Components:

**Client Components — Vitest + React Testing Library (standard React testing):**
```tsx
// components/__tests__/Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '../Button';

test('calls onClick when clicked', () => {
  const onClick = vi.fn();
  render(<Button onClick={onClick}>Click me</Button>);
  fireEvent.click(screen.getByRole('button'));
  expect(onClick).toHaveBeenCalledOnce();
});
```

**Server Components — render to string and assert HTML:**
```tsx
// Jest or Vitest — render async Server Component
import { renderToString } from 'react-dom/server';
import PostList from '../PostList';

vi.mock('@/lib/db', () => ({ getPosts: vi.fn().mockResolvedValue([{ id: '1', title: 'Test' }]) }));

test('renders posts from DB', async () => {
  const html = renderToString(await PostList());
  expect(html).toContain('Test');
});
```

**Route Handlers — test with `fetch` against a running server or use `next/server` mock:**
```ts
import { GET } from '../app/api/posts/route';
import { NextRequest } from 'next/server';

test('GET /api/posts returns array', async () => {
  const req = new NextRequest('http://localhost/api/posts');
  const res = await GET(req);
  const data = await res.json();
  expect(Array.isArray(data)).toBe(true);
});
```

**End-to-end — Playwright:**
```ts
// e2e/dashboard.spec.ts
import { test, expect } from '@playwright/test';
test('dashboard loads', async ({ page }) => {
  await page.goto('/dashboard');
  await expect(page.getByRole('heading', { name: 'Dashboard' })).toBeVisible();
});
```

**Recommended stack:** Vitest + RTL for unit/integration, Playwright for E2E. Jest works too but Vitest is faster and has better ESM support for modern Next.js.

**Related:** [NEXT-034 — Authentication](./08-infra.md#next-034) | [NEXT-011 — Server Components](./02-server-client.md#next-011)

**Source:** [mrhrifat/nextjs-interview-questions MRH-NJS B-15](../../sources/nextjs/github/mrhrifat/question-map.md)

---

*← [16-performance.md](./16-performance.md) | [Next.js README →](./README.md)*
