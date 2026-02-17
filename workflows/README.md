# Workflow Recipes

Multi-skill sequences for common tasks. Each workflow chains skills in order, passing outputs as inputs to the next skill.

## How to Use

1. Start with the first skill in the sequence
2. Complete its checklist/rubric
3. Each skill writes to `.claude/outputs/[skill-name].md`
4. Next skill reads upstream outputs automatically
5. Continue until workflow complete

## Available Workflows

### 1. New Product Launch
**Skills:** `setup` → `financial-model` → `pricing-model` → `pricing-strategy` → `launch-strategy` → `content-strategy` → `email-sequence` → `paid-ads` → `analytics-tracking`

**Description:** End-to-end launch from financial modeling through go-to-market execution.

### 2. Landing Page Optimization
**Skills:** `page-cro` → `copywriting` → `seo-audit` → `schema-markup` → `ab-test-setup` → `analytics-tracking`

**Description:** Audit, rewrite, optimize, and test a landing page for maximum conversion.

### 3. SaaS Pricing Overhaul
**Skills:** `unit-economics` → `pricing-model` → `pricing-strategy` → `paywall-upgrade-cro` → `ab-test-setup`

**Description:** From unit economics analysis through pricing page optimization and testing.

### 4. Content Marketing Engine
**Skills:** `content-strategy` → `seo-audit` → `programmatic-seo` → `copywriting` → `social-content` → `email-sequence`

**Description:** Build a content machine from strategy through distribution.

### 5. AI Feature Deployment
**Skills:** `prompt-engineering` → `model-card` → `bias-testing` → `ai-ethics-policy` → `human-oversight-protocol` → `ai-monitoring-setup`

**Description:** Responsible AI deployment from prompt design through production monitoring.

### 6. New SaaS Application
**Skills:** `system-architecture` → `database-schema` → `auth-design` → `api-design` → `error-handling` → `testing-strategy` → `ci-cd-pipeline` → `monitoring-setup` → `security-hardening`

**Description:** Full-stack technical foundation for a new application.

### 7. Compliance & Legal Foundation
**Skills:** `privacy-policy` → `terms-of-service` → `cookie-consent` → `acceptable-use-policy` → `compliance-checklist` → `incident-response-plan`

**Description:** Legal foundation for a new product or market entry.

### 8. Fundraising Preparation
**Skills:** `financial-model` → `unit-economics` → `kpi-dashboard` → `cap-table` → `fundraising-deck`

**Description:** Financial readiness package for investor conversations.

### 9. Growth Optimization Sprint
**Skills:** `analytics-tracking` → `signup-flow-cro` → `onboarding-cro` → `referral-program` → `ab-test-setup`

**Description:** Systematic growth optimization across the acquisition funnel.

### 10. Security & Compliance Audit
**Skills:** `security-hardening` → `auth-design` → `privacy-policy` → `data-processing-agreement` → `compliance-checklist` → `incident-response-plan`

**Description:** Comprehensive security and compliance review.

### 11. AI System Audit
**Skills:** `algorithmic-impact-assessment` → `bias-testing` → `training-data-audit` → `explainability-design` → `red-team-protocol` → `ai-monitoring-setup`

**Description:** Full audit of an AI system for fairness, safety, and compliance.

### 12. Marketplace Launch
**Skills:** `system-architecture` → `pricing-model` → `terms-of-service` → `dispute-resolution` → `acceptable-use-policy` → `launch-strategy` → `referral-program`

**Description:** Technical and business foundation for a two-sided marketplace.

---

## New Workflows (Tech Expansion)

### 13. Full-Stack Feature
**Skills:** `system-architecture` → `database-schema` → `api-design` → `backend/caching-strategy` → `frontend/react-patterns` → `frontend/state-management` → `testing-strategy` → `monitoring-setup`

**Description:** End-to-end feature from database through UI with caching and testing.

### 14. AI Feature with RAG
**Skills:** `ai-engineering/rag-architecture` → `ai-engineering/embedding-strategy` → `ai-engineering/vector-database` → `ai-engineering/prompt-management` → `ai-engineering/eval-pipeline` → `ai-engineering/ai-cost-optimization` → `ai-governance/ai-monitoring-setup`

**Description:** Build a RAG-powered AI feature with embeddings, evaluation, and monitoring.

### 15. DeFi Protocol
**Skills:** `blockchain/smart-contract-design` → `blockchain/smart-contract-security` → `blockchain/tokenomics` → `blockchain/gas-optimization` → `blockchain/defi-patterns` → `blockchain/dao-governance`

**Description:** Design, secure, and deploy a DeFi protocol with governance.

### 16. Mobile App Launch
**Skills:** `mobile/mobile-architecture` → `mobile/react-native-patterns` → `mobile/mobile-auth` → `mobile/push-notification-design` → `mobile/offline-first-design` → `mobile/mobile-testing` → `mobile/app-store-optimization`

**Description:** From architecture through app store submission for a React Native app.

### 17. Data Platform
**Skills:** `data/data-modeling` → `data/etl-pipeline` → `data/data-warehouse` → `data/streaming-data` → `data/data-validation` → `data/data-privacy-engineering`

**Description:** Build a data platform from modeling through privacy-compliant warehousing.

### 18. Design System
**Skills:** `ui/design-system` → `ui/component-architecture` → `ui/theming` → `frontend/accessibility` → `ui/layout-patterns`

**Description:** Create a design system with accessible, themeable components.

### 19. Real-Time Feature
**Skills:** `backend/real-time-architecture` → `backend/queue-architecture` → `frontend/client-state-sync` → `backend/rate-limiting` → `monitoring-setup`

**Description:** Build real-time features with WebSockets, queues, and state sync.

### 20. Infrastructure Setup
**Skills:** `devops/containerization` → `devops/infrastructure-as-code` → `devops/secrets-management` → `devops/observability-platform` → `devops/scaling-strategy` → `devops/disaster-recovery`

**Description:** Production-ready infrastructure from containers through disaster recovery.
