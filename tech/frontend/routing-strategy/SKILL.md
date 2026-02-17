# Routing Strategy

Design application routing for performance, SEO, and user experience.

## Context Sync Protocol

Read existing routing structure and navigation patterns.

## Decision Tree: Routing Architecture

```
What type of app?
├── Content site (blog, docs, marketing)
│   └── File-based routing (Next.js App Router)
│       → Static generation, SEO-optimized
├── SPA (admin panel, dashboard)
│   └── Client-side routing (React Router or TanStack Router)
│       → Fast navigation, no server round-trips
├── Multi-app (separate domains/subdomains)
│   └── Module federation or separate deployments
│       → Independent deployment, shared auth
├── Hybrid (public + authenticated sections)
│   └── Route groups with different layouts
│       → (public) and (protected) groups, middleware
└── Micro-frontends
    └── Route-based composition
        → Each team owns routes, composed at runtime
```

## Next.js App Router Patterns

| Pattern | When | Implementation |
|---------|------|----------------|
| **Route groups** | Shared layouts without URL impact | `(marketing)/`, `(dashboard)/` |
| **Dynamic routes** | Entity pages | `[id]/page.tsx`, `[slug]/page.tsx` |
| **Catch-all** | Nested paths | `[...slug]/page.tsx` |
| **Parallel routes** | Simultaneous content | `@modal/`, `@sidebar/` |
| **Intercepting routes** | Modal overlays | `(.)/photo/[id]` |
| **Route handlers** | API endpoints | `route.ts` (GET, POST, etc.) |
| **Middleware** | Auth, redirects, rewrites | `middleware.ts` at app root |

## URL Design Principles

| Principle | Good | Bad |
|-----------|------|-----|
| **Readable** | `/jobs/senior-react-developer` | `/jobs/12345` |
| **Predictable** | `/users/123/settings` | `/settings?user=123` |
| **Bookmarkable** | Filters in URL params | Filters in component state |
| **Shareable** | Full state in URL | State lost on share |
| **Hierarchical** | `/org/team/member` | `/member?org=x&team=y` |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **URL design** | Clean, readable, semantic URLs | Mostly clean | Random/ugly URLs |
| **Navigation UX** | Instant transitions, preserved scroll, back works | Mostly works | Broken back button |
| **SEO** | Proper slugs, canonical URLs, sitemap | Basic SEO | No SEO consideration |
| **Code splitting** | Per-route code splitting, minimal bundles | Some splitting | Monolithic bundle |
| **Auth routing** | Protected routes, redirects, middleware | Basic protection | No route protection |
| **Loading states** | Per-route loading.tsx, streaming | Global loading | No loading states |
| **Error handling** | Per-route error.tsx, not-found.tsx | Global error page | Errors crash the app |

**28+ = Excellent navigation UX | 21-27 = Functional | <21 = Broken navigation**
