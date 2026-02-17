# Database Schema

Design normalized, performant database schemas with proper indexing and constraints.

## Context Sync Protocol

1. Read existing database schema and conventions
2. Read `.claude/product-marketing-context.md` for data requirements

## Decision Tree: Database Choice

```
What's your data model?
├── Relational (structured, joins needed)
│   └── PostgreSQL (most versatile, JSONB for semi-structured)
├── Document (flexible schema, nested objects)
│   └── MongoDB or PostgreSQL JSONB columns
├── Key-value (simple lookups, caching)
│   └── Redis, DynamoDB
├── Time-series (events, metrics, logs)
│   └── TimescaleDB (PostgreSQL extension) or InfluxDB
├── Graph (relationships are the data)
│   └── Neo4j, or PostgreSQL with recursive CTEs
└── Search (full-text, faceted)
    └── Elasticsearch, or PostgreSQL full-text search
```

## Schema Design Principles

### Normalization Guidelines
```
Start at 3NF (Third Normal Form):
- 1NF: No repeating groups, atomic values
- 2NF: No partial dependencies on composite keys
- 3NF: No transitive dependencies

Denormalize strategically:
- Materialized views for read-heavy queries
- JSONB columns for flexible/nested data
- Computed columns for frequently calculated values
- Only when query performance demands it with profiling evidence
```

### Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Tables | snake_case, plural | `candidate_profiles` |
| Columns | snake_case | `created_at`, `first_name` |
| Primary key | `id` (UUID preferred) | `id UUID DEFAULT gen_random_uuid()` |
| Foreign key | `referenced_table_singular_id` | `organization_id` |
| Boolean | `is_` or `has_` prefix | `is_active`, `has_verified` |
| Timestamps | `_at` suffix | `created_at`, `updated_at` |
| Enums | snake_case | `assessment_status` |
| Indexes | `idx_table_columns` | `idx_candidates_email` |

### Essential Patterns

```sql
-- Every table should have:
CREATE TABLE resources (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- ... columns
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Auto-update updated_at
CREATE TRIGGER set_updated_at
  BEFORE UPDATE ON resources
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();

-- Soft delete (when needed)
ALTER TABLE resources ADD COLUMN deleted_at TIMESTAMPTZ;
CREATE INDEX idx_resources_active ON resources (id) WHERE deleted_at IS NULL;
```

### Indexing Strategy

| Index Type | When | Example |
|-----------|------|---------|
| **B-tree** (default) | Equality, range, sorting | `CREATE INDEX idx_users_email ON users(email)` |
| **Partial** | Filtering on subset | `CREATE INDEX idx_active ON users(id) WHERE is_active = true` |
| **Composite** | Multi-column queries | `CREATE INDEX idx_attempts ON attempts(user_id, assessment_id)` |
| **GIN** | JSONB, array, full-text | `CREATE INDEX idx_skills ON profiles USING GIN(skills)` |
| **Unique** | Enforce uniqueness | `CREATE UNIQUE INDEX idx_email ON users(email)` |

## Anti-Patterns to Avoid

- **T2: No foreign key constraints** — Enforce referential integrity at the database level.
- **T5: EAV (Entity-Attribute-Value) pattern** — Use JSONB columns instead.
- **T9: No indexes on foreign keys** — Every FK column should be indexed for JOIN performance.
- **T10: UUID as primary key without b-tree consideration** — UUIDv7 is ordered; prefer over UUIDv4 for index performance.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Normalization** | 3NF with strategic denormalization documented | Mostly normalized | Significant redundancy |
| **Constraints** | PKs, FKs, unique, check constraints, not-null | PKs and FKs | PKs only |
| **Indexing** | Query-driven indexes with partial and composite | Basic indexes | No indexes beyond PK |
| **Naming** | Consistent conventions throughout | Mostly consistent | Inconsistent |
| **Migration safety** | Non-breaking migrations, backfill strategy | Basic migrations | Ad-hoc schema changes |
| **Data types** | Appropriate types (timestamptz, UUID, enum) | Mostly appropriate | Strings for everything |
| **Documentation** | Schema diagram, column descriptions, relationship docs | Some documentation | No documentation |

**28+ = Production-ready schema | 21-27 = Needs review | <21 = Data integrity risk**

## Cross-Skill References

- **Upstream:** `system-architecture` (data layer design)
- **Downstream:** `data-migration` (schema changes), `performance-optimization` (query tuning)
- **Council:** Submit to `council-review` for schema review
