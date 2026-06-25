# Print & Event Collateral Design

> Purpose: Give you the production-correct judgment to design posters, flyers, brochures, cards, banners, signage and event collateral that survive the trip from screen to physical paper — correct sizes, bleed, CMYK, resolution, and a print-ready PDF.

**When to read this:** Any time the deliverable is *physical* — anything that will be printed, mounted, folded, worn, or stood up at an event. If it stays on a screen, this is the wrong file (see `../08-visual-composition/format-specs.md` for screen/social formats).

---

## 1. The print mindset (how print differs from screen)

Screen design is forgiving: pixels reflow, colors glow, mistakes are a redeploy away. Print is the opposite — **fixed, physical, and irreversible**. Internalize these shifts before laying out anything.

| Dimension | Screen | Print | Consequence for you |
|---|---|---|---|
| Size | Fluid, responsive, zoomable | Fixed in **mm/inches**, 1:1 physical | Design at the real trim size; nothing reflows |
| Color | RGB, backlit, ~16M vibrant colors | **CMYK** subtractive ink, narrower gamut | Neon/electric RGB shifts dull — design in CMYK |
| Resolution | 72–96 PPI (CSS px) | **300 DPI** raster minimum; prefer vector | A crisp web image pixelates when printed |
| Black | `#000` is true black | Flat 100K looks grey on large fills | Use **rich black** for big dark areas |
| Links | Hyperlinks, hover, scroll | None — paper is static | Bridge to digital with a **QR code / short URL** |
| Edge | Pixel-perfect to viewport | Paper is **physically cut**, drifts ±1mm | Add **bleed**; keep content off the trim |
| Cost of error | Free redeploy | Reprint the whole run | **Proof before printing** — soft proof + hard proof |

**Hard rules of print:**
- **CMYK gamut is smaller than RGB.** Bright saturated blues, greens, oranges and any neon will desaturate or shift. Convert to CMYK *while designing* and judge color there, not on a glowing monitor. See `../02-foundations/color.md` for color theory; this file covers the print-gamut specifics.
- **300 DPI at final size** is the raster floor. Vector (logos, type, shapes) is resolution-independent — prefer it. Large-format viewed from distance can drop to 100–150 DPI (see §7).
- **Bleed + trim + safe** is non-negotiable on anything cut to size (§2).
- **Irreversibility.** Once it's on the press, a typo costs a full reprint. Always generate a proof PDF and review at 100% before committing a run.

---

## 2. Bleed, trim, and safe margin (the three boxes)

Commercial printing prints on oversized sheets and **mechanically cuts** them. The blade drifts by up to ~1mm. To avoid white slivers at the edge, any color/image meant to reach the edge must extend **past** the cut line — that overhang is **bleed**. And anything you can't afford to lose (text, logos, faces) must sit inside a **safe margin** away from the cut.

```
  ┌───────────────────────────────────────────┐  ← BLEED edge  (trim + 3mm)
  │   background/image extends to HERE         │     full-bleed art ends here
  │   ┌───────────────────────────────────┐   │
  │   │ . . . . . . . . . . . . . . . . . │   │  ← TRIM  (final size, e.g. A4 210×297)
  │   │ .   ┌───────────────────────┐   . │   │     the paper is physically cut here
  │   │ .   │                       │   . │   │
  │   │ .   │   SAFE / LIVE AREA    │   . │   │  ← SAFE margin (3–5mm inside trim)
  │   │ .   │   all text + logos    │   . │   │     keep critical content inside
  │   │ .   │   live in here        │   . │   │
  │   │ .   └───────────────────────┘   . │   │
  │   │ . . . . . . . . . . . . . . . . . │   │
  │   └───────────────────────────────────┘   │
  │        ← 3mm bleed all around →            │
  └───────────────────────────────────────────┘
       ⌐ crop marks sit just outside trim ¬
```

| Box | What it is | Standard value | Rule |
|---|---|---|---|
| **Bleed** | Art overhang past trim | **3mm** (EU) / **0.125in** (US) all sides | Backgrounds/images that touch an edge must fill to here |
| **Trim** | The final cut size | The named size (A4 = 210×297mm) | This is your *document* size |
| **Safe / live** | Inner keep-out from trim | **3–5mm** inside trim | All text, logos, key subjects live inside |

