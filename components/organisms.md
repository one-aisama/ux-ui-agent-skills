# Organisms

Complex, multi-molecule components that form distinct sections of the interface. Each organism manages its own state and contains multiple interactive elements.

---

## 1. Header (App Bar)

The persistent top-level navigation bar.

**Composition:** `Logo` + `Navigation Items` + `Search Bar` + `Avatar/Actions`

**Grid Structure:**
```
┌─────────────────────────────────────────────────────────┐
│ [Logo]   [Nav Item] [Nav Item] [Nav Item]   🔍 [Avatar]│
│  ↑                   ↑                        ↑        │
│  Fixed width         Flex grow (center)        Auto     │
└─────────────────────────────────────────────────────────┘
```

**Dimensions:**
- Height: 64px (desktop), 56px (mobile)
- Horizontal padding: `spacing.page.inline-padding` / `page.inline-padding-lg`
- z-index: `z-index.sticky` (20) or `z-index.fixed` (30)

**Content Slots:**
| Slot | Content | Alignment |
|------|---------|-----------|
| Leading | Logo / hamburger menu (mobile) | Left |
| Center | Primary navigation / breadcrumbs | Center or left |
| Trailing | Search, notifications, avatar, CTA | Right |

**Variants:**
| Variant | Behavior |
|---------|----------|
| Static | Scrolls with page |
| Sticky | Sticks to top on scroll (`position: sticky`) |
| Transparent | Overlays hero; becomes opaque on scroll |
| Compact | Reduces height on scroll (64→48px) |

**Responsive:**
- **Desktop (≥ lg):** Full horizontal nav
- **Tablet (md–lg):** Collapsed nav → hamburger menu or priority+ pattern
- **Mobile (< md):** Logo + hamburger + essential actions only

**Animation:**
- Scroll transition: opacity/height change over 200ms `ease-out`
- Mobile menu: slide from left, 250ms `ease-in-out`

**Accessibility:**
- `<header role="banner">` — once per page
- `<nav aria-label="Main navigation">` for primary nav
- Skip link: first focusable element is "Skip to main content"
- Mobile menu: `aria-expanded`, `aria-controls`, focus trap when open

---

## 2. Sidebar

A vertical navigation panel with hierarchical navigation and collapsibility.

**Grid Structure:**
```
┌──────────────────┐
│ [Logo / Collapse] │  ← Fixed header
├──────────────────┤
│ [Search]          │
├──────────────────┤
│ [Nav Group Label] │
│  [Nav Item]       │  ← Scrollable
│  [Nav Item]       │    navigation
│  [Nav Item ▸]     │    area
│   └ [Sub Item]    │
│   └ [Sub Item]    │
├──────────────────┤
│ [User / Settings] │  ← Fixed footer
└──────────────────┘
```

**Dimensions:**
- Width: `sidebar.default` (256px), collapsed: `sidebar.collapsed` (64px)
- z-index: `z-index.fixed` (30) on mobile overlay

**State Management:**
| State | Width | Content | Behavior |
|-------|-------|---------|----------|
| Expanded | 256px | Icons + labels | Default on desktop |
| Collapsed | 64px | Icons only + tooltips | User preference |
| Hidden | 0px | — | Mobile default |
| Overlay | 256px | Full, over content | Mobile when open |

**Collapse Animation:** Width transition 200ms `ease-in-out`. Text fades out at 150ms, icons remain.

**Responsive:**
- **Desktop (≥ lg):** Persistent sidebar, push content
- **Tablet (md–lg):** Collapsed by default, expand on hover/click
- **Mobile (< md):** Hidden; hamburger triggers overlay with backdrop

**Accessibility:**
- `<aside aria-label="Sidebar navigation">`
- `<nav aria-label="Main navigation">` inside
- Collapse button: `aria-expanded="true/false"`, `aria-label="Collapse sidebar"`
- Collapsed: tooltips on hover/focus for icon-only items
- Overlay (mobile): focus trap, Escape to close, backdrop `aria-hidden="true"`

---

## 3. Form

A complete data entry container managing multiple Form Fields with validation.

