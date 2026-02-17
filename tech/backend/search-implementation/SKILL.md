# Search Implementation

Design search features: full-text, faceted, autocomplete, and relevance ranking.

## Research Protocol

### Web Search
- "PostgreSQL full-text search vs Elasticsearch [current year]"
- "search autocomplete implementation [current year]"
- "Meilisearch vs Typesense vs Algolia comparison [current year]"

### WebFetch
- https://www.postgresql.org/docs/current/textsearch.html

---

## Decision Tree: Search Engine

```
What's your search scale?
├── Small (<100K documents, simple search)
│   └── PostgreSQL full-text search (tsvector + tsquery)
│       → No additional infrastructure, good enough for most
├── Medium (100K-10M, faceted search needed)
│   └── Meilisearch or Typesense (self-hosted or cloud)
│       → Fast, typo-tolerant, faceted, easy setup
├── Large (10M+, complex relevance, analytics)
│   └── Elasticsearch or OpenSearch
│       → Powerful but complex, needs dedicated team
├── E-commerce (product search with filters)
│   └── Algolia or Meilisearch
│       → Facets, filters, analytics, merchandising
└── AI-powered (semantic search, natural language)
    └── Vector search (pgvector, Pinecone) + keyword hybrid
        → Embedding-based similarity + traditional relevance
```

## Search Architecture

```
Indexing pipeline:
  Database change → Event → Indexer → Search engine

Query pipeline:
  User input → Query parser → Search engine → Rank → Filter → Return

Components:
  Autocomplete: Prefix search, debounced (200-300ms)
  Full search: Full-text + filters + facets
  Suggestions: "Did you mean...?" (fuzzy matching)
```

## PostgreSQL FTS Quick Start

```sql
-- Add tsvector column
ALTER TABLE products ADD COLUMN search_vector tsvector
  GENERATED ALWAYS AS (
    setweight(to_tsvector('english', coalesce(name, '')), 'A') ||
    setweight(to_tsvector('english', coalesce(description, '')), 'B')
  ) STORED;

-- Add GIN index
CREATE INDEX idx_products_search ON products USING GIN(search_vector);

-- Query
SELECT * FROM products
WHERE search_vector @@ plainto_tsquery('english', 'wireless headphones')
ORDER BY ts_rank(search_vector, plainto_tsquery('english', 'wireless headphones')) DESC;
```

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T41 | LIKE '%query%' for search | Full table scan, no relevance ranking |
| T42 | No debounce on autocomplete | Hammers search engine on every keystroke |
| T43 | Stale search index | Results don't match current data |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Relevance** | Weighted fields, boosting, custom ranking | Basic ranking | No relevance ranking |
| **Speed** | <100ms p95, debounced autocomplete | <500ms | Slow search |
| **Typo tolerance** | Fuzzy matching, "did you mean" | Some tolerance | Exact match only |
| **Faceted search** | Filters with counts, refinement | Basic filters | No filtering |
| **Index freshness** | Near-real-time sync | Periodic sync | Manual reindex |
| **Autocomplete** | Instant suggestions, highlighted matches | Basic autocomplete | No autocomplete |
| **Analytics** | Search terms tracked, zero-result queries | Basic tracking | No analytics |

**28+ = Great search UX | 21-27 = Functional | <21 = Users can't find things**

## Output Protocol

Write to `.claude/outputs/search-implementation.md`:
- Search engine chosen
- Indexing pipeline design
- Query pipeline design
- Facets and filters
