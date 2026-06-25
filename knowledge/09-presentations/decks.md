# Presentation & Slide Design

> Purpose: Design world-class decks — pitch, sales, conference, strategy, webinar, and leave-behind — that carry one clear message with restraint, structure, and readable craft.

**When to read this:** Any time you are asked to create, structure, restyle, or review a slide deck (PowerPoint, Google Slides, Keynote, HTML→PDF). Pair with [composition](../08-visual-composition/composition.md), [format specs](../08-visual-composition/format-specs.md), [color](../02-foundations/color.md), [typography](../02-foundations/typography.md), [brand systems](../08-visual-composition/brand-systems.md), and [production & tools](../13-production/production-and-tools.md).

---

## The one decision that governs everything: speaker-support vs. read-alone

Before laying out a single slide, classify the deck. The two modes are **opposites** and must never be mixed in one file.

| | **Speaker-support** (presented live) | **Read-alone / leave-behind** (sent, no narrator) |
|---|---|---|
| Who fills the gaps | The presenter's voice | The slide itself |
| Text density | Sparse — headline + one visual | Dense — full sentences, captions, footnotes |
| Words per slide | ~10–25 | 50–150+ |
| Body type size | 28–32pt+ | 16–22pt acceptable |
| Slide count | Fewer, bigger | More, self-contained |
| Optimized for | Attention on the speaker | Comprehension when skimmed solo |
| Failure mode | Wall of text the speaker reads aloud | Cryptic headlines no one can decode |

**Why never mix them:** a slide built to be read silently is a terrible backdrop for a talk (audience reads ahead, ignores you), and a sparse keynote slide is useless emailed out (recipients see "Growth" over a line chart and learn nothing). If a client needs both, build the speaker deck, then produce a **separate** read-alone version — or add a written appendix / detailed presenter notes. Do not compromise to a mushy middle that serves neither.

The rest of this file assumes **speaker-support** unless noted, because that is where most decks fail and where design craft matters most.

---

## Deck types and how design differs

| Type | Audience & setting | Length | Density | Tone / look | Design priority |
|---|---|---|---|---|---|
| **Investor pitch** | VCs, projector + emailed | 10–15 | Lean live, denser if sent | Confident, branded, ambitious | Narrative arc + traction proof; one bold claim per slide |
| **Sales / demo** | Prospect, screen-share | 8–20 | Speaker-support | Benefit-led, customer-centric | Their problem first; product as the answer; social proof |
| **Conference talk** | Large room, dark venue | 20–60 | Very sparse, visual | Big ideas, big type, images | Back-of-room legibility; one idea per slide; dark deck |
| **Internal / strategy** | Leadership, conf room | 10–30 | Often read-alone | Sober, data-forward, on-brand | Decision clarity; SCQA; the "ask"/recommendation explicit |
| **Webinar** | Remote, small thumbnail | 15–40 | Medium | Friendly, paced | Legible at 50% zoom; progress cues; talking-head safe zone |
| **Leave-behind / report** | Solo reader, PDF | 15–50 | Dense (read-alone) | Documentary, complete | Self-explanatory; assertion titles; captioned charts |

Set canvas, type scale, and density from this row before designing. A conference slide and a leave-behind report are different products even with identical content.

---

## Narrative structure (decide before you design)

Design serves the story. No layout rescues a deck with no through-line.

- **One core message.** State the single sentence the audience must remember in the elevator afterward. Every slide either advances it or gets cut. Write it down first.
- **Story arc:** `Problem → Solution → Proof → Ask`. Hook with tension (the problem/stakes), resolve it (your solution), substantiate it (data, traction, demo), then make the explicit ask (invest, buy, approve, act). Decks that open with "About us / our history" bury the hook — start with the audience's problem.
- **SCQA (great for strategy/exec):** **S**ituation (shared context) → **C**omplication (what changed / the tension) → **Q**uestion (what should we do?) → **A**nswer (your recommendation). Front-loads the "why are we here."
- **The "so what" test:** every slide must answer *so what?* in one sentence. If a slide has no takeaway, it is a reference appendix slide, not a story slide — move it to the appendix.
- **Length discipline:** rough budget ~1–2 minutes per slide live. A 30-min talk is ~15–25 slides, not 60. An 80-slide deck is almost always an un-edited document masquerading as a presentation.

### The classic pitch-deck sequence (Sequoia / Guy Kawasaki lineage)

~10–12 core slides. Order is a strong default, not a law — but skipping the early slides (problem, why-now) is the most common fatal mistake.

