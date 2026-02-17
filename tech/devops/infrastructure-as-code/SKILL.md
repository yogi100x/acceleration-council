# Infrastructure as Code

Terraform/Pulumi/CloudFormation, state management, modules, drift detection. Design decisions, patterns, and quality standards.

## Research Protocol
### Web Search
- "infrastructure as code best practices [current year]"
- "infrastructure as code tools comparison [current year]"

## Decision Tree

```
START: Assess current state and requirements
├── Greenfield (new infrastructure)
│   └── Start simple, add complexity as needed
├── Migration (from existing setup)
│   └── Incremental migration, parallel running
└── Optimization (existing infrastructure)
    └── Measure first, optimize bottlenecks
```

## Key Patterns

| Pattern | When to Use | Trade-off |
|---------|-------------|-----------|
| **Simple** | Small team, early stage | Easy but limited |
| **Standard** | Growing team, moderate scale | Good balance |
| **Advanced** | Large team, high scale | Powerful but complex |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Automation** | Fully automated, repeatable | Partially automated | Manual processes |
| **Security** | Least privilege, encrypted, audited | Basic security | Security gaps |
| **Reliability** | Redundant, tested failover | Some redundancy | Single points of failure |
| **Monitoring** | Full observability, alerting | Basic monitoring | No monitoring |
| **Cost** | Optimized, tracked, budgeted | Cost-aware | Uncontrolled spend |
| **Documentation** | Runbooks, diagrams, updated | Some documentation | Undocumented |
| **Scalability** | Horizontal scaling, capacity planned | Some scaling | Fixed capacity |

**28+ = Production-grade | 21-27 = Gaps to address | <21 = Risk of outage**

## Output Protocol
Write to `.claude/outputs/infrastructure-as-code.md`:
- Architecture decisions
- Configuration details  
- Monitoring and alerting setup
