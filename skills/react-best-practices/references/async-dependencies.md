---
title: Dependency-Based Parallelization
impact: CRITICAL
impactDescription: 2-10× improvement
tags: async, parallelization, dependencies
---

## Dependency-Based Parallelization

For operations with partial dependencies, start every promise as early as its inputs allow, and chain dependent work with `.then()` instead of awaiting in sequence.

**Incorrect (profile waits for config unnecessarily):**

```typescript
const [user, config] = await Promise.all([
  fetchUser(),
  fetchConfig()
])
const profile = await fetchProfile(user.id)
```

**Correct (config and profile run in parallel):**

```typescript
const userPromise = fetchUser()
const profilePromise = userPromise.then(user => fetchProfile(user.id))

const [user, config, profile] = await Promise.all([
  userPromise,
  fetchConfig(),
  profilePromise
])
```

For large dependency graphs, a small dependency-aware task runner can schedule each task at the earliest possible moment; plain promises are enough for most cases.
