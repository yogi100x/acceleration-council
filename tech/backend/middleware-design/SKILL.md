# Middleware Design

Design request processing pipelines: auth, validation, logging, rate limiting, error handling.

## Research Protocol

### Web Search
- "middleware patterns [current year]"
- "Express vs Hono vs Fastify middleware comparison [current year]"

---

## Decision Tree: Middleware Stack

```
Standard middleware order:
1. Request ID (correlation)
2. Logging (request start)
3. CORS
4. Rate limiting
5. Authentication
6. Authorization
7. Input validation
8. → Route handler ←
9. Error handling
10. Logging (request end)
11. Response formatting
```

## Middleware Patterns

| Pattern | Purpose | Implementation |
|---------|---------|----------------|
| **Chain of Responsibility** | Each middleware decides to pass or reject | Express/Hono `next()` pattern |
| **Guard** | Block unauthorized requests early | Auth check before route handler |
| **Transformer** | Modify request/response | Parse body, format response, add headers |
| **Observer** | Side effects without modifying flow | Logging, analytics, monitoring |
| **Circuit Breaker** | Stop calling failing services | Track failures, open circuit, fallback |

## Common Middleware

```
// Request ID
(req, res, next) → req.id = generateId() → next()

// Auth
(req, res, next) → verify JWT → attach user → next() or 401

// Rate limit
(req, res, next) → check rate → next() or 429

// Validation
(req, res, next) → validate with Zod → next() or 400

// Error handler (last in chain)
(err, req, res, next) → log error → format response → send
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Order** | Correct execution order, early rejection | Mostly correct | Unordered |
| **Auth** | JWT verification + authorization checks | Basic auth | No middleware auth |
| **Validation** | Zod schemas on all inputs | Some validation | No validation |
| **Error handling** | Centralized, structured errors | Per-route errors | Unhandled exceptions |
| **Logging** | Structured logs with request ID, timing | Basic logging | No logging |
| **Rate limiting** | Per-endpoint + per-user limits | Global limit | No rate limiting |
| **Reusability** | Composable, configurable middleware | Some reuse | Duplicated logic |

**28+ = Clean request pipeline | 21-27 = Gaps in pipeline | <21 = Security holes**

## Output Protocol

Write to `.claude/outputs/middleware-design.md`:
- Middleware stack order
- Auth middleware configuration
- Validation approach
- Error handling strategy
