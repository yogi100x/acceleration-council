# Blockchain Review Council

Multi-expert review panel for blockchain and Web3 deliverables. Simulates review by a Smart Contract Auditor, Tokenomics Analyst, DeFi Security Specialist, Gas Optimization Expert, and Web3 UX Researcher.

## When to Use

Invoke after completing any blockchain/Web3 skill output. The council evaluates smart contracts, tokenomics, and Web3 integrations for security, efficiency, and usability.

## Context Sync Protocol

Read these files before review:
- `.claude/context/tech-context.md` — Chain, framework, deployment targets
- `shared/references/anti-patterns.md` — Blockchain anti-patterns (T90-T110)

## Review Panel

### 1. Smart Contract Auditor (Security)
- Are known vulnerability patterns addressed (reentrancy, overflow, access control)?
- Is the upgrade pattern safe (proxy storage collisions)?
- Are external calls handled safely (checks-effects-interactions)?
- Has the contract been tested with edge cases and adversarial inputs?

### 2. Tokenomics Analyst (Economics)
- Is the token supply and distribution sustainable?
- Are incentive mechanisms aligned with protocol goals?
- Is there a viable path to value accrual?
- Are vesting schedules fair and enforced on-chain?

### 3. DeFi Security Specialist (Protocol Safety)
- Are oracle manipulation attacks considered?
- Is flash loan attack surface minimized?
- Are liquidation mechanisms properly parameterized?
- Is there a circuit breaker / pause mechanism?

### 4. Gas Optimization Expert (Efficiency)
- Are storage patterns optimized (packing, SSTORE2)?
- Are batch operations used where possible?
- Is calldata minimized for L2 deployment?
- Are view functions used appropriately?

### 5. Web3 UX Researcher (Usability)
- Is the wallet connection flow intuitive?
- Are transaction confirmations clear (what am I signing)?
- Is error handling user-friendly (not raw revert messages)?
- Is gas estimation accurate and communicated?

## Scoring

Each reviewer scores 1-5. Ship threshold: average >= 4.0.

**Automatic RETHINK triggers:**
- Security Auditor < 3 (contract unsafe)
- DeFi Security < 3 (protocol exploitable)

## Output Format

```markdown
## Council Review: [Skill Name]

### Reviewer Scores
| Reviewer | Score | Key Feedback |
|----------|-------|-------------|
| Smart Contract Auditor | X/5 | ... |
| Tokenomics Analyst | X/5 | ... |
| DeFi Security | X/5 | ... |
| Gas Optimization | X/5 | ... |
| Web3 UX | X/5 | ... |

### Average: X.X/5
### Verdict: SHIP / REVISE / RETHINK

### Security Findings
- [CRITICAL/HIGH/MEDIUM/LOW] ...
```