**Grid Structure:**
```
┌──────────────────────────────────────┐
│ Form Title                           │
│ Description text                     │
├──────────────────────────────────────┤
│ Section Label                        │
│ ┌────────────┐ ┌────────────┐       │
│ │ First Name │ │ Last Name  │       │  ← 2-column row
│ └────────────┘ └────────────┘       │
│ ┌───────────────────────────┐       │
│ │ Email                     │       │  ← Full-width row
│ └───────────────────────────┘       │
│ ┌───────────────────────────┐       │
│ │ Message (textarea)        │       │  ← Full-width row
│ └───────────────────────────┘       │
├──────────────────────────────────────┤
│              [Cancel]  [Submit]      │  ← Action bar
└──────────────────────────────────────┘
```

**Layout:**
- Field gap: `spacing.stack.lg` (16px)
- Section gap: `spacing.stack.2xl` (32px)
- Column gap: `spacing.stack.lg` (16px)
- Grid: 2 columns (desktop) → 1 column (mobile)
- Action bar: right-aligned, primary action last (rightmost)

**State Management:**
| Form State | Behavior |
|------------|----------|
| Empty | All fields default, submit disabled/enabled |
| Dirty | Unsaved changes indicator, confirm on navigate away |
| Validating | Inline validation on blur (not on keystroke) |
| Submitting | Submit button loading, all fields disabled |
| Success | Success alert, optional redirect |
| Error | Scroll to first error, focus first invalid field, error summary |

**Validation Strategy:**
1. **On blur:** Validate individual field when focus leaves
2. **On submit:** Validate all fields, show all errors
3. **On fix:** Revalidate changed field immediately (not on blur again)
4. Error message appears below field, replaces help text

**Accessibility:**
- `<form>` with `aria-label` or `aria-labelledby` pointing to form title
- Group related fields in `<fieldset>` with `<legend>`
- Error summary: `role="alert"` at top listing all errors with links to fields
- Submit: `aria-disabled` while loading, announce "Submitting..." via live region
- Unsaved changes: `beforeunload` confirmation + visual indicator

---

## 4. Data Table

A structured data display with sorting, filtering, pagination, selection, and row actions. The most complex organism — interactions must be precisely specified.

**Grid Structure:**
```
┌──────────────────────────────────────────────────────┐
│ [Title]         [Filter ▼] [Search...]    [+ Add]   │  ← Toolbar
├──┬─────────────┬──────────┬────────┬────┬───────────┤
│☐ │ Name ↕      │ Status ↕ │ Date ↕ │ $  │ Actions   │  ← Header row
├──┼─────────────┼──────────┼────────┼────┼───────────┤
│☐ │ Item Alpha  │ ● Active │ Jan 1  │ 42 │ ⋯         │  ← Data rows
│☑ │ Item Beta   │ ● Error  │ Jan 2  │ 17 │ ⋯         │  ← Selected
│☐ │ Item Gamma  │ ○ Draft  │ Jan 3  │ 89 │ ⋯         │
├──┴─────────────┴──────────┴────────┴────┴───────────┤
│ Showing 1-10 of 247      [10 ▼]   [< 1 2 3 ... >]  │  ← Footer
└──────────────────────────────────────────────────────┘
```

**Content Slots:**
| Slot | Content | Behavior |
|------|---------|----------|
| Toolbar | Title, filter dropdowns, global search, bulk actions (visible when rows selected), add button | Bulk actions replace title area when selection > 0 |
| Header | Column labels, sort indicators (↕ ↑ ↓), select-all checkbox, resize handles | Sticky on scroll (`position: sticky; top: 0`) |
| Body | Data rows, inline editing cells, expandable detail rows | Scrollable, min-height: 200px |
| Footer | "Showing X-Y of Z", items-per-page dropdown (10/25/50/100), pagination controls | Sticky on scroll (`position: sticky; bottom: 0`) |

### Sorting

| Interaction | Behavior |
|-------------|----------|
| Click column header | Cycle: none → ascending → descending → none |
| Shift + Click header | Add secondary sort (multi-column). Badge shows sort order (1, 2, 3) |
| Keyboard: Enter/Space on header | Same as click |
| Sort + active filter | Filter persists. Sort applies within filtered results |
| Sort + selection | Selection persists across sort. Selected rows stay selected |

