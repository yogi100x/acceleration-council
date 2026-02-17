# Error Handling

Design consistent error handling patterns across frontend, backend, and infrastructure layers.

## Context Sync Protocol

1. Read existing error handling patterns in the codebase
2. Read monitoring setup for error tracking configuration

## Decision Tree: Error Strategy

```
Where is the error?
├── User input (validation)
│   └── Return structured error with field-level messages
│       → User can fix it. Be specific and helpful.
├── Business logic (domain rules)
│   └── Return domain error with explanation
│       → User may be able to fix it. Explain what's wrong.
├── External service (API, database)
│   └── Retry if transient, fallback if persistent
│       → User sees graceful degradation, not raw errors.
├── Infrastructure (out of memory, disk full)
│   └── Alert operations, serve error page
│       → User sees "temporarily unavailable" message.
└── Unknown / Unexpected
    └── Log everything, alert, serve generic error
        → User sees generic error. Team investigates.
```

## Error Classification

| Category | HTTP Code | Retry? | User Message | Log Level |
|----------|-----------|--------|-------------|-----------|
| **Validation** | 400/422 | No | Specific field errors | WARN |
| **Authentication** | 401 | No | "Please sign in" | INFO |
| **Authorization** | 403 | No | "You don't have access" | WARN |
| **Not found** | 404 | No | "Resource not found" | INFO |
| **Rate limit** | 429 | Yes (after delay) | "Too many requests, try again" | WARN |
| **Conflict** | 409 | No | Explain conflict | WARN |
| **Server error** | 500 | Yes (with backoff) | "Something went wrong" | ERROR |
| **Service unavailable** | 503 | Yes (with backoff) | "Temporarily unavailable" | ERROR |

## Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Please fix the following issues",
    "details": [
      {
        "field": "email",
        "code": "INVALID_FORMAT",
        "message": "Please enter a valid email address"
      }
    ],
    "request_id": "req_abc123"
  }
}
```

## Frontend Error Boundaries

```
Error boundary hierarchy:
1. App-level boundary: catches unexpected errors, shows error page
2. Route-level boundary: catches page errors, shows route-specific error UI
3. Component-level boundary: catches component errors, shows fallback UI
4. Data-fetching boundary: catches API errors, shows retry UI

Each level should:
- Show appropriate UI (not raw error messages)
- Provide recovery action (retry, go back, contact support)
- Report to error tracking (Sentry)
- Not expose internal details to users
```

## Anti-Patterns to Avoid

- **T8: Catching and swallowing errors** — If you catch, log and handle. Never silently ignore.
- **T9: Raw error messages to users** — Stack traces and internal errors are security leaks.
- **T10: No error tracking** — You can't fix errors you don't know about.
- **T11: Inconsistent error formats** — Every API should return the same error structure.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Consistency** | Uniform error format across all endpoints | Mostly consistent | Inconsistent |
| **User experience** | Helpful messages with recovery actions | Generic but clear | Raw errors shown |
| **Error tracking** | All errors reported with context | Key errors tracked | No tracking |
| **Classification** | Errors categorized with appropriate handling | Some categorization | All treated the same |
| **Retry logic** | Appropriate retry for transient errors | Basic retry | No retry |
| **Boundary coverage** | Error boundaries at all levels | Some boundaries | No boundaries |
| **Documentation** | Error codes documented with resolution steps | Some documentation | No error docs |

**28+ = Robust error handling | 21-27 = Gaps in coverage | <21 = Errors leak to users**

## Cross-Skill References

- **Upstream:** `api-design` (error response format), `system-architecture` (error boundaries)
- **Downstream:** `monitoring-setup` (error alerting), `testing-strategy` (error path testing)
- **Council:** Submit to `council-review` for error handling review
