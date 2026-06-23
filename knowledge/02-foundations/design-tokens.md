# Design Tokens

> Purpose: Define a 3-tier token architecture and a complete, copy-paste token set wired into Tailwind v4 and shadcn — colors, spacing, radius, shadow, type, z-index, and motion.

**When to read this:** Before setting up theming, building a component library, or converting raw values into a maintainable system. Pulls values from [color.md](./color.md), [typography.md](./typography.md), and [layout-spacing.md](./layout-spacing.md).

---

## What design tokens are

A **design token** is a named, single-source-of-truth variable for a design decision (`--color-primary`, `--space-4`). Tokens decouple *intent* from *value*: change the token, change everywhere it's used. They make theming, dark mode, and multi-brand support trivial.

---

## The 3-tier model

| Tier | Also called | Names describe… | Example | Components use? |
|------|-------------|-----------------|---------|-----------------|
| **1. Primitive** | global, base, "options" | the raw value | `--blue-500: #3b82f6` | ❌ never directly |
| **2. Semantic** | alias, system | the **role/intent** | `--color-primary: var(--blue-500)` | ✅ yes |
| **3. Component** | scoped | a specific component part | `--button-bg: var(--color-primary)` | ✅ optional |

Flow: **primitive → semantic → component**. Components reference semantic (or component) tokens, *never* primitives. To re-theme, you only remap the semantic layer.

```css
--blue-500: #3b82f6;                 /* 1 primitive: a fact */
--color-primary: var(--blue-500);    /* 2 semantic: a decision */
--button-bg: var(--color-primary);   /* 3 component: a usage */
```

**Do:** point every component at semantic tokens.
**Don't:** hardcode `#3b82f6` or reference `--blue-500` inside a button.

---

## Naming conventions

Pattern: `--[category]-[concept]-[variant]-[state]`

| Good | Bad | Why |
|------|-----|-----|
| `--color-bg-surface` | `--white2` | role, not value |
| `--color-text-muted` | `--gray-light` | semantic intent |
| `--space-4` | `--padding-medium` | scale-indexed, predictable |
| `--color-border-error` | `--red-border` | role + variant |
| `--radius-md` | `--corner` | category prefix |

