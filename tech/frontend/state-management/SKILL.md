# State Management

Choose and implement the right state management approach for your application's complexity.

## Context Sync Protocol

Read existing state patterns and data flow requirements.

## Decision Tree: State Strategy

```
How complex is your state?
├── Simple (form inputs, toggles, UI state)
│   └── useState / useReducer
│       → Local component state. No library needed.
├── Shared across siblings (2-3 components)
│   └── Lift state up to parent
│       → Pass via props. Still no library.
├── Shared across distant components (5+ consumers)
│   └── React Context + useReducer
│       → Built-in, no deps. Good for auth, theme, locale.
├── Server state (API data, caching, sync)
│   └── TanStack Query (React Query)
│       → Handles fetching, caching, invalidation, optimistic updates
├── Complex client state (many interdependencies)
│   └── Zustand (simple) or Jotai (atomic)
│       → Zustand: single store, selectors. Jotai: atom-based, fine-grained.
└── Very complex (undo/redo, time travel, collaborative)
    └── Redux Toolkit or XState
        → Redux: predictable, middleware. XState: state machines.
```

## State Categories

| Category | Where to Store | Example |
|----------|---------------|---------|
| **UI state** | useState/useReducer | Modal open, sidebar collapsed, active tab |
| **Form state** | react-hook-form or useState | Input values, validation errors |
| **Server state** | TanStack Query | API responses, user data, lists |
| **URL state** | URL params / searchParams | Filters, pagination, selected tab |
| **Auth state** | Context + secure storage | Current user, session, permissions |
| **Theme state** | Context + CSS variables | Dark mode, brand colors |
| **Global app state** | Zustand/Jotai | Shopping cart, notification queue |

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Storing server data in Redux/Zustand | Use TanStack Query for server state |
| Using Context for frequently changing values | Context re-renders all consumers. Use Zustand for frequent updates. |
| Duplicating state (same data in multiple places) | Single source of truth. Derive everything else. |
| Storing derived state | Compute from source state with useMemo |
| Global state for local concerns | Keep state as close to where it's used as possible |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Appropriate tool** | Right tool for each state category | Mostly appropriate | One tool for everything |
| **Single source of truth** | No duplicated state | Minimal duplication | State scattered everywhere |
| **Server state** | Proper caching + invalidation (TanStack Query) | Manual fetching with some caching | useEffect + useState for everything |
| **Performance** | Fine-grained subscriptions, minimal re-renders | Reasonable performance | Full tree re-renders |
| **Predictability** | State changes are traceable and debuggable | Mostly predictable | Spaghetti state updates |
| **Persistence** | URL state for shareable views, localStorage where needed | Some persistence | State lost on refresh |
| **Testing** | State logic tested independently | Some testing | Untested |

**28+ = Clean state architecture | 21-27 = Some tangling | <21 = State spaghetti**