**Visual indicators:**
- Unsorted: `↕` (neutral, `text.tertiary`)
- Ascending: `↑` (active, `text.primary`)
- Descending: `↓` (active, `text.primary`)
- Multi-sort badge: small numeral next to arrow (`1↑`, `2↓`)

### Filtering

| Type | Behavior |
|------|----------|
| Global search | Filters across all text columns. Debounce 300ms. Clears on `Escape` |
| Column filter | Dropdown per column (text: contains/starts with/equals, number: >/</=, date: range, enum: multi-select checkboxes) |
| Active filter indicator | Chip bar below toolbar showing active filters. Each chip has `×` to remove |
| Filter + sort | Filters apply first, then sort within filtered results |
| Filter + pagination | Reset to page 1 when filter changes |
| No results | Show empty state (see below), not an empty table |

**Announce filter results:** `aria-live="polite"` region: "Showing 3 of 247 results for 'search term'"

### Selection

| Interaction | Behavior |
|-------------|----------|
| Click row checkbox | Toggle single row selection |
| Click select-all checkbox | Select all rows on **current page** (not all pages) |
| Shift + Click checkbox | Range select: all rows between last selected and current |
| Ctrl/Cmd + Click checkbox | Toggle individual without affecting others (default behavior) |
| Select all + filter active | Select all **visible** (filtered) rows on current page |

**Bulk actions toolbar** (appears when selection > 0):
```
┌──────────────────────────────────────────────────────┐
│ ☑ 3 selected   [Export] [Delete] [Assign ▼]  [×]   │
└──────────────────────────────────────────────────────┘
```
- Shows count of selected items
- `×` clears selection
- Destructive actions (Delete) require confirmation dialog

### Row Actions

- Overflow menu `⋯` at row end — opens dropdown with: View, Edit, Duplicate, Delete
- Primary action (e.g., View) also available via row click (entire row is clickable)
- Keyboard: `Enter` on focused row opens primary action
- Delete in row menu: confirmation dialog, not immediate

### Inline Editing

- Double-click cell to enter edit mode (or `Enter` on focused cell)
- `Tab` moves to next editable cell. `Shift+Tab` moves back
- `Escape` cancels edit, restores previous value
- `Enter` confirms edit
- Validation errors shown as red border + tooltip below cell
- Only one cell editable at a time

### Expandable Rows

- Chevron `>` at row start to expand detail panel
- Expanded panel spans full row width below the row
- Only one row expanded at a time (accordion behavior) — or configurable to allow multiple
- `Enter`/`Space` on chevron toggles expansion
- `aria-expanded="true/false"` on trigger, `aria-controls` pointing to detail panel

### Column Resizing

- Drag handle between column headers
- Min column width: 80px
- Double-click handle: auto-fit to content width
- Cursor: `col-resize` on hover
- Resize persists in localStorage (per-table key)

### Pagination

- Items per page: dropdown with options `[10, 25, 50, 100]`
- Page numbers: show first, last, current ± 1, ellipsis for gaps: `[1] ... [4] [5] [6] ... [24]`
- Previous/Next arrow buttons (disabled at boundaries)
- Keyboard: arrow buttons are focusable, `Enter`/`Space` to navigate
- Changing items-per-page resets to page 1
- `<nav aria-label="Table pagination">` wrapping pagination controls

### Dimensions

| Element | Value | Token |
|---------|-------|-------|
| Row height | 48px (default), 36px (compact), 64px (comfortable) | `spacing.component.table-row-height-*` |
| Header height | 44px | `spacing.component.table-header-height` |
| Cell padding horizontal | 12px | `spacing.component.table-cell-padding-x` |
| Cell padding vertical | 8px | `spacing.component.table-cell-padding-y` |
| Toolbar height | 56px | — |
| Footer height | 48px | — |

### States

