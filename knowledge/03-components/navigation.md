# Navigation

> Purpose: Choose and build the right navigation pattern for an app's structure — top nav, sidebar, tab bar, breadcrumbs, command palette, mega menu — with correct active state, depth limits, and responsive behavior.

**When to read this:** Before designing an app's nav shell, adding a new section, or fixing mobile nav, active-state ambiguity, or too-deep menu hierarchies.

For nav primitives (tabs, dropdown menus, pagination) see [./components.md](./components.md). For skip links / focus / landmarks see [../05-quality/accessibility.md](../05-quality/accessibility.md). For responsive breakpoints see [../05-quality/responsive.md](../05-quality/responsive.md).

---

## Pattern → app-type decision table

| App type | Primary nav | Why |
|---|---|---|
| Marketing / content site | **Top horizontal nav** (+ mega menu if many categories) | Few top-level items, brand-forward, familiar |
| SaaS dashboard / admin | **Left sidebar** (collapsible) | Many sections, deep features, vertical scales better than horizontal |
| Mobile app (native-feel) | **Bottom tab bar** (≤5 items) | Thumb-reachable, persistent, fast switching |
| Productivity / power tool | Sidebar **+ command palette (⌘K)** | Mouse for discovery, keyboard for speed |
| E-commerce | Top nav + **mega menu** + persistent **search** | Large catalog, browse + search both critical |
| Docs / knowledge base | Left sidebar (tree) + breadcrumbs + on-page TOC | Deep hierarchy, need location + within-page nav |
| Linear flow (checkout, onboarding) | **Stepper** (not free nav) | Constrain to the path; show progress |
| Settings | Sub-sidebar / vertical tabs within the section | Many flat sub-pages under one area |

Rule: pick by **number of destinations** and **device**. ≤5 destinations → tabs/bottom bar. 5–7 → top nav. Many/grouped → sidebar. Power users → add a command palette on top of any of these.

---

## Top navigation

- Best for ≤7 primary destinations. Logo left, nav center/left, account/CTA right.
- Each item is a real `<a>`; current item gets `aria-current="page"` and a visible indicator.
- Overflow into a "More" dropdown rather than shrinking text or wrapping to two rows.
- Wrap in `<nav aria-label="Primary">`. Multiple navs each need a distinct `aria-label`.

## Sidebar navigation

- Best for many sections / deep apps. Supports grouping with section headers, icons + labels, and nested (one level) expand/collapse.
- **Collapsible**: full (icon + label) ⇄ rail (icon-only, label on hover/tooltip). Persist the user's choice.
- Keep it ≤2 levels deep in the sidebar; deeper structure belongs on the page (sub-nav/tabs), not nested 3+ in the rail.
- Active item: filled/tinted background + accent left-border or text, `aria-current="page"`.

## Tab bar (in-app section switcher)

- Switches views **within the same context** (e.g. a profile's Posts/Replies/Media). Not for routing between unrelated app areas.
- Use proper tab semantics only if it's view-switching; if each "tab" is a separate URL/page, use links + `aria-current`, not `role=tab`. See [./components.md](./components.md).

## Breadcrumbs

- Show **location in a hierarchy** for deep structures (docs, catalog, file trees): `Home › Category › Subcategory › Current`.
- Last item = current page, not a link, `aria-current="page"`. Wrap in `<nav aria-label="Breadcrumb">` with an ordered list.
- Don't use breadcrumbs as primary nav, and don't show them for flat 1–2 level sites.
- Truncate long trails in the middle (`Home › … › Current`) on small screens.

## Command palette (⌘K / Ctrl+K)

- A searchable launcher for navigation + actions; accelerator for power users, **never the only way** to reach something.
- Fuzzy search across pages, commands, recent items; group results; keyboard-first (arrows, Enter, Escape); show shortcuts.
- It's a modal dialog: focus trap, Escape closes, focus returns to trigger. `role="dialog"` + a searchable `listbox`.

## Mega menu

- For large, browsable taxonomies (e-commerce categories, big product suites). A panel of grouped links under a top-nav item.
- Open on **click/tap or intentional hover with delay** (avoid menus that snap open on accidental hover). Keyboard accessible: items in tab order, Escape closes.
- Organize into labeled columns; don't dump 80 links flat. Keep it shallow (the menu shows the structure, the user doesn't drill in).

---

## Mobile navigation

### Hamburger menu — tradeoffs
- Pros: saves space, holds many items. Cons: **hides nav behind a click → lower discovery and engagement** of hidden items. "Out of sight, out of mind."
- Use it for **secondary/overflow** items, or when you genuinely have many destinations. Keep your top 3–5 most important destinations visible (e.g. in a bottom bar) and put the rest behind the menu.
- The toggle: `aria-expanded`, `aria-controls`, `aria-label="Menu"`, ≥44×44px. Opens a drawer with a focus trap and Escape to close.

### Bottom tab bar (mobile)
- **Max 5 items** (4 is often better) — thumb-reachable, always visible, fast. Each: icon **+ short label** (icon-only hurts comprehension).
- Use for the top-level destinations only. If you need a 6th, use a "More" tab.
- Mark the active tab with filled icon + accent color + `aria-current="page"` — not color alone.
- Respect safe areas (`env(safe-area-inset-bottom)`) so it clears the home indicator.

