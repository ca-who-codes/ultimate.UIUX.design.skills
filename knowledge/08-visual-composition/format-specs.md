# Format & Dimension Specs — The Master Cheat-Sheet

> Purpose: One lookup table for every canvas size, aspect ratio, safe zone, resolution, and color space an agent needs before designing a deck, social creative, poster, print piece, email, or video.

**When to read this:** The moment you start *any* fixed-canvas asset — before composition. Set the correct dimensions, bleed, and color space first; everything downstream depends on it. Pair with [composition.md](composition.md) for how to fill the canvas.

> ⚠️ Platform pixel specs drift over time and ad specs change most often. These are correct, sensible defaults as of 2025–2026; for paid placements, confirm against the platform's current ad spec. When unsure, **design at the largest supported size and scale down**, and keep critical content inside safe zones.

---

## Quick rules that apply everywhere

| Decision | Default |
|---|---|
| Screen/social/web/email color | **sRGB** (RGB) |
| Print color | **CMYK** (expect vibrant RGB to desaturate) |
| Screen/social resolution | **72–96 PPI** is meaningless on export — what matters is **pixel dimensions**. Export at the exact px size, or **2×** for retina crispness. |
| Print resolution | **300 DPI** at final physical size (line art / vector: scalable) |
| Print bleed | **3 mm** (0.125 in) on every edge that touches the trim |
| Print safe margin | Keep text/logos **≥ 3–5 mm** inside the trim |
| File format (raster social) | PNG (flat color/text), JPG (photos), WebP (web) |
| File format (print) | **PDF/X-1a or PDF/X-4** with bleed + crop marks |
| File format (video) | MP4 (H.264), vertical-first for social |

---

## 1. Social media (pixels, sRGB)

Design vertical/portrait by default — it claims the most screen real estate on mobile. Square is the safe universal fallback.

### Instagram
| Asset | Pixels | Aspect | Notes |
|---|---|---|---|
| Feed — portrait (best reach) | **1080 × 1350** | 4:5 | Default for posts & carousels |
| Feed — square | 1080 × 1080 | 1:1 | Universal fallback |
| Feed — landscape | 1080 × 566 | 1.91:1 | Rarely optimal |
| Story / Reel | **1080 × 1920** | 9:16 | Keep text in the middle ~1080×1420; top ~250px & bottom ~250–340px hold UI |
| Profile photo | 320 × 320 (circle crop) | 1:1 | Legible at 40px |

### LinkedIn
| Asset | Pixels | Aspect |
|---|---|---|
| Feed image (portrait) | 1080 × 1350 | 4:5 |
| Feed image (square) | 1200 × 1200 | 1:1 |
| Link share image | 1200 × 627 | 1.91:1 |
| Document/carousel page | 1080 × 1080 or 1080 × 1350 | 1:1 / 4:5 |
| Personal banner | 1584 × 396 | 4:1 (keep content off lower-left where avatar sits) |
| Company logo | 300 × 300 | 1:1 |

### X / Twitter
| Asset | Pixels | Aspect |
|---|---|---|
| In-stream image | 1600 × 900 | 16:9 |
| Header | 1500 × 500 | 3:1 |
| Profile | 400 × 400 | 1:1 |

### TikTok / Reels / Shorts (vertical video & photo)
| Asset | Pixels | Aspect | Notes |
|---|---|---|---|
| Video / photo | **1080 × 1920** | 9:16 | Right ~120px + bottom ~250–480px = UI/caption zone — keep clear |

### YouTube
| Asset | Pixels | Aspect | Notes |
|---|---|---|---|
| Thumbnail | **1280 × 720** | 16:9 | < 2 MB; must read at ~120px wide |
| Channel banner | 2560 × 1440 | 16:9 | Safe area 1546 × 423 (centered) shows on all devices |
| Video (standard) | 1920 × 1080 | 16:9 | 4K = 3840 × 2160 |

