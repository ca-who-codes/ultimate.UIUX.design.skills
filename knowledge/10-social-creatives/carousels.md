# Swipe Carousels

> Purpose: The definitive guide to designing multi-slide swipe carousels (Instagram, LinkedIn, TikTok photo) as a visual narrative that earns the first swipe, holds attention slide to slide, and converts to a save, follow, or click.

**When to read this:** Designing any multi-card swipeable post — Instagram carousel, LinkedIn document/carousel, TikTok photo mode — where the user pages through a sequence of full-screen frames. For single standalone graphics, ads, and thumbnails see [social-posts.md](social-posts.md).

---

## What a carousel actually is

A carousel is a **paced, linear story told in frames** — closer to a deck or a comic strip than to a poster. The user's thumb is the page-turn, and every frame must do two jobs: deliver its one idea, and pull the thumb to the next frame. A carousel that is just one design repeated 7 times is a flipbook of nothing; a carousel that is 7 unrelated posters has no momentum. The craft is **continuity with progression**.

Why carousels over single posts: they get the highest dwell time and save rate of any organic format because each swipe is a micro-commitment, and the algorithm re-serves the post to people who didn't swipe (a second impression on the same feed). Saves and shares are the goal metric — design for the screenshot, not the scroll.

The three loads that matter:
- **Cover load** — earn the first swipe. ~70% of the design effort lives here.
- **Body load** — one idea per slide, zero friction, visible momentum.
- **Close load** — convert the attention you earned (save / follow / link).

---

## Format & dimensions

Default to **vertical 4:5 (1080 × 1350)** for reach — it occupies the most feed real estate and is the native portrait crop. Square (1080 × 1080) is the safe fallback when slides mix with non-carousel grid content. TikTok photo mode and Reels-context carousels use **9:16 (1080 × 1920)**.

| Platform / context | Use | Aspect | Pixels |
|---|---|---|---|
| Instagram carousel (default) | Max feed height | 4:5 | 1080 × 1350 |
| Instagram carousel (grid-safe) | Uniform grid look | 1:1 | 1080 × 1080 |
| LinkedIn carousel ("document") | Default | 4:5 | 1080 × 1350 |
| LinkedIn carousel (landscape decks) | Slide-deck feel | 1:1 or 4:5 | 1080 × 1080 / 1080 × 1350 |
| TikTok / Reels photo carousel | Full-screen vertical | 9:16 | 1080 × 1920 |

**Pick ONE aspect ratio and use it for every slide.** Instagram crops the whole carousel to the first slide's ratio — a mixed set gets ugly auto-crops. Full master list and per-platform export settings: [../08-visual-composition/format-specs.md](../08-visual-composition/format-specs.md).

---

## Safe zones — keep content out of the UI

Platform chrome (caption, profile name, action buttons, swipe dots, "Sponsored" labels) overlays the creative. Design within a safe inset or critical content gets covered or auto-cropped.

| Edge | Reserve (4:5 / 1:1 feed) | Reserve (9:16 reels/TikTok) | What lives there |
|---|---|---|---|
| Top | ~120 px | ~250 px | Profile name, follow button, time |
| Bottom | ~120 px | ~250–420 px | Caption, action rail, swipe dots, CTA UI |
| Left | ~60 px | ~60 px | Safe gutter |
| Right | ~60 px | ~120 px | Like/comment/share/save rail (vertical) |

Rule of thumb: keep all **must-read text and faces inside the centered safe rectangle** — roughly the middle 80% horizontally and middle 75–80% vertically. Decorative bleed (color, texture, an edge of an image) can run to the edge; meaning cannot. Full numbers: [../08-visual-composition/format-specs.md](../08-visual-composition/format-specs.md).

---

## The cover / hook slide — earn the swipe

If slide 1 fails, slides 2–10 never get seen. The cover competes in a scrolling feed against everything; it must stop the thumb and promise payoff. Give it disproportionate design effort.

A strong cover has four parts:

1. **A big, bold hook** — the single headline, set huge. On a 1080×1350 cover the headline cap-height should read at glance size: **~90–160 px** type, 1–6 words per line, max 3 lines. This is the largest type in the whole carousel.
2. **A clear subject** — one focal point (a face, a number, a product, a single graphic). The eye must land in <0.5s. No competing elements.
3. **Visual intrigue / an open loop** — a curiosity gap the body slides resolve: a number ("7 mistakes…"), a contrast (before/after halves), a partially-revealed element, a provocative claim. The cover sets a question; the swipe is the answer.
4. **A swipe cue** — an explicit affordance (arrow, "swipe", "1/7") so the format is unmistakable. Never assume the user knows it's a carousel.

