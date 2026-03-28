# Design Review Workflow

A structured process for evaluating UI designs against quality, consistency, accessibility, and usability standards.

---

## Review Rubric (Score 1–10)

Score every design across these six dimensions. Each dimension contributes to the overall quality score.

| # | Dimension | Weight | What to Evaluate |
|---|-----------|--------|-----------------|
| 1 | **Visual Hierarchy** | 20% | Clear focal points, scannable layout, meaningful contrast between elements, proper heading structure |
| 2 | **Consistency** | 20% | Token adherence, component reuse, pattern consistency across pages, naming conventions |
| 3 | **Accessibility** | 20% | WCAG 2.2 AA compliance, color contrast, keyboard navigation, screen reader support, target sizes |
| 4 | **Usability** | 20% | Task efficiency, learnability, error prevention, cognitive load, clear affordances |
| 5 | **Responsiveness** | 10% | Mobile-first, breakpoint behavior, touch targets, content reflow, no horizontal scroll at 320px |
| 6 | **Performance** | 10% | Image optimization, animation efficiency, perceived loading speed, skeleton states |

**Scoring Guide:**
| Score | Meaning |
|-------|---------|
| 9–10 | Exemplary — ready to ship, reference-quality |
| 7–8 | Good — minor polish needed |
| 5–6 | Adequate — several issues to address before shipping |
| 3–4 | Below standard — significant rework needed |
| 1–2 | Critical — fundamental problems, back to drawing board |

**Overall Score Formula:**
```
Overall = (Hierarchy × 0.2) + (Consistency × 0.2) + (Accessibility × 0.2)
        + (Usability × 0.2) + (Responsiveness × 0.1) + (Performance × 0.1)
```

---

## Nielsen's 10 Usability Heuristics

Evaluate the design against each heuristic. Flag violations.

| # | Heuristic | What to Check |
|---|-----------|--------------|
| 1 | **Visibility of System Status** | Loading indicators, progress bars, current state indicators, selected states |
| 2 | **Match Between System & Real World** | Natural language, familiar icons, logical ordering, cultural conventions |
| 3 | **User Control & Freedom** | Undo/redo, cancel, back navigation, easy exit from flows, dismiss modals |
| 4 | **Consistency & Standards** | Platform conventions followed, internal consistency, component reuse |
| 5 | **Error Prevention** | Confirmations for destructive actions, constraints, disabled invalid options |
| 6 | **Recognition Over Recall** | Visible options, contextual help, recent items, search suggestions |
| 7 | **Flexibility & Efficiency** | Keyboard shortcuts, power user features, customizable views |
| 8 | **Aesthetic & Minimalist Design** | No unnecessary elements, clear visual weight, purposeful use of space |
| 9 | **Help Users Recognize & Recover from Errors** | Clear error messages, suggestions, inline validation, recovery paths |
| 10 | **Help & Documentation** | Onboarding, tooltips, help center, contextual guidance |

---

## Review Process

### Step 1: Context Gathering
Before reviewing, understand:
- **User**: Who is the target user? What is their technical level?
- **Goal**: What task is the user trying to accomplish?
- **Context**: Where does this screen fit in the user journey?
- **Constraints**: Technical limitations, brand guidelines, timeline

### Step 2: First Impression (30 seconds)
Without interacting:
- What draws your eye first? Is it the right thing?
- Can you tell what the page is for?
- Does it feel cluttered or balanced?
- Is the primary action obvious?

### Step 3: Task Walkthrough
Walk through the primary user task:
- Can you complete the task without instructions?
- How many steps does it take?
- Are there dead ends or confusion points?
- What happens with errors?

### Step 4: Systematic Check
Review against:
- [ ] **Token Compliance** — Are colors, typography, spacing from the token system?
- [ ] **Component Library** — Are standard components used correctly?
- [ ] **Accessibility** — Run through `accessibility/wcag-checklist.md` P0 items
- [ ] **Responsive** — Check mobile, tablet, desktop breakpoints
- [ ] **States** — Empty, loading, error, populated, overflow, edge cases
- [ ] **Content** — Real-ish content (not just "Lorem ipsum"), edge cases (long names, empty fields)

### Step 5: Document Findings

---

## Output Format: Findings Table

Prioritize every finding by severity:

| Severity | Definition | Action |
|----------|-----------|--------|
| **Critical** | Blocks users, a11y violation (P0), data loss risk | Must fix before launch |
| **Major** | Significant UX degradation, inconsistency across flows | Fix in current sprint |
| **Minor** | Small polish issues, non-blocking inconsistencies | Fix when convenient |
| **Enhancement** | Improvement ideas, not bugs | Add to backlog |

