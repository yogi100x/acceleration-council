# Fine-Tuning Strategy

Decide when to fine-tune vs prompt engineer, prepare data, and evaluate fine-tuned models.

## Research Protocol
### Web Search
- "LLM fine-tuning vs prompt engineering decision [current year]"
- "LoRA vs full fine-tuning comparison [current year]"
### WebFetch
- https://platform.openai.com/docs/guides/fine-tuning

## Decision Tree: Fine-Tune or Not?

```
Do you need fine-tuning?
├── Need specific output FORMAT consistently
│   └── Try structured output / JSON mode first
│       → If still inconsistent after 20+ examples in prompt → fine-tune
├── Need specific STYLE or VOICE
│   └── Try few-shot prompting first
│       → If 5+ examples in prompt and still not right → fine-tune
├── Need DOMAIN KNOWLEDGE
│   └── Try RAG first (retrieval-augmented generation)
│       → If RAG is too slow or context window too small → fine-tune
├── Need LOWER LATENCY / COST
│   └── Fine-tune smaller model to match larger model quality
│       → Distillation: generate training data from large model
├── Need CLASSIFICATION or EXTRACTION
│   └── Fine-tune — usually most effective approach
└── General improvement
    └── DON'T fine-tune. Improve prompts first. Always.
```

## Fine-Tuning Methods

| Method | Training Data | Cost | Quality | When |
|--------|-------------|------|---------|------|
| **Full fine-tune** | 1K-100K examples | High ($$$) | Best | Critical production use case |
| **LoRA/QLoRA** | 500-10K examples | Low ($) | Good | Most use cases |
| **Prompt tuning** | 100-1K examples | Very low | Moderate | Quick experiments |
| **Distillation** | Generated from large model | Medium | Good | Cost optimization |

## Data Preparation Checklist

- [ ] Minimum 500 high-quality examples (1000+ preferred)
- [ ] Examples represent real production distribution
- [ ] Input/output format matches production exactly
- [ ] No PII or sensitive data in training set
- [ ] Held-out test set (20% of data)
- [ ] Examples reviewed by domain expert
- [ ] Edge cases and failure modes included

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Decision** | Clear rationale for fine-tune vs alternatives | Some analysis | Fine-tuned without trying alternatives |
| **Data quality** | Expert-reviewed, diverse, representative | Adequate data | Small/biased dataset |
| **Evaluation** | Held-out test set + human eval + production metrics | Test set eval | No evaluation |
| **Method** | Appropriate method (LoRA vs full) for scale | Default method | Over-engineered |
| **Cost** | Cost per inference calculated, within budget | Cost awareness | Unknown cost |
| **Versioning** | Model versions tracked, A/B tested | Some tracking | No versioning |
| **Monitoring** | Production quality monitored, drift detected | Basic monitoring | No monitoring |

**28+ = Justified fine-tuning | 21-27 = Needs more evaluation | <21 = Premature fine-tuning**