| State | Visual | Content |
|-------|--------|---------|
| **Default** | Full table with data | — |
| **Loading (initial)** | Skeleton rows: 5 rows × all columns. Row height matches data row. Shimmer animation | `aria-busy="true"` on `<table>` |
| **Loading (pagination)** | Subtle opacity (0.6) on body + spinner overlay centered on body area. Header/footer remain interactive | `aria-busy="true"` on `<tbody>` |
| **Empty (no data)** | Hide table body. Show: illustration (120×120px) + heading ("No items yet") + body text + primary CTA button. Centered in body area | — |
| **Empty (filtered)** | Hide table body. Show: illustration + "No results for '[query]'" + "Try adjusting your filters" + [Clear filters] button | — |
| **Error** | Hide table body. Show: error icon + "Failed to load data" + "Check your connection and try again" + [Retry] button | `role="alert"` |
| **Selected row** | Background: `interactive.selected-bg`. Checkbox checked | — |
| **Hover row** | Background: `surface.hover` | — |

### Responsive Behavior

| Breakpoint | Layout | Details |
|------------|--------|---------|
| **Desktop (≥ lg)** | Full table | All columns visible, horizontal resize if needed |
| **Tablet (md–lg)** | Horizontal scroll | Sticky first column + checkbox column. Shadow on scroll edge to indicate more content. `tabindex="0"` on scroll container for keyboard scroll |
| **Mobile (< md)** | Card layout | Each row becomes a card. Column headers become labels inside card. Selection checkbox at card top-right. Actions at card bottom. Stack cards vertically with `spacing.stack.md` gap |

**Card layout anatomy (mobile):**
```
┌────────────────────────┐
│ [☐]                    │
│ Name: Item Alpha       │
│ Status: ● Active       │
│ Date: Jan 1, 2025      │
│ Amount: $42            │
│ [View] [Edit] [⋯]     │
└────────────────────────┘
```

### Accessibility

**Semantic structure:**
```html
<div role="region" aria-label="Users table" tabindex="0">
  <table>
    <thead>
      <tr>
        <th scope="col"><input type="checkbox" aria-label="Select all rows" /></th>
        <th scope="col"><button aria-sort="ascending">Name</button></th>
        ...
      </tr>
    </thead>
    <tbody>
      <tr aria-selected="true">
        <td><input type="checkbox" aria-label="Select Item Alpha" checked /></td>
        <td>Item Alpha</td>
        ...
      </tr>
    </tbody>
  </table>
</div>
```

**Keyboard model:**
| Key | Action |
|-----|--------|
| `Tab` | Move between major sections: toolbar → header → body → pagination |
| `Arrow Up/Down` | Move between rows in body |
| `Arrow Left/Right` | Move between cells in a row |
| `Enter` | Primary action on row (view), or activate sort on header |
| `Space` | Toggle checkbox on focused row/header |
| `Escape` | Close inline edit, clear search, deselect all |
| `Home/End` | Jump to first/last row |

**Live regions:**
- Sort change: announce "Sorted by Name, ascending" via `aria-live="polite"`
- Filter change: announce "Showing 3 of 247 results" via `aria-live="polite"`
- Selection change: announce "3 rows selected" via `aria-live="polite"`
- Page change: announce "Page 2 of 24" via `aria-live="polite"`

**Cross-references:** See [ARIA Patterns](../accessibility/aria-patterns.md) for grid/table pattern. See [React + Tailwind](../frameworks/react-tailwind.md) for implementation.

---

## 5. Modal (Dialog)

A focused overlay that interrupts workflow for critical information or action.

**Grid Structure:**
```
┌──────────── Overlay ─────────────────┐
│                                       │
│   ┌────────────────────────────┐     │
│   │ [Icon] Title           [×] │     │  ← Header
│   ├────────────────────────────┤     │
│   │                            │     │
│   │ Content area               │     │  ← Body (scrollable)
│   │                            │     │
│   ├────────────────────────────┤     │
│   │       [Secondary] [Primary]│     │  ← Footer
│   └────────────────────────────┘     │
│                                       │
└───────────────────────────────────────┘
```

**Dimensions:**
| Size | Width | Use Case |
|------|-------|----------|
| sm | 400px | Confirmations, simple forms |
| md | 560px | Complex forms, details |
| lg | 720px | Multi-step, rich content |
| xl | 960px | Data-heavy, split views |
| full | 100% - 48px | Mobile |

