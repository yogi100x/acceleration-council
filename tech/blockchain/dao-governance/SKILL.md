# DAO Governance

Design decentralized governance: voting, proposals, treasury, and delegation systems.

## Research Protocol
### Web Search
- "DAO governance patterns [current year]"
- "OpenZeppelin Governor contract [current year]"
- "on-chain vs off-chain voting comparison [current year]"
### WebFetch
- https://docs.openzeppelin.com/contracts/governance

## Decision Tree: Governance Model

```
What type of DAO?
├── Protocol DAO (DeFi, infrastructure)
│   └── Token-weighted voting + timelock
│       → Governor + TimelockController + token
├── Investment DAO
│   └── Share-based voting + ragequit
│       → Moloch-style: proposal → vote → execute or ragequit
├── Grants DAO
│   └── Multi-sig or committee-based
│       → Gnosis Safe with signers, or Grants committee election
├── Social DAO (community, creator)
│   └── Reputation-based or NFT-gated
│       → Non-transferable tokens or contribution-based weight
└── Sub-DAO (working group within larger DAO)
    └── Delegated authority with budget
        → Parent DAO grants budget, sub-DAO decides allocation
```

## Voting Mechanisms

| Mechanism | Pros | Cons | Best For |
|-----------|------|------|----------|
| **Token-weighted** | Simple, liquid | Plutocratic, whale dominated | Protocol DAOs |
| **Quadratic** | Reduces whale power | Sybil vulnerable | Grants, public goods |
| **Conviction** | Continuous, no deadlines | Slow, complex UX | Budget allocation |
| **Optimistic** | Fast, low overhead | Needs active monitoring | Routine decisions |
| **Multi-sig** | Fast, trusted operators | Centralized | Treasury, emergency |

## Proposal Lifecycle

```
Draft → Discussion (forum) → Temperature Check (off-chain) →
On-chain Proposal → Voting Period → Timelock → Execution

Key parameters:
  Proposal threshold: Min tokens to propose (prevent spam)
  Quorum: Min participation for valid vote (e.g., 4% of supply)
  Voting period: Duration (e.g., 7 days)
  Timelock delay: Wait before execution (e.g., 48 hours)
```

## Security Considerations

- [ ] Timelock on all governance actions (prevent flash loan governance attacks)
- [ ] Snapshot block for voting power (prevent vote manipulation via transfers)
- [ ] Guardian/veto role for emergency (cancel malicious proposals)
- [ ] Quorum requirements prevent low-turnout attacks
- [ ] Delegation doesn't allow infinite delegation chains

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Voting mechanism** | Appropriate for DAO type, tested | Standard token voting | No governance |
| **Security** | Timelock, snapshot, guardian, quorum | Basic timelock | No protections |
| **Participation** | Delegation, off-chain discussion, easy UX | Basic voting UI | Complex/inaccessible |
| **Treasury** | Multi-sig + governance control, budget system | Basic multi-sig | Single key |
| **Proposal process** | Discussion → temp check → on-chain → timelock | Basic proposals | No formal process |
| **Upgradability** | Governance can upgrade its own parameters | Fixed parameters | No upgradability |
| **Documentation** | Constitution, process docs, public forum | Some docs | No documentation |

**28+ = Well-governed DAO | 21-27 = Basic governance | <21 = Governance theater**
