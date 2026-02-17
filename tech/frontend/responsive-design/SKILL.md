# Responsive Design

Build interfaces that work seamlessly across mobile, tablet, and desktop screen sizes.

## Context Sync Protocol

Read existing breakpoints and layout patterns.

## Decision Tree: Responsive Approach

```
What layout pattern?
├── Content-focused (blog, docs, marketing)
│   └── Mobile-first, single column → multi-column
├── Dashboard (data-heavy, many panels)
│   └── Responsive grid with collapsible sidebar
├── App-like (navigation-heavy, interactive)
│   └── Bottom nav on mobile, sidebar on desktop
├── Email/marketing
│   └── Table-based layout (email client compatibility)
└── Data tables
    └── Horizontal scroll on mobile, or card view transformation
```

## Breakpoint Strategy

```css
/* Mobile-first (Tailwind defaults) */
sm: 640px    /* Large phone / small tablet */
md: 768px    /* Tablet */
lg: 1024px   /* Small desktop */
xl: 1280px   /* Desktop */
2xl: 1536px  /* Large desktop */

/* Design for mobile FIRST, then add complexity */
.grid {
  grid-template-columns: 1fr;          /* Mobile: single column */
}
@media (min-width: 768px) {
  .grid {
    grid-template-columns: 1fr 1fr;    /* Tablet: two columns */
  }
}
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: 1fr 1fr 1fr; /* Desktop: three columns */
  }
}
```

## Responsive Patterns

| Pattern | Mobile | Desktop |
|---------|--------|---------|
| **Navigation** | Hamburger menu or bottom nav | Sidebar or top nav |
| **Data table** | Card stack or horizontal scroll | Full table |
| **Sidebar + content** | Full-width content, drawer sidebar | Side-by-side |
| **Multi-column** | Single column, stacked | Grid columns |
| **Modal** | Full-screen sheet | Centered dialog |
| **Images** | `srcset` for appropriate size | Full resolution |
| **Typography** | `clamp()` for fluid sizing | Fixed sizes |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Mobile-first** | Designed for mobile, enhanced for desktop | Responsive but desktop-first | Not responsive |
| **Breakpoints** | Consistent, content-driven breakpoints | Standard breakpoints | No breakpoints |
| **Touch targets** | 44x44px minimum, adequate spacing | Mostly adequate | Tiny tap targets |
| **Typography** | Fluid type with clamp(), readable at all sizes | Fixed but reasonable | Unreadable on mobile |
| **Images** | Responsive with srcset, lazy loaded, proper sizing | Some optimization | Fixed-size images |
| **Navigation** | Appropriate pattern per screen size | Hamburger menu | Desktop nav on mobile |
| **Testing** | Tested on real devices + multiple breakpoints | Chrome DevTools only | Untested |

**28+ = Works everywhere | 21-27 = Rough on some sizes | <21 = Broken on mobile**