Rules: lowercase, kebab-case, category prefix first, never encode the literal value in a semantic name, use t-shirt or numeric scales consistently (don't mix `sm/md/lg` with `2/4/6`).

---

## Border-radius scale

| Token | Value | Tailwind | Use |
|-------|-------|----------|-----|
| `--radius-none` | 0 | `rounded-none` | tables, full-bleed |
| `--radius-sm` | 4px | `rounded-sm` | inputs, small chips |
| `--radius-md` | 8px | `rounded-md` | **default** buttons, cards |
| `--radius-lg` | 12px | `rounded-lg` | large cards, modals |
| `--radius-xl` | 16px | `rounded-xl` | hero cards, sheets |
| `--radius-2xl` | 24px | `rounded-2xl` | marketing panels |
| `--radius-full` | 9999px | `rounded-full` | pills, avatars |

**Nested radius rule:** inner radius = outer radius − padding. A card with `radius-lg` (12) and `p-2` (8) should give inner elements `radius-sm` (4) so corners stay concentric.

**Do:** keep a single base radius and derive others from it.
**Don't:** mix sharp inputs with very round cards arbitrarily — pick a personality.

---

## Elevation / shadow scale

Use **layered, multi-stop shadows** (a tight ambient + a softer cast) for realism — never one harsh blur. Tint shadows toward the background hue, keep them low-alpha.

```css
/* Layered shadows: two stops each = realistic depth */
--shadow-xs: 0 1px 2px 0 rgb(0 0 0 / 0.05);
--shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.06), 0 1px 3px 0 rgb(0 0 0 / 0.10);
--shadow-md: 0 2px 4px -1px rgb(0 0 0 / 0.06), 0 4px 8px -2px rgb(0 0 0 / 0.10);
--shadow-lg: 0 4px 8px -2px rgb(0 0 0 / 0.06), 0 12px 20px -4px rgb(0 0 0 / 0.12);
--shadow-xl: 0 8px 16px -4px rgb(0 0 0 / 0.08), 0 24px 40px -8px rgb(0 0 0 / 0.16);
```

| Token | Elevation | Use |
|-------|-----------|-----|
| `--shadow-xs` | 1 | subtle separation, inputs |
| `--shadow-sm` | 2 | cards at rest |
| `--shadow-md` | 3 | hovered cards, dropdowns |
| `--shadow-lg` | 4 | popovers, modals |
| `--shadow-xl` | 5 | command palette, peak overlays |

In **dark mode**, shadows barely read — express elevation via **lighter surfaces** (see [color.md](./color.md)) and optionally a subtle top inner highlight `inset 0 1px 0 rgb(255 255 255 / 0.06)`.

**Do:** use 2-stop shadows; raise alpha and spread together with elevation.
**Don't:** use `0 0 20px black` — harsh, flat, amateurish.

---

## Motion tokens

```css
/* Durations */
--duration-instant: 100ms;   /* micro feedback, hovers */
--duration-fast:    150ms;   /* most UI transitions */
--duration-normal:  250ms;   /* dropdowns, toggles */
--duration-slow:    400ms;   /* modals, page transitions */

/* Easings */
--ease-standard: cubic-bezier(0.2, 0, 0, 1);     /* enter+exit, default */
--ease-out:      cubic-bezier(0, 0, 0.2, 1);     /* elements entering */
--ease-in:       cubic-bezier(0.4, 0, 1, 1);     /* elements exiting */
--ease-spring:   cubic-bezier(0.34, 1.56, 0.64, 1); /* playful overshoot */
```

Most UI motion should be **150–250ms** with an ease-out. Always respect `prefers-reduced-motion`.

**Do:** animate `transform`/`opacity` (GPU-cheap) with these tokens.
**Don't:** animate `width`/`top`/`height` or exceed ~400ms for routine UI.

---

## Wiring into Tailwind v4 (`@theme`)

Tailwind v4 reads CSS variables in an `@theme` block and generates utilities. Namespaces (`--color-*`, `--spacing-*`, `--radius-*`, `--font-*`, `--shadow-*`, `--ease-*`) map to `bg-*`, `p-*`, `rounded-*`, `font-*`, `shadow-*`, etc.

```css
/* app.css */
@import "tailwindcss";

@theme {
  --color-brand-500: oklch(0.62 0.20 256);
  --color-brand-600: oklch(0.54 0.19 256);
  --color-bg: #ffffff;
  --color-surface: #f9fafb;
  --color-fg: #111827;

  --font-sans: "Inter", system-ui, sans-serif;
  --font-mono: "Geist Mono", ui-monospace, monospace;

  --radius-md: 8px;
  --shadow-md: 0 2px 4px -1px rgb(0 0 0 / 0.06), 0 4px 8px -2px rgb(0 0 0 / 0.10);

  --ease-standard: cubic-bezier(0.2, 0, 0, 1);
}
```

Now `bg-brand-500`, `rounded-md`, `shadow-md`, `font-sans`, `ease-standard` all exist. Override Tailwind's default scale by redefining its variables here.

---

## Wiring into shadcn (`:root` / `.dark`)

shadcn expects semantic CSS variables on `:root` (light) and `.dark` (dark), referenced as `hsl(var(--token))` or directly. Map your primitives into shadcn's role names.

**Do:** define primitives once, alias them into shadcn's semantic names in both blocks.
**Don't:** duplicate raw hex across `:root` and `.dark` with no primitive layer.

---

## Copy-paste starter token set

```css
:root {
  /* ── 1. PRIMITIVES (raw values — never used by components) ── */
  --blue-50:#eff6ff; --blue-100:#dbeafe; --blue-300:#93c5fd; --blue-400:#60a5fa;
  --blue-500:#3b82f6; --blue-600:#2563eb; --blue-700:#1d4ed8; --blue-900:#1e3a8a;
  --gray-0:#ffffff; --gray-50:#f9fafb; --gray-100:#f3f4f6; --gray-200:#e5e7eb;
  --gray-300:#d1d5db; --gray-400:#9ca3af; --gray-600:#4b5563; --gray-700:#374151;
  --gray-900:#111827; --gray-950:#0a0a0a;
  --green-500:#16a34a; --amber-500:#d97706; --red-500:#dc2626;

  /* ── 2. SEMANTIC (components use these) ── */
  --color-bg:            var(--gray-0);
  --color-surface:       var(--gray-50);
  --color-surface-raised:var(--gray-0);
  --color-border:        var(--gray-200);
  --color-border-strong: var(--gray-300);
  --color-fg:            var(--gray-900);   /* primary text */
  --color-fg-muted:      var(--gray-600);   /* secondary text */
  --color-fg-subtle:     var(--gray-400);   /* placeholder */
  --color-primary:       var(--blue-500);
  --color-primary-hover: var(--blue-600);
  --color-primary-active:var(--blue-700);
  --color-primary-fg:    var(--gray-0);     /* text on primary */
  --color-ring:          var(--blue-500);
  --color-success:       var(--green-500);
  --color-warning:       var(--amber-500);
  --color-error:         var(--red-500);

  /* Spacing (8pt grid, 4px sub-grid) */
  --space-1:.25rem; --space-2:.5rem; --space-3:.75rem; --space-4:1rem;
  --space-6:1.5rem; --space-8:2rem; --space-12:3rem; --space-16:4rem; --space-24:6rem;

  /* Radius */
  --radius-sm:4px; --radius-md:8px; --radius-lg:12px; --radius-xl:16px; --radius-full:9999px;

  /* Shadow (layered) */
  --shadow-xs:0 1px 2px 0 rgb(0 0 0/.05);
  --shadow-sm:0 1px 2px 0 rgb(0 0 0/.06),0 1px 3px 0 rgb(0 0 0/.10);
  --shadow-md:0 2px 4px -1px rgb(0 0 0/.06),0 4px 8px -2px rgb(0 0 0/.10);
  --shadow-lg:0 4px 8px -2px rgb(0 0 0/.06),0 12px 20px -4px rgb(0 0 0/.12);
  --shadow-xl:0 8px 16px -4px rgb(0 0 0/.08),0 24px 40px -8px rgb(0 0 0/.16);

  /* Typography */
  --font-sans:"Inter",system-ui,-apple-system,sans-serif;
  --font-mono:"Geist Mono",ui-monospace,monospace;
  --text-xs:.75rem; --text-sm:.875rem; --text-base:1rem; --text-lg:1.125rem;
  --text-xl:1.25rem; --text-2xl:1.5625rem; --text-3xl:1.9375rem; --text-4xl:2.4375rem;
  --leading-tight:1.15; --leading-snug:1.35; --leading-normal:1.5; --leading-relaxed:1.6;
  --weight-normal:400; --weight-medium:500; --weight-semibold:600; --weight-bold:700;

  /* Z-index ladder */
  --z-base:0; --z-dropdown:1000; --z-sticky:1100; --z-overlay:1200;
  --z-modal:1300; --z-popover:1400; --z-toast:1500; --z-max:9999;

  /* Motion */
  --duration-fast:150ms; --duration-normal:250ms; --duration-slow:400ms;
  --ease-standard:cubic-bezier(0.2,0,0,1);
  --ease-out:cubic-bezier(0,0,0.2,1);
  --ease-spring:cubic-bezier(0.34,1.56,0.64,1);
}

.dark {
  /* Remap ONLY the semantic layer — primitives stay constant */
  --color-bg:            var(--gray-950);   /* #0a0a0a, not #000 */
  --color-surface:       #171717;
  --color-surface-raised:#222222;
  --color-border:        #333333;
  --color-border-strong: #404040;
  --color-fg:            #ededed;            /* off-white, not #fff */
  --color-fg-muted:      var(--gray-400);
  --color-fg-subtle:     var(--gray-600);
  --color-primary:       var(--blue-400);   /* lighter for contrast on dark */
  --color-primary-hover: var(--blue-300);
  --color-primary-active:var(--blue-300);
  --color-primary-fg:    var(--gray-950);
  --color-ring:          var(--blue-400);
  --color-success:#22c55e; --color-warning:#f59e0b; --color-error:#f87171;

  /* Shadows barely show on dark — add a top inner highlight for elevation */
  --shadow-sm:0 1px 2px 0 rgb(0 0 0/.4), inset 0 1px 0 rgb(255 255 255/.04);
  --shadow-md:0 4px 12px -2px rgb(0 0 0/.5), inset 0 1px 0 rgb(255 255 255/.05);
}
```

Usage in components (semantic only):

```css
.card   { background:var(--color-surface); border:1px solid var(--color-border);
          border-radius:var(--radius-lg); box-shadow:var(--shadow-sm); padding:var(--space-6); }
.btn    { background:var(--color-primary); color:var(--color-primary-fg);
          border-radius:var(--radius-md); padding:var(--space-2) var(--space-4);
          transition:background var(--duration-fast) var(--ease-standard); }
.btn:hover { background:var(--color-primary-hover); }
.input:focus-visible { outline:none; box-shadow:0 0 0 3px color-mix(in oklch,var(--color-ring) 40%,transparent); }
```

---

## Theming & multi-brand support

| Strategy | Mechanism | Use |
|----------|-----------|-----|
| **Light/Dark** | `.dark` class remaps semantic tokens | Standard |
| **Multi-brand** | `[data-brand="acme"]` overrides primitives → semantics cascade | White-label, partner themes |
| **Density** | `[data-density="compact"]` remaps `--space-*` | Comfortable/compact toggle |
| **Runtime** | set CSS vars via JS (`el.style.setProperty`) | User-customizable accents |

```css
[data-brand="acme"]  { --color-primary: var(--blue-500); --radius-md: 8px; }
[data-brand="vertex"]{ --color-primary: #7c3aed;        --radius-md: 4px; }
```

Because components only read semantic tokens, **one attribute swaps an entire brand** — no component edits.

**Do:** keep the semantic layer as the only thing themes touch.
**Don't:** branch component CSS per brand — re-point tokens instead.

---

## Agent checklist

- [ ] Structure tokens as primitive → semantic → component; components read semantic only.
- [ ] Never hardcode hex/px in components or reference a primitive directly.
- [ ] Name by role, not value (`--color-fg-muted`, not `--gray-light`); kebab-case with category prefix.
- [ ] Define one base radius and derive the scale; use the nested-radius rule for concentric corners.
- [ ] Use 2-stop layered shadows; scale alpha+spread with elevation; never a single harsh blur.
- [ ] In dark mode express elevation with lighter surfaces, not shadows; add a faint top inner highlight.
- [ ] Keep UI motion 150–250ms with ease-out; animate only transform/opacity; honor reduced-motion.
- [ ] Wire tokens into Tailwind v4 via `@theme` so utilities generate from your variables.
- [ ] For shadcn, alias primitives into semantic roles in both `:root` and `.dark`.
- [ ] Re-theme by remapping the semantic layer (class or `data-*` attribute), never per-component CSS.
- [ ] Use `#0a0a0a`/`#ededed` for dark base/fg and the lighter brand step for accents.
