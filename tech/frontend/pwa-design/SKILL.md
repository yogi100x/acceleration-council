# PWA Design

Build Progressive Web Apps with offline support, push notifications, and app-like experience.

## Context Sync Protocol

Read existing service worker configuration and offline requirements.

## Decision Tree: PWA Scope

```
What PWA features do you need?
├── Offline access (content available without internet)
│   └── Service worker + cache-first strategy
├── Push notifications
│   └── Service worker + Push API + notification server
├── Install prompt ("Add to Home Screen")
│   └── Web app manifest + service worker + HTTPS
├── Background sync (queue actions for later)
│   └── Service worker + Background Sync API
└── Full app-like experience
    └── All above + app shell architecture
```

## Caching Strategies

| Strategy | When | Behavior |
|----------|------|----------|
| **Cache-first** | Static assets (CSS, JS, images) | Serve from cache, update in background |
| **Network-first** | API data, dynamic content | Try network, fall back to cache |
| **Stale-while-revalidate** | Semi-static content | Serve cache immediately, update in background |
| **Network-only** | Authentication, payments | Never cache (security-sensitive) |
| **Cache-only** | App shell, offline fallback page | Only from cache |

## Web App Manifest

```json
{
  "name": "Your App Name",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Offline** | Core features work offline | Offline page shown | Broken offline |
| **Install** | Clean install prompt, app-like experience | Install works | No install support |
| **Caching** | Strategy per resource type | Basic caching | No caching |
| **Performance** | App shell loads instantly from cache | Reasonable speed | Slow even cached |
| **Updates** | Users notified of updates, cache refreshed | Auto-update | Stale forever |
| **Push** | Targeted notifications, preferences, opt-out | Basic push | No push or spammy |
| **Testing** | Tested offline, slow network, install flow | Some testing | Untested |

**28+ = Native app feel | 21-27 = Basic PWA | <21 = Not a PWA**
