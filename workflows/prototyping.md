# Prototyping & User Research Workflow

A structured approach to prototyping at the right fidelity, gathering user feedback, and iterating toward validated solutions.

---

## 5-Level Fidelity Ladder

Move up the ladder as confidence increases. Never jump to high fidelity before validating the concept.

### Level 1: Content-First (Text Only)
**Output:** Plain text outline, bullet points, content hierarchy
**Time:** 15–30 minutes
**Tools:** Text editor, markdown
**Purpose:** Validate information architecture and content strategy before any visual work

```markdown
# Dashboard
## KPI Section
- Total Revenue: $X (↑ 12% from last month)
- Active Users: X (↓ 3% from last week)
- Conversion Rate: X% (→ stable)
- Support Tickets: X (↑ 5 new today)

## Primary Chart
- Revenue over time (last 30 days)
- Comparison toggle: vs. previous period

## Recent Activity
- Table: [User] did [Action] on [Object] at [Time]
- Max 10 rows, "View all" link
```

**Validate:** Does the content answer the user's questions? Is anything missing? Is the order right?

### Level 2: Wireframe (Layout)
**Output:** Box-and-line layout, grayscale, no visual design
**Time:** 1–2 hours
**Tools:** Figma (wireframe), Excalidraw, paper sketches
**Purpose:** Validate layout, information hierarchy, and navigation flow

**What to include:**
- Content zones with correct proportions
- Navigation structure
- Interactive elements (buttons, inputs) as gray boxes
- Annotations for behavior ("this opens a modal", "this filters the list")

**What NOT to include:**
- Colors, typography choices, icons
- Pixel-perfect spacing
- Animations or transitions

**Validate:** Can users find what they need? Is the navigation intuitive? Do the content zones make sense?

### Level 3: Low-Fidelity Prototype (Interactive)
**Output:** Clickable wireframes with basic navigation flow
**Time:** 2–4 hours
**Tools:** Figma prototyping, HTML/CSS rough prototype
**Purpose:** Validate user flows and task completion

**What to include:**
- Clickable navigation between screens
- Form flows (multi-step)
- Key interactions (open/close, expand/collapse)
- Error states and empty states
- Real-ish content (not Lorem Ipsum)

**Validate:** Can users complete the primary task? Where do they get stuck? How many steps does it take?

### Level 4: High-Fidelity Mockup (Visual Design)
**Output:** Pixel-perfect designs with design tokens applied
**Time:** 4–8 hours per page
**Tools:** Figma with design system components
**Purpose:** Validate visual design, brand alignment, and design system compliance

**What to include:**
- All design tokens applied (colors, typography, spacing, shadows)
- All component states (default, hover, focus, disabled, error, loading)
- Responsive layouts (mobile, tablet, desktop)
- Dark mode (if applicable)
- All edge cases (empty, overflow, error)

**Validate:** Does it feel right? Is the brand consistently applied? Are there any accessibility issues?

### Level 5: Production Prototype (Code)
**Output:** Working code with real interactions, fake data
**Time:** 1–3 days
**Tools:** React/Next.js with design system components
**Purpose:** Validate technical feasibility, real-device behavior, performance, and accessibility

**What to include:**
- Real components from the design system
- Realistic data (seeded, not Lorem Ipsum)
- Real interactions (forms submit, lists filter, modals open)
- Responsive behavior on real devices
- Keyboard navigation and screen reader support

**Validate:** Does it perform well? Does it feel responsive? Are there technical constraints that require design changes?

---

## User Journey Mapping

### Template

