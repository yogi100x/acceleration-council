# Security Hardening

Systematically secure applications against OWASP Top 10 and common attack vectors.

## Context Sync Protocol

1. Read existing security measures in the codebase
2. Read `.claude/product-marketing-context.md` for data sensitivity level

## OWASP Top 10 Checklist

| # | Vulnerability | Defense | Priority |
|---|--------------|---------|----------|
| A01 | **Broken Access Control** | RBAC, RLS, authorization checks on every endpoint | Critical |
| A02 | **Cryptographic Failures** | TLS everywhere, bcrypt/argon2 for passwords, encrypt PII at rest | Critical |
| A03 | **Injection** | Parameterized queries, input validation, CSP headers | Critical |
| A04 | **Insecure Design** | Threat modeling, security requirements, secure defaults | High |
| A05 | **Security Misconfiguration** | Hardened defaults, remove debug modes, security headers | High |
| A06 | **Vulnerable Components** | Dependency scanning, update policy, SCA tools | High |
| A07 | **Auth Failures** | MFA, rate limiting, session management, password policy | Critical |
| A08 | **Data Integrity Failures** | Signed updates, integrity checks, CI/CD security | Medium |
| A09 | **Logging Failures** | Security event logging, tamper-proof logs, alerting | High |
| A10 | **SSRF** | URL validation, allowlists, network segmentation | Medium |

## Security Headers

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'; script-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

## Input Validation Rules

```
Every external input must be:
1. Validated (type, format, range, length)
2. Sanitized (escape special characters for context)
3. Parameterized (never string-concatenate into queries)

Trust boundaries:
- User input: NEVER trust
- Internal API calls: Trust if authenticated
- Database results: Trust for type, validate for business rules
- Environment variables: Trust at startup, validate format
```

## Anti-Patterns to Avoid

- **T11: Security as afterthought** — Build security in from the start, not as a patch.
- **T12: Storing secrets in code** — Use environment variables or secret managers.
- **T13: No dependency scanning** — Known vulnerabilities in dependencies are easy wins for attackers.
- **T14: Disabling security in development** — Dev should mirror production security where possible.

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **OWASP coverage** | All Top 10 addressed with specific measures | Top 5 addressed | Ad-hoc security |
| **Input validation** | All inputs validated at trust boundaries | Key inputs validated | No systematic validation |
| **Authentication** | MFA, rate limiting, secure session management | Basic auth | Weak auth |
| **Authorization** | Per-resource checks, RLS, tested edge cases | Role-based checks | No authorization |
| **Dependency security** | Automated scanning, update policy, SCA | Periodic manual review | No scanning |
| **Security headers** | All recommended headers configured | Some headers | No security headers |
| **Secrets management** | Secret manager, rotation, no secrets in code | Environment variables | Secrets in code |

**28+ = Hardened | 21-27 = Known gaps | <21 = Vulnerable**

## Cross-Skill References

- **Upstream:** `system-architecture` (security architecture), `auth-design` (authentication)
- **Downstream:** `incident-response-plan` (security incidents), `compliance-checklist` (security compliance)
- **Council:** Submit to `council-review` for security review
