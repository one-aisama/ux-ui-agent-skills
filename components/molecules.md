# Molecules

Composed of atoms, molecules form functional groups that handle a specific user task.

---

## 1. Form Field

A complete form input unit: label + input + help text + error message.

**Composition:** `Label` + `Input` + (optional) help text + (optional) error text

**Anatomy:**
```
┌─────────────────────────────┐
│ Label *                     │  ← Label atom + required indicator
│ ┌─────────────────────────┐ │
│ │ [icon] Placeholder...   │ │  ← Input atom
│ └─────────────────────────┘ │
│ Help text or error message  │  ← Caption text style
└─────────────────────────────┘
```

**Layout:**
- Stack: vertical (default) — label on top
- Inline: horizontal — label left, input right (forms with short labels)
- Label-input gap: `spacing.stack.xs` (4px)
- Input-help gap: `spacing.stack.xs` (4px)
- Field-to-field gap: `spacing.stack.lg` (16px)

**States:**
- Default → Hover → Focus → Filled → Error → Disabled
- Error replaces help text, uses `feedback.error-text` color + error icon
- Required: asterisk on label + `aria-required="true"` on input

**Responsive:**
- Inline layout collapses to stacked below `md` breakpoint
- In multi-column forms, fields span full width below `sm`

**Accessibility:**
- `<label for>` associates label with input
- Help text: `aria-describedby` on input
- Error: `aria-invalid="true"` + `aria-describedby` pointing to error message
- Error message includes `role="alert"` or is in a live region

