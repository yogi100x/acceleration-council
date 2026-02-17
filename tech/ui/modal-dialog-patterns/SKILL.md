# Modal & Dialog Patterns

You are an expert in building accessible modals, drawers, sheets, and confirmation dialogs with proper focus management, scroll locking, and keyboard navigation.

## Research Protocol

**Web Search Queries:**
- "accessible modal dialog ARIA 2026"
- "react focus trap implementation"
- "scroll lock body modal open"
- "confirmation dialog UX best practices"

**WebFetch URLs:**
- https://www.radix-ui.com/primitives/docs/components/dialog
- https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/
- https://react-spectrum.adobe.com/react-aria/useDialog.html
- https://web.dev/building-a-dialog-component/

## Context Sync Protocol

Read upstream outputs:
- `.claude/outputs/user-flows.md` - Where dialogs are needed
- `.claude/outputs/form-requirements.md` - Dialog content
- `components/ui/` - Existing dialog components

## Decision Tree

### Modal vs Inline
```
Does this action require user's full attention?
├─ YES → Modal dialog (blocks background interaction)
│   └─ Example: Delete confirmation, payment form
├─ NO → Inline content or popover
│   └─ Example: Tooltips, dropdown menus
└─ Mobile consideration? → Sheet/drawer (slides from bottom/side)
```

### Dialog Type Selection
```
What's the content structure?
├─ Short message + 1-2 buttons → Alert Dialog
│   └─ Example: "Are you sure?" confirmation
├─ Form with multiple fields → Modal Dialog
│   └─ Example: Create new job, edit profile
├─ Large content (document, image) → Full-page modal
└─ Side panel for context → Drawer/Sheet
    └─ Example: Candidate profile sidebar
```

### Confirmation Pattern
```
Is the action destructive?
├─ YES → Two-step confirmation
│   └─ Modal with "Are you sure?" + type to confirm
│   └─ Example: Delete account (type "DELETE")
├─ NO → Single-step
│   └─ Simple confirm/cancel buttons
└─ Reversible? → Toast notification with undo
```

### Focus Management Strategy
```
On modal open:
1. Trap focus inside modal (Tab loops within dialog)
2. Focus first interactive element (button or input)
3. Lock body scroll (prevent background scroll)

On modal close:
1. Return focus to trigger element
2. Restore body scroll
3. Clean up event listeners
```

## Modal Patterns

### Accessible Modal Dialog
```typescript
'use client'
import * as Dialog from '@radix-ui/react-dialog'
import { X } from 'lucide-react'

export function CreateJobModal({ trigger }: { trigger: React.ReactNode }) {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>{trigger}</Dialog.Trigger>

      <Dialog.Portal>
        {/* Backdrop */}
        <Dialog.Overlay className="fixed inset-0 bg-black/50 data-[state=open]:animate-fadeIn" />

        {/* Content */}
        <Dialog.Content
          className="fixed left-1/2 top-1/2 w-full max-w-md -translate-x-1/2 -translate-y-1/2 rounded-lg bg-white p-6 shadow-lg data-[state=open]:animate-slideIn"
          aria-describedby="dialog-description"
        >
          <Dialog.Title className="text-xl font-bold">
            Create New Job
          </Dialog.Title>
          <Dialog.Description id="dialog-description" className="sr-only">
            Fill out the form below to create a new job posting
          </Dialog.Description>

          {/* Form content */}
          <form className="mt-4 space-y-4">
            <div>
              <label htmlFor="job-title" className="block text-sm font-medium">
                Job Title
              </label>
              <input
                id="job-title"
                type="text"
                className="mt-1 w-full rounded border px-3 py-2"
                autoFocus
              />
            </div>

            <div className="flex gap-2">
              <Dialog.Close asChild>
                <button type="button" className="rounded border px-4 py-2">
                  Cancel
                </button>
              </Dialog.Close>
              <button type="submit" className="rounded bg-primary px-4 py-2 text-white">
                Create
              </button>
            </div>
          </form>

          {/* Close button */}
          <Dialog.Close asChild>
            <button
              className="absolute right-4 top-4 rounded p-1 hover:bg-gray-100"
              aria-label="Close"
            >
              <X className="h-5 w-5" />
            </button>
          </Dialog.Close>
        </Dialog.Content>
      </Dialog.Portal>
    </Dialog.Root>
  )
}
```

