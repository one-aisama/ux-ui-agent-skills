# Code Output Rules

Rules governing how the agent generates code, communicates in UI copy, and handles motion design. These apply to every framework output.

---

## Framework Output Formats

### React + Tailwind (primary web framework)

- **Language**: TypeScript (strict mode, no `any`)
- **Pattern**: `forwardRef` + `cva` (class-variance-authority) + `cn()` utility
- **Tokens**: Mapped to Tailwind v4 `@theme` CSS custom properties
- **File convention**: `components/ui/[name].tsx`
- **Full reference**: `frameworks/react-tailwind.md`

```tsx
// Example: Button component structure
import { forwardRef } from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md font-medium transition-colors focus-visible:outline-none focus-visible:ring-2",
  {
    variants: {
      variant: {
        primary: "bg-[--color-action-primary] text-[--color-text-on-primary]",
        secondary: "border border-[--color-border-default] bg-transparent",
        ghost: "bg-transparent hover:bg-[--color-surface-hover]",
        destructive: "bg-[--color-feedback-error] text-white",
      },
      size: {
        sm: "h-8 px-3 text-sm",
        md: "h-10 px-4 text-base",
        lg: "h-12 px-5 text-lg",
      },
    },
    defaultVariants: { variant: "primary", size: "md" },
  }
);
```

### Next.js 15 (full-stack web)

- **Router**: App Router with route groups and parallel routes
- **Components**: Server Components by default; `"use client"` pushed to leaf components
- **Assets**: `next/font` for font loading, `next/image` for images
- **Mutations**: Server Actions (not API routes for form submissions)
- **Loading states**: `loading.tsx` + `error.tsx` boundaries per route segment
- **Full reference**: `frameworks/nextjs.md`

### SwiftUI 6 (Apple platforms)

- **Tokens**: Asset Catalogs + `Color.DS` / `Font.DS` / `Spacing` extensions
- **Components**: `ButtonStyle`, `ViewModifier` for styling consistency
- **Accessibility**: `@ScaledMetric` for Dynamic Type, `@Environment(\.accessibilityReduceMotion)` for motion
- **Platform adaptation**: `#if os(iOS)` / `#if os(macOS)` for platform-specific behavior
- **Full reference**: `frameworks/swiftui.md`

---

## Code Generation Rules

These rules apply to **every code output**, regardless of framework:

### 1. Use Design Tokens — Never Hardcode

```tsx
// CORRECT: references token
className="bg-[--color-action-primary] text-[--color-text-on-primary]"

// WRONG: hardcoded value
className="bg-blue-600 text-white"
```

Every color, font size, spacing value, shadow, and border radius must trace to a design token.

### 2. Include Accessibility Attributes

Every interactive element gets proper ARIA or native semantics:

```tsx
// Button with loading state
<button
  aria-busy={isLoading}
  aria-disabled={isDisabled}
  disabled={isDisabled}
>
  {isLoading ? <Spinner aria-hidden="true" /> : null}
  {label}
</button>

// Input with error
<input
  aria-invalid={!!error}
  aria-describedby={error ? `${id}-error` : undefined}
/>
{error && <p id={`${id}-error`} role="alert">{error}</p>}
```

### 3. Handle All States

Every interactive component must implement: default, hover, focus, active, disabled, and loading (if async). No component ships with only a default state.

### 4. Support Dark Mode

Use semantic color tokens that switch automatically. Never reference primitive color values.

```css
/* Tokens auto-switch via [data-theme="dark"] */
--color-surface-page: var(--primitive-white);   /* light */
--color-surface-page: var(--primitive-gray-950); /* dark */
```

### 5. Mobile-First Responsive

Start with the mobile layout, add complexity at larger breakpoints:

```tsx
// Mobile-first: stack by default, row on medium+
className="flex flex-col gap-4 md:flex-row md:gap-6"
```

### 6. Copy-Paste Ready

Generated code should work with minimal adaptation:
- All imports included
- All types defined or imported
- No placeholder comments like `// TODO: implement`
- Props interface exported for composition
- Default export for the component

