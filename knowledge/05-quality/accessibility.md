# Accessibility

> Purpose: Ship interfaces that work for keyboard, screen reader, low-vision, motor-impaired, and cognitively diverse users — WCAG 2.2 AA, applied as concrete code, not theory.

**When to read this:** Before writing any interactive markup, before declaring a UI done, and during every review. Accessibility is not a final pass — it is structural. Pair with [color.md](../02-foundations/color.md) for contrast math and [review-checklist.md](./review-checklist.md) for the full QA gate.

---

## The 30-second model

Accessibility = give every user the same information and operations through a different channel. Four obligations (WCAG's POUR):

- **Perceivable** — content reaches the user (text alternatives, contrast, captions).
- **Operable** — every action works by keyboard, touch, and voice, not just mouse.
- **Understandable** — predictable, labeled, error-recoverable.
- **Robust** — correct semantics so assistive tech (AT) parses it.

You target **WCAG 2.2 Level AA**. That is the legal and professional baseline (ADA, EN 301 549, Section 508 all map to it).

> Automated tools (axe, Lighthouse) catch ~30% of issues. They cannot judge reading order, focus management, ARIA correctness, or alt-text quality. The other 70% is manual. A green Lighthouse score is necessary, never sufficient.

---

## Rule 0: Semantic HTML first — div soup is the #1 failure

The single most common, most damaging mistake agents make is rebuilding native elements out of `<div>` and `<span>`. Native elements come with **role, keyboard behavior, focusability, and state** for free. A `<div>` has none of it.

```html
<!-- DON'T: invisible to AT, not focusable, no keyboard, no Enter/Space -->
<div class="btn" onclick="submit()">Submit</div>

<!-- DO: focusable, Enter/Space fire it, announced as "Submit, button" -->
<button type="button" onclick="submit()">Submit</button>
```

| Need | Use this | Not this |
|------|----------|----------|
| Clickable action | `<button>` | `<div onclick>` |
| Navigation to a URL | `<a href>` | `<span onclick>` |
| Page regions | `<header> <nav> <main> <footer>` | `<div class="nav">` |
| Heading | `<h1>`–`<h6>` | `<div class="title">` |
| List | `<ul>/<ol>/<li>` | stack of `<div>` |
| Form control | `<input> <select> <textarea>` | styled `<div contenteditable>` |
| Toggle on/off | `<input type="checkbox">` | `<div role="switch">` w/ manual JS |
| Expandable section | `<details><summary>` | `<div>` + JS |
| Table data | `<table><th scope><td>` | grid of `<div>` |

**Decision rule:** if a native element exists for what you're building, use it. You only reach for ARIA when no native element fits (e.g. tabs, comboboxes, tree views). See ARIA rules below.

**Button vs link:** `<a>` goes somewhere (changes URL/location). `<button>` does something (mutates state, opens, submits). Never `<a href="#">` with a JS handler — that's a button wearing a link costume.

---

## Headings & landmarks: the screen reader's table of contents

Screen reader users navigate by pulling up a list of headings (VoiceOver: `VO+U`; NVDA: `H`) and landmarks. Broken structure = lost users.

- Exactly **one `<h1>`** per page (or per view in an SPA) — the page's subject.
- Never skip levels for styling. `<h2>` → `<h4>` is a violation (1.3.1 Info and Relationships). Style separately from level.
- Landmarks: `<header>` (banner), `<nav>`, `<main>` (exactly one), `<aside>` (complementary), `<footer>` (contentinfo). Label repeated landmarks: `<nav aria-label="Primary">` and `<nav aria-label="Pagination">`.
- Provide a **skip link** as the first focusable element (2.4.1 Bypass Blocks):

```html
<a href="#main" class="skip-link">Skip to main content</a>
<!-- visually hidden until :focus -->
<main id="main" tabindex="-1">…</main>
```

```css
.skip-link { position:absolute; left:-9999px; }
.skip-link:focus { left:1rem; top:1rem; z-index:999; /* visible on focus */ }
```

---

## Keyboard navigation: everything operable, no mouse

WCAG 2.1.1 (Keyboard) and 2.1.2 (No Keyboard Trap) are non-negotiable. Test every flow with **Tab / Shift+Tab / Enter / Space / Arrows / Esc only**. If you can't complete it, it's broken.

- **Tab order follows visual order.** Don't use positive `tabindex` (`tabindex="3"`) — it hijacks order and creates chaos. Only `tabindex="0"` (add to tab order) and `tabindex="-1"` (focusable by script, not Tab) are legitimate.
- **DOM order = reading order = tab order.** If CSS (flex `order`, grid placement, absolute positioning) makes the visual order differ from DOM order, you've created a focus-order bug (2.4.3). Fix the DOM, not with `tabindex`.
- **No traps.** Focus must always be able to leave a component via the keyboard (the one exception is a modal, which traps *intentionally* and releases on Esc/close).
- Standard keys by widget: buttons/links = Enter (links) and Enter+Space (buttons); checkboxes/radios = Space + arrows; menus/tabs/listboxes = Arrow keys + Home/End; dialogs = Esc to close.

### Visible focus — never remove the outline without replacing it

Removing focus styling blinds keyboard users (2.4.7 Focus Visible). The cardinal sin:

```css
/* DON'T — this is an accessibility crime, full stop */
*:focus { outline: none; }
```

```css
/* DO — use :focus-visible so the ring shows for keyboard, not mouse clicks */
:focus-visible {
  outline: 2px solid var(--focus-ring, #2563eb);
  outline-offset: 2px;
  border-radius: inherit;
}
/* Optional: suppress ring on pointer focus only */
:focus:not(:focus-visible) { outline: none; }
```

WCAG 2.2 adds **2.4.11 Focus Not Obscured** — the focused element must not be hidden behind sticky headers/footers. Use `scroll-margin` and check that fixed bars don't cover the focus ring.

The focus indicator must meet **3:1 contrast** against adjacent colors and be at least 2px thick (2.4.13 Focus Appearance, AAA, but treat as the target).

---

## Focus management: modals, menus, dynamic content

Native HTML handles focus for you. The moment you build custom widgets or move content with JS, **you** own focus. The three rules:

1. **On open** — move focus into the new context (first focusable element, or the dialog container).
2. **While open (modal)** — trap focus inside (Tab cycles within; Esc closes).
3. **On close** — return focus to the element that triggered it. Losing focus to `<body>` strands the user.

```js
// Modal pattern (or just use <dialog>.showModal() which does most of this)
function openModal(trigger, dialog) {
  lastFocused = trigger;                       // remember where we came from
  dialog.showModal();                          // native <dialog> traps + Esc-closes
  dialog.querySelector('[autofocus], button, [href], input')?.focus();
}
function closeModal(dialog) {
  dialog.close();
  lastFocused?.focus();                        // restore — critical
}
```

```html
<dialog aria-labelledby="dlg-title" aria-modal="true">
  <h2 id="dlg-title">Delete project?</h2>
  …
</dialog>
```

**Prefer native `<dialog>`** — it gives you the top layer, backdrop, Esc-to-close, and inert background. If you hand-roll a modal, you must add `aria-modal="true"`, trap focus, mark the rest of the page `inert`, and handle Esc yourself.

**SPA route changes:** a route change is silent to screen readers. On navigation, move focus to the new `<h1>` (or a focusable `<main tabindex="-1">`) and update `document.title`, or announce via a live region. Otherwise users have no idea the page changed.

---

## Color & contrast

Full theory and OKLCH math live in [color.md](../02-foundations/color.md). The hard requirements:

| Content | Ratio | WCAG SC |
|---------|-------|---------|
| Body text (< 18.66px bold / < 24px regular) | **4.5:1** | 1.4.3 |
| Large text (≥ 24px, or ≥ 18.66px bold) | **3:1** | 1.4.3 |
| UI components & graphical objects (borders, icons, focus rings, chart bars) | **3:1** | 1.4.11 |
| AAA body text (aim higher where you can) | 7:1 | 1.4.6 |

- **Never encode meaning in color alone** (1.4.1). Error states need an icon or text, not just red. Links in body text need underline or a non-color cue, not just a different hue.
- **Placeholder text is not a label** and routinely fails 4.5:1 — never rely on it (also fails 3.3.2).
- Test contrast against the **actual rendered background**, including overlays and gradients on images.
- Support **`forced-colors` / Windows High Contrast**: don't kill system colors; use `forced-colors: active` media query to fix icon visibility and never set backgrounds that hide text.

---

## ARIA: the rules of engagement

**First Rule of ARIA: don't use ARIA.** If a native element or attribute gives you the semantics, use it. Bad ARIA is worse than no ARIA — it actively lies to AT.

The five rules, distilled:
1. Prefer native HTML over `role`.
2. Don't change native semantics (`<button role="heading">` — never).
3. All interactive ARIA widgets must be keyboard operable.
4. Don't put `aria-hidden="true"` on a focusable element — it creates a "phantom" focus stop AT can't describe.
5. Every interactive element needs an accessible name.

### ARIA quick-reference table

| Attribute / role | What it does | Use when | Watch out |
|------------------|--------------|----------|-----------|
| `aria-label="…"` | Sets accessible name from a string | Icon-only buttons, unlabeled controls | Overrides visible text — don't mismatch (2.5.3) |
| `aria-labelledby="id"` | Name from another element's text | Reuse a visible heading as the label | Wins over `aria-label`; id must exist |
| `aria-describedby="id"` | Extra description (hints, errors) | Field help text, error messages | Announced after the name |
| `aria-hidden="true"` | Removes from accessibility tree | Decorative icons, duplicated content | Never on focusable/interactive elements |
| `aria-expanded="true/false"` | Collapsed/expanded state | Accordions, disclosure, menu/combobox triggers | Must update on toggle |
| `aria-current="page/step/true"` | Marks the current item in a set | Active nav link, current step, current date | Use `page` for nav |
| `aria-live="polite/assertive"` | Announce dynamic changes | Toasts, status, async results | `assertive` interrupts — reserve for errors |
| `aria-pressed="true/false"` | Toggle button state | A `<button>` that toggles | Don't combine with `role="checkbox"` |
| `aria-selected="true/false"` | Selected state in a widget | Tabs, listbox options, grid cells | For composite widgets, not buttons |
| `aria-disabled="true"` | Disabled but still discoverable | When you need it focusable/announced | `disabled` attr removes from tab order; `aria-disabled` doesn't |
| `aria-invalid="true"` | Field has an error | Failed validation | Pair with `aria-describedby` → error text |
| `aria-controls="id"` | Element controls another | Tabs → panels, trigger → region | Inconsistent AT support; pair with `aria-expanded` |
| `role="dialog"` / `aria-modal` | Modal semantics | Hand-rolled dialogs | Prefer native `<dialog>` |
| `role="alert"` | Implicit assertive live region | Critical, time-sensitive messages | Announces immediately on insert |
| `role="status"` | Implicit polite live region | Non-urgent status updates | Won't interrupt |

### Live regions (the part agents get wrong)

A live region announces content that changes **without** moving focus (search results updated, "Saved", "3 items added"). The region must **exist in the DOM before** you inject text — AT only watches pre-existing live regions.

```html
<!-- Render this empty on load; update its textContent later -->
<div aria-live="polite" aria-atomic="true" class="sr-only" id="status"></div>
```

```js
document.getElementById('status').textContent = 'Settings saved'; // announced
```

Use `polite` for almost everything; `assertive`/`role="alert"` only for errors that must interrupt. Don't pile multiple assertive regions — they fight.

---

## Forms: where accessibility is won or lost

- **Every input has a programmatic label.** Visible `<label for>` is best. Placeholder ≠ label.

```html
<!-- DO -->
<label for="email">Email address</label>
<input id="email" type="email" name="email" autocomplete="email"
       aria-describedby="email-hint" required>
<p id="email-hint">We'll never share it.</p>
```

- **Group related controls** with `<fieldset>` + `<legend>` (radio groups, address blocks).
- **Required & errors:** mark `required`; on failure set `aria-invalid="true"` and point `aria-describedby` at the error text. Errors must be **text** (3.3.1), identify the field, and suggest a fix (3.3.3).
- **Move focus to the first error** (or a summary) on failed submit; don't just color fields red.
- **`autocomplete` tokens** (2.5.5 / 1.3.5) let browsers and AT autofill — `autocomplete="name email tel street-address"`.
- **Don't disable the submit button** as the only validation feedback — screen reader users can't tell why; explain what's missing instead.
- WCAG 2.2: **3.3.7 Redundant Entry** (don't re-ask for info already given) and **3.3.8 Accessible Authentication** (no cognitive puzzles like "type the 3rd character of your password"; allow paste into OTP/password fields).

---

## Images & alt text: decorative vs meaningful

`alt` is required on every `<img>`. The question is what goes in it.

| Image type | alt value | Rationale |
|------------|-----------|-----------|
| Meaningful (conveys info) | Describe the info/function | "Bar chart: Q3 revenue up 40%" |
| Functional (link/button) | Describe the **action**, not the picture | `alt="Search"` on a magnifier icon |
| Decorative (purely visual) | `alt=""` (empty, not missing) | Removes it from AT — correct |
| Redundant (caption already says it) | `alt=""` | Avoid double announcement |
| Complex (chart/diagram) | Short alt + long description nearby | `aria-describedby` or visible text |

```html
<img src="trend.svg" alt="Signups doubled month over month, Jan to Jun">
<img src="divider.svg" alt="">                <!-- decorative -->
<button><img src="trash.svg" alt="Delete"></button>
```

- Inline **SVG**: add `role="img"` + `<title>`, or `aria-label`; mark purely decorative SVG `aria-hidden="true"`.
- **Icon fonts / background images** carrying meaning need an accessible name via `aria-label` on the element or visually-hidden text.
- Don't start alt with "image of" — AT already says "graphic".

---

## Motion & animation

- Honor **`prefers-reduced-motion`** (2.3.3). Vestibular disorders make parallax, big slides, and zoom-ins nauseating.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

  Keep *essential* feedback (e.g. a subtle fade) but kill motion-heavy effects. Replace slides with fades; disable autoplay/parallax.
- **No flashing > 3×/second** (2.3.1 — seizure risk). Hard rule.
- **Auto-playing content > 5s** (carousels, video) needs a pause/stop/hide control (2.2.2).
- Animation drives attention but never *only* conveys state — pair with a static cue.

---

## Target size (WCAG 2.2)

- **2.5.8 Target Size (Minimum, AA): 24×24 CSS px** minimum for any pointer target, OR sufficient spacing so a 24px circle around it doesn't overlap neighbors.
- **2.5.5 (AAA) and platform guidance recommend 44×44** (Apple HIG) / 48×48 (Material). **Treat 44×44 as the real target** for primary touch actions; 24×24 is the floor, not the goal.
- Pad small icons to hit-area size with padding or a pseudo-element, even if the glyph is tiny.
- Adjacent tap targets need spacing so fat fingers don't mis-hit; inline text links are exempt from 2.5.8.

---

## Screen reader testing basics

You cannot certify accessibility without listening to it. Minimum viable test:

| SR | Platform / Browser | Turn on | Core keys |
|----|--------------------|---------|-----------|
| **VoiceOver** | macOS + Safari | `Cmd+F5` | `VO = Ctrl+Opt`; `VO+→` next; `VO+U` rotor; `VO+Space` activate |
| **VoiceOver** | iOS + Safari | Settings → Accessibility | Swipe right = next; double-tap activate |
| **NVDA** | Windows + Firefox/Chrome | free download | `H` next heading; `Tab` controls; `Ins+F7` elements list |
| **TalkBack** | Android + Chrome | Settings → Accessibility | Swipe right = next |

What to verify on each interactive element: **Name** (does it announce a useful label?), **Role** (button? link? checkbox?), **State** (expanded/selected/checked?), **Value** where relevant. This is WCAG **4.1.2 Name, Role, Value** — the most-failed criterion. Walk the full task (e.g. add to cart → checkout) eyes-closed.

---

## The most common violations agents produce — and the fix

| Violation | Why it's wrong | Fix |
|-----------|----------------|-----|
| `<div onclick>` as a button | No role, focus, or keyboard | Use `<button>` |
| `outline: none` globally | Blinds keyboard users (2.4.7) | `:focus-visible` ring |
| Icon-only button, no label | Announced as bare "button" (4.1.2) | `aria-label` or visually-hidden text |
| Placeholder used as the label | Disappears on type; fails 4.5:1 & 3.3.2 | Real `<label for>` |
| Skipped heading levels | Breaks 1.3.1 structure | Sequential `h1→h2→h3`; style separately |
| Modal doesn't trap/return focus | Strands keyboard users | Native `<dialog>` or manual trap + restore |
| Low-contrast gray text | `#999 on #fff` = 2.8:1, fails 1.4.3 | ≥ 4.5:1; darken text |
| `alt` missing or "image123.png" | No/garbage alt (1.1.1) | Describe info, or `alt=""` if decorative |
| Color-only error state | Fails 1.4.1 | Add icon + text |
| `aria-label` on a `<div>` with no role | Ignored — not interactive | Put it on the real control |
| `aria-hidden="true"` on focusable | Phantom focus stop | Remove, or also remove from tab order |
| SPA nav doesn't move focus/title | Silent page change | Focus `<h1>`, update `document.title` |
| Toast with no live region | Never announced | Pre-rendered `aria-live` region |
| Positive `tabindex` | Hijacks tab order (2.4.3) | DOM order + `tabindex="0/-1"` only |

---

## Ship-blocking a11y failures (do not release)

These are not "nice to have." Any one present = **not done**:

1. Any interactive control unreachable or unusable by keyboard (2.1.1).
2. A keyboard trap with no escape (2.1.2).
3. `outline: none` with no visible focus replacement (2.4.7).
4. Form input with no programmatic label (1.3.1 / 4.1.2).
5. Body text below 4.5:1 contrast, or UI/graphics below 3:1 (1.4.3 / 1.4.11).
6. Image conveying info with missing/junk alt (1.1.1).
7. Modal/menu that doesn't trap and restore focus.
8. Error identified by color alone, or with no text explanation (1.4.1 / 3.3.1).
9. Content that flashes more than 3×/second (2.3.1).
10. Touch targets below 24×24 px with no spacing exception (2.5.8).
11. Custom widget (tabs/combobox/menu) missing required ARIA roles/states/keys (4.1.2).
12. Animation that ignores `prefers-reduced-motion` for non-essential motion (2.3.3).

---

## Agent checklist

- [ ] Reach for a **native element** before any `<div>`/ARIA; reserve ARIA for widgets HTML can't express.
- [ ] Walk every flow with **keyboard only** — Tab/Shift+Tab/Enter/Space/Arrows/Esc — and confirm nothing is unreachable or trapped.
- [ ] Provide a **visible `:focus-visible` ring** everywhere and never ship `outline: none` without a replacement.
- [ ] For modals/menus/dynamic content: move focus in, trap if modal, **restore focus** to the trigger on close.
- [ ] Give **every** interactive element an accessible **name, role, and state**; label every form input with `<label for>`.
- [ ] Verify **contrast**: 4.5:1 text, 3:1 large text + UI/graphics; never signal state by color alone.
- [ ] Write correct **alt** — descriptive for meaningful images, `alt=""` for decorative.
- [ ] Honor **`prefers-reduced-motion`**, ban >3 flashes/sec, and add pause controls to auto-playing content.
- [ ] Size touch targets to **44×44** (24×24 absolute floor with spacing).
- [ ] Pre-render **`aria-live`** regions for toasts/status; announce SPA route changes and move focus.
- [ ] Run **axe/Lighthouse** for the easy 30%, then **listen** with VoiceOver or NVDA for the other 70%.
- [ ] Re-check the **ship-blocking list** above before declaring done — any single hit means not done.
