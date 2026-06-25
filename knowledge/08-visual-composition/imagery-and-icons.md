# Imagery & Icons

> Purpose: How to choose, treat, crop, and combine photography, illustration, icons, textures, and AI-generated images on a static canvas — including the recipe for making text readable over any image.

**When to read this:** Whenever a design uses a photo, illustration, icon set, background texture, gradient, or generated image — i.e. almost every poster, slide, social post, or print piece. Pair with [composition.md](./composition.md) for placement, [brand-systems.md](./brand-systems.md) for keeping imagery consistent across a set, and the shared foundations ([../02-foundations/color.md](../02-foundations/color.md), [../02-foundations/typography.md](../02-foundations/typography.md)). For DPI/resolution targets per medium see [format-specs.md](./format-specs.md); for the tools that perform these treatments see [../13-production/production-and-tools.md](../13-production/production-and-tools.md).

---

## Choosing photography

The image is usually the loudest element on the canvas; a weak or cliché photo sinks the whole design.

| Choose | Avoid |
|--------|-------|
| **Authentic, specific moments** — real texture, candid light, genuine expression | Stock clichés: forced handshakes, laughing-alone-with-salad, generic "diverse team at laptop," thumbs-up |
| **A clear single subject** with obvious focal clarity | Busy scenes with no subject; everything in focus and equally important |
| **Consistent treatment** across the set (same light, grade, mood) | A patchwork of mismatched stock from five sources |
| **Real depth of field** — subject sharp, background soft | Flat phone snapshots with no subject separation |
| **Negative space built in** (or room to add it) for text | Edge-to-edge busy frames with nowhere for type |
| **Honest, on-brand representation** | Over-retouched, plastic, obviously-fake compositing |

- **Subject & focal clarity first.** Before treatment, confirm the photo has one clear subject and that the subject reads at the size you'll use it. A great grade can't save a photo with no point.
- **Consistency makes mismatched stock look intentional.** When you must mix sources, unify them with a single treatment (one duotone, one color grade, one grain) — see below.
- **Resolution must match the medium.** Screen ≈ 72–150 ppi at display size; print needs **300 ppi at final size** (large-format/billboards can drop to 100–150 ppi because viewing distance is large). Never upscale a small image to fill a print page — see [format-specs.md](./format-specs.md).

---

## Image treatment

Treatment unifies disparate photos, controls mood, and (critically) buys legibility for text. Apply the *same* recipe across a set so it reads as one family.

| Treatment | What it does | When to use |
|-----------|--------------|-------------|
| **Duotone** | Maps shadows→one brand color, highlights→another; flattens detail, screams "branded" | Posters, covers, event series — instant cohesion across mixed stock |
| **Color overlay / tint** | Solid brand color at 20–60% over the photo | Brand-coloring any photo; calming a busy background |
| **Dark/light scrim** | Black or white gradient over part of the image | Carving out a calm zone for text (the legibility workhorse) |
| **Gradient map / wash** | Smooth color gradient blended into the image | Modern, atmospheric hero backgrounds |
| **Grain / noise** | Adds film texture; kills gradient banding; warms digital flatness | Editorial, music, premium "analog" feel; over big flat fields/gradients |
| **Blur (background)** | Pushes background back, creates depth, makes a calm bed for foreground text | Hero text over photography; faux depth-of-field |
| **Desaturate / mono** | Strips color so an accent color elsewhere pops | When color in the photo competes with your brand accent |

- **Commit to one grade.** Pick a treatment and apply it to every image in the piece/set. Inconsistent grading is the fastest way to look amateur.
- **Duotone is the cheapest unifier** for a deck or campaign built from mixed stock — one brand-color duotone makes ten random photos look like a deliberate set.
- **Grain/noise (2–5%)** over large gradients prevents banding and adds tactile richness; keep it subtle.

---

## Make text readable over any image (the recipe)

Text over a photo fails when local contrast drops below ~4.5:1 *anywhere* a letter sits. Work through these in order; stop when text clears 4.5:1 (3:1 for large/display) measured at the worst pixel:

1. **Place text in the calmest, most uniform region** of the image (sky, blurred area, shadow, an out-of-focus wall) — never across the busy/detailed part. This is free and the most important step.
2. **Add a scrim** — a semi-transparent overlay between image and text:
   - Full-frame flat scrim: **dark 40–60% black** (`rgba(0,0,0,0.4–0.6)`) for white text; **white 50–70%** for dark text.
   - **Gradient scrim** (preferred — keeps more image visible): opaque at the text edge → transparent toward the open image, e.g. bottom third `rgba(0,0,0,0.7)` → `0` upward. Text sits in the dense end.
3. **Darken/blur the image itself** if a scrim alone isn't enough — drop exposure, or Gaussian-blur the region under the text so no high-frequency detail breaks up the letters.
4. **Boost the text** — heavier weight, larger size, and as a last resort a subtle text-shadow (`0 1px 3px rgba(0,0,0,0.5)`) or a knockout panel/lozenge behind the text.
5. **Verify at final size** with a contrast checker on the worst spot, not the average. A headline that passes over the dark corner can fail over the bright cloud in the same image.

> Rule of thumb: **white text → 40–60% dark scrim; dark text → 50–70% light scrim; gradient scrim beats flat** because it preserves the image where there's no text. Never trust "it looks fine" — a 4.5:1 check per the [color foundations](../02-foundations/color.md) is non-negotiable.

| Do | Don't |
|----|-------|
| Use a gradient scrim anchored under the text | Slap a flat 20% scrim and hope (usually fails) |
| Put type over sky/blur/shadow — the calm zones | Run a thin headline across the subject's busy face/pattern |
| Test contrast at the single worst pixel | Eyeball the average brightness |
| Pick images *with* a built-in calm area for text | Force text onto a photo that has nowhere quiet to live |

---

## Illustration

Illustration trades photo-realism for personality, clarity, and total control of style and content (no stock-photo limits).

| Style | Personality | Good for |
|-------|-------------|----------|
| **Flat / geometric** | Clean, modern, friendly, scalable | Tech, SaaS, explainer, onboarding |
| **Line / outline** | Light, elegant, editorial | Minimal brands, icon-adjacent spots |
| **Isometric** | Technical, "systems," product-y | Architecture diagrams, how-it-works |
| **Hand-drawn / textured** | Warm, human, crafted, approachable | Food, kids, wellness, indie brands |
| **3D / gradient blob** | Playful, contemporary, eye-catching | Fintech, app marketing, hero spots |
| **Editorial / conceptual** | Sophisticated, idea-driven | Long-form, opinion, abstract concepts |

- **Use illustration over photo when:** the concept is abstract (security, growth, "the cloud"), you need on-brand color control, no honest photo exists, or you want a warmer/more playful tone than stock allows.
- **Pick one style and stay in it.** Mixing flat + 3D + hand-drawn in one piece looks like a clip-art accident. A single illustration style is itself a brand asset.
- **Match illustration color to the brand palette**, not the stock illustration's default colors.

---

## Iconography

Icons are a system, not a grab-bag. Inconsistency here is instantly visible.

- **One family, never mixed.** Pick a single icon set — **Lucide**, **Phosphor**, **Heroicons**, **Feather**, **Material Symbols** — and use only that set. Don't mix Lucide outlines with filled Material icons; the stroke weight, corner radius, and grid won't match.
- **Consistent stroke weight.** All icons share the same stroke (commonly 1.5–2px on a 24px grid). If you scale an icon up, scale the stroke proportionally — or use a weight variant — so a big icon doesn't look hairline-thin next to small ones.
- **Drawn on a shared grid** (typically 24×24 with consistent optical padding) so they align and feel evenly weighted. Phosphor and Lucide give you this for free.
- **One visual style:** all outline *or* all filled *or* all duotone — don't mix metaphors. Pick weight (thin/regular/bold) to match the brand's type weight.
- **Sizing:** keep icons on the spacing scale (16 / 20 / 24 / 32 / 48). Inline-with-text icons match the cap-height; standalone feature icons go large (48px+).
- **Optical alignment & balance.** Center icons optically, not mathematically — a triangle/play icon needs nudging right to look centered. Keep icons that appear together at matching visual weight, even if their bounding boxes differ.

