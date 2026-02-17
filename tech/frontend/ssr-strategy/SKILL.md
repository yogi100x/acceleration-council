# SSR Strategy

Choose the right rendering strategy for each page: SSR, SSG, ISR, CSR, or streaming.

## Context Sync Protocol

Read routing structure and page requirements.

## Decision Tree: Rendering Mode

```
What does this page need?
├── Static content (marketing, docs, blog)
│   └── SSG (Static Site Generation)
│       → Built at build time. Fastest. CDN-cached.
├── Static but updates periodically (product catalog, pricing)
│   └── ISR (Incremental Static Regeneration)
│       → SSG + revalidation interval. Best of both worlds.
├── Personalized content (dashboard, settings)
│   └── SSR (Server-Side Rendering)
│       → Rendered per-request. Has user context.
├── SEO not needed, highly interactive (admin panel, editor)
│   └── CSR (Client-Side Rendering)
│       → SPA behavior. Fastest interactivity after load.
├── Mixed (shell is static, data is dynamic)
│   └── Streaming SSR + Suspense
│       → Send shell immediately, stream data as ready.
└── Real-time (chat, live dashboard)
    └── CSR + WebSocket/SSE
        → Initial SSR for SEO, then client takes over.
```

## Next.js App Router Patterns

| Pattern | Implementation | Use Case |
|---------|---------------|----------|
| **Server Component** (default) | No directive needed | Data fetching, static content |
| **Client Component** | `'use client'` | Interactivity, hooks, browser APIs |
| **Dynamic rendering** | `export const dynamic = 'force-dynamic'` | Per-request data |
| **Static + revalidate** | `export const revalidate = 3600` | ISR (1 hour) |
| **Streaming** | `<Suspense fallback={<Skeleton/>}>` | Progressive loading |
| **Parallel routes** | `@slot` folders | Simultaneous data loading |
| **Intercepting routes** | `(.)` convention | Modal overlays on navigation |

## Performance Implications

| Strategy | TTFB | LCP | SEO | Personalization | Caching |
|----------|------|-----|-----|-----------------|---------|
| **SSG** | Fastest | Fastest | Best | None | CDN |
| **ISR** | Fast | Fast | Good | None | CDN + revalidate |
| **SSR** | Medium | Medium | Good | Full | Per-request |
| **Streaming** | Fast (shell) | Medium | Good | Full | Partial |
| **CSR** | Fast (empty) | Slow | Poor | Full | Client only |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Strategy selection** | Right rendering per page type | Mostly appropriate | One strategy for everything |
| **SEO** | SSG/SSR for public pages, meta tags, structured data | Some SSR | CSR for SEO-needed pages |
| **Performance** | Streaming, parallel loading, optimized TTFB | Basic optimization | No optimization |
| **Caching** | Multi-layer caching (CDN, ISR, client) | Some caching | No caching strategy |
| **Data fetching** | Server components for data, minimal client fetching | Mixed | Client-side fetching for everything |
| **Loading states** | Suspense boundaries with skeletons | Basic loading | Blank screens |
| **Error handling** | error.tsx boundaries per route | Global error page | Errors crash the page |

**28+ = Optimized rendering | 21-27 = Works but suboptimal | <21 = Wrong rendering choices**
