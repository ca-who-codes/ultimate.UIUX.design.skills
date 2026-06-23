# Layout & Spacing

> Purpose: Compose balanced, responsive layouts on a consistent grid — the 8pt system, container widths, breakpoints, Grid vs Flexbox, layout archetypes, alignment, and z-index.

**When to read this:** Before structuring a page, choosing Grid vs Flexbox, setting spacing/gutters, or fixing cramped/unbalanced layouts. Pair with [typography.md](./typography.md) for vertical rhythm and [design-tokens.md](./design-tokens.md) for spacing tokens.

---

## The 8pt grid (with 4px sub-grid)

Size and space everything in **multiples of 8px**, dropping to a **4px sub-grid** for fine adjustments (icon gaps, tight padding). This keeps spacing consistent and aligns to most device pixel densities.

| Why 8 | Effect |
|-------|--------|
| Divisible, scales cleanly | 8 → 16 → 24 → 32 at any zoom/DPR |
| Limited vocabulary | Fewer decisions, fewer one-off values |
| Aligns components | Heights (32/40/48), paddings, gaps all snap together |

**Do:** snap padding, margin, gap, width, height to 4/8 multiples.
**Don't:** ship `padding: 13px` or `margin: 7px` — they break rhythm.

---

## Spacing scale

| Token | px | rem | Tailwind | Typical use |
|-------|----|----|----------|-------------|
| 0.5 | 2 | 0.125 | `p-0.5` | hairline nudge |
| 1 | 4 | 0.25 | `p-1` | icon/text gap (sub-grid) |
| 2 | 8 | 0.5 | `p-2` | tight inner padding |
| 3 | 12 | 0.75 | `p-3` | compact control padding |
| 4 | 16 | 1 | `p-4` | **default element padding** |
| 6 | 24 | 1.5 | `p-6` | card padding, group gaps |
| 8 | 32 | 2 | `p-8` | section inner padding |
| 12 | 48 | 3 | `p-12` | between content blocks |
| 16 | 64 | 4 | `p-16` | section spacing |
| 24 | 96 | 6 | `p-24` | large section / hero |

**Do:** use a step or two of contrast between nested spacings (16 inside, 32 around).
**Don't:** use the same gap for every level — hierarchy disappears.

---

## Whitespace as a design tool

Whitespace is not "empty" — it does work:

- **Proximity** groups related items; distance separates them (Gestalt). A label 4px from its input but 24px from the next field reads as a unit.
- **Macro whitespace** (between sections) sets pace and signals premium/clean.
- **Micro whitespace** (line-height, padding) drives readability.
- More whitespace ⇒ perceived higher quality; cramped ⇒ cheap/stressful.

**Do:** increase spacing *between* groups more than *within* groups.
**Don't:** fill every pixel — let key elements breathe.

---

## Density vs comfort

| Mode | Row height | Padding | Use |
|------|-----------|---------|-----|
| **Comfortable** | 48–56px | `p-4`/`p-6` | Consumer apps, marketing, onboarding |
| **Compact** | 36–40px | `p-2`/`p-3` | Dashboards, tables, power-user tools |
| **Dense** | 28–32px | `p-1`/`p-2` | Data grids, IDEs, finance terminals |

Offer a density toggle for data-heavy products. Keep **touch targets ≥ 44×44px** regardless of density on touch devices.

**Do:** match density to the user's task — analysts want dense, first-timers want comfortable.
**Don't:** cram a consumer onboarding flow into a dense grid.

---

## Container widths & max-widths

| Content type | Max-width | Why |
|--------------|-----------|-----|
| Prose / article | **~65ch (≈ 700px)** | Optimal reading measure (see typography) |
| App content | **1200–1280px** | Comfortable on laptops, not edge-to-edge on desktop |
| Wide dashboard | 1440–1536px | Data needs room |
| Full-bleed | 100% | Hero images, maps, backgrounds |

```html
<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">…</div>  <!-- 80rem = 1280px -->
<article class="mx-auto max-w-[65ch] px-4">…</article>
```

Always pair `max-width` with horizontal padding so content never touches viewport edges on mobile.

**Do:** center content in a capped container with responsive side padding.
**Don't:** stretch text the full width of an ultrawide monitor.

---

## Responsive breakpoints

Mobile-first: write base styles for small screens, layer `min-width` overrides up.

| Name | Min-width | Tailwind prefix | Targets |
|------|-----------|-----------------|---------|
| (base) | 0 | — | phones |
| sm | **640px** | `sm:` | large phones / small tablets |
| md | **768px** | `md:` | tablets |
| lg | **1024px** | `lg:` | laptops / small desktops |
| xl | **1280px** | `xl:` | desktops |
| 2xl | **1536px** | `2xl:` | large desktops |

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">…</div>
```

**Do:** design the smallest screen first, then enhance upward.
**Don't:** add a breakpoint for every device — design for ranges, not models.

---

## 12-column grid

12 divides into 2, 3, 4, 6 — flexible for halves, thirds, quarters.

```html
<div class="grid grid-cols-12 gap-6">
  <main class="col-span-12 lg:col-span-8">…</main>
  <aside class="col-span-12 lg:col-span-4">…</aside>