So an **A4 flyer** designed with bleed is a **216×303mm** document (210+3+3 × 297+3+3), trimmed to 210×297, with text kept inside roughly a 200×287 live area. **Crop marks** (thin lines just outside the trim corners) tell the cutter where to slice; the PDF export adds them.

> **Do:** Push backgrounds and edge photos a full 3mm past trim. **Don't:** Place page numbers or a tagline 1mm from the trim — it'll be cut off or look mis-aligned.

For exact dimensions of every common size, see `../08-visual-composition/format-specs.md`. The reference table below is the print-collateral subset.

### Standard sizes (memorize these)

| Format | Size (mm) | Size (in) | Typical use |
|---|---|---|---|
| A6 | 105 × 148 | 4.1 × 5.8 | Postcard, small flyer |
| A5 | 148 × 210 | 5.8 × 8.3 | Flyer, leaflet, menu |
| A4 | 210 × 297 | 8.3 × 11.7 | Poster, one-pager, brochure page |
| A3 | 297 × 420 | 11.7 × 16.5 | Poster, small signage |
| A2 | 420 × 594 | 16.5 × 23.4 | Poster |
| A1 | 594 × 841 | 23.4 × 33.1 | Large poster |
| A0 | 841 × 1189 | 33.1 × 46.8 | Large-format poster |
| US Letter | 216 × 279 | 8.5 × 11 | US flyer/one-pager |
| US Legal | 216 × 356 | 8.5 × 14 | US documents |
| US Tabloid/Ledger | 279 × 432 | 11 × 17 | US poster |
| Business card (EU) | 85 × 55 | 3.35 × 2.17 | Standard outside US |
| Business card (US) | 89 × 51 | 3.5 × 2.0 | Standard in US |
| DL ("compliment") | 99 × 210 | 3.9 × 8.3 | Tri-fold A4 panel, rack card |
| Roll-up banner | 850 × 2000 | 33.5 × 78.7 | Pull-up display |

*A-series rule: each step up doubles the area; the long side of the smaller becomes the short side of the larger (A4→A3 = rotate and double).*

---

## 3. Color, black, and resolution for print

### CMYK and the gamut shift
Design and **soft-proof in CMYK**. The biggest failures are saturated RGB colors (electric blue `#0000FF`, neon green, hot magenta) that the press cannot reproduce — they print noticeably duller. If the brand color must match, specify a **Pantone (spot) color** instead of building it from process CMYK. See `../02-foundations/color.md` and `../08-visual-composition/brand-systems.md` for brand-color governance.

### Black: rich vs flat
| Black type | Build | Use for |
|---|---|---|
| **Flat / 100K** | C0 M0 Y0 **K100** | Body text, thin rules, small type (keeps edges sharp, no misregistration) |
| **Rich black** | **C60 M40 Y40 K100** | Large solid black fills, backgrounds, big headlines |
| **Registration black** | C100 M100 Y100 K100 | **Never** for art — printer's marks only; over-inks and smears |

> **Do:** Set body text in **100K only** so it stays crisp and can't misregister into a fuzzy edge. **Don't:** Fill a full A3 background with 100K — it prints as a weak, uneven grey; use rich black.

### Resolution
- **Raster art: 300 DPI at final print size.** A 1000×1000px image is only ~85mm at 300 DPI — check effective DPI after scaling.
- **Prefer vector** for logos, icons, type, and shapes (infinitely sharp). See `../08-visual-composition/imagery-and-icons.md`.
- **Line weight floor: ≥0.25pt (≈0.09mm).** Thinner "hairlines" can vanish or break up on press.
- **Large-format** can relax to 100–150 DPI because viewing distance is large (§7).

---

## 4. Posters (A4 / A3 / A2 / large-format)

A poster competes for attention from a distance, in a cluttered environment, in seconds. Design for **distance reading first**, detail second.

### The 5-second / 3-feet / 1-glance test
- **5 seconds:** A passer-by gives you ~5s. The *single* most important message must land in that time.
- **3 feet (and 10 feet):** The headline must be readable from across a room. Stand back (or zoom your canvas to fit a small thumbnail) — if you can't read the headline, the type is too small.
- **1 glance:** One dominant focal point. One message. **One CTA.** This aligns with the project non-negotiable: *one primary action per screen* — here, per poster.

### Hierarchy for distance
Strong, decisive hierarchy — far more extreme than screen UI. See `../02-foundations/typography.md` for scale theory; the distance-driven sizing is below.

