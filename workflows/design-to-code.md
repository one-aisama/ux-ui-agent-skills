# Design-to-Code Handoff Workflow

A systematic process for translating designs into production code. Covers measurement, token mapping, state documentation, edge cases, and the definition of done.

---

## Phase 1: Measurement → Token Mapping

Before writing any code, map every design value to a design token.

### Color Mapping
```
Design value         →  Token reference
──────────────────────────────────────────
#2563EB (button bg)  →  component.button.primary-bg
#111827 (text)       →  semantic.text.primary
#E5E7EB (border)     →  semantic.border.default
#F9FAFB (background) →  semantic.surface.sunken
```

**If a value doesn't match any token:**
1. Check if it's close enough (within 1-2 shades) → use nearest token
2. If it's intentionally different → discuss with design, potentially add new token
3. Never hardcode hex values in components

### Typography Mapping
```
Design spec           →  Token reference
──────────────────────────────────────────
Inter 24px Semibold   →  textStyle.heading-4
Inter 16px Regular    →  textStyle.body-base
Inter 14px Medium     →  textStyle.label
Inter 12px Regular    →  textStyle.caption
```

### Spacing Mapping
```
Design measurement  →  Token reference
──────────────────────────────────────────
24px padding        →  spacing.card.padding (semantic)
16px gap            →  spacing.stack.lg (semantic)
8px inline gap      →  spacing.inline.sm (semantic)
12px button gap     →  spacing.inline.md (semantic)
```

**Rule of thumb:** If a spacing value is on the 4px grid, it has a token. Map to the semantic level first; fall back to the scale level.

### Shadow & Border Mapping
```
0 1px 2px rgba(0,0,0,.05) →  elevation.sm
border: 1px solid #E5E7EB →  style.default
border-radius: 8px        →  radius.card
```

---

## Phase 2: State Documentation

Every interactive element must have all states documented before coding. Missing states cause the most rework.

### The 8 States Checklist

| # | State | Description | Visual Treatment |
|---|-------|-------------|-----------------|
| 1 | **Default** | Resting state | Base tokens |
| 2 | **Hover** | Mouse over | Background shift, cursor change |
| 3 | **Focus** | Keyboard focus | Focus ring (shadow.focus-ring) |
| 4 | **Active/Pressed** | Mouse down | Deeper background shift |
| 5 | **Disabled** | Non-interactive | 50% opacity, no pointer events |
| 6 | **Loading** | Async operation | Spinner, skeleton, progress |
| 7 | **Error** | Validation failed | Red border, error message |
| 8 | **Selected/Active** | Current selection | Accent background, checkmark |

**Not every element needs all 8** — but you must consciously decide which apply.

### State Matrix Example: Input Field

| State | Border | Background | Text | Icon | Additional |
|-------|--------|------------|------|------|-----------|
| Default | gray-200 | white | gray-900 | — | — |
| Hover | gray-300 | white | gray-900 | — | — |
| Focus | blue-500 | white | gray-900 | — | Focus ring |
| Active | blue-500 | white | gray-900 | — | Caret visible |
| Disabled | gray-200 | gray-100 | gray-400 | gray-300 | cursor: not-allowed |
| Loading | gray-200 | white | gray-400 | spinner | — |
| Error | red-500 | white | gray-900 | red error | Error text below |
| Filled | gray-200 | white | gray-900 | — | Value displayed |

---

## Phase 3: Edge Case Checklist

Before marking implementation complete, test these edge cases:

### Content Edge Cases
- [ ] **Long text** — Names/titles that are 50+ characters. Truncate with ellipsis? Wrap?
- [ ] **Empty content** — No items, no data. Show empty state illustration + CTA
- [ ] **Single item** — Does layout still make sense with just one item?
- [ ] **Many items** — 100+ items. Does it paginate? Virtual scroll?
- [ ] **Special characters** — Unicode, emoji, RTL text, HTML entities
- [ ] **Missing optional fields** — Avatar without image, card without description
- [ ] **Number extremes** — $0.00, $999,999.99, negative numbers, NaN

