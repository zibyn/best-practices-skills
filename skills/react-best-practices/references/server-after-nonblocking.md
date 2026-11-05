---
title: Run Side Effects After the Response
impact: MEDIUM
impactDescription: faster response times
tags: server, async, logging, analytics, side-effects
---

## Run Side Effects After the Response

Schedule logging, analytics, and other side effects to run after the response is sent, using the framework's or runtime's post-response hook. Awaiting them before returning adds their latency to every request.

Common hooks: Next.js `after()`, `ctx.waitUntil()` in Workers-style runtimes, or a job queue for work that must survive process restarts.

**Incorrect (blocks response):**

```tsx
export async function POST(request: Request) {
  await updateDatabase(request)

  // Logging blocks the response
  const userAgent = request.headers.get('user-agent') || 'unknown'
  await logUserAction({ userAgent })

  return Response.json({ status: 'success' })
}
```

**Correct (non-blocking, shown with Next.js `after()`):**

```tsx
import { after } from 'next/server'

export async function POST(request: Request) {
  await updateDatabase(request)

  // Read request data before the response; run the side effect after it
  const userAgent = request.headers.get('user-agent') || 'unknown'
  after(() => logUserAction({ userAgent }))

  return Response.json({ status: 'success' })
}
```

**Common use cases:**

- Analytics tracking
- Audit logging
- Sending notifications
- Cache invalidation
- Cleanup tasks

**Important notes:**

- Do not fire-and-forget a bare promise without a hook: serverless runtimes may freeze or kill the process once the response is sent.
- Work that must not be lost (billing, emails) belongs in a durable queue, not a post-response hook.
