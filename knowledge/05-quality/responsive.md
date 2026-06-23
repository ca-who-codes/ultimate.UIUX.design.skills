# Responsive Design

> Purpose: Build layouts that fluidly adapt from a 320px phone to a 4K monitor — mobile-first, fluid by default, transforming (not just shrinking) at each breakpoint.

**When to read this:** Before writing any layout CSS, choosing breakpoints, or sizing typography and images. Pair with [layout-spacing.md](../02-foundations/layout-spacing.md) for the spacing scale and [accessibility.md](./accessibility.md) for touch-target rules that responsive work must respect.

---

## Mobile-first is a methodology, not a viewport

Start your CSS at the smallest screen and **layer enhancements upward** with `min-width` media/container queries. This is not aesthetic preference — it forces you to prioritize content, produces less CSS, and degrades gracefully.

```css
/* Base = mobile. No media query. Single column, stacked. */
.grid { display: grid; gap: 1rem; grid-template-columns: 1fr; }

/* Enhance upward as space appears. */
@media (min-width: 48rem) { .grid { grid-template-columns: repeat(2, 1fr); } }
@media (min-width: 64rem) { .grid { grid-template-columns: repeat(3, 1fr); } }
```

```css
/* DON'T: desktop-first forces you to claw features back, and you'll forget some */
.grid { grid-template-columns: repeat(3, 1fr); }
@media (max-width: 64rem) { .grid { grid-template-columns: 1fr; } } /* fragile */
```

**Always use `min-width` (mobile-first).** Mixing `min-` and `max-` breakpoints creates overlap bugs.

---

## Breakpoints

Use a small, named, content-driven set. These are the de-facto industry values (Tailwind), in `rem` so they respect user zoom (1rem = 16px):

| Name | px | rem | Typical device | Layout intent |
|------|-----|-----|----------------|---------------|
| (base) | 0–639 | — | Phones portrait | 1 column, stacked, bottom nav |
| `sm` | 640 | 40rem | Phones landscape / small tablets | Still mostly 1 col, larger type |
| `md` | 768 | 48rem | Tablets | 2 columns, sidebar can appear |
| `lg` | 1024 | 64rem | Laptops / small desktop | Multi-column, persistent sidebar |
| `xl` | 1280 | 80rem | Desktop | Full layout, max content width caps |
| `2xl` | 1536 | 96rem | Large desktop | Wider gutters, more columns |

Rules:
- **Pick breakpoints where the layout breaks, not where a device sits.** Devices are infinite; add a custom breakpoint if content demands it. The table is a starting grid, not gospel.
- **Cap content width** so lines don't stretch to 2000px: `max-width: 75ch` for prose, a container max (~1280px) for app shells. Reading line length is 45–75 characters regardless of viewport.
- Test the **gaps between** breakpoints (e.g. 900px), not just the named values.

---

## Fluid layouts: clamp, min/max, container queries (the modern approach)

Media-query breakpoints are coarse. Modern CSS lets layouts flex *continuously*, so you need far fewer breakpoints.

### `clamp()` — fluid sizing without media queries

`clamp(MIN, PREFERRED, MAX)` scales smoothly between two bounds. The middle value should mix a relative unit (`vw`) with a `rem` floor so it never collapses:

```css
.container { width: clamp(20rem, 90vw, 75rem); }     /* 320px → 90% → 1200px */
.section   { padding-block: clamp(2rem, 6vw, 6rem); } /* fluid vertical rhythm */
```

### `min()` / `max()` — guardrails

```css
.card  { width: min(100%, 28rem); }   /* never wider than 28rem, shrinks below */
.media { width: max(50%, 18rem); }    /* at least 18rem even when 50% is smaller */
```

### Intrinsic responsive grid — zero media queries

```css
/* Cards wrap automatically; each is ≥ 16rem, fills the row */
.cards {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(min(16rem, 100%), 1fr));
}
```

The `min(16rem, 100%)` guard prevents overflow on very narrow screens — without it the 16rem floor causes horizontal scroll under ~256px.

### Container queries — respond to the *component's* space, not the viewport

The biggest modern shift: a card in a sidebar and the same card in a main column should lay out by **their own width**, not the page width. Media queries can't do this; container queries can.

```css
.card-wrap { container-type: inline-size; container-name: card; }

/* When the CONTAINER is ≥ 30rem, go horizontal — regardless of viewport */
@container card (min-width: 30rem) {
  .card { grid-template-columns: 8rem 1fr; }
}
```

Reach for container queries for any reusable component placed in varying-width slots. Use viewport media queries for **page-level** structure (overall columns, nav mode).

---

## Layouts must transform, not just shrink

Squishing a desktop layout into a phone produces unusable UIs. The pattern should **change** at breakpoints:

