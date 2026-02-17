# Frontend SEO

Implement technical SEO best practices for client-rendered and server-rendered applications.

## Context Sync Protocol

Read existing meta tags, sitemap, and rendering strategy.

## Decision Tree: SEO Priority

```
What type of page?
├── Landing page / Homepage
│   └── Critical: Full SSR/SSG, all meta tags, structured data, Core Web Vitals
├── Content page (blog, docs)
│   └── High: SSG, canonical URL, heading hierarchy, internal linking
├── Product / Listing page
│   └── High: SSR/ISR, structured data, unique meta per item
├── Dashboard / App (authenticated)
│   └── Low: noindex, focus on performance not SEO
└── Error pages (404, 500)
    └── Medium: Custom 404 with navigation, proper status codes
```

## SEO Checklist

### Meta Tags
```html
<title>Primary Keyword - Brand (50-60 chars)</title>
<meta name="description" content="Compelling description with keywords (150-160 chars)">
<link rel="canonical" href="https://yourdomain.com/page">
<meta name="robots" content="index, follow">

<!-- Open Graph -->
<meta property="og:title" content="...">
<meta property="og:description" content="...">
<meta property="og:image" content="https://yourdomain.com/og-image.png">
<meta property="og:type" content="website">

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image">
```

### Technical SEO
- [ ] SSR or SSG for all public pages (not CSR)
- [ ] Sitemap.xml generated and submitted
- [ ] robots.txt properly configured
- [ ] Canonical URLs on all pages
- [ ] Heading hierarchy (single H1, logical H2-H6)
- [ ] Image alt text on all images
- [ ] Internal linking between related content
- [ ] 301 redirects for changed URLs
- [ ] Core Web Vitals passing (LCP, FID, CLS)
- [ ] Structured data (JSON-LD) for rich results

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Rendering** | SSR/SSG for all public pages | Mostly server-rendered | CSR for public pages |
| **Meta tags** | Unique title/description per page + OG | Basic meta | Missing meta |
| **Structured data** | JSON-LD for all applicable pages | Some structured data | None |
| **Technical** | Sitemap, robots, canonicals, redirects | Most in place | Missing basics |
| **Performance** | Core Web Vitals green | Mostly passing | Failing CWV |
| **Content** | Proper headings, alt text, internal links | Some optimization | No content SEO |
| **Monitoring** | Search Console, rank tracking, CWV alerts | Basic monitoring | No monitoring |

**28+ = SEO-optimized | 21-27 = Gaps in coverage | <21 = Invisible to search**
