---
title: Defer Non-Critical Third-Party Libraries
impact: MEDIUM
impactDescription: loads after hydration
tags: bundle, third-party, analytics, defer
---

## Defer Non-Critical Third-Party Libraries

Analytics, logging, and error tracking don't block user interaction. Load them after hydration.

**Incorrect (blocks initial bundle):**

```tsx
import { Analytics } from 'some-analytics/react'

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <DeferredAnalytics />
      </body>
    </html>
  )
}
```

In Next.js, `next/dynamic` with `{ ssr: false }` does the same in one call.

**Correct (loads after hydration):**

```tsx
import { lazy, Suspense, useEffect, useState } from 'react'

const Analytics = lazy(() =>
  import('some-analytics/react').then(m => ({ default: m.Analytics }))
)

// Render only on the client, after hydration
function DeferredAnalytics() {
  const [mounted, setMounted] = useState(false)
  useEffect(() => setMounted(true), [])
  return mounted ? <Suspense fallback={null}><Analytics /></Suspense> : null
}

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <DeferredAnalytics />
      </body>
    </html>
  )
}
```

In Next.js, `next/dynamic` with `{ ssr: false }` does the same in one call.
