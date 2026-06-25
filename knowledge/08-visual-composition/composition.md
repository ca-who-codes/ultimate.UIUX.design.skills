# Composition

> Purpose: The master method for arranging elements on a single static canvas (poster, slide, social square, print page) so the eye lands where you want it, in the order you want, and the page reads as intentional rather than assembled.

**When to read this:** Before laying out any fixed-canvas design — whenever you're deciding where things go, what dominates, and how the eye should travel. This is the static-design analog of the screen-side layout file; pair it with [imagery-and-icons.md](./imagery-and-icons.md), [brand-systems.md](./brand-systems.md), and the shared foundations ([../02-foundations/layout-spacing.md](../02-foundations/layout-spacing.md), [../02-foundations/typography.md](../02-foundations/typography.md), [../02-foundations/color.md](../02-foundations/color.md), [../01-principles/design-principles.md](../01-principles/design-principles.md)). For exact canvas dimensions and safe areas see [format-specs.md](./format-specs.md).

---

## The one rule above all rules: one dominant element

A static canvas has **one** job: deliver a single message in one glance. Decide the **focal point** — the one thing the viewer must see first — before placing anything else. Everything else is subordinate to it.

- **Hierarchy of three.** Most strong layouts have exactly three tiers: a **hero** (focal point — the headline, the product, the face, the one number), a **support** layer (subhead, supporting copy, secondary image), and **detail** (metadata, logo, date, fine print). If you can't name which element is the hero, the design has no focal point — fix that first.
- **The 60/30/10 of attention.** Give the hero ~60% of the visual weight, support ~30%, detail ~10%. Two elements fighting for "biggest" = no focal point = the eye bounces and gives up.
- **One canvas, one idea.** If the brief has two equally-important messages, you need two canvases (or two slides), not one crowded one.

| Do | Don't |
|----|-------|
| Pick the single most important element and make it unmistakably dominant | Make the headline, the image, and the logo all "big and bold" |
| Force one clear entry point for the eye | Distribute emphasis evenly so nothing leads |
| Demote the logo/date/URL to the detail tier | Treat the logo as the hero (it almost never is) |

---

## Visual hierarchy: the five levers

Hierarchy = controlling the **order** the eye reads things. You have five tools. Use 2–3 in combination per element; one alone is weak, all five at once is noise.

| Lever | How it creates emphasis | Cheap, strong move |
|-------|------------------------|--------------------|
| **Size / scale** | Bigger = seen first. The single most powerful lever. | Make the hero 3–5× the body size, not 1.5× |
| **Weight** | Bolder/heavier draws the eye | Hero in Bold/Black, body in Regular |
| **Color** | One saturated element on a muted field jumps forward | Single accent color on the one thing that matters |
| **Position** | Top-left and optical center read first; isolated = important | Place the hero on a thirds intersection (below) |
| **Space** | Whitespace around an element isolates and elevates it | Surround the hero with empty space, crowd the detail |

**Contrast is the multiplier.** Hierarchy works through *difference*: big-next-to-small, bold-next-to-light, color-next-to-gray. A 40px headline only reads as "big" because the body is 16px. Push the gaps — timid contrast (18px vs 16px) reads as a mistake, not a hierarchy. See the type-scale ratios in [../02-foundations/typography.md](../02-foundations/typography.md); on a poster or hero slide go bigger and use larger jumps (ratio 1.5–2.0) than on a screen.

---

## Grids, margins & the safe area

A grid is the invisible skeleton that makes a layout feel composed instead of accidental. Even "free" layouts ride a grid underneath.

### Canvas margin (the frame)
Reserve a consistent outer margin on all four sides — nothing important touches the edge.

| Canvas | Margin guideline |
|--------|-----------------|
| Social square / story (1080px) | 64–96px (≈6–9%) |
| Slide (1920×1080) | 80–120px |
| A-series / letter print | 12–20mm, plus 3mm **bleed** beyond trim for anything running off-edge |
| Billboard / large format | Keep all text in the central 70–80% — edges get cropped/curved |

