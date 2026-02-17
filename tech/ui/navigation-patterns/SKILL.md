# Navigation Patterns

You are an expert in designing intuitive, accessible navigation systems including sidebars, tabs, breadcrumbs, command palettes, and mobile menus.

## Research Protocol

**Web Search Queries:**
- "navigation patterns 2026 UX best practices"
- "command palette cmdk implementation"
- "mobile hamburger menu accessibility"
- "breadcrumb navigation ARIA"

**WebFetch URLs:**
- https://www.radix-ui.com/primitives/docs/components/navigation-menu
- https://cmdk.paco.me/
- https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/
- https://web.dev/patterns/web-vitals-patterns/navigation

## Context Sync Protocol

Read upstream outputs:
- `.claude/outputs/site-structure.md` - Page hierarchy
- `.claude/outputs/user-flows.md` - Navigation paths
- `app/` - Next.js route structure

## Decision Tree

### Navigation Type Selection
```
How deep is the hierarchy?
├─ 1-2 levels → Top navigation bar
├─ 3-4 levels → Sidebar navigation
└─ 5+ levels → Sidebar + breadcrumbs
    └─ Or: Multi-level accordion menu
```

### Mobile Menu Pattern
```
Content priority?
├─ Navigation is primary → Bottom tab bar (app-like)
├─ Content is primary → Hamburger menu (top-left)
└─ Mixed → Drawer that slides from left
    └─ With backdrop overlay for focus management
```

### Command Palette Decision
```
Users need quick access to actions?
├─ YES → Command palette (Cmd+K / Ctrl+K)
│   └─ Search + keyboard shortcuts
│   └─ Example: "Create new job", "Go to settings"
└─ NO → Standard navigation
```

### Tabs vs Accordion
```
Switching frequency?
├─ High (within same session) → Tabs
│   └─ Keep all content in DOM, toggle visibility
├─ Low (separate tasks) → Accordion or separate pages
└─ Many options (>7) → Vertical tabs or list
```

## Navigation Patterns

### Sidebar with Active State
```typescript
'use client'
import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { Home, Users, Settings, BarChart } from 'lucide-react'
import { cn } from '@/lib/utils'

const navItems = [
  { href: '/dashboard', label: 'Dashboard', icon: Home },
  { href: '/candidates', label: 'Candidates', icon: Users },
  { href: '/analytics', label: 'Analytics', icon: BarChart },
  { href: '/settings', label: 'Settings', icon: Settings },
]

export function Sidebar() {
  const pathname = usePathname()

  return (
    <nav className="flex h-full w-64 flex-col border-r bg-background p-4">
      <div className="mb-8 text-xl font-bold">SwiftHyre</div>
      <ul className="space-y-2">
        {navItems.map((item) => {
          const isActive = pathname === item.href
          const Icon = item.icon
          return (
            <li key={item.href}>
              <Link
                href={item.href}
                className={cn(
                  'flex items-center gap-3 rounded-lg px-3 py-2 transition-colors',
                  isActive
                    ? 'bg-primary text-primary-foreground'
                    : 'hover:bg-gray-100 dark:hover:bg-gray-800'
                )}
                aria-current={isActive ? 'page' : undefined}
              >
                <Icon className="h-5 w-5" />
                <span>{item.label}</span>
              </Link>
            </li>
          )
        })}
      </ul>
    </nav>
  )
}
```

### Mobile Drawer Menu
```typescript
'use client'
import { useState } from 'react'
import { Menu, X } from 'lucide-react'
import { Dialog } from '@repo/ui'

export function MobileNav() {
  const [open, setOpen] = useState(false)

  return (
    <>
      <button
        onClick={() => setOpen(true)}
        className="lg:hidden p-2"
        aria-label="Open menu"
      >
        <Menu className="h-6 w-6" />
      </button>

      <Dialog open={open} onOpenChange={setOpen}>
        <div className="fixed inset-0 z-50 flex">
          {/* Backdrop */}
          <div
            className="fixed inset-0 bg-black/50"
            onClick={() => setOpen(false)}
          />

          {/* Drawer */}
          <div
            className="relative z-50 w-3/4 max-w-sm bg-background p-6 shadow-lg"
            role="dialog"
            aria-modal="true"
            aria-label="Navigation menu"
          >
            <button
              onClick={() => setOpen(false)}
              className="absolute top-4 right-4"
              aria-label="Close menu"
            >
              <X className="h-6 w-6" />
            </button>

            <nav className="mt-8">
              <ul className="space-y-4">
                {navItems.map((item) => (
                  <li key={item.href}>
                    <Link
                      href={item.href}
                      onClick={() => setOpen(false)}
                      className="block px-3 py-2 text-lg"
                    >
                      {item.label}
                    </Link>
                  </li>
                ))}
              </ul>
            </nav>
          </div>
        </div>
      </Dialog>
    </>
  )
}
```

