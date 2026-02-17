# AI Cost Optimization

Reduce LLM costs through model routing, caching, batching, and prompt optimization.

## Research Protocol
### Web Search
- "LLM cost optimization strategies [current year]"
- "semantic caching for LLM [current year]"
- "model routing cheap vs expensive [current year]"

## Decision Tree: Cost Reduction Strategy

```
Where is cost coming from?
├── High per-token cost (using large model for everything)
│   └── Model routing: cheap model first, expensive on fallback
│       → Route simple tasks to Haiku, complex to Opus
├── Repeated identical/similar queries
│   └── Semantic caching: cache similar query results
│       → Hash exact matches, vector similarity for near-matches
├── Large prompts (long context, many examples)
│   └── Prompt compression: reduce token count
│       → Fewer examples, shorter system prompts, summarize context
├── High volume (many requests)
│   └── Batching: group requests for batch API (50% cheaper)
│       → For non-real-time: reports, processing, eval
├── Expensive fine-tuned model
│   └── Distillation: train smaller model on larger model's outputs
│       → Generate training data from expensive model
└── All of the above
    └── Implement in order: caching → routing → compression → batching
```

## Cost Comparison (approximate)

| Model | Input $/1M tokens | Output $/1M tokens | When to Use |
|-------|-------------------|---------------------|-------------|
| Haiku | $0.25 | $1.25 | Classification, extraction, simple Q&A |
| Sonnet | $3 | $15 | Most tasks, good balance |
| Opus | $15 | $75 | Complex reasoning, creative, critical tasks |
| GPT-4o-mini | $0.15 | $0.60 | Simple tasks, high volume |
| GPT-4o | $2.50 | $10 | General purpose |

*Prices change frequently. Always verify via web search.*

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Model routing** | Task-appropriate model selection | Some routing | One model for everything |
| **Caching** | Semantic cache with TTL | Exact-match cache | No caching |
| **Prompt efficiency** | Optimized prompts, minimal tokens | Reasonable length | Bloated prompts |
| **Batching** | Batch API for non-real-time | Some batching | All real-time |
| **Monitoring** | Per-feature cost tracking, budget alerts | Total cost tracked | No cost tracking |
| **Budget** | Per-user/per-feature budgets | Global budget | No budget |
| **Projection** | Cost projected at 10x scale | Current cost known | No projection |

**28+ = Cost-effective AI | 21-27 = Some waste | <21 = Burning money**
