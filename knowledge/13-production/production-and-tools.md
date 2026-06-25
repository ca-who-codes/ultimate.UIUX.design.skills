# Production & Tools — How to Actually Render the Asset

> Purpose: Maps every design artifact to a concrete production path — how to turn a design into a real `.png`, `.pdf`, `.pptx`, `.mp4`, or Express/Canva doc — using portable methods first and environment tools when available.

**When to read this:** After you've designed the asset (composition, tokens, copy decided) and need to produce the actual file. Also read it *before* starting if the output format constrains the design (print bleed, email HTML, slide aspect ratio).

> **Core principle — HTML/CSS is the universal design source-of-truth.** You can express almost any fixed-canvas design (slide, carousel slide, poster, email, one-pager) as a self-contained HTML/CSS document with exact pixel/mm dimensions, then render it deterministically to PNG/PDF or import it into a design tool. This gives you full typographic and layout control, version-controllable source, and repeatable output. Author in HTML → render to the medium.

---

## The decision table — artifact → recommended path

| Artifact | Primary path | Fast alternative | Fallback |
|---|---|---|---|
| **Deck / slides** | HTML slides → PDF, **or** generate `.pptx` programmatically (`pptx` skill / python-pptx) | Canva / Gamma / Google Slides MCP | Markdown → reveal.js / Marp |
| **Social carousel** | HTML slide template → render each slide to **PNG** at 1080×1350 | Adobe Express / Canva | Figma export |
| **Single social post / ad** | HTML → PNG at target size | Express / Canva | AI image gen + text overlay |
| **Poster / flyer (print)** | HTML + `@page` bleed → **PDF/X**, 300 DPI assets | Adobe Express / InDesign / Canva print | Vector (SVG) → PDF |
| **Business card / badge** | HTML→PDF with bleed, or vector | Canva / Express print | InDesign |
| **Email / newsletter** | **MJML** → HTML, or hand-coded table HTML, CSS inlined | Beefree / Stripo | ESP's drag-drop builder |
| **Video / motion** | **Remotion** (React → MP4) | CapCut / After Effects | Lottie/Rive for vector loops |
| **One-pager / report / doc** | HTML→PDF, or `docx`/`pdf` skills | Google Docs | Canva Docs |
| **Spreadsheet / data sheet** | `xlsx` skill | Google Sheets | CSV |
| **Image edit / background / cutout** | Image MCP tools (remove bg, adjust, vectorize) | Photoshop | AI editor |

---

## Method A — HTML → PNG (social, creatives, carousels)

The workhorse for pixel-perfect social/marketing visuals.

1. Build one self-contained HTML file. Set the canvas to exact dimensions and embed fonts/images (base64 or absolute URLs). Example skeleton:
   ```html
   <div class="canvas" style="width:1080px;height:1350px;overflow:hidden;
        font-family:Inter,sans-serif">…</div>
   ```
2. Render with a headless browser at a fixed viewport and device-scale-factor for crispness:
   - Playwright/Puppeteer: set viewport `1080×1350`, `deviceScaleFactor: 2`, screenshot the `.canvas` element.
   - For a **carousel**, put each slide in its own sized element and screenshot each → `slide-01.png … slide-07.png`.
3. Verify fonts loaded before capture (wait for `document.fonts.ready`), and that text didn't overflow.

This pattern is exactly what code-rendered carousel/infographic pipelines use (e.g. an HTML→PNG script). Prefer it when you need brand-exact type and zero "AI-template" look.

---

## Method B — HTML → PDF (print, posters, one-pagers, decks)

For print, the design must carry **bleed** and print-safe color.

```css
@page { size: 216mm 303mm; margin: 0; }   /* A4 + 3mm bleed all sides */
.trim   { width:210mm; height:297mm; }      /* actual trim */
.safe   { padding: 5mm; }                    /* keep content inside */
```
- Render with a headless browser's `printToPDF` (Chromium) or Prince/Weasyprint for stronger print CSS.
- Embed/outline fonts; place images at ≥300 DPI; convert to **CMYK** and export **PDF/X** in a prepress step (most cheap printers also accept high-res RGB PDF — confirm).
- Add crop marks if the printer needs them. See [../11-print-and-events/print-posters.md](../11-print-and-events/print-posters.md) for the full preflight.

For **screen-only** PDFs (digital one-pager, report), skip bleed/CMYK; keep RGB, add hyperlinks, A4/Letter.

---

## Method C — Programmatic decks (`.pptx`)

When the user needs an editable PowerPoint (not just a PDF):
- Use the **`pptx` skill** / `python-pptx` to build slides from the deck design in [../09-presentations/decks.md](../09-presentations/decks.md): set 16:9, a master with brand colors/fonts, then add slides by archetype.
- Keep text as real text (editable, accessible) — avoid flattening slides to images unless intentional.
- Alternative: a **Google Slides / presentation MCP** (e.g. `create_slides` + `apply_theme`) when the user lives in Google Workspace.
- For speed-over-control, Gamma or Canva generate decks from an outline — good for first drafts, then refine.