```
Journey: [User Goal — e.g., "Purchase a subscription"]
Persona: [Who — e.g., "Small business owner, non-technical"]
Context: [Where/When — e.g., "Mobile, during commute"]

Phase 1: Awareness
├── Action: Sees ad / lands on homepage
├── Touchpoint: Landing page
├── Thought: "Does this solve my problem?"
├── Emotion: Curious 😐
├── Opportunity: Clear value prop above the fold
└── Metric: Bounce rate

Phase 2: Exploration
├── Action: Browses features, reads case studies
├── Touchpoint: Features page, testimonials
├── Thought: "How does this compare to alternatives?"
├── Emotion: Evaluating 🤔
├── Opportunity: Comparison table, social proof
└── Metric: Pages per session, time on site

Phase 3: Decision
├── Action: Views pricing, starts trial
├── Touchpoint: Pricing page, sign-up flow
├── Thought: "Is it worth it? What if I don't like it?"
├── Emotion: Anxious 😟
├── Opportunity: Free trial, money-back guarantee, simple pricing
└── Metric: Trial sign-up rate

Phase 4: Onboarding
├── Action: Sets up account, imports data
├── Touchpoint: Onboarding wizard, dashboard
├── Thought: "How do I get started?"
├── Emotion: Overwhelmed → Relieved 😰→😊
├── Opportunity: Progressive disclosure, guided setup, quick wins
└── Metric: Onboarding completion rate, time to first value

Phase 5: Retention
├── Action: Regular use, discovers advanced features
├── Touchpoint: Dashboard, notifications, reports
├── Thought: "This is part of my workflow now"
├── Emotion: Confident 😊
├── Opportunity: Feature discovery, personalization, integrations
└── Metric: DAU/MAU, feature adoption, NPS
```

---

## Information Architecture Methodology

### Card Sorting (Discover Mental Models)

**Open Card Sort** — Users group and label content themselves
1. Write each content item / feature on a card (30–60 cards)
2. Ask 5–15 users to group cards into categories they define
3. Analyze: Look for common groupings and label patterns
4. Use findings to inform navigation structure

**Closed Card Sort** — Users sort content into predefined categories
1. Define your proposed categories (navigation labels)
2. Give users the same cards
3. Ask them to place each card in the most appropriate category
4. Measure: % agreement per card. < 60% agreement = confusing category

### Tree Testing (Validate Navigation)

1. Create a text-only hierarchy of your navigation (no visual design)
2. Give users tasks: "Where would you find [X]?"
3. Measure:
   - **Success rate**: % who found the right location
   - **Directness**: % who went straight there (no backtracking)
   - **Time**: How long to complete each task
4. Target: > 80% success rate, > 60% directness

---

## Usability Testing Script Template

### Before the Session (5 min)

```
"Hi [Name], thanks for joining today. I'm [Your Name].

We're testing [product/feature], not testing you — there are no wrong answers.

I'll ask you to complete some tasks while thinking out loud. Tell me what you're
looking at, what you're thinking, and what you expect to happen.

I might not answer questions during the tasks because I want to see how you'd
figure things out on your own. I'll answer everything after.

This session will take about [30-45] minutes. Any questions before we start?"
```

### Task Format

```
Task [N]: [Clear, goal-oriented instruction]
─────────────────────────────────────────
Scenario: "Imagine you're [context]. You want to [goal]."
Task: "Starting from this page, [specific action]."
Success criteria: [What "done" looks like]
Observe:
  □ Did they find the right path?
  □ How long did it take?
  □ Did they hesitate or backtrack?
  □ Any verbal confusion or frustration?
  □ Did they use the expected interaction pattern?
```

### Example Tasks

```
Task 1: Find Information
Scenario: "You're a new user who just signed up."
Task: "Find the setting to change your notification preferences."
Success: User navigates to Settings → Notifications
Time limit: 2 minutes

Task 2: Complete an Action
Scenario: "You need to add a new team member to your project."
Task: "Invite alex@example.com as an editor to the 'Q1 Campaign' project."
Success: Invitation sent with correct role
Time limit: 3 minutes

Task 3: Recover from Error
Scenario: "You accidentally deleted an important item."
Task: "Recover the item you just deleted."
Success: Item restored from trash/undo
Time limit: 2 minutes
```

