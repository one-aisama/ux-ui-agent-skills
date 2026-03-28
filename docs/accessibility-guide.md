# Accessibility Guide

Accessibility is the #2 priority in the decision framework — it outranks consistency, aesthetics, and developer experience. Beautiful but inaccessible is broken.

The baseline is **WCAG 2.2 Level AA**. This is the minimum, not the goal.

---

## Mandatory Checks (P0 — Every Component)

These six checks must pass for every interactive component before it ships:

| # | Check | Pass criteria | How to verify |
|---|-------|--------------|---------------|
| 1 | **Keyboard navigable** | Tab reaches the component; Enter/Space activates it; Escape dismisses (if applicable) | Manual: unplug mouse, complete the task with keyboard only |
| 2 | **Focus visible** | Focus ring is visible and meets 3:1 contrast against adjacent colors | Inspect `shadow.focus-ring` token; verify with contrast checker |
| 3 | **Screen reader** | Announces name, role, and current state | Test with NVDA (Windows), VoiceOver (macOS/iOS), TalkBack (Android) |
| 4 | **Color contrast** | Text: 4.5:1 minimum. UI components: 3:1 minimum | Automated: axe-core, Lighthouse. Manual: browser DevTools contrast picker |
| 5 | **Target size** | Clickable/tappable area >= 24x24px (WCAG 2.5.8) | Inspect computed dimensions; check padding extends tap area |
| 6 | **No color-only** | Information is not conveyed by color alone — paired with icon, text, or pattern | Visual check: view in grayscale mode |

### When to run these checks

- **During design**: checks 4, 5, 6 can be verified in mockups
- **During development**: all 6 checks, using automated + manual testing
- **Before merge**: automated a11y scan (axe-core) must pass with zero violations

---

## WCAG 2.2 Key Criteria Summary

### Perceivable

| Criterion | Requirement | Design system implementation |
|-----------|------------|------------------------------|
| 1.1.1 Non-text Content | All images have alt text | `alt` prop required on Image components |
| 1.3.1 Info and Relationships | Structure conveyed programmatically | Use semantic HTML: `<nav>`, `<main>`, `<section>`, headings |
| 1.4.1 Use of Color | Color is not sole indicator | Icons + text labels accompany color signals |
| 1.4.3 Contrast (Minimum) | 4.5:1 text, 3:1 large text | Enforced via semantic color tokens |
| 1.4.11 Non-text Contrast | 3:1 for UI components | `border.strong` for essential borders, not `border.default` |
| 1.4.13 Content on Hover/Focus | Dismissible, hoverable, persistent | Tooltips stay open while hovered, Escape dismisses |

### Operable

| Criterion | Requirement | Design system implementation |
|-----------|------------|------------------------------|
| 2.1.1 Keyboard | All functionality via keyboard | Every interactive component has keyboard model |
| 2.4.3 Focus Order | Logical, meaningful focus sequence | DOM order matches visual order; no positive `tabindex` |
| 2.4.7 Focus Visible | Focus indicator always visible | `shadow.focus-ring` double-ring pattern |
| 2.4.11 Focus Not Obscured | Focused element not hidden by sticky elements | Sticky headers use `scroll-padding-top`; focused element scrolled into view |
| 2.5.8 Target Size | >= 24x24px minimum, 44x44px recommended for primary actions | Minimum enforced via component sizing; padding extends tap area |

### Understandable

| Criterion | Requirement | Design system implementation |
|-----------|------------|------------------------------|
| 3.1.1 Language of Page | `lang` attribute on `<html>` | Set in root layout |
| 3.3.1 Error Identification | Errors identified and described in text | Form Field component includes error message slot |
| 3.3.2 Labels | Inputs have visible labels | Label component is mandatory for all inputs |
| 3.3.8 Accessible Authentication | No cognitive function tests | Allow paste in password fields; support password managers |

### Robust

| Criterion | Requirement | Design system implementation |
|-----------|------------|------------------------------|
| 4.1.2 Name, Role, Value | All components have accessible name + role | ARIA patterns defined per component |
| 4.1.3 Status Messages | Dynamic content announced without focus | Use `aria-live` regions for toasts, alerts, loading states |