| Do | Don't |
|----|-------|
| Use one icon family throughout | Pull icons from three sets because each "had the perfect one" |
| Keep stroke weight uniform across sizes | Let a scaled-up icon go hairline next to small ones |
| Match icon style/weight to the type weight | Pair a thin geometric font with chunky filled icons |
| Use icons to *aid* scanning, sparingly | Decorate every line with an icon until they're noise |

---

## Backgrounds: patterns, textures, meshes, gradients

A background sets mood and depth — but it's a *background*; it must never out-shout the foreground.

| Element | Tasteful use | Failure mode |
|---------|--------------|--------------|
| **Solid color** | Safest, most premium; lets content dominate | (none — when in doubt, solid) |
| **Subtle gradient** | Two adjacent hues, low angle, OKLCH-interpolated; depth without distraction | Rainbow/high-contrast gradients that fight the text |
| **Mesh gradient** | Soft multi-point color blend; modern hero beds | Garish neon mesh with no calm zone for content |
| **Geometric pattern** | Low-contrast, tiled motif tied to brand shapes | Busy high-contrast pattern under body text |
| **Texture (paper/grain/noise)** | Quiet tactile richness; anti-banding | Loud texture that lowers text contrast |
| **Photo background** | When the image *is* the message (needs a scrim) | Full-detail photo behind paragraphs of text |

- **Keep backgrounds low-contrast** relative to foreground content. If a pattern/texture drops your text below 4.5:1, mute it (lower opacity, blur, or scrim) or remove it.
- **Gradients:** keep hue span ≤ 60°, interpolate `in oklch`, add ~2% noise to prevent banding — see gradient guidance in [../02-foundations/color.md](../02-foundations/color.md).
- **Derive pattern/mesh colors from the brand palette** so the background reinforces identity instead of introducing rogue color.
- One background treatment per canvas. Gradient *and* pattern *and* photo = mud.

---

## Aspect ratios & focal-point-aware cropping

The same image must be recropped — not letterboxed — for each canvas shape.

| Ratio | Typical use |
|-------|-------------|
| **1:1** | Instagram feed square, profile, generic social |
| **4:5** | Instagram portrait (max feed real estate) |
| **9:16** | Stories, Reels, TikTok, vertical full-screen |
| **16:9** | Slides, YouTube, widescreen, presentations |
| **3:2 / 2:3** | Classic photography, print posters |
| **A-series (1:1.414)** | A4/A3 print posters, flyers |

- **Crop to protect the focal point.** Identify the subject/eyes/key object, then crop so it lands on a rule-of-thirds power point ([composition.md](./composition.md)) — never crop so the subject ends up dead-center-boring or, worse, half-out-of-frame.
- **Recompose per ratio.** Going 16:9 → 9:16 is a *re-crop* decision, not an auto-resize: a wide hero shot may need the subject re-centered and the rest discarded. Auto "smart crop" often cuts off heads — always verify.
- **Don't crop through a face, hand, or the natural joint of a body** (no cropping at the wrists/ankles/neck). Crop on the "long bones," not the joints.
- **Leave lead room** in the direction a subject faces or moves; crop tighter on the trailing edge.
- **Tight crop = drama/intimacy; wide crop = context/calm.** Pick to match the message.

---

## Image quality for the medium

- **Match resolution to output:** web ~150 ppi at display size; **print 300 ppi at final print size**; large-format 100–150 ppi (far viewing distance). See [format-specs.md](./format-specs.md).
- **Vectors (SVG) for logos, icons, line art** — infinitely scalable, crisp at any size. **Raster (PNG/JPG) for photos.** Never rasterize a logo you'll scale up; never expect a small JPG to enlarge cleanly.
- **Format:** JPG for photos (smaller), PNG for flat graphics/transparency, SVG for vector, WebP/AVIF for web when supported. Print pipelines want **CMYK** at 300 ppi; screen wants **sRGB** (see color-space notes in [../02-foundations/color.md](../02-foundations/color.md)).
- **Compress, but verify** — over-compressed JPGs show blocky artifacts that ruin a hero image. Inspect at 100%.
- **Never upscale a low-res source to fill a large canvas.** Re-shoot, re-source, or (carefully) use an AI upscaler — then check for invented detail.

