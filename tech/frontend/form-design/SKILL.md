# Form Design

Design forms that are validated, accessible, performant, and user-friendly.

## Context Sync Protocol

Read existing form patterns and validation library usage.

## Decision Tree: Form Approach

```
How complex is the form?
├── Simple (1-5 fields, no dependencies)
│   └── useState + inline validation
├── Medium (5-15 fields, validation rules)
│   └── react-hook-form + zod schema
│       → Uncontrolled by default, performant, type-safe validation
├── Complex (multi-step, conditional fields, file uploads)
│   └── react-hook-form + zod + useFieldArray + step state
├── Dynamic (user-configurable fields)
│   └── Form builder pattern with JSON schema
└── Wizard (multi-page with save progress)
    └── Multi-step with per-step validation + persistence
```

## Form Best Practices

| Practice | Implementation |
|----------|---------------|
| **Validate on blur, not keystroke** | Show errors when user leaves field |
| **Inline errors below fields** | Not in alert/toast — users can't find them |
| **Disable submit when submitting** | Prevent double submission |
| **Show loading state on submit** | User knows it's working |
| **Preserve form data on error** | Don't clear the form on server error |
| **Auto-save for long forms** | Save draft to localStorage or server |
| **Mark required fields** | Asterisk or "(required)" label |
| **Group related fields** | Fieldsets with legends |

## Validation Strategy

```
Client-side (UX):
  - Zod schema for type-safe validation
  - Validate on blur (not onChange for performance)
  - Show inline errors below each field
  - Disable submit until form is valid (optional)

Server-side (Security):
  - ALWAYS validate on server (client validation is bypassable)
  - Same Zod schema shared between client and server
  - Return field-level errors for display
  - Sanitize inputs (XSS prevention)
```

## Accessibility Checklist

- [ ] All inputs have associated `<label>` elements
- [ ] Error messages linked with `aria-describedby`
- [ ] Required fields marked with `aria-required="true"`
- [ ] Invalid fields marked with `aria-invalid="true"`
- [ ] Form can be submitted with keyboard (Enter key)
- [ ] Focus moves to first error on submit
- [ ] Error messages are announced by screen readers

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Validation** | Client + server with shared schema (Zod) | Client-side only | No validation |
| **UX** | Inline errors, loading states, preserved data | Basic error display | Alert boxes |
| **Accessibility** | Full ARIA, keyboard nav, screen reader tested | Labels present | No a11y |
| **Performance** | Uncontrolled inputs, minimal re-renders | Some optimization | Controlled everything |
| **Multi-step** | Step validation, progress indicator, back navigation | Basic steps | Single giant form |
| **Error recovery** | Auto-save, preserved state on error | Some preservation | Form clears on error |
| **File uploads** | Preview, progress, drag-drop, size/type validation | Basic upload | No upload support |

**28+ = User-friendly forms | 21-27 = Functional but rough | <21 = User frustration**
