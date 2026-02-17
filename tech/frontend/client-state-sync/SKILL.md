# Client State Synchronization

Keep client state in sync with server state through optimistic updates, real-time sync, and cache management.

## Context Sync Protocol

Read existing data fetching and caching patterns.

## Decision Tree: Sync Strategy

```
How fresh does data need to be?
├── Eventually consistent (seconds to minutes OK)
│   └── Polling or refetch on focus
│       → TanStack Query with refetchInterval or refetchOnWindowFocus
├── Near-real-time (seconds)
│   └── Server-Sent Events (SSE) for server push
│       → Unidirectional updates, simpler than WebSocket
├── Real-time (instant)
│   └── WebSocket for bidirectional sync
│       → Chat, collaboration, live cursors
├── Optimistic (assume success, rollback on failure)
│   └── TanStack Query optimistic updates
│       → Instant UI response, rollback on error
└── Offline-first (works without network)
    └── Local-first with sync engine
        → IndexedDB + sync protocol (complex)
```

## Optimistic Updates Pattern

```typescript
// TanStack Query optimistic update
useMutation({
  mutationFn: updateTodo,
  onMutate: async (newTodo) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] })
    const previous = queryClient.getQueryData(['todos'])
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo])
    return { previous }
  },
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previous) // Rollback
  },
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] }) // Refetch truth
  },
})
```

## Cache Invalidation Strategies

| Strategy | When | Implementation |
|----------|------|----------------|
| **Time-based** | Data is OK to be stale for N seconds | `staleTime: 30_000` |
| **Event-based** | After mutation, invalidate related queries | `queryClient.invalidateQueries()` |
| **Real-time** | Server pushes updates | WebSocket/SSE updates cache |
| **Focus-based** | Refresh when user returns to tab | `refetchOnWindowFocus: true` |
| **Manual** | User pulls to refresh | Refetch on button click |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Freshness** | Appropriate staleness per data type | Global staleness | Always stale or always fetching |
| **Optimistic updates** | Instant UI with proper rollback | Some optimistic | Wait for server on every action |
| **Error recovery** | Graceful rollback, retry, user notification | Basic rollback | Broken state on error |
| **Cache management** | Smart invalidation, no stale data leaks | Basic invalidation | Cache never invalidated |
| **Loading states** | Skeleton + stale data while refetching | Spinner on every fetch | No loading states |
| **Offline handling** | Graceful degradation or offline support | Error on offline | Crashes offline |
| **Deduplication** | Same data fetched once, shared across components | Some sharing | Duplicate fetches |

**28+ = Seamless sync | 21-27 = Some lag/inconsistency | <21 = Stale or broken data**
