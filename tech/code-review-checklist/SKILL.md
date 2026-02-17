# Code Review Checklist

Systematic code review process that catches bugs, enforces standards, and shares knowledge.

## Context Sync Protocol

1. Read existing code conventions and patterns
2. Read `.claude/product-marketing-context.md` for quality standards

## Decision Tree: Review Depth

```
What's the PR size?
├── Small (<50 lines, single concern)
│   └── Quick review: correctness, style, tests
├── Medium (50-300 lines, 1-3 files)
│   └── Standard review: full checklist below
├── Large (300+ lines, many files)
│   └── Request split into smaller PRs, or deep review with extra time
└── Critical (auth, billing, data migration)
    └── Double review: two reviewers, security-focused
```

## Review Checklist

### Correctness
- [ ] Logic is correct for all input cases
- [ ] Edge cases handled (null, empty, boundary values)
- [ ] Error handling is appropriate (not swallowed, not over-caught)
- [ ] Race conditions considered (concurrent access, async operations)
- [ ] Data integrity maintained (transactions where needed)

### Security
- [ ] No hardcoded secrets or credentials
- [ ] Input validation at trust boundaries
- [ ] Authorization checks on all protected resources
- [ ] No SQL injection, XSS, or CSRF vulnerabilities
- [ ] Sensitive data not logged or exposed in errors

### Performance
- [ ] No N+1 queries
- [ ] Database queries have appropriate indexes
- [ ] No unbounded list queries (pagination)
- [ ] Heavy operations offloaded to background jobs
- [ ] No unnecessary re-renders (React)

### Maintainability
- [ ] Code is readable without comments (self-documenting)
- [ ] Functions/methods have single responsibility
- [ ] No code duplication (DRY, but not premature abstraction)
- [ ] Naming is clear and consistent
- [ ] Follows existing codebase conventions

### Testing
- [ ] New code has tests
- [ ] Tests cover happy path and error paths
- [ ] Tests are independent and deterministic
- [ ] No testing of implementation details

### Documentation
- [ ] Complex logic has inline comments explaining WHY
- [ ] Public APIs have documentation
- [ ] Breaking changes documented in PR description
- [ ] Migration guide for breaking changes

## Review Etiquette

| Do | Don't |
|----|-------|
| Ask questions ("Why did you choose...?") | Make demands ("Change this to...") |
| Suggest alternatives with reasoning | Block on style preferences |
| Distinguish blocking vs non-blocking feedback | Leave vague comments ("This could be better") |
| Acknowledge good work | Only point out problems |
| Review promptly (<24 hours) | Let PRs sit for days |

## Feedback Classification

```
Use prefixes to classify feedback:
  [blocking] Must fix before merge
  [suggestion] Consider this improvement
  [nit] Minor style/formatting (non-blocking)
  [question] Seeking understanding
  [praise] Calling out good work
```

## Anti-Patterns to Avoid

- **T1: Rubber-stamp reviews** — Actually read the code. Fast approvals miss bugs.
- **T4: Bikeshedding** — Don't spend 30 minutes debating variable names. Focus on logic and architecture.
- **T7: Gatekeeping** — Reviews should enable shipping, not block it.
- **T12: No review for "small changes"** — Small changes cause big outages. Always review.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Thoroughness** | All checklist items verified | Key items checked | Cursory glance |
| **Feedback quality** | Specific, actionable, classified by priority | Somewhat specific | Vague or unhelpful |
| **Turnaround** | <24 hours, <4 hours for urgent | <48 hours | Multi-day delays |
| **Knowledge sharing** | Explains reasoning, teaches patterns | Some teaching | Directives only |
| **Consistency** | Same standards applied to all PRs | Mostly consistent | Varies by reviewer |
| **Risk awareness** | Extra scrutiny on critical paths | Some awareness | Same depth for all |
| **Follow-up** | Re-review after changes, verify resolution | Some follow-up | Approve without re-review |

**28+ = High-quality review process | 21-27 = Needs consistency | <21 = Reviews not adding value**

## Cross-Skill References

- **Upstream:** `testing-strategy` (tests in PRs), `documentation` (docs in PRs)
- **Parallel:** All tech skills define what to look for in reviews
- **Council:** Complex PRs can be submitted to `council-review` for deep review
