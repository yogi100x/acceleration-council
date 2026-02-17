# Legal Review Council

Multi-expert review panel for legal deliverables. Simulates review by a General Counsel, Privacy Officer, IP Attorney, Employment Lawyer, and Compliance Director.

## When to Use

Invoke after completing any legal skill output. The council validates legal documents for completeness, accuracy, and risk.

## Context Sync Protocol

Read these files before review:
- `.claude/legal-context.md` — Jurisdiction, entity type, data practices
- `shared/references/anti-patterns.md` — Legal anti-patterns (L1-L12)

## Review Panel

### 1. General Counsel (Risk Assessment)
- Are liability limitations appropriate?
- Is dispute resolution mechanism clear?
- Are force majeure and termination provisions adequate?

### 2. Privacy Officer (Data Protection)
- GDPR/CCPA compliance verified?
- Data processing purposes clearly stated?
- User rights mechanisms defined?

### 3. IP Attorney (Intellectual Property)
- Are IP ownership clauses clear?
- Is licensing scope appropriate?
- Are third-party IP dependencies identified?

### 4. Employment Lawyer (Workforce)
- Are employment/contractor classifications correct?
- Is IP assignment language enforceable?
- Are non-compete/NDA terms reasonable?

### 5. Compliance Director (Regulatory)
- Are industry-specific regulations addressed?
- Is record-keeping adequate?
- Are audit rights properly defined?

## Scoring

Each reviewer scores 1-5. Ship threshold: average >= 4.0. Any reviewer scoring 2 or below = automatic RETHINK.

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| General Counsel | X/5 | ... |
| Privacy Officer | X/5 | ... |
| IP Attorney | X/5 | ... |
| Employment Lawyer | X/5 | ... |
| Compliance Director | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Risk Flags (if any)
- [HIGH/MEDIUM/LOW] ...
```
