# Accessibility (a11y)

Build accessible web applications that work for everyone, meeting WCAG 2.1 AA standards.

## Context Sync Protocol

Read existing component library and accessibility patterns.

## Decision Tree: Accessibility Approach

```
What are you building?
├── Custom interactive component (dropdown, modal, tabs)
│   └── Use Radix UI / Headless UI primitives
│       → Built-in ARIA, keyboard, focus management
├── Content page (text, images, links)
│   └── Semantic HTML + alt text + heading hierarchy
├── Form
│   └── Labels, error announcements, keyboard submission
├── Data visualization (charts, graphs)
│   └── Alternative text descriptions + data tables
├── Media (video, audio)
│   └── Captions, transcripts, audio descriptions
└── Navigation
    └── Landmarks, skip links, focus management on route change
```

## WCAG 2.1 AA Quick Reference

| Principle | Key Requirements |
|-----------|-----------------|
| **Perceivable** | Alt text, color contrast (4.5:1), captions, responsive zoom |
| **Operable** | Keyboard accessible, no time limits, skip navigation, focus visible |
| **Understandable** | Readable, predictable, error identification and correction |
| **Robust** | Valid HTML, ARIA where needed, works with assistive tech |

## Common Fixes

| Issue | Fix |
|-------|-----|
| Images without alt text | `<img alt="Description of image content">` |
| Low color contrast | Use 4.5:1 for text, 3:1 for large text |
| No keyboard support | Add `tabIndex`, `onKeyDown`, focus management |
| No focus indicator | `:focus-visible` styles (never `outline: none`) |
| Custom dropdown | Use Radix `Select` or add full ARIA role/state |
| Page title missing | `<title>` and `<h1>` on every page |
| No skip link | `<a href="#main" class="sr-only focus:not-sr-only">` |
| Auto-playing media | Never auto-play with sound. Provide controls. |

## Testing Checklist

- [ ] Keyboard navigation: Tab through all interactive elements
- [ ] Screen reader: VoiceOver/NVDA announces everything correctly
- [ ] Zoom: Page works at 200% zoom
- [ ] Color: Information not conveyed by color alone
- [ ] axe DevTools: 0 violations
- [ ] Lighthouse accessibility: 95+ score

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Semantic HTML** | Proper elements, landmarks, headings | Some semantics | Divs everywhere |
| **Keyboard** | Full keyboard navigation, visible focus | Tab works | Not keyboard accessible |
| **Screen reader** | Properly announced, ARIA where needed | Mostly readable | Broken for screen readers |
| **Color/contrast** | 4.5:1+ everywhere, no color-only info | Mostly compliant | Low contrast, color-dependent |
| **Forms** | Labeled, errors announced, keyboard submit | Labels present | Unlabeled inputs |
| **Dynamic content** | Live regions, focus management on changes | Some announcements | Silent updates |
| **Testing** | Automated + manual + screen reader testing | Automated only | No testing |

**28+ = WCAG 2.1 AA compliant | 21-27 = Known gaps | <21 = Inaccessible**