**Responsive behavior:**
| Breakpoint | Layout |
|------------|--------|
| Desktop (≥ lg) | Centered modal at specified size, max-height: 85vh, scrollable body |
| Tablet (md–lg) | Same as desktop but max-width: calc(100% - 48px) |
| Mobile (< md) | Full-screen: width 100%, height 100%, no border-radius, no outer margin. Header sticky, body scrollable, footer sticky. Close button always visible in header |

**Scrolling:** Body scrolls independently. Header and footer remain fixed. When body is scrollable, show a subtle border-bottom on header and border-top on footer to indicate scroll position.

**Tokens:**
- Background: `surface.card`, Overlay: `surface.overlay`
- Radius: `radius.modal` (12px)
- Shadow: `shadow.xl`
- z-index: `z-index.modal` (50)
- Padding: `spacing.component.modal-padding` (24px)

**Animation:**
- Open: overlay fade-in 150ms + modal scale(0.95→1) + fade-in 150ms
- Close: reverse at 100ms
- Respect `prefers-reduced-motion`: skip scale, use fade only

**Focus Trap:**
1. On open: focus first focusable element in modal
2. Tab cycles only within modal
3. Escape closes modal
4. On close: return focus to trigger element

**Accessibility:**
- `role="dialog"`, `aria-modal="true"`
- `aria-labelledby` → title, `aria-describedby` → description
- Set `inert` attribute on content behind modal (or `aria-hidden="true"`)
- See `aria-patterns.md` → Dialog

---

## 6. Drawer

A panel that slides in from the edge for secondary content or navigation.

**Grid Structure:**
```
┌────────────────────────────────────────┐
│                            ┌──────────┐│
│         Main Content       │ [×]      ││  ← Drawer header
│         (dimmed)           ├──────────┤│
│                            │ Content  ││  ← Scrollable body
│                            │          ││
│                            ├──────────┤│
│                            │ [Actions]││  ← Optional footer
│                            └──────────┘│
└────────────────────────────────────────┘
```

**Variants:**
| Position | Width/Height | Use Case |
|----------|-------------|----------|
| Right | 320-480px | Detail panels, edit forms |
| Left | 256-320px | Navigation (mobile) |
| Bottom | 40-90vh | Mobile actions sheets |

**Tokens:**
- Same surface/shadow as Modal
- z-index: `z-index.overlay` (40) or `z-index.modal` (50) with backdrop

**Animation:**
- Slide from edge: `translateX(100%)` → `translateX(0)` over 250ms `ease-out`
- Backdrop: fade-in 150ms
- Respect `prefers-reduced-motion`

**Behavior:**
- With backdrop: blocks main content (like a modal)
- Without backdrop: inline, pushes content (persistent drawer)
- Swipe-to-close on touch devices (bottom/left/right drawers)

**Focus Management:**
- With backdrop: focus trap (same as Modal)
- Without backdrop: no focus trap, focus moves naturally

**Accessibility:**
- Same as Modal: `role="dialog"`, `aria-modal="true"` (if blocking), `aria-label`
- Escape to close
- Return focus to trigger on close

---

## 7. Command Palette

A keyboard-driven search overlay for quick access to actions, navigation, and recent items.

**Composition:** `Input` (search) + `[section headers]` + `[result items]` + `[keyboard shortcut hints]`

**Grid Structure:**
```
┌──────────── Overlay ───────────────────────┐
│                                             │
│   ┌───────────────────────────────────┐    │
│   │ 🔍 Type a command...              │    │  ← Search input
│   ├───────────────────────────────────┤    │
│   │ Recent                            │    │  ← Section header
│   │  [icon] Open Settings      ⌘ ,   │    │  ← Result item + shortcut
│   │  [icon] Toggle Dark Mode   ⌘ D   │    │
│   ├───────────────────────────────────┤    │
│   │ Navigation                        │    │  ← Section header
│   │  [icon] Dashboard                 │    │
│   │  [icon] Projects                  │    │
│   │  [icon] Settings                  │    │
│   ├───────────────────────────────────┤    │
│   │ Actions                           │    │  ← Section header
│   │  [icon] Create new project        │    │
│   │  [icon] Invite team member        │    │
│   └───────────────────────────────────┘    │
│                                             │
└─────────────────────────────────────────────┘
```

