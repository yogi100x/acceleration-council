# Bundle Optimization

Minimize JavaScript bundle size for faster page loads and better Core Web Vitals.

## Context Sync Protocol

Read current bundle size and performance budgets.

## Decision Tree: Optimization Target

```
What's the biggest issue?
├── Large initial bundle (>200KB gzipped)
│   ├── Code splitting: dynamic imports for routes and heavy components
│   ├── Tree shaking: ensure dead code is eliminated
│   └── Analyze: `next/bundle-analyzer` to find big modules
├── Large dependency (moment.js, lodash full import)
│   ├── Replace with lighter alternative (date-fns, lodash-es)
│   ├── Import only what's used: `import debounce from 'lodash/debounce'`
│   └── Check if you even need the dependency
├── Duplicate dependencies (same lib, different versions)
│   ├── Dedupe: `npm dedupe`
│   ├── Check `npm ls <package>` for version conflicts
│   └── Update packages to align versions
├── Fonts and CSS
│   ├── Subset fonts (only characters you use)
│   ├── PurgeCSS for unused styles (Tailwind does this by default)
│   └── Self-host fonts instead of Google Fonts CDN
└── Images and media
    ├── Next/Image for automatic optimization
    ├── WebP/AVIF format
    └── Lazy loading below the fold
```

## Quick Wins

| Technique | Impact | Effort |
|-----------|--------|--------|
| `next/dynamic` for heavy components | High | Low |
| Replace moment.js with date-fns | Medium | Low |
| Import specific lodash functions | Medium | Low |
| Add bundle analyzer to CI | High (visibility) | Low |
| Enable Tailwind purge (default) | High | None |
| Lazy load below-fold images | Medium | Low |
| Self-host fonts | Medium | Low |
| Use `import type` for types | Low | Low |

## Bundle Budget

```
Initial load budget:
  HTML: <50KB gzipped
  CSS: <50KB gzipped
  JS: <200KB gzipped (total first-load)
  Fonts: <100KB (2 weights max)
  Images: Lazy loaded, WebP, properly sized

Per-route additional JS:
  <50KB gzipped per route chunk
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Bundle size** | <200KB initial JS gzipped | <400KB | >400KB |
| **Code splitting** | Per-route + dynamic for heavy components | Per-route only | No splitting |
| **Tree shaking** | Verified, no dead code shipped | Mostly working | Dead code shipped |
| **Dependencies** | Audited, minimal, no duplicates | Some bloat | Unaudited, large |
| **Images** | WebP, lazy loaded, properly sized | Some optimization | Unoptimized |
| **Fonts** | Self-hosted, subsetted, display: swap | Google Fonts | Multiple full font files |
| **Monitoring** | Bundle size in CI, alerts on increase | Manual checks | No monitoring |

**28+ = Fast loading | 21-27 = Room for improvement | <21 = Slow loading**
