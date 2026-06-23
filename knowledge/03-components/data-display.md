# Data Display

> Purpose: Present data so it's scannable and truthful — tables with correct alignment, the right chart for the question, lists, KPI cards, empty states, and large-dataset strategies.

**When to read this:** Before building any table, data grid, list, stat/KPI card, or chart — or when fixing misaligned numbers, the wrong chart type, missing empty states, or sluggish large datasets.

For tabular primitives (sorting controls, pagination, checkboxes) see [./components.md](./components.md). For color semantics and contrast see [../05-quality/accessibility.md](../05-quality/accessibility.md). For type scale / tabular numerals see [../02-foundations/typography.md](../02-foundations/typography.md).

---

## Tables

### Alignment rules (the most-broken thing in tables)

| Content | Align | Notes |
|---|---|---|
| Text (names, labels, descriptions) | **Left** | Ragged right; easiest to scan |
| Numbers (amounts, counts, %) | **Right** | So digits line up by place value |
| Numbers — use **tabular figures** | `font-variant-numeric: tabular-nums` | Equal-width digits so columns align |
| Currency | Right, align decimal points; symbol consistent | Consider aligning on the decimal |
| Dates | Left (or right if compared as magnitudes) | Consistent format (don't mix) |
| Booleans / status | Center (icon/badge) | Or left if a worded label |
| Actions (icons/buttons) | Right | Last column |
| **Column headers** | **Match their column's alignment** | Right-align numeric headers too |

```css
.cell-number { text-align: right; font-variant-numeric: tabular-nums; }
.cell-text   { text-align: left; }
```

### Anatomy & structure
- Use semantic `<table>` > `<thead>`/`<tbody>` > `<th scope="col">` / `<th scope="row">`. Don't fake tables with divs unless you reimplement the grid role/keyboard model.
- `<caption>` (can be visually hidden) names the table for SR users.

### Sticky headers
- For long tables, `position: sticky; top: 0` on `<thead>` so column meaning persists while scrolling. Add a bottom border/shadow when stuck. For wide tables, also sticky the first column (row identifier).

### Row density
- Offer density options for data-heavy apps: **comfortable** (~48–56px rows) vs **compact** (~32–40px). Compact fits more; comfortable is easier to scan. Default to comfortable, let power users go compact.
- Vertical padding, not font shrinking, drives density. Keep ≥12px horizontal cell padding.

### Zebra vs borders
- **Zebra striping** (alternating row tint) helps scan **wide** tables (many columns) by guiding the eye across.
- **Horizontal borders** (hairlines between rows) suffice for **narrow** tables and look cleaner.
- Don't use both heavily. Avoid full grid lines (every cell boxed) unless it's a spreadsheet — it adds noise. Maximize data, minimize chrome.

### Sorting
- Sortable headers: clickable, show direction (▲/▼) and current sort column; `aria-sort="ascending|descending|none"` on the `<th>`. Indicate the default sort. One sort column at a time unless you support multi-sort explicitly.

### Selection & bulk actions
- Row selection: checkbox in the first column, plus a header checkbox for select-all (with indeterminate state for partial). Selected rows get a tint + the row checkbox checked.
- When ≥1 row is selected, reveal a **bulk action bar** (sticky) showing "N selected" + actions (Delete, Export, Tag) + a clear-selection. Confirm destructive bulk actions.

### Responsive table strategies
Tables don't shrink gracefully. Pick a strategy:

| Strategy | When |
|---|---|
| **Horizontal scroll** (wrap in `overflow-x:auto`, sticky first col) | Many columns, all matter; data-grid feel |
| **Stack into cards** (each row → a labeled card) | Few-column tables on mobile; reading not comparing |
| **Hide low-priority columns** (progressive disclosure, "show more") | Some columns optional |
| **Priority columns + expand row** | Show 2–3 key columns, tap to expand details |

Never let a table cause whole-page horizontal scroll — scope the scroll to the table container.

### Top 3 table mistakes
1. **Numbers left-aligned / non-tabular** so columns don't line up — hardest data to read.
2. **No responsive strategy** → table overflows and breaks the page on mobile.
3. **Faking tables with divs** and losing headers/scope/keyboard semantics.

---

## Data grids

- A data grid = table + interaction: in-cell editing, frozen rows/cols, virtualization, resizable/reorderable columns, cell selection.
- Use `role="grid"` and implement the **grid keyboard model**: arrow keys move cell focus, Enter/F2 edits, Escape cancels, Home/End, Tab exits the grid. This is a real commitment — use a vetted library (TanStack Table/Virtual, AG Grid) rather than hand-rolling.
- **Virtualize** rows (and columns if many) for >~100s of rows to keep scrolling smooth — render only visible rows. See "Large datasets".

---

## Lists

- Use lists (not tables) when each item is an **entity with a few attributes** read top-to-bottom, not compared column-by-column (inbox, notifications, search results, feeds).
- Each row: clear primary text, secondary/meta text, optional leading avatar/icon and trailing action/metadata. Consistent height or predictable variation.
- Make the whole row a target where appropriate; keep nested actions reachable. Provide dividers or spacing for separation.
- For long lists: virtualize; add sticky section headers for grouped lists (A–Z, by date).

---

## Stat / KPI cards

Anatomy: `[ label ][ big value ][ delta vs prior (↑/↓ + %) ][ optional sparkline/context ]`.

- The **value** is the hero — largest, boldest; use tabular figures so it doesn't jiggle on live update.
- **Always give context**: a raw "1,284" means little. Show change vs previous period, a target, or a trend sparkline.
- **Delta semantics**: color + arrow + sign, and remember up isn't always good (rising churn/cost is bad) — color by *good/bad*, not by *up/down*, and never by color alone (include arrow + label).
- Group related KPIs in an even grid; keep label position and number formatting consistent across cards.

Top 3 mistakes:
1. A number with no comparison/context.
2. Green-for-up even when up is bad (cost, errors, churn).
3. Misaligned/jumping digits (non-tabular numerals on live data).

---

## Charts

### Chart-selection decision table

| You want to show | Use | Avoid |
|---|---|---|
| Trend over time (continuous) | **Line** | Pie, donut |
| Compare values across categories | **Bar** (horizontal if labels long / many categories) | 3D bars |
| Part-to-whole, exact comparison | **Stacked bar** or just a table | Pie (hard to compare slices) |
| Part-to-whole, 2–3 parts, rough | A single pie/donut **only if ≤3 slices and proportions matter** | Pie with >5 slices |
| Cumulative total over time | **Area** (1 series) | Overlapping filled areas (>2 series) |
| Relationship between 2 variables | **Scatter** | Line (implies order) |
| Distribution of one variable | **Histogram / box plot** | Pie |
| Ranking | **Sorted horizontal bar** | Unsorted bars |
| Precise values / many series | **Table** (a table is a valid "chart") | Cramming into one chart |

**Avoid pie charts** for anything but the simplest part-to-whole with ≤3 slices — humans compare angles poorly; a sorted bar is almost always clearer. Never use a pie to compare across categories or over time.

### Chart hygiene
- **Maximize data-ink**: remove chartjunk — heavy gridlines, backgrounds, 3D, drop shadows, redundant borders. Light gridlines only where they aid reading.
- **Label directly over legends** where possible: put the series name at the end of its line, or value labels on bars. Legends force a constant eye round-trip and color-matching (a colorblind hazard). If you must use a legend, also differentiate by line style/marker/pattern, not color alone.
- **Start bar-chart y-axis at zero** (truncating exaggerates differences and misleads). Line charts may use a non-zero baseline when showing change, but label it.
- **Limit series**: ~5 lines / categories max before it's spaghetti; group or small-multiple instead.
- **Order categorical bars by value** (not alphabetically) unless order is meaningful (time, size buckets).
- **Format numbers**: abbreviate axes (1.2k, 3M), consistent decimals, units stated.
- **Accessibility**: don't rely on color alone (use patterns/markers/direct labels); provide a text/table alternative; give the chart an accessible name/description; ensure ≥3:1 contrast for meaningful colors. See [../05-quality/accessibility.md](../05-quality/accessibility.md).

Top 3 chart mistakes:
1. **Pie chart** for comparison or many slices.
2. **Truncated bar-chart axis** that lies about differences.
3. **Legend-only** color encoding that's a round-trip and fails colorblind users.

---

## Empty states

Every collection view needs a designed empty state — it's a first impression and a guidance moment, not an error.

| Empty cause | Show |
|---|---|
| **First use** (nothing yet) | Friendly illustration + one line of value + a **primary CTA** ("Create your first project") |
| **No search/filter results** | "No results for '…'" + suggestions (clear filters, broaden, check spelling) + clear-filters button |
| **Error loading** | Distinct from empty: explain + **Retry** + don't pretend it's empty |
| **Permissions/empty by design** | Explain why and what to do |

- Don't ship a blank panel or a lone spinner-that-finished-to-nothing.
- Keep it short, actionable, on-brand; the CTA is the point. Differentiate "no data yet" from "load failed" from "no matches."

Top 3 empty-state mistakes:
1. No empty state at all (blank screen, looks broken).
2. Same UI for "no results" and "error" — user can't tell if retry helps.
3. Cute illustration but **no next action**.

---

## Handling large datasets

### Pagination vs infinite scroll vs load-more

| Pattern | Best for | Pros | Cons |
|---|---|---|---|
| **Pagination** | Tables, search results, anything users navigate/reference/bookmark | Sense of scope ("page 4 of 50"), reachable footer, shareable URLs, predictable | Extra clicks; context switch per page |
| **Infinite scroll** | Exploratory feeds (social, media discovery) | Effortless browsing, engagement | **Footer unreachable**, no sense of position, bad for "find again", memory/perf, SR-hostile |
| **Load more** (button) | Feeds where you want a footer + control | User-controlled, footer reachable, keyboard/SR friendly | Still grows the DOM |

Guidance:
- **Default to pagination** for tables, dashboards, and anything users compare/reference/return to.
- Use **infinite scroll only** for casual discovery feeds — and even then prefer **"Load more"** so the footer stays reachable and it's accessible.
- **Never** put important controls (footer links, contact) below an infinite-scroll list.

### Performance for big data
- **Virtualize** long lists/tables (render only visible rows + a small overscan) — TanStack Virtual, react-window. Essential beyond a few hundred rows.
- **Server-side paginate/sort/filter** for large datasets; don't ship 50k rows to the client.
- **Debounce** search/filter input (~250–400ms); cancel stale requests.
- Show **skeletons** for initial load, inline spinners for "load more"; keep layout stable to avoid CLS.
- Keep a stable sort key so pagination/virtualization doesn't reshuffle rows mid-scroll.

---

## Agent checklist

- [ ] Left-align text, right-align numbers, and apply `font-variant-numeric: tabular-nums` to all numeric columns.
- [ ] Align each column header the same way as its data; keep date/number formats consistent.
- [ ] Use semantic `<table>` with `<th scope>` and a `<caption>`; add sticky header (and first column when wide).
- [ ] Choose a deliberate responsive table strategy (scroll / stack / hide) so it never breaks the page on mobile.
- [ ] Provide row selection with a select-all (indeterminate) checkbox and a sticky bulk-action bar; confirm destructive bulk actions.
- [ ] Make sortable headers show direction and set `aria-sort`; default to a sensible sort.
- [ ] Pick charts from the decision table; avoid pie charts beyond ≤3 slices and never for comparison/time.
- [ ] Start bar-chart axes at zero, order bars by value, cap at ~5 series, and label series directly over using legends.
- [ ] Never encode chart meaning by color alone; provide a table/text alternative and an accessible name.
- [ ] Give KPI cards context (delta/target/sparkline), color deltas by good/bad not up/down, and use tabular figures.
- [ ] Design distinct empty states for first-use, no-results, and load-error — each with a clear next action.
- [ ] Default to pagination for tables/reference data; use load-more over infinite scroll; virtualize and server-paginate large datasets.
