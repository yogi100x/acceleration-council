# GraphQL Design

Design GraphQL APIs: schema, resolvers, performance, and real-time subscriptions.

## Research Protocol

### Web Search
- "GraphQL best practices [current year]"
- "GraphQL N+1 DataLoader pattern [current year]"
- "GraphQL vs REST decision [current year]"

### WebFetch
- https://graphql.org/learn/best-practices/

---

## Decision Tree: GraphQL vs REST

```
Should you use GraphQL?
├── Multiple clients with different data needs (web, mobile, third-party)
│   └── GraphQL — clients query exactly what they need
├── Simple CRUD with consistent data shapes
│   └── REST — simpler, well-understood, better caching
├── Real-time subscriptions needed
│   └── GraphQL subscriptions or REST + WebSocket
├── Public developer API
│   └── REST (with OpenAPI) — more universal, better tooling
└── Internal API with TypeScript full-stack
    └── tRPC — end-to-end type safety, simplest
```

## Schema Design Principles

```graphql
# Use clear, domain-driven naming
type User {
  id: ID!
  email: String!
  profile: UserProfile!         # Separate concerns into types
  posts(first: Int, after: String): PostConnection!  # Relay pagination
}

# Use input types for mutations
input CreateUserInput {
  email: String!
  name: String!
}

# Return union types for errors
union CreateUserResult = User | ValidationError | AlreadyExistsError

type Mutation {
  createUser(input: CreateUserInput!): CreateUserResult!
}
```

## Performance Patterns

| Problem | Solution |
|---------|----------|
| **N+1 queries** | DataLoader for batching and caching |
| **Over-fetching** | Query complexity analysis + depth limiting |
| **Large responses** | Pagination (Relay cursor-based) |
| **Caching** | Persisted queries + CDN caching |
| **Authorization** | Directive-based or middleware auth per field |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Schema design** | Domain-driven, clear types, input types | Reasonable schema | Flat/unstructured |
| **Performance** | DataLoader, complexity limits, pagination | Some optimization | N+1 everywhere |
| **Error handling** | Union types for domain errors | Error extensions | Generic errors |
| **Auth** | Per-field authorization | Per-type | No auth |
| **Documentation** | Schema descriptions, examples | Some descriptions | No docs |
| **Subscriptions** | Where needed, with connection management | Basic subscriptions | None or broken |
| **Testing** | Query tests, schema tests | Some testing | Untested |

**28+ = Well-designed GraphQL | 21-27 = Works but fragile | <21 = Performance problems**

## Output Protocol

Write to `.claude/outputs/graphql-design.md`:
- Schema design decisions
- Performance patterns applied
- Auth strategy
- Subscription design
