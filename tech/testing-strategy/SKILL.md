# Testing Strategy

Design comprehensive testing strategies covering unit, integration, and end-to-end tests.

## Context Sync Protocol

1. Read existing test patterns and coverage
2. Read `.claude/product-marketing-context.md` for critical user flows

## Decision Tree: What to Test

```
What level of testing?
├── Unit tests (isolated logic)
│   └── Business logic, utility functions, data transformations
│       → Fast, many, cheap. Mock external dependencies.
├── Integration tests (component interactions)
│   └── API routes + database, service interactions
│       → Medium speed, test real integrations. Mock external APIs.
├── E2E tests (full user flows)
│   └── Critical paths: signup, purchase, core workflows
│       → Slow, few, expensive. Test the most important flows.
└── Other
    ├── Performance tests → Load testing, stress testing
    ├── Security tests → Penetration testing, vulnerability scanning
    └── Visual regression → Screenshot comparison
```

## The Testing Pyramid

```
        /  E2E   \     5-10 tests (critical paths)
       /  (slow)  \
      / Integration \   20-50 tests (API + DB)
     /   (medium)    \
    /    Unit tests    \ 100+ tests (business logic)
   /     (fast)         \
```

**Not the inverted pyramid:** Don't have more E2E tests than unit tests. E2E is for confidence, not coverage.

## What to Test (Priority Order)

| Priority | Test Target | Method | Coverage Goal |
|----------|------------|--------|--------------|
| **P0** | Authentication flows | E2E | 100% of auth paths |
| **P0** | Payment/billing | Integration + E2E | 100% of payment paths |
| **P1** | Core user workflows | E2E | Top 5 user journeys |
| **P1** | Business logic | Unit | >80% of domain logic |
| **P1** | API endpoints | Integration | All endpoints |
| **P2** | Error handling | Unit + Integration | Error paths |
| **P2** | Edge cases | Unit | Boundary conditions |
| **P3** | UI components | Unit (snapshot) | Key components |
| **P3** | Performance | Load test | Key endpoints |

## Test Design Patterns

### Arrange-Act-Assert
```
test('creates user with valid data', async () => {
  // Arrange
  const userData = { email: 'test@example.com', name: 'Test' }
  
  // Act
  const result = await createUser(userData)
  
  // Assert
  expect(result.email).toBe('test@example.com')
  expect(result.id).toBeDefined()
})
```

### Test Fixtures
```
Use factories for test data:
- Consistent, realistic data
- Isolated per test (no shared state)
- Easy to customize for specific scenarios
```

## Anti-Patterns to Avoid

- **T1: No tests** — Even minimal tests catch regressions.
- **T3: Testing implementation, not behavior** — Tests should verify what, not how.
- **T6: Flaky tests** — Fix or quarantine. Flaky tests erode trust in the suite.
- **T15: 100% coverage target** — Diminishing returns. Focus on critical paths.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Strategy** | Testing pyramid with clear priorities | Some test organization | Random test collection |
| **Coverage** | Critical paths covered, >80% business logic | Key paths covered | Minimal coverage |
| **Speed** | Full suite <5 minutes, fast feedback | <15 minutes | >15 minutes |
| **Reliability** | <1% flaky rate | <5% flaky | Frequently flaky |
| **Maintainability** | DRY fixtures, clear naming, easy to add tests | Somewhat organized | Brittle, hard to maintain |
| **CI integration** | Tests run on every PR, block merging | Tests in CI | No CI integration |
| **Data isolation** | Each test is independent, no shared state | Mostly isolated | Tests depend on each other |

**28+ = Confident in deployments | 21-27 = Known gaps | <21 = Shipping blind**

## Cross-Skill References

- **Upstream:** `system-architecture` (testability), `api-design` (what to test)
- **Downstream:** `ci-cd-pipeline` (running tests), `monitoring-setup` (production validation)
- **Council:** Submit to `council-review` for testing strategy review
