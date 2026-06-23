# Color

> Purpose: Build a perceptually consistent, accessible color system from a single brand hue — covering OKLCH, palettes, contrast, dark mode, states, and gradients.

**When to read this:** Before choosing any color, defining tokens, building a theme, or reviewing a UI for contrast/accessibility. Pair with [design-tokens.md](./design-tokens.md) to wire these values into a token set.

---

## Color models: HSL vs OKLCH

| Model | Syntax | Strength | Fatal flaw | Use it for |
|-------|--------|----------|------------|------------|
| HEX | `#3b82f6` | Universal, copy-paste | Opaque, no math | Final output / legacy |
| RGB | `rgb(59 130 246)` | Native to screens | Not human-readable | Compositing, canvas |
| HSL | `hsl(217 91% 60%)` | Intuitive H/S/L | **Lightness lies** — `hsl(60 100% 50%)` (yellow) looks far brighter than `hsl(240 100% 50%)` (blue) at the same `L` | Quick tweaks only |
| **OKLCH** | `oklch(0.62 0.21 256)` | **Perceptually uniform lightness**, wide-gamut (P3) | Newer syntax, needs fallbacks for very old browsers | **Authoring palettes & scales** |

### Why OKLCH wins

In HSL, equal `L` values do **not** mean equal perceived brightness. A 500-step yellow and a 500-step blue at `L: 50%` fail to look like the same "weight," so HSL-built palettes drift in lightness across hues. OKLCH fixes this: its `L` (0–1) maps to actual perceived lightness, so **every hue at `L 0.62` reads as the same brightness**. This is the single biggest reason OKLCH-built scales look professionally consistent.

- `L` = perceptual lightness, `0` (black) → `1` (white)
- `C` = chroma (saturation), `0` (gray) → ~`0.37` (max displayable)
- `H` = hue angle, `0–360` (same wheel as HSL: 0 red, 120 green, 240 blue)

```css
/* Same lightness, three hues — all read equally bright */
--blue:   oklch(0.62 0.20 256);
--green:  oklch(0.62 0.20 145);
--red:    oklch(0.62 0.20 27);
```

**Do:** author scales in OKLCH, hold `L` constant across same-step semantic colors.
**Don't:** build a multi-hue palette in HSL and expect consistent weight.

---

## Building a palette

A complete system needs four families. Keep it disciplined — more colors is not more design.

| Family | Role | Count | Notes |
|--------|------|-------|-------|
| **Primary / Brand** | Identity, primary actions, focus | 1 hue, 11 steps | The "10" in 60-30-10 |
| **Neutral / Gray** | Text, backgrounds, borders, surfaces | 1 hue, 11 steps | The "60". Tint slightly toward brand hue (±5°) for cohesion |
| **Semantic** | Success / Warning / Error / Info | 4 hues, ~5 steps each | Fixed meanings; never restyle them per page |
| **Accent (optional)** | Secondary highlight, charts | 1–2 hues | Only if the product needs it |

### Semantic color anchors

| Token | Hue (OKLCH H) | Light hex | Dark hex | Meaning |
|-------|---------------|-----------|----------|---------|
| `success` | 145 (green) | `#16a34a` | `#22c55e` | Confirmation, positive |
| `warning` | 75 (amber) | `#d97706` | `#f59e0b` | Caution, needs attention |
| `error` | 27 (red) | `#dc2626` | `#f87171` | Destructive, failure |
| `info` | 256 (blue) | `#2563eb` | `#60a5fa` | Neutral notice |

**Do:** keep success green, error red — users have learned these.
**Don't:** make your brand color red and also use red for errors; pick a different brand hue or shift error to a distinct red.

---

## The 60-30-10 rule

Allocate visual surface area, not number of colors:

- **60% Neutral** — page background, cards, large surfaces (your gray family)
- **30% Secondary** — supporting surfaces, secondary text, borders, muted UI
- **10% Brand/Accent** — primary buttons, links, active states, key highlights

```text
[████████████████████████████████████  60% neutral  ]
[██████████████████  30% secondary/muted  ]
[██████  10% brand accent  ]
```

**Do:** let the accent earn attention by being scarce.
**Don't:** flood the page with brand color — it stops signaling "act here."

---

## Tint/shade scales (50–950)