### Layout Edge Cases
- [ ] **Min-width content** — Viewport at 320px, no horizontal scroll
- [ ] **Max-width content** — 4K display, content doesn't stretch infinitely
- [ ] **Zoom 200%** — All content visible, no overlap
- [ ] **Dynamic font size** — iOS Dynamic Type / browser font scaling
- [ ] **Long words** — "Llanfairpwllgwyngyllgogerychwyrndrobwllllantysiliogogogoch" — use `word-break: break-word`
- [ ] **No-JS fallback** — If applicable, does basic functionality work without JavaScript?

### Interaction Edge Cases
- [ ] **Double click** — Prevent double submission
- [ ] **Rapid clicking** — Debounce where needed
- [ ] **Back button** — Browser back behavior in SPAs
- [ ] **Refresh** — Form state preserved? Data refetched?
- [ ] **Offline** — Error message? Cached content?
- [ ] **Concurrent edits** — Two users editing same data

---

## Phase 4: Responsive Specifications

Document responsive behavior at each breakpoint. Use a table for each major component.

### Responsive Spec Template

| Breakpoint | Layout | Visibility | Behavior |
|------------|--------|-----------|----------|
| Mobile (< 640px) | Single column, stacked | Hide secondary nav, show hamburger | Touch gestures, bottom sheet for actions |
| Tablet (640–1023px) | 2 columns, sidebar collapsed | Show icon-only sidebar | Hover not reliable, larger touch targets |
| Desktop (≥ 1024px) | 3 columns, full sidebar | Show all navigation | Hover interactions, keyboard shortcuts |

### Common Responsive Patterns

| Pattern | Description | When to Use |
|---------|-------------|------------|
| **Stack** | Horizontal → vertical | Form fields, card grids |
| **Collapse** | Sidebar → hamburger/bottom nav | Navigation |
| **Priority+** | Show top items, "More" menu for rest | Tab bars, toolbars |
| **Off-canvas** | Slide panel from edge | Detail panels, filters |
| **Truncate** | Full text → truncated + "Show more" | Long descriptions, tables |
| **Transform** | Table → card list | Data tables on mobile |

---

## Phase 5: Animation Specifications

Document every animation with precise specs for engineering implementation.

### Animation Spec Template

| Property | Trigger | From | To | Duration | Easing | Delay |
|----------|---------|------|----|----------|--------|-------|
| opacity | mount | 0 | 1 | 150ms | ease-out | 0ms |
| transform | mount | scale(0.95) | scale(1) | 150ms | ease-out | 0ms |
| max-height | expand | 0 | auto | 200ms | ease-in-out | 0ms |
| background | hover | transparent | gray-50 | 100ms | ease | 0ms |

### Animation Rules
1. **Duration**: Use 100-300ms for UI transitions. Never > 500ms.
2. **Easing**: `ease-out` for entrances, `ease-in` for exits, `ease-in-out` for state changes
3. **Reduced motion**: Always respect `prefers-reduced-motion`. Replace motion with opacity-only or instant transition.
4. **Purpose**: Every animation must serve a purpose — guide attention, show connection, provide feedback

### Common Animation Patterns

| Animation | Duration | Easing | Use Case |
|-----------|----------|--------|----------|
| Fade in | 150ms | ease-out | Modals, tooltips, content load |
| Fade out | 100ms | ease-in | Dismiss, remove |
| Slide in | 250ms | ease-out | Drawers, mobile menus |
| Scale in | 150ms | ease-out | Dropdowns, popovers |
| Collapse | 200ms | ease-in-out | Accordion, disclosure |
| Skeleton pulse | 1.5s | ease-in-out | Loading placeholders |
| Spinner | 600ms | linear | Button loading, async operations |

---

## Definition of Done

A component is "done" when ALL of the following are true:

