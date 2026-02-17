# API Design

Design consistent, intuitive, and well-documented APIs for internal and external consumption.

## Context Sync Protocol

1. Read existing API patterns in the codebase
2. Read `.claude/product-marketing-context.md` for API consumers and use cases

## Decision Tree: API Style

```
Who consumes the API?
├── Internal frontend only
│   └── REST or tRPC (type-safe, simpler)
├── External developers
│   └── REST with OpenAPI spec (industry standard, well-tooled)
├── Mobile apps
│   └── REST or GraphQL (flexible queries, bandwidth optimization)
├── Microservices communication
│   └── gRPC (performance) or REST (simplicity)
└── Real-time features
    └── WebSockets or Server-Sent Events
```

## REST API Design Principles

### URL Structure
```
GET    /api/v1/resources          # List
GET    /api/v1/resources/:id      # Get one
POST   /api/v1/resources          # Create
PUT    /api/v1/resources/:id      # Full update
PATCH  /api/v1/resources/:id      # Partial update
DELETE /api/v1/resources/:id      # Delete

Nested resources:
GET    /api/v1/users/:id/orders   # User's orders

Filtering:
GET    /api/v1/resources?status=active&sort=-created_at&limit=20&offset=0
```

### Response Format
```json
{
  "data": { ... },
  "meta": {
    "total": 100,
    "page": 1,
    "per_page": 20
  }
}

// Error response
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      { "field": "email", "message": "Must be a valid email address" }
    ]
  }
}
```

### HTTP Status Codes

| Code | When to Use |
|------|-------------|
| 200 | Success (GET, PUT, PATCH, DELETE) |
| 201 | Created (POST) |
| 204 | No content (DELETE with no body) |
| 400 | Bad request (validation error) |
| 401 | Unauthorized (no/invalid auth) |
| 403 | Forbidden (valid auth, no permission) |
| 404 | Not found |
| 409 | Conflict (duplicate, state conflict) |
| 422 | Unprocessable entity (semantic validation) |
| 429 | Rate limited |
| 500 | Server error |

## Versioning Strategy

| Strategy | Pros | Cons | Best For |
|----------|------|------|----------|
| **URL path** (`/v1/`) | Clear, easy to route | URL pollution | External APIs |
| **Header** (`Accept: application/vnd.api.v1+json`) | Clean URLs | Hidden, harder to test | Internal APIs |
| **Query param** (`?version=1`) | Simple | Easy to forget | Low-traffic APIs |
| **No versioning** | Simplest | Breaking changes are hard | Internal-only |

## Anti-Patterns to Avoid

- **T5: Inconsistent API naming** — Pick a convention (camelCase vs snake_case) and stick to it.
- **T6: No pagination** — Unbounded list endpoints will crash at scale.
- **T7: Leaking internal IDs** — Consider UUIDs or opaque IDs for external APIs.
- **T8: No rate limiting** — Every public API needs rate limiting.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Consistency** | Uniform naming, response format, error handling | Mostly consistent | Inconsistent |
| **Documentation** | OpenAPI spec with examples | Basic docs | No docs |
| **Error handling** | Structured errors with actionable messages | Error codes | Generic 500s |
| **Pagination** | Cursor or offset pagination on all lists | Some endpoints paginated | No pagination |
| **Authentication** | Secure auth with appropriate granularity | Basic auth | No auth |
| **Versioning** | Clear strategy with migration path | Version exists | No versioning |
| **Rate limiting** | Per-endpoint or per-tier limits | Global rate limit | No rate limiting |

**28+ = Developer-friendly API | 21-27 = Needs polish | <21 = Will cause integration issues**

## Cross-Skill References

- **Upstream:** `system-architecture` (API layer design), `auth-design` (authentication)
- **Downstream:** `documentation` (API docs), `testing-strategy` (API tests)
- **Council:** Submit to `council-review` for API design review