| # | Slide | Job / the one question it answers | Make-or-break content |
|---|---|---|---|
| 1 | **Title** | Who you are, in one line | Company, one-line positioning, logo |
| 2 | **Problem** | Why does this matter *now*? | A real, painful, specific problem |
| 3 | **Solution** | What you do about it | Clear value prop, not a feature list |
| 4 | **Why now** | Why didn't this exist before? | Shift in tech, market, regulation, behavior |
| 5 | **Market size** | How big can this get? | TAM/SAM/SOM, bottom-up over hand-wavy top-down |
| 6 | **Product** | How does it actually work? | Screens, demo, the "aha"; show don't tell |
| 7 | **Business model** | How do you make money? | Pricing, unit economics, who pays |
| 8 | **Traction** | Is it working? | Growth chart, revenue, logos, retention — the proof |
| 9 | **Competition** | Why you, not them? | Honest landscape + your differentiation |
| 10 | **Team** | Why are *you* the ones to win? | Founder–market fit, relevant credibility |
| 11 | **Financials** | Where is this heading? | Projections + key assumptions |
| 12 | **Ask** | What do you want? | Amount raising, use of funds, milestones |

Sales decks reuse the spine but swap traction-for-them: Problem → Solution → How it works → Proof (case studies, ROI) → Pricing → Next step.

---

## Slide anatomy & archetypes

Most slides are one of ~10 archetypes. Build each as a **master layout** so they recur identically. Layout sketches below use a 16:9 frame.

**Title / cover** — establishes brand and topic; spare.
```
┌──────────────────────────────┐
│                              │
│   BIG DECK TITLE             │  ← 44–60pt
│   One-line subtitle / date   │  ← 20–24pt
│                              │
│ logo                  name • │  ← footer
└──────────────────────────────┘
```

**Section divider** — resets attention between acts. Use a color flip (inverted background) so it reads as a chapter break.
```
┌──────────────────────────────┐
│██████████████████████████████│
│███   02 — How it works    ███│  ← inverted bg, big number
│██████████████████████████████│
└──────────────────────────────┘
```

**Agenda / roadmap** — 3–5 items max; reusable as a progress tracker (dim completed items).
```
┌──────────────────────────────┐
│ Today                        │
│  01  The problem             │
│  02  Our approach    ◄ here  │
│  03  Proof                   │
│  04  What we're asking       │
└──────────────────────────────┘
```

**Big statement / quote** — one sentence owns the frame. Maximum impact, near-zero density.
```
┌──────────────────────────────┐
│                              │
│   "We cut onboarding from    │  ← 36–54pt
│    3 weeks to 3 hours."      │
│                  — Customer  │  ← attribution 20pt
└──────────────────────────────┘
```

**Single chart** — one chart, one message. Title is the takeaway; chart is the evidence.
```
┌──────────────────────────────┐
│ Revenue tripled in 12 months │  ← assertion title
│  ┌────────────────────────┐  │
│  │            ╱╲___╱       │  │  ← one series highlighted
│  │      ____╱              │  │
│  └────────────────────────┘  │
│  Source: internal, FY24      │  ← caption
└──────────────────────────────┘
```

**Comparison** — two columns, parallel structure. Old/new, us/them, before/after. Keep rows aligned.
```
┌──────────────────────────────┐
│ Headline takeaway            │
│  ┌─────────┐   ┌─────────┐   │
│  │ Before  │   │ After   │   │  ← accent on the winning side
│  │ • slow  │   │ • fast  │   │
│  │ • manual│   │ • auto  │   │
│  └─────────┘   └─────────┘   │
└──────────────────────────────┘
```

**Timeline / process** — left→right flow, 3–5 steps, consistent node shape. Avoid >6 steps (split the slide).
```
┌──────────────────────────────┐
│ How it works in 4 steps      │
│  ①────▶②────▶③────▶④         │
│  Sign  Sync  Score  Ship     │
└──────────────────────────────┘
```

**Team** — photo grid, consistent crop and size; name + role + one credibility line.
```
┌──────────────────────────────┐
│ Built by operators           │
│  ◯      ◯      ◯              │  ← identical circular crops
│  Asha   Ravi   Lin           │
│  CEO    CTO    Design        │
└──────────────────────────────┘
```

**Image full-bleed** — photo to all four edges; text on a scrim or in a clear-space corner. Emotional/section beats.
```
┌──────────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓ Overlaid headline ▓▓▓▓▓▓▓▓▓│  ← scrim behind text for contrast
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
└──────────────────────────────┘
```

**Closing / CTA** — one action, contact, and a memorable last line. Not "Thank you / Questions?" with nothing else.
```
┌──────────────────────────────┐
│   Let's build this.          │
│   → invest@company.com       │  ← single clear next step
│   company.com                │
└──────────────────────────────┘
```

---

## The craft rules (non-negotiable)

1. **One idea per slide.** If you need "and" in the takeaway, it is two slides. More slides each doing one thing beats fewer crammed slides.
2. **Slide titles are takeaways, not labels (assertion-evidence model).** The title is the conclusion as a full sentence; the slide body is the evidence.

