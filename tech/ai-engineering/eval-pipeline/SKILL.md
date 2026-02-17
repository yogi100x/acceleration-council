# Evaluation Pipeline

Design LLM evaluation systems: benchmarks, human eval, automated scoring, and regression testing.

## Research Protocol
### Web Search
- "LLM evaluation best practices [current year]"
- "LLM-as-judge evaluation pattern [current year]"
### WebFetch
- https://docs.anthropic.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations

## Decision Tree: Evaluation Approach

```
What are you evaluating?
├── Factual accuracy (RAG, Q&A)
│   └── Ground truth comparison + hallucination detection
├── Generation quality (writing, summarization)
│   └── LLM-as-judge + human eval sample
├── Classification / Extraction
│   └── Precision, recall, F1 against labeled dataset
├── Conversation quality (chatbot)
│   └── Multi-turn evaluation + user satisfaction proxy
├── Safety (harmful outputs)
│   └── Red team dataset + safety classifier
└── Overall system
    └── End-to-end metrics + A/B testing in production
```

## Evaluation Metrics

| Task Type | Primary Metrics | Secondary |
|-----------|----------------|-----------|
| **RAG** | Retrieval recall@k, answer accuracy | Faithfulness, relevance |
| **Generation** | LLM-judge score, human preference | Coherence, fluency |
| **Classification** | Precision, recall, F1 | Confusion matrix |
| **Extraction** | Exact match, partial match | Field-level accuracy |
| **Conversation** | Task completion rate | User satisfaction, turns to complete |

## LLM-as-Judge Pattern

```
Use a strong model to evaluate a weaker model's output:

System: You are evaluating the quality of an AI response.
Score 1-5 on: accuracy, helpfulness, safety.
Provide brief justification for each score.

Input: [user question]
Reference: [ground truth if available]
Response: [model output to evaluate]

Rules:
- Use a STRONGER model as judge (e.g., Claude Opus judging Haiku)
- Include reference answers when possible
- Randomize order for pairwise comparisons
- Run 3x and average (reduce variance)
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Dataset** | Diverse, representative, labeled, versioned | Basic test set | No eval dataset |
| **Metrics** | Task-appropriate metrics, tracked over time | Some metrics | No metrics |
| **Automation** | Automated eval in CI, regression alerts | Manual eval | No evaluation |
| **Human eval** | Regular human eval sample | Occasional | Never |
| **A/B testing** | Production A/B with statistical rigor | Basic comparison | No A/B |
| **Regression** | Eval runs on every prompt/model change | Periodic | No regression testing |
| **Bias testing** | Eval across demographic groups | Some analysis | No bias evaluation |

**28+ = Rigorous evaluation | 21-27 = Basic coverage | <21 = Shipping blind**
