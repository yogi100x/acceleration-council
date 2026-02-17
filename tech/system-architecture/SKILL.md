# System Architecture

Design scalable, maintainable system architectures for software products.

## Context Sync Protocol

1. Read `.claude/product-marketing-context.md` for product scope and user scale
2. Read `.claude/finance-context.md` for infrastructure budget constraints

## Decision Tree: Architecture Pattern

```
What are you building?
├── Single product, small team (<5 devs)
│   └── Modular monolith
│       → Single deployable, clear module boundaries, shared database
├── Multiple products/services, growing team
│   └── Service-oriented architecture
│       → 3-7 services, API gateway, shared auth
├── High scale, large team (>20 devs)
│   └── Microservices
│       → Independent services, event-driven, service mesh
├── Content-heavy, SEO-critical
│   └── Jamstack / Static-first
│       → SSG/SSR, CDN, headless CMS, API layer
└── Real-time, collaborative
    └── Event-driven + WebSockets
        → Message broker, CQRS, real-time sync
```

## Architecture Decision Record (ADR) Template

```markdown
# ADR-NNN: [Decision Title]

## Status: [Proposed | Accepted | Deprecated | Superseded by ADR-NNN]

## Context
What is the issue that we're seeing that is motivating this decision?

## Decision
What is the change we're proposing and/or doing?

## Consequences
What becomes easier or harder because of this change?

## Alternatives Considered
What other options did we evaluate and why did we reject them?
```

## Architecture Layers

| Layer | Responsibility | Technology Choices |
|-------|---------------|-------------------|
| **Presentation** | UI, client-side logic | Next.js, React, mobile frameworks |
| **API** | Request routing, validation, auth | REST, GraphQL, tRPC, gRPC |
| **Business Logic** | Domain rules, workflows | Service layer, domain models |
| **Data Access** | Database queries, caching | ORM/query builder, DAL pattern |
| **Infrastructure** | Hosting, networking, storage | Cloud provider, CDN, object storage |
| **Background** | Async tasks, scheduled jobs | Job queue, cron, event consumers |
| **Observability** | Logging, monitoring, tracing | APM, error tracking, log aggregation |

## Scaling Checklist

- [ ] Stateless services (session stored externally)
- [ ] Database connection pooling
- [ ] CDN for static assets
- [ ] Horizontal scaling capability
- [ ] Rate limiting at API gateway
- [ ] Caching strategy (application, CDN, database)
- [ ] Background job queue for long operations
- [ ] Health checks and readiness probes
- [ ] Graceful shutdown handling

## Anti-Patterns to Avoid

- **T1: Premature microservices** — Start with monolith. Extract services when team/scale demands it.
- **T2: Shared database between services** — Each service owns its data. Communicate via APIs/events.
- **T3: No separation of concerns** — Business logic in controllers, database queries in components.
- **T4: Big bang architecture** — Design for current needs + 1 year. Don't over-architect.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Fit to scale** | Architecture matches current and near-term scale | Somewhat appropriate | Over/under-architected |
| **Separation of concerns** | Clear layers with defined boundaries | Some separation | Tangled responsibilities |
| **Data design** | Normalized, indexed, with clear ownership | Reasonable schema | No data design |
| **Scalability plan** | Identified bottlenecks with mitigation strategies | General scaling awareness | No scaling consideration |
| **Failure handling** | Graceful degradation, circuit breakers, retries | Basic error handling | Fail-open or crash |
| **Security** | Auth, authz, encryption, input validation at boundaries | Basic security | Security as afterthought |
| **Documentation** | ADRs, diagrams, runbooks | Some documentation | No documentation |

**28+ = Production-ready architecture | 21-27 = Needs refinement | <21 = Fundamental issues**

## Cross-Skill References

- **Upstream:** `product-marketing-context` (requirements), `financial-model` (infra budget)
- **Downstream:** All tech skills implement within this architecture
- **Council:** Submit to `council-review` (tech council) for architecture review
