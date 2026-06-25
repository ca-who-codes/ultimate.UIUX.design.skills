# Dashboard & Data UI Playbook

> Purpose: A blueprint for app dashboards and data interfaces — the shell layout, the information hierarchy, KPI card design, visualization choice, filtering, and the discipline that makes a dashboard glanceable in five seconds.

**When to read this:** Building any authenticated data surface — analytics dashboard, admin panel, ops console, financial overview, SaaS home screen, monitoring view, or any screen whose job is "show me the state of things and let me act."

---

## The 5-second rule (the law of the dashboard)

A user must extract the **single most important answer** within five seconds of the dashboard loading. If they have to read, scroll, or hunt to learn whether things are okay, the dashboard has failed its core job.

This rule drives every decision below: what goes top-left, how big the hero KPI is, what gets a color, what gets hidden behind a click. Design for the *glance* first, the *scan* second, the *drill-down* third.

> Don't: 30 equally-sized charts in a uniform grid where nothing is more important than anything else.
> Do: one hero number, a row of supporting KPIs, then detail — a clear visual hierarchy that answers the headline question instantly.

---

## The shell: sidebar + topbar

The proven dashboard frame separates **navigation** (where can I go) from **context/actions** (where am I, what can I do) from **content** (the data).

```
┌──────────┬───────────────────────────────────────────────────────┐
│  LOGO    │  Page title        [⌕ search]    [date ▾] [⌗] [👤▾]   │ ◄ TOPBAR
│          ├───────────────────────────────────────────────────────┤   context +
│ ▸ Home   │                                                       │   global
│ ▸ Sales  │   ┌─────────────┐  ┌────────┐ ┌────────┐ ┌────────┐   │   actions
│ ▸ Users  │   │  HERO KPI   │  │ KPI    │ │ KPI    │ │ KPI    │   │
│ ▸ Reports│   │  $1.2M  ▲12%│  │ 4,201  │ │ 89%    │ │ 23m    │   │ ◄ KPI ROW
│          │   └─────────────┘  └────────┘ └────────┘ └────────┘   │   (top-left
│ ──────   │                                                       │    = most
│ ⚙ Settings│  ┌──────────────────────────┐ ┌───────────────────┐  │    important)
│ ? Help   │  │  TREND CHART (primary)   │ │  BREAKDOWN        │  │
│          │  │  ╱╲    ╱╲___              │ │  ▇▇▇▇ A           │  │ ◄ CONTENT
│          │  │ ╱  ╲__╱                   │ │  ▇▇▇  B           │  │
│ [collapse]│ └──────────────────────────┘ └───────────────────┘  │
│          │  ┌───────────────────────────────────────────────┐   │
│          │  │  DETAIL TABLE  (sortable, paginated)          │   │
│          │  └───────────────────────────────────────────────┘   │
└──────────┴───────────────────────────────────────────────────────┘
   NAV                          MAIN
```

**Shell rules:**
- **Left sidebar = navigation only.** Sections, not actions. Collapsible to icons on smaller viewports and for power users. Persist the open/closed state.
- **Topbar = page context + global controls.** Page title, global search, the date-range picker, notifications, account menu. These are scoped to the *whole* page.
- **Main = content,** organized by the inverted pyramid (below).
- Max content width is fine for reading layouts but data dashboards usually go **full-bleed** to use horizontal space for tables and multi-column charts.
- Keep chrome quiet: the data is the hero, not the navigation. Low-contrast sidebar, restrained accent use. See [../02-foundations/color.md](../02-foundations/color.md).

**When to use a topnav instead of a sidebar:** few sections (≤ 5), marketing-adjacent products, or when horizontal space is precious. Sidebar wins when nav is deep (sections + sub-sections) or when the product is a daily-driver tool where muscle memory matters.

---

## Inverted pyramid of information

Order content by decreasing importance, top to bottom, left to right. The headline answer first; the supporting detail next; the granular raw data last.

```
  ┌───────────────────────────────────────┐
  │            HERO KPI / status           │  ◄ "Are we okay?"  (1 number)
  ├───────────────────────────────────────┤
  │   supporting KPIs (3–5 stat cards)     │  ◄ "Why / what's moving?"
  ├───────────────────────────────────────┤
  │   trends & breakdowns (charts)         │  ◄ "How is it changing?"
  ├───────────────────────────────────────┤
  │   raw detail (tables, logs, rows)      │  ◄ "Show me the specifics"
  └───────────────────────────────────────┘
       importance ▼  density ▲  audience-narrowing ▼
```

