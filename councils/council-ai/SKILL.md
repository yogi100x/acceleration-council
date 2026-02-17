# AI Governance Review Council

Multi-expert review panel for AI deliverables. Simulates review by an AI Ethics Lead, ML Engineer, Fairness Researcher, Product Safety Manager, and Regulatory Specialist.

## When to Use

Invoke after completing any AI governance skill output. The council evaluates AI systems for safety, fairness, and compliance.

## Context Sync Protocol

Read these files before review:
- `.claude/ai-context.md` — AI capabilities, data sources, deployment scope
- `shared/references/anti-patterns.md` — AI anti-patterns (A1-A12)

## Review Panel

### 1. AI Ethics Lead (Values Alignment)
- Are ethical principles clearly defined and applied?
- Is there meaningful human oversight?
- Are affected stakeholders identified?

### 2. ML Engineer (Technical Robustness)
- Is the model architecture appropriate?
- Are failure modes documented?
- Is monitoring sufficient for drift detection?

### 3. Fairness Researcher (Bias & Equity)
- Are protected attributes identified?
- Is bias testing comprehensive across demographics?
- Are mitigation strategies proportionate?

### 4. Product Safety Manager (Risk)
- Are high-risk use cases identified?
- Is the risk assessment methodology sound?
- Are fallback mechanisms adequate?

### 5. Regulatory Specialist (Compliance)
- EU AI Act classification correct?
- Are transparency requirements met?
- Is documentation audit-ready?

## Scoring

Each reviewer scores 1-5. Ship threshold: average >= 4.0. Any safety-critical finding (fairness < 3 or safety < 3) = automatic RETHINK.

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| AI Ethics Lead | X/5 | ... |
| ML Engineer | X/5 | ... |
| Fairness Researcher | X/5 | ... |
| Product Safety | X/5 | ... |
| Regulatory Specialist | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Safety Flags
- ...
```
