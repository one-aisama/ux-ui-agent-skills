# Atoms

The smallest, indivisible UI elements. Each atom maps directly to design tokens and has a single, focused responsibility.

---

## 1. Button

The primary interactive control for triggering actions.

**Anatomy:** `[icon?] [label] [icon?]` within a pressable container.

**Variants:**
| Variant | Use Case | Token Mapping |
|---------|----------|---------------|
| Primary | Main CTA — one per visible area | `component.button.primary-*` |
| Secondary | Supporting actions | `component.button.secondary-*` |
| Ghost | Tertiary, toolbar, inline actions | `component.button.ghost-*` |
| Destructive | Delete, remove, irreversible | `component.button.destructive-*` |
| Link | Navigation-styled as button | `semantic.text.link` + underline |

**Sizes:**
| Size | Height | Padding | Font | Icon |
|------|--------|---------|------|------|
| sm | 32px | 12px × 8px | body-sm (14px) | 16px |
| md | 40px | 16px × 10px | body-base (16px) | 20px |
| lg | 48px | 24px × 12px | body-lg (18px) | 24px |

**States (6):**
1. **Default** — resting state
2. **Hover** — cursor over; background shifts one shade
3. **Active/Pressed** — mouse down; background shifts two shades
4. **Focus** — keyboard focus; apply `focus-ring` shadow token
5. **Disabled** — `opacity: 0.5`, `cursor: not-allowed`, `pointer-events: none`
6. **Loading** — replace icon with spinner, add `aria-busy="true"`, disable interaction

**Accessibility:**
- Native `<button>` element always; never `<div>`
- Icon-only: add `aria-label`
- Loading: `aria-busy="true"`, announce via live region
- Disabled: use `disabled` attribute (removes from tab order) or `aria-disabled="true"` (keeps in tab order for discoverability)
- Minimum target size: 24×24px (WCAG 2.5.8)

**Cross-references:** [ARIA Button Pattern](../accessibility/aria-patterns.md) | [React Implementation](../frameworks/react-tailwind.md) | Tokens: `tokens/colors.json` → `component.button.*`

---

## 2. Input

Single-line text entry field.

**Anatomy:** `[leading-icon?] [prefix?] [input] [suffix?] [trailing-icon?] [clear?]`

**Variants:** Text, Email, Password (toggle visibility), Number, Search, URL, Tel

**Sizes:**
| Size | Height | Padding | Font |
|------|--------|---------|------|
| sm | 32px | 12px × 8px | body-sm |
| md | 40px | 12px × 10px | body-base |
| lg | 48px | 16px × 12px | body-lg |

**States (6):**
1. **Default** — `input.border` (gray-200)
2. **Hover** — `input.border-hover` (gray-300)
3. **Focus** — `input.border-focus` (blue-500) + focus ring
4. **Error** — `input.border-error` (red-500) + error icon + message
5. **Disabled** — `input.bg-disabled` + reduced opacity
6. **Read-only** — same as default but non-editable, no hover state

**Accessibility:**
- Always associate with `<label>` via `for`/`id`
- Error messages: `aria-describedby` linking to error text + `aria-invalid="true"`
- Required: `aria-required="true"` + visual indicator (asterisk)
- Password toggle: `aria-label="Show password"` / `"Hide password"`

**Cross-references:** [ARIA Combobox Pattern](../accessibility/aria-patterns.md) | [React Implementation](../frameworks/react-tailwind.md) | Tokens: `tokens/colors.json` → `component.input.*`

---

## 3. Label

Text label associated with a form control.

**Anatomy:** `[label-text] [required-indicator?] [optional-indicator?]`

**Token Mapping:** `textStyle.label` (14px/medium), `semantic.text.primary`

**Accessibility:**
- Use `<label for="input-id">` — clicking label focuses input
- Required: append `<span aria-hidden="true">*</span>` + `aria-required` on input
- Group labels: use `<legend>` inside `<fieldset>`

---

## 4. Icon

Decorative or informational pictogram.

**Sizes:** 12px, 16px, 20px, 24px, 32px

**Token Mapping:** Inherits `currentColor` from parent.