- **Safe area** = the inner zone where critical content (text, faces, logo) must live, so platform UI (story tap-zones, profile chrome) or print trimming never clips it. On Instagram stories keep text out of the top ~250px and bottom ~250px. See [format-specs.md](./format-specs.md) for per-platform safe areas.
- **Bleed vs trim vs safe** (print): design extends to *bleed* (3mm past trim), the page is cut at *trim*, and content stays inside the *safe* margin (3mm inside trim). Background art must reach bleed or you risk a white sliver at the cut.

### Column & modular grids
- **Columns** (4 / 6 / 12) divide width; place text and image blocks to span whole columns, align to column edges, and use the consistent gutter between them. 12 columns is the most flexible (divides into halves, thirds, quarters).
- **Modular grid** = columns *plus* rows → a matrix of cells. Ideal for dense layouts (magazines, infographics, multi-photo posters): every block snaps to cell boundaries, so even busy pages feel orderly.
- **Baseline grid**: snap all text to a common vertical rhythm (line increments, e.g. every 8px) so columns of type align line-for-line.
- Keep all spacing on one scale — reuse the **8pt system** ([../02-foundations/layout-spacing.md](../02-foundations/layout-spacing.md)); don't invent one-off gaps.

```
Thirds grid + margin + focal point on an intersection
┌─────────────────────────────────────────┐
│  ← margin                                 │   ● = focal point sits on a
│   ┌───────┬───────┬───────┐               │       power-point intersection
│   │       │       │       │               │   ─ subhead rides the lower
│   │       │     ● │       │  ← upper-right │       third line for stability
│   │       │       │       │     power pt   │
│   ├───────┼───────┼───────┤  ← lines at    │   Negative space (left + below)
│   │       │       │       │     1/3, 2/3    │       isolates and elevates ●
│   │       │       │       │               │
│   ├───────┼───────┼───────┤               │
│   │  HERO TEXT on lower-third line ──────► │
│   │       │       │       │               │
│   └───────┴───────┴───────┘               │
│                              logo ▪ (detail)│
└─────────────────────────────────────────┘
```

---

## Rule of thirds & power points

Divide the canvas into a 3×3 grid with two horizontal and two vertical lines. The **four intersections are "power points"** — placing the focal point on one (rather than dead-center) creates a more dynamic, engaging composition.

- Place the **subject/eye/key object on an intersection**, not the middle. A face works well with the eyes on the upper-third line.
- Run **horizons and major dividers along a third line**, not across the center — a centered horizon splits the canvas into two competing halves.
- **Centered is not wrong** — it's stable, formal, symmetric (good for luxury, certificates, classic posters). Thirds is dynamic and casual. Choose deliberately; don't land off-center by accident.

## The golden ratio (use lightly)

The golden ratio (≈**1.618**) and its phi-grid intersections sit slightly nearer the center than the rule-of-thirds points. It's a useful sanity check for proportion — sizing a sidebar vs. main area (38/62), or a type scale built on 1.618. **Don't over-engineer it.** The rule of thirds gets you 95% of the benefit with none of the math; reach for golden-ratio proportions only when you want an especially "settled," classical feel.

---

## Balance & visual weight

Every element has **visual weight** — how much it pulls the eye. Balance = distributing weight so the canvas doesn't feel like it'll tip over.