### Post-Task Questions
After each task:
1. "How easy or difficult was that?" (1–5 scale)
2. "Was there anything confusing?"
3. "What did you expect to happen when you clicked [X]?"

### Post-Session Questions (5 min)
1. "What was your overall impression?"
2. "What was the easiest part? The hardest?"
3. "Is there anything you expected to find but didn't?"
4. "Would you use this? Why or why not?"
5. "Any other feedback?"

### Synthesis Template

| Task | Success Rate | Avg Time | Avg Difficulty | Key Observations |
|------|-------------|----------|---------------|-----------------|
| 1. Find notifications | 4/5 (80%) | 45s | 2.1/5 | 1 user looked in Profile first |
| 2. Invite team member | 3/5 (60%) | 2m 10s | 3.8/5 | Role picker was confusing |
| 3. Recover deleted item | 5/5 (100%) | 15s | 1.2/5 | Undo toast worked well |

**Top Findings:**
1. **Critical:** Role picker needs clearer labels (60% success → target 90%)
2. **Major:** Notification settings should be accessible from notification bell
3. **Minor:** Users expected drag-and-drop for reordering

**Recommended Actions:**
1. Redesign role picker with descriptions per role
2. Add "Notification settings" link to notification dropdown
3. Add drag-and-drop as enhancement in next sprint

---

## Research Methods Cheat Sheet

| Method | When | Sample Size | Time | Output |
|--------|------|-------------|------|--------|
| Stakeholder interviews | Project kickoff | 3–5 | 30 min each | Requirements, constraints |
| Competitive analysis | Discovery | 3–5 competitors | 2–4 hours | Feature matrix, patterns |
| Card sorting | IA design | 15–30 users | 15 min each | Navigation structure |
| Tree testing | IA validation | 30–50 users | 10 min each | Findability scores |
| Usability testing | Any fidelity | 5 users | 30–45 min each | Task success, pain points |
| A/B testing | Optimization | 1000+ users | 1–2 weeks | Conversion data |
| Surveys | Broad feedback | 100+ users | 5 min each | Quantitative satisfaction |
| Diary studies | Usage patterns | 10–15 users | 1–2 weeks | Behavioral insights |

---

## Usability Test Report Template

Use this template to document findings from a usability testing session. One report per test round.

### Report Header

```
USABILITY TEST REPORT
─────────────────────────────────────────────
Product:         [Product name]
Feature/Flow:    [Feature being tested]
Test Date:       [YYYY-MM-DD]
Facilitator:     [Name]
Participants:    [N] participants ([demographics summary])
Prototype Level: [Level 1-5 from fidelity ladder]
Environment:     [Remote/In-person, device type, tool used]
```

### Task Results

For each task tested, fill in one row:

| # | Task | Success Rate | Avg Time on Task | Errors (total) | Critical Errors | Participant Quotes | Recommendations |
|---|------|:---:|:---:|:---:|:---:|---|---|
| 1 | [Task description] | X/N (%) | Xm Xs | X | X | "Direct quote from participant" | [What to change] |
| 2 | ... | ... | ... | ... | ... | ... | ... |

**Success criteria definitions:**
- **Complete success**: Task completed without assistance, within time limit
- **Partial success**: Task completed with minor hesitation or one backtrack
- **Failure**: Task not completed, required facilitator help, or exceeded time limit

### Error Classification

| Error Type | Definition | Example |
|-----------|-----------|---------|
| **Navigation error** | User went to the wrong section/page | Looked in Profile instead of Settings |
| **Comprehension error** | User misunderstood a label, icon, or instruction | Thought "Archive" meant "Delete" |
| **Interaction error** | User used wrong gesture, click target, or input method | Tried to drag when should have clicked |
| **Recovery error** | User could not recover from a mistake | Could not find undo or back navigation |

