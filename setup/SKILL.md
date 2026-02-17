---
name: setup
version: 1.0.0
description: |
  Generates and maintains project context files used by all execution skills. Run this first after installing acceleration-council. Also use to update context when your business changes. Triggers: "setup my project context", "update context:", "review my project context", "setup context".
---

# Project Context Setup

You manage the project context files that all execution skills depend on. You have three modes:

1. **Initial Setup** — Generate all context files through conversation
2. **Update** — Modify specific context fields based on new information
3. **Review** — Check all context files for staleness and prompt for updates

---

## Context Files

| File | Used By | Purpose |
|------|---------|---------|
| `.claude/product-marketing-context.md` | All marketing skills | Product, audience, positioning, competitors |
| `.claude/finance-context.md` | All finance skills | Revenue, costs, funding, unit economics |
| `.claude/legal-context.md` | All legal skills | Entity, jurisdictions, data types, compliance |
| `.claude/ai-context.md` | All AI governance skills | Models, risk classification, data practices |
| `.claude/tech-context.md` | All tech skills | Stack, architecture, constraints, infrastructure |

---

## Mode 1: Initial Setup

When user says "setup my project context" or similar:

### Step 1: Business Basics (generates all files)

Ask these questions **one at a time**. Use multiple choice where possible.

**Q1: What type of business?**
- SaaS (B2B)
- SaaS (B2C)
- Marketplace (two-sided)
- E-commerce
- Developer tools / API
- Agency / Services
- Mobile app
- Other (describe)

**Q2: What stage?**
- Pre-revenue / Idea
- Pre-seed / MVP
- Seed ($0-$1M ARR)
- Series A ($1M-$10M ARR)
- Series B ($10M-$50M ARR)
- Growth ($50M+ ARR)
- Bootstrapped & profitable

**Q3: What industry/vertical?**
(Open-ended — e.g., "healthcare recruiting", "fintech", "developer tools")

**Q4: Who is your primary customer?**
(Open-ended — e.g., "Hospital HR directors", "Solo developers", "E-commerce store owners")

**Q5: What does your product do in one sentence?**
(Open-ended)

**Q6: Which context files do you need?**
- All 5 (recommended)
- Marketing + Finance only
- Marketing only
- Custom selection (list which ones)

### Step 2: Context-Specific Questions

Based on which files the user needs, ask targeted questions for each. Only ask what isn't already obvious from the basics.

#### For product-marketing-context.md:
- What's your current pricing? (tiers, amounts)
- Who are your top 3 competitors?
- What's your primary acquisition channel?
- What's your key differentiator in one sentence?

#### For finance-context.md:
- Current ARR or MRR? (or "pre-revenue")
- Monthly burn rate? (or "profitable")
- Total funding raised? (or "bootstrapped")
- Primary billing tool? (Stripe, Paddle, etc.)

#### For legal-context.md:
- Entity type? (C-Corp, LLC, Ltd, etc.)
- Operating jurisdictions? (US, EU, both, other)
- Do you handle PII? Health data? Financial data?
- Any compliance certifications? (SOC 2, HIPAA, PCI, ISO 27001)