**Dimensions:**
| Property | Value |
|----------|-------|
| Width | 640px (max), centered horizontally |
| Max height | 480px (scrollable results area) |
| Top offset | 20% from viewport top |
| Radius | `radius.modal` (12px) |
| Shadow | `shadow.xl` |
| z-index | `z-index.modal` (50) |

**Content Slots:**
| Slot | Content |
|------|---------|
| Search input | Text input with search icon, placeholder, clear button |
| Section headers | Category labels (Recent, Navigation, Actions, etc.) |
| Result items | Icon + label + optional description + optional keyboard shortcut |
| Footer | Hint bar: "↑↓ Navigate  ↵ Select  Esc Close" |

**Tokens:**
- Overlay backdrop: `surface.overlay`
- Container bg: `surface.card`
- Search input: `component.input.*` tokens, lg size
- Section header: `textStyle.overline`, `semantic.text.tertiary`
- Result item bg (default): transparent
- Result item bg (highlighted): `interactive.hover-bg`
- Result item text: `semantic.text.primary` (label), `semantic.text.secondary` (description)
- Shortcut badge: `textStyle.caption`, `surface.sunken` bg, `radius.sm`, `spacing.inline.xs` padding
- Separator: `border.default`

**Features:**
- **Fuzzy search:** Match against item labels and descriptions; highlight matching characters in results
- **Sections:** Group results by type (Recent, Navigation, Actions, Files, etc.); sections auto-hide when no matches
- **Recent items:** Show last 5 used commands when input is empty
- **Keyboard shortcuts:** Display shortcut hint (e.g., "Ctrl+K") next to items that have global bindings
- **Empty state:** "No results found" with suggestion to try different keywords

**States:**
1. **Closed** — not visible, activated by keyboard shortcut (Ctrl+K / Cmd+K)
2. **Open (empty)** — shows recent items and all sections
3. **Searching** — filtered results update as user types (debounce 100ms)
4. **Highlighted** — one result item has visual focus (keyboard or mouse)
5. **Loading** — spinner in search input for async results (optional)
6. **No results** — empty state message

**Behavior:**
- Open: Ctrl+K / Cmd+K (global shortcut); focus immediately lands in search input
- Close: Escape, click outside, or select a result
- Navigation: Up/Down arrows move highlight through results (wraps at boundaries)
- Selection: Enter activates highlighted item; item executes its action (navigate, toggle, etc.)
- Mouse: hover highlights item; click activates

**Animation:**
- Open: overlay fade-in 100ms + palette scale(0.98→1) + fade-in 100ms
- Close: fade-out 75ms
- Respect `prefers-reduced-motion`: fade only

**Accessibility:**
- Container: `role="dialog"`, `aria-modal="true"`, `aria-label="Command palette"`
- Search input: `role="combobox"`, `aria-expanded="true"`, `aria-controls="results-list"`, `aria-autocomplete="list"`
- Results list: `role="listbox"`, `id="results-list"`
- Result items: `role="option"`, `aria-selected="true/false"`
- Section headers: `role="presentation"` (not selectable) or use `role="group"` with `aria-label`
- Highlighted item: `aria-activedescendant` on the combobox points to the highlighted option
- Keyboard shortcut text: `aria-hidden="true"` (supplementary, not essential)
- Focus trap: Tab cycles within palette (input → results → footer hints are not focusable)
- On close: return focus to previously focused element
- Announce result count: live region "5 results" / "No results found"
- See `aria-patterns.md` → Combobox

---

## 8. Toast / Notification

A transient, non-modal message providing feedback about an action or system event.

**Composition:** `Icon` + `[Title?]` + `Message` + `[Action?]` + `[Close?]` + `[Progress bar?]`

**Grid Structure:**
```
┌─── Toast Stack (top-right) ──────────┐
│ ┌──────────────────────────────────┐ │
│ │ ✓  Item saved        [Undo] [×] │ │  ← Toast 1 (newest)
│ └──────────────────────────────────┘ │
│ ┌──────────────────────────────────┐ │
│ │ ⓘ  2 files uploaded         [×] │ │  ← Toast 2
│ └──────────────────────────────────┘ │
│ ┌──────────────────────────────────┐ │
│ │ ⚠  Connection unstable      [×] │ │  ← Toast 3 (oldest visible)
│ └──────────────────────────────────┘ │
└──────────────────────────────────────┘
```