### Breadcrumb Navigation
```typescript
import Link from 'next/link'
import { ChevronRight } from 'lucide-react'

interface BreadcrumbItem {
  label: string
  href?: string
}

export function Breadcrumb({ items }: { items: BreadcrumbItem[] }) {
  return (
    <nav aria-label="Breadcrumb">
      <ol className="flex items-center gap-2 text-sm text-muted-foreground">
        {items.map((item, index) => {
          const isLast = index === items.length - 1
          return (
            <li key={index} className="flex items-center gap-2">
              {item.href && !isLast ? (
                <Link href={item.href} className="hover:text-foreground">
                  {item.label}
                </Link>
              ) : (
                <span
                  className={isLast ? 'font-medium text-foreground' : ''}
                  aria-current={isLast ? 'page' : undefined}
                >
                  {item.label}
                </span>
              )}
              {!isLast && <ChevronRight className="h-4 w-4" />}
            </li>
          )
        })}
      </ol>
    </nav>
  )
}

// Usage
<Breadcrumb
  items={[
    { label: 'Home', href: '/' },
    { label: 'Candidates', href: '/candidates' },
    { label: 'John Doe' },
  ]}
/>
```

### Command Palette (cmdk)
```typescript
'use client'
import { useEffect, useState } from 'react'
import { Command } from 'cmdk'
import { Search, Plus, Settings, LogOut } from 'lucide-react'
import { useRouter } from 'next/navigation'

export function CommandPalette() {
  const [open, setOpen] = useState(false)
  const router = useRouter()

  useEffect(() => {
    const down = (e: KeyboardEvent) => {
      if (e.key === 'k' && (e.metaKey || e.ctrlKey)) {
        e.preventDefault()
        setOpen((open) => !open)
      }
    }
    document.addEventListener('keydown', down)
    return () => document.removeEventListener('keydown', down)
  }, [])

  return (
    <Command.Dialog open={open} onOpenChange={setOpen} label="Command Menu">
      <Command.Input placeholder="Type a command or search..." />
      <Command.List>
        <Command.Empty>No results found.</Command.Empty>

        <Command.Group heading="Actions">
          <Command.Item
            onSelect={() => {
              router.push('/jobs/new')
              setOpen(false)
            }}
          >
            <Plus className="mr-2 h-4 w-4" />
            Create new job
          </Command.Item>
        </Command.Group>

        <Command.Group heading="Navigation">
          <Command.Item onSelect={() => router.push('/dashboard')}>
            Dashboard
          </Command.Item>
          <Command.Item onSelect={() => router.push('/candidates')}>
            Candidates
          </Command.Item>
        </Command.Group>

        <Command.Group heading="Settings">
          <Command.Item onSelect={() => router.push('/settings')}>
            <Settings className="mr-2 h-4 w-4" />
            Settings
          </Command.Item>
          <Command.Item onSelect={() => router.push('/logout')}>
            <LogOut className="mr-2 h-4 w-4" />
            Sign out
          </Command.Item>
        </Command.Group>
      </Command.List>
    </Command.Dialog>
  )
}

// Trigger button
<button
  onClick={() => setOpen(true)}
  className="flex items-center gap-2 rounded-lg border px-3 py-2 text-sm text-muted-foreground"
>
  <Search className="h-4 w-4" />
  <span>Search...</span>
  <kbd className="ml-auto text-xs">⌘K</kbd>
</button>
```