### Findings Summary

```
TOP FINDINGS (sorted by severity)
──────────────────────────────────────────────────────

Finding 1: [Title]
  Severity:    Critical / Major / Minor / Enhancement
  Tasks:       Affected task #1, #3
  Frequency:   X/N participants encountered this
  Description: [What happened]
  Evidence:    "[Participant quote]" — P2
               "[Another quote]" — P4
  Root Cause:  [Why it happened — label unclear, hidden control, etc.]
  Recommendation: [Specific fix with token/component reference if applicable]

Finding 2: ...
```

### Metrics Dashboard

```
OVERALL METRICS
──────────────────────────────────────────────────────
Task completion rate (all tasks):    X%    Target: >80%
Average time on task:                Xm Xs
System Usability Scale (SUS):        X/100  Target: >68
Net Promoter Score (NPS):            X      Target: >0
Single Ease Question (SEQ avg):      X/7    Target: >5.5

PER-TASK BREAKDOWN
──────────────────────────────────────────────────────
Task 1: [name]    Success: X%   Time: Xm   SEQ: X/7
Task 2: [name]    Success: X%   Time: Xm   SEQ: X/7
Task 3: [name]    Success: X%   Time: Xm   SEQ: X/7
Task 4: [name]    Success: X%   Time: Xm   SEQ: X/7
Task 5: [name]    Success: X%   Time: Xm   SEQ: X/7
```

### Action Items

| Priority | Action | Owner | Deadline | Finding # |
|----------|--------|-------|----------|-----------|
| P0 | [Must fix before next round] | [Name] | [Date] | #X |
| P1 | [Fix this sprint] | [Name] | [Date] | #X |
| P2 | [Backlog] | [Name] | — | #X |

---

## Example Test Script: Analytics Dashboard App

A complete, ready-to-use test script with 5 realistic tasks for testing a SaaS analytics dashboard.

### Pre-Session Setup

```
Product:     DataPulse Analytics Dashboard
Prototype:   Level 4 (High-fidelity Figma prototype)
Participant: Small business owner or marketing manager
Duration:    40 minutes
Device:      Desktop (1440px viewport)
```

### Introduction (3 minutes)

```
"Hi [Name], thanks for taking the time today. I'm [Your Name].

We're testing a new analytics dashboard for a product called DataPulse.
I want to emphasize — we're testing the product, not you. There are no
wrong answers, and you can't break anything.

I'll give you some tasks and ask you to think out loud as you work through
them. Tell me what you see, what you expect, and what confuses you.

I might stay quiet during the tasks because I want to see how you'd figure
things out naturally. I'll answer all your questions at the end.

This session will take about 40 minutes. We'll do 5 short tasks, then I'll
ask for your overall thoughts. Ready to begin?"
```

### Task 1: Understand the Dashboard (Orientation)

```
Task 1: Get the Big Picture
──────────────────────────────────────────────────────
Scenario:  "Imagine you just logged into DataPulse for the first time
            today. This is your company's analytics dashboard."

Task:      "Without clicking anything, tell me: what is the most
            important metric on this page? And how is the business
            doing overall — good, bad, or mixed?"

Success:   Participant identifies the primary KPI (revenue) and can
           articulate whether the trend is positive or negative
           within 30 seconds.

Time limit: 1 minute

Observe:
  [ ] Where do their eyes go first? (matches intended visual hierarchy?)
  [ ] Can they read the KPI values without squinting or leaning in?
  [ ] Do they understand the trend indicators (arrows, percentages)?
  [ ] Do they notice the comparison period ("vs. last month")?
  [ ] Do they express confidence or confusion?

Post-task:
  "How easy was it to understand what's going on?" (1–5)
  "Was there anything you weren't sure about?"
```

### Task 2: Filter Data by Date Range (Core Interaction)