### Facebook / Pinterest / Threads
| Asset | Pixels | Aspect |
|---|---|---|
| FB feed | 1200 × 1500 (4:5) or 1080 × 1080 | 4:5 / 1:1 |
| FB cover | 1640 × 856 | ~1.91:1 |
| Pinterest pin (standard) | 1000 × 1500 | 2:3 |
| Pinterest pin (long) | 1000 × 2100 | 1:2.1 |
| Threads | 1080 × 1350 | 4:5 |

> Carousel design specifics → [../10-social-creatives/carousels.md](../10-social-creatives/carousels.md). Single posts/ads/thumbnails → [../10-social-creatives/social-posts.md](../10-social-creatives/social-posts.md).

---

## 2. Presentations / slides (pixels, sRGB)

| Format | Pixels | Aspect | Use |
|---|---|---|---|
| **Widescreen (default)** | **1920 × 1080** (or 1280 × 720) | 16:9 | Standard for all modern decks |
| Standard (legacy) | 1024 × 768 | 4:3 | Only old projectors |
| Vertical / social deck | 1080 × 1350 or 1080 × 1920 | 4:5 / 9:16 | Mobile-viewed / LinkedIn docs |
| On-screen min body type | ~24 pt (never < 18 pt) | — | Readable from the back row |

Title-safe: keep content ~5% inside each edge. Deck design → [../09-presentations/decks.md](../09-presentations/decks.md).

---

## 3. Print (millimetres + inches, CMYK, 300 DPI, +3 mm bleed)

### ISO A-series (trim size — add 3 mm bleed each side)
| Size | mm | inches | With bleed (mm) | Pixels @300 DPI |
|---|---|---|---|---|
| A6 | 105 × 148 | 4.1 × 5.8 | 111 × 154 | 1240 × 1748 |
| A5 | 148 × 210 | 5.8 × 8.3 | 154 × 216 | 1748 × 2480 |
| **A4** | **210 × 297** | 8.3 × 11.7 | 216 × 303 | **2480 × 3508** |
| A3 | 297 × 420 | 11.7 × 16.5 | 303 × 426 | 3508 × 4961 |
| A2 | 420 × 594 | 16.5 × 23.4 | 426 × 600 | 4961 × 7016 |
| A1 | 594 × 841 | 23.4 × 33.1 | — | 7016 × 9933 |
| A0 | 841 × 1189 | 33.1 × 46.8 | — | 9933 × 14043 |

### US / common
| Item | Size | Pixels @300 DPI |
|---|---|---|
| US Letter | 8.5 × 11 in (216 × 279 mm) | 2550 × 3300 |
| US Legal | 8.5 × 14 in | 2550 × 4200 |
| Tabloid / Ledger | 11 × 17 in | 3300 × 5100 |
| **Business card (EU)** | 85 × 55 mm | 1004 × 650 (+ bleed → 1063 × 709) |
| Business card (US) | 3.5 × 2 in | 1050 × 600 |
| Postcard | 148 × 105 mm (A6) | 1748 × 1240 |
| DL flyer / rack | 99 × 210 mm | 1169 × 2480 |

### Folded pieces
| Fold | Flat size (from) | Panels |
|---|---|---|
| Bi-fold (A4→A5) | 297 × 210 mm | 4 panels (2 per side) |
| **Tri-fold (A4)** | 297 × 210 mm | 6 panels; right panel folds in **first** → make it ~2 mm narrower |
| Z-fold | 297 × 210 mm | 6 panels, accordion |
| Gate-fold | wide | 2 outer panels meet center |

### Large-format / events
| Item | Typical size | Notes |
|---|---|---|
| Roll-up / pull-up banner | 850 × 2000 mm (also 800×1800, 1000×2000) | Keep key content above ~900 mm (table/heads height); design ~75–150 DPI at full size |
| Step-&-repeat backdrop | 2400 × 2400 mm (8×8 ft) | Logo tiled; leave clear center for photos |
| Poster (event) | A2 / A1 / A0 | Headline must read across a room — see [../11-print-and-events/print-posters.md](../11-print-and-events/print-posters.md) |
| Tri-/A-frame, signage | varies | High contrast, big sans-serif |
| Lanyard badge | 85 × 110 mm or A6 | Name huge; safe area around punch hole |

