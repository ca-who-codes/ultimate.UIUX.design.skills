# Brand Systems

> Purpose: How to build and apply a coherent visual system across a *set* of marketing assets — decks, social posts, print, email — so every piece looks like one family, and how to derive that whole kit from a single brand color plus one font pairing.

**When to read this:** When you're producing more than one asset, building a campaign or template set, applying an existing brand, or being asked to make scattered pieces "look consistent." Pair with [composition.md](./composition.md) (arranging each canvas), [imagery-and-icons.md](./imagery-and-icons.md) (keeping imagery on-brand), and the shared foundations ([../02-foundations/color.md](../02-foundations/color.md), [../02-foundations/typography.md](../02-foundations/typography.md), [../02-foundations/layout-spacing.md](../02-foundations/layout-spacing.md)). The screen-side analog — turning these decisions into reusable code tokens — is [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md). For dimensions and export specs see [format-specs.md](./format-specs.md).

---

## What a brand system is (and why)

A brand system is a small set of **fixed decisions** — colors, type, spacing, imagery, marks — reused across every asset so the audience recognizes you instantly. Consistency is the entire point: a viewer should know a piece is yours *before* reading a word. The job here is to **define once, reuse everywhere** — the static-design equivalent of design tokens.

> The mark of amateur output is that ten assets look like ten different brands. The mark of pro output is that ten assets look like one brand expressed ten ways.

---

## The elements of a brand kit

A complete kit defines all of the following. Anything left undefined becomes an inconsistency someone improvises later.

| Element | What it fixes |
|---------|--------------|
| **Logo + variations** | Primary, secondary/horizontal, icon/mark-only, on-light, on-dark, mono |
| **Color palette + roles** | Brand, neutral, accent, semantic — *with assigned roles*, not just swatches |
| **Type system** | Display/heading + body fonts, scale, weights, tracking, case rules |
| **Spacing & grid** | One spacing scale + margin/column system applied to every format |
| **Imagery style** | Photo grade/duotone, illustration style, do's & don'ts, subject guidance |
| **Iconography** | One icon family, weight, size rules |
| **Graphic motifs / shapes** | Signature shapes, corner radius, dividers, frames, accent device |
| **Tone / mood** | The feeling every asset must convey (e.g. calm-premium vs. bold-energetic) |

The first three (logo, color, type) carry ~80% of recognition. Nail those before the rest.

---

## Logo usage

The logo is the most-abused asset; lock its rules first.

- **Clear space.** Reserve minimum empty space around the logo equal to a measured unit of the logo itself (commonly the cap-height or the width of the mark/"x"). Nothing — text, edge, other element — intrudes into it.
- **Minimum size.** Set a smallest legible size (e.g. ≥ 24px tall on screen, ≥ 10mm in print) below which the mark must not be used; switch to the icon-only version when smaller.
- **Variations, used correctly:**
  - *Primary* (full lockup) for hero/cover placements.
  - *Horizontal/stacked* alternates for tight aspect ratios.
  - *Icon/mark-only* for avatars, favicons, small spots, app icons.
  - *On-light* and *on-dark* versions — pick by background; never the dark logo on a dark photo.
  - *Monochrome / knockout* for single-color print, watermarks, embossing.
- **Do-not-distort rules:** never stretch/squash (lock aspect ratio), recolor outside approved variants, rotate, add effects (drop-shadow/glow/bevel), place on low-contrast or busy backgrounds, or re-create the lockup by hand.
- **Favicon / avatar / app icon:** use the mark-only version, centered with its own padding, tested at 16px and 512px. The full wordmark is illegible at favicon size.

| Do | Don't |
|----|-------|
| Keep full clear space around the logo | Crowd it against text or the canvas edge |
| Switch to mark-only below minimum size | Shrink the full lockup until the wordmark mushes |
| Use the on-dark version over dark backgrounds | Place the dark logo on a dark photo with no plate |
| Lock aspect ratio; use approved color variants only | Stretch, recolor, add a shadow, or rotate it |

---

## Color palette with roles

A palette is only useful when each color has a **job**. Assign roles, not just hex values — this is what keeps 50 assets consistent. (Build the actual ramps/contrast per [../02-foundations/color.md](../02-foundations/color.md).)

| Role | Typical allocation | Notes |
|------|--------------------|-------|
| **Brand / primary** | The signature hue; ~10% surface area | Headlines accents, key shapes, primary buttons |
| **Neutral / ink** | Text + most backgrounds; ~60% | Near-black ink + a few grays + off-white/paper |
| **Accent / secondary** | Sparingly; highlights, charts | Only if needed; keep it scarce |
| **Semantic** | Success/warning/error/info | Fixed meanings; consistent across all assets |
| **Background / surface** | Paper, dark, tinted surface options | Defined light *and* dark variants |