```
┌───────────────────────────┐
│   ████ HEADLINE ████      │  ← huge, instant read (the hook)
│   Supporting subhead       │  ← second beat, fills in the promise
│                            │
│      [ FOCAL IMAGE ]       │  ← one dominant visual
│                            │
│   what · when · where      │  ← the facts (events)
│   ▌ CTA / QR ▐             │  ← one action
└───────────────────────────┘
```

### Viewing distance → minimum type size (cap height)
Rule of thumb: **~1 inch (≈25mm) of letter height per 10 feet (≈3m)** of viewing distance for comfortable reading; double it for "grab attention across the room."

| Viewing distance | Min readable cap height | Context |
|---|---|---|
| Arm's length (A4/A5 flyer in hand) | 10–14pt body / 24–48pt headline | Held documents |
| Across a room (A3/A2 wall poster, ~3m) | ~25mm (≈70pt) headline | Notice board |
| ~6m (large poster / signage) | ~50mm headline | Corridor, hall |
| ~10m+ (banner, exhibition) | ~100mm+ headline | Trade-show floor |

### Poster essentials
- **One focal image** — a single strong photo/illustration beats a collage. Crop hard.
- **One message + one CTA.** Resist adding "while we're at it" content.
- **Generous margins and whitespace** — crowding kills legibility at distance.
- **High contrast** text vs background (≥4.5:1; bigger headlines can use 3:1 but more is safer). Avoid text over busy photo areas; add a scrim/overlay if needed.

### Event poster — required content
Every event poster must answer, in priority order:
1. **WHAT** — event name / headline (the hook).
2. **WHEN** — date + time, made **prominent** (people scan for this).
3. **WHERE** — venue + address, prominent.
4. **CTA** — register / buy tickets / RSVP, with the QR / URL.
5. Supporting: who/host, price, logos/sponsors (smallest tier).

> **Do:** Make **date and venue** large and unmissable — they're the #1 thing attendees look for. **Don't:** Bury the date in body copy or omit the year.

### QR codes (bridging print → digital)
A poster can't be clicked, so the QR code is the bridge.
- **Minimum printed size ~2×2cm (20mm); 2.5–3cm is safer** for phone-scan reliability. Bigger for distance/large-format.
- **Quiet zone:** keep a clear margin of ~4 modules (≈the width of 4 QR dots) of empty space around it — don't crowd it with text or graphics.
- **Contrast:** dark code on light background. Inverted (light on dark) often fails to scan — test it.
- **Always include a human-readable short URL** as a fallback (`example.com/event`).
- **TEST the actual exported/printed code** with a phone before sending to press. A broken QR on 5,000 flyers is a wasted run.

---

## 5. Flyers, leaflets & brochures (folds)

### Single sheet (flat flyer)
A5 or A4, one or two sides. **Front** = hook (headline, image, the offer). **Back** = detail (body, contact, map, QR, terms). Keep a consistent inner margin; mind bleed on every edge that touches color.

