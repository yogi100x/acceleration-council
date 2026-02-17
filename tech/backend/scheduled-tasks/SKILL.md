# Scheduled Tasks

Design cron jobs, recurring tasks, and scheduled processing with reliability.

## Research Protocol

### Web Search
- "distributed cron job patterns [current year]"
- "Trigger.dev vs Inngest vs BullMQ scheduled [current year]"

---

## Decision Tree: Scheduling Approach

```
What needs to be scheduled?
├── Simple recurring (daily cleanup, hourly sync)
│   └── Cron expression + distributed lock
├── User-defined schedules (reminders, reports)
│   └── Database-backed scheduler + worker
├── Delayed execution (send email in 30 min)
│   └── Delayed queue job (BullMQ, SQS delay)
├── Complex workflows (multi-step, conditional)
│   └── Workflow engine (Trigger.dev, Temporal)
└── Time-window processing (batch overnight)
    └── Cron + chunked processing + progress tracking
```

## Reliability Patterns

| Problem | Solution |
|---------|----------|
| **Duplicate execution** | Distributed lock (Redis SETNX with TTL) |
| **Missed execution** | Dead-man's switch monitoring, catch-up logic |
| **Long-running task overlaps** | Lock check at start, skip if already running |
| **Timezone handling** | Store all times in UTC, convert at display |
| **DST transitions** | Use cron library that handles DST |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Reliability** | Distributed lock, idempotent, catch-up | Basic locking | No duplicate prevention |
| **Monitoring** | Execution tracking, missed-run alerts | Basic logging | No monitoring |
| **Error handling** | Retry + alert + dead letter | Basic retry | Silent failure |
| **Scalability** | Horizontal workers, partitioned tasks | Single worker | Single server cron |
| **Timezone** | UTC storage, user-timezone display | UTC only | Timezone bugs |
| **Observability** | Run history, duration, success rate | Basic logs | No history |
| **Configuration** | Dynamic schedules, no redeploy needed | Config file | Hardcoded |

**28+ = Reliable scheduling | 21-27 = Mostly works | <21 = Missed/duplicate runs**

## Output Protocol

Write to `.claude/outputs/scheduled-tasks.md`:
- Scheduling approach
- Task list with cron expressions
- Locking strategy
- Monitoring setup
