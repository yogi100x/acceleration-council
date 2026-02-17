# Protocol Design

REST vs gRPC vs GraphQL vs WebSocket vs MQTT — comprehensive decision framework. Decision frameworks for systems-level engineering.

## Research Protocol
### Web Search
- "protocol design patterns [current year]"
- "protocol design comparison [current year]"

## Decision Tree

```
START: What are the requirements?
├── Simplicity (small scale, clear requirements)
│   └── Use the simplest approach that works
├── Performance (high throughput, low latency)
│   └── Benchmark and profile before optimizing
├── Reliability (fault tolerance, consistency)
│   └── Design for failure, test failure modes
└── Scalability (growing data, users, traffic)
    └── Horizontal scaling, partitioning, caching
```

## Key Patterns

| Pattern | When to Use | Trade-off |
|---------|-------------|-----------|
| **Simple** | Known problem, small scale | Easy to understand and maintain |
| **Standard** | Common patterns, moderate scale | Well-documented, community support |
| **Advanced** | Unique constraints, large scale | Complex but handles edge cases |

## Common Mistakes

| Mistake | Impact | Fix |
|---------|--------|-----|
| Premature optimization | Wasted effort, complexity | Profile first, optimize bottlenecks |
| Wrong abstraction level | Hard to change later | Start concrete, abstract when patterns emerge |
| Ignoring failure modes | System crashes under stress | Design for failure from the start |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Correctness** | Provably correct, handles all edge cases | Correct for common cases | Known bugs |
| **Performance** | Benchmarked, meets requirements | Reasonable | Untested |
| **Scalability** | Handles 10x growth | Handles current load | Breaks under load |
| **Simplicity** | Simplest solution that works | Reasonable complexity | Over-engineered |
| **Testability** | Comprehensive tests, property-based | Unit tests | Untested |
| **Documentation** | Algorithmic analysis, design rationale | Some docs | Undocumented |
| **Failure handling** | Graceful degradation, recovery | Basic error handling | Crashes |

**28+ = Solid engineering | 21-27 = Needs hardening | <21 = Fragile system**

## Output Protocol
Write to `.claude/outputs/protocol-design.md`:
- Algorithm/pattern chosen with rationale
- Complexity analysis
- Failure modes and handling