**Findings Table Template:**

| # | Severity | Category | Location | Finding | Recommendation | Heuristic |
|---|----------|----------|----------|---------|---------------|-----------|
| 1 | Critical | A11y | Login form | Error messages only use red color | Add error icon + text description | — |
| 2 | Major | Usability | Dashboard | No empty state for new users | Add onboarding empty state with CTA | H6 |
| 3 | Minor | Consistency | Settings | Save button alignment differs from other forms | Right-align to match pattern | H4 |
| 4 | Enhancement | Efficiency | Data table | No keyboard shortcuts for bulk actions | Add Shift+Click for range select | H7 |

---

## Design Audit Process (Existing Products)

For auditing an existing product (not a new design):

### 1. Inventory
- Screenshot every unique screen
- Catalog all unique components (buttons, inputs, cards, etc.)
- Document all color values, font sizes, spacing values in use

### 2. Gap Analysis
Compare inventory against design system tokens:
- **Off-token colors**: hex values not in `tokens/colors.json`
- **Off-token typography**: font sizes/weights not in `tokens/typography.json`
- **Off-token spacing**: padding/margin values not on the 4px grid
- **Inconsistent components**: same concept, different implementations

### 3. Severity Mapping
Map every gap to a severity:
- **Critical**: Accessibility violations (contrast, target size, keyboard)
- **Major**: Inconsistencies that confuse users
- **Minor**: Visual polish (1px off, slightly wrong shade)
- **Enhancement**: Opportunities to improve

### 4. Remediation Roadmap
Group findings into actionable batches:
1. **Quick wins** — Token swaps, contrast fixes (< 1 hour each)
2. **Component standardization** — Replace one-offs with system components
3. **Layout refactors** — Grid alignment, responsive fixes
4. **New patterns** — Missing states, new components needed

---

## Concrete Finding Examples

8 real-world examples across all severity levels, demonstrating what to look for and how to document findings.

### Critical Findings

