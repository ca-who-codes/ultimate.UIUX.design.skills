# Typography

> Purpose: Set type that is readable, hierarchical, and harmonious — modular scales, line-height, measure, pairing, fluid sizing, and data figures with exact values.

**When to read this:** Before choosing fonts, defining a type scale, styling headings/body, or fixing readability and rhythm. Pair with [layout-spacing.md](./layout-spacing.md) for vertical rhythm and [design-tokens.md](./design-tokens.md) for typography tokens.

---

## Type scale (modular scale)

Pick **one ratio** and multiply from a 16px base. Larger ratios = more dramatic hierarchy.

| Ratio | Name | Feel | Use for |
|-------|------|------|---------|
| **1.200** | Minor third | Tight, dense | Dashboards, data UIs, dense apps |
| **1.250** | Major third | Balanced (default) | Most product UIs, SaaS |
| **1.333** | Perfect fourth | Expressive, airy | Marketing, editorial, landing pages |

```text
base 16px × 1.25:
16 → 20 → 25 → 31.25 → 39 → 48.8 → 61
```

**Do:** generate the scale mathematically, then round to clean px.
**Don't:** hand-pick arbitrary sizes (17, 19, 23) with no relationship.

---

## Recommended sizes

| Role | Size | rem | Floor rule |
|------|------|-----|-----------|
| Body | **16px** | 1rem | **Never below 16px for primary reading** (mobile zoom trigger below) |
| Small / secondary | 14px | 0.875rem | Captions, helper text, table cells |
| Caption / legal | 12–13px | 0.75–0.8125rem | Lowest acceptable; never for paragraphs |
| Lead / intro | 18–20px | 1.125–1.25rem | Opening paragraph |
| H3 | 20–24px | 1.25–1.5rem | |
| H2 | 30–36px | 1.875–2.25rem | |
| H1 | 36–48px | 2.25–3rem | App; up to 60–72px for hero |

**Do:** keep body ≥ 16px — iOS zooms inputs < 16px on focus.
**Don't:** ship 13px body text to "fit more"; reduce content instead.

---

## Line-height (leading)

Leading scales **inversely** with size — big text needs tighter leading, small text needs looser.

| Text | line-height | Tailwind |
|------|-------------|----------|
| Body paragraphs | **1.5–1.6** | `leading-relaxed` (1.625) / `leading-7` |
| Small text | 1.4–1.5 | `leading-snug`/`leading-normal` |
| H3 / subheads | 1.25–1.35 | `leading-snug` |
| H1 / H2 | **1.1–1.2** | `leading-tight` (1.25) / `leading-none` |
| Display / hero | 1.0–1.1 | `leading-none` |
| UI labels, buttons | 1.0–1.2 | `leading-none` |

**Do:** tighten leading as font size grows.
**Don't:** use 1.5 line-height on a 48px headline — it floats apart.

---

## Measure (line length)

| Metric | Value |
|--------|-------|
| Acceptable range | **45–75 characters** per line |
| **Ideal** | **~66ch** |
| Multi-column | 40–50ch per column |
| Hard cap (CSS) | `max-width: 65ch` for prose |

```css
.prose { max-width: 65ch; }       /* ~600–700px at 16px */
```
```html
<article class="max-w-[65ch] mx-auto">…</article>
```

**Do:** constrain paragraph width even in wide layouts.
**Don't:** let body text run the full 1280px container — eyes lose the next line.

---

## Font pairing

| Strategy | How | Example |
|----------|-----|---------|
| **Superfamily** (safest) | One family, vary weight/size | Inter everywhere |
| Contrast pairing | Serif display + sans body, or vice versa | Fraunces headings + Inter body |
| Sans + mono | Sans UI + mono for data/code | Geist + Geist Mono |
| Limit | **≤ 2 families** (3 max if one is mono) | — |