### Confirmation Dialog
```typescript
'use client'
import * as AlertDialog from '@radix-ui/react-alert-dialog'

interface ConfirmDialogProps {
  trigger: React.ReactNode
  title: string
  description: string
  confirmLabel?: string
  onConfirm: () => void | Promise<void>
  destructive?: boolean
}

export function ConfirmDialog({
  trigger,
  title,
  description,
  confirmLabel = 'Confirm',
  onConfirm,
  destructive = false,
}: ConfirmDialogProps) {
  const [open, setOpen] = useState(false)
  const [loading, setLoading] = useState(false)

  async function handleConfirm() {
    setLoading(true)
    try {
      await onConfirm()
      setOpen(false)
    } catch (error) {
      console.error(error)
    } finally {
      setLoading(false)
    }
  }

  return (
    <AlertDialog.Root open={open} onOpenChange={setOpen}>
      <AlertDialog.Trigger asChild>{trigger}</AlertDialog.Trigger>

      <AlertDialog.Portal>
        <AlertDialog.Overlay className="fixed inset-0 bg-black/50" />
        <AlertDialog.Content className="fixed left-1/2 top-1/2 w-full max-w-sm -translate-x-1/2 -translate-y-1/2 rounded-lg bg-white p-6 shadow-lg">
          <AlertDialog.Title className="text-lg font-bold">
            {title}
          </AlertDialog.Title>
          <AlertDialog.Description className="mt-2 text-sm text-gray-600">
            {description}
          </AlertDialog.Description>

          <div className="mt-6 flex justify-end gap-2">
            <AlertDialog.Cancel asChild>
              <button className="rounded border px-4 py-2">Cancel</button>
            </AlertDialog.Cancel>
            <AlertDialog.Action asChild>
              <button
                onClick={handleConfirm}
                disabled={loading}
                className={`rounded px-4 py-2 text-white ${
                  destructive ? 'bg-red-600' : 'bg-primary'
                }`}
              >
                {loading ? 'Loading...' : confirmLabel}
              </button>
            </AlertDialog.Action>
          </div>
        </AlertDialog.Content>
      </AlertDialog.Portal>
    </AlertDialog.Root>
  )
}

// Usage
<ConfirmDialog
  trigger={<button>Delete Account</button>}
  title="Delete Account"
  description="This action cannot be undone. All your data will be permanently deleted."
  confirmLabel="Delete"
  onConfirm={async () => {
    await deleteAccount()
  }}
  destructive
/>
```

### Drawer/Sheet (Side Panel)
```typescript
'use client'
import * as Dialog from '@radix-ui/react-dialog'
import { X } from 'lucide-react'

export function Drawer({
  trigger,
  side = 'right',
  children,
}: {
  trigger: React.ReactNode
  side?: 'left' | 'right'
  children: React.ReactNode
}) {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>{trigger}</Dialog.Trigger>

      <Dialog.Portal>
        <Dialog.Overlay className="fixed inset-0 bg-black/50" />
        <Dialog.Content
          className={`fixed inset-y-0 w-full max-w-md bg-white shadow-lg data-[state=open]:animate-slideInFromRight ${
            side === 'right' ? 'right-0' : 'left-0'
          }`}
        >
          <div className="flex h-full flex-col">
            <div className="flex items-center justify-between border-b p-4">
              <Dialog.Title className="text-lg font-bold">
                Candidate Profile
              </Dialog.Title>
              <Dialog.Close asChild>
                <button aria-label="Close">
                  <X className="h-5 w-5" />
                </button>
              </Dialog.Close>
            </div>

            <div className="flex-1 overflow-y-auto p-4">{children}</div>
          </div>
        </Dialog.Content>
      </Dialog.Portal>
    </Dialog.Root>
  )
}
```

