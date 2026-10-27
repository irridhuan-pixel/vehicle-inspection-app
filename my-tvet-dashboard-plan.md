# MY TVET Dashboard — Plan

## Top-Level Overview

**Goal:** Build a single-file, responsive web application page titled "MY TVET Dashboard" using plain HTML, CSS, and vanilla JavaScript — no frameworks, no build tools, openable directly in any browser.

**Scope:**
- A top header/nav bar with the dashboard title
- Three summary cards: Students Registered, Tasks Remaining, Workshop Status
- An interactive Preparation Checklist with checkboxes that update the Tasks Remaining card in real time
- Realistic sample data baked into the HTML/JS
- Fully responsive layout (mobile, tablet, desktop)

**Non-Goals:**
- No backend, no API calls, no data persistence across page reloads
- No external CSS frameworks (e.g. Bootstrap, Tailwind)
- No JavaScript frameworks (e.g. React, Vue)

**Output:** A single file — `index.html` — that opens correctly in any modern browser.

---

## Sub-Tasks

---

### Sub-Task 1 — Page Shell and Responsive CSS Foundation

**Intent:**
Establish the single-file HTML skeleton and the global CSS that will govern layout, colour palette, typography, and responsive breakpoints. Everything else builds on top of this layer.

**Expected Outcomes:**
- `index.html` exists and opens in a browser without errors
- The page has a visible header bar with the title "MY TVET Dashboard"
- A responsive CSS grid/flex layout is defined, ready to receive cards and the checklist
- Colour palette, font, spacing tokens are set as CSS custom properties (variables) so they can be reused consistently

**Todo List:**
1. Create `index.html` with standard HTML5 boilerplate (`<!DOCTYPE html>`, `<meta charset>`, `<meta name="viewport">`)
2. Add a `<style>` block in `<head>` with CSS custom properties for colour, font, spacing
3. Define the top header bar (`<header>`) with the title and a subtle TVET-themed accent colour
4. Define a main content wrapper with a responsive CSS Grid layout (single column on mobile, two columns on tablet, three columns on desktop)
5. Verify the page renders correctly at 375px, 768px, and 1280px viewport widths

