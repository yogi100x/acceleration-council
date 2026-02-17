# Monitoring Setup

Design observability systems covering metrics, logging, tracing, and alerting for production services.

## Context Sync Protocol

1. Read existing monitoring configuration
2. Read `.claude/product-marketing-context.md` for SLA requirements

## Decision Tree: Observability Stack

```
What's your scale?
├── Early stage (<1K users)
│   └── Essential: Error tracking (Sentry) + uptime monitoring + basic logging
├── Growing (1K-100K users)
│   └── Standard: Sentry + APM + structured logging + dashboards
├── Scale (100K+ users)
│   └── Full: Distributed tracing + custom metrics + log aggregation + alerting
└── Enterprise
    └── Complete: All above + SLO tracking + on-call rotation + runbooks
```

## Three Pillars of Observability

### 1. Metrics (What's happening?)

| Metric Type | Examples | Tool |
|------------|---------|------|
| **Infrastructure** | CPU, memory, disk, network | Cloud provider metrics |
| **Application** | Request rate, error rate, latency (RED) | APM (Datadog, New Relic) |
| **Business** | Signups, conversions, revenue | Analytics + custom metrics |
| **SLO** | Availability, latency percentiles | SLO tracking tool |

### 2. Logs (Why did it happen?)

```
Structured logging format:
{
  "timestamp": "2026-02-17T10:30:00Z",
  "level": "error",
  "service": "api",
  "request_id": "req_abc123",
  "user_id": "usr_def456",
  "message": "Payment processing failed",
  "error": "stripe_card_declined",
  "metadata": { "amount": 2500, "currency": "usd" }
}

Log levels:
  ERROR: Something failed, needs attention
  WARN: Something unexpected, may need attention
  INFO: Significant business events
  DEBUG: Detailed diagnostic info (not in production)
```

### 3. Traces (Where in the system?)

```
Distributed tracing tracks requests across services:

User Request → API Gateway → Auth Service → Database → Response
    ├── span: api_gateway (2ms)
    ├── span: auth_check (15ms)
    ├── span: db_query (45ms)
    └── total: 62ms

Essential for:
- Finding slow services in a request chain
- Debugging cross-service failures
- Understanding dependency relationships
```

## Alerting Strategy

| Severity | Examples | Notification | Response |
|----------|---------|--------------|----------|
| **P1 Critical** | Service down, data loss | PagerDuty + phone call | Immediate, 24/7 |
| **P2 High** | Error rate >5%, latency >5s | Slack + PagerDuty | <1 hour, business hours |
| **P3 Medium** | Error rate >1%, degraded performance | Slack | Next business day |
| **P4 Low** | Warning thresholds, capacity planning | Email/dashboard | Weekly review |

**Alert fatigue prevention:**
- Only alert on actionable conditions
- Set meaningful thresholds (not arbitrary)
- Group related alerts
- Auto-resolve when condition clears
- Review alert volume monthly (target: <5 alerts/week)

## Anti-Patterns to Avoid

- **T8: No monitoring in production** — You can't fix what you can't see.
- **T9: Alert on everything** — Alert fatigue leads to ignored alerts. Be selective.
- **T14: No structured logging** — Grep through unstructured logs is painful at scale.
- **T15: No request tracing** — Without trace IDs, debugging cross-service issues is guesswork.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Metrics coverage** | Infrastructure, application, and business metrics | Application metrics | Basic uptime only |
| **Logging** | Structured, searchable, retained appropriately | Semi-structured | Unstructured console.log |
| **Tracing** | Distributed tracing across all services | Some tracing | No tracing |
| **Alerting** | Tiered alerts with appropriate urgency | Basic alerts | No alerts |
| **Dashboards** | Service health, business metrics, SLOs visible | Some dashboards | No dashboards |
| **Runbooks** | Alert-specific runbooks for on-call | Some documentation | No runbooks |
| **Cost management** | Log retention policies, metric cardinality managed | Some cost awareness | Unbounded costs |

**28+ = Production-grade observability | 21-27 = Blind spots | <21 = Flying blind**

## Cross-Skill References

- **Upstream:** `system-architecture` (what to monitor), `ci-cd-pipeline` (deploy monitoring)
- **Downstream:** `incident-response-plan` (triggered by alerts), `performance-optimization` (guided by metrics)
- **Council:** Submit to `council-review` for observability review