**Dimensions:**
| Property | Value |
|----------|-------|
| Width | 360px (default), max 480px |
| Min height | 48px |
| Radius | `radius.card` (8px) |
| Shadow | `shadow.lg` |
| z-index | `z-index.toast` (60) |
| Position | Fixed, top-right (default) with 16px inset from viewport edges |

**Variants:**
| Variant | Icon | Colors | ARIA Role | Auto-dismiss |
|---------|------|--------|-----------|-------------|
| Info | ⓘ | `feedback.info-*` | `role="status"` | Yes (5s) |
| Success | ✓ | `feedback.success-*` | `role="status"` | Yes (5s) |
| Warning | ⚠ | `feedback.warning-*` | `role="alert"` | No (persistent) |
| Error | ✕ | `feedback.error-*` | `role="alert"` | No (persistent) |

**Sizes:**
| Size | Padding | Font | Icon |
|------|---------|------|------|
| sm | 12px × 12px | body-sm (14px) | 16px |
| md | 16px × 16px | body-base (16px) | 20px |

**States:**
1. **Entering** — slide in + fade in animation
2. **Visible** — resting, auto-dismiss timer running (if applicable)
3. **Hovered** — pause auto-dismiss timer; show close button if hidden by default
4. **Action pending** — user clicked undo/action; toast persists until action completes
5. **Dismissing** — slide out + fade out animation
6. **Dismissed** — removed from DOM

**Token Mapping:**
- Background: `surface.card` with left border `border.width.thick` (4px) in variant color
- Icon: variant feedback color (`feedback.success-icon`, etc.)
- Title: `textStyle.label` (14px/medium), `semantic.text.primary`
- Message: `textStyle.body-sm`, `semantic.text.secondary`
- Action button: ghost button style, variant color text
- Close button: `semantic.text.tertiary`, 20px icon
- Progress bar (auto-dismiss): 2px height at bottom, variant color, shrinks from 100% to 0%
- Gap between toasts in stack: `spacing.stack.sm` (8px)

**Stack Behavior:**
- Maximum 3 toasts visible simultaneously; older toasts queue and appear as newer ones dismiss
- Stack direction: newest on top (prepend)
- When a 4th toast arrives: oldest auto-dismissable toast is dismissed immediately; if all are persistent, queue the new one
- Stack position options: top-right (default), top-left, top-center, bottom-right, bottom-left, bottom-center

**Auto-dismiss:**
- Default timer: 5000ms for info/success
- Warning/error: persistent by default (user must dismiss)
- Pause timer on hover/focus; resume on mouse leave/blur
- Optional progress bar shows remaining time

**Swipe to Dismiss:**
- Touch devices: swipe in the direction of the toast's position edge to dismiss (swipe right for top-right toasts)
- Threshold: 50% of toast width triggers dismiss; otherwise snap back
- Desktop: not applicable (use close button or auto-dismiss)

**Animation:**
- Enter: `translateX(100%)` → `translateX(0)` over 200ms `ease-out` (right-positioned); adjust axis for other positions
- Exit: `translateX(0)` → `translateX(100%)` + fade-out over 150ms `ease-in`
- Stack reflow: remaining toasts slide to fill gap, 200ms `ease-in-out`
- Respect `prefers-reduced-motion`: use fade only, skip slide

**Accessibility:**
- Info/Success: `role="status"` with `aria-live="polite"` — announced after current speech
- Warning/Error: `role="alert"` with `aria-live="assertive"` — announced immediately
- Each toast: `aria-atomic="true"` (announce full content, not just changes)
- Close button: `aria-label="Dismiss notification"`
- Action button: descriptive label (e.g., `aria-label="Undo delete"`)
- Auto-dismiss: never auto-dismiss toasts with actions (undo, retry) — user needs time to act
- Keyboard: Tab reaches action/close buttons within the toast; Escape dismisses the focused toast
- Focus management: toasts do not steal focus from the user's current task; focus only enters toast on explicit Tab navigation
- Screen reader: toast content is announced via live region without focus shift
- Multiple toasts: each is an independent live region announcement