### Folded formats — panels and panel order
Folding multiplies surfaces but introduces **panel logic**: which physical panel becomes which face after folding, and which panels are slightly **narrower** (the panel that tucks *inside* must be ~2–3mm smaller so it doesn't buckle).

| Fold | Panels | Folds | Notes |
|---|---|---|---|
| **Bi-fold** | 4 (2 per side) | 1 | Simple booklet feel; like a greeting card |
| **Tri-fold (letter/C-fold)** | 6 (3 per side) | 2 | Classic brochure; right panel folds in first |
| **Z-fold** | 6 (3 per side) | 2 | Zig-zag accordion; all panels ~equal width |
| **Gate-fold** | 6 | 2 | Two side panels fold in to meet at centre — dramatic reveal |
| **Roll/barrel-fold** | 8+ | 3+ | Panels roll inward; each inner panel progressively narrower |

#### Tri-fold (A4 landscape → 3 DL panels) panel map
Lay out as **one landscape sheet per side**. On a letter/C tri-fold the **right-most front panel folds inward first**, so it becomes the inside-left when opened.

```
OUTSIDE (printed side 1)              INSIDE (printed side 2)
┌─────────┬─────────┬─────────┐       ┌─────────┬─────────┬─────────┐
│  Panel  │  Panel  │  Panel  │       │  Panel  │  Panel  │  Panel  │
│   3     │   1     │   2     │       │   4     │   5     │   6     │
│ (back)  │ (front  │ (folds  │       │ (inside │ (inside │ (inside │
│         │  COVER) │  in,    │       │  left)  │ middle) │  right) │
│ contact │ hook +  │ first   │       │  ─ the spread the reader   │
│ + map   │ logo +  │ flap)   │       │    sees on full open ─     │
│ + QR    │ image   │ teaser  │       │                            │
└─────────┴─────────┴─────────┘       └─────────┴─────────┴─────────┘
   ↑ the tuck-in panel (2) is ~2–3mm narrower than the others
```

- **Cover (panel 1):** headline + brand + image. The reason to open it.
- **Tuck-in flap (panel 2):** seen second; teaser/intro. Make it ~2–3mm narrower.
- **Back (panel 3):** contact, address, map, QR, social.
- **Inside spread (4–5–6):** the full story; you can run a design or image across all three since they're one continuous surface when open — but **keep important content ~5mm clear of the fold lines** (folding crushes ink on the crease).

> **Do:** Treat the inside as one wide canvas but keep headlines off the creases. **Don't:** Center a face or logo *on* a fold line — it'll crack.

### Fold-safe & margin rules
- **Per-panel margin:** ~5mm inner margin on each panel, not just the outer sheet.
- **Fold tolerance:** keep critical content ≥5mm from any fold.
- **Bleed** still applies to the outer sheet edges as normal.
- For brochures with **many pages** (saddle-stitched booklets), page count must be a **multiple of 4**, and set up as **reader spreads → imposition handled at export/print** (let the RIP impose; you supply single pages).

---

## 6. Smaller items (cards, badges, table tents, certificates, menus)

### Business cards
- **Size:** 85×55mm (EU) or 89×51mm / 3.5×2in (US). Add **3mm bleed** → 91×61mm doc (EU).
- **Safe text margin:** keep text/logos **≥4–5mm from trim** — cards are cut in stacks and drift more.
- **Double-sided:** front = name + logo + role; back = contact, QR, tagline (or a bold brand graphic). Don't cram both sides full.
- **Min type:** ~7–8pt for fine print; ~9–10pt for contact details. Never below 6pt.
- **Finishes** (affect file setup): spot UV, foil, emboss, rounded corners — each needs a **separate spot-color layer/mask** named per the printer's spec. Rounded corners shrink the safe area.

### Badges & lanyards (events)
- **Name legibility is the whole job.** First name **huge** (readable at conversational distance, ~24–48pt+), last name smaller, org/role smaller still.
- **Role / colour coding:** use a color band or icon for Attendee / Speaker / Staff / Press / VIP so roles are identifiable at a glance.
- **Hole/clip safe area:** leave a **clear zone at the top** (~15–20mm) for the punch hole / clip / lanyard slot — no text or logo there, or it gets obscured/torn.
- **Double-sided** if it can flip in the holder — print the name on both sides.
- Standard insert sizes: ~A6 (105×148) or 4×3in; match the holder.

### Table tents
- Folded standing card (tent). Design is **two-up and mirrored** so both sides read upright. Top half = the message seen across the table; bottom half tucks/bases. Keep content in the upper readable zone.

### Certificates
- Usually **A4 landscape**. Formal hierarchy: title ("Certificate of …") → recipient name (largest personalized element) → reason → date + signatures + seal.
- Leave room for a **signature line** and **seal/logo**. Center-balanced, generous margins, classical/serif type reads as official. If printed on pre-printed bordered stock, design to that border's safe area.
- For **variable names** (many recipients), use a data-merge workflow — see `../13-production/production-and-tools.md`.

### Menus
- Sizes: A4, A5, or DL; often laminated or folded. **Scannable hierarchy:** section headers → item name → description → price. Align prices for easy scanning (leader dots or a right-aligned price column).
- Keep type ≥9–10pt (read in dim restaurant light). High contrast. Group logically (Starters / Mains / Desserts / Drinks).

---

## 7. Large-format & event environments

Viewing distance is large, so **DPI can drop (100–150)** but **type and contrast must scale up** dramatically.

### Roll-up / pull-up banners
- **Typical size 850×2000mm** (also 800×1800, 1000×2000). Vertical.
- **Eye-level safe zone:** the most important content (logo, headline, key message) lives in the **top ~1200mm** — roughly chest-to-eye level for a standing viewer.
- **Keep critical content above ~500–600mm from the floor** (table height): at events, the bottom is blocked by tables, crowds, the banner base mechanism, and the curl-in at the very bottom. Treat the bottom ~100mm as a no-content "graphic only" zone (it rolls into the cassette).
- **Top-down hierarchy:** Logo → Headline → 2–3 benefit bullets → CTA + QR, large.
- Resolution: 100–150 DPI at full size is fine; ensure logos are vector.

```
850 × 2000 mm pull-up
┌───────────────┐  top
│     LOGO      │
│               │
│   HEADLINE    │  ← eye level (~1.4–1.6m)
│   ● benefit   │
│   ● benefit   │
│   ● benefit   │
│  CTA + [QR]   │  ← still above table height
│···············│  ← ~600mm: table/crowd line — keep content above
│ (brand color) │
└───────────────┘  ~bottom 100mm rolls into base — no content
```

### Backdrops / step-and-repeat
- **Step-and-repeat:** logo (often two logos alternating) **tiled** across the whole surface for press/photo backdrops. Tile size must be large enough that at least 2–3 logos appear behind a person in frame.
- **Safe centre:** keep a clean band where people stand and are photographed; don't let a logo sit awkwardly behind a head. Plan the repeat so logos frame, not crown, subjects.
- Big backdrops are printed in panels/seams — keep critical art off seam lines.

### Signage & wayfinding
- **Contrast first:** dark text on light or vice-versa, well above 4.5:1; aim higher for distance and odd lighting.
- **Sans-serif, big, simple.** Avoid thin weights, tight tracking, decorative fonts. One clear word/arrow per sign.
- **Directional clarity:** arrows + short labels. Consistent placement and color-coding across a venue.
- **Mounting height** affects size: floor-standing vs overhead changes the minimum cap height (see §4 distance table).

### Exhibition stand basics
- **Three zones:** attract (big headline/brand, readable across the hall), engage (mid-level product/benefit content), convert (close-up demo + CTA + QR).
- Brand consistency across banner, backdrop, table runner, handouts — pull from one brand system (`../08-visual-composition/brand-systems.md`).
- Keep critical messaging **above head height** so it survives a crowded booth.

---

## 8. Production & handoff (print-ready PDF)

The deliverable to a printer is almost always a **press-ready PDF** built to the **PDF/X** standard (PDF/X-1a for CMYK-only/flattened, PDF/X-4 for live transparency + ICC). For *how* to actually generate these (HTML→PDF with `@page` bleed, InDesign, Express, Canva, design tools), see `../13-production/production-and-tools.md`. This section is *what correct output requires*.

### Export settings (the checklist)
- **Document size = trim size**, with **3mm bleed** added on all sides.
- **Crop marks ON**, offset so they sit outside the bleed. Add registration/color bars if the printer asks.
- **Color: CMYK** (or CMYK + named spot colors). Embed/assign the printer's **ICC profile** (e.g. `FOGRA39`/`PSO Coated` in EU, `GRACoL`/`US Web Coated SWOP` in US) if known.
- **Fonts embedded** — or **outlined** (converted to vector curves) to eliminate any font-substitution risk. Outlining is bulletproof but un-editable; embedding keeps text live. Embed at minimum.
- **Images: 300 DPI** effective, CMYK, no profile mismatches.
- **PDF/X-1a** (safest, fully flattened, no transparency surprises) or **PDF/X-4** (modern, keeps transparency/ICC live).
- **No hyperlinks/interactive** elements expected to function — paper.

### Overprint & knockout (the trap)
- **Knockout (default):** top color "punches a hole" in the layer beneath so inks don't mix.
- **Overprint:** top ink prints *on top of* the one below (inks combine). Used deliberately for trapping and for keeping small **black text overprinting** so it doesn't misregister into white halos.
- **The classic bug:** an object accidentally set to overprint with a white or light fill **disappears** on press (white overprinting nothing = invisible). Always run **Overprint Preview** before export and confirm whites knock out.

### Transparency flattening
Drop shadows, blends, and opacity are "live transparency." On older RIP/PDF-X-1a output they get **flattened**, which can cause thin white seams or color shifts where transparent and opaque areas meet. Either export PDF/X-4 (transparency-aware) or flatten intentionally at high resolution and check the result.

### Prepress preflight checklist (run before sending)
1. **Document size** correct + **3mm bleed** present on all four sides.
2. **Bleed actually filled** — backgrounds/images extend into the bleed, no white slivers.
3. **Safe margin respected** — no text/logo within ~3–5mm of trim.
4. **Color mode = CMYK** (or CMYK + intended spots); no stray RGB images.
5. **Images ≥300 DPI** effective at placed size; no upscaled low-res art.
6. **Fonts embedded or outlined**; nothing missing/substituted.
7. **Black is correct** — body text 100K, large fills rich black, no registration-black art.
8. **No hairlines <0.25pt**; thin rules thickened.
9. **Overprint preview** clean — no invisible whites, no unintended overprints.
10. **Crop marks + bleed** present in the exported PDF/X.
11. **Folds/panels** correct (tuck-in panel narrower; content clear of fold lines).
12. **Soft-proof reviewed** at 100%; ideally a **hard proof** for color-critical or large runs. Get printer sign-off — **proof before printing.**

---

## 9. Screen-to-print gotchas

The failures that look fine on your monitor and ruin the print run.

| Gotcha | Why it happens | Fix |
|---|---|---|
| **RGB neon won't print** | CMYK gamut can't hit electric/saturated RGB | Design in CMYK; soft-proof; use Pantone spot for must-match brand color |
| **Hairlines disappear** | Lines <0.25pt fall below press resolution | Thicken rules to ≥0.25pt (≈0.09mm) |
| **Low-DPI images pixelate** | Web art is ~72–96 PPI; print needs 300 | Source/scale to 300 DPI at final size; check *effective* DPI after resizing |
| **Text too close to trim** | No safe margin; blade drifts ±1mm | Keep all text ≥3–5mm inside trim |
| **Pure-black large fills look grey/blotchy** | 100K alone is thin ink on big areas | Use rich black (C60 M40 Y40 K100) for large dark fills |
| **Faint white halo around black text** | Black knocks out and misregisters | Set small black text to overprint; keep it 100K |
| **Edges show white slivers** | No bleed; art stops at trim | Extend backgrounds 3mm past trim |
| **Transparency/shadow shows seams or shifts** | Flattening live transparency | Export PDF/X-4 or flatten hi-res; check overprint preview |
| **Colors look different than screen** | Monitor is RGB & backlit | Trust CMYK soft proof + hard proof, not the screen |
| **Content cut off at folds** | Art placed on the crease | Keep critical content ≥5mm from fold lines |
| **QR won't scan** | Too small, no quiet zone, low contrast, or inverted | ≥2cm, quiet zone, dark-on-light, **test the printed code** |
| **Missing/substituted fonts** | Fonts not embedded | Embed or outline all fonts in the PDF |

---

## Agent checklist

- [ ] Confirm the **physical format and exact trim size** (mm/in) before laying anything out; design at 1:1 real size.
- [ ] Build the document with **3mm (0.125in) bleed** on all sides and crop marks; fill backgrounds into the bleed.
- [ ] Keep **all text, logos, and key subjects ≥3–5mm inside the trim** (safe area); more for stacked-cut items like business cards.
- [ ] Work in **CMYK and soft-proof color there**; flag any saturated RGB/neon and propose a Pantone spot for must-match brand colors.
- [ ] Set **body text to 100K**, **large dark fills to rich black (C60 M40 Y40 K100)**, and never use registration black for art.
- [ ] Ensure **raster images are 300 DPI at final size** (100–150 DPI OK for large-format), prefer vector, and thicken any rule below 0.25pt.
- [ ] For posters/signage, pass the **5-second / distance test** — one focal point, one message, one CTA, headline readable from across the room.
- [ ] For events, make **WHAT/WHEN/WHERE/CTA** unmistakable, with **date and venue prominent**, and a tested **QR code (≥2cm, quiet zone, dark-on-light)** plus a fallback URL.
- [ ] For folds, lay out by **panel** with the tuck-in panel ~2–3mm narrower and all critical content ≥5mm clear of fold lines.
- [ ] For banners/backdrops, keep key content at **eye level and above table height**, with vector logos and a no-content bottom zone.
- [ ] Export a **PDF/X with embedded/outlined fonts**, run **overprint preview**, and complete the **prepress preflight** (§8) before handoff.
- [ ] **Proof before printing** — review at 100%, get a hard proof for color-critical/large runs, and obtain printer sign-off; remember the run is irreversible.
