# Performance Optimization

Identify and resolve performance bottlenecks across frontend, backend, and database layers.

## Context Sync Protocol

1. Read monitoring dashboards for current performance baselines
2. Read `.claude/product-marketing-context.md` for performance SLAs

## Decision Tree: Where's the Bottleneck?

```
What's slow?
├── Page load (frontend)
│   ├── Large bundle → Code splitting, tree shaking, lazy loading
│   ├── Slow rendering → Memoization, virtualization, reduce re-renders
│   ├── Too many requests → Batching, prefetching, caching
│   └── Large assets → Image optimization, CDN, compression
├── API response (backend)
│   ├── Slow database queries → Indexing, query optimization
│   ├── N+1 queries → Eager loading, DataLoader pattern
│   ├── External API calls → Caching, parallel requests, circuit breaker
│   └── Heavy computation → Background jobs, caching results
├── Database (data layer)
│   ├── Missing indexes → Add targeted indexes
│   ├── Full table scans → Optimize WHERE clauses, partial indexes
│   ├── Connection exhaustion → Pooling, pgBouncer
│   └── Lock contention → Optimize transactions, advisory locks
└── Infrastructure
    ├── Under-provisioned → Right-size instances
    ├── No CDN → Add CDN for static assets
    └── Cold starts → Keep-alive, provisioned concurrency
```

## Performance Budgets

| Metric | Target | Measure With |
|--------|--------|-------------|
| **LCP** (Largest Contentful Paint) | <2.5s | Core Web Vitals |
| **FID** (First Input Delay) | <100ms | Core Web Vitals |
| **CLS** (Cumulative Layout Shift) | <0.1 | Core Web Vitals |
| **TTFB** (Time to First Byte) | <200ms | Server metrics |
| **API p50 latency** | <100ms | APM |
| **API p99 latency** | <500ms | APM |
| **Bundle size** | <200KB gzipped (initial) | Webpack analyzer |
| **Database query** | <50ms p95 | Query logging |

## Optimization Techniques

### Frontend
| Technique | Impact | Effort |
|-----------|--------|--------|
| Code splitting / lazy loading | High | Low |
| Image optimization (WebP, lazy load) | High | Low |
| CDN for static assets | High | Low |
| Memoization (useMemo, React.memo) | Medium | Low |
| Virtual scrolling for long lists | High | Medium |
| Service worker caching | Medium | Medium |

### Backend
| Technique | Impact | Effort |
|-----------|--------|--------|
| Response caching (Redis, CDN) | High | Medium |
| Database query optimization | High | Medium |
| Connection pooling | High | Low |
| Parallel external API calls | Medium | Low |
| Background job offloading | High | Medium |

### Database
| Technique | Impact | Effort |
|-----------|--------|--------|
| Add missing indexes | High | Low |
| EXPLAIN ANALYZE on slow queries | High | Low |
| Partial indexes for common filters | Medium | Low |
| Materialized views for dashboards | High | Medium |
| Partitioning for large tables | High | High |

## Anti-Patterns to Avoid

- **T10: Premature optimization** — Profile first, optimize second. Don't guess at bottlenecks.
- **T1: Caching without invalidation strategy** — Stale cache is worse than no cache.
- **T9: N+1 queries** — Use DataLoader, eager loading, or JOIN to batch queries.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Profiling** | Data-driven optimization with profiling evidence | Some profiling | Guessing at bottlenecks |
| **Budget compliance** | All performance budgets met | Most budgets met | No performance budgets |
| **Caching strategy** | Multi-layer caching with invalidation | Some caching | No caching |
| **Query optimization** | All slow queries identified and optimized | Key queries optimized | No query optimization |
| **Bundle optimization** | Code split, tree shaken, lazy loaded | Some optimization | Monolithic bundle |
| **Monitoring** | Performance dashboards with trend tracking | Basic metrics | No performance monitoring |
| **Regression prevention** | Performance tests in CI, bundle size checks | Some checks | No prevention |

**28+ = Performant system | 21-27 = Known bottlenecks | <21 = Performance problems**

## Cross-Skill References

- **Upstream:** `monitoring-setup` (identify bottlenecks), `database-schema` (query patterns)
- **Downstream:** `ci-cd-pipeline` (performance regression checks)
- **Council:** Submit to `council-review` for performance review
