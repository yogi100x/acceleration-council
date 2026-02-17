# Email System

Design transactional and marketing email systems with deliverability, templates, and tracking.

## Research Protocol

### Web Search
- "transactional email best practices [current year]"
- "email deliverability DKIM SPF DMARC [current year]"
- "Resend vs SendGrid vs Postmark comparison [current year]"

### WebFetch
- https://resend.com/docs
- https://www.learndmarc.com/

---

## Decision Tree: Email Architecture

```
What type of email?
├── Transactional (password reset, receipts, notifications)
│   └── Dedicated transactional provider (Resend, Postmark)
│       → High deliverability, fast, per-email pricing
├── Marketing (newsletters, campaigns, drip sequences)
│   └── Marketing platform (ConvertKit, Mailchimp)
│       → List management, unsubscribe, analytics
├── Both
│   └── Separate providers for transactional vs marketing
│       → Different sending domains, different reputation
└── High volume (>100K/day)
    └── Dedicated IP + warm-up + monitoring
```

## Email Infrastructure

```
Authentication stack (ALL required for deliverability):
  SPF: Authorizes sending servers
  DKIM: Cryptographic signature on emails
  DMARC: Policy for handling authentication failures
  Return-Path: Bounce handling domain

Sending architecture:
  App → Email service (Resend/Postmark) → Recipient
  
  Templates: React Email or MJML (responsive, tested)
  Tracking: Open tracking pixel, click tracking links
  Bounce handling: Webhook for bounces → update user status
```

## Template Design

| Principle | Implementation |
|-----------|---------------|
| **Responsive** | MJML or React Email (not raw HTML) |
| **Plain text fallback** | Always include plain text version |
| **Preview text** | First 90 chars matter (preheader text) |
| **Unsubscribe** | One-click unsubscribe header (RFC 8058) |
| **Testing** | Test across Gmail, Outlook, Apple Mail |
| **Personalization** | Dynamic content with fallbacks |

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T38 | No SPF/DKIM/DMARC | Emails go to spam |
| T39 | Same domain for transactional and marketing | Marketing reputation affects transactional delivery |
| T40 | No unsubscribe mechanism | CAN-SPAM violation, spam reports |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Deliverability** | SPF+DKIM+DMARC, dedicated domain, warm-up | Basic authentication | No authentication |
| **Templates** | Responsive, tested across clients, plain text | Basic responsive | Raw HTML |
| **Tracking** | Open/click tracking, bounce handling | Basic tracking | No tracking |
| **Unsubscribe** | One-click, preference center | Unsubscribe link | No unsubscribe |
| **Reliability** | Queue-based sending, retry, idempotent | Basic retry | Fire-and-forget |
| **Testing** | Cross-client testing, spam score check | Basic testing | Untested |
| **Monitoring** | Delivery rate, open rate, bounce rate, spam complaints | Basic metrics | No monitoring |

**28+ = Professional email | 21-27 = Deliverability risk | <21 = Going to spam**

## Output Protocol

Write to `.claude/outputs/email-system.md`:
- Email provider chosen
- Authentication (SPF/DKIM/DMARC) configuration
- Template system
- Sending architecture