> Do: Huge 3-word hook, one face looking at the viewer, a bold number badge, a small "→ swipe" bottom-right. Subject fills the frame.
> Don't: A busy collage, a small centered headline lost in whitespace, three ideas fighting, no swipe cue. The thumb keeps moving.

Cover legibility: maximum contrast (see [../02-foundations/color.md](../02-foundations/color.md)). If text sits over a photo, add a solid scrim, gradient, or text plate — never light text on a busy mid-tone photo. The cover headline must be readable as a feed thumbnail at ~40% size.

---

## One idea per slide

Each body slide carries **exactly one point** — one tip, one stat, one step, one myth. If a slide needs the word "and" twice or has two headers, split it. The user is paging at ~1.5–3s per slide; a slide that requires study breaks the rhythm and they bail.

Per-slide anatomy (body slide):
- **Index / step marker** — "02", "Step 2", or a progress dot, top-aligned.
- **One headline** — the idea, 3–8 words, large.
- **One supporting line or two** — the "why"/detail, smaller, short measure.
- **One optional visual** — icon, diagram, screenshot, or photo reinforcing the point.
- **Shared chrome** — header bar, footer handle, progress indicator (see template system).

Word budget per body slide: aim for **a headline + ~15–35 words of support**. If you're writing paragraphs, you're writing a blog post, not a carousel. (Copy *strategy* lives in the separate Marketing skill — here, design for the word count you're given and push back when it's too dense.)

---

## Text density & sizing for mobile

Carousels are read one-handed on a phone held ~30 cm away, the slide rendered ~6–9 cm tall. Type that looks fine in your editor is illegible in feed. Size generously.

| Role | Effective min on 1080-wide | Typical sweet spot | Notes |
|---|---|---|---|
| Cover hook | 90 px | 110–160 px | Largest element in the set |
| Body headline | 56 px | 64–88 px | One line ideally, two max |
| Body support | 36 px | 40–52 px | Short lines |
| Caption / label / index | 28 px | 30–40 px | Never below ~30 px effective |
| Footer handle / fine print | 24 px | 26–32 px | Lowest priority |

Rules:
- **Min effective text ≈ 30–40 px** on a 1080-wide slide. Below that it vanishes in feed.
- **Short measure:** 20–35 characters per line. Long lines force re-reading on small screens. Break early.
- **Left-align body text** for scannability; center only short hero lines. Avoid full justification (ragged rivers on narrow measure).
- **High contrast always** — 4.5:1 minimum for body text, more for large display. Test by squinting / viewing at thumbnail scale.
- **Line-height 1.2–1.35** for display, ~1.4 for support text. Tighter than body web because lines are short.
- **2–3 type sizes max per slide.** More than that reads as noise. See [../02-foundations/typography.md](../02-foundations/typography.md).

---

## A consistent template system

The body slides share a **template** so the set reads as one coherent piece and the user's eye doesn't relearn the layout every swipe. Build a master grid once; vary only the content zone.

Shared, locked across all slides:
- **Margins / safe grid** — same insets every slide (e.g., 80 px side margins on 1080).
- **Header zone** — brand bar, logo, or topic label in the same spot.
- **Footer zone** — @handle, optional CTA hint, consistent.
- **Progress indicator** — "03 / 07", a dot row, or a thin top progress bar that fills across the set. This both reassures ("almost done") and signals there's more.
- **Color system & type roles** — one accent, fixed display/body/label sizes (see [../08-visual-composition/brand-systems.md](../08-visual-composition/brand-systems.md)).

Varies, by design:
- The content zone (headline + support + visual layout).
- Slide background between two or three approved variants (e.g., dark / light / accent) to create rhythm.

> Do: Same 80 px margins, same footer handle position, a top progress bar that fills L→R, headline always top-left of the content zone. The set feels engineered.
> Don't: Margins drift each slide, the logo jumps around, type sizes wander. The set feels like a random pile of screenshots.

---

## Swipe affordance & continuity cues

Carousels live or die on the user knowing — and wanting — to swipe.

