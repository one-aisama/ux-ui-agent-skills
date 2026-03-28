# Token System

Design tokens are the single source of truth for every visual value in the system. No component should contain hardcoded colors, sizes, spacing, or shadows — everything traces back to a token.

---

## Architecture: 3-Tier Token Hierarchy

```
┌──────────────────────────────────────────────────────────┐
│  COMPONENT TOKENS  (use in code)                         │
│  button-bg-primary  →  {semantic.action.primary}         │
├──────────────────────────────────────────────────────────┤
│  SEMANTIC TOKENS  (use in design)                        │
│  action.primary  →  {primitive.blue.600}                 │
├──────────────────────────────────────────────────────────┤
│  PRIMITIVE TOKENS  (never reference directly)            │
│  blue.600  →  #2563EB                                    │
└──────────────────────────────────────────────────────────┘
```

| Tier | Purpose | Who uses it | Example |
|------|---------|-------------|---------|
| **Primitive** | Raw palette values. The building blocks. | Token authors only | `blue.600 = #2563EB` |
| **Semantic** | Purpose-based aliases that describe *intent*. | Designers, general styling | `action.primary`, `text.secondary`, `surface.elevated` |
| **Component** | Scoped to a single component's anatomy. | Developers, component code | `button.primary-bg`, `input.border-focus` |

**Rule**: Code references component tokens. Designs reference semantic tokens. Nobody references primitives directly — they exist only as the foundation layer.

---

## Naming Convention

```
{category}.{property}.{variant}-{state}
```

### Examples

| Token name | Resolves to |
|-----------|-------------|
| `semantic.text.primary` | Main body text color |
| `semantic.text.secondary` | Muted/supporting text color |
| `semantic.feedback.error-text` | Error message text color |
| `component.button.primary-bg` | Primary button background (default) |
| `component.button.primary-bg-hover` | Primary button background on hover |
| `component.input.border-focus` | Input border color when focused |

### Naming rules

1. Use dot notation for hierarchy, hyphens for compound words
2. States are always the last segment: `-hover`, `-active`, `-disabled`, `-focus`
3. Avoid abbreviations except universally understood ones (`bg`, `fg`, `sm`, `md`, `lg`)
4. Semantic tokens never include specific color names (`error`, not `red`)

---

## Token Format: DTCG

All token files use **Design Tokens Community Group** (DTCG) format with `$type` and `$value` properties:

```json
{
  "color": {
    "blue": {
      "600": {
        "$type": "color",
        "$value": "#2563EB",
        "$description": "Primary blue, meets 4.5:1 on white"
      }
    }
  }
}
```

### Token source files

| File | Contents |
|------|----------|
| `tokens/colors.json` | 3-tier color system: 6 hues x 11 shades + semantic + component + dark mode overrides |
| `tokens/typography.json` | Major Third (1.25) modular scale + composite text styles |
| `tokens/spacing.json` | 4px base unit scale + semantic spacing aliases |
| `tokens/shadows.json` | 5-level elevation + inner shadows + colored shadows + focus ring |
| `tokens/borders.json` | Radius scale + semantic radii + width scale |
| `tokens/breakpoints.json` | Mobile-first breakpoints + container widths + grid config + z-index layers |

---

## Color Guidelines

### Contrast Requirements (WCAG 2.2)

| Element | Minimum Ratio | Verification example |
|---------|:------------:|----------------------|
| Normal text (< 24px) | **4.5:1** | `text.primary` on `surface.page` = 15.4:1 |
| Large text (>= 24px or >= 18.66px bold) | **3:1** | `text.secondary` on `surface.page` = 5.7:1 |
| UI components & graphical objects | **3:1** | `border.default` on `surface.page` = 1.4:1 FAIL — use `border.strong` for essential borders |
| Focus indicators | **3:1** | Focus ring uses `shadow.focus-ring` (double-ring pattern) |

### Color Usage Rules

1. **Never use color as the only indicator** — always pair with icon, text label, or pattern
2. **Feedback palette**: success = green, warning = amber, error = red, info = blue
3. **Interactive elements**: all clickable items use `action.primary` or `text.link`
4. **Limit palette per screen**: 1 primary + 1 destructive + neutrals. Accent colors used sparingly.
5. **Colored shadows**: only on hover states for emphasis (see `tokens/shadows.json` -> `colored`)

### Generating New Palettes

Use **OKLCH color space** for perceptually uniform shade scales:

1. Define the brand hue (e.g., `hue = 264` for purple)
2. Generate 11 shades: L = 97% (shade 50) down to L = 15% (shade 950), consistent chroma
3. Verify shade 500 meets **4.5:1** contrast on white (text-safe)
4. Verify shade 600 meets **3:1** contrast on white (UI-safe)
5. Test the full scale in both light and dark modes before shipping

---

## Typography Guidelines

### Scale: Major Third (ratio 1.25)

```
xs=12  sm=14  base=16  lg=18  xl=20  2xl=24  3xl=30  4xl=36  5xl=48  6xl=60  7xl=72
```

### Font stacks

| Purpose | Font | Fallback |
|---------|------|----------|
| UI text | Inter | system-ui, -apple-system, sans-serif |
| Editorial / marketing | Lora | Georgia, serif |
| Code / data values | JetBrains Mono | ui-monospace, monospace |

### Typography rules

1. **One font family for UI** — do not mix sans-serif faces within the interface
2. **Heading hierarchy** — `h1` appears once per page; headings never skip levels (no h2 -> h4)
3. **Line length** — Body text: 45-75 characters per line. Optimal: `max-width: 65ch`
4. **Line height** — Headings: 1.25 (tight). Body: 1.5 (normal). Captions: 1.5 (normal)
5. **Font weight mapping**:
   - 400 (Regular) — body text
   - 500 (Medium) — labels, UI controls
   - 600 (Semibold) — section headings
   - 700 (Bold) — page titles only
6. **Responsive type** — Scale down by one step on mobile (e.g., `3xl` heading becomes `2xl`)

See composite text styles in `tokens/typography.json` -> `textStyle`.

---

## Spacing Guidelines

### Base Unit: 4px

All spacing values are multiples of 4px:

```
0  2  4  6  8  10  12  14  16  20  24  28  32  36  40  44  48  56  64  80  96
```

### Spacing rules

1. **Outer > Inner** — Container padding > element gaps > element internal padding
2. **Related items closer** — Group related elements with tighter spacing; separate unrelated groups with wider spacing
3. **Vertical rhythm** — Establish a consistent vertical rhythm and maintain it across the full page
4. **Use semantic tokens** — Reference purpose-named tokens (`card.padding`, `stack.lg`, `section.gap`) over raw pixel values
5. **Responsive spacing** — Reduce spacing by one step on mobile (e.g., `32px` -> `24px`)

See `tokens/spacing.json` for the full scale and semantic aliases.

---

## Dark Mode Strategy

Dark mode operates at the **semantic** layer only. Primitives remain unchanged.

| Aspect | Light Mode | Dark Mode |
|--------|-----------|-----------|
| Surfaces | Light backgrounds (#FAFAFA) | Dark backgrounds (#1A1A2E) |
| Text | Dark text on light surfaces | Light text on dark surfaces |
| Elevation | Lighter shadows | Subtle lighter surfaces (not shadows) |
| Token swap | Semantic tokens point to light primitives | Semantic tokens point to dark primitives |

### Implementation

- CSS custom properties swapped via `[data-theme="dark"]` or `prefers-color-scheme: dark`
- Override map defined in `tokens/colors.json` -> `dark` section
- **Every component state must be tested in both modes** — do not assume light-mode correctness implies dark-mode correctness
