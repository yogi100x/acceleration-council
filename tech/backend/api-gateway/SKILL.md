# API Gateway

Design API gateway patterns for routing, load balancing, and cross-cutting concerns.

## Research Protocol

### Web Search
- "API gateway patterns [current year]"
- "Kong vs Traefik vs AWS API Gateway comparison [current year]"

---

## Decision Tree: Gateway Approach

```
What's your architecture?
├── Single app, single domain
│   └── No gateway needed — use middleware
├── Multiple services, single domain
│   └── Reverse proxy (Nginx, Caddy, Traefik)
│       → Route by path prefix to services
├── Microservices with cross-cutting concerns
│   └── Full API gateway (Kong, AWS API Gateway)
│       → Auth, rate limiting, transforms, monitoring
├── Multi-region or edge
│   └── Edge gateway (Cloudflare Workers, Vercel Edge)
│       → Lowest latency, geo-routing
└── GraphQL federation
    └── GraphQL gateway (Apollo Router)
        → Compose multiple GraphQL services
```

## Gateway Responsibilities

| Concern | Implementation |
|---------|---------------|
| **Routing** | Path-based, header-based, or query-based routing |
| **Authentication** | Verify JWT/API key before forwarding |
| **Rate limiting** | Per-user, per-endpoint limits |
| **Load balancing** | Round-robin, least connections, weighted |
| **Circuit breaker** | Open circuit on repeated failures |
| **Request transform** | Add headers, modify body, version translation |
| **Response caching** | Cache GET responses at edge |
| **Logging** | Centralized request logging with correlation IDs |
| **CORS** | Handle preflight and CORS headers |
| **SSL termination** | Terminate TLS at gateway |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Routing** | Dynamic, path/header/query routing | Path-based | Hardcoded |
| **Auth** | Gateway validates, services trust | Per-service auth | No auth |
| **Rate limiting** | Gateway-level + service-level | Gateway only | None |
| **Resilience** | Circuit breaker, retry, timeout | Basic timeout | No resilience |
| **Observability** | Centralized logging, tracing, metrics | Basic logging | No gateway logging |
| **Performance** | <5ms added latency, caching | <20ms added | Significant overhead |
| **Deployment** | Independent of services, hot reload | Restart required | Tightly coupled |

**28+ = Solid gateway | 21-27 = Basic routing | <21 = Missing cross-cutting concerns**

## Output Protocol

Write to `.claude/outputs/api-gateway.md`:
- Gateway technology chosen
- Routing rules
- Cross-cutting concerns implemented
- Service registry