#### For ai-context.md:
- Do you use AI/ML in your product? (yes → which models)
- Does AI make automated decisions about people? (scoring, ranking, filtering)
- What data does AI process? (text, images, video, PII)
- (Skip this file entirely if user doesn't use AI)

#### For tech-context.md:
- Primary framework? (Next.js, Rails, Django, etc.)
- Database? (PostgreSQL, MongoDB, MySQL, etc.)
- Hosting? (Vercel, AWS, GCP, etc.)
- Monorepo or single repo?

### Step 3: Generate Files

Generate each context file with this format:

```markdown
# [Council] Context
<!-- Last updated: YYYY-MM-DD -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: monthly -->

## [Section]
- Field: Value <!-- updated YYYY-MM-DD -->
- Field: Value <!-- updated YYYY-MM-DD -->

...

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| YYYY-MM-DD | All | — | Initial setup | setup |
```

### Step 4: Confirm

Show the user a summary:
```
Created 5 context files:
  .claude/product-marketing-context.md (12 fields)
  .claude/finance-context.md (8 fields)
  .claude/legal-context.md (10 fields)
  .claude/ai-context.md (6 fields)
  .claude/tech-context.md (9 fields)

All execution skills will now read these files automatically.
Run 'update context: [change]' anytime to update.
Run 'review my project context' monthly to check freshness.
```

---

## Mode 2: Update Context

When user says "update context: [change]" or a skill proposes an update:

1. Parse what changed from the user's message
2. Read ALL context files to find which ones are affected
3. Show a clear diff:
   ```
   finance-context.md:
   - ARR: $5M → $7.2M
   - Stage: Series A → Series A (no change)

   product-marketing-context.md:
   - Pricing: Free + Pro ($29/mo) → Free + Pro ($29/mo) + Enterprise (custom)
   ```
4. Ask user to confirm
5. Update the files, add `<!-- updated YYYY-MM-DD -->` to changed fields
6. Append to changelog in each modified file

### Handling Skill-Triggered Updates

When another skill detects context drift and proposes an update, it passes the proposed changes to this skill's update mode. The format is:

```
Proposed context update from [skill-name]:
- File: [context-file]
- Field: [field-name]
- Current: [old-value]
- Proposed: [new-value]
- Reason: [why the skill thinks this changed]
```

Always show the user and ask for confirmation. Never silently update.

---

## Mode 3: Review Context

When user says "review my project context":

1. Read all context files
2. Check `<!-- updated -->` dates on each field
3. Flag fields older than the review interval (default: 30 days for finance, 60 days for others)
4. Ask targeted questions about stale fields:
   ```
   Your finance context has 3 fields that haven't been updated in 45+ days:

   1. ARR: $5M — still accurate?
   2. Burn rate: $200K/month — still accurate?
   3. CAC: $450 — still accurate?

   Which ones changed? (or "all current" to skip)
   ```
5. Update confirmed changes
6. Reset review dates on confirmed-current fields

### Staleness Thresholds

| Context File | Review Interval | Rationale |
|-------------|----------------|-----------|
| finance-context.md | 30 days | Financial metrics change monthly |
| product-marketing-context.md | 60 days | Positioning changes quarterly |
| legal-context.md | 90 days | Legal structure rarely changes |
| ai-context.md | 60 days | Models and data practices evolve |
| tech-context.md | 90 days | Stack changes are infrequent |

---

## Context File Templates

### product-marketing-context.md

```markdown
# Product Marketing Context
<!-- Last updated: {date} -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: 60 days -->

## Company
- Name: {company_name} <!-- updated {date} -->
- Type: {business_type} <!-- updated {date} -->
- Stage: {stage} <!-- updated {date} -->
- Industry: {industry} <!-- updated {date} -->

## Product
- Description: {one_sentence} <!-- updated {date} -->
- Key differentiator: {differentiator} <!-- updated {date} -->
- Primary use case: {use_case} <!-- updated {date} -->

## Audience
- Primary customer: {customer} <!-- updated {date} -->
- Customer pain points: {pains} <!-- updated {date} -->
- Customer language: {language_they_use} <!-- updated {date} -->

## Positioning
- Category: {category} <!-- updated {date} -->
- Alternatives: {competitors} <!-- updated {date} -->
- Why us: {why_choose_us} <!-- updated {date} -->

## Pricing
- Model: {pricing_model} <!-- updated {date} -->
- Tiers: {tiers_and_prices} <!-- updated {date} -->

## Channels
- Primary acquisition: {primary_channel} <!-- updated {date} -->
- Secondary channels: {other_channels} <!-- updated {date} -->

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| {date} | All | — | Initial setup | setup |
```

### finance-context.md

```markdown
# Finance Context
<!-- Last updated: {date} -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: 30 days -->

## Business Model
- Type: {business_type} <!-- updated {date} -->
- Revenue model: {revenue_model} <!-- updated {date} -->
- Current ARR/MRR: {arr} <!-- updated {date} -->
- Growth rate: {growth_rate} <!-- updated {date} -->

## Unit Economics
- CAC (blended): {cac} <!-- updated {date} -->
- LTV: {ltv} <!-- updated {date} -->
- LTV:CAC ratio: {ratio} <!-- updated {date} -->
- Gross margin: {margin} <!-- updated {date} -->
- Payback period: {payback} <!-- updated {date} -->

## Costs
- Monthly burn rate: {burn} <!-- updated {date} -->
- Runway: {runway} <!-- updated {date} -->
- Largest cost centers: {costs} <!-- updated {date} -->

## Funding
- Stage: {funding_stage} <!-- updated {date} -->
- Total raised: {total_raised} <!-- updated {date} -->
- Last valuation: {valuation} <!-- updated {date} -->
- Next milestone: {next_milestone} <!-- updated {date} -->

## Tools
- Billing: {billing_tool} <!-- updated {date} -->
- Accounting: {accounting_tool} <!-- updated {date} -->
- Payroll: {payroll_tool} <!-- updated {date} -->

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| {date} | All | — | Initial setup | setup |
```

### legal-context.md

```markdown
# Legal Context
<!-- Last updated: {date} -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: 90 days -->

## Entity
- Type: {entity_type} <!-- updated {date} -->
- Jurisdiction of incorporation: {jurisdiction} <!-- updated {date} -->
- Operating jurisdictions: {operating} <!-- updated {date} -->

## Data & Privacy
- Data types handled: {data_types} <!-- updated {date} -->
- GDPR applicable: {yes_no} <!-- updated {date} -->
- CCPA applicable: {yes_no} <!-- updated {date} -->
- Other regulations: {other_regs} <!-- updated {date} -->

## Compliance
- Certifications: {certs} <!-- updated {date} -->
- In progress: {in_progress} <!-- updated {date} -->
- Required by customers: {required} <!-- updated {date} -->

## Agreements
- Standard customer contract: {contract_type} <!-- updated {date} -->
- Key vendors: {vendors} <!-- updated {date} -->
- Employee vs contractor: {workforce} <!-- updated {date} -->

## IP
- Trademarks: {trademarks} <!-- updated {date} -->
- Patents: {patents} <!-- updated {date} -->
- Open source used: {oss} <!-- updated {date} -->
- Open source published: {oss_published} <!-- updated {date} -->

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| {date} | All | — | Initial setup | setup |
```

### ai-context.md

```markdown
# AI Context
<!-- Last updated: {date} -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: 60 days -->

## Models Used
- Model 1: {model_name} — {purpose} <!-- updated {date} -->
- Model 2: {model_name} — {purpose} <!-- updated {date} -->

## Risk Classification
- EU AI Act category: {risk_level} <!-- updated {date} -->
- Automated decisions about people: {yes_no_what} <!-- updated {date} -->
- Human oversight mechanism: {mechanism} <!-- updated {date} -->

## Data Practices
- Data processed by AI: {data_types} <!-- updated {date} -->
- Training data: {own_or_pretrained} <!-- updated {date} -->
- Data retention for AI: {retention_policy} <!-- updated {date} -->

## Monitoring
- Bias testing: {yes_no_frequency} <!-- updated {date} -->
- Performance monitoring: {approach} <!-- updated {date} -->
- Incident response: {has_plan} <!-- updated {date} -->

## Vendors
- AI providers: {providers} <!-- updated {date} -->
- SLAs in place: {yes_no} <!-- updated {date} -->

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| {date} | All | — | Initial setup | setup |
```

### tech-context.md

```markdown
# Tech Context
<!-- Last updated: {date} -->
<!-- Updated by: setup skill (initial) -->
<!-- Review interval: 90 days -->

## Stack
- Language: {language} <!-- updated {date} -->
- Framework: {framework} <!-- updated {date} -->
- Database: {database} <!-- updated {date} -->
- Auth: {auth_provider} <!-- updated {date} -->
- API style: {rest_graphql} <!-- updated {date} -->

## Architecture
- Repo structure: {mono_multi} <!-- updated {date} -->
- Apps/services: {list} <!-- updated {date} -->
- Background jobs: {tool} <!-- updated {date} -->

## Infrastructure
- Hosting: {hosting} <!-- updated {date} -->
- CDN: {cdn} <!-- updated {date} -->
- CI/CD: {cicd} <!-- updated {date} -->
- Monitoring: {monitoring} <!-- updated {date} -->

## Constraints
- Scale requirements: {scale} <!-- updated {date} -->
- Accessibility: {wcag_level} <!-- updated {date} -->
- Browser support: {browsers} <!-- updated {date} -->
- Performance budget: {budget} <!-- updated {date} -->

## Changelog
| Date | Field | Old Value | New Value | Updated By |
|------|-------|-----------|-----------|------------|
| {date} | All | — | Initial setup | setup |
```

---

## Cross-Skill Integration

Every execution skill includes a standard "Context Sync Protocol" section. When a skill detects that new information was provided during execution that differs from or extends the context files, it should:

1. Note the specific discrepancy
2. After completing its primary task, show the proposed update
3. Ask the user: "Update context? [Yes] [No] [Edit first]"
4. If yes, update the file following the format above (add date comments, append changelog)

### What Skills Should Track

| Context File | Fields That Change Often |
|-------------|------------------------|
| product-marketing-context.md | Pricing, competitors, positioning, channels |
| finance-context.md | ARR, burn rate, CAC, LTV, funding stage |
| legal-context.md | Compliance certifications, new jurisdictions |
| ai-context.md | New models, risk classification changes |
| tech-context.md | New services, framework upgrades, tool changes |
