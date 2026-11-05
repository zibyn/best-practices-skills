---
title: Use a Data-Fetching Cache for Automatic Deduplication
impact: MEDIUM-HIGH
impactDescription: automatic deduplication
tags: client, deduplication, data-fetching, cache
---

## Use a Data-Fetching Cache for Automatic Deduplication

A keyed data-fetching cache (for example TanStack Query or SWR) deduplicates requests, caches results, and revalidates across component instances. Raw `fetch` in `useEffect` gives every instance its own request.

**Incorrect (no deduplication, each instance fetches):**

```tsx
function UserList() {
  const [users, setUsers] = useState([])
  useEffect(() => {
    fetch('/api/users')
      .then(r => r.json())
      .then(setUsers)
  }, [])
}
```

**Correct (multiple instances share one request, shown with TanStack Query):**

```tsx
import { useQuery } from '@tanstack/react-query'

function UserList() {
  const { data: users } = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
}
```

**For immutable data (never refetch):**

```tsx
function StaticContent() {
  const { data } = useQuery({
    queryKey: ['config'],
    queryFn: fetchConfig,
    staleTime: Infinity,
  })
}
```

**For mutations:**

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

function UpdateButton() {
  const queryClient = useQueryClient()
  const { mutate } = useMutation({
    mutationFn: updateUser,
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ['user'] }),
  })
  return <button onClick={() => mutate()}>Update</button>
}
```

Use whichever cache library the project already has; the rule is the shared key-based cache, not a specific library.
