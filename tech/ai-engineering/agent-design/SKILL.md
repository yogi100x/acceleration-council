# Agent Design

Design LLM-powered agents with tool use, planning, memory, and safety guardrails.

## Research Protocol
### Web Search
- "LLM agent architecture patterns [current year]"
- "ReAct vs plan-and-execute agent [current year]"
- "multi-agent orchestration patterns [current year]"
### WebFetch
- https://docs.anthropic.com/en/docs/agents-and-tools

## Decision Tree: Agent Architecture

```
What does the agent need to do?
├── Single tool, simple task (search, calculate)
│   └── Simple tool-use (one-shot function calling)
├── Multi-step with dynamic decisions
│   └── ReAct (Reason + Act loop)
│       → Think → Act → Observe → Think → ...
├── Complex task requiring planning upfront
│   └── Plan-and-Execute
│       → Plan full approach → Execute steps → Replan if needed
├── Multiple specialized capabilities
│   └── Multi-agent with router
│       → Router agent delegates to specialist agents
├── Long-running with context across sessions
│   └── Agent with persistent memory
│       → Short-term (conversation) + long-term (database/vector store)
└── User-facing assistant
    └── Conversational agent with guardrails
        → System prompt + tool use + safety filters + human-in-the-loop
```

## Agent Components

| Component | Purpose | Implementation |
|-----------|---------|----------------|
| **Planner** | Decide what to do | System prompt with reasoning instructions |
| **Tools** | Execute actions | Function calling with typed schemas |
| **Memory** | Retain context | Short: conversation history. Long: vector DB |
| **Guardrails** | Prevent harm | Input validation, output filtering, scope limits |
| **Evaluator** | Check results | Self-critique step, validation functions |
| **Router** | Direct to right agent | Classification step or intent detection |

## Tool Design Principles

```
Good tools:
- Single responsibility (one action per tool)
- Typed parameters with descriptions
- Return structured data (not prose)
- Include error messages that help the agent recover
- Have clear names that describe what they do

Bad tools:
- "do_everything" mega-tool
- Untyped parameters
- Return raw HTML or unstructured text
- Silent failures
- Ambiguous names
```

## Safety Checklist

- [ ] Scope limits: Agent can only access what it needs
- [ ] Rate limiting: Max actions per conversation
- [ ] Human-in-the-loop for irreversible actions (delete, send, pay)
- [ ] Output validation before showing to user
- [ ] Input sanitization (no prompt injection via tool results)
- [ ] Timeout on agent loops (max iterations)
- [ ] Audit log of all agent actions

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Architecture** | Right pattern for task complexity | Reasonable choice | Over/under-engineered |
| **Tool design** | Single-purpose, typed, documented | Functional tools | Ambiguous mega-tools |
| **Memory** | Appropriate memory for task duration | Basic conversation | No memory management |
| **Safety** | Guardrails, scope limits, human-in-the-loop | Some safety | No safety measures |
| **Error handling** | Agent recovers from tool failures | Basic retry | Agent crashes on errors |
| **Evaluation** | Agent outputs verified, quality measured | Some testing | Untested |
| **Cost control** | Token budget, max iterations, model routing | Some limits | Unbounded loops |

**28+ = Reliable agent | 21-27 = Needs guardrails | <21 = Dangerous or unreliable**