The pyramid also maps to **audience**: executives stop at the top, managers read the middle, operators live in the bottom table. One dashboard can serve all three if it's layered this way — but if the audiences are truly different, build separate views rather than one compromise.

---

## What goes top-left (priority guide)

The top-left cell is the most valuable real estate on the screen — it's where the eye lands first (F-pattern) and what answers the 5-second question. Spend it on the **one metric the user opened the dashboard to check.**

| Dashboard type | Top-left hero metric |
|---|---|
| Revenue / finance | MRR or total revenue, with period delta |
| Sales pipeline | Pipeline value or deals-closing-this-period |
| Product analytics | Active users (DAU/WAU) or core-action count |
| Ops / infrastructure | System health / uptime / error rate (red when bad) |
| Support | Open tickets or SLA-breach count |
| Marketing | Conversions or qualified leads this period |
| E-commerce | Today's sales vs target |
| Personal finance | Net worth or available balance |

**Decision test:** "If the user could see only one number before the screen went dark, which would it be?" That number goes top-left, biggest, with a trend indicator. Everything else is supporting.

> Don't put a date-range picker or a "Welcome back, Sam!" banner in the top-left content slot. That's prime metric real estate, not greeting space.

---

## KPI / stat card design

The stat card is the dashboard's atom. A good one is glanceable, contextual, and honest.

```
┌─────────────────────────────┐
│  Monthly Revenue        ⓘ   │ ◄ label (what) + optional info tooltip
│                             │
│  $1,284,500          ▲12.4% │ ◄ value (big) + delta (vs comparison)
│                             │
│  ▁▂▃▅▇▆  vs last month      │ ◄ sparkline + comparison context
└─────────────────────────────┘
```

**Anatomy, in priority order:**
1. **Value** — the largest element. Format for humans: `$1.28M` not `1284500`. Use tabular/lining figures so digits align across cards.
2. **Label** — what this is, plain language. Above or below the value, smaller, lower contrast.
3. **Delta / trend** — change vs a comparison period, with direction (▲▼) *and* color. **Color must encode meaning, not direction** — a falling churn rate is *good* and should be green even though it's down. Never assume up = green.
4. **Context** — "vs last month", target, or a sparkline. A number without a baseline is noise.
5. **Tooltip (ⓘ)** — definition of the metric for anyone unsure how it's calculated.

**Rules:**
- 3–5 cards per row max; beyond that they shrink to illegibility.
- One card per concept. Don't cram three metrics into one card.
- Give the hero KPI more visual weight (size, span) than the supporting ones — uniform cards = no hierarchy.
- Show loading skeletons, not spinners, while values fetch. See [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md).
- Round intelligently. `89.2%` is fine; `89.23847%` is hostile.

> Don't show a delta without saying delta-of-what. "▲12%" is meaningless without "vs last month."

---

## Choosing the right visualization

Match the chart to the question. The most common dashboard mistake is the wrong chart type — a pie chart asked to do a line chart's job.

| Question / data shape | Use | Avoid |
|---|---|---|
| Change over time (trend) | **Line** or area chart | Bar (implies discrete categories) |
| Compare values across categories | **Bar / column** | Pie if > 3 slices |
| Part-to-whole, 2–4 parts | Pie / donut (sparingly) | Pie with 8 slices (use bar) |
| Composition over time | Stacked area / stacked bar | Too many stacks (>5 = mush) |
| Single number vs target | **Big number + delta**, or gauge/progress | A whole chart for one value |
| Distribution | Histogram, box plot | Line chart |
| Correlation (2 vars) | Scatter | Anything else |
| Ranked list / leaderboard | **Sorted bar or table** | Pie |
| Geographic | Choropleth / map | Only if geography is the point |
| Dense tabular detail | **Table** (sortable, filterable) | Charting what should be a table |