---

## AI image generation basics

Treat generation as art-directed sourcing — you still need composition, treatment, and consistency.

### Prompt structure
Build prompts in this order — it reads like a shot brief and yields controllable results:

**`[subject] + [style/medium] + [lighting] + [composition/framing] + [color/mood] + [technical]`**

> *"A ceramic coffee cup on a linen cloth (subject), editorial product photography (style/medium), soft morning side-light from the left (lighting), centered with generous negative space above for text, shallow depth of field (composition), warm muted earth tones (color/mood), 50mm, high detail, 4:5 (technical)."*

- **Be specific and concrete.** "Soft side-light," "shallow depth of field," "negative space top-left for text," "shot on 35mm" beat vague adjectives. Name the framing you need for your layout.
- **Specify the aspect ratio** you'll actually place it in, and **build in negative space** for your text up front.

### Consistency across a set
- **Reuse a fixed style suffix** on every prompt (same medium + lighting + color + lens), changing only the subject — this is what makes a series look unified.
- **Lock a seed / use reference or style-reference images** where the tool supports it, to hold character/look across frames.
- **Unify in post anyway:** apply the *same* duotone/grade/grain to all generated images, exactly as you would for mixed stock — generation alone rarely matches frame-to-frame.

### Common pitfalls (always check)
- **Hands, fingers, teeth, ears, eyes** — the classic distortions; inspect and regenerate or retouch.
- **Garbled text/logos** — generators can't spell; never let AI render your headline or logo. Add real type/logos in the design tool, on top.
- **Off-brand color/lighting** — regrade to the brand palette; don't accept the model's default mood.
- **Symmetry & sameness** — generated faces/scenes drift toward generic; push specificity in the prompt.
- **Invented detail on upscale** — AI upscalers hallucinate texture; verify at 100%.
- **Rights & authenticity** — confirm usage rights/licensing for the tool, and disclose AI imagery where required; don't fake real people, places, or events.

| Do | Don't |
|----|-------|
| Write structured prompts; specify framing, lighting, ratio, and text space | Type one vague noun and accept the first result |
| Keep a fixed style suffix for a consistent series | Generate each image with a different style and call it a set |
| Add real headlines/logos as type, on top of the image | Let the model render text or brand marks |
| Inspect hands/faces/text and regrade to brand | Ship the raw generation with six-fingered hands |

---

## Agent checklist

- [ ] Choose authentic, single-subject images over stock clichés; confirm focal clarity before treatment.
- [ ] Apply one consistent treatment (duotone/grade/grain) across every image in the piece or set.
- [ ] For text on a photo, run the recipe: calm region → gradient scrim (40–60% dark for white text) → darken/blur → boost type → verify 4.5:1 at the worst pixel.
- [ ] Use one illustration style throughout, colored from the brand palette.
- [ ] Use a single icon family, uniform stroke weight on a shared grid, sized on the spacing scale; align optically.
- [ ] Keep backgrounds (gradient/pattern/texture/mesh) low-contrast, brand-derived, and one-per-canvas.
- [ ] Re-crop per aspect ratio to protect the focal point on a thirds power point; never crop through faces/joints; leave lead room.
- [ ] Match resolution to medium (300 ppi print, ~150 web); vectors for logos/icons, raster for photos; correct color space (CMYK print / sRGB screen).
- [ ] Never upscale low-res sources to fill a large canvas without verifying invented detail.
- [ ] Prompt AI images as subject + style + lighting + composition + color + technical, with built-in negative space and a fixed style suffix for series.
- [ ] Never let AI render headlines or logos; add real type/marks on top and inspect hands/faces/text.
- [ ] Regrade generated imagery to the brand palette and confirm usage rights.