**Accessibility:**
- **Decorative**: `aria-hidden="true"` (most icons next to text labels)
- **Informational**: `role="img"` + `aria-label="Description"`
- **Interactive**: wrap in `<button>` with `aria-label`

---

## 5. Badge

A small status descriptor for counts or labels.

**Anatomy:** `[dot?] [text] [remove-button?]`

**Variants:**
| Variant | Use Case | Tokens |
|---------|----------|--------|
| Neutral | Default tags | `component.badge.neutral-*` |
| Primary | Active/selected | `component.badge.primary-*` |
| Success | Positive status | `component.badge.success-*` |
| Warning | Attention needed | `component.badge.warning-*` |
| Error | Critical/failed | `component.badge.error-*` |
| Dot | Minimal indicator | 8px circle, no text |

**Sizes:** sm (20px height), md (24px height)

**Token Mapping:** `radius.badge` (full), `spacing.badge-padding-*`

**Accessibility:**
- Notification badges: `aria-label="3 unread messages"` (don't rely on number alone)
- Status badges: pair with descriptive text, not just color

---

## 6. Avatar

A circular representation of a user or entity.

**Anatomy:** `[image | initials | fallback-icon]` within a circle.

**Sizes:** xs (24px), sm (32px), md (40px), lg (48px), xl (64px), 2xl (96px)

**Token Mapping:** `component.avatar.*`, `radius.avatar` (full)

**Fallback Hierarchy:**
1. User-uploaded image
2. Initials (first + last name initial)
3. Generic person icon

**Accessibility:**
- Image avatars: `alt="[User name]"` or `alt=""` if name is adjacent
- Initials: `aria-label="[Full name]"`
- Status indicator (online dot): announce via `aria-label`, not just color

---

## 7. Checkbox

A control for binary or tri-state selection.

**Anatomy:** `[checkbox-box] [label] [help-text?]`

**Sizes:** sm (16px box), md (20px box), lg (24px box)

**States:** Unchecked, Checked, Indeterminate, Hover, Focus, Disabled

**Accessibility:**
- Prefer native `<input type="checkbox">`
- Indeterminate: set via JS `el.indeterminate = true` + `aria-checked="mixed"`
- Group: wrap in `<fieldset>` with `<legend>`
- See `aria-patterns.md` → Checkbox

---

## 8. Radio

A control for selecting exactly one option from a group.

**Anatomy:** `[radio-circle] [label] [help-text?]`

**Sizes:** sm (16px), md (20px), lg (24px)

**States:** Unselected, Selected, Hover, Focus, Disabled

**Accessibility:**
- Always in a `<fieldset>` with `<legend>` describing the group
- Arrow keys navigate between radios
- See `aria-patterns.md` → Radio Group

---

## 9. Toggle (Switch)

An on/off control for immediate state changes.

**Anatomy:** `[track] [thumb]` with optional `[label]` and `[status-text]`

**Sizes:**
| Size | Track | Thumb |
|------|-------|-------|
| sm | 36×20px | 16px |
| md | 44×24px | 20px |
| lg | 52×28px | 24px |

**States:** Off, On, Hover, Focus, Disabled

**Accessibility:**
- Use `role="switch"` with `aria-checked`
- Label must describe what the switch controls
- Avoid using for form submissions — use checkbox instead
- See `aria-patterns.md` → Switch

---

## 10. Tooltip

A non-interactive text popup for supplementary information.

**Anatomy:** `[trigger-element]` → `[tooltip-container] [arrow] [text]`

**Placement:** Top (default), Right, Bottom, Left — auto-flip when near viewport edge.

**Token Mapping:** `surface: gray-900`, `text: white`, `radius: tooltip (md)`, `shadow: md`

**Behavior:**
- Show on hover (300ms delay) and focus
- Hide on mouse leave, blur, Escape, scroll
- Max width: 240px (prevent tooltip walls)
- No interactive content inside

**Accessibility:**
- `role="tooltip"` + `aria-describedby` on trigger
- Must appear on keyboard focus, not just hover
- Content is supplementary, not essential
- See `aria-patterns.md` → Tooltip

---

## 11. Divider (Separator)

A visual boundary between content sections or list items.

**Anatomy:** `[line]` or `[line] [label] [line]` for labeled variants.

**Variants:**
| Variant | Use Case | Token Mapping |
|---------|----------|---------------|
| Horizontal | Section separation, list dividers | `component.divider.horizontal-*` |
| Vertical | Inline element separation (toolbars, nav) | `component.divider.vertical-*` |
| Labeled | Section breaks with descriptive text (e.g., "OR") | `component.divider.labeled-*` |
| Inset | Indented from one or both edges (list items with icons) | `component.divider.inset-*` |

**Token Mapping:**
- Color: `border.default` (gray-200 light, gray-700 dark)
- Width (horizontal): `border.width.thin` (1px)
- Width (vertical): `border.width.thin` (1px)
- Label text: `textStyle.caption` (12px), `semantic.text.secondary`
- Label gap: `spacing.inline.md` (12px) on each side of label text
- Inset margin: `spacing.inline.2xl` (40px) from leading edge

**Sizes:**
| Orientation | Thickness | Length |
|-------------|-----------|--------|
| Horizontal | 1px height | 100% parent width |
| Vertical | 1px width | 100% parent height or explicit height |

**Accessibility:**
- Use `<hr>` for horizontal thematic breaks (carries implicit `role="separator"`)
- Vertical/decorative dividers: `role="separator"` + `aria-orientation="vertical"`
- Labeled dividers: `role="separator"` with visible label; label text is decorative, not actionable
- Purely decorative dividers (visual only, no semantic break): `role="none"` or `aria-hidden="true"`

---

## 12. Spinner (Loading)

An animated indicator communicating an in-progress operation.

**Anatomy:** `[circular-track] [rotating-indicator]` with optional `[label]`.

**Variants:**
| Variant | Use Case | Token Mapping |
|---------|----------|---------------|
| Default | Standalone loading indicator | `component.spinner.default-*` |
| Overlay | Centered over content with dimmed backdrop | `component.spinner.overlay-*` |
| Inline | Within text flow or next to a label | `component.spinner.inline-*` |
| Button | Inside a button replacing the icon slot | Inherits button color tokens |

**Sizes:**
| Size | Diameter | Stroke | Use Case |
|------|----------|--------|----------|
| sm | 16px | 2px | Inline, button, badge |
| md | 24px | 3px | Default standalone |
| lg | 48px | 4px | Page/section loading, overlay center |

**Token Mapping:**
- Track: `border.subtle` (gray-100 light, gray-800 dark) at `opacity: 0.3`
- Indicator: `action.primary` (blue-600) or `currentColor` to inherit parent
- Overlay backdrop: `surface.overlay` (black at 40% opacity)
- Animation: `rotate 360deg`, duration `750ms`, timing `linear`, iteration `infinite`
- Reduced motion: replace rotation with pulsing opacity (`opacity: 0.4 ↔ 1`, 1s ease-in-out)

**States:**
1. **Spinning** — default active state
2. **Determinate** (optional) — shows progress arc (0–100%) for known-duration operations

**Accessibility:**
- Container: `role="status"` + `aria-live="polite"` for non-blocking loading
- Urgent/blocking loading: `aria-live="assertive"`
- Add `aria-busy="true"` on the region being loaded (not on the spinner itself)
- Visually hidden label: `aria-label="Loading"` or visible label beneath spinner
- Overlay variant: set `aria-busy="true"` + `aria-hidden="true"` on obscured content
- Never rely on animation alone — always provide text alternative (visible or SR-only)

---

## 13. Skeleton

A placeholder shape representing content that is loading, providing perceived performance.

**Anatomy:** `[shape]` with shimmer animation.

**Variants:**
| Variant | Shape | Dimensions | Use Case |
|---------|-------|------------|----------|
| Line | Rounded rectangle | Height: 12–16px, width: 60–100% | Text lines, labels |
| Circle | Circle | 32–64px diameter | Avatars, icons |
| Card | Rounded rectangle | Matches card dimensions | Card placeholders |
| Table row | Row of rectangles | Matches table cell widths | Table body loading |
| Image | Rectangle | Aspect ratio of target image | Media placeholders |

**Sizes:**
| Size | Line height | Radius |
|------|-------------|--------|
| sm | 12px | `radius.sm` (4px) |
| md | 16px | `radius.md` (6px) |
| lg | 24px | `radius.lg` (8px) |

**Token Mapping:**
- Base color: `surface.sunken` (gray-100 light, gray-800 dark)
- Shimmer highlight: `surface.raised` (gray-50 light, gray-700 dark) at 60% opacity
- Animation: shimmer gradient sweep left-to-right, duration `1.5s`, timing `ease-in-out`, iteration `infinite`
- Radius: matches the target component's radius token (e.g., skeleton for a card uses `radius.card`)
- Spacing: matches the target component's spacing (skeleton layout mirrors real layout)
- Reduced motion: disable shimmer, show static placeholder at base color

**States:**
1. **Loading** — shimmer animation active
2. **Loaded** — fade out skeleton, fade in real content (crossfade 200ms)

**Accessibility:**
- Container: `aria-busy="true"` on the loading region
- Skeleton elements: `aria-hidden="true"` (they are purely visual)
- Provide a visually hidden `role="status"` element: "Loading content..." for screen readers
- On load complete: remove `aria-busy`, announce content via live region if appropriate
- Never use skeleton as the only loading signal for critical, time-sensitive operations — pair with a text status

---

## 14. Text

A semantic text element for body content, captions, and supporting text.

**Anatomy:** `[text-content]` with optional `[truncation]`.

**Variants (Semantic Levels):**
| Variant | Style Token | Size | Weight | Use Case |
|---------|-------------|------|--------|----------|
| Body | `textStyle.body-base` | 16px / 1.5 | Regular (400) | Primary reading content |
| Body Small | `textStyle.body-sm` | 14px / 1.5 | Regular (400) | Secondary content, descriptions |
| Body Large | `textStyle.body-lg` | 18px / 1.5 | Regular (400) | Lead paragraphs, emphasis |
| Caption | `textStyle.caption` | 12px / 1.5 | Regular (400) | Metadata, timestamps, help text |
| Overline | `textStyle.overline` | 12px / 1.5 | Medium (500), uppercase, `letter-spacing: 0.05em` | Section labels, category tags |
| Label | `textStyle.label` | 14px / 1.43 | Medium (500) | Form labels, UI labels |

**Token Mapping:**
- Color: `semantic.text.primary` (default), `semantic.text.secondary`, `semantic.text.tertiary`, `semantic.text.disabled`
- Font family: `font.family.sans` (Inter / system-ui)
- Max line width: `65ch` for body text (optimal readability)
- Paragraph spacing: `spacing.stack.md` (8px) between paragraphs

**Truncation:**
| Mode | Behavior | CSS | Use Case |
|------|----------|-----|----------|
| Single-line ellipsis | Cut with "..." at container edge | `overflow: hidden; text-overflow: ellipsis; white-space: nowrap` | Table cells, nav items, tags |
| Multi-line clamp | Cut after N lines with "..." | `-webkit-line-clamp: N; display: -webkit-box; -webkit-box-orient: vertical; overflow: hidden` | Card descriptions, previews |
| None | Wrap naturally within `max-width: 65ch` | Default | Body content, articles |

**Sizes:** Not applicable — use semantic variant to select size. Do not add arbitrary size props; map to the design scale.

**States:**
1. **Default** — `semantic.text.primary`
2. **Secondary** — `semantic.text.secondary` (reduced emphasis)
3. **Disabled** — `semantic.text.disabled` (muted, non-interactive context)
4. **Link** — `semantic.text.link` + underline (when text is interactive)
5. **Error** — `semantic.feedback.error-text` (inline validation messages)

**Accessibility:**
- Use semantic HTML elements: `<p>` for paragraphs, `<span>` for inline, `<small>` for fine print, `<strong>`/`<em>` for emphasis
- Overline text: despite visual styling, use appropriate heading level or `<span>` — uppercase alone does not convey hierarchy to screen readers
- Truncated text: ensure the full text is available via `title` attribute or expandable interaction (tooltip, "Read more")
- Minimum font size: 12px (`textStyle.caption`). Never go below 12px for any user-facing text.
- Line length: enforce `max-width: 65ch` on body text containers for readability (WCAG 1.4.8 recommendation)
- Color contrast: `semantic.text.primary` on `surface.page` must meet 4.5:1; `semantic.text.secondary` must meet 4.5:1 for normal text or 3:1 for large text
