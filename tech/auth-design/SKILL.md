# Auth Design

Design authentication and authorization systems that are secure, scalable, and user-friendly.

## Context Sync Protocol

1. Read existing auth patterns in the codebase
2. Read `.claude/product-marketing-context.md` for user types and access requirements

## Decision Tree: Auth Strategy

```
What's your product type?
├── Consumer app (B2C)
│   └── Social login + email/password + magic link
├── Business app (B2B)
│   └── Email/password + SSO (SAML/OIDC) + MFA
├── API / Developer platform
│   └── API keys + OAuth 2.0 for third-party integrations
├── Multi-tenant SaaS
│   └── Per-tenant SSO + role-based access + organization management
└── Internal tool
    └── SSO via corporate identity provider
```

## Authentication Patterns

| Method | Security | UX | Best For |
|--------|----------|-----|----------|
| **Email + Password** | Medium (with MFA: High) | Familiar | Universal fallback |
| **Magic Link** | High | Excellent | Low-friction apps |
| **Social Login** | Depends on provider | Excellent | Consumer apps |
| **SSO (SAML/OIDC)** | High | Good (for enterprise) | B2B, enterprise |
| **API Key** | Medium | N/A (programmatic) | Developer APIs |
| **OAuth 2.0** | High | Medium | Third-party integrations |
| **Passkeys/WebAuthn** | Very High | Good | Security-first apps |

## Authorization Patterns

| Pattern | Complexity | Best For |
|---------|-----------|----------|
| **Role-Based (RBAC)** | Low | Most SaaS apps (admin, member, viewer) |
| **Attribute-Based (ABAC)** | Medium | Complex rules (department + role + resource) |
| **Row-Level Security (RLS)** | Medium | Multi-tenant database isolation |
| **Permission-Based** | Medium | Granular feature access |
| **Organization-Based** | Medium | Multi-tenant with team management |

## JWT Token Strategy

```
Access Token:
- Short-lived (15-60 minutes)
- Contains: user_id, role, organization_id
- Stored: Memory (not localStorage)
- Validated: On every API request

Refresh Token:
- Long-lived (7-30 days)
- Contains: Minimal (session_id)
- Stored: HttpOnly, Secure, SameSite cookie
- Used: To get new access tokens
- Rotated: On each use (detect reuse = compromise)
```

## Security Checklist

- [ ] Passwords hashed with bcrypt/argon2 (never MD5/SHA)
- [ ] Rate limiting on login/signup endpoints
- [ ] Account lockout after N failed attempts
- [ ] CSRF protection on auth endpoints
- [ ] Secure cookie flags (HttpOnly, Secure, SameSite)
- [ ] Token rotation on privilege changes
- [ ] Session invalidation on password change
- [ ] MFA option for sensitive accounts
- [ ] Audit logging of auth events

## Anti-Patterns to Avoid

- **T11: Storing JWTs in localStorage** — XSS can steal them. Use HttpOnly cookies.
- **T12: No token refresh strategy** — Long-lived access tokens are a security risk.
- **T13: Rolling your own auth** — Use battle-tested libraries (Supabase Auth, Auth.js, Clerk).
- **T14: No audit logging** — Auth events must be logged for security investigation.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Authentication methods** | Appropriate methods for user types with MFA | Basic email/password | Single method, no MFA |
| **Authorization model** | RBAC/ABAC with RLS, tested edge cases | Basic role checks | No authorization model |
| **Token management** | Short-lived tokens, rotation, secure storage | Reasonable token strategy | Long-lived tokens in localStorage |
| **Session security** | CSRF protection, secure cookies, invalidation | Some protections | Minimal security |
| **Multi-tenancy** | Proper tenant isolation at every layer | Mostly isolated | Isolation gaps |
| **Audit trail** | All auth events logged with context | Login/logout logged | No logging |
| **Recovery flows** | Password reset, account recovery, locked account | Basic reset | No recovery |

**28+ = Secure auth system | 21-27 = Needs hardening | <21 = Security risk**

## Cross-Skill References

- **Upstream:** `system-architecture` (auth layer), `security-hardening` (security requirements)
- **Downstream:** `api-design` (auth middleware), `testing-strategy` (auth test cases)
- **Council:** Submit to `council-review` for security review
