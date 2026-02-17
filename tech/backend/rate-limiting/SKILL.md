# Rate Limiting

Protect APIs from abuse with intelligent rate limiting per user, endpoint, and tier.

## Research Protocol

### Web Search
- "rate limiting algorithms comparison [current year]"
- "distributed rate limiting Redis [current year]"

---

## Decision Tree: Rate Limiting Algorithm

```
What's your constraint?
├── Simple per-user limit
│   └── Fixed window (easy, some burst at boundary)
├── Smooth rate enforcement
│   └── Sliding window (accurate, moderate complexity)
├── Allow controlled bursts
│   └── Token bucket (burst-friendly, most flexible)
├── Strict throughput limit
│   └── Leaky bucket (constant rate, no bursts)
└── Distributed system
    └── Redis-based sliding window or token bucket
```

## Rate Limit Design

| Endpoint Type | Limit | Window | Response |
|--------------|-------|--------|----------|
| Login/Signup | 5 attempts | 15 min | 429 + Retry-After |
| Public API | 100 requests | 1 min | 429 + Retry-After |
| Authenticated API | 1000 requests | 1 min | 429 + Retry-After |
| Premium tier | 5000 requests | 1 min | 429 + Retry-After |
| Webhook sending | 10,000/hour | 1 hour | Queue excess |
| File upload | 10 files | 1 hour | 429 |

## Response Headers

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1708200000
Retry-After: 30
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Granularity** | Per-endpoint + per-user + per-tier | Per-user | Global only |
| **Algorithm** | Sliding window or token bucket | Fixed window | No algorithm |
| **Distributed** | Works across multiple servers (Redis) | Single server | Not distributed |
| **Response** | 429 + Retry-After + rate limit headers | 429 only | Generic error |
| **Bypass protection** | IP + user + fingerprint | IP + user | IP only |
| **Tiering** | Different limits per plan/tier | Uniform limits | One size fits all |
| **Monitoring** | Rate limit hit rate, top offenders | Basic tracking | No monitoring |

**28+ = Abuse-resistant | 21-27 = Basic protection | <21 = Vulnerable to abuse**

## Output Protocol

Write to `.claude/outputs/rate-limiting.md`:
- Algorithm chosen
- Limits per endpoint and tier
- Redis key design
- Response format
