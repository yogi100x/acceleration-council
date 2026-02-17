# Tool Use Design

Design function calling schemas, error handling, and permission scoping for LLM tool use.

## Research Protocol
### Web Search
- "LLM function calling best practices [current year]"
- "Claude tool use patterns [current year]"
### WebFetch
- https://docs.anthropic.com/en/docs/build-with-claude/tool-use

## Decision Tree: Tool Design

```
What should the tool do?
├── Read data (search, fetch, query)
│   └── Read-only tool with clear return schema
│       → Safe to auto-execute, no confirmation needed
├── Create/modify data (write, update, send)
│   └── Write tool with confirmation for irreversible actions
│       → Human-in-the-loop for: delete, send message, payment
├── External API call
│   └── Wrapper tool with error handling and rate limiting
│       → Cache results, handle timeouts, retry transient errors
├── Multi-step operation
│   └── Orchestrator tool that chains sub-tools
│       → Or let the agent plan the sequence itself
└── Computation (calculate, format, transform)
    └── Pure function tool (no side effects)
        → Always safe, deterministic
```

## Schema Design Principles

```json
{
  "name": "search_candidates",
  "description": "Search for candidates matching skills and location criteria. Returns top 10 matches.",
  "input_schema": {
    "type": "object",
    "properties": {
      "skills": {
        "type": "array",
        "items": { "type": "string" },
        "description": "Required skills (e.g., ['React', 'TypeScript'])"
      },
      "location": {
        "type": "string",
        "description": "City or region (e.g., 'San Francisco')"
      }
    },
    "required": ["skills"]
  }
}

Rules:
- Clear, specific description (model needs to know WHEN to use it)
- Typed parameters with descriptions and examples
- Required vs optional clearly marked
- Return type documented
- Error cases documented
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Schema design** | Clear names, typed params, descriptions + examples | Basic typing | Untyped/ambiguous |
| **Permission scoping** | Read/write separation, least privilege | Some scoping | All tools unrestricted |
| **Error handling** | Structured errors that help model recover | Basic errors | Silent failures |
| **Rate limiting** | Per-tool rate limits | Global limits | No limits |
| **Human-in-the-loop** | Confirmation for irreversible actions | Some confirmation | All auto-executed |
| **Testing** | Tool schemas tested, edge cases covered | Basic testing | Untested |
| **Documentation** | Full tool catalog with examples | Basic docs | No documentation |

**28+ = Reliable tool use | 21-27 = Works but risky | <21 = Unreliable or unsafe**