**Relevant Context:**
- No external stylesheets; all CSS lives in the `<style>` block of `index.html`
- Breakpoints: mobile < 600px, tablet 600–1024px, desktop > 1024px
- Suggested palette: deep teal header (#1a4f6e), white card backgrounds, light grey page background (#f4f6f8), accent green for positive status (#2e7d32)

**Status:** [x] done

---

### Sub-Task 2 — Three Summary Cards

**Intent:**
Render the three at-a-glance metric cards with realistic sample data and distinct visual styling to communicate workshop health instantly.

**Expected Outcomes:**
- Three cards appear in a horizontal row on desktop, stacking vertically on mobile
- Each card shows: an icon, a label, a primary metric, and a short supporting sub-text
- The "Workshop Status" card uses a colour-coded badge (e.g. green = On Track, amber = At Risk)
- The "Tasks Remaining" card displays a live count that will be updated by Sub-Task 4

**Sample Data:**

| Card | Primary Metric | Sub-text |
|---|---|---|
| Students Registered | 34 / 40 | 6 seats remaining · Registration closes in 3 days |
| Tasks Remaining | 5 / 12 | 2 tasks overdue |
| Workshop Status | On Track | Next session: 24 July 2025 · Room B104 |

**Todo List:**
1. Add a `<section class="cards-grid">` containing three `<div class="card">` elements
2. Each card gets an icon (Unicode emoji or inline SVG), a `<h2>` label, a `<p class="metric">` for the big number, and a `<p class="sub-text">` for supporting detail
3. For "Tasks Remaining", give the metric element an `id="tasks-remaining-count"` so JavaScript can update it
4. For "Workshop Status", add a `<span class="badge badge--green">On Track</span>` element
5. Style cards with box shadow, rounded corners, and a left border accent in the card's theme colour

**Relevant Context:**
- The `id="tasks-remaining-count"` created here is consumed by the checklist logic in Sub-Task 4
- Card theme colours: Students = teal, Tasks = amber, Status = green

**Status:** [x] done

---

### Sub-Task 3 — Preparation Checklist UI

**Intent:**
Build the visual structure and styling of the Preparation Checklist section, with all checklist items rendered and styled, before wiring up interactivity.

**Expected Outcomes:**
- A clearly labelled "Preparation Checklist" section appears below the summary cards
- 12 checklist items are visible, each with a checkbox, label text, an owner tag, and a due-date tag
- Items that are pre-checked appear visually struck through
- The section has a progress bar showing completion out of total items

**Sample Checklist Items (12 total, 7 pre-completed):**

| # | Task | Owner | Due | Pre-checked |
|---|---|---|---|---|
| 1 | Confirm venue booking and room layout | Admin | 18 Jul | Yes |
| 2 | Send registration confirmation emails | Admin | 19 Jul | Yes |
| 3 | Prepare student handout materials | Facilitator | 20 Jul | Yes |
| 4 | Test projector and audio equipment | Tech Lead | 21 Jul | Yes |
| 5 | Set up digital check-in system | Tech Lead | 21 Jul | Yes |
| 6 | Assign roles to co-facilitators | Facilitator | 22 Jul | Yes |
| 7 | Confirm catering / refreshments order | Admin | 22 Jul | Yes |
| 8 | Upload course slides to shared drive | Facilitator | 23 Jul | No |
| 9 | Brief all facilitators on session flow | Lead Trainer | 23 Jul | No |
| 10 | Print attendance and feedback forms | Admin | 24 Jul | No |
| 11 | Conduct final room setup walkthrough | Tech Lead | 24 Jul | No |
| 12 | Prepare post-workshop survey link | Admin | 24 Jul | No |

**Todo List:**
1. Add a `<section class="checklist-section">` below the cards grid
2. Add a progress bar element (`<div class="progress-bar">` with an inner fill `<div id="checklist-progress-fill">`) and a label (`<span id="checklist-progress-label">7 / 12 complete</span>`)
3. Render a `<ul class="checklist">` with 12 `<li>` items; each item contains: `<input type="checkbox">`, `<label>`, `<span class="tag tag--owner">`, `<span class="tag tag--due">`
4. Pre-check the first 7 items in HTML (`checked` attribute)
5. Style checked items with `text-decoration: line-through` and reduced opacity using the CSS `:checked + label` selector
6. Style overdue due-date tags in amber/red to visually flag items due before today's date

**Relevant Context:**
- The progress bar fill element and label created here are driven by the JS logic in Sub-Task 4
- The 5 unchecked items (items 8–12) correspond to "Tasks Remaining: 5" shown in the summary card

**Status:** [x] done

---

### Sub-Task 4 — Interactivity and Live Card Updates

**Intent:**
Wire up vanilla JavaScript so that toggling a checklist item immediately updates the Tasks Remaining card count and the checklist progress bar — making the dashboard feel live and responsive to user actions.

**Expected Outcomes:**
- Checking an unchecked item decrements the "Tasks Remaining" count by 1 and advances the progress bar
- Unchecking a checked item increments "Tasks Remaining" by 1 and retreats the progress bar
- The progress bar fill width animates smoothly via CSS transition
- The progress label text updates to reflect the new X / 12 fraction
- No page reload is required for any of these updates

**Todo List:**
1. Add a `<script>` block at the bottom of `<body>`
2. Write an `initDashboard()` function that runs on `DOMContentLoaded`
3. Inside `initDashboard()`, select all checklist checkboxes and attach a `change` event listener to each
4. On each `change` event, recount total checked items and compute remaining = 12 - checked
5. Update `#tasks-remaining-count` text to the new remaining value
6. Update `#checklist-progress-fill` width as a percentage: `(checked / 12) * 100 + '%'`
7. Update `#checklist-progress-label` text to `checked + ' / 12 complete'`
8. Add a CSS `transition: width 0.3s ease` on the progress bar fill for smooth animation

**Relevant Context:**
- Depends on `id="tasks-remaining-count"` from Sub-Task 2 and the progress elements from Sub-Task 3
- All logic is self-contained in one `<script>` block — no modules, no imports

**Status:** [x] done

---

### Sub-Task 5 — Responsive Polish and Final Validation

**Intent:**
Ensure the finished page looks polished and correct at all viewport sizes, with consistent spacing, readable typography, and no layout breakage.

**Expected Outcomes:**
- Cards stack to a single column on mobile without overflow
- Checklist items wrap cleanly on narrow screens (owner/due tags drop below label if needed)
- Header title remains readable and centred/aligned at all widths
- Progress bar renders correctly at all widths
- Page passes a manual visual check at 375px, 768px, 1280px

**Todo List:**
1. Review and adjust CSS Grid column definitions for the cards section at each breakpoint
2. Ensure checklist `<li>` items use `flex-wrap: wrap` so tags flow correctly on small screens
3. Add `max-width` to the main content wrapper to prevent over-stretching on ultra-wide screens
4. Confirm all touch targets (checkboxes + labels) are at least 44px tall for mobile usability
5. Do a final read-through of the complete `index.html` to remove any stray debug code or inconsistencies

**Relevant Context:**
- This is a polish-only sub-task; no new features are added
- All changes are CSS-only unless a layout bug requires a minor HTML tweak

**Status:** [x] done

---

## File Inventory

| File | Purpose |
|---|---|
| `index.html` | Single deliverable — contains all HTML structure, `<style>` CSS, and `<script>` JS |
| `my-tvet-dashboard-plan.md` | This plan file |
