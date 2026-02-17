# React Native Patterns

Navigation, native modules, performance (Hermes), Expo vs bare workflow. Decision frameworks and quality standards for mobile development.

## Research Protocol
### Web Search
- "react native patterns best practices [current year]"
- "react native patterns react native [current year]"

## Decision Tree

```
START: What platform and approach?
├── iOS + Android (same codebase)
│   └── React Native or Flutter
├── iOS + Android (separate codebases)
│   └── Native (Swift/Kotlin)
├── Mobile-first web
│   └── PWA with responsive design
└── Existing web app → mobile
    └── React Native (if React web) or Capacitor
```

## Key Patterns

| Pattern | When to Use | Trade-off |
|---------|-------------|-----------|
| **Simple** | MVP, basic features | Fast to build, limited |
| **Standard** | Production app | Good balance |
| **Advanced** | Performance-critical | Complex but optimized |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Performance** | Smooth 60fps, fast startup | Reasonable perf | Janky/slow |
| **UX** | Native feel, platform conventions | Mostly native | Web-wrapped feel |
| **Offline** | Core features work offline | Graceful offline message | Crashes offline |
| **Security** | Secure storage, cert pinning | Basic security | Tokens in plain text |
| **Testing** | Device matrix, CI, crash reporting | Some testing | Manual only |
| **Accessibility** | VoiceOver/TalkBack, dynamic text | Some a11y | Not accessible |
| **Distribution** | Automated builds, staged rollout | Manual builds | Ad-hoc distribution |

**28+ = Production-quality | 21-27 = Needs polish | <21 = Not ready for users**

## Output Protocol
Write to `.claude/outputs/react-native-patterns.md`:
- Architecture decisions
- Platform-specific considerations
- Performance targets