### Responsive nav transformation
- Top nav (desktop) → hamburger drawer or bottom bar (mobile).
- Sidebar (desktop) → off-canvas drawer or bottom bar (mobile); collapse to rail at medium widths.
- Don't just shrink desktop nav; **transform** it to the device-appropriate pattern at sensible breakpoints. See [../05-quality/responsive.md](../05-quality/responsive.md).

---

## Active / current state indication

- **Always** show where the user is. Use `aria-current="page"` for the current page link, `aria-current="step"` in a stepper, `aria-current="location"` in a partially-matching section.
- Visual cue must be **multi-channel**: background tint + bold/weight + accent border/underline + (optionally) filled icon — never color alone (fails colorblind users and low contrast).
- For nested nav, indicate both the active leaf and its active parent/section.

```tsx
<a href="/reports"
   aria-current={isActive ? "page" : undefined}
   className={cn(
     "flex items-center gap-2 rounded-md px-3 py-2 text-sm",
     isActive
       ? "bg-blue-50 font-semibold text-blue-700 border-l-2 border-blue-600"
       : "text-gray-600 hover:bg-gray-50"
   )}>
  <ReportIcon className="h-4 w-4" aria-hidden /> Reports
</a>
```

---

## Information architecture basics

- **Group by user mental model**, not your org chart or database schema. Card-sort if unsure.
- Labels: short, concrete, jargon-free, and mutually exclusive (a destination shouldn't plausibly fit two top-level buckets).
- **Breadth over depth**: a wider, shallower tree beats a narrow deep one. Users find things faster with more visible top-level choices than with many nested clicks.
- The most important / most frequent destinations get the most prominent placement.
- Keep navigation **consistent across pages** — same items, same order, same active styling.

## Nav hierarchy depth limits

- **Cap primary navigation at ~3 levels** (e.g. Section → Subsection → Page). Beyond that, users get lost and breadcrumbs become essential.
- If you need depth 4+, reconsider IA (flatten, regroup) or use on-page navigation (sub-tabs, in-page TOC) instead of more nested menus.
- Each level should be reachable and show its trail; never bury a key feature 4 clicks deep.

---

## Sticky headers

- Keep the primary nav/header accessible on scroll for long pages — reduces "scroll back to top" friction.
- Keep it **slim** (don't eat vertical space, especially on mobile); consider hide-on-scroll-down / show-on-scroll-up.
- Account for sticky height when anchoring: add `scroll-margin-top` to anchor targets so headings aren't hidden behind the header.
- Don't stack multiple sticky bars; one is enough.

---

## Skip links

- Provide a **"Skip to main content"** link as the first focusable element, visually hidden until focused, jumping to `#main`. Essential so keyboard/SR users bypass the nav on every page.
- Use landmark roles/elements: `<header>`, `<nav>`, `<main>`, `<footer>` so AT users can jump by region. See [../05-quality/accessibility.md](../05-quality/accessibility.md).

```html
<a href="#main" class="sr-only focus:not-sr-only focus:absolute focus:left-4 focus:top-4
                       focus:z-50 focus:rounded focus:bg-white focus:px-4 focus:py-2">
  Skip to main content
</a>
```

---

## Search placement

- For search-heavy products (e-commerce, docs, data apps), put a **persistent, prominent search field** in the header — don't hide it behind an icon on desktop.
- On mobile, an icon that expands to a full-width field is acceptable, but keep it one tap away.
- Pair with a command palette for power users.
- Search input: `type="search"`, clear button, `role="searchbox"` or labeled, results with keyboard navigation and an empty/"no results" state.

---

## Top mistakes (cross-cutting)

1. **No visible current state** — user can't tell where they are.
2. **Color-only active indicator** — inaccessible.
3. **Burying primary destinations in a hamburger** on mobile, tanking engagement.
4. **>5 items in a bottom tab bar** or icon-only tabs with no labels.
5. **Hierarchy deeper than 3 levels** with no breadcrumbs.
6. **Shrinking** desktop nav onto mobile instead of transforming it.
7. **`<div onClick>` "links"** — not focusable, no `aria-current`, breaks back button.

---

## Agent checklist

- [ ] Pick the nav pattern from the decision table by destination count and device; don't default to a hamburger.
- [ ] Cap bottom tab bars at 5 items, each with an icon AND a short label.
- [ ] Always indicate the current location with `aria-current` plus a multi-channel visual cue (never color alone).
- [ ] Use real `<a>` elements inside `<nav aria-label="…">` landmarks; give each nav a distinct label.
- [ ] Keep primary hierarchy ≤3 levels deep and add breadcrumbs for deep structures.
- [ ] Transform (not shrink) nav across breakpoints: top/sidebar → drawer or bottom bar on mobile.
- [ ] Keep the top 3–5 destinations visible on mobile; relegate only secondary items behind a hamburger.
- [ ] Add a focusable "Skip to main content" link as the first element and use header/nav/main/footer landmarks.
- [ ] Make the menu toggle ≥44×44px with `aria-expanded`/`aria-controls`, and trap focus in the opened drawer.
- [ ] Keep sticky headers slim and add `scroll-margin-top` to anchor targets so they aren't hidden.
- [ ] Add a command palette (⌘K) as an accelerator for power tools, never as the only path to a destination.
- [ ] Give search a prominent, labeled field with a "no results" state for search-heavy apps.
