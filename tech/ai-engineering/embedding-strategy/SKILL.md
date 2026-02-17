# Embedding Strategy

Choose embedding models, dimensions, and similarity metrics for vector search and retrieval.

## Research Protocol
### Web Search
- "text embedding models comparison [current year]"
- "OpenAI embeddings vs Cohere vs open source [current year]"
### WebFetch
- https://platform.openai.com/docs/guides/embeddings

## Decision Tree: Embedding Model

```
What are you embedding?
├── Short text (queries, titles, <512 tokens)
│   └── Small model (text-embedding-3-small, 1536d)
│       → Cheaper, faster, good for most search
├── Long documents (articles, docs, >512 tokens)
│   └── Large model (text-embedding-3-large, 3072d) + chunking
│       → Better semantic capture, need chunking strategy
├── Code
│   └── Code-specific model (CodeBERT, StarCoder embeddings)
├── Multi-language
│   └── Multilingual model (Cohere multilingual, mE5)
├── Privacy-sensitive (can't send to API)
│   └── Open-source model (BGE, E5, GTE) self-hosted
└── Multi-modal (text + images)
    └── CLIP or multi-modal embedding model
```

## Chunking Strategies

| Strategy | When | Implementation |
|----------|------|----------------|
| **Fixed size** | Simple, predictable | Split at N tokens with overlap |
| **Semantic** | Preserve meaning | Split at paragraph/section boundaries |
| **Recursive** | Mixed content | Try paragraph → sentence → token splits |
| **Document-aware** | Structured docs | Use headers, sections as boundaries |

Overlap: 10-20% of chunk size to preserve context at boundaries.

## Similarity Metrics

| Metric | When | Note |
|--------|------|------|
| **Cosine** | Normalized embeddings (most APIs) | Default choice |
| **Dot product** | When magnitude matters | Faster, needs normalization |
| **Euclidean** | Clustering, anomaly detection | Sensitive to magnitude |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Model selection** | Benchmarked on your data | Industry standard | Random choice |
| **Chunking** | Semantic chunking with overlap | Fixed size | No chunking strategy |
| **Dimensionality** | Optimized for storage/quality tradeoff | Default dimensions | Over-sized embeddings |
| **Versioning** | Embedding version tracked, re-embed on model change | Some tracking | No versioning |
| **Batch processing** | Efficient batch embed pipeline | Individual calls | No batching |
| **Cost** | Cost per embed calculated, budget managed | Cost awareness | Unconstrained |
| **Evaluation** | Retrieval quality measured (recall@k, MRR) | Basic testing | No evaluation |

**28+ = Optimized embeddings | 21-27 = Functional | <21 = Poor retrieval quality**

## Output Protocol
Write to `.claude/outputs/embedding-strategy.md`:
- Model chosen with rationale
- Chunking strategy and parameters
- Similarity metric
- Embedding pipeline design