```
Task 2: Change the Date Range
──────────────────────────────────────────────────────
Scenario:  "Your CEO asked for last quarter's numbers specifically.
            You need to see data for Q4 2025 (October through December)."

Task:      "Change the dashboard to show only Q4 2025 data."

Success:   Participant finds the date picker, selects Oct 1 – Dec 31
           (or selects a "Q4" preset), and the dashboard updates.

Time limit: 2 minutes

Observe:
  [ ] Do they find the date filter immediately or search for it?
  [ ] Do they try the preset ("Last Quarter") or manually select dates?
  [ ] Is the date picker interaction smooth (no mis-clicks)?
  [ ] Do they confirm the dashboard updated (check for visual feedback)?
  [ ] Do they understand what "Compared to" period changed to?

Post-task:
  "How easy was that?" (1–5)
  "Was the date picker what you expected?"
```

### Task 3: Investigate a Metric (Drill-Down)

```
Task 3: Investigate a Drop in Conversions
──────────────────────────────────────────────────────
Scenario:  "You notice the conversion rate dropped 12% compared to last
            quarter. Your marketing team wants to know which traffic
            source caused the drop."

Task:      "Find out which traffic source had the biggest decline in
            conversions."

Success:   Participant navigates to a conversion breakdown (by source),
           identifies the underperforming channel (e.g., "Paid Social
           decreased 34%").

Time limit: 3 minutes

Observe:
  [ ] Do they click on the conversion rate KPI card? (expected path)
  [ ] Do they look for a "Conversions" tab or section in the sidebar?
  [ ] Do they understand the drill-down chart/table when they reach it?
  [ ] Can they compare sources to identify the worst performer?
  [ ] Do they try to hover/click chart elements for more detail?

Post-task:
  "How easy was it to find that information?" (1–5)
  "How would you share this finding with your marketing team?"
```

### Task 4: Create and Save a Report (Complex Action)

```
Task 4: Save a Custom Report
──────────────────────────────────────────────────────
Scenario:  "You want to create a saved report that shows revenue and
            conversion rate by traffic source, so you can check it
            weekly without reconfiguring the dashboard each time."

Task:      "Create a new saved report with revenue and conversion rate
            broken down by traffic source. Name it 'Weekly Channel
            Performance'."

Success:   Participant finds report builder, selects the correct metrics,
           applies traffic source breakdown, names the report, and saves.

Time limit: 4 minutes

Observe:
  [ ] Do they find the "Reports" or "Save" feature?
  [ ] Do they understand how to select metrics (drag-and-drop? checkboxes?)
  [ ] Is the "breakdown by" concept clear (dimension vs. metric)?
  [ ] Do they successfully name and save the report?
  [ ] Do they know where to find the saved report later?
  [ ] Any confusion about "Save" vs "Save As" vs "Export"?

Post-task:
  "How easy was that?" (1–5)
  "Was there anything confusing about creating the report?"
  "Where would you expect to find this report next time you log in?"
```

### Task 5: Invite a Team Member (Settings/Admin)

```
Task 5: Invite a Colleague
──────────────────────────────────────────────────────
Scenario:  "Your marketing director Sarah (sarah@datapulse-demo.com)
            needs access to the dashboard. She should be able to view
            reports but not change settings."

Task:      "Invite Sarah with view-only access."

Success:   Participant navigates to team/settings, enters the email,
           selects a "Viewer" role (not Admin or Editor), and sends
           the invitation.

Time limit: 3 minutes

Observe:
  [ ] Do they look in Settings, Team, or the user menu?
  [ ] Is the "Invite" button easy to find?
  [ ] Do they understand the role options (Viewer vs Editor vs Admin)?
  [ ] Do they select the correct role (Viewer) for the scenario?
  [ ] Is there confirmation that the invitation was sent?
  [ ] Do they feel confident it worked?

Post-task:
  "How easy was that?" (1–5)
  "Were the role options clear? Could you explain the difference
   between Viewer and Editor?"
```