### Bottom Sheet (Mobile)
```typescript
'use client'
import * as Dialog from '@radix-ui/react-dialog'

export function BottomSheet({ trigger, children }: { trigger: React.ReactNode; children: React.ReactNode }) {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>{trigger}</Dialog.Trigger>

      <Dialog.Portal>
        <Dialog.Overlay className="fixed inset-0 bg-black/50" />
        <Dialog.Content className="fixed inset-x-0 bottom-0 max-h-[80vh] rounded-t-xl bg-white data-[state=open]:animate-slideInFromBottom">
          {/* Drag handle */}
          <div className="flex justify-center p-4">
            <div className="h-1 w-12 rounded-full bg-gray-300" />
          </div>

          <div className="overflow-y-auto px-4 pb-4">{children}</div>
        </Dialog.Content>
      </Dialog.Portal>
    </Dialog.Root>
  )
}
```

### Full-Screen Modal
```typescript
export function FullScreenModal({ trigger, children }: { trigger: React.ReactNode; children: React.ReactNode }) {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>{trigger}</Dialog.Trigger>

      <Dialog.Portal>
        <Dialog.Content className="fixed inset-0 bg-white z-50 overflow-y-auto">
          <div className="sticky top-0 z-10 flex justify-between border-b bg-white p-4">
            <Dialog.Title className="text-xl font-bold">
              Document Viewer
            </Dialog.Title>
            <Dialog.Close asChild>
              <button>
                <X className="h-6 w-6" />
              </button>
            </Dialog.Close>
          </div>

          <div className="p-8">{children}</div>
        </Dialog.Content>
      </Dialog.Portal>
    </Dialog.Root>
  )
}
```

### Custom Focus Trap (without Radix)
```typescript
'use client'
import { useEffect, useRef } from 'react'

export function useFocusTrap(isOpen: boolean) {
  const containerRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (!isOpen) return

    const container = containerRef.current
    if (!container) return

    const focusableElements = container.querySelectorAll<HTMLElement>(
      'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    )
    const firstElement = focusableElements[0]
    const lastElement = focusableElements[focusableElements.length - 1]

    function handleTab(e: KeyboardEvent) {
      if (e.key !== 'Tab') return

      if (e.shiftKey) {
        if (document.activeElement === firstElement) {
          e.preventDefault()
          lastElement?.focus()
        }
      } else {
        if (document.activeElement === lastElement) {
          e.preventDefault()
          firstElement?.focus()
        }
      }
    }

    container.addEventListener('keydown', handleTab)
    firstElement?.focus()

    return () => container.removeEventListener('keydown', handleTab)
  }, [isOpen])

  return containerRef
}
```

## Anti-Patterns

**MD-50: No Focus Trap**
- Tab key escapes modal, focus moves to background content
- Fix: Use Radix Dialog or implement custom focus trap

**MD-60: Background Scrolls When Modal Open**
- Body scroll not locked, confusing UX
- Fix: Add `overflow: hidden` to body when modal open

**MD-70: No Return Focus After Close**
- Focus lost after closing modal
- Fix: Store trigger element, focus it on close

**MD-80: Destructive Action Without Confirmation**
- Delete button with no second check
- Fix: Use confirmation dialog for irreversible actions

## Quality Rubric (Ship at 28/35)

| Dimension | 1 | 3 | 5 |
|-----------|---|---|---|
| Accessibility | No ARIA | Basic dialog role | Full focus trap + ARIA |
| Keyboard Support | Mouse-only | Esc closes | Tab trap + Enter/Space |
| Visual Feedback | Static | Backdrop overlay | Animations + backdrop blur |
| Focus Management | Lost focus | Returns to trigger | Full focus restoration |
| Mobile UX | Desktop modal | Responsive width | Bottom sheet pattern |
| Code Quality | Inline logic | Custom hook | Radix/Headless UI |
| Error Handling | No feedback | Toast on error | Inline validation + retry |

## Output Protocol

Write to `.claude/outputs/`:
- `dialog-components.tsx` - Modal, drawer, confirmation
- `focus-management.ts` - Focus trap utilities
- `modal-accessibility.md` - ARIA guidelines
- `confirmation-patterns.md` - When to use each type
