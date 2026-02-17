# Data Migration

Plan and execute safe database migrations with zero downtime and rollback capability.

## Context Sync Protocol

1. Read existing migration patterns and conventions
2. Read database schema documentation

## Decision Tree: Migration Type

```
What kind of change?
├── Add column (nullable or with default)
│   └── Safe: Deploy migration, then code changes
├── Add column (NOT NULL, no default)
│   └── Multi-step: Add nullable → backfill → add constraint
├── Remove column
│   └── Multi-step: Stop reading → stop writing → drop column
├── Rename column
│   └── Multi-step: Add new → copy data → update code → drop old
├── Add table
│   └── Safe: Deploy migration, then code changes
├── Drop table
│   └── Multi-step: Stop all references → backup → drop
├── Change data type
│   └── Multi-step: Add new column → migrate data → swap → drop old
└── Large data backfill
    └── Batched: Process in chunks, resumable, monitored
```

## Safe Migration Pattern

### The Expand-Contract Pattern

```
Phase 1: EXPAND (backward compatible)
  - Add new column/table
  - Deploy code that writes to both old and new
  - Backfill existing data

Phase 2: MIGRATE (transition)
  - Deploy code that reads from new
  - Verify data consistency
  - Monitor for issues

Phase 3: CONTRACT (cleanup)
  - Deploy code that stops writing to old
  - Drop old column/table
  - Clean up migration code
```

### Backfill Strategy

```sql
-- Batched backfill (safe for large tables)
DO $$
DECLARE
  batch_size INT := 1000;
  rows_updated INT;
BEGIN
  LOOP
    UPDATE target_table
    SET new_column = computed_value
    WHERE new_column IS NULL
    LIMIT batch_size;

    GET DIAGNOSTICS rows_updated = ROW_COUNT;
    EXIT WHEN rows_updated = 0;

    PERFORM pg_sleep(0.1); -- Reduce lock pressure
    RAISE NOTICE 'Updated % rows', rows_updated;
  END LOOP;
END $$;
```

## Migration Safety Checklist

- [ ] Migration is backward compatible (old code works with new schema)
- [ ] Migration is idempotent (safe to run twice)
- [ ] Rollback plan documented and tested
- [ ] Large tables: migration won't lock table for extended period
- [ ] Backfill runs in batches (not single UPDATE)
- [ ] Migration tested on staging with production-like data volume
- [ ] Monitoring in place for migration progress and errors

## Anti-Patterns to Avoid

- **T2: Breaking schema change without expand-contract** — Will cause downtime during deploy.
- **T5: Unbatched backfills on large tables** — Will lock the table and block reads/writes.
- **T8: No rollback plan** — Every migration needs a documented rollback procedure.
- **T9: Testing only on empty databases** — Test with production-scale data volumes.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Backward compatibility** | All migrations are backward compatible | Most are | Breaking changes |
| **Rollback plan** | Tested rollback for every migration | Rollback documented | No rollback |
| **Zero downtime** | No table locks, batched operations | Brief locks acceptable | Extended downtime |
| **Data validation** | Pre and post migration data verification | Basic checks | No validation |
| **Monitoring** | Progress tracking, error alerting during migration | Some monitoring | No monitoring |
| **Testing** | Tested on production-scale data | Tested on staging | Tested on dev only |
| **Documentation** | Migration plan with steps, timing, and rollback | Basic documentation | No documentation |

**28+ = Safe migration | 21-27 = Needs review | <21 = Data loss risk**

## Cross-Skill References

- **Upstream:** `database-schema` (schema design), `system-architecture` (deployment strategy)
- **Downstream:** `monitoring-setup` (migration monitoring)
- **Council:** Submit to `council-review` for migration review