### Post-Session Debrief (5 minutes)

```
"Great, that's all the tasks. Thank you for working through those.

Now I have a few overall questions:

1. What was your overall impression of the dashboard?
2. What was the easiest thing you did today? The hardest?
3. On a scale of 1-10, how likely would you recommend this to a
   colleague? [NPS question]
4. Is there anything you expected to find but didn't?
5. If you could change one thing about this dashboard, what would it be?
6. Any other thoughts or feedback?"
```

### Example Filled Report (for reference)

```
USABILITY TEST REPORT
─────────────────────────────────────────────
Product:         DataPulse Analytics Dashboard
Feature/Flow:    Core dashboard + reporting + team management
Test Date:       2025-03-15
Facilitator:     Alex Kim
Participants:    5 (3 marketing managers, 2 small business owners)
Prototype Level: Level 4 (High-fidelity Figma)
Environment:     Remote, desktop, Figma prototype via Maze
```

| # | Task | Success Rate | Avg Time | Errors | Critical | Key Quote | Recommendation |
|---|------|:---:|:---:|:---:|:---:|---|---|
| 1 | Understand dashboard | 5/5 (100%) | 18s | 0 | 0 | "Oh, revenue is front and center, that's what I need." | None — hierarchy working well |
| 2 | Change date range | 4/5 (80%) | 52s | 2 | 0 | "I was looking for a 'Q4' button but had to select dates manually." | Add quarter presets to date picker |
| 3 | Investigate conversion drop | 3/5 (60%) | 2m 15s | 4 | 1 | "I didn't realize I could click on the card to drill down." | Add explicit "View details" link on KPI cards |
| 4 | Create saved report | 2/5 (40%) | 3m 40s | 7 | 2 | "I don't understand the difference between a metric and a dimension." | Rename to "What to measure" / "How to group it". Add helper text. |
| 5 | Invite team member | 4/5 (80%) | 1m 05s | 1 | 0 | "Viewer makes sense, but I'm not sure what 'Editor' can change." | Add role description tooltips in the invite modal |

```
TOP FINDINGS
──────────────────────────────────────────────────────

Finding 1: Report builder uses technical jargon
  Severity:    Critical
  Tasks:       #4
  Frequency:   4/5 participants confused
  Description: Terms "metric" and "dimension" are not understood by
               non-technical users. 3 participants selected a dimension
               as a metric.
  Evidence:    "What's a dimension? Is that like a filter?" — P3
               "I just want to see revenue by channel, why is this
                so complicated?" — P5
  Root Cause:  BI terminology leaked into the UI
  Recommendation: Replace "Metrics" with "What to measure", replace
                  "Dimensions" with "Group by". Add contextual
                  examples under each section header.

Finding 2: KPI cards not obviously clickable
  Severity:    Major
  Tasks:       #3
  Frequency:   3/5 did not initially click the card
  Description: Users did not realize KPI cards are interactive.
               They looked in the sidebar for a "Conversions" page.
  Evidence:    "Can I click on these numbers?" — P2 (after 45s)
  Root Cause:  No hover state visible in Figma prototype. No visual
               affordance (arrow, underline, cursor change).
  Recommendation: Add hover state (elevation.md shadow + slight
                  bg shift). Add "View details" text link below
                  each KPI value. Use cursor: pointer.

Finding 3: Date picker lacks quarter presets
  Severity:    Minor
  Tasks:       #2
  Frequency:   2/5 looked for presets first
  Description: Users expected a "Q4" shortcut button but had to
               manually select October 1 and December 31.
  Evidence:    "I see 'Last 7 days' and 'Last 30 days' but no
                quarter option." — P1
  Root Cause:  Presets were limited to rolling windows, not calendar
               quarters
  Recommendation: Add preset buttons: "This Quarter", "Last Quarter",
                  "This Year", "Last Year" alongside existing presets.
```
