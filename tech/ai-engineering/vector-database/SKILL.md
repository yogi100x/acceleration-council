# Vector Database

Choose and configure vector storage for embedding-based search and retrieval.

## Research Protocol
### Web Search
- "pgvector vs Pinecone vs Weaviate vs Qdrant [current year]"
- "vector database comparison [current year]"
### WebFetch
- https://github.com/pgvector/pgvector

## Decision Tree: Vector DB Selection

```
What's your scale and infrastructure?
├── Already using PostgreSQL + <1M vectors
│   └── pgvector extension
│       → No new infrastructure, familiar SQL, good enough for most
├── 1M-100M vectors, need managed service
│   └── Pinecone (managed) or Qdrant (self-hosted or cloud)
│       → Purpose-built, faster at scale, metadata filtering
├── Need multi-modal (text + images + audio)
│   └── Weaviate
│       → Built-in vectorization, multi-modal support
├── Need real-time updates + search
│   └── Qdrant or Milvus
│       → Real-time indexing, low-latency search
└── Cost-sensitive, open source required
    └── Qdrant (self-hosted) or pgvector
```

## Index Types

| Index | Speed | Recall | Memory | Best For |
|-------|-------|--------|--------|----------|
| **Flat (brute force)** | Slow | 100% | Low | <10K vectors, exact results |
| **IVF** | Fast | ~95% | Medium | 10K-10M, good balance |
| **HNSW** | Fastest | ~98% | High | Most use cases, best quality/speed |
| **PQ (Product Quantization)** | Fast | ~90% | Very low | >10M, memory-constrained |

## Hybrid Search Pattern

```
Combine vector search + keyword search:

1. Vector search: Find semantically similar (top 50)
2. Keyword search: Find exact matches (top 50)  
3. Reciprocal Rank Fusion: Merge and rerank results
4. Return top K combined results

Why: Vector search misses exact terms. Keyword search misses meaning.
Hybrid catches both.
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Selection** | Benchmarked on your data | Industry standard | Random choice |
| **Index** | HNSW with tuned parameters | Default index | No index (brute force at scale) |
| **Hybrid search** | Vector + keyword + reranking | Vector only | Keyword only |
| **Filtering** | Pre-filter metadata + vector search | Post-filter | No filtering |
| **Scaling** | Sharding, replication plan | Single instance | No scaling plan |
| **Updates** | Real-time or near-real-time index updates | Batch updates | Manual reindex |
| **Monitoring** | Query latency, recall, index size tracked | Basic metrics | No monitoring |

**28+ = Optimized retrieval | 21-27 = Functional | <21 = Poor search quality**
