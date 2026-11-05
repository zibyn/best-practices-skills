---
title: Deduplicate Global Event Listeners
impact: LOW
impactDescription: single listener for N components
tags: client, event-listeners, subscription
---

## Deduplicate Global Event Listeners

Share one global event listener across component instances instead of registering one per instance.

**Incorrect (N instances = N listeners):**

```tsx
function useKeyboardShortcut(key: string, callback: () => void) {
  useEffect(() => {
    const handler = (e: KeyboardEvent) => {
      if (e.metaKey && e.key === key) {
        callback()
      }
    }
    window.addEventListener('keydown', handler)
    return () => window.removeEventListener('keydown', handler)
  }, [key, callback])
}
```

When using the `useKeyboardShortcut` hook multiple times, each instance will register a new listener.

**Correct (N instances = 1 listener):**

```tsx
// Module-level registry: one window listener, attached while any callback exists
const keyCallbacks = new Map<string, Set<() => void>>()

function onKeydown(e: KeyboardEvent) {
  if (e.metaKey) keyCallbacks.get(e.key)?.forEach(cb => cb())
}

function useKeyboardShortcut(key: string, callback: () => void) {
  useEffect(() => {
    if (keyCallbacks.size === 0) window.addEventListener('keydown', onKeydown)
    if (!keyCallbacks.has(key)) keyCallbacks.set(key, new Set())
    keyCallbacks.get(key)!.add(callback)

    return () => {
      const set = keyCallbacks.get(key)
      if (set) {
        set.delete(callback)
        if (set.size === 0) keyCallbacks.delete(key)
      }
      if (keyCallbacks.size === 0) window.removeEventListener('keydown', onKeydown)
    }
  }, [key, callback])
}

function Profile() {
  // Multiple shortcuts will share the same listener
  useKeyboardShortcut('p', () => { /* ... */ })
  useKeyboardShortcut('k', () => { /* ... */ })
  // ...
}
```

Data-fetching libraries with subscription APIs can manage the same shared lifecycle if the project already uses one.