Full print guidance, bleed diagram, prepress preflight → [../11-print-and-events/print-posters.md](../11-print-and-events/print-posters.md).

---

## 4. Email (pixels, sRGB)

| Spec | Value |
|---|---|
| Body width | **600 px** (max ~640 px); content scales to mobile |
| Layout | Single column (default), table-based |
| Min body type | 14–16 px (headings 22–30 px) |
| Tap targets / buttons | ≥ 44 × 44 px |
| Hero image | 600 px wide (export @2× = 1200 px for retina) |
| Total weight | Keep < ~100 KB HTML (Gmail clips at ~102 KB) |
| Preheader text | ~40–100 chars |

Full constraints (Outlook, dark mode, bulletproof buttons) → [../12-email-design/email.md](../12-email-design/email.md).

---

## 5. Video & motion (pixels)

| Use | Resolution | Aspect | FPS |
|---|---|---|---|
| Vertical social (Reels/TikTok/Shorts) | 1080 × 1920 | 9:16 | 30 |
| Square social | 1080 × 1080 | 1:1 | 30 |
| Landscape / YouTube | 1920 × 1080 (4K 3840×2160) | 16:9 | 30/60 |
| Slide-embedded loop | match slide (1920×1080) | 16:9 | 30 |

Always burn in captions; design a strong first frame (it's the thumbnail). Programmatic video → Remotion (see [../13-production/production-and-tools.md](../13-production/production-and-tools.md)).

---

## 6. Documents / PDF (digital)

| Item | Size | Notes |
|---|---|---|
| Digital one-pager / report | A4 or US Letter | RGB if screen-only; CMYK + bleed if it may be printed |
| Whitepaper / ebook | A4 / Letter, or 16:9 for screen-reading | Hyperlinked TOC, generous margins |
| Invoice / proposal | A4 / Letter | — |

---

## Decision shortcut: "What size do I make this?"

| The user asks for… | Make it… |
|---|---|
| "an Instagram post" | 1080 × 1350 (portrait) |
| "a carousel" | 1080 × 1350 slides, consistent template |
| "a story / reel cover" | 1080 × 1920, content centered |
| "a presentation / deck / PPT" | 1920 × 1080 (16:9) |
| "a poster / flyer" (print) | A4/A3 trim + 3 mm bleed, CMYK, 300 DPI |
| "a poster" (screen/social) | 1080 × 1350 or 1920 × 1080, sRGB |
| "a banner for an event" | 850 × 2000 mm roll-up |
| "an email / newsletter" | 600 px wide, single column |
| "a business card" | 85 × 55 mm + bleed, CMYK |
| "a YouTube thumbnail" | 1280 × 720 |
| "a one-pager" | A4 / Letter (RGB screen, or CMYK if printing) |

---

## Agent checklist

- [ ] Did I set the exact pixel/mm dimensions **before** composing?
- [ ] Correct color space — sRGB for screen/social/email, CMYK for print?
- [ ] Print pieces: 3 mm bleed added, 300 DPI, key content ≥ 3–5 mm inside trim?
- [ ] Social: designed portrait/vertical where it boosts reach, critical content inside safe zones?
- [ ] Exported social/web at 1× (or 2× for crispness), not at print DPI?
- [ ] Slides at 16:9 (1920×1080) unless a vertical/social deck is requested?
- [ ] Email ≤ 600 px wide, single column, under the Gmail clip weight?
- [ ] Did I confirm current ad specs for any paid placement rather than assuming?
- [ ] Chose the right export format (PDF/X for print, PNG/JPG for social, MP4 for video)?