**Example 1: Form input with insufficient contrast**
| Field | Detail |
|-------|--------|
| Component | Text Input (login form, email field) |
| What's wrong | Input border uses `border.default` (gray.200, #e5e7eb) on `surface.page` (white). Contrast ratio is 1.4:1 — below WCAG 2.2 SC 1.4.11 (3:1 for UI components). |
| Token/guideline violated | WCAG 2.2 Level AA, SC 1.4.11 Non-text Contrast. Border should reference `border.strong` (gray.300) minimum for essential UI boundaries. |
| How to fix | Change resting border to `border.strong` (gray.300, #d1d5db — 1.9:1) and add a subtle inner shadow or background tint (`surface.sunken`) to reinforce the input boundary without relying on border contrast alone. For a fully compliant border-only approach, use gray.500 (#6b7280 — 4.6:1). |

**Example 2: Error state communicates only through color**
| Field | Detail |
|-------|--------|
| Component | Form Field (registration form, password field) |
| What's wrong | Invalid password shows a red border (`border.error`) with no icon, no text, and no change to the label. Color-blind users (8% of males) cannot detect the error. |
| Token/guideline violated | WCAG 2.2 SC 1.4.1 Use of Color. Design system rule: color is never the only indicator. |
| How to fix | Add an error icon (circle-exclamation) using `feedback.error-icon` before the error message text. Display inline error text below the field using `feedback.error-text`. Change the label text to include "(required)" or the specific validation rule that failed. |

### Major Findings

**Example 3: Missing empty state on dashboard**
| Field | Detail |
|-------|--------|
| Component | Data Table (projects list, main dashboard) |
| What's wrong | New users see a blank area with only the table header and zero rows. No guidance on what to do next. |
| Token/guideline violated | Nielsen Heuristic #6 (Recognition Over Recall), progressive disclosure principle. `components/organisms.md` Data Table requires an empty state. |
| How to fix | Add an empty state with: illustration (subtle, on-brand), heading "No projects yet", description "Create your first project to start tracking progress", and a primary CTA button "Create Project" using `action.primary`. |

**Example 4: Inconsistent button hierarchy across pages**
| Field | Detail |
|-------|--------|
| Component | Button (settings page vs. profile page) |
| What's wrong | Settings page uses a filled primary button for "Save", while the profile page uses an outlined button for the same action. Both are the primary page action. Secondary actions are styled identically on both pages. |
| Token/guideline violated | Nielsen Heuristic #4 (Consistency & Standards). `components/atoms.md` Button section: primary action always uses `component.button.primary-bg`. |
| How to fix | Standardize: primary page action always uses the primary filled variant (`component.button.primary-bg`). Secondary actions use the secondary variant. Audit all pages for the same inconsistency. |

### Minor Findings

**Example 5: Spacing inconsistency in card grid**
| Field | Detail |
|-------|--------|
| Component | Card Grid (project cards, dashboard) |
| What's wrong | Gap between cards is 20px. The design system spacing scale uses 4px increments. 20px is on the scale (`spacing.5`), but the semantic token `spacing.stack.lg` (16px) or `spacing.stack.xl` (24px) should be used instead of a raw value. |
| Token/guideline violated | Token architecture rule: use semantic spacing tokens, not raw scale values. See `tokens/spacing.json` semantic section. |
| How to fix | Choose `spacing.stack.lg` (16px) for tighter layout or `spacing.stack.xl` (24px) for more breathing room. Apply the semantic token, not a hardcoded pixel value. |

**Example 6: Save button alignment differs from other forms**
| Field | Detail |
|-------|--------|
| Component | Button group (settings form, bottom action bar) |
| What's wrong | Save/Cancel buttons are left-aligned on the settings page but right-aligned on the billing page and the profile page. |
| Token/guideline violated | Nielsen Heuristic #4 (Consistency & Standards). Internal pattern: form action buttons are right-aligned per `components/templates.md` Settings template. |
| How to fix | Right-align the button group on the settings page to match all other form pages. Primary action (Save) on the right, secondary (Cancel) to its left. |

### Enhancement Findings

**Example 7: No keyboard shortcut for common action**
| Field | Detail |
|-------|--------|
| Component | Data Table (bulk actions toolbar) |
| What's wrong | Users must click a checkbox for each row, then click the action button. No keyboard shortcut for "select all" or range selection. Power users with 100+ items are significantly slowed. |
| Token/guideline violated | Nielsen Heuristic #7 (Flexibility & Efficiency of Use). |
| How to fix | Add Ctrl/Cmd+A for select all visible rows, Shift+Click for range selection, and a "Select all X items" link that appears after Ctrl+A selects the current page. Add keyboard shortcut hints to the toolbar tooltip. |

**Example 8: Tooltip delay is too long**
| Field | Detail |
|-------|--------|
| Component | Tooltip (icon-only buttons in the toolbar) |
| What's wrong | Tooltips appear after 800ms hover delay. Icon-only buttons have no visible text label, so users must wait 800ms to learn what each button does. Default expected delay is 200-400ms. |
| Token/guideline violated | Nielsen Heuristic #6 (Recognition Over Recall). `components/atoms.md` Tooltip: recommended delay is 200ms for icon-only triggers. |
| How to fix | Reduce tooltip delay to 200ms for icon-only buttons. Keep 400ms for elements that already have visible text labels (where the tooltip is supplementary). |

---

## Scoring Rubric: Detailed Breakdown

What a 3/10, 7/10, and 10/10 looks like for each of the six scoring dimensions.

### 1. Visual Hierarchy (20%)

| Score | Description |
|-------|-------------|
| **3/10** | No clear focal point. Heading, body text, and CTAs compete for attention at similar visual weight. User cannot tell what the page is about within 3 seconds. Multiple elements use the same font size and weight. Primary action button is the same visual weight as secondary content. |
| **7/10** | Clear primary focal point (heading or hero element). Scanning path follows logical order. Primary CTA stands out. Minor issues: one section competes with the hero, or a secondary element is slightly too prominent. Typography scale used but heading hierarchy has one skip. |
| **10/10** | Immediate focal point that communicates the page purpose. Perfect scanning path: Z-pattern or F-pattern appropriate to content type. Every element has intentional visual weight — headings, body, captions, CTAs each occupy a distinct level. Whitespace creates clear grouping. Primary action is unmissable, secondary actions are subordinate. |

### 2. Consistency (20%)

| Score | Description |
|-------|-------------|
| **3/10** | Multiple off-token values (hardcoded hex colors, arbitrary spacing). Same action uses different button variants on different pages. Component names/styles vary between sections. Naming conventions are inconsistent (camelCase mixed with kebab-case). Patterns are reinvented instead of reused. |
| **7/10** | All values map to design tokens. Components are reused correctly. One or two minor inconsistencies: button alignment differs on one page, or icon style shifts in one section. Naming is mostly consistent. |
| **10/10** | 100% token adherence — zero hardcoded values. Every recurring pattern uses the same component. Naming conventions are rigorous. Page-to-page navigation feels like one coherent product. A new team member could predict how an unvisited page looks based on existing patterns. |

### 3. Accessibility (20%)

| Score | Description |
|-------|-------------|
| **3/10** | Multiple WCAG AA violations. Text contrast below 4.5:1 in key areas. No visible focus indicators. Interactive elements unreachable by keyboard. Color used as the sole indicator for errors/status. Missing alt text on images. Touch targets below 24px. |
| **7/10** | All text passes 4.5:1 contrast. Focus indicators present on all interactive elements. Keyboard navigation works for the primary flow. ARIA labels present on icon-only buttons. Minor issues: one or two focus traps in complex components, or a few touch targets at 20-23px. |
| **10/10** | Full WCAG 2.2 AA compliance. All contrast ratios pass. Keyboard navigation covers every flow including modals and dropdowns with correct focus management. ARIA patterns implemented per spec. Touch targets at 44px for primary actions, 24px+ for all targets. Screen reader announces all states (loading, error, success). Reduced-motion preferences respected. |

### 4. Usability (20%)

| Score | Description |
|-------|-------------|
| **3/10** | Primary task takes 8+ clicks when 3 would suffice. Users encounter dead ends. Error messages are cryptic ("Error 422"). No undo for destructive actions. Cognitive load is high — user must remember information across screens. Labels are ambiguous. |
| **7/10** | Primary task is completable in a reasonable number of steps. Clear labels and affordances. Error messages explain the problem. Undo exists for major actions. Minor issues: one step in the flow is non-obvious, or a secondary task requires too many clicks. |
| **10/10** | Primary task is efficient and intuitive — zero hesitation. Progressive disclosure limits cognitive load to what's needed at each step. Error messages state what went wrong, why, and how to fix it. Undo/redo for all destructive actions. Confirmation dialogs for irreversible actions. First-time user can complete the task without instructions. Power users have shortcuts. |

### 5. Responsiveness (10%)

| Score | Description |
|-------|-------------|
| **3/10** | Layout breaks at mobile widths. Horizontal scrolling required at 320px. Touch targets overlap or are too small. Content is clipped or hidden without scroll indicators. Desktop layout is simply squished onto mobile. |
| **7/10** | Layouts reflow correctly at all major breakpoints (mobile, tablet, desktop). No horizontal scroll. Touch targets are adequate. Minor issues: one component doesn't reflow cleanly, or a data table is awkward on mobile. |
| **10/10** | Mobile-first design with intentional layout choices at every breakpoint. Navigation transforms appropriately (sidebar to hamburger). Data-heavy content transforms (table to card list). Touch targets are 44px on mobile. Content priority shifts for mobile context (key metric first, charts second). Tested at 320px, 375px, 768px, 1024px, 1440px. |

### 6. Performance (10%)

| Score | Description |
|-------|-------------|
| **3/10** | Large unoptimized images. No loading states — content pops in after a delay. Animations are janky (layout thrashing, 60fps not maintained). No skeleton screens. Perceived load time > 3 seconds for primary content. |
| **7/10** | Images are optimized (WebP/AVIF, lazy loaded). Skeleton screens for async content. Animations run at 60fps using transforms/opacity. Minor issues: one large image not optimized, or a skeleton screen is missing on a secondary view. |
| **10/10** | All images are next-gen format (WebP/AVIF), responsive (srcset), and lazy loaded below the fold. Skeleton screens for every async-loaded section. Animations use only compositor properties (transform, opacity). Fonts loaded with font-display: swap and preloaded. Perceived instant rendering — critical content visible within 1 second. |

---

## Review Checklist (Quick Reference)

```
□ Visual hierarchy — clear focal point and scanning path
□ Typography — heading levels correct, body text readable
□ Color — sufficient contrast, not only means of information
□ Spacing — consistent rhythm, breathing room
□ Alignment — grid-aligned, no visual drift
□ Components — using established patterns correctly
□ States — all 6 states defined for interactive elements
□ Empty states — meaningful illustration + CTA
□ Loading — skeleton or spinner for async content
□ Errors — clear, actionable, accessible error messages
□ Responsive — mobile, tablet, desktop considered
□ Touch targets — 24px minimum (48px recommended)
□ Focus indicators — visible on all interactive elements
□ Keyboard — full flow completable without mouse
□ Screen reader — meaningful landmarks, labels, live regions
□ Motion — animations serve purpose, respect prefers-reduced-motion
□ Content — real content, edge cases (long strings, empty, overflow)
□ Dark mode — if supported, tested thoroughly
```