Pairing rules: pair fonts that **differ clearly** (don't pair two similar sans), share a similar x-height, and keep one as the workhorse for body.

**Do:** use weight and size for hierarchy before reaching for a second font.
**Don't:** mix three display fonts — it reads as chaos.

---

## Recommended free stacks

```css
/* Inter — the reliable product default */
font-family: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;

/* Geist — modern, geometric (Vercel) */
font-family: "Geist", system-ui, sans-serif;

/* Pure system stack — zero load cost, fast */
font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
             Roboto, Helvetica, Arial, sans-serif;

/* Mono for data/code */
font-family: "Geist Mono", "JetBrains Mono", ui-monospace, "SF Mono",
             Menlo, Consolas, monospace;
```

Always provide a fallback chain ending in a generic family. Enable Inter's optical sizing and add `font-feature-settings: "cv01", "ss01";` if you want its alternate glyphs.

**Do:** ship a system-stack fallback so text renders before webfonts load.
**Don't:** load 6 font weights — subset to the 2–3 you actually use.

---

## Font weights

| Weight | Value | Use |
|--------|-------|-----|
| Regular | 400 | **Body minimum** — never lighter for paragraphs |
| Medium | 500 | UI labels, emphasized body, buttons |
| Semibold | 600 | Subheads, card titles |
| Bold | 700 | Headings, strong emphasis |
| Light / Thin | 100–300 | **Large display only**, never body |

**Do:** use 400 for body; 500–600 for UI; 600–700 for headings.
**Don't:** set body or small text below 400 — it shreds on low-DPI screens.

---

## Letter-spacing (tracking)

| Context | Adjustment | Tailwind |
|---------|-----------|----------|
| Large headings (≥ 30px) | **tighten** −0.01 to −0.03em | `tracking-tight` (−0.025em) |
| Body | leave at 0 (`normal`) | `tracking-normal` |
| All-caps / labels | **loosen** +0.05 to +0.1em | `tracking-wide`/`tracking-wider` |
| Small caps, eyebrows | +0.08em | `tracking-widest` |

**Do:** tighten big display type; loosen all-caps so letters breathe.
**Don't:** track out lowercase body text — it slows reading.

---

## Vertical rhythm

Anchor spacing to the type scale so blocks stack predictably:

- Use a **baseline unit** (e.g. 4px / 8px) — see [layout-spacing.md](./layout-spacing.md).
- Set heading `margin-top` larger than `margin-bottom` (e.g. `mt-12 mb-4`) so a heading bonds to the text it introduces.
- Paragraph spacing ≈ 0.75–1× line-height (`space-y-4` for 16px body).

```css
h2 { margin-top: 3rem; margin-bottom: 1rem; }   /* mt > mb: groups with body below */
p + p { margin-top: 1rem; }
```

**Do:** give headings more space above than below.
**Don't:** use uniform `margin` everywhere — content loses grouping.

---

## Fluid typography with clamp()

Scale type smoothly between breakpoints — no media-query jumps.

```css
/* clamp(MIN, PREFERRED, MAX) */
--step-0: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);   /* body 16→18 */
--step-2: clamp(1.5rem, 1.3rem + 1vw, 2rem);          /* H3 24→32 */
--step-4: clamp(2.25rem, 1.8rem + 2.2vw, 3.5rem);     /* H1 36→56 */

h1 { font-size: var(--step-4); line-height: 1.1; }
```

Tailwind v4: `text-[clamp(2.25rem,1.8rem+2.2vw,3.5rem)]`.

**Do:** cap fluid type with a sensible MAX so it never gets absurd on 4K.
**Don't:** use raw `vw` units without `clamp()` — text becomes unreadable at extremes.

---

## Numeric & tabular figures

For tables, prices, timers, dashboards, code — use **tabular (monospaced) figures** so digits align in columns:

```css
.data, td.num, .price { font-variant-numeric: tabular-nums; }   /* Tailwind: tabular-nums */
```

Other useful settings:
- `font-variant-numeric: slashed-zero;` to disambiguate 0 from O (`slashed-zero`).
- `lining-nums` for caps-height digits in UI; `oldstyle-nums` only for running prose.

**Do:** apply `tabular-nums` to any column of numbers that updates or aligns.
**Don't:** leave proportional figures in a financial table — digits jitter.

---

## Truncation & wrapping

| Goal | CSS | Tailwind |
|------|-----|----------|
| Single-line ellipsis | `overflow:hidden; text-overflow:ellipsis; white-space:nowrap` | `truncate` |
| Multi-line clamp | `display:-webkit-box; -webkit-line-clamp:3; -webkit-box-orient:vertical; overflow:hidden` | `line-clamp-3` |
| Balanced headlines | `text-wrap: balance` | `text-balance` |
| Pretty paragraphs (no orphans) | `text-wrap: pretty` | `text-pretty` |
| Break long URLs/words | `overflow-wrap: anywhere` | `break-words` |

`text-wrap: balance` evens out ragged headline lines (best ≤ 4 lines). `text-wrap: pretty` prevents a single-word last line in paragraphs.

**Do:** apply `text-balance` to headings and `text-pretty` to body copy.
**Don't:** truncate without a `title`/tooltip — users can't recover hidden text.

---

## Ready-to-use type scale (16px base, ratio 1.250)

| Token | px | rem | line-height | weight | tracking | Tailwind |
|-------|----|----|-------------|--------|----------|----------|
| caption | 12 | 0.75 | 1.4 | 400 | normal | `text-xs leading-snug` |
| small | 14 | 0.875 | 1.45 | 400 | normal | `text-sm leading-normal` |
| **body** | **16** | **1.0** | **1.6** | 400 | normal | `text-base leading-relaxed` |
| body-lg / lead | 18 | 1.125 | 1.55 | 400 | normal | `text-lg leading-relaxed` |
| h4 | 20 | 1.25 | 1.4 | 600 | normal | `text-xl font-semibold leading-snug` |
| h3 | 25 | 1.5625 | 1.3 | 600 | −0.01em | `text-2xl font-semibold leading-tight` |
| h2 | 31 | 1.9375 | 1.2 | 700 | −0.02em | `text-3xl font-bold tracking-tight leading-tight` |
| h1 | 39 | 2.4375 | 1.15 | 700 | −0.02em | `text-4xl font-bold tracking-tight leading-tight` |
| display | 49 | 3.0625 | 1.05 | 700 | −0.03em | `text-5xl font-bold tracking-tighter leading-none` |
| display-xl | 61 | 3.8125 | 1.0 | 800 | −0.03em | `text-6xl font-extrabold tracking-tighter leading-none` |

(Tailwind's default `rem` for each step differs slightly; override via `@theme` — see [design-tokens.md](./design-tokens.md) — to match this scale exactly.)

---

## Agent checklist

- [ ] Pick one modular ratio (1.2 dense, 1.25 default, 1.333 editorial) and generate the scale.
- [ ] Keep body ≥ 16px and never set paragraph weight below 400.
- [ ] Set body line-height 1.5–1.6; tighten to 1.1–1.2 for H1/display.
- [ ] Constrain prose to ~65ch (`max-w-[65ch]`), even inside wide containers.
- [ ] Limit to ≤ 2 font families (3 if one is mono); ship a system-stack fallback.
- [ ] Tighten tracking on large headings (`tracking-tight`); loosen all-caps (`tracking-wide`).
- [ ] Give headings more top margin than bottom so they group with following text.
- [ ] Use `clamp()` for fluid sizes with a capped MAX; avoid raw `vw`.
- [ ] Apply `tabular-nums` to every numeric column, price, or timer.
- [ ] Use `text-balance` on headings and `text-pretty` on body; never truncate without a tooltip.