**Visualization rules:**
- Default to the simplest chart that answers the question. A big number beats a gauge; a sorted bar beats a pie.
- Label axes and units. A y-axis without a unit is a guess.
- Never start a bar chart's y-axis above zero — it lies about magnitude. Line charts may zoom, with the truncation made obvious.
- Limit a single chart to what the eye can parse: ~7 series max on a line chart, fewer is better. Use a legend or direct labels.
- Color carries meaning consistently across the whole dashboard — if blue = revenue in one chart, blue = revenue everywhere.
- Provide the data behind every chart (hover tooltip, "view as table", or export). Charts show shape; tables show truth.

> Don't use a pie chart to compare 8 categories. Humans can't judge angle differences — use a sorted bar.
> Don't use dual y-axes to imply a correlation that isn't there. It's a classic deception; split into two charts.

---

## Filters & date ranges

Filters turn a static report into an exploration tool. Make them obvious, sticky, and reversible.

**Date range** is the king filter on most dashboards — give it a dedicated, prominent control in the topbar.
- Offer presets: `Today · 7d · 30d · 90d · YTD · Custom`. Presets cover 90% of needs; custom handles the rest.
- Show the *active* range as a clear label, not just inside a closed dropdown.
- Default to the most useful range for the use case (often 30d), and **persist the user's last choice**.
- When a range is applied, every card and chart updates together — and the deltas recompute against the *comparable previous* period.

**Other filters (segment, region, product, status):**
- Place them in a filter bar directly above the content they affect.
- Show applied filters as removable chips: `[Region: EU ✕] [Status: Active ✕] [Clear all]`. The user must always see what's currently filtering the view.
- Filtering should feel instant — debounce, optimistic UI, skeletons; never a full-page reload.
- Make state shareable: encode filters in the URL so a filtered view can be sent to a teammate.

> Don't hide the active date range inside a collapsed control. A dashboard showing "last 7 days" data that *looks* like all-time data causes real decisions on wrong numbers.

---

## Density & scannability

Dashboards live or die by information density done *legibly*. Aim for high information-per-pixel without crossing into clutter.

- **Whitespace is structure, not waste.** Group related cards with spacing; separate sections with generous gaps. Spacing communicates relationships faster than borders. See [../02-foundations/layout-spacing.md](../02-foundations/layout-spacing.md).
- **Tabular figures everywhere.** Numbers in tables and cards must use lining/tabular numerals so columns align and the eye can scan down a column of digits.
- **Right-align numbers, left-align text** in tables. Always.
- **Restrained color.** Reserve saturated color for meaning — alerts, deltas, the one series that matters. A dashboard where everything is colorful is a dashboard where nothing stands out.
- **Consistent units and formats** across the whole surface. Don't mix `$1.2M` and `$1,200,000` and `1.2 million` on one screen.
- **One accent for "look here."** Use it for the hero metric, the breaching alert, the primary action — and nothing else.

> Don't draw a heavy border around every element. Borders add visual noise; whitespace and subtle background tints separate groups more cleanly.

---

## Drill-down patterns

A dashboard is an entry point, not a dead end. Every aggregate should let the user ask "why?" and go deeper.

| Pattern | When | How |
|---|---|---|
| **Click-through** | KPI → underlying records | Click "4,201 users" → filtered user list |
| **Chart → detail** | Data point → its rows | Click a spike in the line → that day's events |
| **Expand-in-place** | Quick detail without leaving | Row expands to show sub-rows / mini chart |
| **Side panel / drawer** | Inspect one item, keep context | Click a row → drawer slides in with full record |
| **Cross-filter** | Click one chart, filter others | Click "EU" segment → whole dashboard scopes to EU |
| **Breadcrumb drill** | Hierarchical data | Region → Country → City, with breadcrumb back |

**Rules:** make drillable things look clickable (hover affordance, cursor). Always provide a path back (breadcrumb, drawer close, "clear filter"). Preserve the user's filters and date range as they drill — losing context on a click is infuriating.

---

## Customization

Power users want their dashboard *their* way; new users want a sane default. Serve both.

- Ship a strong **default layout** first. Customization is a power-user reward, never a setup tax — never make the user build their dashboard before they can use it.
- Common customizations, in order of value: choose date range (always), show/hide cards, reorder via drag, pick metrics, save views/segments, set a comparison period.
- **Saved views** ("My team", "EU only", "This quarter") are higher-leverage than freeform drag-and-drop for most users.
- Persist customization per user. Respect it on return.
- Provide "reset to default" — customization that can't be undone traps users in a mess they made.