**What adds weight:** large size · dark/saturated color · high contrast against background · dense detail/texture · isolation (space around it) · faces and text (we're wired to look) · hard edges over soft.

| Balance type | Feel | Use for |
|--------------|------|---------|
| **Symmetric** (mirror across center axis) | Formal, stable, calm, trustworthy, classic | Luxury, weddings, certificates, institutional, editorial covers |
| **Asymmetric** (different elements, equal total weight) | Dynamic, modern, energetic, editorial | Most contemporary marketing, tech, posters with a single strong image |
| **Radial** (elements radiate from a center) | Focused, hypnotic | Event posters, badges, hero focal moments |

- **Asymmetric balance** is the pro default: a big element on one side counterbalanced by a small high-contrast element (or a block of type) on the other, plus negative space. It looks intentional and alive where pure symmetry can look static.
- Test by squinting (below): if the page looks lopsided as a blur, redistribute weight — enlarge the light side, add an accent, or shift the focal point.

---

## Alignment & edge alignment

Misalignment is the #1 tell of amateur work. The fix is nearly free.

- **Every element aligns to something** — a grid column, a margin, or another element's edge. Nothing is placed "by eye."
- **Pick an edge and commit.** A left-aligned block should share one crisp left edge; ragged left edges read as sloppy. Prefer **flush-left** for body text (centered body text is hard to read past 2–3 lines).
- **Optical alignment beats mathematical.** Round shapes, punctuation, and italics must overshoot the margin slightly to *look* aligned. A bullet or quotation mark hung into the margin reads cleaner than one boxed inside it.
- **Reduce the number of alignment edges.** Two or three vertical alignment lines across the whole canvas looks tight; seven looks chaotic.

| Do | Don't |
|----|-------|
| Share strong left/right/top edges across blocks | Let each block float to its own arbitrary x-position |
| Hang bullets/quotes/dashes into the margin (optical) | Box punctuation inside, leaving a visual notch |
| Center only short, deliberate, symmetric layouts | Center long paragraphs of body copy |

---

## Proximity & grouping (Gestalt)

The brain groups things that are **close together** and reads things that are **far apart** as separate. This is your strongest tool for structure without lines or boxes — see the full Gestalt set in [../01-principles/design-principles.md](../01-principles/design-principles.md).

- **Proximity > borders.** Tighten the space *within* a group and widen the space *between* groups; you rarely need a box or divider line. A label belongs ~4–8px from its value but ~32px+ from the next pair.
- **Consistent gaps signal relationships.** Equal spacing = equal hierarchy = a set; varied spacing = a sequence or ranking.
- **Common region & similarity:** a shared background tint, shape, or color binds items into a unit even when they're not adjacent.
- The most common composition bug: uniform spacing everywhere, so nothing groups. Vary the gaps.

---

## Figure/ground & contrast

The viewer must instantly separate **figure** (subject) from **ground** (background). Weak figure/ground = the message disappears.

- Maximize subject-vs-background contrast — in **lightness first** (light subject on dark ground or vice-versa), then color, then sharpness. Lightness contrast survives grayscale, color-blindness, and tiny thumbnails; color contrast alone doesn't.
- Don't place a busy subject on a busy background. Calm one side: blur, darken, desaturate, or mask the background (see image treatment in [imagery-and-icons.md](./imagery-and-icons.md)).
- Beware **ambiguous figure/ground** (where it's unclear what's foreground) unless it's a deliberate, clever effect.
- Keep contrast ratios honest for any text: 4.5:1 body, 3:1 large/UI — same bar as screens ([../02-foundations/color.md](../02-foundations/color.md)).

---

## Directional cues & eye-flow

You control the *path* the eye travels, not just where it starts.

- **Z-pattern** — for sparse layouts (posters, ads, simple slides): eye enters top-left → moves right → diagonally down-left → right along the bottom. Put the logo top-left, the hero/headline along the top, the supporting visual in the middle, and the **call-to-action at the bottom-right** (the natural exit point).
- **F-pattern** — for text-heavy layouts (long-copy flyers, editorial, web-like pages): readers scan the top line, drop down, scan a shorter second line, then skim the left edge. Front-load meaning at the start of lines and headings.
- **Leading lines** — roads, arms, gazes, arrows, edges, gradients — physically point the eye toward the focal point. A subject's **gaze direction** is powerful: viewers follow where a face looks, so face the model *toward* your headline/CTA, never off-canvas.
- **Big-to-small** — the eye goes large → small, high-contrast → low, color → gray, top → bottom. Sequence your hierarchy along that natural slide.
- Give the eye an **exit**: a single CTA, URL, or logo at the end of the path. Don't trap it in a loop.

---

## Whitespace / negative space

Negative space (empty area) is an active design element, not wasted room. It is the cheapest way to look premium.

- **Whitespace creates focus.** Isolating the hero in empty space makes it shout louder than making it bigger. Apple-style "one product, vast empty field" works because of restraint.
- **Macro space** (around major blocks) sets the calm/luxury vs. busy/urgent tone; **micro space** (between letters, lines, label/value) determines legibility and grouping.
- **More space ≈ more premium / more confident.** Cramming = budget, sale, urgency. Match the void to the brand.
- Don't fill every corner because it "looks empty." Resist the urge to add a flourish, a third icon, a background texture. Empty is finished.

| Do | Don't |
|----|-------|
| Let the hero breathe in a generous void | Fill margins with decorative noise |
| Use space to group and separate (proximity) | Distribute space evenly so nothing reads as grouped |
| Treat negative space as a shape you compose | Treat it as leftover area to be filled |

---

## Scale & dramatic size contrast

Big size *jumps* — not gentle steps — create impact and feeling.

- A single **oversized element** (a huge number, one giant word bleeding off the edge, a face filling the frame) is instantly bold and memorable. The contrast with smaller elements is what sells it.
- **Crop in.** A tightly cropped subject (face, product detail) feels more intimate and dramatic than a small subject lost in space.
- **Type as image.** At poster scale, set the headline so large it becomes the main graphic. Tighten letter-spacing on huge type (large sizes look loosely spaced by default).
- Pair one dominant element with quiet support — drama needs calm around it or it reads as chaos.

---

## The squint test

The fastest self-review for any static canvas. **Blur your eyes (or the file) until detail disappears and only shapes/values remain**, then check:

1. **Does one element clearly dominate?** If not, your focal point is too weak — push size/contrast.
2. **Does the eye land where you intended first?** If it goes somewhere else, that element is over-weighted.
3. **Is the value structure (light/dark) balanced and legible?** Lopsided blur = rebalance weight.
4. **Do related things still group?** If everything dissolves into uniform gray mush, you lack contrast and spacing variation.

Squinting strips away color seduction and detail and shows the *bones*. Do it on every draft.

---

## Framing & cropping

- **Crop to the message.** Remove anything that doesn't serve the focal point. A tighter crop is almost always stronger than "fit it all in."
- **Frame within the frame** — an arch, window, doorway, or block of color around the subject concentrates attention.
- **Respect the subject's gaze/motion space** — leave room in front of where a subject looks or moves; cramping the leading edge feels claustrophobic.
- **Mind the aspect ratio** the canvas demands (square vs. vertical vs. wide) — recrop the focal point for each, don't just letterbox. (Cropping mechanics live in [imagery-and-icons.md](./imagery-and-icons.md).)

## Tension & dynamism

A perfectly safe, centered, evenly-spaced layout is calm but forgettable. Controlled imbalance creates energy.

- **Bleed off the edge.** Letting an image or huge word run past the trim implies the world continues and adds momentum (must extend to print bleed).
- **Diagonals = energy.** A tilted element, a diagonal leading line, or text on an angle injects movement; horizontals/verticals = stability.
- **Asymmetry + negative space = sophisticated tension** — a small element near a corner pulling against a large mass and a big void.
- **Break the grid once, on purpose.** A single element that violates the grid (rotated, oversized, overlapping a margin) becomes the hero by contrast — but only if everything else obeys the grid. Break it twice and you have chaos.

---

## Agent checklist

- [ ] Name the single focal point first; make it unmistakably dominant (~60% of visual weight) before placing anything else.
- [ ] Build a 3-tier hierarchy (hero / support / detail) using 2–3 of: size, weight, color, position, space — with bold, not timid, contrast.
- [ ] Lay everything on a grid (columns or modular) with consistent margins/safe area and bleed for print; keep spacing on the 8pt scale.
- [ ] Place the focal point on a rule-of-thirds power point (or center it deliberately for a formal feel), not by accident.
- [ ] Choose symmetric vs. asymmetric balance on purpose; distribute visual weight so the canvas doesn't tip.
- [ ] Align every element to a shared edge or grid line; flush-left body text; hang punctuation optically.
- [ ] Vary spacing to group related items (tight within, loose between) instead of borders.
- [ ] Ensure strong figure/ground separation in lightness first; keep busy subjects off busy backgrounds.
- [ ] Direct eye-flow with a Z- or F-pattern and leading lines/gaze; end on a single CTA or logo.
- [ ] Use generous negative space to elevate the hero; don't fill empty areas with decoration.
- [ ] Run the squint test: one thing dominates, the eye lands right, values balance, groups hold.
- [ ] Add controlled tension (a bleed, a diagonal, one grid-break) only with everything else disciplined.
