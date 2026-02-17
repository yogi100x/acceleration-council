# Animation Design

Implement performant, purposeful animations and transitions that enhance UX.

## Context Sync Protocol

Read existing animation patterns and performance budgets.

## Decision Tree: Animation Approach

```
What's the animation purpose?
├── Feedback (button press, form submit, hover)
│   └── CSS transitions (transform, opacity)
│       → 150-300ms, ease-out
├── State change (expand/collapse, show/hide)
│   └── CSS transitions + height animation
│       → 200-400ms, ease-in-out
├── Entrance/exit (modal, toast, page)
│   └── Framer Motion or CSS @keyframes
│       → Enter: ease-out, Exit: ease-in, shorter
├── Scroll-triggered (parallax, reveal)
│   └── Intersection Observer + CSS classes
│       → Use will-change sparingly
├── Complex choreography (page transitions, onboarding)
│   └── Framer Motion AnimatePresence + layout animations
│       → Coordinate multiple elements
└── Data visualization (charts updating, number ticking)
    └── requestAnimationFrame or animation library
        → 60fps minimum, reduce motion support
```

## Performance Rules

| Rule | Why |
|------|-----|
| **Only animate `transform` and `opacity`** | These are GPU-composited (no layout/paint) |
| **Use `will-change` sparingly** | Over-use wastes GPU memory |
| **Respect `prefers-reduced-motion`** | Users with motion sensitivity |
| **Keep animations <400ms** | Longer feels sluggish |
| **Avoid animating `width`/`height`/`top`/`left`** | Triggers layout recalculation |
| **Use `requestAnimationFrame`** for JS animations | Syncs with browser refresh |

## Reduced Motion Support

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Purpose** | Every animation serves UX purpose | Most are purposeful | Gratuitous animation |
| **Performance** | 60fps, GPU-composited only | Mostly smooth | Janky/laggy |
| **Duration** | Appropriate timing per type | Reasonable | Too slow or instant |
| **Reduced motion** | Full `prefers-reduced-motion` support | Some support | Ignored |
| **Consistency** | Consistent easing and duration across app | Mostly consistent | Random animations |
| **Loading** | Skeleton screens, progressive content | Basic spinners | Blank screen |
| **Library choice** | Appropriate tool (CSS for simple, Framer for complex) | One tool for all | No system |

**28+ = Polished interactions | 21-27 = Functional | <21 = Distracting or janky**