> Don't open onboarding with an empty canvas asking the user to assemble widgets. Give them a populated default; let them tweak later.

---

## Empty & first-run dashboards

The first-run dashboard is the highest-stakes screen for activation — and the easiest to botch with a sea of zeros. A brand-new account has no data, so "$0 · 0 users · 0%" everywhere reads as "this product is broken/empty."

**First-run rules:**
- Replace empty charts with **guidance**, not blank axes. Show what the chart *will* show plus the action to make it real.
- Lead with the **activating next step**: "Connect a data source" / "Invite your team" / "Create your first project" — the action that turns zeros into data.
- Use realistic **sample/preview data** clearly labeled as a demo, so the user sees the payoff before they've earned it. Label it unmistakably ("Sample data") so it's never mistaken for real.
- Provide a short setup **checklist** (see [auth-onboarding.md](./auth-onboarding.md)) so the path from empty to valuable is visible and finite.
- Degrade gracefully *per card*: a card with no data shows its own empty state ("No sales yet — they'll appear here"), not a crash or a misleading 0.

```
┌─────────────────────────────────────────────┐
│   No data yet                                │
│                                              │
│        📊  Your revenue will show up here    │
│            once you make your first sale.    │
│                                              │
│        [ Create your first product → ]       │
└─────────────────────────────────────────────┘
```

See [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md) for the full empty-state pattern.

---

## Real-time & refresh

- Show **when the data was last updated** ("Updated 2m ago") — staleness invisibly erodes trust.
- For live data, update smoothly (animate value changes), don't flash the whole screen. Sudden full reloads disorient.
- Offer manual refresh *and* sensible auto-refresh; let the user pause auto-refresh while they read.
- Never reset scroll position or lose the user's filters on a background refresh.

---

## Dashboard mistakes (the catalog)

> Don't make a uniform grid of same-sized charts. Without hierarchy, the 5-second answer is impossible to find.

> Don't chart what should be a number. A single value vs target is a big number with a delta, not a gauge or a one-bar bar chart.

> Don't assume up = green. Encode good/bad by meaning; falling churn is good and green.

> Don't truncate bar-chart y-axes. It exaggerates differences and misleads decisions.

> Don't hide the active date range or applied filters. Users will read filtered data as full data and act on it.

> Don't open new users into a wall of zeros. Replace empty with guidance and the activating next step.

> Don't over-color. Reserve saturated color for meaning; a rainbow dashboard has no focal point.

> Don't paginate so aggressively that scanning is impossible, nor dump 10,000 rows unvirtualized. Use sort, filter, search, and virtualized tables.

> Don't lose context on drill-down. Carry filters and date range through, and always offer a path back.

> Don't omit "last updated." Stale data presented as live is worse than no data.

---

## Related playbooks

- [auth-onboarding.md](./auth-onboarding.md) — first-run flow that fills the empty dashboard.
- [landing-marketing.md](./landing-marketing.md) — the page that drives signups to this product.
- [../03-components/data-display.md](../03-components/data-display.md) — sortable, dense data tables.
- [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md) — per-card and full empty states.
- [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md) — skeletons for cards and charts.
- [../02-foundations/color.md](../02-foundations/color.md) — semantic color for deltas and alerts.

---

## Agent checklist

- [ ] Identify the single 5-second question and place its answer top-left, largest, with a trend.
- [ ] Build the shell as sidebar (nav only) + topbar (context/date/account) + full-bleed content.
- [ ] Order content by the inverted pyramid: hero KPI → supporting KPIs → charts → detail tables.
- [ ] Design stat cards with value (big, human-formatted) + label + meaning-encoded delta + context.
- [ ] Encode delta color by good/bad, never by up/down direction.
- [ ] Match each chart to its question; default to the simplest type; never truncate bar y-axes.
- [ ] Give the date-range picker a prominent topbar slot with presets, persisted, deltas recomputed.
- [ ] Show applied filters as removable chips and keep the active range always visible.
- [ ] Make aggregates drillable with preserved filters and a clear path back.
- [ ] Replace first-run zeros with guidance, an activating next step, and labeled sample data.
- [ ] Use tabular figures, right-aligned numbers, restrained color, and whitespace-based grouping.
- [ ] Show "last updated" and refresh without resetting scroll or filters.
