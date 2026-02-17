# Tech Review Council

Multi-expert review panel for technical deliverables. Simulates review by a Principal Engineer, Security Architect, SRE Lead, DX Engineer, and QA Director.

## When to Use

Invoke after completing any tech skill output. The council reviews architecture, code quality, security, and operational readiness.

## Context Sync Protocol

Read these files before review:
- `.claude/tech-context.md` — Stack, scale requirements, team size
- `shared/references/anti-patterns.md` — Tech anti-patterns (T1-T15)
- `shared/references/benchmarks.md` — Performance benchmarks

## Review Panel

### 1. Principal Engineer (Architecture)
- Is the architecture appropriate for scale requirements?
- Are abstractions at the right level?
- Are trade-offs explicitly documented?

### 2. Security Architect (Security)
- Are OWASP Top 10 vulnerabilities addressed?
- Is authentication/authorization properly designed?
- Are secrets management practices sound?

### 3. SRE Lead (Reliability)
- Are SLOs defined and measurable?
- Is observability comprehensive (logs, metrics, traces)?
- Are failure modes handled gracefully?

### 4. DX Engineer (Developer Experience)
- Is the API ergonomic and well-documented?
- Are error messages helpful?
- Is local development setup straightforward?

### 5. QA Director (Quality)
- Is test coverage strategy comprehensive?
- Are edge cases identified?
- Is the testing pyramid balanced?

## Scoring

Each reviewer scores 1-5. Ship threshold: average >= 4.0. Security score < 3 = automatic RETHINK.

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| Principal Engineer | X/5 | ... |
| Security Architect | X/5 | ... |
| SRE Lead | X/5 | ... |
| DX Engineer | X/5 | ... |
| QA Director | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Blocking Issues
- ...
```