Explicit cues:
- **Arrow / "swipe →"** on the cover (and optionally each slide's edge).
- **Index "1/7", "2/7"…** — sets an expectation of length and creates completion pull.
- **Progress bar / dots** that advance.

Implicit (stronger) cues — **continuity across the slide edge:**
- **The peek:** let an element bleed off the right edge so it's visually "cut" — the brain wants to see the rest, so it swipes. The next slide completes it.
- **A continuous line, shape, or path** that runs across slide boundaries (a line exits slide 2 right, enters slide 3 left at the same Y).
- **A running visual** (a character walking, a bar chart growing, a number counting up) that only resolves by swiping.
- **An open loop in the copy hierarchy** ("Mistake #1 …" implies #2 exists).

> Do: A bold arrow that physically points off the right edge into the seam; "01/07" badge; the headline's underline continues onto the next slide.
> Don't: A static, self-contained slide with no exit cue. Nothing tells the thumb to move.

---

## Number of slides

Enough to deliver value, few enough to finish. Completion rate decays with length.

| Count | Use when | Note |
|---|---|---|
| 3–4 | Quick tip, single contrast, announcement | High completion, low depth |
| **6–8** | **Default sweet spot** — listicle, how-to, framework | Best dwell-rate balance |
| 9–10 | Deep how-to, detailed teardown | Front-load value; expect drop-off |
| >10 | Rare — only for genuinely sequential reference content | Most users won't finish |

Instagram allows up to 20 (carousel) / 35 in some surfaces; **just because you can doesn't mean you should.** Cut to the strongest slides. Each weak filler slide is an exit point.

---

## Visual rhythm & variety (without breaking consistency)

Sameness across 8 slides is boring; chaos is illegible. The answer is **controlled variation** — vary content layout within a fixed system.

- **Alternate backgrounds** between 2–3 approved variants (dark/light/accent) on a rhythm, e.g., cover bold → body slides alternate → CTA bold. The variation creates a heartbeat as you swipe.
- **Vary the content composition** — slide 2 text-left/visual-right, slide 3 full-bleed image with caption, slide 4 big number, slide 5 two-column compare. The *frame* (margins, header, footer, progress) stays identical; the *content block* changes.
- **Vary scale** — let one or two slides be a single huge word or number for punch; surrounded by denser slides it lands harder.
- **Keep one constant accent** so even varied slides feel sibling. One accent color, one type family, one icon style throughout (see [../08-visual-composition/imagery-and-icons.md](../08-visual-composition/imagery-and-icons.md)).

---

## The final CTA slide — convert the attention

The last slide is where earned attention becomes a follow, save, or click. Don't waste it on "Thanks for reading."

Strong close-slide elements (pick a primary ask, don't stack five):
- **One clear action:** "Save this for later" · "Follow @handle for more" · "Link in bio" · "Comment X to get it."
- **Recap / takeaway** — a one-line distillation so the slide is itself save-worthy.
- **Brand sign-off** — logo + @handle, consistent with the set.
- **A save/share nudge** — point at where the buttons are, or literally draw an arrow toward the save icon position.

Because Instagram/LinkedIn often **loop the carousel back to slide 1**, a close slide that visually rhymes with the cover creates a satisfying loop and a second look at your hook. Carousels can't carry tappable links on most surfaces (link in bio / profile instead), so the CTA must direct the user to the link's location explicitly.

---

## Save-ability & screenshot-ability

The highest-value action is the **save** (and the screenshot/share). Design specific slides to be independently valuable out of context:
- Make at least one slide a **standalone reference** — a checklist, a cheat-sheet, a summary table, a "save this" frame — that's useful even cropped and screenshotted into someone's notes.
- Keep your **@handle visible on every slide's footer** so a screenshot still credits you.
- Avoid putting essential info only in the caption — if it's worth saving, it's on the slide.

---

## Accessibility

- **Contrast:** body text ≥ 4.5:1, large display ≥ 3:1, against the actual background pixels (not the average). Add scrims under text on photos. See [../02-foundations/color.md](../02-foundations/color.md).
- **Don't rely on color alone** for myth/fact, before/after, do/don't — pair with icons, labels, or position.
- **Alt text:** write descriptive alt text per slide in the platform composer; transcribe key on-slide text so screen-reader users get the content.
- **Legible type:** no ultra-thin weights at small sizes; avoid all-caps for long strings; keep stroke contrast moderate so text survives compression.
- **Motion-safe:** carousels are static, but if exported from animated source, the still frames must stand alone.

---

## Slide-by-slide wireframe — 7-slide how-to carousel (1080 × 1350)

```
SLIDE 1 — COVER / HOOK                SLIDE 2–6 — BODY (template)
┌───────────────────────────┐        ┌───────────────────────────┐
│ ▓ progress bar (empty)    │        │ ▓▓▓░░░░  03/07            │  ← progress + index
│                           │        │ ─ topic label ──────────  │  ← header (locked)
│   7 MISTAKES              │        │                           │
│   KILLING YOUR            │  ← 130px│   ONE IDEA HEADLINE       │  ← 72px, top-left
│   ONBOARDING              │        │   short support line,     │  ← 44px
│                           │        │   one or two lines max.   │
│   [ one focal visual ]    │        │   ┌─────────────┐         │
│                           │        │   │  icon/visual│ )) peek │  ← bleeds off right
│   swipe to fix them  →    │  ← cue │   └─────────────┘         │
│ @handle                   │        │ @handle                   │  ← footer (locked)
└───────────────────────────┘        └───────────────────────────┘

SLIDE 7 — CTA / CLOSE
┌───────────────────────────┐
│ ▓▓▓▓▓▓▓  07/07            │  ← progress full
│ ─ topic label ──────────  │
│                           │
│   FOUND THIS USEFUL?      │  ← 80px
│                           │
│   Save it ⌄  ·  Follow    │  ← one primary ask, point at icon
│   @handle for more        │
│                           │
│   [ logo ]   recap line   │  ← rhymes with cover, save-worthy
│ @handle                   │
└───────────────────────────┘
```

Notes: progress bar fills across the set · index in the same spot every slide · footer @handle on all 7 (screenshot credit) · slide 7 visually echoes slide 1 for the loop · backgrounds alternate (cover bold → bodies alternate light/dark → CTA bold).

---

## Content-pattern table

| Pattern | Cover hook | Body structure | Close |
|---|---|---|---|
| **Listicle** | "7 X that Y" + number badge | 1 item per slide, numbered, parallel layout | "Save the list" + recap |
| **How-to / tutorial** | "How to X in N steps" | 1 step per slide, sequential, step marker | "Try it → link in bio" |
| **Myth vs fact** | "X myths about Y" | Split slide: ✗ myth (top/red) vs ✓ fact (bottom/green) + icon | "Stop believing #1 — save this" |
| **Before / after** | Split or teaser of the "after" | Each slide one transformation; consistent before-left/after-right | "Want this? Follow / DM" |
| **Story / case study** | A hook from the result ("$0 → $40k") | Chronological beats, one per slide, continuous timeline line | Lesson + CTA |
| **Framework / model** | Name the framework + intrigue | One component per slide, builds the whole on the last | Save the full framework |
| **Stat / data drop** | The single most shocking number, huge | One stat per slide, big number + 1-line context | Source + "follow for data" |

For myth/fact and before/after, lock the visual convention (which side, which color, which icon) on slide 1 and never flip it — the user learns the pattern and reads faster.

---

## Production

Carousels are ideal to generate as **HTML → PNG slides**: build one HTML template with locked chrome (header, footer, progress, margins) and a swappable content slot, then render each slide to a 1080 × 1350 PNG. This guarantees pixel-consistent margins, type sizes, and brand bars across slides — the hardest thing to keep consistent by hand. See [../13-production/production-and-tools.md](../13-production/production-and-tools.md) for the HTML→PNG pipeline, fonts/embedding, color profile, and export settings.

---

## Agent checklist

- Pick ONE aspect ratio (default 4:5 / 1080×1350) and apply it to every slide.
- Spend the most design effort on the cover: big hook (~90–160 px), one clear subject, an open loop, an explicit swipe cue.
- Put exactly one idea on each body slide; split anything with two headlines.
- Size text for the phone: min ~30–40 px effective, short 20–35 char lines, 4.5:1 contrast, left-aligned body.
- Build a locked template (margins, header, footer, progress indicator) and vary only the content zone.
- Add continuity cues — peek/bleed off the right edge, running line, "n/7" index — so the thumb keeps swiping.
- Keep @handle on every slide's footer for screenshot credit; make one slide a standalone save-worthy reference.
- Default to 6–8 slides; cut filler — every weak slide is an exit point.
- Respect safe zones (top/bottom ~120 px feed, ~250 px reels) so no UI covers key content.
- End on a single clear CTA (save / follow / link) that visually rhymes with the cover for the loop.
- Write per-slide alt text and don't rely on color alone for do/don't, myth/fact, before/after.
- Generate as HTML→PNG slides for pixel-consistent chrome — see the production file.