| Don't (label) | Do (assertion) |
|---|---|
| "Q4 Results" | "Q4 revenue grew 40%, beating plan" |
| "Market Overview" | "A $12B market growing 20% a year" |
| "Customer Feedback" | "Users cite onboarding as the #1 reason they stay" |
| "Architecture" | "One pipeline replaces three legacy systems" |

3. **Minimal text. Kill bullet walls.** The presenter speaks; the slide shows. Replace paragraphs with a phrase, a number, or an image. **6×6 guideline:** at most ~6 bullets, ~6 words each — and treat that as a ceiling to break *downward*, not a target. Prefer one idea to six bullets.
4. **Big, readable type.** Title ~40–54pt; body ~24–32pt; **never below ~18pt** on a presented slide (anything smaller is an appendix/data table for read-alone only). If text won't fit at size, you have too much text — cut it, don't shrink it. See [typography](../02-foundations/typography.md).
5. **Consistent master & grid.** Every slide inherits from a master: same title position, same margins, same type scale, same accent. Inconsistent placement reads as amateurism even when individual slides look fine. See [composition](../08-visual-composition/composition.md).
6. **Generous margins & safe zones.** Keep a clear margin (~5% of width, ~64–96px on 1920px) on all sides; nothing critical in the outer edge (projectors/overscan crop it). Whitespace is a feature, not wasted space.
7. **High contrast.** Body text vs. background ≥ 4.5:1 (large text ≥ 3:1). Pale-gray-on-white and thin type vanish at the back of a bright room. See [color](../02-foundations/color.md).
8. **One accent color.** A neutral base (near-black / off-white) plus **one** brand accent used only to direct the eye — the highlighted bar, the key number, the CTA. Rainbow decks have no hierarchy.
9. **Consistent footer & page numbers.** Quiet, small (12–14pt), bottom corner; helps Q&A ("slide 14"). Suppress on title and full-bleed slides.

---

## Data & charts on slides

A slide chart is not a spreadsheet export. Its job is to make one point obvious in three seconds. For choosing the *right* chart type for the data, see [data display](../03-components/data-display.md).

- **One message per chart.** Decide the sentence first ("retention is climbing"), then strip everything that doesn't serve it.
- **Label directly, kill the legend.** Put series names at the end of their lines; legends force a back-and-forth eye scan. Same for data labels over axis-reading where it matters.
- **Highlight the point with color.** Mute every bar/line to gray, color *only* the one that matters in your accent. Color is the argument.
- **Strip chart junk.** Remove gridlines (or make them faint), drop shadows, 3D, heavy borders, redundant axis ticks, and the chart's own title (your slide title is the title).
- **Big number for big points.** A single KPI ("3.2×") at 80–120pt beats a chart when the message *is* the number. Add a tiny label and trend arrow.
- **Caption the source.** `Source: …, FY24` at ~12pt builds trust and is required for read-alone decks.
- **Round and simplify.** "$2.4M," not "$2,431,907." Slides are for magnitude, not precision; precision lives in the appendix.

```
Don't: 8 bars all blue, legend on the right, gridlines, "Sales by Quarter" chart title under a slide titled "Sales by Quarter"
Do:    7 bars gray + 1 bar accent, value labels on bars, no legend, slide title = "Q4 broke our record at $2.4M"
```

---

## Visual consistency & format

- **Slide size — 16:9 is the default.** `1920×1080` (or `1280×720`, same ratio). Use **4:3 (`1024×768`)** only for legacy projectors that demand it. Use **vertical (`1080×1350` / `1080×1920`)** for mobile/social or stories — design those as their own format, not a cropped 16:9. Full specs in [format specs](../08-visual-composition/format-specs.md).
- **Title-safe area.** Keep titles and key content inside the inner ~90% so projector overscan and video-call UI (camera bubble, control bar) don't clip them. On webinars, reserve a corner for the talking-head.
- **Master templates carry the brand.** Build layouts (title, section, content, chart, quote, closing) once in the theme/master so color, type, and spacing come from the [brand system](../08-visual-composition/brand-systems.md), not per-slide tweaks. Editing the master should reflow the whole deck.
- **Animation & transitions: restraint.** Default to **none** or a single subtle transition (a quick fade) used everywhere. No swirls, cubes, or page-curls. Use **build animations** purposefully — reveal a complex slide one element at a time so the audience follows your pacing — and only on transform/opacity. One transition style across the whole deck; consistency over variety.
- **Dark vs. light deck by venue.** **Dark** background (light text) for large, dim rooms (conferences, keynotes) — less glare, more cinematic. **Light** background for bright rooms, printed handouts, and dense data/read-alone decks (easier to read and print). Pick one and commit; don't alternate.

---

## Accessibility & delivery

