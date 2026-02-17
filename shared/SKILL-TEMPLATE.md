# SKILL-TEMPLATE

Use this template when creating new skills. Every skill MUST include all three protocols.

---

```markdown
---
name: [skill-name]
version: 2.0.0
category: [marketing|finance|legal|ai-governance|tech/core|tech/frontend|tech/backend|tech/ui|tech/ai-engineering|tech/blockchain|tech/data|tech/devops|tech/mobile|tech/systems]
description: "[When to trigger this skill — natural language description]"
---

# [Skill Title]

[1-2 sentence expert persona and goal statement]

---

## Research Protocol

Before executing, verify current versions and best practices.

### Web Search (required — always available in Claude Code)
- "[Library/Technology] latest stable version [current year]"
- "[Library/Technology] best practices [current year]"
- "[Specific search relevant to this skill]"

### WebFetch (canonical documentation)
- [Official docs URL 1]
- [Official docs URL 2]

### Update `.claude/versions.md`
After verifying, update the versions file:
| Library | Version | Verified Date | Notes |
|---------|---------|---------------|-------|
| [lib] | [ver] | [date] | [any breaking changes] |

### Context7 (optional — if MCP available)
- Resolve: "[library-name]"
- Query: "[specific API or pattern to look up]"

---

## Context Sync Protocol

### Read Context (required)
- `.claude/context/[relevant]-context.md`

### Read Upstream Outputs (if they exist)
- `.claude/outputs/[upstream-skill-1].md` — [what to look for]
- `.claude/outputs/[upstream-skill-2].md` — [what to look for]

---

## Decision Tree: [Primary Decision]

```
[Decision tree specific to this skill]
```

---

## [Main Content]

[Patterns, tables, checklists, guidelines — the core expertise]

---

## Anti-Pattern References

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| [XX] | [Name] | [Consequence] |

See `shared/references/anti-patterns.md` for full descriptions.

---

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| [Dim 1] | [Excellent] | [Adequate] | [Poor] |
| [Dim 2] | ... | ... | ... |
| [Dim 3] | ... | ... | ... |
| [Dim 4] | ... | ... | ... |
| [Dim 5] | ... | ... | ... |
| [Dim 6] | ... | ... | ... |
| [Dim 7] | ... | ... | ... |

**28+ = Ship | 21-27 = Revise | <21 = Rethink**

---

## Cross-Skill References

| Relationship | Skill | What It Provides |
|-------------|-------|-----------------|
| **Upstream** | [skill] | [what this skill reads from it] |
| **Downstream** | [skill] | [what this skill provides to it] |
| **Parallel** | [skill] | [shared concerns] |
| **Council** | [council-name] | [review type] |

---

## Output Protocol (required after execution)

Write to `.claude/outputs/[this-skill-name].md`:

### Decisions Made
- [Decision]: [Choice] — [Rationale]

### Constraints for Downstream Skills
- [What downstream skills must respect]

### Interfaces Defined
- [APIs, schemas, contracts, file structures]

### Open Questions
- [Unresolved items for other skills]
```
