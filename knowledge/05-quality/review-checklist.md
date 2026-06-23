# Review Checklist

> Purpose: The master QA gate. Run every check here against any UI before declaring it done — each item is objectively verifiable, not a matter of taste.

**When to read this:** After building any view and before saying "done." This is the single most important self-review file. When a check fails, jump to the linked deep-dive: [accessibility.md](./accessibility.md) · [responsive.md](./responsive.md) · [performance.md](./performance.md) · [color.md](../02-foundations/color.md) · [typography.md](../02-foundations/typography.md) · [layout-spacing.md](../02-foundations/layout-spacing.md).

---

## How to use this file

- Run it **top to bottom** on the actual rendered UI, not the code in your head.
- Every box is **falsifiable** — you can point at the screen and say pass/fail. If you can't verify it, it fails.
- A UI is **not done** while any box in **Accessibility** or **Components & states** is unchecked — those are functional, not cosmetic.
- Re-run after every meaningful change. Polish regresses silently.

---

## 1. Visual hierarchy & layout

- [ ] The single most important element is the most prominent (size, weight, color, or position) — squint test: what do you see first?
- [ ] There is exactly **one** primary action per view; secondary actions are visually subordinate.
- [ ] Reading flow follows a clear path (Z or F pattern); the eye isn't ping-ponging.
- [ ] Related items are grouped by proximity; unrelated items are separated by whitespace.
- [ ] Content has a sensible max-width (prose ≤ ~75ch); text doesn't span the full width of a wide screen.
- [ ] The layout uses a consistent grid/column system, not arbitrary placement.
- [ ] Whitespace is intentional and generous — the design is not cramped; nothing fights for space.
- [ ] Visual density is appropriate to the content (dashboard ≠ marketing page).
- [ ] No accidental focal-point competition — two elements aren't both screaming "look at me."
- [ ] Above-the-fold communicates the page's purpose without scrolling.

## 2. Spacing & alignment

- [ ] All spacing comes from a defined scale (4/8px system) — no random `13px`, `27px` values.
- [ ] Spacing is consistent between like elements (every card has the same internal padding).
- [ ] Elements align to shared edges — left edges of stacked items line up to the pixel.
- [ ] Related items share a baseline or center line; nothing is off by 1–2px.
- [ ] Gutters between columns/cards are uniform.
- [ ] Padding is symmetric where expected (equal left/right inside buttons, cards, inputs).
- [ ] Vertical rhythm is consistent — gaps between sections follow the scale, not eyeballed.
- [ ] No double-margins collapsing or stacking into uneven gaps.
- [ ] Icons are optically aligned with adjacent text (centered on the cap height / x-height, not the box).
- [ ] Nested containers don't create cramped or doubled-up padding at edges.

## 3. Typography

