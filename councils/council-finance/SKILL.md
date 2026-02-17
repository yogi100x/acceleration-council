# Finance Review Council

Multi-expert review panel for financial deliverables. Simulates review by a CFO, FP&A Director, Tax Advisor, Investor Relations Lead, and Revenue Operations Manager.

## When to Use

Invoke after completing any finance skill output. The council validates financial models, projections, and strategy.

## Context Sync Protocol

Read these files before review:
- `.claude/finance-context.md` — Revenue model, funding stage, burn rate
- `shared/references/benchmarks.md` — SaaS financial benchmarks
- `shared/references/anti-patterns.md` — Finance anti-patterns (F1-F15)

## Review Panel

### 1. CFO (Strategic Finance)
- Are assumptions clearly stated and defensible?
- Does the model account for multiple scenarios?
- Is cash runway properly calculated?

### 2. FP&A Director (Model Integrity)
- Are formulas correct and auditable?
- Do projections tie to historical data?
- Are growth rates justified by market data?

### 3. Tax Advisor (Compliance)
- Are tax implications considered?
- Is revenue recognition compliant (ASC 606)?
- Are international considerations addressed?

### 4. Investor Relations (Narrative)
- Do metrics tell a compelling growth story?
- Are comparables relevant and current?
- Is the funding ask justified by unit economics?

### 5. Revenue Operations (Execution)
- Are billing mechanics implementable?
- Do pricing tiers map to customer segments?
- Is the payment infrastructure considered?

## Scoring

Each reviewer scores 1-5 on their domain. Ship threshold: average >= 4.0.

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| CFO | X/5 | ... |
| FP&A Director | X/5 | ... |
| Tax Advisor | X/5 | ... |
| IR Lead | X/5 | ... |
| Rev Ops | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Required Changes (if REVISE)
1. ...
```
