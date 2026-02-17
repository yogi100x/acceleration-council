# Documentation

Create and maintain technical documentation that helps developers understand and contribute to the codebase.

## Context Sync Protocol

1. Read existing documentation structure
2. Read `.claude/product-marketing-context.md` for audience and scope

## Decision Tree: Documentation Type

```
What needs documenting?
├── Getting started (new developer)
│   └── README: Prerequisites, install, run, key concepts
├── Architecture (understanding the system)
│   └── ADRs, system diagrams, data flow, module descriptions
├── API (consuming the API)
│   └── OpenAPI spec, endpoint reference, authentication, examples
├── Operations (running in production)
│   └── Runbooks, deployment guide, monitoring, incident response
├── Contributing (making changes)
│   └── Code conventions, PR process, testing requirements
└── End user (using the product)
    └── User guides, FAQ, tutorials, API reference
```

## Documentation Structure

```
docs/
├── README.md              # Quick start, project overview
├── CONVENTIONS.md          # Code style, patterns, naming
├── DATABASE.md             # Schema reference, migrations
├── TROUBLESHOOTING.md      # Common issues and solutions
├── AUTH_PATTERNS.md         # Authentication/authorization guide
├── DEPLOYMENT_GUIDE.md      # Deployment procedures
├── API.md                   # API reference (or auto-generated)
└── internal_docs/           # Working documents, design docs
```

## Documentation Quality Principles

| Principle | Implementation |
|-----------|---------------|
| **Current** | Update docs when code changes. Stale docs are worse than no docs. |
| **Findable** | Clear navigation, search-friendly, linked from relevant code |
| **Scannable** | Headers, tables, code blocks. Don't write essays. |
| **Actionable** | Every doc answers "what do I do?" not just "what is this?" |
| **Tested** | Code examples should be runnable. Test them in CI if possible. |
| **Audience-aware** | Write for the reader, not the writer. New developer vs expert. |

## What NOT to Document

- Implementation details that change frequently (read the code instead)
- Obvious code (don't add `// increment counter` above `counter++`)
- Information available in git history (use `git blame` for "why was this changed")
- Aspirational architecture (document what IS, not what you wish it was)

## Anti-Patterns to Avoid

- **T12: No documentation** — At minimum: README, getting started, key architectural decisions.
- **T13: Documentation that duplicates code** — Document the WHY, not the WHAT. Code shows what.
- **T14: Unmaintained docs** — Outdated docs mislead. Better to delete than leave stale.
- **T15: Documentation as afterthought** — Write docs alongside code, not after.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Coverage** | All key areas documented (start, architecture, API, ops) | Key areas covered | README only |
| **Accuracy** | All docs match current code | Mostly accurate | Outdated |
| **Findability** | Clear structure, cross-linked, searchable | Some organization | Scattered files |
| **Onboarding** | New dev productive in <1 day | <1 week | Multi-week ramp-up |
| **Maintainability** | Docs updated with code changes, review in PRs | Occasionally updated | Never updated |
| **Examples** | Runnable code examples for key patterns | Some examples | No examples |
| **Audience fit** | Tailored to reader level (beginner vs expert) | One level | Too technical or too basic |

**28+ = Developer-friendly | 21-27 = Has gaps | <21 = Documentation debt**

## Cross-Skill References

- **Upstream:** All skills produce artifacts that should be documented
- **Downstream:** Enables all other skills by providing context
- **Council:** Submit to `council-review` for documentation review