---

## Method D — Design-tool handoff (Adobe Express / Canva)

When the user wants to keep editing in a visual tool, or needs templated brand assets at scale:
- **Adobe Express** — author the design as HTML and import it: validate with the HTML-export readiness step, then export HTML → Express to get an editable Express document. Express also exposes image tools (remove background, adjust, vectorize, generative expand) useful for prepping creative assets.
- **Canva** — generate/edit designs, apply brand kits, resize across formats, and export. Good when the team already uses Canva templates.
- Use these when collaboration/editability matters more than pixel-exactness. Use Method A/B when you need exact control and versionable source.

---

## Method E — Video (Remotion)

For data-driven or templated video (social cuts, animated explainers, personalized media):
- **Remotion** renders React components to real MP4. Define compositions at the target resolution (1080×1920 vertical, 1920×1080 landscape), drive content from props/data, render server-side.
- Reuse your brand tokens and even your HTML/CSS design language inside Remotion compositions so video matches static assets.
- Always burn in captions and design a strong first frame (it's the thumbnail). See video specs in [../08-visual-composition/format-specs.md](../08-visual-composition/format-specs.md).

---

## Method F — Imagery & AI generation

- **AI image generation** for hero art/backgrounds/illustration: structure prompts as *subject + style + lighting + composition + medium + aspect ratio*; generate a consistent set with shared style language. See [../08-visual-composition/imagery-and-icons.md](../08-visual-composition/imagery-and-icons.md). (Dedicated video/image prompt skills, e.g. a Higgsfield prompt skill, can craft platform-specific prompts.)
- **Image editing tools** (Adobe image MCP or equivalent): remove/replace background, color/exposure adjust, vectorize logos, generative-expand to a new aspect ratio, add grain. Use these to prep raw assets before composing.
- Always re-check licensing for any stock/AI imagery before client delivery.

---

## Using available environment skills & MCPs (if present)

This repo is portable, but in a configured environment you may have ready-made pipelines. Reach for them when they fit — they encode brand + production in one step:

| Capability | Look for |
|---|---|
| On-brand carousel slides (HTML→PNG into a publish pipeline) | a "carousel" / "static-visual" skill |
| Dense one-pager **infographic** (HTML→PNG) | a "research-infographic" skill |
| Hand-drawn **whiteboard explainer** | a "whiteboard-explainer" skill |
| `.pptx` / `.docx` / `.xlsx` / `.pdf` files | the document skills of those names |
| Editable design docs | Adobe Express / Canva MCP tools |
| Programmatic video | Remotion |

If none are available, fall back to the portable Methods A–F above — they need only a headless browser and standard libraries.

> **Division of labor:** this repo owns the *visual design craft*. For the *copy, strategy, and distribution* of marketing assets (ad copy, subject lines, SEO, CRO, deliverability), pair it with a marketing-skills library. Design here; write/distribute there.

---

## Pre-delivery production checklist

- [ ] Rendered at the **exact** target dimensions (and 2× for crisp social/web)?
- [ ] Correct color space exported (sRGB screen / CMYK print)?
- [ ] Print: bleed present, 300 DPI, fonts embedded/outlined, PDF/X, proofed before printing?
- [ ] Fonts actually rendered (no fallback/`Times New Roman` surprise) and no text overflow/clipping?
- [ ] Images sharp at final size (no upscaled blur), reasonable file weight?
- [ ] Email: CSS inlined, < ~100 KB, tested in major clients, dark-mode checked?
- [ ] Video: captions burned in, strong first frame, right aspect ratio per platform?
- [ ] Output matches the brand system ([../08-visual-composition/brand-systems.md](../08-visual-composition/brand-systems.md)) and passed [../05-quality/review-checklist.md](../05-quality/review-checklist.md)?
- [ ] Editable source (HTML/PPTX/Express) handed off if the user will iterate?

## Agent checklist

- [ ] Did I pick the production method from the decision table that matches the artifact?
- [ ] Did the output format's constraints inform the design (bleed, email HTML, aspect ratio) *before* I built it?
- [ ] Did I default to HTML-as-source for pixel-exact control where it fits?
- [ ] Did I prefer real editable text over flattened images (decks, email, docs)?
- [ ] Did I use an available branded pipeline skill when one fit, else fall back to portable methods?
- [ ] Did I verify fonts, dimensions, color space, and resolution on the actual rendered file?
- [ ] Did I hand off editable source if the user will keep iterating?
- [ ] Did I route copy/strategy to the marketing layer and keep this layer focused on design?
