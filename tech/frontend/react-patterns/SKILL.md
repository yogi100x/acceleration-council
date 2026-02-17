# React Patterns

Select and apply the right React patterns for component design, data flow, and performance.

## Context Sync Protocol

Read existing component patterns before starting.

## Decision Tree: Component Pattern

```
What are you building?
├── Reusable UI element (button, card, input)
│   └── Compound component pattern
│       → Flexible composition via children/slots
├── Data-connected component (fetches/displays data)
│   └── Container/Presenter pattern
│       → Container fetches, presenter renders
├── Stateful logic shared across components
│   └── Custom hook pattern
│       → Extract logic into useX() hook
├── Component with many variants
│   └── Polymorphic component + CVA
│       → Variants via props, styled with class-variance-authority
├── Third-party integration (maps, editors, charts)
│   └── Wrapper component pattern
│       → Isolate third-party API behind your interface
└── Complex multi-step UI (wizard, builder)
    └── State machine pattern (useReducer or XState)
        → Explicit states and transitions
```

## Component Composition Patterns

| Pattern | When | Example |
|---------|------|---------|
| **Children** | Flexible content insertion | `<Card>{content}</Card>` |
| **Render props** | Dynamic rendering logic | `<List renderItem={(item) => ...}/>` |
| **Compound** | Related components that share state | `<Select><Option/><Option/></Select>` |
| **HOC** | Cross-cutting concerns (rare in modern React) | `withAuth(Component)` |
| **Custom hooks** | Shared stateful logic | `useDebounce()`, `useLocalStorage()` |
| **Provider** | Deep prop drilling elimination | `<ThemeProvider><App/></ThemeProvider>` |

## Performance Patterns

| Issue | Solution |
|-------|----------|
| Unnecessary re-renders | `React.memo()`, `useMemo()`, `useCallback()` |
| Expensive computations | `useMemo()` with proper deps |
| Large lists | Virtual scrolling (`react-virtual`, `react-window`) |
| Heavy components | `React.lazy()` + `Suspense` |
| Prop drilling | Context or state management library |
| Frequent updates | `useRef` for values that don't trigger re-render |

**Rule: Don't optimize until you measure.** React DevTools Profiler first.

## Anti-Patterns

- **Prop drilling 5+ levels** — Use Context or state management
- **useEffect for derived state** — Use `useMemo` instead
- **State for things that can be computed** — Derive from existing state
- **Giant monolith components** — Extract custom hooks and sub-components
- **Premature memoization** — Profile first, memoize second

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Composition** | Flexible, reusable patterns | Some reuse | Monolith components |
| **State design** | Minimal, derived where possible | Reasonable | Redundant/duplicated state |
| **Performance** | Profiled, optimized where needed | Basic optimization | No performance consideration |
| **Hook design** | Custom hooks for shared logic | Some hooks | Logic duplicated across components |
| **Error handling** | Error boundaries, fallback UI | Some error handling | Errors crash the app |
| **Testing** | Component tests for behavior | Some tests | No tests |
| **Accessibility** | ARIA, keyboard, screen reader | Some a11y | No accessibility |

**28+ = Production-quality | 21-27 = Needs refinement | <21 = Maintenance burden**