---

## Voice & Tone

### UI Copy Principles

Write for clarity. Every word should earn its place.

| Principle | Rule | Good | Bad |
|-----------|------|------|-----|
| **Frontload the verb** | Start with the action | "Save changes" | "Click here to save your changes" |
| **Be specific** | Say exactly what will happen | "Delete 3 items" | "Delete items" |
| **No jargon** | Use words users understand | "Something went wrong" | "Runtime exception occurred" |
| **Sentence case** | Only capitalize first word + proper nouns | "Account settings" | "Account Settings" |

### Error Messages

Follow the pattern: **What happened** + **Why** + **How to fix it**

```
GOOD:
"Password must be at least 8 characters. Try adding numbers or symbols."
"Can't connect to the server. Check your internet connection and try again."
"This email is already registered. Sign in instead?"

BAD:
"Error: Invalid input"
"Something went wrong"
"403 Forbidden"
```

### Empty States

Follow the pattern: **Explain the value** + **Guide to first action**

```
GOOD:
"No projects yet. Create your first project to get started."
"Your inbox is empty. Messages from your team will appear here."
"No results for 'widget'. Try a different search term."

BAD:
"No data"
"Nothing to show"
"0 results"
```

### Confirmation Dialogs

For destructive actions, be explicit about what will happen:

```
Title:  "Delete project 'Website Redesign'?"
Body:   "This will permanently delete the project and all 12 tasks inside it.
         This action cannot be undone."
Actions: [Cancel] [Delete project]
```

### Loading States

- Use skeleton screens for initial loads (not spinners)
- Use inline spinners for actions (button loading states)
- Show progress bars for operations > 3 seconds
- Always communicate what is happening: "Saving changes..." not just a spinner

---

## Motion Design Rules

### Duration

| Context | Duration | Example |
|---------|----------|---------|
| Micro-interactions | 100-150ms | Button hover, toggle switch |
| State transitions | 150-250ms | Panel expand, tab switch |
| Page transitions | 250-350ms | Route change, modal open |
| **Maximum** | **500ms** | Never exceed for UI transitions |

### Easing

| Movement type | Easing function | When to use |
|--------------|----------------|-------------|
| Entrance | `ease-out` (decelerate) | Element appearing: modal open, dropdown expand |
| Exit | `ease-in` (accelerate) | Element leaving: modal close, toast dismiss |
| State change | `ease-in-out` | Morphing between states: tab switch, accordion |
| Spring | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful UI: toggle bounce, notification pop |

### Motion Principles

1. **Purpose over decoration** — every animation must guide attention, show connection, or provide feedback. Remove animations that serve no function.
2. **Respect reduced motion** — always honor `prefers-reduced-motion: reduce`. Replace with opacity fade or remove entirely.
3. **Stagger, don't overwhelm** — when animating lists, stagger items by 30-50ms. Never animate more than 5 items simultaneously.
4. **Match physics** — elements should feel like they have weight. Larger elements move slower. Small elements are snappier.

### CSS Implementation Pattern

```css
/* Base transition */
.component {
  transition-property: background-color, color, border-color, box-shadow;
  transition-duration: 150ms;
  transition-timing-function: ease-out;
}

/* Reduced motion override */
@media (prefers-reduced-motion: reduce) {
  .component {
    transition-duration: 0ms;
  }
}
```

---

## Output Format by Request Type

When responding to user requests, match the output format to the request:

| Request Type | Output Format |
|-------------|--------------|
| **Token generation** | JSON in DTCG format (`$type`/`$value`), 3-tier architecture |
| **Component design** | Anatomy diagram + variants table + states table + token mapping + a11y notes + code example |
| **Code generation** | Copy-paste ready, typed, accessible, responsive, dark-mode aware |
| **Design review** | Scored rubric (6 dimensions) + prioritized findings table |
| **Accessibility audit** | WCAG criterion reference + severity + specific fix |
| **Prototyping** | Appropriate fidelity level with validation plan |
| **User flow** | Step-by-step with decision points, error paths, and edge cases |
