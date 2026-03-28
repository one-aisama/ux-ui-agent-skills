# Dark Mode Strategy

A comprehensive guide to implementing dark mode across the design system. Covers semantic token swaps, surface hierarchy, component overrides, and testing methodology.

Reference: `tokens/colors.json` section `dark` defines the baseline semantic overrides.

---

## 1. Semantic Token Swap Table

Dark mode operates at the **semantic layer only**. Primitive tokens remain unchanged. The `[data-theme="dark"]` selector remaps semantic tokens to different primitives.

### Text Tokens

| Semantic Token | Light Mode Primitive | Dark Mode Primitive | Rationale |
|---------------|---------------------|---------------------|-----------|
| `text.primary` | `gray.900` (#111827) | `gray.50` (#f9fafb) | Maximum contrast on dark surfaces |
| `text.secondary` | `gray.600` (#4b5563) | `gray.400` (#9ca3af) | Legible secondary text without harshness |
| `text.tertiary` | `gray.400` (#9ca3af) | `gray.500` (#6b7280) | Subdued hints, not essential info |
| `text.disabled` | `gray.300` (#d1d5db) | `gray.600` (#4b5563) | Clearly de-emphasized |
| `text.on-action` | `white` (#ffffff) | `white` (#ffffff) | Unchanged — action buttons stay high contrast |
| `text.on-dark` | `gray.50` (#f9fafb) | `gray.50` (#f9fafb) | Unchanged — already designed for dark bg |
| `text.link` | `blue.600` (#2563eb) | `blue.400` (#60a5fa) | Lighter blue readable on dark surfaces |
| `text.link-hover` | `blue.800` (#1e40af) | `blue.300` (#93c5fd) | Visible hover shift on dark bg |

### Surface Tokens

| Semantic Token | Light Mode Primitive | Dark Mode Primitive | Rationale |
|---------------|---------------------|---------------------|-----------|
| `surface.page` | `white` (#ffffff) | `gray.950` (#030712) | Deepest background layer |
| `surface.card` | `white` (#ffffff) | `gray.900` (#111827) | One step above page — creates elevation |
| `surface.raised` | `white` (#ffffff) | `gray.800` (#1f2937) | Popovers, dropdowns — above card |
| `surface.sunken` | `gray.50` (#f9fafb) | `black` (#000000) | Recessed areas (code blocks, sidebar bg) |
| `surface.overlay` | `rgba(0,0,0,0.5)` | `rgba(0,0,0,0.7)` | Darker overlay to contrast against dark page |
| `surface.disabled` | `gray.100` (#f3f4f6) | `gray.800` (#1f2937) | Muted, non-interactive surface |

### Action Tokens

| Semantic Token | Light Mode Primitive | Dark Mode Primitive | Rationale |
|---------------|---------------------|---------------------|-----------|
| `action.primary` | `blue.600` (#2563eb) | `blue.500` (#3b82f6) | Slightly lighter for visibility on dark bg |
| `action.primary-hover` | `blue.700` (#1d4ed8) | `blue.400` (#60a5fa) | Hover lightens instead of darkening |
| `action.primary-active` | `blue.800` (#1e40af) | `blue.300` (#93c5fd) | Active continues the lightening direction |
| `action.secondary` | `gray.100` (#f3f4f6) | `gray.800` (#1f2937) | Neutral surface for secondary actions |
| `action.secondary-hover` | `gray.200` (#e5e7eb) | `gray.700` (#374151) | Lighter on hover for dark mode |
| `action.destructive` | `red.600` (#dc2626) | `red.500` (#ef4444) | Brighter red for dark surfaces |
| `action.destructive-hover` | `red.700` (#b91c1c) | `red.400` (#f87171) | Lightens on hover |

### Border Tokens

| Semantic Token | Light Mode Primitive | Dark Mode Primitive | Rationale |
|---------------|---------------------|---------------------|-----------|
| `border.default` | `gray.200` (#e5e7eb) | `gray.800` (#1f2937) | Subtle separation on dark surfaces |
| `border.strong` | `gray.300` (#d1d5db) | `gray.700` (#374151) | More prominent borders when needed |
| `border.focus` | `blue.500` (#3b82f6) | `blue.400` (#60a5fa) | Focus ring visible on dark bg |
| `border.error` | `red.500` (#ef4444) | `red.400` (#f87171) | Brighter error indication |
| `border.disabled` | `gray.200` (#e5e7eb) | `gray.700` (#374151) | Visible but muted |

### Feedback Tokens

| Semantic Token | Light Primitive | Dark Primitive | Rationale |
|---------------|----------------|----------------|-----------|
| `feedback.success-bg` | `green.50` | `green.950` (#052e16) | Dark tinted background |
| `feedback.success-text` | `green.800` | `green.300` (#86efac) | Readable on dark bg |
| `feedback.success-border` | `green.300` | `green.800` (#166534) | Visible without being harsh |
| `feedback.success-icon` | `green.600` | `green.400` (#4ade80) | Bright enough to scan |
| `feedback.warning-bg` | `amber.50` | `amber.950` (#451a03) | Dark tinted background |
| `feedback.warning-text` | `amber.800` | `amber.300` (#fcd34d) | Readable on dark bg |
| `feedback.warning-border` | `amber.300` | `amber.800` (#92400e) | Visible separation |
| `feedback.warning-icon` | `amber.600` | `amber.400` (#fbbf24) | Scannable icon color |
| `feedback.error-bg` | `red.50` | `red.950` (#450a0a) | Dark tinted background |
| `feedback.error-text` | `red.800` | `red.300` (#fca5a5) | Readable on dark bg |
| `feedback.error-border` | `red.300` | `red.800` (#991b1b) | Visible separation |
| `feedback.error-icon` | `red.600` | `red.400` (#f87171) | Scannable icon color |
| `feedback.info-bg` | `blue.50` | `blue.950` (#172554) | Dark tinted background |
| `feedback.info-text` | `blue.800` | `blue.300` (#93c5fd) | Readable on dark bg |
| `feedback.info-border` | `blue.300` | `blue.800` (#1e40af) | Visible separation |
| `feedback.info-icon` | `blue.600` | `blue.400` (#60a5fa) | Scannable icon color |

### Interactive Tokens

| Semantic Token | Light Primitive | Dark Primitive | Rationale |
|---------------|----------------|----------------|-----------|
| `interactive.hover-overlay` | `rgba(0,0,0,0.04)` | `rgba(255,255,255,0.06)` | White-based overlay on dark surfaces |
| `interactive.active-overlay` | `rgba(0,0,0,0.08)` | `rgba(255,255,255,0.10)` | Stronger white overlay for pressed state |
| `interactive.selected-bg` | `blue.50` | `blue.950` (#172554) | Dark tinted selection |
| `interactive.selected-border` | `blue.200` | `blue.800` (#1e40af) | Visible selection border |
| `interactive.focus-ring` | `blue.500` | `blue.400` | Brighter focus ring on dark bg |

---

## 2. Shadow Behavior

### Principle

In light mode, shadows create depth by darkening the surface below an element. In dark mode, surfaces are already dark, so traditional shadows become invisible. Dark mode uses **two strategies** to communicate elevation:

1. **Lighter surface colors** — Higher elements use lighter shades of gray (page=950 < card=900 < raised=800). This is the primary elevation signal.
2. **Reduced, softened shadows** — Shadows use lower opacity and larger blur to create a subtle ambient glow rather than a hard drop shadow.

### Shadow Token Overrides

| Shadow Token | Light Mode Value | Dark Mode Value |
|-------------|-----------------|-----------------|
| `elevation.sm` | `0 1px 2px rgba(0,0,0,0.05)` | `0 1px 2px rgba(0,0,0,0.3)` |
| `elevation.md` | `0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -2px rgba(0,0,0,0.1)` | `0 4px 6px -1px rgba(0,0,0,0.4), 0 2px 4px -2px rgba(0,0,0,0.3)` |
| `elevation.lg` | `0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -4px rgba(0,0,0,0.1)` | `0 10px 15px -3px rgba(0,0,0,0.5), 0 4px 6px -4px rgba(0,0,0,0.4)` |
| `elevation.xl` | `0 20px 25px -5px rgba(0,0,0,0.1), 0 8px 10px -6px rgba(0,0,0,0.1)` | `0 20px 25px -5px rgba(0,0,0,0.6), 0 8px 10px -6px rgba(0,0,0,0.5)` |
| `elevation.2xl` | `0 25px 50px -12px rgba(0,0,0,0.25)` | `0 25px 50px -12px rgba(0,0,0,0.7)` |
| `shadow.inner` | `inset 0 2px 4px rgba(0,0,0,0.05)` | `inset 0 2px 4px rgba(0,0,0,0.3)` |
| `shadow.focus-ring` | `0 0 0 2px white, 0 0 0 4px blue.500` | `0 0 0 2px gray.900, 0 0 0 4px blue.400` |
| `shadow.colored` | `0 4px 14px rgba(37,99,235,0.25)` | `0 4px 14px rgba(59,130,246,0.35)` |

### Rules

1. **Surface color is the primary elevation cue** — shadows are secondary reinforcement.
2. **Never rely solely on shadow** to distinguish overlapping surfaces in dark mode.
3. **Focus ring inner color** changes from white to `gray.900` to match the dark surface behind it.
4. **Colored shadows** increase opacity slightly so the color remains perceptible against dark backgrounds.

---

## 3. Border Behavior

### Principle

In light mode, borders on white surfaces use light grays (gray.200, gray.300) for subtle separation. In dark mode, these same light values would create harsh, high-contrast lines. Dark mode borders use darker grays that are one or two steps lighter than the surface they sit on.

### Contrast Considerations

| Surface | Light Border | Dark Border | Visible? |
|---------|-------------|-------------|----------|
| `surface.page` (950) | gray.200 | gray.800 | Yes — 1 step lighter |
| `surface.card` (900) | gray.200 | gray.800 | Subtle — same as page border, but card itself is lighter |
| `surface.raised` (800) | gray.200 | gray.700 | Yes — `border.strong` maps here |

### Rules

1. **Use `border.strong` for essential boundaries** in dark mode. The default border (gray.800 on gray.900 card) has low contrast — acceptable for decorative dividers, insufficient for interactive element boundaries.
2. **Input fields need `border.strong` in dark mode** — users must clearly see input boundaries. Override `component.input.border` to `border.strong` in dark mode.
3. **Dividers (`<hr>`, table row borders)** can use `border.default` — they are navigational aids, not interactive boundaries.
4. **Focus and error borders** use brighter variants (blue.400, red.400) that do not need additional adjustment.

### Component Border Overrides (Dark Mode)

| Component | Light Border Token | Dark Border Override | Why |
|-----------|-------------------|---------------------|-----|
| Input (resting) | `border.default` (gray.200) | `border.strong` (gray.700) | Must be clearly visible |
| Input (hover) | `border.strong` (gray.300) | `gray.600` | Hover must be distinguishable from resting |
| Card | `border.default` (gray.200) | `border.default` (gray.800) | Decorative, surface color difference is enough |
| Table row | `border.default` (gray.200) | `border.default` (gray.800) | Navigational aid, low contrast acceptable |
| Dropdown menu | `border.default` (gray.200) | `border.strong` (gray.700) | Floating elements need clear boundaries |

---

## 4. Icons & Illustrations

### System Icons (Mono-color)

System icons use `currentColor` and inherit from their parent text color. They automatically adapt to dark mode through the text token swap.

**Rules:**
- Always set icon color via a semantic text token (`text.primary`, `text.secondary`, `feedback.error-icon`).
- Never hardcode a fill or stroke color on a system icon.
- Test icon visibility at all three text hierarchy levels (primary, secondary, tertiary).

### Multi-color / Custom Illustrations

Custom illustrations and spot illustrations (onboarding, empty states, marketing) do NOT automatically adapt. They require explicit dark variants.

**Strategy options (pick one per project):**

| Strategy | Description | Effort | Quality |
|----------|-------------|--------|---------|
| **Dual asset** | Create separate light and dark illustration files | High | Best — full creative control |
| **CSS filter** | Apply `filter: brightness(0.85) saturate(1.1)` in dark mode | Low | Acceptable — may shift colors unexpectedly |
| **SVG token swap** | Use CSS custom properties inside SVGs for key fills | Medium | Good — preserves intent, some manual work |

**Recommendations:**
1. For hero illustrations and branding — use **dual assets**.
2. For decorative spots (empty states, placeholders) — use **CSS filter** as a baseline, replace with dual assets only if the filter produces poor results.
3. For inline SVG icons with 2-3 colors — use **SVG token swap**.

### Dark Variant File Naming

```
illustration-onboarding.svg        ← light mode
illustration-onboarding-dark.svg   ← dark mode
```

Load the correct variant via a utility:

```tsx
function useThemedAsset(baseName: string): string {
  const theme = useTheme(); // "light" | "dark"
  return theme === "dark" ? `${baseName}-dark` : baseName;
}
```

---

## 5. Images

### Principle

Photographic images and screenshots on a bright white background are visually jarring on a dark surface. The white areas of the image glow and break the visual hierarchy.

### Rules

1. **Reduce brightness slightly** — apply `filter: brightness(0.9)` to `<img>` elements in dark mode. This dims bright whites without destroying the image.
2. **Add a subtle border-radius and border** — images on dark surfaces benefit from a 1px `border.default` to define their edges, especially screenshots with white backgrounds.
3. **Avoid pure white image backgrounds** — when generating assets, use `gray.50` (#f9fafb) instead of #ffffff so they blend better in both modes.
4. **Dark mode screenshots** — if your product supports dark mode, show dark mode screenshots when the user is in dark mode. Use the `<picture>` element with `prefers-color-scheme` media query.

### Implementation

```css
[data-theme="dark"] img:not([data-no-dim]) {
  filter: brightness(0.9);
}

[data-theme="dark"] img[data-screenshot] {
  border: 1px solid var(--border-default);
  border-radius: var(--radius-md);
}
```

```html
<picture>
  <source srcset="screenshot-dark.png" media="(prefers-color-scheme: dark)" />
  <img src="screenshot-light.png" alt="Dashboard overview" />
</picture>
```

---

## 6. Component-Specific Overrides

Beyond the global semantic swap, some components need additional dark mode attention.

### Card Surfaces

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Background | `surface.card` (white) | `surface.card` (gray.900) | Handled by semantic swap |
| Border | `border.default` (gray.200) | `border.default` (gray.800) | May be invisible — consider removing border entirely and relying on surface contrast |
| Shadow | `elevation.sm` | `elevation.sm` (dark override) | Reduced effectiveness, surface contrast is primary cue |
| Hover | Increase shadow to `elevation.md` | Lighten background to `gray.800` | Shadow-based hover is weak in dark mode; surface lightening is more effective |

### Input Backgrounds

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Background | `surface.card` (white) | `gray.800` | Slightly lighter than card surface for visual separation |
| Border (resting) | `border.default` (gray.200) | `gray.700` | Stronger border needed |
| Border (hover) | `border.strong` (gray.300) | `gray.600` | Must differentiate from resting |
| Border (focus) | `border.focus` (blue.500) | `blue.400` | Brighter for visibility |
| Placeholder text | `text.tertiary` (gray.400) | `gray.500` | Subdued but readable |

### Modal / Dialog Overlays

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Backdrop | `rgba(0,0,0,0.5)` | `rgba(0,0,0,0.7)` | Heavier dimming needed because dark page is already dark |
| Modal surface | `surface.raised` (white) | `surface.raised` (gray.800) | Must clearly float above page |
| Modal border | none | `border.default` (gray.700) | Add border in dark mode for edge definition |
| Modal shadow | `elevation.xl` | `elevation.xl` (dark override) | Supplementary to surface contrast |

### Badge / Tag

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Neutral bg | `gray.100` | `gray.800` | |
| Neutral text | `gray.700` | `gray.300` | |
| Primary bg | `blue.100` | `blue.900` (#1e3a8a) | Deep saturated background |
| Primary text | `blue.700` | `blue.300` (#93c5fd) | Light text on dark badge |
| Success bg | `green.100` | `green.900` (#14532d) | |
| Success text | `green.700` | `green.300` (#86efac) | |
| Warning bg | `amber.100` | `amber.900` (#78350f) | |
| Warning text | `amber.700` | `amber.300` (#fcd34d) | |
| Error bg | `red.100` | `red.900` (#7f1d1d) | |
| Error text | `red.700` | `red.300` (#fca5a5) | |

### Avatar

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Fallback bg | `gray.200` | `gray.700` | |
| Fallback text | `gray.700` | `gray.200` | |
| Border (stacked) | `white` | `gray.900` | Matches card surface for clean stacking |

### Toggle / Switch

| Property | Light Mode | Dark Mode | Notes |
|----------|-----------|-----------|-------|
| Track (off) | `gray.200` | `gray.700` | |
| Track (on) | `blue.600` | `blue.500` | Matches action.primary dark swap |
| Thumb | `white` | `white` | Stays white in both modes |

---

## 7. Implementation

### CSS Custom Properties Approach

Define all semantic tokens as CSS custom properties on `:root` (light mode defaults), then override in the `[data-theme="dark"]` selector.

```css
/* === Light mode (default) === */
:root {
  /* Text */
  --text-primary: #111827;
  --text-secondary: #4b5563;
  --text-tertiary: #9ca3af;
  --text-disabled: #d1d5db;
  --text-on-action: #ffffff;
  --text-link: #2563eb;
  --text-link-hover: #1e40af;

  /* Surfaces */
  --surface-page: #ffffff;
  --surface-card: #ffffff;
  --surface-raised: #ffffff;
  --surface-sunken: #f9fafb;
  --surface-overlay: rgba(0, 0, 0, 0.5);
  --surface-disabled: #f3f4f6;

  /* Actions */
  --action-primary: #2563eb;
  --action-primary-hover: #1d4ed8;
  --action-primary-active: #1e40af;
  --action-secondary: #f3f4f6;
  --action-secondary-hover: #e5e7eb;
  --action-destructive: #dc2626;
  --action-destructive-hover: #b91c1c;

  /* Borders */
  --border-default: #e5e7eb;
  --border-strong: #d1d5db;
  --border-focus: #3b82f6;
  --border-error: #ef4444;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1);
  --shadow-focus-ring: 0 0 0 2px #ffffff, 0 0 0 4px #3b82f6;

  /* Interactive */
  --hover-overlay: rgba(0, 0, 0, 0.04);
  --active-overlay: rgba(0, 0, 0, 0.08);
  --selected-bg: #eff6ff;
}

/* === Dark mode === */
[data-theme="dark"] {
  /* Text */
  --text-primary: #f9fafb;
  --text-secondary: #9ca3af;
  --text-tertiary: #6b7280;
  --text-disabled: #4b5563;
  --text-on-action: #ffffff;
  --text-link: #60a5fa;
  --text-link-hover: #93c5fd;

  /* Surfaces */
  --surface-page: #030712;
  --surface-card: #111827;
  --surface-raised: #1f2937;
  --surface-sunken: #000000;
  --surface-overlay: rgba(0, 0, 0, 0.7);
  --surface-disabled: #1f2937;

  /* Actions */
  --action-primary: #3b82f6;
  --action-primary-hover: #60a5fa;
  --action-primary-active: #93c5fd;
  --action-secondary: #1f2937;
  --action-secondary-hover: #374151;
  --action-destructive: #ef4444;
  --action-destructive-hover: #f87171;

  /* Borders */
  --border-default: #1f2937;
  --border-strong: #374151;
  --border-focus: #60a5fa;
  --border-error: #f87171;

  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.4), 0 2px 4px -2px rgba(0, 0, 0, 0.3);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.5), 0 4px 6px -4px rgba(0, 0, 0, 0.4);
  --shadow-focus-ring: 0 0 0 2px #111827, 0 0 0 4px #60a5fa;

  /* Interactive */
  --hover-overlay: rgba(255, 255, 255, 0.06);
  --active-overlay: rgba(255, 255, 255, 0.10);
  --selected-bg: #172554;
}

/* === System preference fallback === */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* Same overrides as [data-theme="dark"] above */
    /* Only applies when no explicit data-theme is set */
  }
}
```

### Theme Switching Logic

```typescript
type Theme = "light" | "dark" | "system";

function setTheme(theme: Theme): void {
  const root = document.documentElement;

  if (theme === "system") {
    root.removeAttribute("data-theme");
    localStorage.removeItem("theme");
  } else {
    root.setAttribute("data-theme", theme);
    localStorage.setItem("theme", theme);
  }
}

// Initialize on page load (prevents flash of wrong theme)
function initTheme(): void {
  const stored = localStorage.getItem("theme") as Theme | null;
  if (stored) {
    document.documentElement.setAttribute("data-theme", stored);
  }
  // If no stored preference, data-theme is absent → system preference applies via @media query
}
```

### Preventing Flash of Incorrect Theme (FOIT)

Place a blocking `<script>` in `<head>` before any stylesheet:

```html
<script>
  (function () {
    var t = localStorage.getItem("theme");
    if (t) document.documentElement.setAttribute("data-theme", t);
  })();
</script>
```

### Tailwind v4 Integration

In Tailwind v4 with `@theme`, map design tokens to CSS custom properties in your theme config. Use the `dark:` variant which reads from `[data-theme="dark"]` or `prefers-color-scheme`:

```css
@theme {
  --color-surface-page: var(--surface-page);
  --color-surface-card: var(--surface-card);
  --color-text-primary: var(--text-primary);
  /* ... all semantic tokens ... */
}
```

Then in components:

```html
<div class="bg-surface-card text-text-primary border-border-default">
  <!-- Automatically adapts to dark mode via CSS custom property swap -->
</div>
```

---

## 8. Testing Checklist

Run this checklist for **every component** and **every page** before shipping dark mode support.

### Per-Component Checks

```
COMPONENT: _______________

Contrast & Readability
  [ ] Primary text on component surface passes 4.5:1
  [ ] Secondary text on component surface passes 4.5:1
  [ ] Placeholder/tertiary text is distinguishable (even if below 4.5:1)
  [ ] Text on action buttons (primary, destructive) passes 4.5:1
  [ ] Icon colors are visible against their backgrounds (3:1 minimum)
  [ ] Link text is distinguishable from body text

Surface & Elevation
  [ ] Component surface is visually distinct from the page background
  [ ] Raised elements (dropdowns, popovers) are clearly above the surface below
  [ ] Elevation hierarchy is preserved: page < card < raised < overlay
  [ ] Sunken areas (code blocks, sidebars) are visually recessed

Borders & Separators
  [ ] Interactive element boundaries (inputs, selects) are clearly visible
  [ ] Decorative borders provide sufficient separation
  [ ] Focus ring is visible against both surface.card and surface.page
  [ ] Error borders are clearly distinct from default/focus borders

States
  [ ] Default state is correct
  [ ] Hover state shows a visible change
  [ ] Focus state shows the focus ring clearly
  [ ] Active/pressed state is distinct from hover
  [ ] Disabled state looks disabled (muted, no pointer)
  [ ] Loading state skeleton/spinner is visible
  [ ] Error state is clearly communicated (not just color)
  [ ] Selected state is distinguishable from hover

Feedback & Status
  [ ] Success alert: bg, border, text, icon all visible and cohesive
  [ ] Warning alert: bg, border, text, icon all visible and cohesive
  [ ] Error alert: bg, border, text, icon all visible and cohesive
  [ ] Info alert: bg, border, text, icon all visible and cohesive
  [ ] Badges are readable at all color variants

Images & Media
  [ ] Images are not blindingly bright on dark surfaces
  [ ] Screenshots have defined edges (border or radius)
  [ ] Illustrations look intentional (not just dimmed)
  [ ] Video player controls are visible

Transition
  [ ] Switching themes does not cause layout shift
  [ ] Transition between themes feels smooth (optional 150ms transition on bg/color)
  [ ] No flash of wrong theme on page load
  [ ] Theme preference persists across sessions
```

### Page-Level Checks

```
PAGE: _______________

  [ ] No pure white (#fff) or pure black (#000) surfaces bleeding through
  [ ] Overall page does not feel "inverted" — it feels intentionally designed
  [ ] Navigation elements are legible and interactive states work
  [ ] Scrollbar styling matches theme (if custom)
  [ ] Selection highlight (::selection) is themed
  [ ] Print stylesheet is not affected by dark mode overrides
  [ ] Embedded third-party widgets (maps, video, charts) look acceptable
  [ ] Content from CMS/markdown renders correctly (code blocks, blockquotes)
```

### Automated Testing

| Tool | What It Checks | Integration |
|------|---------------|-------------|
| axe-core | Color contrast violations | CI pipeline, Storybook addon |
| Chromatic / Percy | Visual regression across themes | PR checks |
| Playwright | Toggle theme → screenshot → compare | E2E test suite |
| Storybook | Render every component in both themes | Dev environment |

### Recommended Test Matrix

| Component | Light Default | Light Hover | Light Focus | Light Error | Dark Default | Dark Hover | Dark Focus | Dark Error |
|-----------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Button (primary) | | | | | | | | |
| Button (secondary) | | | | | | | | |
| Button (destructive) | | | | | | | | |
| Input | | | | | | | | |
| Select | | | | | | | | |
| Card | | | | | | | | |
| Modal | | | | | | | | |
| Alert (all 4) | | | | | | | | |
| Badge (all variants) | | | | | | | | |
| Avatar | | | | | | | | |
| Toggle | | | | | | | | |
| Tooltip | | | | | | | | |
| Dropdown | | | | | | | | |
| Data Table | | | | | | | | |

Mark each cell with PASS / FAIL / N/A during the review.