Generate 11 steps per family. Target these OKLCH lightness values (hold `H` ~constant, scale `C` down at the extremes so they don't look neon or muddy):

| Step | OKLCH `L` | Typical use |
|------|-----------|-------------|
| 50 | 0.97 | Lightest tint, subtle bg wash |
| 100 | 0.94 | Hover bg on light surfaces |
| 200 | 0.88 | Borders (light), dividers |
| 300 | 0.80 | Disabled bg, muted borders |
| 400 | 0.70 | Placeholder text, icons |
| **500** | **0.62** | **Base brand color** |
| 600 | 0.54 | Hover for 500 buttons |
| 700 | 0.46 | Active/pressed, strong text |
| 800 | 0.38 | Headings on light bg |
| 900 | 0.30 | Highest-contrast text |
| 950 | 0.22 | Near-black brand ink |

```css
/* Derive a full brand ramp from one hue (256) by varying L, easing C at the ends */
--brand-50:  oklch(0.97 0.02 256);
--brand-100: oklch(0.94 0.04 256);
--brand-200: oklch(0.88 0.07 256);
--brand-300: oklch(0.80 0.11 256);
--brand-400: oklch(0.70 0.16 256);
--brand-500: oklch(0.62 0.20 256);  /* base */
--brand-600: oklch(0.54 0.19 256);
--brand-700: oklch(0.46 0.17 256);
--brand-800: oklch(0.38 0.14 256);
--brand-900: oklch(0.30 0.10 256);
--brand-950: oklch(0.22 0.06 256);
```

**Do:** reduce chroma at 50/100 and 900/950 — full chroma there looks artificial.
**Don't:** keep a flat `C` across all 11 steps.

---

## Contrast & WCAG

| Target | Min ratio | Applies to |
|--------|-----------|------------|
| **Body text** (< 18px, or < 14px bold) | **4.5:1** (AA), 7:1 (AAA) | Paragraphs, labels, inputs |
| **Large text** (≥ 24px, or ≥ 18.66px bold) | **3:1** (AA), 4.5:1 (AAA) | Headings, hero copy |
| **UI components & graphics** | **3:1** | Icons, input borders, focus rings, chart strokes |
| Disabled elements | exempt | But keep them perceivable |

Verify every pairing. Contrast is a ratio of **relative luminance**, not OKLCH `L` difference — always check with a tool (DevTools, `apca`/WCAG checker), never eyeball.

### Accessible pairing cheatsheet

| Background | Safe body text | Avoid |
|------------|----------------|-------|
| White `#ffffff` | `gray-700 #374151`+ (≥5.7:1) | gray-400/500 (fails) |
| `gray-100 #f3f4f6` | `gray-700`+ | brand-500 thin text |
| `brand-500 #3b82f6` | white (≥3.7:1 — large only at 500; use brand-600 for body) | brand-200, yellow |
| Dark `#0a0a0a` | `gray-200 #e5e7eb` (≥14:1) | gray-600, brand-700 |

**Do:** test brand-on-white — many brand 500s only pass for *large* text. Use the 600/700 step for body-size links.
**Don't:** rely on color alone to convey state — pair with icon/text (color-blind safety).

---

## Dark mode strategy

Dark mode is not "invert everything." Follow these rules:

| Rule | Value | Why |
|------|-------|-----|
| No pure black bg | `#0a0a0a` / `oklch(0.15 0 0)` | Pure `#000` causes halation & harsh contrast; smudges OLED smearing |
| No pure white text | off-white `#ededed` / `oklch(0.93 0 0)` | Pure `#fff` on black vibrates; ~90% white reads calmer |
| **Elevate with lightness, not just shadow** | each surface +3–6% `L` | Shadows are nearly invisible on dark bg; lighter = "closer" |
| Reduce saturation of accents | drop `C` ~10–20% | Saturated colors glow / bloom on dark |
| Lighten brand for text | use 400 instead of 600 | Maintain the 4.5:1 ratio against dark bg |
| Soften borders | use low-`L` gray or 8% white overlay | Hard borders look heavy in dark |

### Dark elevation ladder (surfaces by lightness)

| Surface | OKLCH | Hex | Use |
|---------|-------|-----|-----|
| Base / page | `oklch(0.15 0 0)` | `#0a0a0a` | App background |
| Surface 1 | `oklch(0.19 0 0)` | `#171717` | Cards |
| Surface 2 | `oklch(0.23 0 0)` | `#222222` | Popovers, raised cards |
| Surface 3 | `oklch(0.27 0 0)` | `#2e2e2e` | Modals, menus |
| Border | `oklch(0.30 0 0)` | `#333333` | Hairlines |

**Do:** make modals lighter than the cards behind them.
**Don't:** stack `box-shadow` to fake depth on dark — it barely shows.

---

## State colors

Derive interactive states from the base, not from arbitrary new colors:

| State | Light theme rule | Dark theme rule |
|-------|------------------|-----------------|
| Default | `brand-500` | `brand-500` |
| **Hover** | shift one step darker → `brand-600` (or `oklch L −0.06`) | one step **lighter** → `brand-400` |
| **Active/Pressed** | `brand-700` (or `L −0.12`) | `brand-300/400` |
| **Focus** | keep fill, add ring `0 0 0 3px brand-500/40%` | same, ring slightly brighter |
| **Disabled** | `opacity: 0.5` or `gray-300` bg + `gray-400` text | `opacity: 0.4`, never below 3:1 perceptible |
| Selected | `brand-100` bg + `brand-700` text | `brand-900` bg + `brand-200` text |

```css
.btn { background: var(--brand-500); }
.btn:hover  { background: var(--brand-600); }
.btn:active { background: var(--brand-700); }
.btn:focus-visible { outline: none; box-shadow: 0 0 0 3px oklch(0.62 0.20 256 / 0.4); }
.btn:disabled { background: var(--gray-300); color: var(--gray-500); cursor: not-allowed; }
```

**Do:** hover = darker in light mode, lighter in dark mode.
**Don't:** change hue on hover — only lightness/chroma.

---

## Gradients (tastefully)

| Do | Don't |
|----|-------|
| Use adjacent hues (≤ 60° apart): `256→280` blue→indigo | Span the wheel (blue→orange) — muddy gray midpoint |
| Interpolate in OKLCH: `linear-gradient(in oklch, ...)` | Default sRGB interpolation (dull middle) |
| Keep subtle: 2 stops, low angle, near-equal `L` | 5-stop rainbow buttons |
| Use for large surfaces (hero, cards), mesh backgrounds | Body text, small icons |
| Overlay a 1–2% noise to kill banding | Hard banding on big gradients |

```css
/* OKLCH gradient avoids the gray dead-zone of sRGB */
background: linear-gradient(135deg in oklch,
  oklch(0.65 0.20 256),
  oklch(0.60 0.21 290));
```

**Do:** keep gradient stops within ~0.08 `L` of each other for a smooth feel.
**Don't:** put low-contrast text over a gradient without a scrim.

---

## Concrete example palette

A complete, copy-ready system. Brand hue 256 (blue), neutrals tinted +4° toward brand.

### Brand

| Step | Light hex | Light OKLCH | Dark-mode usage |
|------|-----------|-------------|-----------------|
| 50 | `#eff6ff` | `oklch(0.97 0.02 256)` | rare bg wash |
| 100 | `#dbeafe` | `oklch(0.93 0.04 256)` | selected bg → use as dark text |
| 300 | `#93c5fd` | `oklch(0.80 0.11 256)` | dark-mode link/text |
| 400 | `#60a5fa` | `oklch(0.70 0.16 256)` | **dark-mode primary** |
| **500** | `#3b82f6` | `oklch(0.62 0.20 256)` | **light-mode primary** |
| 600 | `#2563eb` | `oklch(0.54 0.19 256)` | hover (light) |
| 700 | `#1d4ed8` | `oklch(0.46 0.17 256)` | active, body links |
| 900 | `#1e3a8a` | `oklch(0.30 0.10 256)` | dark-mode selected bg |

### Neutral

| Step | Light hex | Dark hex | Role |
|------|-----------|----------|------|
| 0 | `#ffffff` | `#0a0a0a` | page bg |
| 50 | `#f9fafb` | `#171717` | subtle surface |
| 100 | `#f3f4f6` | `#222222` | card / hover bg |
| 200 | `#e5e7eb` | `#333333` | border |
| 400 | `#9ca3af` | `#6b7280` | placeholder / muted |
| 600 | `#4b5563` | `#9ca3af` | secondary text |
| 700 | `#374151` | `#d1d5db` | body text |
| 900 | `#111827` | `#ededed` | headings / primary text |

### Semantic (light / dark fill)

| Token | Light | Dark | Text-on-fill |
|-------|-------|------|--------------|
| success | `#16a34a` | `#22c55e` | white |
| warning | `#d97706` | `#f59e0b` | `#1c1917` (dark text) |
| error | `#dc2626` | `#f87171` | white |
| info | `#2563eb` | `#60a5fa` | white |

---

## Agent checklist

- [ ] Author all palettes in OKLCH; hold `L` constant across same-step semantic hues.
- [ ] Ship exactly four families: brand, neutral, 4 semantics — add accents only if justified.
- [ ] Generate 11 steps (50–950) per family, easing chroma at the extremes.
- [ ] Apply 60-30-10 by surface area; keep brand color scarce (~10%).
- [ ] Verify every text pairing: 4.5:1 body, 3:1 large/UI — with a tool, not your eye.
- [ ] In dark mode use `#0a0a0a` bg and ~90% off-white text, never pure `#000`/`#fff`.
- [ ] Elevate dark surfaces by raising lightness 3–6% per level, not by shadows.
- [ ] Lighten brand to the 400 step for accents/links in dark mode to hold contrast.
- [ ] Derive hover (darker/lighter by one step) and active states from the base; never change hue.
- [ ] Keep gradients ≤ 60° hue span, interpolate `in oklch`, add subtle noise to prevent banding.
- [ ] Never signal state with color alone — pair with icon or text for color-blind users.