### Functional
- [ ] All variants implemented as designed
- [ ] All 8 states handled (or explicitly N/A'd)
- [ ] Edge cases from checklist addressed
- [ ] Error handling implemented
- [ ] Loading states implemented

### Visual
- [ ] Pixel-accurate to design (within 1-2px tolerance)
- [ ] All values mapped to design tokens (zero hardcoded values)
- [ ] Dark mode supported (if applicable)
- [ ] Responsive at all breakpoints (320px → 1536px)
- [ ] Animations spec'd and implemented with reduced-motion fallback

### Accessible
- [ ] Keyboard navigable (Tab, Enter, Space, Escape, Arrows)
- [ ] Screen reader tested (VoiceOver or NVDA)
- [ ] Color contrast passes WCAG AA (4.5:1 text, 3:1 UI)
- [ ] Focus indicators visible
- [ ] ARIA attributes correct per `accessibility/aria-patterns.md`
- [ ] Touch targets ≥ 24×24px

### Code Quality
- [ ] TypeScript — no `any` types, proper interfaces
- [ ] Props documented with JSDoc or Storybook
- [ ] Compound component patterns where appropriate
- [ ] `forwardRef` for DOM access
- [ ] No inline styles — all Tailwind classes via `cva`/`cn`

### Tested
- [ ] Unit tests for logic (variants, states, calculations)
- [ ] Visual regression tests (screenshot comparison)
- [ ] A11y automated tests (axe-core)
- [ ] Manual screen reader test
- [ ] Cross-browser tested (Chrome, Firefox, Safari)

---

## Worked Example: Button Component (End to End)

A complete walkthrough from design spec to production-ready code.

### Step 1: Design Spec

The designer delivers a Button component with these variants:
- **Primary**: filled blue, white text
- **Secondary**: light gray fill, dark text
- **Ghost**: transparent, dark text
- **Destructive**: filled red, white text

Sizes: `sm` (32px height, 12px text), `md` (40px height, 14px text), `lg` (48px height, 16px text).

### Step 2: Token Mapping

```
Design Value                    Token Reference
───────────────────────────────────────────────────────────────────
Primary bg: #2563EB           → component.button.primary-bg
Primary bg hover: #1D4ED8     → component.button.primary-bg-hover
Primary bg active: #1E40AF    → component.button.primary-bg-active
Primary text: #FFFFFF         → component.button.primary-text
Secondary bg: #F3F4F6         → component.button.secondary-bg
Secondary bg hover: #E5E7EB   → component.button.secondary-bg-hover
Secondary text: #111827       → component.button.secondary-text
Ghost bg: transparent         → component.button.ghost-bg
Ghost bg hover: rgba(0,0,0,0.04) → component.button.ghost-bg-hover
Ghost text: #111827           → component.button.ghost-text
Destructive bg: #DC2626       → component.button.destructive-bg
Destructive bg hover: #B91C1C → component.button.destructive-bg-hover
Destructive text: #FFFFFF     → component.button.destructive-text
Disabled bg: #F3F4F6          → component.button.disabled-bg
Disabled text: #D1D5DB        → component.button.disabled-text
Focus ring: 0 0 0 2px white, 0 0 0 4px #3B82F6 → shadow.focus-ring
Border radius: 8px            → radius.interactive (semantic)
Padding horizontal sm: 12px   → spacing.inline.md
Padding horizontal md: 16px   → spacing.inline.lg
Padding horizontal lg: 20px   → spacing.inline.xl
Font weight: 500 (Medium)     → font.weight.medium
```

### Step 3: React + Tailwind Implementation

```tsx
// components/ui/button.tsx
import { forwardRef } from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";
import { Loader2 } from "lucide-react";

const buttonVariants = cva(
  // Base styles (all buttons)
  [
    "inline-flex items-center justify-center gap-2",
    "rounded-interactive font-medium",
    "transition-colors duration-100 ease-in-out",
    "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-border-focus focus-visible:ring-offset-2",
    "disabled:pointer-events-none disabled:opacity-50",
    "whitespace-nowrap select-none",
  ],
  {
    variants: {
      variant: {
        primary:
          "bg-action-primary text-text-on-action hover:bg-action-primary-hover active:bg-action-primary-active",
        secondary:
          "bg-action-secondary text-text-primary hover:bg-action-secondary-hover active:bg-action-secondary-active",
        ghost:
          "bg-transparent text-text-primary hover:bg-hover-overlay active:bg-active-overlay",
        destructive:
          "bg-action-destructive text-text-on-action hover:bg-action-destructive-hover active:bg-action-destructive-active",
      },
      size: {
        sm: "h-8 px-3 text-xs",      // 32px height, 12px padding, 12px text
        md: "h-10 px-4 text-sm",     // 40px height, 16px padding, 14px text
        lg: "h-12 px-5 text-base",   // 48px height, 20px padding, 16px text
      },
    },
    defaultVariants: {
      variant: "primary",
      size: "md",
    },
  }
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  /** Show a loading spinner and disable interaction */
  loading?: boolean;
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, loading, disabled, children, ...props }, ref) => {
    const isDisabled = disabled || loading;

    return (
      <button
        ref={ref}
        className={cn(buttonVariants({ variant, size }), className)}
        disabled={isDisabled}
        aria-busy={loading || undefined}
        aria-disabled={isDisabled || undefined}
        {...props}
      >
        {loading && (
          <Loader2
            className="h-4 w-4 animate-spin"
            aria-hidden="true"
          />
        )}
        {children}
      </button>
    );
  }
);

Button.displayName = "Button";

export { Button, buttonVariants };
```

### Step 4: Accessibility Check

| Check | Status | Detail |
|-------|--------|--------|
| Keyboard: Tab reaches button | PASS | Native `<button>` element, in tab order |
| Keyboard: Enter/Space activates | PASS | Native `<button>` behavior |
| Focus ring visible | PASS | `focus-visible:ring-2` with `border-focus` (blue.500), 3:1 contrast on white |
| Contrast: primary text on bg | PASS | White (#fff) on blue.600 (#2563eb) = 4.6:1 |
| Contrast: destructive text on bg | PASS | White (#fff) on red.600 (#dc2626) = 4.6:1 |
| Contrast: secondary text on bg | PASS | gray.900 (#111827) on gray.100 (#f3f4f6) = 14.8:1 |
| Disabled state communicated | PASS | `disabled` attribute + `aria-disabled` + visual opacity |
| Loading state communicated | PASS | `aria-busy="true"` + visible spinner + interaction blocked |
| Target size (sm) | CHECK | 32px height. Meets 24px WCAG minimum. Below 44px recommendation for mobile primary actions — acceptable for desktop toolbars. |
| Target size (md, lg) | PASS | 40px and 48px — adequate for primary actions |
| Screen reader: loading announced | PASS | `aria-busy` triggers screen reader announcement |

### Step 5: Definition of Done

```
Functional
  [x] All 4 variants implemented (primary, secondary, ghost, destructive)
  [x] All states: default, hover, focus, active, disabled, loading
  [x] Loading state blocks interaction and shows spinner
  [x] Disabled state is visual and semantic

Visual
  [x] All values map to design tokens (zero hardcoded hex/px values)
  [x] Dark mode: inherits from semantic token swap (no component-level overrides needed)
  [x] Responsive: button fills container on mobile via className override
  [x] Transition on hover: 100ms ease-in-out

Accessible
  [x] Keyboard: Tab, Enter, Space — native behavior
  [x] Focus ring visible: 2px ring with 2px offset
  [x] ARIA: aria-busy for loading, aria-disabled for disabled
  [x] Contrast: all variant/text combos pass 4.5:1 AA

Code Quality
  [x] TypeScript: proper interface, VariantProps, forwardRef
  [x] No `any` types
  [x] cva for variant management
  [x] forwardRef for DOM access
  [x] JSDoc on props
```

---

## Extended Edge Case Checklist

Beyond the standard edge case list in Phase 3, test these specific scenarios for every component.

### Long Text (100+ characters)

| Scenario | Expected Behavior | How to Test |
|----------|-------------------|-------------|
| Button label: "Submit Application and Proceed to Payment Verification Step" (58 chars) | Text stays on one line, button width grows. If constrained, text truncates with ellipsis. | Set `max-width: 200px` on parent. |
| Input value: 120-character email address | Input scrolls horizontally. Value is not clipped. Full value accessible via selection. | Type `aaaa...@very-long-domain.example.com` (120 chars). |
| Dropdown option: company name 100+ chars | Option text truncates with ellipsis. Full text visible on hover tooltip. | Add option "International Business Machines Corporation — Enterprise Solutions Division" (85 chars). |
| Table cell: long URL with no spaces | Cell expands vertically with `word-break: break-all`, or truncates with tooltip. Never breaks table layout. | Insert `https://example.com/very/long/path/without/any/spaces/that/keeps/going/forever`. |
| Card title: 100+ chars | Title truncates at 2 lines with ellipsis (`-webkit-line-clamp: 2`). Full title in `title` attribute. | Use a real-length project title. |

### RTL Layout

| Scenario | Expected Behavior | How to Test |
|----------|-------------------|-------------|
| Form field with label + input + error | Label right-aligned, input text right-aligned, error icon flips to right side. | Set `dir="rtl"` on `<html>`, switch to Arabic content. |
| Navigation sidebar | Sidebar appears on the right. Expand arrow points left. Chevrons flip. | Toggle `dir="rtl"`. |
| Button with leading icon | Icon appears to the right of the text (logical "start"). | Use `flex-direction` with logical properties, not `row`/`row-reverse`. |
| Breadcrumb separators | Arrows flip direction (point left instead of right). | Verify SVG icons use `transform: scaleX(-1)` in RTL or use logical separators. |
| Progress indicators | Progress bar fills from right to left. Step indicators reverse. | Check `direction` CSS property is inherited correctly. |

### Empty State

| Scenario | Expected Behavior | How to Test |
|----------|-------------------|-------------|
| Table with zero rows | Show illustration + heading + description + primary CTA. Header row hidden or displayed without data rows. | Clear all data from the API mock. |
| Search with zero results | Show "No results for [query]" with suggestions: check spelling, try different keywords, clear filters. | Search for a nonsense string. |
| Chart with no data points | Show placeholder with message "Not enough data yet. Data will appear after your first [event]." No empty axes. | Return empty dataset from API. |
| Avatar without image | Show initials on `avatar.bg` background. If no name, show generic person icon. | Remove `src` prop and optionally `name` prop. |
| List with one item | Layout should not look broken. No "showing 1 of 1" pagination. Single item should feel intentional, not empty. | Provide exactly one data item. |

### Loading State Overlay

| Scenario | Expected Behavior | How to Test |
|----------|-------------------|-------------|
| Button loading | Spinner replaces or sits beside text. Button stays same width (prevent layout shift). Pointer events disabled. | Click button, observe transition to loading. |
| Page section loading | Skeleton screen matching the expected content layout. Pulse animation at 1.5s ease-in-out. | Throttle network to slow 3G. |
| Modal with async content | Modal opens immediately with skeleton. Content fades in when ready. Close button works during loading. | Add 3s delay to modal content API. |
| Inline loading (save action) | Small spinner next to the save button. Success replaces spinner with checkmark for 2s. | Click save, observe spinner then checkmark. |
| Full page loading | Top progress bar (thin, accent color) or centered spinner. Not a blank white screen. | Refresh page on slow connection. |

### Disabled with Tooltip

| Scenario | Expected Behavior | How to Test |
|----------|-------------------|-------------|
| Disabled button with explanation | Tooltip appears on hover explaining WHY it's disabled ("Complete all required fields to continue"). Button uses `aria-describedby` pointing to tooltip content. | Hover over disabled button. |
| Disabled button keyboard access | Tooltip accessible via keyboard focus. Use `tabindex="0"` on the wrapper or `aria-disabled` instead of `disabled` attribute (native `disabled` removes from tab order). | Tab to the disabled button. |
| Disabled dropdown option | Option is visually muted. Tooltip on hover explains why ("Unavailable in your current plan"). | Hover over grayed-out option in select menu. |
| Touch device: disabled button | Long-press shows tooltip. Alternative: show inline text below the button instead of tooltip. | Test on mobile device or touch simulator. |
