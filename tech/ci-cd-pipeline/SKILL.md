# CI/CD Pipeline

Design continuous integration and deployment pipelines for reliable, fast software delivery.

## Context Sync Protocol

1. Read existing CI/CD configuration
2. Read `.claude/product-marketing-context.md` for deployment requirements

## Decision Tree: Pipeline Architecture

```
What's your setup?
├── Single app, small team
│   └── GitHub Actions with simple workflow
│       → lint → test → build → deploy
├── Monorepo, multiple apps
│   └── Turborepo + GitHub Actions with affected detection
│       → Only build/test/deploy changed packages
├── Microservices
│   └── Per-service pipelines with shared workflows
│       → Independent deploy, integration tests
└── Enterprise / Regulated
    └── Multi-stage with approvals
        → Dev → Staging → UAT → Production (with gates)
```

## Pipeline Stages

| Stage | Purpose | Fail = Block? | Typical Duration |
|-------|---------|---------------|-----------------|
| **Lint** | Code style, static analysis | Yes | 30s-2min |
| **Type check** | TypeScript compilation | Yes | 1-3min |
| **Unit tests** | Business logic verification | Yes | 1-5min |
| **Build** | Compilation, bundling | Yes | 2-10min |
| **Integration tests** | API, database tests | Yes | 3-10min |
| **E2E tests** | Full user flow tests | Warn (flaky tolerance) | 5-20min |
| **Security scan** | Dependency vulnerabilities | Block on critical | 1-3min |
| **Deploy to staging** | Staging environment update | Yes | 2-5min |
| **Deploy to production** | Production release | Yes (manual gate optional) | 2-5min |

## Deployment Strategies

| Strategy | Risk | Rollback Speed | Best For |
|----------|------|----------------|----------|
| **Direct deploy** | High | Slow (redeploy) | Internal tools |
| **Blue-green** | Low | Instant (switch) | Stateless services |
| **Canary** | Low | Fast (route away) | High-traffic services |
| **Rolling** | Medium | Medium | Containerized services |
| **Feature flags** | Very low | Instant (toggle) | Any, especially UI |

## Caching Strategy

```
Cache these for faster CI:
- node_modules (key: package-lock.json hash)
- Build output (.next, dist) (key: source hash)
- Turborepo remote cache (for monorepos)
- Docker layers (for containerized builds)
- Test results (for unchanged code)
```

## Anti-Patterns to Avoid

- **T3: No CI/CD** — Manual deployments are error-prone and slow.
- **T6: Flaky tests blocking deploys** — Quarantine flaky tests, don't disable CI.
- **T7: No staging environment** — Test in production-like environment before deploying.
- **T15: Slow pipelines** — If CI takes >15 minutes, developers won't wait. Parallelize and cache.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Coverage** | Lint, type check, test, build, deploy all automated | Key stages automated | Partial automation |
| **Speed** | <10 minutes total | <20 minutes | >20 minutes |
| **Reliability** | <1% flaky failure rate | <5% flaky | Frequently broken |
| **Caching** | Dependencies, builds, and test results cached | Some caching | No caching |
| **Rollback** | One-click rollback with verification | Manual rollback possible | No rollback plan |
| **Security** | Secrets management, vulnerability scanning | Basic secrets handling | Secrets in code |
| **Monitoring** | Deploy notifications, pipeline metrics | Basic notifications | No visibility |

**28+ = Fast and reliable | 21-27 = Needs optimization | <21 = Delivery bottleneck**

## Cross-Skill References

- **Upstream:** `system-architecture` (deployment architecture), `testing-strategy` (what to test)
- **Downstream:** `monitoring-setup` (post-deploy monitoring)
- **Council:** Submit to `council-review` for pipeline review