### Tabs with Keyboard Navigation
```typescript
'use client'
import { useState } from 'react'
import * as Tabs from '@radix-ui/react-tabs'

export function ProfileTabs() {
  return (
    <Tabs.Root defaultValue="overview">
      <Tabs.List
        className="flex gap-4 border-b"
        aria-label="Profile sections"
      >
        <Tabs.Trigger
          value="overview"
          className="border-b-2 border-transparent px-4 py-2 data-[state=active]:border-primary"
        >
          Overview
        </Tabs.Trigger>
        <Tabs.Trigger
          value="activity"
          className="border-b-2 border-transparent px-4 py-2 data-[state=active]:border-primary"
        >
          Activity
        </Tabs.Trigger>
        <Tabs.Trigger
          value="settings"
          className="border-b-2 border-transparent px-4 py-2 data-[state=active]:border-primary"
        >
          Settings
        </Tabs.Trigger>
      </Tabs.List>

      <Tabs.Content value="overview" className="mt-4">
        Overview content
      </Tabs.Content>
      <Tabs.Content value="activity" className="mt-4">
        Activity content
      </Tabs.Content>
      <Tabs.Content value="settings" className="mt-4">
        Settings content
      </Tabs.Content>
    </Tabs.Root>
  )
}
```

### Mega Menu (Multi-Column Dropdown)
```typescript
'use client'
import * as NavigationMenu from '@radix-ui/react-navigation-menu'

export function MegaMenu() {
  return (
    <NavigationMenu.Root>
      <NavigationMenu.List className="flex gap-4">
        <NavigationMenu.Item>
          <NavigationMenu.Trigger className="px-4 py-2">
            Products
          </NavigationMenu.Trigger>
          <NavigationMenu.Content className="absolute left-0 top-full w-screen bg-white shadow-lg">
            <div className="grid grid-cols-3 gap-8 p-8 max-w-6xl mx-auto">
              <div>
                <h3 className="font-bold mb-2">For Recruiters</h3>
                <ul className="space-y-2">
                  <li><Link href="/talent-pool">Talent Pool</Link></li>
                  <li><Link href="/pipeline">Pipeline</Link></li>
                </ul>
              </div>
              <div>
                <h3 className="font-bold mb-2">For Candidates</h3>
                <ul className="space-y-2">
                  <li><Link href="/assessments">Assessments</Link></li>
                  <li><Link href="/jobs">Browse Jobs</Link></li>
                </ul>
              </div>
              <div>
                <h3 className="font-bold mb-2">Resources</h3>
                <ul className="space-y-2">
                  <li><Link href="/docs">Documentation</Link></li>
                  <li><Link href="/blog">Blog</Link></li>
                </ul>
              </div>
            </div>
          </NavigationMenu.Content>
        </NavigationMenu.Item>
      </NavigationMenu.List>
    </NavigationMenu.Root>
  )
}
```

## Anti-Patterns

**NP-50: Hamburger Icon Without Label**
- Icon-only button with no accessible name
- Fix: Add aria-label="Open menu"

**NP-60: Breadcrumbs Without ARIA**
- Missing aria-label="Breadcrumb" on <nav>
- Fix: Use semantic nav + aria-current on last item

**NP-70: Tabs Not Keyboard Accessible**
- Arrow keys don't move between tabs
- Fix: Use Radix UI Tabs or implement keyboard handlers

**NP-80: Command Palette Conflicts with Browser Shortcuts**
- Cmd+K overrides browser's search
- Fix: Document the shortcut, provide alternative trigger

## Quality Rubric (Ship at 28/35)

| Dimension | 1 | 3 | 5 |
|-----------|---|---|---|
| Accessibility | No ARIA | Basic landmarks | Full keyboard + screen reader |
| Mobile UX | Desktop-only | Responsive hamburger | Native app-like (bottom tabs) |
| Visual Feedback | No active state | Hover + active | Focus visible + animations |
| Keyboard Support | Mouse-only | Tab navigation | Full shortcuts + command palette |
| Performance | Layout shift | Stable | Prefetch on hover |
| Code Quality | Inline handlers | Custom hooks | Radix/Headless UI |
| User Clarity | Generic labels | Descriptive | Icons + labels + breadcrumbs |

## Output Protocol

Write to `.claude/outputs/`:
- `navigation-components.tsx` - Sidebar, breadcrumb, command palette
- `mobile-nav-pattern.md` - Mobile menu strategy
- `keyboard-shortcuts.md` - Documented shortcuts
- `navigation-accessibility.md` - ARIA guidelines