</div>
```

Use consistent **gutters** (the gap between columns): 16px (compact) / 24px (default) / 32px (spacious). Keep gutters equal to or proportional to your spacing scale.

**Do:** keep all gutters in a layout identical.
**Don't:** mix 20px and 24px gaps in the same grid.

---

## CSS Grid vs Flexbox — decision rules

| Use **Grid** when… | Use **Flexbox** when… |
|--------------------|------------------------|
| 2D layout (rows **and** columns) | 1D layout (a single row or column) |
| Page/section scaffolding | Distributing items in a toolbar/navbar |
| Explicit track sizes / overlap | Content-driven sizing, wrapping chips |
| Card galleries, dashboards, bento | Buttons, form rows, list items |
| You need named areas | You need `gap` + alignment of a line |

```css
/* Grid: 2D, responsive without media queries */
.cards { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1.5rem; }

/* Flex: 1D row, push actions to the right */
.toolbar { display: flex; align-items: center; gap: .75rem; }
.toolbar .spacer { margin-left: auto; }
```

**Do:** reach for Grid for page structure, Flex for component-internal rows.
**Don't:** nest 5 levels of flexbox to fake a grid.

---

## Layout archetypes

| Archetype | Structure | When |
|-----------|-----------|------|
| **Centered / single column** | One capped column, `mx-auto` | Articles, auth, focused tasks |
| **Sidebar** | Fixed/sticky nav + fluid content | Apps, docs, dashboards |
| **Holy grail** | Header / sidebar / content / aside / footer | Classic app shell |
| **Split** | Two 50/50 panes | Editors, before/after, login + hero |
| **Bento** | Mixed-size grid cells | Feature showcases, marketing, overviews |
| **Masonry** | Variable-height columns | Image galleries, Pinterest-style feeds |

```css
/* Holy grail with Grid areas */
.shell {
  display: grid;
  grid-template: "header header" auto
                 "nav     main"   1fr
                 "footer  footer" auto / 240px 1fr;
  min-height: 100dvh;
}

/* Bento */
.bento { display: grid; grid-template-columns: repeat(4, 1fr); grid-auto-rows: 180px; gap: 1rem; }
.bento .feature { grid-column: span 2; grid-row: span 2; }
```

**Do:** pick the archetype that matches the task flow, then keep it consistent across pages.
**Don't:** invent a novel layout per screen — users rely on stable structure.

---

## Alignment & optical alignment

- **Establish alignment edges:** left-align text blocks; align numbers right; align controls to a shared grid line.
- **Optical alignment** beats mathematical: visually-centered ≠ geometrically-centered for circles, triangles, icons (a play ▶ icon nudges right to *look* centered).
- **Icon + text:** vertically center on the cap-height/x-height, not the bounding box.
- Limit alignment points — every new edge adds visual noise.

**Do:** trust your eye for triangles, glyphs, and icons; nudge until balanced.
**Don't:** center a play button geometrically and call it done.

---

## The squint test

Squint (or blur the screen) to judge **balance and hierarchy** without reading content:

- Does one area feel too heavy / too empty?
- Does the eye land on the primary action first?
- Are groups visually distinct as blobs?

If the blurred composition is lopsided or you can't tell where to look, fix weight/spacing before polishing details.

**Do:** squint-test every key screen; redistribute whitespace until balanced.
**Don't:** evaluate balance only at 100% with text readable — content masks layout flaws.

---

## Z-index scale management

Define a **named ladder** so stacking is intentional, never `z-index: 99999`.

| Layer | Token | Value | Example |
|-------|-------|-------|---------|
| Base | `--z-base` | 0 | normal flow |
| Dropdown | `--z-dropdown` | 1000 | menus, selects |
| Sticky | `--z-sticky` | 1100 | sticky headers |
| Overlay/scrim | `--z-overlay` | 1200 | modal backdrop |
| Modal | `--z-modal` | 1300 | dialogs |
| Popover | `--z-popover` | 1400 | popovers, tooltips above modal |
| Toast | `--z-toast` | 1500 | notifications |
| Max | `--z-max` | 9999 | critical, rare |

```css
.dropdown { z-index: var(--z-dropdown); }
.toast    { z-index: var(--z-toast); }
```

**Do:** use spaced values (steps of 100/1000) so you can insert layers later.
**Don't:** sprinkle arbitrary z-index numbers — you'll fight stacking wars forever.

---

## Agent checklist

- [ ] Snap all sizing/spacing to the 8pt grid (4px sub-grid for fine work); no odd values.
- [ ] Use more space between groups than within them to express hierarchy.
- [ ] Cap prose at ~65ch and app content at 1200–1280px with responsive side padding.
- [ ] Design mobile-first; layer `sm/md/lg/xl/2xl` overrides upward.
- [ ] Use Grid for 2D page structure, Flexbox for 1D component rows.
- [ ] Keep all gutters in a layout identical and tied to the spacing scale.
- [ ] Pick one layout archetype per flow and keep it consistent across pages.
- [ ] Optically align icons/glyphs; don't trust geometric centering alone.
- [ ] Run the squint test on every key screen to check balance and focal point.
- [ ] Match density to the audience; keep touch targets ≥ 44×44px.
- [ ] Manage stacking with a named z-index ladder, never magic numbers.
