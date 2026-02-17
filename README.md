# Acceleration Council

A comprehensive library of **166 execution skills**, **6 review councils**, and **1 orchestrator** for Claude Code, organized across business and technical domains. Built by [The Acceleration Guy](https://github.com/yogi100x).

## What Are Skills?

Skills are structured prompts that give Claude Code domain expertise. Each skill contains:

- **Research Protocol** — Web search for latest versions/APIs before executing
- **Context Sync Protocol** — What project context to read before executing
- **Decision Trees** — Structured decision-making for common choices
- **Quality Rubric** — 5-point scoring across 7 dimensions (35 total)
- **Output Protocol** — Writes decisions to `.claude/outputs/` for downstream skills
- **Cross-Skill References** — When to hand off to other skills

## Structure

```
acceleration-council/
├── setup/                        # Orchestrator + context generation
│   └── orchestrator/             # Meta-skill: maps tasks → skill sequences
├── shared/
│   ├── references/               # Benchmarks, anti-patterns, industry profiles
│   └── SKILL-TEMPLATE.md         # Template for creating new skills
├── marketing/         (25 skills)
├── finance/           (13 skills)
├── legal/             (13 skills)
├── ai-governance/     (12 skills)
├── tech/             (101 skills)
│   ├── frontend/      (12)       # React, SSR, forms, a11y, animations
│   ├── backend/       (12)       # Caching, queues, real-time, email, search
│   ├── ui/            (10)       # Design systems, components, data viz
│   ├── ai-engineering/(12)       # RAG, embeddings, agents, eval, streaming
│   ├── blockchain/    (10)       # Smart contracts, DeFi, NFTs, tokenomics
│   ├── data/           (8)       # Modeling, ETL, warehousing, streaming
│   ├── devops/         (8)       # Containers, IaC, secrets, DR, observability
│   ├── mobile/         (8)       # Architecture, RN, offline-first, push
│   ├── systems/        (6)       # Algorithms, distributed, concurrency
│   └── (15 standalone)           # Architecture, security, auth, CI/CD, etc.
├── councils/           (6 councils)
├── workflows/                    # 20 multi-skill recipes
├── docs/                         # Expansion plan & architecture
└── SKILL-MAP.md                  # Dependency graph
```

## Quick Start

### 1. Install a single skill

```bash
mkdir -p .claude/skills/copywriting
cp acceleration-council/marketing/copywriting/SKILL.md .claude/skills/copywriting/
```

### 2. Install a full category

```bash
cp -r acceleration-council/marketing/ .claude/skills/
```

### 3. Install everything

```bash
cp -r acceleration-council/* .claude/skills/
```

### 4. Run the setup skill first

The `setup` skill generates context files that other skills read:

```
.claude/context/product-marketing.md
.claude/context/finance.md
.claude/context/legal.md
.claude/context/ai.md
.claude/context/tech.md
```

## Categories

### Marketing (25 skills)
Copywriting, pricing strategy, launch strategy, content strategy, paid ads, A/B testing, email sequences, page CRO, SEO audit, analytics tracking, signup flow CRO, onboarding CRO, referral programs, social content, marketing psychology, form CRO, popup CRO, paywall/upgrade CRO, copy editing, competitor alternatives, programmatic SEO, schema markup, free tool strategy, marketing ideas, product marketing context.

### Finance (13 skills)
Financial modeling, unit economics, pricing models, billing design, budget allocation, fundraising decks, cap tables, revenue recognition, cost optimization, LTD deal structure, tax strategy, treasury management, KPI dashboards.

### Legal (13 skills)
Privacy policy, terms of service, contract drafting, cookie consent, data processing agreements, IP protection, employment agreements, compliance checklists, incident response, AI impact assessment, acceptable use policy, regulatory filing, dispute resolution.

### AI Governance (12 skills)
Model cards, bias testing, prompt engineering, AI ethics policy, algorithmic impact assessment, explainability design, human oversight protocols, AI monitoring, training data audits, AI incident response, AI vendor evaluation, red team protocols.

### Tech (101 skills)

| Sub-Category | Count | Highlights |
|--------------|-------|------------|
| **Frontend** | 12 | React patterns, SSR, state management, a11y, animations, PWA |
| **Backend** | 12 | Caching, queues, real-time, search, email, rate limiting |
| **UI Engineering** | 10 | Design systems, theming, data viz, tables, drag & drop |
| **AI Engineering** | 12 | RAG, embeddings, agents, eval, streaming, tool use, safety |
| **Blockchain/Web3** | 10 | Smart contracts, security, DeFi, NFTs, tokenomics, DAOs |
| **Data Engineering** | 8 | Modeling, ETL, warehousing, streaming, data privacy |
| **DevOps** | 8 | Containers, IaC, secrets, DR, observability, cloud cost |
| **Mobile** | 8 | Architecture, React Native, offline-first, push, app store |
| **Systems** | 6 | Algorithms, distributed systems, concurrency, protocols |
| **Standalone** | 15 | Architecture, security, auth, CI/CD, monitoring, testing |

### Councils (6 review panels)
Marketing, Finance, Legal, AI Governance, Tech, and Blockchain councils. Each assembles domain experts for structured review with cross-member deliberation and devil's advocate challenge.

## Cross-Skill Interaction

Skills interact through three protocols:

### 1. Handoff Protocol
Skills write decisions to `.claude/outputs/[skill-name].md`. Downstream skills read these files.

```
api-design → writes API contract → auth-design reads it
database-schema → writes schema → backend skills read it
```

### 2. Research Protocol
Every skill searches for latest versions before executing:
```
WebSearch: "[technology] latest version [current year]"
WebFetch: official docs for current API patterns
```

### 3. Orchestrator
The `setup/orchestrator` skill maps any task to a skill sequence with dependency order.

## Workflow Recipes

20 pre-built multi-skill sequences. See [workflows/README.md](workflows/README.md).

| Workflow | Skills | Domain |
|----------|--------|--------|
| New Product Launch | 9 | Finance → Marketing |
| Landing Page Optimization | 6 | Marketing |
| Full-Stack Feature | 8 | Tech (Frontend + Backend) |
| AI Feature with RAG | 7 | AI Engineering + Governance |
| DeFi Protocol | 6 | Blockchain |
| Mobile App Launch | 7 | Mobile + DevOps |
| Data Platform | 6 | Data + DevOps |
| Design System | 5 | UI + Frontend |
| Real-Time Feature | 5 | Backend + Frontend |
| Infrastructure Setup | 6 | DevOps |
| + 10 more... | | |

## License

MIT
