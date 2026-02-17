# Prompt Management

Version, test, and manage prompts across environments with A/B testing and migration support.

## Research Protocol
### Web Search
- "prompt management system [current year]"
- "prompt versioning best practices [current year]"

## Decision Tree: Prompt Management Approach

```
How many prompts do you manage?
├── 1-5 prompts (simple app)
│   └── Version controlled in code (constants file)
├── 5-20 prompts (growing AI features)
│   └── Prompt registry (database or config file)
│       → Version, A/B test, rollback
├── 20+ prompts (AI-heavy product)
│   └── Prompt management platform (Langfuse, Humanloop)
│       → Full lifecycle: edit, test, deploy, monitor
└── Multi-model (same prompt across providers)
    └── Template system with model-specific adapters
```

## Prompt Lifecycle

```
Draft → Review → Test (eval pipeline) → Deploy → Monitor → Iterate

Each prompt has:
  - Unique ID
  - Version (semver)
  - Template with variables
  - Model target (which LLM)
  - Eval results (score on test set)
  - A/B test results (if applicable)
  - Rollback capability
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Versioning** | Semver, change log, rollback | Basic versioning | No versioning |
| **Testing** | Eval pipeline before deploy | Manual testing | Untested changes |
| **A/B testing** | Statistical A/B on prompt changes | Basic comparison | No A/B |
| **Monitoring** | Quality metrics per prompt version | Basic tracking | No monitoring |
| **Templates** | Variable substitution, model adapters | Basic templates | Hardcoded strings |
| **Environment** | Dev/staging/prod prompt promotion | Some separation | Same everywhere |
| **Documentation** | Each prompt documented with intent and constraints | Some docs | No documentation |

**28+ = Professional prompt ops | 21-27 = Basic management | <21 = Prompts scattered in code**
