# Background Job Design

Design reliable background job systems for async processing, scheduled tasks, and long-running operations.

## Context Sync Protocol

1. Read existing background job patterns
2. Read `.claude/product-marketing-context.md` for async processing needs

## Decision Tree: Job Type

```
What needs to happen?
├── Immediate async (user triggered, <30s)
│   └── Queue job, return immediately, poll/notify for result
│       Example: File processing, email sending
├── Scheduled (recurring)
│   └── Cron-style scheduling with idempotent jobs
│       Example: Daily reports, subscription renewal
├── Long-running (minutes to hours)
│   └── Resumable jobs with progress tracking and checkpointing
│       Example: Bulk data import, AI grading
├── Event-driven (react to changes)
│   └── Event queue/pub-sub with ordered processing
│       Example: Webhook processing, data sync
└── Batch processing (large datasets)
    └── Chunked processing with parallelism controls
        Example: Bulk email, data migration
```

## Job Design Principles

### Every Job Must Be

| Principle | Implementation |
|-----------|---------------|
| **Idempotent** | Running twice produces same result |
| **Resumable** | Can restart from last checkpoint on failure |
| **Observable** | Progress, status, and errors are visible |
| **Bounded** | Has a timeout and maximum retry count |
| **Isolated** | One job's failure doesn't affect others |

### Job Lifecycle

```
PENDING → RUNNING → COMPLETED
                 → FAILED → RETRYING → RUNNING
                                     → DEAD (max retries exceeded)
```

### Concurrency Control

| Pattern | When | Implementation |
|---------|------|----------------|
| **Queue-based** | Process in order | FIFO queue, single consumer |
| **Worker pool** | Parallel processing | N workers, shared queue |
| **Rate limited** | External API constraints | Token bucket, leaky bucket |
| **Unique** | Only one instance running | Lock/mutex, unique job key |
| **Priority** | Some jobs more urgent | Priority queue, separate queues |

## Error Handling

```
Retry strategy per job type:
  Transient errors (network, timeout): Retry with backoff
  Permanent errors (validation, auth): Don't retry, alert
  Partial failure: Checkpoint progress, retry remaining

Dead letter queue:
  After max retries, move to dead letter queue
  Alert on-call team
  Provide manual retry capability
  Log full context for debugging
```

## Anti-Patterns to Avoid

- **T3: Jobs without idempotency** — Jobs will be retried. Make them safe to re-run.
- **T5: No timeout** — A stuck job can block the entire queue.
- **T7: No monitoring** — Jobs fail silently without alerting.
- **T8: Unbounded concurrency** — Can overwhelm databases and external APIs.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Idempotency** | All jobs are idempotent with verification | Most jobs | No idempotency |
| **Error handling** | Retry, dead letter, alerting, manual retry | Basic retries | No error handling |
| **Observability** | Progress tracking, status dashboard, logs | Basic logging | No visibility |
| **Concurrency** | Appropriate controls per job type | Global limits | No controls |
| **Scheduling** | Cron with overlap prevention and drift handling | Basic cron | Manual triggering |
| **Scalability** | Horizontal scaling with work distribution | Some scaling | Single worker |
| **Testing** | Job-level unit tests, integration tests | Some testing | No testing |

**28+ = Production-grade | 21-27 = Needs reliability work | <21 = Jobs will fail silently**

## Cross-Skill References

- **Upstream:** `system-architecture` (job infrastructure), `api-design` (trigger points)
- **Downstream:** `monitoring-setup` (job monitoring), `error-handling` (failure patterns)
- **Council:** Submit to `council-review` for reliability review