| Desktop pattern | Mobile transform | Why |
|-----------------|------------------|-----|
| Persistent sidebar | Off-canvas drawer / **bottom sheet** | Sidebar steals scarce width |
| Top horizontal nav | Hamburger menu or **bottom tab bar** | Bottom bar sits in the thumb zone |
| Wide data **table** | **Stacked cards** (label: value pairs) | Tables overflow; can't read 8 cols on a phone |
| Multi-column form | Single column | Side-by-side fields are unusable at 320px |
| Hover tooltips/menus | Tap-to-open, click-outside-to-close | No hover on touch |
| Mega-menu | Accordion list | Vertical space > horizontal on phones |
| Modal dialog | Full-screen sheet / bottom sheet | Tiny centered modals are cramped |
| Inline filters bar | Filter button → bottom sheet | Filters eat the viewport |

```css
/* Table → cards example */
@media (max-width: 48rem) {
  table, thead, tbody, th, td, tr { display: block; }
  thead { position: absolute; left: -9999px; } /* hide header row visually */
  tr { border: 1px solid var(--border); border-radius: .5rem; margin-bottom: .75rem; }
  td { display: flex; justify-content: space-between; padding: .5rem .75rem; }
  td::before { content: attr(data-label); font-weight: 600; } /* re-label each cell */
}
```

---

## Touch targets & reachability

- **44×44 CSS px minimum** for any touch target (Apple HIG; Material 48dp). WCAG 2.5.8 floor is 24×24 — design to 44. See [accessibility.md](./accessibility.md).
- Space adjacent targets ≥ 8px so fat-finger taps don't mis-hit.
- **Thumb zones:** on a phone held one-handed, the bottom-center is easy, top corners are hard. Put primary actions (submit, next, key nav) in the **bottom third**; push destructive/rare actions to the top.
- Bottom tab bars and bottom sheets exist because of the thumb zone — use them on mobile instead of top-anchored controls.
- Keep a comfortable bottom inset above the home indicator / system gesture area (see safe areas).

---

## Responsive typography

Body text should scale fluidly between a readable mobile size and a comfortable desktop size — without a jump at each breakpoint.

```css
:root {
  /* min 16px (never smaller for body), max 18px, fluid between */
  --step-0: clamp(1rem, 0.95rem + 0.4vw, 1.125rem);
  --step-1: clamp(1.25rem, 1.1rem + 0.9vw, 1.75rem);   /* h3 */
  --step-2: clamp(1.6rem, 1.3rem + 1.8vw, 2.75rem);    /* h2 */
  --step-3: clamp(2rem, 1.5rem + 3.5vw, 4rem);         /* h1 / hero */
}
body { font-size: var(--step-0); line-height: 1.6; }
h1   { font-size: var(--step-3); line-height: 1.1; }
```

Rules:
- **Never below 16px for body** on mobile — smaller text triggers iOS auto-zoom on input focus and hurts readability.
- Always include a `rem` term in the clamp so type doesn't vanish at tiny viewports and **still scales with user zoom** (pure `vw` breaks zoom — a WCAG 1.4.4 fail).
- Tighten `line-height` as size grows (1.6 body → 1.1 display); loosen `letter-spacing` slightly on large headings.
- Cap measure with `max-width: 65ch` on text blocks. Full theory in [typography.md](../02-foundations/typography.md).

---

## Responsive images

Bad images are the #1 cause of slow mobile pages. Serve the right size and format.

```html
<img
  src="hero-800.jpg"
  srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1600.jpg 1600w"
  sizes="(max-width: 48rem) 100vw, 50vw"
  width="1600" height="900"
  loading="lazy" decoding="async"
  alt="…">
```

- **`srcset` + `sizes`** lets the browser pick the smallest sufficient file for the device's DPR and slot width. `sizes` describes the rendered width at each breakpoint.
- **`width`/`height` attributes** (or `aspect-ratio` in CSS) reserve space → no layout shift (CLS). Mandatory. See [performance.md](./performance.md).
- **`loading="lazy"`** on below-the-fold images; **never** lazy-load the LCP/above-the-fold hero (it delays the largest paint).
- **Modern formats** via `<picture>`: AVIF (best compression) → WebP → JPEG fallback.

```html
<picture>
  <source type="image/avif" srcset="hero.avif">
  <source type="image/webp" srcset="hero.webp">
  <img src="hero.jpg" width="1600" height="900" alt="…">
</picture>
```

- Use `object-fit: cover` + a fixed `aspect-ratio` for art that must fill a box without distortion.
- For art-direction (different crop on mobile), use `<picture>` with `media` on each `<source>`, not just resolution switching.

---

## Avoiding horizontal scroll

Unintended horizontal scroll is the most common responsive bug. Causes and fixes:

| Cause | Fix |
|-------|-----|
| Fixed-width element wider than viewport | Use `max-width: 100%`, `min-width: 0` |
| Long unbreakable string/URL | `overflow-wrap: anywhere;` / `word-break: break-word` |
| Image with no max-width | `img { max-width: 100%; height: auto; }` |
| Negative margins / off-canvas leaking | Clip parent or use `overflow-x: clip` |
| Flex/grid child refusing to shrink | Add `min-width: 0` to the flex/grid item |
| `100vw` ignoring scrollbar | Prefer `100%`; `100vw` includes the scrollbar width |
| Absolute positioning past edge | Constrain with `inset`/`max-width` |

Debug fast: `* { outline: 1px solid red; }` and scroll right to spot the overflowing box, or in DevTools toggle the device toolbar at 320px.

---

## Safe areas (notches, home indicators, rounded corners)

Devices with notches/home indicators (iPhone, many Android) reserve screen edges. Opt into the full screen, then **pad with `env()`** so content isn't clipped.

```html
<meta name="viewport"
      content="width=device-width, initial-scale=1, viewport-fit=cover">
```

```css
.app-bar    { padding-top: env(safe-area-inset-top); }
.bottom-nav { padding-bottom: max(env(safe-area-inset-bottom), 0.75rem); }
body        { padding-inline: env(safe-area-inset-left) env(safe-area-inset-right); }
```

`viewport-fit=cover` is required for `env()` to report nonzero values. Wrap in `max()` so you keep a baseline pad on devices without insets.

---

## Viewport meta & dynamic viewport units

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

- **Never** set `user-scalable=no` or `maximum-scale=1` — it blocks pinch-zoom (WCAG 1.4.4 fail). Let users zoom.
- Mobile browser chrome (URL bar) shrinks/grows, making `100vh` jump. Use the dynamic units:
  - `100svh` (small — when chrome is shown), `100lvh` (large — chrome hidden), `100dvh` (dynamic — adjusts live). For a full-height hero that doesn't jank as the bar hides, use `min-height: 100dvh`.

---

## Orientation

- Support both portrait and landscape; **never lock orientation** (WCAG 1.3.4) unless essential.
- In landscape on phones, vertical space is scarce — collapse tall headers, avoid full-height heroes, reflow with `@media (orientation: landscape)` where needed.
- Keep critical actions reachable without scrolling in both orientations.

---

## Testing matrix

Test these widths, both orientations, at 1× and high DPR. Don't trust a single laptop window.

| Width | Represents | Check for |
|-------|-----------|-----------|
| **320px** | Smallest supported (iPhone SE 1, fold cover) | No horizontal scroll, no clipped text/buttons |
| 360–390px | Mainstream Android / modern iPhone | Thumb-zone actions, tap target sizes |
| 414–430px | Large phones | Layout still single-purpose |
| 768px | Tablet portrait / breakpoint edge | Sidebar/2-col transition |
| 1024px | Tablet landscape / small laptop | Multi-column kicks in cleanly |
| 1280–1440px | Standard desktop | Max-width caps engage, gutters look right |
| **1920px+** | Large/4K desktop | Content doesn't stretch ugly; centered with max-width |

Also test: **browser zoom to 200%** (1.4.10 Reflow — no horizontal scroll, no clipped content at 320px-equivalent), **text-only zoom 200%** (1.4.4), reduced-motion, and real device touch (emulators lie about hover).

---

## Agent checklist

- [ ] Write **mobile-first** with `min-width` queries; base styles are the phone layout.
- [ ] Use **`clamp()`/`min()`/`max()`** and intrinsic grids to minimize hard breakpoints; reserve breakpoints for where content actually breaks.
- [ ] Reach for **container queries** for reusable components; viewport queries for page structure.
- [ ] Make layouts **transform** at breakpoints (sidebar→sheet, table→cards), not merely shrink.
- [ ] Size touch targets to **44×44** and place primary actions in the **thumb zone** (bottom third).
- [ ] Scale type fluidly with `clamp()`, keep body **≥ 16px** on mobile, and never break zoom with pure `vw`.
- [ ] Serve **`srcset`/`sizes`**, set `width`/`height` (or `aspect-ratio`), `loading="lazy"` below the fold, and AVIF/WebP via `<picture>`.
- [ ] Eliminate **horizontal scroll** at 320px (`max-width:100%`, `min-width:0`, `overflow-wrap`).
- [ ] Set the correct **viewport meta** with `viewport-fit=cover`, never disable zoom, and use `dvh`/`svh` for full-height.
- [ ] Pad edges with **`env()` safe-area insets** wrapped in `max()`.
- [ ] Run the **testing matrix 320px→1920px+**, both orientations, plus 200% zoom and a real touch device.
