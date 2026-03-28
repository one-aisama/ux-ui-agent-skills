# UX/UI Agent Skills

> Transform Claude into a **Senior Design Architect** with structured design tokens, accessibility standards, and production-ready component patterns.

A comprehensive skill pack that gives Claude deep UX/UI expertise. Drop it into any project — get consistent, accessible, token-driven design outputs.

---

## Highlights

- **Modular architecture** — lightweight 80-line CLAUDE.md entry point + 4 focused reference docs
- **35+ components** — from atoms (Button, Spinner, Skeleton) to organisms (Data Table, Command Palette, Toast)
- **Full interaction specs** — Data Table with sorting, multi-select, filtering, column resize, responsive card layout
- **Dark mode strategy** — semantic token swaps, shadow behavior, icon guidance, component overrides, testing checklist
- **Real workflow examples** — 10+ design review findings, Button handoff walkthrough, usability test scripts
- **Cross-referenced** — components link to ARIA patterns, framework code, and tokens
- **WCAG 2.2 AA** — accessibility-first with P0/P1/P2 prioritized checklist

---

## Quick Start

### 1. Clone

```bash
git clone https://github.com/one-aisama/ux-ui-agent-skills.git
```

### 2. Copy into your project

```bash
cp -r ux-ui-agent-skills/ your-project/
```

### 3. Use with Claude

Open the project in **Claude Code** or any Claude-powered IDE. `CLAUDE.md` is loaded automatically — Claude becomes a design system expert with access to all reference files.

**Example prompts:**
```
"Design a notification component with all states and accessibility"
"Review this login page against WCAG 2.2 and Nielsen's heuristics"
"Generate React + Tailwind code for a data table with sorting and pagination"
"Create a color token palette for a fintech brand"
"Audit this form for accessibility — give me a prioritized findings table"
```

---

## Project Structure

```
.
├── CLAUDE.md                         # Agent entry point (modular, ~80 lines)
│
├── docs/                             # Detailed reference guides
│   ├── token-system.md               # 3-tier token architecture & rules
│   ├── component-standards.md        # Quality bar, state requirements
│   ├── accessibility-guide.md        # WCAG summary & decision rules
│   ├── code-output-rules.md          # Framework code generation rules
│   └── dark-mode-strategy.md         # Complete dark mode implementation guide
│
├── tokens/                           # Design tokens (DTCG format)
│   ├── colors.json                   # 6 hues × 11 shades + semantic + component + dark mode
│   ├── typography.json               # Major Third scale, 3 font families, 14 text styles
│   ├── spacing.json                  # 4px base unit, 21 values + semantic aliases
│   ├── shadows.json                  # 5-level elevation + inner + colored + focus ring
│   ├── borders.json                  # Radius & width scales + semantic radii
│   └── breakpoints.json              # Mobile-first breakpoints + grid + z-index
│
├── components/                       # Component specs (Atomic Design)
│   ├── atoms.md                      # Button, Input, Icon, Badge, Divider, Spinner, Skeleton, Text...
│   ├── molecules.md                  # Form Field, Card, Alert, Pagination, Breadcrumb, Tabs...
│   ├── organisms.md                  # Header, Data Table, Modal, Command Palette, Toast...
│   └── templates.md                  # Dashboard, Auth, Settings, List/Detail layouts
│
├── accessibility/                    # WCAG & ARIA references
│   ├── wcag-checklist.md             # WCAG 2.2 checklist (POUR, P0/P1/P2)
│   └── aria-patterns.md             # WAI-ARIA patterns for 15+ components
│
├── workflows/                        # Design process guides
│   ├── design-review.md              # Review rubric + 10 real finding examples
│   ├── design-to-code.md             # Handoff workflow + worked Button example
│   └── prototyping.md                # Fidelity ladder + usability test scripts
│
└── frameworks/                       # Framework implementation patterns
    ├── react-tailwind.md             # React 19 + Tailwind v4 + TypeScript + cva
    ├── nextjs.md                     # Next.js 15 App Router patterns
    └── swiftui.md                    # SwiftUI 6 + Dynamic Type
```

---

## Token Architecture

Three-tier hierarchy using the [DTCG](https://design-tokens.github.io/community-group/format/) standard:

```
Component Tokens    button-bg-primary ──► Semantic Tokens    action.primary ──► Primitive Tokens    blue.600 = #2563EB
(use in code)                            (use in design)                       (raw palette)
```

- **Primitive** — Raw values. Never referenced directly in components.
- **Semantic** — Purpose-based aliases (`action.primary`, `text.secondary`, `surface.card`).
- **Component** — Scoped to specific components (`button.primary-bg`, `input.border-focus`).

Dark mode swaps semantic tokens — primitives stay the same. See `docs/dark-mode-strategy.md` for the complete implementation guide.

---

## Supported Frameworks

| Framework | Version | Key Patterns |
|-----------|---------|-------------|
| React + Tailwind | React 19, Tailwind v4 | `forwardRef`, `cva`, `cn()`, CSS custom properties |
| Next.js | 15 (App Router) | Server/Client Components, `next/font`, Server Actions |
| SwiftUI | 6 (iOS 18+) | `ButtonStyle`, `ViewModifier`, `@ScaledMetric`, Dynamic Type |

---

## Accessibility

All outputs follow **WCAG 2.2 Level AA** minimum:

- Color contrast: 4.5:1 (text), 3:1 (UI)
- Keyboard navigable with visible focus indicators
- Screen reader compatible with proper ARIA roles
- Touch targets: 24×24px minimum (WCAG 2.5.8)
- WCAG 2.2 specific: Focus Not Obscured, Target Size, Accessible Authentication

---

## Customization

This is a **starter kit** — adapt it to your project:

- **Brand colors:** Edit `tokens/colors.json` primitives, update semantic references
- **Typography:** Swap font families in `tokens/typography.json`
- **Components:** Add new ones following the spec format in `components/`
- **Frameworks:** Add `vue.md`, `flutter.md`, etc. in `frameworks/`

---

## Requirements

- [Claude Code](https://claude.com/claude-code) CLI, desktop app, or any Claude-powered IDE
- No build tools, dependencies, or runtime needed — pure knowledge layer

---

## License

MIT