- **Apply 60-30-10 by area** (neutral / secondary / brand accent) on every canvas so brand color stays scarce and powerful.
- **Define light *and* dark application** so a piece works on white paper and on a dark social background without re-deciding color each time.
- **Accessibility is part of the brand.** Verify brand-on-background and text-on-brand contrast (4.5:1 body, 3:1 large/UI) — many brand 500s only pass for large text, so designate the darker step for body-size use. Full guidance in [../02-foundations/color.md](../02-foundations/color.md).

---

## Type system

One pairing, used the same way everywhere, is more recognizable than any single clever font.

- **Two fonts, defined roles:** a **display/heading** face (personality) + a **body/UI** face (legibility). One can do both if it has enough weights. Mechanics of pairing and scale: [../02-foundations/typography.md](../02-foundations/typography.md).
- **Fix the scale and weights** the brand uses (e.g. Display 700, H1 600, body 400, caption 500) and reuse them — don't improvise a new size per asset.
- **Fix case & tracking rules** (e.g. eyebrows in uppercase +8% tracking; headlines tight tracking; body default). These small consistencies are strong recognition cues.
- **Embed/outline fonts on export** so the brand face survives in PDFs and on machines without it (see [format-specs.md](./format-specs.md) / [../13-production/production-and-tools.md](../13-production/production-and-tools.md)). Define a system-font fallback for email/web.

---

## Graphic motifs & shapes

The "secret sauce" that makes a system feel designed rather than templated.

- **A signature device** — a specific corner radius, a recurring shape (circle/arch/blob), a divider style, an accent underline, a framing rule, a dot-grid, a sticker/lozenge. Reuse it across assets as a recognizable fingerprint.
- **Consistent corner radius** everywhere (all-sharp, or one radius value), matching the logo's geometry.
- **Derive shapes from the logo or letterforms** for cohesion (a brand built on circles uses circular crops, round buttons, dot bullets).
- Keep motifs **subordinate** — they accent, they don't compete with the message.

---

## Consistency across decks / social / print / email

Same system, different containers. The system is constant; only the **frame and aspect ratio** change.

| Surface | Constant (from the kit) | Adapts per surface |
|---------|------------------------|--------------------|
| **Decks/slides** | Colors, type scale, motifs, logo | 16:9 master layouts, title/section/content templates |
| **Social** | Same, plus duotone/photo grade | 1:1 / 4:5 / 9:16 crops; safe-area for platform UI; bolder, fewer words |
| **Print** | Same, in CMYK at 300ppi | Bleed/trim/safe; mm units; embed fonts; richer detail |
| **Email** | Same, with web-safe fallback fonts | Single-column, ~600px, bulletproof buttons, alt text, dark-mode safe |

- **Master templates per format.** Build one slide master, one 1:1 social master, one story master, one email master — then everything inherits. Never start a sibling asset from scratch.
- **Reusable layout blocks.** Standardize repeatable units (a stat block, a quote card, a title bar, a CTA footer, a logo-lockup corner) and reuse them across assets and formats.
- **Recompose, don't just resize.** Reflowing 16:9 → 9:16 is a layout decision (re-stack, re-crop the focal point) per [composition.md](./composition.md) — not a stretch.
- **A "brand sheet."** Keep a single one-page reference showing the logo variants, palette with roles + hex, type scale, spacing, imagery samples, icon set, motifs, and do/don'ts. Every asset is checked against it. This is the human-readable analog to [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md).

---

## Template thinking: define once, reuse

The core discipline. Treat every recurring decision as a **named, reusable definition**:

- **Define once:** the title-slide layout, the stat block, the photo treatment, the button, the footer lockup, the color roles, the type styles. Name them.
- **Reuse, don't recreate.** Each new asset assembles from existing blocks; you only ever design something genuinely new.
- **Centralize the source.** One master file / one set of named styles → change the brand color or font in one place, everything updates. Hard-coded one-off values are the bugs of design.
- This *is* tokenization for static design — same philosophy as [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md), expressed as master files and named styles instead of CSS variables.

---

## Tone & mood alignment

Visual choices must match the brand's intended feeling. Decide the adjectives first (e.g. *calm, premium, trustworthy* — or *bold, playful, urgent*), then let them drive every variable:

| If the tone is… | Lean toward… |
|-----------------|--------------|
| **Premium / calm / luxury** | Lots of negative space, restrained palette, refined serif or light sans, subtle motion, symmetry, muted imagery |
| **Bold / energetic / youthful** | Big type, high contrast, saturated accent, asymmetry, diagonals, expressive crops |
| **Trustworthy / institutional** | Blue/neutral palette, structured grid, clean sans, even spacing, conservative imagery |
| **Warm / human / friendly** | Rounded shapes, hand-drawn touches, warm color, candid photography |

Mismatch (a luxury brand using a crowded, neon, jokey layout) breaks trust faster than any single ugly element. Keep tone consistent across the whole set.

---

## Workflow: derive a coherent kit from one color + one font pairing

When handed only a brand color and a font (or asked to invent a system), build the rest deterministically:

1. **Anchor color.** Take the brand hue → generate a full tint/shade ramp (50–950) in OKLCH, easing chroma at the ends ([../02-foundations/color.md](../02-foundations/color.md)).
2. **Neutrals.** Build a gray ramp tinted ±5° toward the brand hue (cohesion), with an off-white "paper" and a near-black "ink." This is your 60%.
3. **Accent & semantics.** Add one accent only if needed; lock success/warning/error/info to fixed, accessible hues distinct from the brand.
4. **Type pairing.** Choose a display + body pair (or one versatile family); fix the scale (e.g. 1.5× ratio for marketing), weights, and case/tracking rules per [../02-foundations/typography.md](../02-foundations/typography.md).
5. **Spacing & grid.** Adopt the 8pt scale and a 12-column (or modular) grid with defined margins/safe areas per format ([../02-foundations/layout-spacing.md](../02-foundations/layout-spacing.md)).
6. **Imagery rule.** Decide one photo treatment (e.g. brand-color duotone) or one illustration style, plus one icon family ([imagery-and-icons.md](./imagery-and-icons.md)).
7. **Motif.** Derive a corner radius and one signature shape from the logo/letterforms.
8. **Verify contrast & tone**, then **codify** it all on a one-page brand sheet and master templates. Now every asset is assembly, not invention.

This produces a coherent system from minimal input — the same way a screen-side token set is derived from one brand hue and a font.

---

## Brand kit checklist (every asset/spec a complete kit defines)

| Category | Must define |
|----------|-------------|
| **Logo** | Primary lockup · horizontal/stacked alt · icon/mark-only · on-light · on-dark · mono/knockout · clear-space rule · minimum size · misuse list |
| **Color** | Brand ramp (50–950) · neutral/ink ramp · paper + dark surfaces · accent(s) · semantic 4 · 60-30-10 roles · light+dark application · contrast-verified pairings |
| **Type** | Display font · body font · scale + ratio · weights used · line-height/measure · case & tracking rules · fallback fonts · embed/outline-on-export rule |
| **Spacing & grid** | 8pt scale · column/modular grid · per-format margins · safe areas · bleed/trim (print) |
| **Imagery** | Photo treatment/grade or duotone recipe · illustration style · subject do's & don'ts · text-over-image rule · resolution per medium |
| **Iconography** | Single icon family · stroke weight · grid/size scale |
| **Motifs** | Corner radius · signature shape/device · divider/frame style · accent device |
| **Tone** | 3–5 mood adjectives · examples of on-brand vs. off-brand |
| **Templates** | Slide master(s) · social masters (1:1 / 4:5 / 9:16) · email master · reusable blocks (stat/quote/CTA/title) · the one-page brand sheet |

---

## Agent checklist

- [ ] Lock logo, color, and type first — they carry ~80% of recognition.
- [ ] Define every logo variation (light/dark/mono/mark-only) plus clear-space, minimum size, and a misuse list; never distort or shadow the logo.
- [ ] Assign a *role* to every color and apply 60-30-10 by area; verify contrast (4.5:1 body, 3:1 large/UI) in light and dark.
- [ ] Fix one type pairing with a set scale, weights, and case/tracking rules; embed or outline fonts on export.
- [ ] Adopt one spacing scale, grid, and per-format margins/safe areas/bleed across every surface.
- [ ] Choose one photo treatment, one illustration style, and one icon family — and apply them everywhere.
- [ ] Add a signature motif (corner radius + shape) derived from the logo to fingerprint the system.
- [ ] Build master templates and reusable blocks per format; assemble new assets from them rather than starting fresh.
- [ ] Recompose (not stretch) when moving between aspect ratios.
- [ ] Pick 3–5 tone adjectives and let them drive every visual variable; keep tone consistent across the set.
- [ ] Derive a full kit deterministically from one color + one font pairing when that's all you're given.
- [ ] Codify everything on a one-page brand sheet and check every asset against it before shipping.
