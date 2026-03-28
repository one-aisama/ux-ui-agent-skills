# Component Standards

Every component in the design system follows the same quality bar, structure, and documentation format. This ensures consistency, composability, and accessibility across the entire product.

---

## Atomic Design Hierarchy

Build from small to large. Never skip levels.

| Level | Definition | Examples | File |
|-------|-----------|----------|------|
| **Atoms** | Indivisible UI primitives. Single-purpose, no internal dependencies. | Button, Input, Label, Icon, Badge, Avatar, Checkbox, Radio, Toggle, Tooltip | `components/atoms.md` |
| **Molecules** | Small groups of atoms working together for a single task. | Form Field (Label + Input + Error), Search Bar, Card, Navigation Item, Alert, Dropdown | `components/molecules.md` |
| **Organisms** | Complex, distinct sections composed of molecules and atoms. | Header, Sidebar, Form, Data Table, Modal, Drawer | `components/organisms.md` |
| **Templates** | Page-level layout structures that define content zones. | Dashboard, Auth, Settings, List/Detail | `components/templates.md` |
| **Pages** | Templates filled with real content and data. | Specific product screens | Project-specific |

### Principles

- **Atoms are context-free** — a Button works the same whether inside a Modal or a Sidebar
- **Molecules solve one task** — a Form Field handles label + input + validation as a unit
- **Organisms are self-contained sections** — they manage their own layout and internal state
- **Templates define slots, not content** — they specify where content goes, not what the content is

---

## Component Quality Bar

Every component — regardless of Atomic level — must ship with all six documentation elements:

### 1. Anatomy Diagram

Visual breakdown of the component's structural parts. Label every region.

```
┌─────────────────────────────────────────┐
│  [Icon]  Label Text          [Chevron]  │  <- Surface
│          Helper text                    │  <- Supporting text
└─────────────────────────────────────────┘
   ^         ^                     ^
   Leading   Content area          Trailing
```

### 2. Variants Table

All visual variants with their use case and visual distinction:

| Variant | Use case | Visual treatment |
|---------|----------|-----------------|
| Primary | Main CTA, single per view | Filled background, high contrast |
| Secondary | Supporting actions | Outlined border, transparent fill |
| Ghost | Tertiary / inline actions | No border, no fill, text only |
| Destructive | Irreversible actions | Red fill or red outline |

### 3. Size Specifications

| Size | Height | Padding (h) | Font size | Icon size | Use case |
|------|--------|-------------|-----------|-----------|----------|
| `sm` | 32px | 12px | 14px | 16px | Dense UI, tables, toolbars |
| `md` | 40px | 16px | 16px | 20px | Default for most contexts |
| `lg` | 48px | 20px | 18px | 24px | Hero sections, primary CTAs |

### 4. State Definitions

See the full state matrix below.

### 5. Token Mapping

Every visual property traces to a design token. Zero hardcoded values.

```
background:  {component.button.primary-bg}       -> {semantic.action.primary}      -> {primitive.blue.600}
text:        {component.button.primary-text}      -> {semantic.text.on-primary}     -> {primitive.white}
border:      {component.button.primary-border}    -> transparent
focus-ring:  {shadow.focus-ring}
```

### 6. Accessibility Specification

- ARIA role and attributes
- Keyboard interaction model
- Screen reader announcement format
- Focus management rules

---

## State Requirements

All interactive components must implement these states. Non-interactive components (Label, Badge) implement only the subset that applies.

| # | State | Required? | Token pattern | Behavior |
|---|-------|-----------|--------------|----------|
| 1 | **Default** | Always | Base tokens | Resting state, ready for interaction |
| 2 | **Hover** | Always | `-hover` suffix | Visual feedback on pointer hover. 100-150ms transition. |
| 3 | **Focus** | Always | `shadow.focus-ring` | Visible focus ring (3:1 contrast). Must work without hover. |
| 4 | **Active / Pressed** | Always | `-active` suffix | Momentary press feedback. Slightly darker/smaller than hover. |
| 5 | **Disabled** | Always | `opacity: 0.5` + `pointer-events: none` | Visually muted. Not focusable. Cursor: `not-allowed`. |
| 6 | **Loading** | If async | Spinner + `aria-busy="true"` | Replaces content or shows inline spinner. Prevents double-submit. |
| 7 | **Error** | If input | `border.error` + error message | Red border + error text below. `aria-invalid="true"` + `aria-describedby`. |
| 8 | **Selected** | If selectable | `interactive.selected-bg` | Persistent highlight. `aria-selected="true"` or `aria-checked`. |

### State transition order

```
Default -> Hover -> Active -> Default (click complete)
Default -> Focus (tab) -> Active (enter/space) -> Default
Default -> Disabled (prop change, no transition)
Default -> Loading (async action triggered)
Error persists until resolved, overlays on top of other states.
```

---

## Component Anatomy Format

When documenting a new component, use this template:

```markdown
## [Component Name]

### Purpose
One sentence: what task does this component serve?

### Anatomy
[ASCII diagram with labeled parts]

### Variants
[Table: variant | use case | visual treatment]

### Sizes
[Table: size | height | padding | font | icon]

### States
[Reference the 8-state matrix, note which states apply]

### Token Mapping
[Full token chain for each visual property]

### Keyboard Interaction
- Tab: moves focus to/from the component
- Enter/Space: activates the component
- Escape: dismisses (if applicable)
- Arrow keys: navigates within (if composite)

### ARIA
- Role: `button` / `textbox` / `dialog` / etc.
- Required attributes: [list]
- Screen reader announcement: "[name], [role], [state]"

### Edge Cases
- Long text: truncate with ellipsis + title attribute
- Empty state: show placeholder or guidance
- Overflow: scroll or wrap behavior
- RTL: mirrored layout where applicable

### Code Example
[Framework-specific implementation]
```

---

## Design-to-Code Handoff Checklist

Before marking any component ready for development:

1. All values mapped to design tokens (zero hardcoded values)
2. All applicable states documented (minimum 6 for interactive components)
3. Edge cases addressed: long text, empty, overflow, single item, many items
4. Responsive behavior specified at each breakpoint
5. Animation specified: property, duration, easing, `prefers-reduced-motion` fallback
6. Accessibility annotations complete: ARIA roles, keyboard model, focus management

### Definition of Done

A component is done when it passes all five quality gates:

| Gate | Criteria |
|------|----------|
| **Functional** | All variants, all states, all edge cases working |
| **Visual** | Pixel-accurate to spec, all tokens applied, responsive, dark mode |
| **Accessible** | Keyboard navigable, screen reader tested, contrast passing, target sizes met |
| **Code quality** | TypeScript (no `any`), `forwardRef`, `cva` patterns, documented props |
| **Tested** | Unit tests, visual regression, automated a11y scan, manual screen reader verification |
