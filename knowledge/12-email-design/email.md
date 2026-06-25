# Email & Newsletter Design

> Purpose: Design and build email that renders correctly across a hostile, fragmented client landscape — the visual design AND the technical constraints of the email medium, in one reference.

**When to read this:** Any time you design, lay out, style, or code an HTML email or newsletter — marketing campaign, transactional notification, digest, welcome series, or any branded email. Read this BEFORE writing markup. (Subject-line copy, list strategy, and deliverability/sending live in the separate marketing repo — this file is DESIGN + build constraints.)

---

## Why email ≠ web (the core mental model)

Email is **HTML from ~2005 rendered by a dozen incompatible engines you can't detect or patch.** You are not building a webpage; you are building a printable document that must degrade gracefully when half its CSS is stripped. Design AROUND the constraints below — never assume modern CSS works.

| Constraint | Reality | Design around it |
|---|---|---|
| Layout engine | No flexbox/grid reliably (Outlook desktop uses **Microsoft Word's** engine) | `<table>`-based layout, `role="presentation"` |
| CSS delivery | No `<link>` external CSS, no reliable `<style>` in Gmail | **Inline every style** as `style="..."` attributes |
| JavaScript | Stripped everywhere. No JS, ever | Static design; "interactivity" = links only |
| CSS support | Partial & inconsistent (`float`, `position`, `background-image`, modern units flaky) | Stick to a [tested safe subset](#css-support-the-safe-subset) |
| Images | **Blocked by default** in many clients until user opts in | Never put critical info in images; alt text + bg colors |
| Fonts | Custom web fonts fail in Outlook/Gmail/most clients | Web-safe stacks with fallbacks |
| Dark mode | Clients auto-invert colors unpredictably | Design + test light AND dark |
| Width | Tiny preview panes; mostly mobile opens | 600px single column, fluid down |
| Size | Gmail **clips at ~102KB**; slow connections | Keep HTML lean, host images |

If you internalize one thing: **inline CSS + tables + no critical info in images.** Everything else is detail.

---

## Layout & structure

### The 600px single-column default
- **Body width: 600px** (safe everywhere; max ~640px). Center it on a full-width background "wrapper" table.
- **Single column wins.** Multi-column reflows unpredictably and breaks on mobile. Default to one column; use 2 columns only for simple side-by-side (image + text) that you'll stack on mobile.
- **Generous padding:** 20–40px inner padding (left/right ≥ 20px so text never kisses the edge). Vertical rhythm between blocks: 24–40px.
- See spacing/grid foundations in [`../02-foundations/layout-spacing.md`](../02-foundations/layout-spacing.md) — the 8pt scale still applies (use 16/24/32/40px gaps).

### The modular block system
Build every email from stacked, swappable blocks — easy to reorder, reuse, and test:

| Block | Role | Notes |
|---|---|---|
| **Preheader** | Hidden preview text | First ~40–90 chars; see [preheader](#preheaderpreview-text) |
| **Header / logo** | Brand anchor | Logo 120–200px wide; link to site; padded solid bg (dark-mode safe) |
| **Hero** | Primary message + image | One idea; headline + supporting line + primary CTA |
| **Body blocks** | Content sections | Text, image+text rows, dividers, content cards |
| **Primary CTA** | The one action | Bulletproof button, above the fold |
| **Secondary content** | Supporting links/cards | Newsletter digest items, related links |
| **Footer** | Legal + utility | Unsubscribe, physical address, social, preferences |

### Visual hierarchy & scanning
- Readers **scan in an F-pattern** — most important content top-left and along the top. Front-load the value.
- **Inverted pyramid:** logo → concise headline → supporting copy → ONE primary CTA, all above the fold (first ~300–400px on mobile). Everything below is bonus.
- **One primary action per email** (same non-negotiable as the rest of the system). Secondary links are fine but visually subordinate — see [`../03-components/components.md`](../03-components/components.md) for primary vs. secondary button treatment.

### Mobile-first / responsive
- **Most opens are mobile** — design the 1-column mobile view first, then enhance for desktop.
- **Hybrid/spongy approach** (most robust): tables sized with `width` capped by `max-width`, so layout fluidly shrinks even in clients (older Outlook, some apps) that ignore media queries. Add `@media` tweaks on top as progressive enhancement — never as the only line of defense.
- Stack multi-column rows on narrow screens (`@media (max-width:600px){ .col{display:block!important;width:100%!important;} }`), but assume some clients won't honor it — so the default must already be acceptable.

---

## Typography for email

Custom fonts are unreliable: Outlook and Gmail ignore `@font-face` and fall back to a default (often Times New Roman in Outlook) — so **never rely on a web font carrying the design.** Use web-safe stacks; treat any web font as progressive enhancement with a strong fallback.

```
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
/* serif fallback stack: Georgia, 'Times New Roman', Times, serif; */
```

| Element | Size | Line-height | Notes |
|---|---|---|---|
| Body | **14–16px** (16 preferred) | **1.4–1.6** (22–26px) | Below 14px is unreadable on mobile |
| H1 / hero | **22–30px** | 1.2–1.3 | Bigger is fine; keep it bold |
| H2 / section | 18–22px | 1.3 | |
| Caption / footer | 12–13px | 1.4 | Footer/legal only; never body |

- **Left-align body text.** Centered paragraphs hurt readability past one line; reserve centering for short hero headlines and CTAs.
- **Measure:** 1-column at 600px naturally yields a good line length; keep paragraphs short (2–4 lines).
- Set font styles **inline on every text element** (`<p>`, `<td>`, `<a>`) — clients drop inherited/`<style>` rules. Don't rely on cascade.
- Full type system rationale: [`../02-foundations/typography.md`](../02-foundations/typography.md).

---

## Color & dark mode

- **Brand color** drives the primary CTA, links, and accents; keep the rest neutral. One accent, consistent — same restraint principle as [`../08-visual-composition/brand-systems.md`](../08-visual-composition/brand-systems.md).
- **Contrast:** body text ≥ **4.5:1** against its background; large text/UI ≥ 3:1 — same WCAG bar as the rest of the system. Don't ship light-gray-on-white. See [`../02-foundations/color.md`](../02-foundations/color.md) and [`../05-quality/accessibility.md`](../05-quality/accessibility.md).
- Set an explicit **background color on the body/wrapper table** (not transparent) so text stays legible if an image background fails to load.

### The dark-mode problem (test BOTH modes)
Clients handle dark mode three ways — **partial inversion** (Apple Mail, Outlook recolor your palette), **full inversion** (some apps flip everything), or **no change** (Gmail web). You can't control which, so design defensively:

| Problem | Fix |
|---|---|
| **Transparent PNG logo → white halo / invisible on dark** | Use a logo on a **solid or padded background** (white pill / brand-color block), or supply a dark-mode logo via `@media (prefers-color-scheme: dark)` where supported |
| Dark text on a color block gets inverted to unreadable | Pick text/bg pairs that stay legible if lightly inverted; avoid pure-black text on brand color |
| Pure white (#FFFFFF) areas darkened to muddy gray | Use off-white (#FAFAFA) backgrounds; they invert more gracefully |
| Borders/dividers vanish | Use mid-tone borders that read in both modes |
| Brand color shifts | Add `color-scheme: light dark;` + `<meta name="color-scheme">` and `<meta name="supported-color-schemes" content="light dark">` to opt into predictable handling |

**Always preview the email in dark mode** (Apple Mail dark, Outlook dark) before sending — it's the #1 source of "looks broken" reports.

---

## Buttons / CTAs

Image-only buttons disappear when images are blocked. **Build real, "bulletproof" buttons** out of HTML/CSS so they render even with images off and stay tappable.

**Bulletproof technique (conceptual):** a styled `<a>` inside a `<table>` cell with background-color, padding, and border-radius on the cell — plus **VML roundrect markup** in an Outlook-only conditional comment (`<!--[if mso]>...<![endif]-->`) because Outlook ignores padding/`border-radius` on links. The result: solid filled button everywhere, including Outlook.

| Rule | Spec |
|---|---|
| Tap target | **≥ 44×44px** (use generous cell padding: ~14px vertical / 24–32px horizontal) |
| Contrast | Button text vs. fill ≥ 4.5:1; fill vs. page bg ≥ 3:1 |
| Label | **Descriptive verb phrase** — "Browse spring listings", not "Click here" |
| Primary vs. secondary | One filled primary; secondaries are outlined or text links, visually quieter |
| Don't | Don't rely on an **image-only** button — it's gone when images are blocked and unreadable to screen readers |

- Give the CTA breathing room (24–32px above/below) and place the primary one above the fold.
- Component anatomy & states: [`../03-components/components.md`](../03-components/components.md).

---

## Imagery

Email images are **blocked by default** in many clients until the user clicks "display images." Design so the email works with **images off**.

| Rule | Why / spec |
|---|---|
| **Image-to-text balance** | Skew toward real text (~60–80% text). All-image emails look broken when blocked and hurt deliverability |
| **Critical info / CTA never image-only** | If images are off and the CTA was an image, there's no action. Keep headline, offer, and CTA as live text |
| **Alt text always** | Every `<img>` gets meaningful `alt` (it shows when blocked + serves screen readers). Decorative images: `alt=""` |
| **Background colors behind images** | Set `bgcolor`/`background-color` on the image's cell so the area isn't a blank white hole when blocked |
| **Retina / 2x** | Export at **2× display size**, then constrain with `width`/`height` attributes (e.g., a 300px slot → 600px source). Sharp on high-DPI screens |
| **File weight** | Compress hard. Keep individual images < ~200KB, total email images modest; total HTML < 102KB to dodge [Gmail clipping](#client-gotchas) |
| **Always set width + height** | Explicit dimensions reserve space and prevent reflow when images load/are blocked |
| **Hero** | One focused hero image; don't bury the headline IN the image — overlay text in live HTML, not baked-in pixels |
| **GIFs** | Outlook shows only the **first frame** — so design the first frame to stand alone (key message visible without animation). Keep GIFs light |

- Host images on a reliable HTTPS server/CDN (no embedding huge base64 — it bloats HTML toward the clip limit). Production hosting notes: [`../13-production/production-and-tools.md`](../13-production/production-and-tools.md).

---

## Preheader/preview text

The **preheader** is the snippet shown after the subject line in the inbox list. Control it or the client grabs whatever text comes first (often "View in browser" — wasteful).

- **Length: ~40–90 characters** (clients vary; the first ~40–50 chars matter most on mobile).
- Place a hidden preheader element right after `<body>`:
  ```html
  <div style="display:none;max-height:0;overflow:hidden;mso-hide:all;
              opacity:0;color:transparent;height:0;width:0;">
    Your 40–50 char hook that complements (not repeats) the subject.
  </div>
  ```
- **Pad with a zero-width-space run** (`&zwnj;&nbsp;` repeated) after your text so following body content doesn't bleed into the preview.
- Make it complement the subject line, not duplicate it. (Subject-line copy strategy lives in the marketing repo.)

---

## Sender / from presentation & footer

### From / sender
- **From name** is the most-seen identity element — use a recognizable name ("Acme Team", "Jane at Acme"), consistent across sends. Consistency builds recognition and trust.
- This is presentation/design; the authentication that lets it land (SPF/DKIM/DMARC) is deliverability — handled in the marketing repo, not here.

### The footer (legal + design role)
The footer is **required infrastructure**, not an afterthought — design it to be clear and findable, not hidden:

| Element | Requirement |
|---|---|
| **Unsubscribe** | Clearly visible, one-click, genuinely working. Legally required for marketing email |
| **Physical mailing address** | Required by law in many jurisdictions (e.g., CAN-SPAM) |
| **Sender identity** | Who sent this; why the recipient is getting it |
| **Preferences** | Optional but reduces unsubscribes (let them dial down frequency) |
| **View in browser** | Fallback link for clients that mangle rendering |

- Footer type is small (12–13px) and muted but **must still pass contrast** — don't make unsubscribe a 9px ghost-gray link. Accessibility + legal both demand it's readable.

---

## Accessibility

Email a11y is the same WCAG bar as the web ([`../05-quality/accessibility.md`](../05-quality/accessibility.md)) plus email-specific moves:

- **`role="presentation"` on every layout table** — tells screen readers "this is layout, not data," so it reads content in order instead of announcing rows/columns.
- **Set `lang`** on `<html>` (e.g., `lang="en"`) so screen readers use correct pronunciation.
- **Real text over text-in-images** — live HTML text is selectable, translatable, resizable, and screen-reader-readable; baked-in text is none of these. (Also survives image blocking.)
- **Alt text** on every meaningful image; `alt=""` on decorative ones.
- **Contrast** ≥ 4.5:1 body / 3:1 large & UI — applies to footer and dark mode too.
- **Logical DOM order** = reading order: code top-to-bottom in the sequence content should be heard, even if visually rearranged.
- **Descriptive link text** ("Read the spring market report"), never "click here."
- Use a single, clear **document structure**; don't fake headings with bold-only text where a semantic emphasis helps assistive tech.

---

## Newsletter-specific design patterns

Newsletters are scannable digests, not single-message campaigns — design for skim-and-dip:

- **Consistent template** issue-to-issue: same header, section order, type scale, color. Recognition builds habit; consistency is the brand asset.
- **Clear sectioning:** distinct sections with section headers and dividers (rules, spacing, or background bands) so readers can jump to what interests them.
- **Content cards:** each story = a card (optional thumbnail + headline + 1–2 line teaser + "Read more" link). Uniform card structure makes a long email scannable.
- **Table of contents / anchor links** at the top for long issues (anchor links work in most clients) — let readers jump to sections.
- **One feature + several briefs:** a hero story up top, then shorter digest items. Don't make every item compete as equal-weight.
- **Scannability over density:** generous spacing between items; a wall of equal-size text is unreadable. Whitespace is a feature.
- Keep the **primary CTA discipline** even here — each issue should still have one clear "main" action even amid the digest.

---

## CSS support: the safe subset

| Safe (use freely) | Risky (test or avoid) | Banned (will fail) |
|---|---|---|
| `<table>` layout, `align`, `valign` | `background-image` (Outlook needs VML) | External `<link>` stylesheets |
| Inline `style="..."` | `@media` queries (ignored by some) | `<script>` / any JS |
| `width`/`height` attributes | `border-radius` (Outlook ignores on links) | `position`, `float` (unreliable) |
| `color`, `background-color`, `bgcolor` | `max-width` (great, but pair with table widths) | Flexbox / CSS grid |
| `padding`/`margin` on `<td>` (prefer padding) | Web fonts (`@font-face`) | Form fields, embeds, iframes (mostly) |
| `font-family/size/weight`, `line-height` | `display:none` (mostly works for preheader) | Negative margins, `calc()` (flaky) |

**Rule:** if you're unsure whether a property works, assume Outlook strips it and provide a table-based fallback.

---

## Production & build

Full tooling, hosting, and testing guidance: [`../13-production/production-and-tools.md`](../13-production/production-and-tools.md). Email essentials:

1. **Author with a tested email framework, not hand-rolled tables.** Use **MJML** (compiles clean semantic markup → bulletproof responsive table HTML, handles Outlook VML/conditionals for you) or a vetted framework (Foundation for Emails, Maizzle). Hand-coding raw tables is error-prone — let the framework emit the gnarly conditional comments.
2. **Inline the CSS before sending.** If you wrote `<style>`, run an inliner (Juice, Premailer, or your ESP's auto-inliner) so styles survive Gmail/clients that strip `<head>`. MJML inlines for you.
3. **Keep total HTML < 102KB** to avoid Gmail clipping (it hides everything past the limit behind "View entire message," cutting off your footer/unsubscribe). Minify; host images externally.
4. **Test across clients before every send** — conceptually a Litmus / Email on Acid render matrix: at minimum **Gmail (web + iOS/Android app), Outlook (Windows desktop + web), Apple Mail (macOS + iOS)**, in **light and dark mode**. These three families cover the bulk of opens and break in the most different ways.
5. **Send a real test** to seed addresses and check: images-off rendering, dark mode, tap targets on a phone, every link, the unsubscribe flow, and the preheader as it appears in the inbox list.
6. **Validate against [`../05-quality/review-checklist.md`](../05-quality/review-checklist.md)** for the universal design bar (states, contrast, hierarchy) before declaring done.

---

## Client gotchas

| Client / issue | What breaks | Mitigation |
|---|---|---|
| **Outlook (Windows desktop)** | Renders with **MS Word engine** — ignores `padding`/`margin` on some elements, `border-radius` on links, `background-image`, `max-width`; adds extra space | Use tables + cell padding; VML for buttons & bg images; `mso-` properties; conditional `<!--[if mso]>` blocks |
| **Outlook word/line spacing** | Mysterious gaps; line-height ignored; image gaps under `<img>` | Set `mso-line-height-rule:exactly`; `font-size:0;line-height:0` on spacer cells; `display:block` on images |
| **Gmail** | **Clips message at ~102KB**; strips `<style>` in some contexts; doesn't support `<head>` reliably | Inline all CSS; keep HTML lean; host images; test the clip boundary |
| **Gmail (no dark-mode CSS)** | Ignores `prefers-color-scheme` on web; does its own light treatment | Don't depend on dark-mode media queries; design a palette that survives untouched |
| **Apple Mail** | Best CSS support BUT aggressive **auto dark-mode inversion**; **MPP inflates open tracking** | Design + test dark mode; treat open rate as directional only (measurement → marketing repo) |
| **Image blocking (all)** | Images off by default → broken-looking, info-less email | Alt text, bg colors, live-text CTAs, text-heavy balance |
| **No media-query support (some apps/older Outlook)** | Responsive `@media` ignored → desktop layout on mobile | Hybrid/fluid tables with `max-width` so it shrinks WITHOUT media queries |
| **Yahoo / AOL** | Can strip/merge classes; `<style>` quirks | Inline styles; avoid relying on class-based CSS |
| **Dark-mode logo halo** | Transparent PNG → ugly halo / invisible | Solid/padded-background logo; dark-mode swap where supported |

---

## Agent checklist

- [ ] Build with **600px single column**, table-based layout, every table `role="presentation"`.
- [ ] **Inline all CSS** (or compile via MJML + an inliner); assume `<style>` and external CSS are stripped.
- [ ] Body text **14–16px / line-height ~1.5**, left-aligned, web-safe font stack with fallbacks — styles set inline on each element.
- [ ] **One primary action**, a **bulletproof (HTML, not image) CTA** above the fold, tap target ≥ 44×44px, contrast ≥ 4.5:1.
- [ ] **No critical info or CTA in images**; every `<img>` has alt text, explicit width/height, a 2× source, and a background color on its cell.
- [ ] Add a hidden **~40–50 char preheader** padded so body text doesn't bleed into the preview.
- [ ] Footer carries **working one-click unsubscribe + physical address**, readable (passes contrast), not hidden.
- [ ] Design and **preview in dark mode**; use solid/padded logo, off-white bgs, `color-scheme` meta to avoid inversion damage.
- [ ] Keep total **HTML under 102KB** (host images externally, minify) to dodge Gmail clipping.
- [ ] **Test the render matrix** — Gmail / Outlook desktop+web / Apple Mail, light + dark, images on AND off — before send.
- [ ] Send a **live seed test**: check links, unsubscribe flow, mobile tap targets, and images-off appearance.
- [ ] Run the universal QA pass in [`../05-quality/review-checklist.md`](../05-quality/review-checklist.md) before declaring done.
