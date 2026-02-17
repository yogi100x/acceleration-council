# AI Safety Engineering

Implement guardrails, content filtering, and abuse prevention for AI features.

## Research Protocol
### Web Search
- "LLM safety guardrails implementation [current year]"
- "prompt injection prevention [current year]"
- "AI content moderation best practices [current year]"

## Decision Tree: Safety Layer

```
What safety measure do you need?
├── Prevent harmful outputs (hate speech, violence, illegal)
│   └── Output classifier + blocklist
│       → Run classifier on output before showing to user
├── Prevent prompt injection
│   └── Input sanitization + instruction hierarchy
│       → Separate system/user content, validate tool results
├── Prevent PII leakage
│   └── PII detection + redaction
│       → Scan outputs for emails, phones, SSNs, names
├── Prevent abuse (automated, high-volume)
│   └── Rate limiting + usage monitoring
│       → Per-user limits, anomaly detection
├── Prevent jailbreaking
│   └── Multi-layer defense
│       → System prompt hardening + output validation + monitoring
└── Prevent data exfiltration via AI
    └── Scope restriction + output filtering
        → Limit what data AI can access, filter outputs
```

## Defense Layers

```
Layer 1: INPUT
  - Sanitize user input (remove injection attempts)
  - Rate limit per user
  - Block known attack patterns

Layer 2: SYSTEM PROMPT
  - Clear role and boundaries
  - Explicit refusal instructions
  - Separate instructions from user data (delimiters)

Layer 3: OUTPUT
  - Content safety classifier
  - PII detection and redaction
  - Format validation (structured output)

Layer 4: MONITORING
  - Log all interactions
  - Alert on safety classifier triggers
  - Track refusal rate (too high = UX problem, too low = safety gap)
```

## Safety Checklist

- [ ] Input validation and sanitization
- [ ] System prompt with clear boundaries
- [ ] Output content safety check
- [ ] PII detection in outputs
- [ ] Rate limiting per user
- [ ] Prompt injection defenses (delimiter separation)
- [ ] Human review queue for flagged outputs
- [ ] Incident response plan for safety failures
- [ ] Regular red-teaming of AI features
- [ ] Monitoring dashboard for safety metrics

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Input safety** | Sanitization + injection defense + rate limiting | Basic validation | No input checks |
| **Output safety** | Content classifier + PII detection + format check | Basic filtering | No output checks |
| **Prompt design** | Hardened system prompt with clear boundaries | Basic system prompt | No safety instructions |
| **Monitoring** | Real-time safety dashboard, alerting | Basic logging | No monitoring |
| **Red teaming** | Regular adversarial testing | Occasional testing | Never tested |
| **Incident response** | Documented playbook, kill switch | Some process | No response plan |
| **User controls** | Report button, feedback loop, opt-out | Report button | No user recourse |

**28+ = Safety-first AI | 21-27 = Known gaps | <21 = Unsafe to ship**
