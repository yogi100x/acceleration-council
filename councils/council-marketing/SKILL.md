# Marketing Review Council

Multi-expert review panel for marketing deliverables. Simulates review by a CMO, Growth Lead, Brand Strategist, Data Analyst, and Content Director.

## When to Use

Invoke after completing any marketing skill output. The council reviews work against quality rubrics and industry benchmarks.

## Context Sync Protocol

Read these files before review:
- `.claude/product-marketing-context.md` — Product positioning, ICP, messaging
- `shared/references/benchmarks.md` — Industry conversion/engagement benchmarks
- `shared/references/anti-patterns.md` — Marketing anti-patterns (M1-M20)

## Review Panel

### 1. CMO (Strategic Alignment)
- Does this align with overall marketing strategy?
- Is the target audience clearly defined and addressed?
- Will this move key metrics (CAC, LTV, conversion)?

### 2. Growth Lead (Performance)
- Are conversion benchmarks realistic and referenced?
- Is there a clear measurement plan?
- Are A/B testing opportunities identified?

### 3. Brand Strategist (Consistency)
- Does tone match brand voice guidelines?
- Is messaging consistent across touchpoints?
- Are brand differentiators highlighted?

### 4. Data Analyst (Measurement)
- Are KPIs defined and measurable?
- Is attribution properly planned?
- Are statistical significance requirements noted?

### 5. Content Director (Quality)
- Is copy clear, concise, and compelling?
- Are CTAs action-oriented and specific?
- Is content structured for scannability?

## Scoring

Each reviewer scores 1-5 on their domain. Ship threshold: average >= 4.0 across all reviewers.

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| CMO | X/5 | ... |
| Growth Lead | X/5 | ... |
| Brand Strategist | X/5 | ... |
| Data Analyst | X/5 | ... |
| Content Director | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Required Changes (if REVISE)
1. ...

### Strengths
1. ...
```