---

## Accessibility Decision Rules

Use these rules when accessibility conflicts with other priorities:

### Rule 1: Accessibility > Aesthetics

If a design looks better but fails contrast or target size requirements, **change the design**. Do not ship the "pretty" version with a plan to fix accessibility later.

**Example**: A light gray placeholder text (#C0C0C0) on white fails 4.5:1 contrast. Solution: darken to #767676 minimum, or use a visible label instead.

### Rule 2: Keyboard First, Mouse Second

Design the keyboard interaction model before the hover states. If a feature only works with a mouse, it is incomplete.

**Example**: A custom dropdown must support Arrow Up/Down to navigate, Enter to select, Escape to close, and Home/End to jump to first/last option.

### Rule 3: Semantic HTML Before ARIA

Use native HTML elements whenever possible. ARIA is a repair tool for when HTML falls short.

```
GOOD: <button>Save</button>
BAD:  <div role="button" tabindex="0" onclick="...">Save</div>
```

Only reach for ARIA when:
- No native HTML element exists for the pattern (e.g., `role="tablist"`)
- A complex widget needs additional state communication (e.g., `aria-expanded`)
- Dynamic content needs live region announcements (e.g., `aria-live="polite"`)

### Rule 4: Test With Real Assistive Technology

Automated tools (axe-core, Lighthouse) catch roughly 30-40% of accessibility issues. The rest require manual testing:

| Test | Tool | Catches |
|------|------|---------|
| Automated scan | axe-core, Lighthouse | Missing alt text, contrast failures, missing ARIA |
| Keyboard testing | Unplug mouse | Focus traps, unreachable elements, missing shortcuts |
| Screen reader | NVDA, VoiceOver | Announcement order, missing context, confusing labels |
| Zoom testing | Browser zoom to 200% | Layout breakage, text overflow, lost functionality |
| Motion testing | `prefers-reduced-motion` | Animations that cause vestibular issues |

### Rule 5: Reduced Motion is Not Optional

Every animation must have a `prefers-reduced-motion` fallback:

```css
/* Default: animated */
.transition {
  transition: transform 200ms ease-out;
}

/* Reduced motion: instant or fade only */
@media (prefers-reduced-motion: reduce) {
  .transition {
    transition: opacity 100ms ease-out;
  }
}
```

Acceptable reduced-motion alternatives:
- Replace slide/scale with simple opacity fade
- Replace continuous animation with static state
- Keep the functional behavior, remove the decorative movement

---

## Contrast Quick Reference

### Text contrast (4.5:1 minimum)

| Token pair | Ratio | Pass? |
|-----------|:-----:|:-----:|
| `text.primary` on `surface.page` | 15.4:1 | Yes |
| `text.secondary` on `surface.page` | 5.7:1 | Yes |
| `text.tertiary` on `surface.page` | 3.8:1 | No — only for large text or decorative |
| `text.on-primary` on `action.primary` | 8.1:1 | Yes |

### UI contrast (3:1 minimum)

| Token pair | Ratio | Pass? |
|-----------|:-----:|:-----:|
| `border.default` on `surface.page` | 1.4:1 | No — decorative only, not for essential borders |
| `border.strong` on `surface.page` | 3.2:1 | Yes |
| `action.primary` on `surface.page` | 4.8:1 | Yes |
| Focus ring inner + outer | 3:1+ | Yes — double-ring pattern ensures visibility on any background |

---

## ARIA Pattern References

Full implementation patterns for 15+ components are in `accessibility/aria-patterns.md`, including:

- Button, Toggle Button, Icon Button
- Checkbox, Radio Group, Switch
- Text Input, Combobox, Select
- Dialog (Modal), Alert Dialog
- Tabs, Accordion
- Menu, Menu Button
- Tooltip, Toast/Notification
- Data Table (sortable, selectable)
- Tree View, Breadcrumb, Pagination

Each pattern includes: required ARIA attributes, keyboard interaction model, focus management rules, and screen reader announcement expectations.
