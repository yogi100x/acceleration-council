# Caching Strategy

Design multi-layer caching for performance, consistency, and cost optimization.

## Research Protocol

### Web Search
- "Redis latest version [current year]"
- "CDN caching best practices [current year]"
- "cache invalidation patterns [current year]"

### WebFetch
- https://redis.io/docs/latest/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching

---

## Context Sync Protocol

### Read Upstream
- `.claude/outputs/system-architecture.md` — infrastructure choices
- `.claude/outputs/database-schema.md` — query patterns to cache

---

## Decision Tree: Cache Layer

```
What are you caching?
├── Static assets (JS, CSS, images, fonts)
│   └── CDN + immutable headers (Cache-Control: max-age=31536000, immutable)
├── API responses (same data for all users)
│   └── CDN or edge cache + stale-while-revalidate
├── API responses (per-user data)
│   └── Application cache (Redis) with user-scoped keys
├── Database query results
│   └── Application cache (Redis) with query-hash keys
├── Computed/expensive results (AI, reports)
│   └── Application cache (Redis) with TTL based on freshness needs
├── Session data
│   └── Redis with absolute TTL
└── Full page (SSG/ISR)
    └── CDN + revalidation (Next.js ISR pattern)
```

## Cache Invalidation Strategies

| Strategy | When | Implementation |
|----------|------|----------------|
| **TTL (time-based)** | Data is OK to be stale for N seconds | `SET key value EX 300` |
| **Event-based** | After mutations, invalidate related cache | Pub/sub or direct invalidation after write |
| **Version-based** | Cache key includes version/hash | `user:123:v5` → increment version on change |
| **Tag-based** | Multiple keys share a tag for bulk invalidation | Tag: `org:456` → invalidate all org cache at once |
| **Write-through** | Cache is always current | Write to cache AND database simultaneously |
| **Write-behind** | Buffer writes for performance | Write to cache, async flush to database |

## Cache Key Design

```
Pattern: {entity}:{id}:{qualifier}:{version}

Examples:
  user:123:profile:v3
  org:456:members:page:1
  search:hash(query):results
  api:GET:/users:hash(params)

Rules:
- Deterministic (same input → same key)
- Namespaced (no collisions between entities)
- Includes version or hash for invalidation
- Human-readable for debugging
```

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T2 | Cache without invalidation strategy | Stale data served indefinitely |
| T30 | Cache everything | Memory bloat, stale data everywhere |
| T31 | No cache monitoring | Can't see hit rate, stale entries, memory usage |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Layer selection** | Right cache layer per data type | Some layering | No strategy |
| **Invalidation** | Event-based + TTL fallback | TTL only | No invalidation |
| **Key design** | Namespaced, versioned, deterministic | Basic keys | Random/collision-prone |
| **Hit rate** | >90% cache hit rate measured | Hit rate tracked | Not measured |
| **Consistency** | Cache-aside with invalidation on write | Mostly consistent | Stale data issues |
| **Failure handling** | App works without cache (degraded) | Errors on cache miss | App crashes without cache |
| **Monitoring** | Hit/miss rate, memory, eviction alerts | Basic monitoring | No monitoring |

**28+ = Fast and consistent | 21-27 = Gaps in strategy | <21 = Cache is causing problems**

## Output Protocol

Write to `.claude/outputs/caching-strategy.md`:
- Cache layers chosen per data type
- Invalidation strategy per entity
- Key naming convention
- TTL values per cache type