- [ ] A clear type scale is used (limited set of sizes); sizes aren't arbitrary.
- [ ] Body text is **≥ 16px** (never smaller on mobile — triggers iOS zoom and hurts readability).
- [ ] Line height is comfortable: ~1.5–1.6 for body, tighter (1.1–1.3) for headings.
- [ ] Line length for prose is 45–75 characters (`max-width: ~65ch`).
- [ ] Heading hierarchy is visually and semantically correct (h1 > h2 > h3, no skipped levels).
- [ ] At most 2 typefaces; weights are used purposefully (not 6 random weights).
- [ ] Letter-spacing is tuned: slightly tighter on large display text, default/looser on small caps.
- [ ] No widows/orphans in headings; no single word stranded on its own line in key copy.
- [ ] Text wraps gracefully — long words/URLs use `overflow-wrap`, no overflow.
- [ ] Numerals align (tabular figures) in tables and data columns.
- [ ] No text in images (it can't be selected, translated, or scaled).
- [ ] Sufficient contrast between heading and body weight so hierarchy reads instantly.

## 4. Color & contrast

- [ ] Body text meets **4.5:1** contrast; large text and UI/graphics meet **3:1** (see [color.md](../02-foundations/color.md)).
- [ ] Color is never the *only* signal of meaning (errors, status, links have a second cue — icon, text, underline).
- [ ] The palette is cohesive — colors come from defined tokens, not ad-hoc hexes.
- [ ] One accent/brand color leads; it isn't diluted by competing accents.
- [ ] Semantic colors are consistent (same green = success everywhere, same red = error/destructive).
- [ ] Disabled states are visibly distinct but not invisible.
- [ ] Dark mode (if present) isn't just inverted — backgrounds are dark-gray not pure black, contrast still holds, shadows adjusted.
- [ ] Hover/active/focus colors are derived from the base, not random.
- [ ] No pure-black `#000` on pure-white `#fff` for large text blocks (harsh; use near-black/near-white).
- [ ] Gradients and overlays don't drop text below contrast minimums.

## 5. Components & states (empty / loading / error / success)

- [ ] **Empty state** designed: helpful illustration/message + a clear next action (not a blank void).
- [ ] **Loading state** present: skeleton or spinner; layout doesn't jump when content arrives.
- [ ] **Error state** designed: explains what went wrong in plain language + how to recover/retry.
- [ ] **Success/confirmation** feedback exists for completed actions (saved, sent, deleted).
- [ ] **Partial/zero-results** state for searches and filters ("No results for X. Try …").
- [ ] Every interactive component has **default, hover, focus, active, disabled** states.
- [ ] Buttons show a loading/pending state on async actions and are disabled to prevent double-submit.
- [ ] Forms show inline validation with clear, specific messages tied to the field.
- [ ] Long content truncates intentionally (ellipsis + tooltip/expand), not overflowing or clipping.
- [ ] Lists/tables handle 0, 1, many, and very many items without breaking.
- [ ] Destructive actions require confirmation or are undoable.
- [ ] Toasts/notifications are announced, dismissible, and don't cover critical UI.

## 6. Interaction & motion

- [ ] Every interactive element gives immediate feedback on press (< 100ms) — pressed/hover/active.
- [ ] Hover states exist on all clickable elements (desktop) and don't break on touch.
- [ ] Cursor is correct: `pointer` on clickable, `text` on editable, `not-allowed` on disabled.
- [ ] Transitions are quick (150–300ms) and use easing, not linear; nothing feels sluggish or abrupt.
- [ ] Animations use `transform`/`opacity` only (no janky `width`/`top` animation) — see [performance.md](./performance.md).
- [ ] Motion respects `prefers-reduced-motion`.
- [ ] No animation blocks the user from acting (can't interact until a 1s intro finishes = bad).
- [ ] Scrolling is smooth; no jank on scroll, drag, or type.
- [ ] Drag/swipe gestures (if any) have visible affordances and keyboard/click alternatives.
- [ ] Optimistic UI or instant feedback used where round-trips would otherwise stall the user.

## 7. Responsive

- [ ] No horizontal scroll at **320px** (smallest supported width).
- [ ] Tested across the matrix: 320 / 375 / 768 / 1024 / 1280 / 1920px (see [responsive.md](./responsive.md)).
- [ ] Layout **transforms** at breakpoints (sidebar→sheet, table→cards), doesn't just shrink.
- [ ] Touch targets are **≥ 44×44px** with adequate spacing on mobile.
- [ ] Primary actions sit in the thumb zone (bottom third) on mobile.
- [ ] Type scales fluidly and stays readable at every width.
- [ ] Images use `srcset`/`sizes` and never overflow their container (`max-width: 100%`).
- [ ] Safe-area insets respected on notched devices (`env()`), nothing clipped by notch/home indicator.
- [ ] Works in both portrait and landscape; orientation isn't locked.
- [ ] Pinch-zoom is not disabled; layout reflows at 200% browser zoom without clipping (1.4.10).
- [ ] Nothing important is hidden only behind hover (unavailable on touch).

## 8. Accessibility

- [ ] Semantic HTML throughout — real `<button>`, `<a>`, `<nav>`, `<main>`, `<h1>`; no div-soup controls (see [accessibility.md](./accessibility.md)).
- [ ] Full keyboard operation: Tab/Shift+Tab/Enter/Space/Arrows/Esc complete every flow; no traps.
- [ ] Visible **`:focus-visible`** ring on every interactive element; `outline: none` never used without replacement.
- [ ] Tab order matches visual order (DOM order correct; no positive `tabindex`).
- [ ] Modals/menus trap focus while open and **restore focus** to the trigger on close.
- [ ] Skip-to-content link present.
- [ ] Every form input has an associated `<label>`; placeholder is not used as the label.
- [ ] Errors are identified by text (not color alone), tied to the field via `aria-describedby`, with `aria-invalid`.
- [ ] All images have appropriate `alt` (descriptive for meaningful, `alt=""` for decorative).
- [ ] Icon-only buttons have an accessible name (`aria-label` or visually-hidden text).
- [ ] Dynamic updates (toasts, async results) announced via pre-rendered `aria-live` regions.
- [ ] Interactive ARIA widgets expose correct name, role, state (4.1.2) and standard keyboard patterns.
- [ ] No content flashes more than 3×/second.
- [ ] Page has one `h1`, logical heading structure, and labeled landmarks.
- [ ] SPA route changes move focus and update `document.title`.

## 9. Performance

- [ ] LCP < 2.5s, INP < 200ms, CLS < 0.1 (verified, not assumed) — see [performance.md](./performance.md).
- [ ] No layout shift as images/fonts/async content load (dimensions reserved).
- [ ] Hero/LCP image is preloaded with `fetchpriority="high"` and not lazy-loaded.
- [ ] Below-the-fold images are lazy-loaded; all use modern formats (AVIF/WebP) and are compressed.
- [ ] Fonts are woff2, subset, self-hosted, preloaded, `font-display: swap` with matched fallback metrics.
- [ ] JS is code-split by route; heavy components lazy-loaded; non-critical scripts deferred.
- [ ] Long lists are virtualized; expensive handlers are debounced/throttled.
- [ ] Initial JS budget respected (< ~170KB compressed).
- [ ] Tested on a throttled mid-tier mobile, not just the dev machine.
- [ ] First meaningful content appears fast; perceived performance handled with skeletons/optimistic UI.

## 10. Content & microcopy

- [ ] Copy is clear, concise, and jargon-free; written for the user, not the system.
- [ ] Button labels describe the action ("Save changes", not "Submit" / "OK").
- [ ] Error messages are specific and actionable ("Email is already in use — try signing in"), never "An error occurred".
- [ ] Empty states guide the user toward a first action.
- [ ] Tone is consistent across the product (formal/casual, person, capitalization).
- [ ] No Lorem Ipsum, placeholder text, or "TODO" left in the shipped UI.
- [ ] Dates, numbers, currencies are formatted and localized correctly.
- [ ] Capitalization style is consistent (sentence case vs title case — pick one and hold it).
- [ ] Truncated/abbreviated text has a way to see the full value.
- [ ] Pluralization is correct ("1 item" / "2 items", not "1 items").
- [ ] No spelling or grammar errors (actually proofread the rendered text).

## 11. Polish details (the small things)

- [ ] Border radii are consistent across siblings (cards, buttons, inputs share the radius scale).
- [ ] Border widths and colors are uniform; no 1px here / 2px there by accident.
- [ ] Shadows are consistent and physically plausible (same light source; elevation maps to a scale).
- [ ] Icons share one style, weight, and grid size; no mixing of outline + filled randomly.
- [ ] Optical alignment applied where mathematical centering looks off (play icons, arrows nudged).
- [ ] Hover states present on **every** interactive element — none feel "dead."
- [ ] Focus rings present and attractive, not an afterthought.
- [ ] No orphaned text, stray single words, or awkward line breaks in headings.
- [ ] Consistent use of dividers/borders vs whitespace — not both fighting to separate the same things.
- [ ] Alignment of mixed-size elements is optical, not just box-based.
- [ ] Interactive elements have generous, consistent hit areas.
- [ ] Loading shimmer/skeletons match the real content's shape.
- [ ] Corner cases look intentional: very long names, missing avatars, huge numbers, empty fields.
- [ ] Z-index/layering is correct — dropdowns, tooltips, modals stack above content, nothing clipped.
- [ ] No console errors or warnings in the browser.

---

## Red flags that scream amateur

If you spot any of these, stop and fix before shipping — they're instant tells:

- Default browser styles untouched (Times New Roman, default blue links, native focus glow only).
- Inconsistent spacing — gaps of 12, 13, 16, 19px with no system.
- `outline: none` with no focus replacement (keyboard users blinded).
- Pure-black text on pure-white, harsh shadows (`0 0 10px #000`), or muddy gray-on-gray text.
- One giant breakpoint where everything just shrinks; broken layout between breakpoints.
- Placeholder used as the label; inputs with no labels.
- No empty/loading/error states — only the happy path designed.
- Buttons that don't change on hover/press; "dead" clickable elements.
- Layout that jumps as images and fonts load (CLS).
- Mixed icon styles, mismatched border radii, off-by-a-few-px alignment everywhere.
- Lorem Ipsum, "Button", "Title here", or TODO text in the rendered UI.
- Center-aligned long paragraphs; text spanning 1500px line length.
- Tiny 11–13px body text; touch targets you can barely tap.
- Generic copy: "Submit", "Error occurred", "Something went wrong" with no recovery path.

## Signs of craft

The details that separate world-class from "fine" — aim for these:

- Spacing follows one scale so rigorously the whole UI feels engineered.
- A clear, calm hierarchy: you always know where to look and what to do next.
- Every state is designed — empty, loading, error, success, zero-results, very-long-content.
- Focus rings are intentional and beautiful; keyboard nav is a first-class path.
- Motion is subtle, fast, and purposeful; it guides attention and never blocks.
- Optical adjustments: icons nudged, text vertically centered on cap height, alignment that *looks* right.
- Microcopy that sounds human and helps the user recover from mistakes.
- Responsive layouts that *rethink* themselves per device rather than squishing.
- Consistent radii, shadows, borders, and icon style — a coherent visual system.
- It's fast and *feels* fast: instant feedback, skeletons, no layout shift, smooth scroll.
- Edge cases handled gracefully — long names, missing data, huge numbers all look deliberate.
- Accessible by construction: works with a screen reader and keyboard without special effort.

---

## Agent checklist

- [ ] Run all 11 sections against the **rendered UI**, top to bottom — not from memory.
- [ ] Treat **Accessibility** and **Components & states** as functional gates; unchecked = not done.
- [ ] Do the **squint test** — confirm one clear focal point and a single primary action per view.
- [ ] Verify every interactive element has **hover, focus, active, and disabled** states.
- [ ] Confirm **empty, loading, error, success** states exist for every async surface.
- [ ] Check **320px → 1920px** for horizontal scroll, transforms, and touch-target sizing.
- [ ] Tab through the whole flow by **keyboard**; confirm visible focus and no traps.
- [ ] Confirm **CLS, LCP, INP** are within target and nothing jumps on load.
- [ ] Proofread rendered **copy**; kill placeholder text, fix labels and error messages.
- [ ] Sweep the **polish list** — radii, shadows, icons, alignment, console errors.
- [ ] Scan for **red flags**; if any are present, fix before declaring done.
- [ ] Re-run this checklist after the final change — polish regresses silently.
