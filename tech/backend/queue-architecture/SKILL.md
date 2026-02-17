# Queue Architecture

Design message queue systems for async processing, event-driven architecture, and decoupling.

## Research Protocol

### Web Search
- "message queue comparison Redis vs SQS vs RabbitMQ [current year]"
- "event-driven architecture patterns [current year]"

---

## Decision Tree: Queue Type

```
What's the messaging pattern?
├── Task queue (work to be done)
│   └── Redis Queue (BullMQ) or SQS
│       → Workers pull tasks, process, acknowledge
├── Pub/Sub (events broadcast to many consumers)
│   └── Redis Pub/Sub, Kafka, or SNS
│       → Publisher doesn't know consumers
├── Request/Reply (async RPC)
│   └── Temporary queues with correlation IDs
│       → Caller waits for response on reply queue
├── Ordered processing (events must be in sequence)
│   └── Kafka partitions or SQS FIFO
│       → Partition by entity ID for per-entity ordering
└── Fan-out + Fan-in (parallel processing + aggregation)
    └── Workflow engine (Trigger.dev, Temporal, Step Functions)
        → Orchestrated multi-step processing
```

## Queue Selection

| Queue | Strength | Weakness | Best For |
|-------|----------|----------|----------|
| **Redis (BullMQ)** | Simple, fast, priority queues | No persistence guarantee | Background jobs, small-medium scale |
| **SQS** | Managed, scalable, reliable | No ordering (standard) | AWS workloads, decoupling |
| **Kafka** | High throughput, replay, ordering | Complex ops | Event streaming, large scale |
| **RabbitMQ** | Flexible routing, protocols | Operational overhead | Complex routing patterns |

## Delivery Guarantees

| Guarantee | Implementation | Trade-off |
|-----------|---------------|-----------|
| **At-most-once** | Fire and forget | Fast, may lose messages |
| **At-least-once** | Ack after processing, retry on failure | May duplicate, consumer must be idempotent |
| **Exactly-once** | Idempotency key + at-least-once | Complex, consumer responsible for dedup |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Delivery guarantee** | At-least-once with idempotent consumers | Basic retry | Fire-and-forget |
| **Dead letter queue** | DLQ with alerting and manual retry | DLQ exists | No DLQ |
| **Ordering** | Per-entity ordering where needed | Global ordering | No ordering guarantee |
| **Scaling** | Horizontal consumers, partitioning | Some scaling | Single consumer |
| **Monitoring** | Queue depth, processing rate, lag | Basic metrics | No monitoring |
| **Backpressure** | Consumer rate limiting, circuit breaker | Some limits | No backpressure |
| **Idempotency** | All consumers are idempotent | Most consumers | Not considered |

**28+ = Reliable messaging | 21-27 = Needs hardening | <21 = Message loss risk**

## Output Protocol

Write to `.claude/outputs/queue-architecture.md`:
- Queue technology chosen
- Topics/queues defined
- Delivery guarantee per queue
- Consumer design
