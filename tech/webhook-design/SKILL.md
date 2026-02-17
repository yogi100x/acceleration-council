# Webhook Design

Design reliable webhook systems for both sending and receiving events.

## Context Sync Protocol

1. Read existing webhook implementations
2. Read `.claude/product-marketing-context.md` for integration requirements

## Decision Tree: Webhook Role

```
Are you sending or receiving?
├── Receiving webhooks (from Stripe, GitHub, etc.)
│   └── Focus: Signature verification, idempotency, async processing
├── Sending webhooks (to customers/integrations)
│   └── Focus: Delivery guarantees, retry strategy, payload design
└── Both
    └── Apply both patterns
```

## Receiving Webhooks

### Processing Pattern

```
1. Receive request
2. Verify signature (HMAC-SHA256 typical)
3. Return 200 immediately (process async)
4. Check idempotency (have we processed this event ID before?)
5. Process event in background job
6. Record result

Never:
- Do heavy processing synchronously (webhook sender will timeout)
- Return non-2xx before processing (will trigger retry)
- Skip signature verification (anyone can POST to your endpoint)
```

### Idempotency

```
Every webhook handler must be idempotent:

1. Store processed event IDs (event_id + provider)
2. Before processing, check if already processed
3. If duplicate, return 200 (success) without reprocessing
4. Use database transactions to prevent race conditions

CREATE TABLE webhook_events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  provider TEXT NOT NULL,
  event_id TEXT NOT NULL,
  event_type TEXT NOT NULL,
  processed_at TIMESTAMPTZ DEFAULT now(),
  payload JSONB,
  UNIQUE(provider, event_id)
);
```

## Sending Webhooks

### Delivery Guarantees

| Guarantee | Implementation | Use Case |
|-----------|---------------|----------|
| **At-most-once** | Fire and forget | Non-critical notifications |
| **At-least-once** | Retry on failure (with idempotency key) | Most use cases |
| **Exactly-once** | At-least-once + receiver idempotency | Financial transactions |

### Retry Strategy

```
Exponential backoff with jitter:
  Attempt 1: Immediate
  Attempt 2: 1 minute + jitter
  Attempt 3: 5 minutes + jitter
  Attempt 4: 30 minutes + jitter
  Attempt 5: 2 hours + jitter
  Attempt 6: 8 hours + jitter
  
After all retries: Mark as failed, alert, allow manual retry

Jitter: random(0, 0.5 × delay) to prevent thundering herd
```

### Payload Design

```json
{
  "id": "evt_abc123",
  "type": "candidate.verified",
  "created_at": "2026-02-17T10:30:00Z",
  "data": {
    "candidate_id": "cand_def456",
    "assessment": "react-senior",
    "score": 92
  },
  "api_version": "2026-02-01"
}
```

## Anti-Patterns to Avoid

- **T6: No signature verification** — Anyone can fake a webhook. Always verify.
- **T7: Synchronous processing** — Process async. Return 200 fast.
- **T8: No idempotency** — Webhooks are delivered at-least-once. Handle duplicates.
- **T9: No retry mechanism** — Network failures happen. Implement retries with backoff.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Signature verification** | HMAC verification on all incoming webhooks | Most verified | No verification |
| **Idempotency** | Event deduplication with database tracking | Basic dedup | No dedup |
| **Async processing** | Background job for all webhook processing | Most async | Synchronous |
| **Retry strategy** | Exponential backoff with jitter, configurable | Basic retries | No retries |
| **Monitoring** | Delivery success rates, latency, failure alerts | Basic logging | No monitoring |
| **Documentation** | Event catalog, payload schemas, integration guide | Basic docs | No docs |
| **Testing** | Webhook simulation, replay capability | Some testing | Manual only |

**28+ = Reliable integration | 21-27 = Needs hardening | <21 = Data loss risk**

## Cross-Skill References

- **Upstream:** `api-design` (endpoint design), `system-architecture` (event architecture)
- **Downstream:** `monitoring-setup` (webhook monitoring), `error-handling` (failure handling)
- **Council:** Submit to `council-review` for reliability review