**Cross-references:** Composed of [Input](atoms.md#2-input) + [Label](atoms.md#3-label). See [ARIA Patterns](../accessibility/aria-patterns.md) for form validation.

---

## 2. Search Bar

A text input optimized for search with clear functionality and optional suggestions.

**Composition:** `Icon` (search) + `Input` (type="search") + `Button` (clear) + optional dropdown

**Anatomy:**
```
┌─────────────────────────────────────┐
│ 🔍 Search products...          [×] │
└─────────────────────────────────────┘
│ Recent: Blue widget                 │  ← Optional suggestion dropdown
│ Popular: Red gadget                 │
└─────────────────────────────────────┘
```

**Variants:**
| Variant | Behavior |
|---------|----------|
| Instant | Filters as you type (debounce 300ms) |
| Submit | Search on Enter or button click |
| Combobox | Shows autocomplete suggestions |

**States:** Empty, Typing (show clear button), Has value, Loading (spinner replaces search icon), No results

**Responsive:**
- Desktop: full width in toolbar or constrained by container
- Mobile: collapses to icon button → expands to full-width overlay on tap

**Accessibility:**
- `role="search"` on container or use `<search>` element
- `type="search"` provides native clear on some browsers
- Clear button: `aria-label="Clear search"`
- With suggestions: use Combobox ARIA pattern (`aria-patterns.md` → Combobox)
- Announce result count: live region "5 results found"

---

## 3. Card

A contained surface grouping related content and actions.

**Composition:** `[image?]` + `[header: Badge? + Title + Subtitle?]` + `[body]` + `[footer: Button*]`

**Anatomy:**
```
┌──────────────────────────────┐
│         [Image/Media]         │  ← Optional media slot
├──────────────────────────────┤
│ [Badge]                       │
│ Card Title            [Menu]  │  ← heading-5 + optional overflow menu
│ Supporting text               │  ← body-sm, text-secondary
├──────────────────────────────┤
│ Body content area             │  ← body-base
├──────────────────────────────┤
│           [Cancel] [Action]   │  ← Footer with actions
└──────────────────────────────┘
```

**Variants:**
| Variant | Border | Shadow | Use Case |
|---------|--------|--------|----------|
| Outlined | `border.default` | none | Lists, dense layouts |
| Elevated | none | `shadow.sm` | Standalone, dashboards |
| Interactive | `border.default` | `shadow.sm` on hover | Clickable cards |
| Filled | none | none | On sunken backgrounds |

**Token Mapping:** `component.card.*`, `spacing.card.*`, `radius.card`

**Layout:**
- Padding: `spacing.card.padding` (24px), compact: `spacing.card.padding-sm` (16px)
- Internal gap: `spacing.card.gap` (16px)
- Image: full-bleed (no padding) with `border-radius` top corners only

**Responsive:**
- Grid: 3 columns (desktop) → 2 (tablet) → 1 (mobile)
- Card min-width: 280px; max-width: none (fills grid cell)

**Accessibility:**
- Interactive card: wrap in `<a>` or `<button>` with full-card click area
- If card has multiple links, use "stretched link" pattern with one primary `<a>`
- Card title should be a heading level appropriate to page hierarchy
- Image: meaningful `alt` text or `alt=""` if title duplicates content

---

## 4. Navigation Item

A single item within a navigation structure (sidebar, tabs, breadcrumbs).

**Composition:** `Icon` + `Label` + `Badge?` + `Chevron?` (for nested)

**Anatomy:**
```
┌──────────────────────────────┐
│ [icon]  Label          [3]  │  ← Icon + text + count badge
└──────────────────────────────┘
```

**Variants:**
| Variant | Context |
|---------|---------|
| Sidebar item | Vertical navigation with icon + text |
| Tab item | Horizontal tab bar |
| Breadcrumb item | Path trail with separator |
| Bottom nav item | Mobile bottom bar (icon + label stacked) |

**States:** Default, Hover, Active/Current, Focus, Disabled

**Active Indicator:**
- Sidebar: left border 3px `action.primary` + `interactive.selected-bg`
- Tab: bottom border 2px `action.primary`
- Breadcrumb: bold text, no link

**Token Mapping:**
- Text: `text.secondary` (default), `text.primary` (active)
- Background: transparent (default), `interactive.selected-bg` (active)
- Icon: matches text color

**Accessibility:**
- Use `<nav aria-label="Main navigation">` wrapper
- Current item: `aria-current="page"` (sidebar/breadcrumb) or `aria-selected="true"` (tabs)
- Collapsible groups: `aria-expanded` on parent
- Badge count: `aria-label="Messages, 3 unread"` on the nav item

---

## 5. Alert

A contextual message communicating feedback, status, or important information.

**Composition:** `Icon` + `[Title?]` + `Message` + `[Action?]` + `[Close?]`

**Anatomy:**
```
┌──────────────────────────────────────┐
│ ⓘ  Alert Title                  [×] │
│    Alert message with details.       │
│    [Action Link]                     │
└──────────────────────────────────────┘
```

**Variants:**
| Variant | Icon | Colors | Use Case |
|---------|------|--------|----------|
| Info | ⓘ | `feedback.info-*` | Tips, general information |
| Success | ✓ | `feedback.success-*` | Completed actions |
| Warning | ⚠ | `feedback.warning-*` | Potential issues |
| Error | ✕ | `feedback.error-*` | Failures, blocking issues |

**Layout:**
- Full-width within container (not page-width)
- Padding: `spacing.card.padding-sm` (16px)
- Icon-text gap: `spacing.inline.md` (12px)
- Border-left: `border.width.thick` (4px) in variant color
- Radius: `radius.card` (8px)

**Behavior:**
- Inline alerts: persistent, dismissed by user or resolved condition
- Toast alerts: auto-dismiss after 5s (non-critical) or persistent (errors)
- Stacking: newest on top, max 3 visible

**Accessibility:**
- Use `role="alert"` for dynamic errors/warnings (assertive)
- Use `role="status"` for success messages (polite)
- Inline informational alerts: no ARIA role needed (static content)
- Dismiss button: `aria-label="Dismiss alert"`
- Icon: `aria-hidden="true"` (role is communicated by color/text, not icon alone)
- Never rely solely on color — icon + text + border all convey the variant

---

## 6. Dropdown

A button-triggered floating panel with a list of options or actions.

**Composition:** `Button` (trigger) + floating `[list of items]`

**Anatomy:**
```
[Trigger Button ▾]
┌────────────────────────┐
│ [icon] Option One      │  ← menu item
│ [icon] Option Two      │
│────────────────────────│  ← separator
│ [icon] Danger Action   │  ← destructive variant
└────────────────────────┘
```

**Variants:**
| Variant | Trigger | Content |
|---------|---------|---------|
| Menu | Button with chevron | Action items (`role="menu"`) |
| Select | Input-like trigger | Option selection (`role="listbox"`) |
| Multi-select | Input-like trigger | Checkable options |
| Context menu | Right-click | Action items |

**Layout:**
- Min-width: trigger width; Max-width: 320px
- Item height: 36px (md), 32px (sm)
- Padding: `spacing.component.input-padding-*`
- Shadow: `shadow.lg`
- Radius: `radius.dropdown` (8px)
- Placement: bottom-start (default), auto-flip when near viewport edges

**States (items):** Default, Hover, Focus, Active, Selected (checkmark), Disabled (gray, skip in keyboard nav)

**Behavior:**
- Opens on click (not hover)
- Closes on: selection, Escape, click outside
- Scroll: max-height with overflow if > 8 items visible
- Type-ahead: typing characters focuses matching item

**Accessibility:**
- Menu variant: see `aria-patterns.md` → Menu
- Select variant: see `aria-patterns.md` → Listbox
- Trigger: `aria-haspopup="true"`, `aria-expanded="true/false"`
- Items: `role="menuitem"` or `role="option"`
- Keyboard: Down/Up arrows navigate, Enter selects, Escape closes

---

## 7. Pagination

A navigation control for traversing paged content.

**Composition:** `Button` (prev) + `Button[]` (page numbers) + `Button` (next) + optional `[page-size selector]` + optional `[result count]`

**Anatomy:**
```
┌──────────────────────────────────────────────────┐
│ Showing 1–10 of 247    [<] [1] [2] [3] ... [25] [>] │
└──────────────────────────────────────────────────┘
```

**Variants:**
| Variant | Behavior | Use Case |
|---------|----------|----------|
| Numbered | Page buttons with prev/next arrows | Data tables, search results |
| Load more | Single button appends next batch | Feeds, card grids |
| Infinite scroll | Auto-loads on scroll near bottom | Social feeds, infinite lists |
| Simple | Prev/Next only (no page numbers) | Mobile, narrow containers |
| Compact | Current page input + total ("Page [3] of 25") | Tight toolbar spaces |

**Sizes:**
| Size | Button height | Font | Use Case |
|------|---------------|------|----------|
| sm | 32px | body-sm (14px) | Inline, compact tables |
| md | 40px | body-base (16px) | Default |

**States:**
1. **Default** — resting, all controls active
2. **Hover** — background shift on page buttons
3. **Active/Current** — current page highlighted: `action.primary` bg, white text
4. **Focus** — `focus-ring` shadow token on focused button
5. **Disabled** — prev disabled on page 1, next disabled on last page; `opacity: 0.5`, `pointer-events: none`
6. **Loading** — spinner replaces content area during fetch (load-more/infinite scroll)

**Token Mapping:**
- Page button: `component.pagination.button-*`
- Active page: `action.primary` bg, `text.on-primary` text
- Ellipsis: `semantic.text.tertiary`, non-interactive
- Gap between buttons: `spacing.inline.xs` (4px)
- Nav container: `<nav aria-label="Pagination">`

**Behavior:**
- Numbered: show max 7 page buttons; collapse middle with ellipsis ("1 2 ... 12 13 14 ... 25")
- Load more: button shows "Load more" or "Show N more"; replaces with spinner on click
- Infinite scroll: trigger fetch when scroll position is within 200px of bottom; debounce 300ms; show spinner at bottom during load

**Accessibility:**
- Wrap in `<nav aria-label="Pagination">`
- Current page: `aria-current="page"` on the active page button
- Prev/Next: `aria-label="Go to previous page"` / `aria-label="Go to next page"`
- Page buttons: `aria-label="Go to page 3"`
- Disabled prev/next: `aria-disabled="true"`
- Ellipsis: `aria-hidden="true"` (decorative)
- Infinite scroll: announce new content via `aria-live="polite"` region ("Loaded 10 more items")
- Keyboard: Tab to navigate between buttons; Enter/Space activates

---

## 8. Breadcrumb

A navigation trail showing the user's position within a site hierarchy.

**Composition:** `Link[]` (ancestor pages) + `Text` (current page) + `Icon[]` (separators)

**Anatomy:**
```
┌──────────────────────────────────────────────┐
│ Home  /  Products  /  Electronics  /  Phones │
│  ↑         ↑             ↑            ↑      │
│ link     link          link      current (text)│
└──────────────────────────────────────────────┘
```

**Variants:**
| Variant | Behavior | Use Case |
|---------|----------|----------|
| Default | All items visible | Short hierarchies (3–4 levels) |
| Collapsed | Middle items collapse to "..." menu | Deep hierarchies (5+ levels) |
| Mobile | Show only last 2 items (parent + current) | Mobile viewports |

**Sizes:**
| Size | Height | Font | Separator icon |
|------|--------|------|----------------|
| sm | 24px | body-sm (14px) | 12px |
| md | 32px | body-base (16px) | 16px |

**States:**
1. **Default** — links in `semantic.text.link` color
2. **Hover** — underline on hovered link
3. **Focus** — `focus-ring` on focused link
4. **Current** — last item is plain text (`semantic.text.primary`), not a link
5. **Overflow** — middle items hidden behind ellipsis dropdown

**Token Mapping:**
- Separator: `semantic.text.tertiary` color; default icon: `/` character or chevron-right icon (16px)
- Separator token: `component.breadcrumb.separator-color`
- Link: `semantic.text.link` (default), `semantic.text.link-hover` (hover)
- Current item: `semantic.text.primary`, `font-weight: 500`
- Item gap: `spacing.inline.xs` (4px) on each side of separator
- Container padding: none (breadcrumb is inline within its parent)

**Overflow Handling:**
- When items exceed container width or depth > 4: collapse middle items into an ellipsis button
- Ellipsis button opens a dropdown menu listing collapsed items
- Always show: first item (root) + last 2 items (parent + current)
- Example: `Home / ... / Electronics / Phones` (dropdown reveals "Products", "Categories")

**Responsive:**
- Desktop (>= md): full breadcrumb visible or collapsed variant
- Mobile (< md): show only last 2 items — `[< Parent]  Current Page` or `Parent / Current`

**Accessibility:**
- Wrap in `<nav aria-label="Breadcrumb">`
- Use `<ol>` for ordered list of links
- Current page: `aria-current="page"` on the last item
- Separator: `aria-hidden="true"` (decorative, already conveyed by list semantics)
- Collapsed ellipsis: `aria-label="Show hidden breadcrumb items"`, `aria-expanded="true/false"`, `aria-haspopup="true"`
- Keyboard: Tab navigates links; Enter activates; Escape closes ellipsis dropdown

---

## 9. Tabs

A set of layered content sections, only one visible at a time, with a tab list for switching.

**Composition:** `Button[]` (tab triggers) + `[panels]` (content areas) + optional `Badge` + optional `Icon`

**Anatomy:**
```
┌──────────────────────────────────────────────┐
│ [Tab 1]  [Tab 2]  [Tab 3 (5)]   [→]        │  ← Tab list (horizontal)
├──────────────────────────────────────────────┤
│                                              │
│  Tab panel content                           │  ← Active panel
│                                              │
└──────────────────────────────────────────────┘
```

**Variants:**
| Variant | Layout | Use Case |
|---------|--------|----------|
| Horizontal | Tabs in a row above content | Default, most common |
| Vertical | Tabs in a column left of content | Settings, documentation sidebars |
| Underline | Active tab indicated by bottom border | Clean, minimal style |
| Contained | Active tab has filled background | Prominent section switching |
| Pill | Active tab has rounded pill background | Compact, toggle-like |

**Sizes:**
| Size | Tab height | Font | Icon |
|------|-----------|------|------|
| sm | 32px | body-sm (14px) | 16px |
| md | 40px | body-base (16px) | 20px |
| lg | 48px | body-lg (18px) | 24px |

**States:**
1. **Default** — inactive tab: `semantic.text.secondary`, no indicator
2. **Hover** — `interactive.hover-bg` background shift
3. **Active/Selected** — `semantic.text.primary` + active indicator (underline/fill/pill)
4. **Focus** — `focus-ring` shadow token
5. **Disabled** — `opacity: 0.5`, `cursor: not-allowed`, skip in keyboard nav
6. **With badge** — `Badge` atom positioned to the right of the tab label

**Token Mapping:**
- Active indicator (underline): `border.width.thick` (3px), `action.primary` color
- Active indicator (contained): `action.primary` bg, `text.on-primary` text
- Active indicator (pill): `interactive.selected-bg`, `radius.pill` (full)
- Tab gap: `spacing.inline.xs` (4px)
- Tab padding: `spacing.inline.md` (12px) horizontal, `spacing.stack.xs` (4px) vertical
- Panel padding: `spacing.stack.lg` (16px) top

**Overflow Handling:**
- When tabs exceed container width: enable horizontal scroll with fade gradient on edges
- Show scroll arrow buttons on overflow edges (left/right or up/down for vertical)
- Arrow buttons: `component.tabs.scroll-button-*`, 32px square, ghost style
- Never wrap tabs to a second row — always scroll

**Behavior:**
- Tab activation: immediate on click/Enter/Space (automatic activation)
- Tab switching does not trigger page navigation — content swaps in place
- Optional: lazy-load panel content on first activation

**Responsive:**
- Desktop: all tabs visible or horizontal scroll
- Tablet: horizontal scroll with arrows if needed
- Mobile: horizontal scroll (swipeable) or collapse to dropdown selector

**Accessibility:**
- Tab list: `role="tablist"`, `aria-label="[descriptive name]"` or `aria-labelledby`
- Each tab: `role="tab"`, `aria-selected="true/false"`, `aria-controls="panel-id"`
- Each panel: `role="tabpanel"`, `aria-labelledby="tab-id"`, `tabindex="0"` (to make panel focusable)
- Keyboard (horizontal): Left/Right arrow moves focus between tabs; Home/End jump to first/last tab; Enter/Space activates focused tab
- Keyboard (vertical): Up/Down arrow replaces Left/Right
- Disabled tabs: `aria-disabled="true"`, skip in arrow key navigation
- Badge content: include count in tab's `aria-label` (e.g., `aria-label="Messages, 5 unread"`)
- See `aria-patterns.md` → Tabs