- **Contrast for the back of the room** — meet 4.5:1, then go higher; what passes on your laptop can fail on a washed-out projector. Avoid red/green as the *only* distinction (color-blind safe). See [color](../02-foundations/color.md).
- **Size for the cheap seats** — if you can read it standing 2 m back from your laptop, the back row can read it on the projector. When unsure, go bigger.
- **Alt text in exported decks** — add alt text to images and charts so screen readers and accessible PDF exports work. Required for any deck shared as a document.
- **Presenter notes carry the words** — the narration belongs in the notes pane, *not* on the slide. Good notes let a colleague deliver your deck and make the exported file useful to absent attendees.
- **Don't read your slides verbatim.** The slide is the headline; you add the story, examples, and emphasis. Reading bullets aloud is the fastest way to lose a room — and a tell that the slide has too much text.
- **Reading order & structure** — logical heading/title structure exports to a tagged, navigable PDF; keep one clear title per slide.

---

## Production: how to actually build it

Pick the tool by output and constraints; design *inside the brand system* either way ([brand systems](../08-visual-composition/brand-systems.md)). Full tooling in [production & tools](../13-production/production-and-tools.md).

| Need | Best path |
|---|---|
| Editable `.pptx` for a client to own/edit | Generate via code/skills (e.g. `python-pptx`) or PowerPoint; ship the source file |
| Collaborative, link-shared, live edits | Google Slides |
| Pixel-perfect custom layout, designer control | Design in HTML/CSS → export to PDF (1920×1080 page), or Keynote |
| Fast first draft / AI-generated deck | Gamma, Canva, Beautiful.ai — then refine for craft, don't ship raw |
| On-brand, locked template for a team | Master/theme built from the brand system; lock layouts |

Production rules regardless of tool:
- **Build the master/theme first**, then pour content into layouts — never style slides one at a time.
- **Embed or use safe fonts.** Unembedded brand fonts fall back to Arial on the client's machine and wreck the design. Embed fonts in `.pptx`/PDF, or use web-safe/Google fonts.
- **Use real text, not screenshots of text.** Keep text selectable, editable, accessible, and crisp at any zoom.
- **Export deliverables:** a presented deck → editable source **plus** a flattened PDF (fonts embedded, won't reflow) for sending.

---

## Top mistakes

| Mistake | Why it kills the deck | Fix |
|---|---|---|
| **Wall of text** | Audience reads, ignores speaker; nothing memorable | One idea, one image, a phrase — narration in notes |
| **Tiny fonts** | Unreadable past row 3; signals "too much content" | ≥24pt body, never <18pt; cut text instead of shrinking |
| **Label titles** | "Overview" tells the reader nothing | Assertion titles — the takeaway as a sentence |
| **Clip-art & stock cheese** | Cheapens the brand instantly | Real product shots, real data, brand imagery, or nothing |
| **Inconsistent everything** | Shifting fonts/colors/positions read as careless | One master, one type scale, one accent, fixed margins |
| **Reading slides verbatim** | Redundant and dull; loses the room | Slide = headline, you = the story |
| **Rainbow color use** | No hierarchy; eye doesn't know where to look | Neutral base + one accent for emphasis only |
| **80-slide decks** | A document pretending to be a talk | Cut to the through-line; push detail to an appendix |
| **Gratuitous transitions** | Distracting, dated, amateur | One subtle transition (or none) everywhere |
| **Mixing speaker & read-alone** | Serves neither audience | Pick one mode; build a second version if both are needed |

---

## Agent checklist

- [ ] State the **one core message** and the deck **type/mode** (speaker-support vs. read-alone) before designing.
- [ ] Outline a narrative arc (Problem→Solution→Proof→Ask or SCQA); confirm every slide passes the "so what?" test.
- [ ] Set canvas to **16:9 (1920×1080)** unless the venue demands otherwise; respect title-safe margins.
- [ ] Write **assertion titles** — the takeaway as a full sentence, never a label.
- [ ] Enforce **one idea per slide**; kill bullet walls; obey 6×6 as a ceiling, not a goal.
- [ ] Hold type sizes: title ~40–54pt, body ~24–32pt, **never below ~18pt**; cut text rather than shrink it.
- [ ] Build from a **master/theme** so margins, type scale, **one accent color**, and footer stay consistent.
- [ ] Make every chart say **one thing**: mute to gray, highlight in accent, label directly, strip junk, cite source.
- [ ] Verify **contrast ≥4.5:1** and back-of-room legibility; choose dark vs. light deck by venue.
- [ ] Put narration in **presenter notes**, add **alt text**, and never read slides verbatim.
- [ ] Keep transitions to one subtle style (or none); use builds only to pace complex slides.
- [ ] Embed fonts; export an **editable source + flattened PDF**; draw all styling from the brand system.
