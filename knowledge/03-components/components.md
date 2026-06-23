# Core Components

> Purpose: Build or select the core UI primitives correctly — exact anatomy, sizing, every interactive state, and the mistakes that ship most often.

**When to read this:** Before implementing any button, input, select, modal, tooltip, tab, toast, menu, or pagination — or when reviewing a component for state/sizing/a11y completeness.

This is the definitive primitive reference. For form-level composition (validation timing, error wording, layout) see [./forms.md](./forms.md). For nav primitives see [./navigation.md](./navigation.md). For tables/charts/lists see [./data-display.md](./data-display.md). For contrast/focus/keyboard rules see [../05-quality/accessibility.md](../05-quality/accessibility.md). For tokens (color/spacing/radius) see [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md).

---

## The component states matrix (every interactive component MUST satisfy this)

Every interactive component must define and visually distinguish all of these. If a state is missing, the component is incomplete.

| State | Trigger | Required treatment | Common failure |
|---|---|---|---|
| `default` | Resting | Baseline style | — |
| `hover` | Pointer over | Subtle bg/border/elevation shift (never the only signal) | Hover-only affordance on touch devices |
| `active`/`pressed` | Mouse down / `:active` | Visible depression: darker bg, scale 0.98, or inset | No press feedback → feels dead |
| `focus-visible` | Keyboard focus | 2px ring, ≥3:1 contrast vs adjacent, `:focus-visible` only | Removing outline with `outline:none` and no replacement |
| `disabled` | Not actionable | ~40% opacity OR muted token, `cursor:not-allowed`, `aria-disabled` | `disabled` with no tooltip explaining why |
| `loading` | Async in flight | Spinner replaces label OR inline spinner; keep width stable; `aria-busy` | Layout shift; double-submit allowed |
| `error`/`invalid` | Validation fail | Red border + icon + message + `aria-invalid` | Color-only error (fails colorblind users) |
| `selected`/`checked`/`current` | Chosen | Filled/checked + `aria-selected`/`aria-current`/`aria-checked` | Selected state indistinguishable from hover |
| `read-only` | Viewable, not editable | Muted but full-contrast text, no edit affordance | Confused with disabled (read-only must be readable) |

Rules:
- **Never rely on color alone** for any state (use icon, weight, border, position too).
- **focus-visible** uses `:focus-visible`, not `:focus`, so mouse clicks don't show rings but keyboard does.
- **Maintain dimensions** across states — loading/error must not shift layout.
- **Hover is an enhancement**, never a requirement; the component must work without it (touch, keyboard).

---

## Buttons

Anatomy: `[ optional leading icon ][ label ][ optional trailing icon ]` inside a padded, radius-rounded box with a hit area ≥44×44px.

### Variants (semantic hierarchy — at most ONE primary per view/section)

| Variant | Use for | Visual |
|---|---|---|
| `primary` | The single most important action on screen | Solid filled, brand/accent bg, high contrast label |
| `secondary` | Important but not the main action | Outlined or tonal (subtle filled), neutral |
| `tertiary` | Low-emphasis supporting action | Text + subtle bg on hover |
| `ghost` | Toolbar/icon actions, minimal chrome | Transparent bg, bg appears on hover |
| `destructive` | Delete/remove/irreversible | Solid red (or red outline for secondary-destructive); often needs confirmation |
| `link` | Inline navigation styled as text | Underlined or colored text, no box |

### Sizes (height drives everything)

| Size | Height | Padding-x | Font | Icon |
|---|---|---|---|---|
| `sm` | 32px | 12px | 13–14px | 16px |
| `md` (default) | 40px | 16px | 14–16px | 18–20px |
| `lg` | 48px | 20–24px | 16px | 20–24px |

- **Minimum hit target 44×44px** even if visual height is 32px — pad the clickable area or add invisible padding. Critical on touch. See [../05-quality/accessibility.md](../05-quality/accessibility.md).
- Icon-only buttons: square (e.g. 40×40), `aria-label` required, tooltip recommended.

### States
- `default → hover`: darken/lighten bg ~8–10%.
- `active`: darken ~12–16% or `scale(0.98)`.
- `focus-visible`: 2px ring offset 2px.
- `disabled`: reduce opacity, remove hover, `cursor:not-allowed`.
- `loading`: swap label for spinner OR prefix spinner; **disable** to prevent double-submit; keep button width fixed (reserve label width).

### Loading pattern (React + Tailwind)
```tsx
<button
  className="inline-flex h-10 min-w-[7rem] items-center justify-center gap-2 rounded-md bg-blue-600 px-4 text-sm font-medium text-white hover:bg-blue-700 active:bg-blue-800 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed"
  disabled={isLoading}
  aria-busy={isLoading}
>
  {isLoading ? <Spinner className="h-4 w-4 animate-spin" aria-hidden /> : <SaveIcon className="h-4 w-4" aria-hidden />}
  {isLoading ? "Saving…" : "Save"}
</button>
```

### Icon placement
- **Leading icon** = describes the action (save, download, add). Default choice.
- **Trailing icon** = direction/disclosure (→, chevron, external-link ↗).
- Gap between icon and label: 6–8px. Don't use both leading + trailing unless meaningful.

### Top 3 mistakes
1. **Multiple primary buttons** competing — destroys hierarchy. One primary per context.
2. **Button width collapses during loading** — reserve width or use `min-w`.
3. **Disabled without explanation** — user can't tell why; prefer enabling + showing inline error on click, or add a tooltip.

---

## Inputs / text fields

Anatomy: `[ label (top) ][ optional help text ][ field: leading-icon? value placeholder? trailing-icon? ][ error/success message ]`.

| Property | Value |
|---|---|
| Height | 40–44px (md), 36px (sm), 48px (lg) |
| Padding-x | 12px (add 36–40px if icon present) |
| Border | 1px neutral; 2px or color on focus |
| Radius | 6–8px |
| Font size | **≥16px on mobile** to prevent iOS auto-zoom |
| Label | Top-aligned, 14px, 4–6px gap above field |

### States
- `default`: 1px neutral border.
- `hover`: border darkens slightly.
- `focus`: 2px accent border + ring; remove the OS glow only if you replace it.
- `filled`: same as default (don't restyle just because it has a value).
- `disabled`: muted bg + muted text, `cursor:not-allowed`.
- `read-only`: full-contrast text, no border emphasis, not editable.
- `error`: red 1.5–2px border + error icon + message + `aria-invalid="true"` + `aria-describedby` pointing to the message.
- `success` (use sparingly): green border + check, only when confirmation adds value (e.g. username available).

### Top 3 mistakes
1. **Placeholder as label** — disappears on input, fails a11y, hurts recall. Always use a visible top label. See [./forms.md](./forms.md).
2. **<16px font on mobile** triggers zoom-on-focus.
3. **Error styling without `aria-invalid`/`aria-describedby`** — screen readers miss it.

---

## Selects & comboboxes

- **Native `<select>`** for ≤ ~12 simple options, no search: best a11y, mobile-friendly, free keyboard support. Prefer it.
- **Custom combobox** only when you need search/filter, multi-select, async loading, or rich option rendering. Then you OWN the a11y: `role="combobox"`, `aria-expanded`, `aria-controls`, `aria-activedescendant`, full arrow-key navigation, Enter/Escape, type-ahead.

Sizing matches inputs (40–44px). Listbox: max-height ~ 8–10 rows then scroll; highlight active option (≥3:1 contrast, not color-only).

Top 3 mistakes:
1. Building a custom select when native would do — losing keyboard/mobile/a11y.
2. Custom dropdown with no keyboard support (arrows/Enter/Escape).
3. No empty/"no results" state in a searchable combobox.

---

## Checkboxes, radios, switches

| Control | Use when | Selection |
|---|---|---|
| Checkbox | Multiple independent choices, or single opt-in (terms) | 0..n |
| Radio | One choice from a small mutually-exclusive set (2–6, all visible) | exactly 1 |
| Switch | Instant binary on/off **that takes effect immediately** (no save) | on/off |

- Control box: 16–20px; **hit target ≥44×44px** including the label (label is clickable via `<label for>` or wrapping).
- Switch ≠ checkbox: switch = immediate effect; checkbox = staged, applied on submit. Don't use a switch inside a form that requires "Save".
- States: default/hover/checked/focus-visible/disabled/indeterminate (checkbox `aria-checked="mixed"` for parent-of-group).

Top 3 mistakes:
1. Radios for >6 options or for non-exclusive choices (use select / checkboxes).
2. Tiny click target — only the 16px box is clickable, not the label.
3. Switch used where a Save button applies the change (ambiguous — did it save?).

---

## Cards

Anatomy: `[ media? ][ header/title ][ body ][ footer/actions ]` in a bordered or elevated container, radius 8–12px, padding 16–24px.

- Use border **or** subtle shadow — not heavy both. Elevation should map to interactivity (raised on hover only if clickable).
- Whole-card-clickable: make the title an `<a>` and stretch it (`::after` overlay) so the link target is semantic; keep nested buttons accessible (don't nest interactive in interactive).
- Consistent internal spacing and image aspect ratios across a grid.

Top 3 mistakes:
1. Entire `<div>` with `onClick` — not keyboard-focusable, not a real link.
2. Inconsistent padding/heights making a grid look broken.
3. Nesting buttons/links inside a card that is itself a link (invalid, ambiguous).

---

## Modals / dialogs

Anatomy: `[ overlay/scrim ][ dialog: header + close-X | body (scrollable) | footer actions ]`. Centered or sheet; max-width ~ 480–640px for forms.

Mandatory behaviors:
- **Focus trap**: focus moves into the dialog on open, cycles within it, returns to the trigger on close.
- **Escape closes** (unless destructive-unsaved — then confirm).
- **Scroll lock** the background (`overflow:hidden` on body; preserve scrollbar width to avoid shift).
- **Overlay click** closes for non-destructive; for forms with data, confirm or ignore overlay click.
- **`role="dialog"` + `aria-modal="true"` + `aria-labelledby`** (title) and `aria-describedby` (body) if relevant.
- Inert the background (`inert` attr or `aria-hidden` on siblings) so SR/keyboard can't reach it.
- Initial focus: first interactive element, or the dialog container — **not** a destructive button.

Use `<dialog>` element or a vetted primitive (Radix Dialog) rather than hand-rolling — it handles trap/inert/escape.

Top 3 mistakes:
1. **No focus trap / focus return** — keyboard users get lost behind the modal.
2. **Background scrolls** or shifts when modal opens.
3. **Escape doesn't close** or overlay isn't dismissible (with no other escape).

---

## Tooltips

- For **supplementary** info only — never put essential info or actions in a tooltip.
- Trigger: hover AND keyboard focus (focusable trigger required). Delay ~300–500ms in, instant out.
- Position near trigger, flip to stay in viewport, small arrow pointing to trigger.
- `role="tooltip"`, linked via `aria-describedby`. Not focusable itself; no interactive content inside (use a popover for that).
- Touch has no hover — provide an alternative (tap to reveal, or just show the info inline).

Top 3 mistakes:
1. Putting links/buttons in a tooltip (use a popover).
2. Tooltip on a non-focusable element → keyboard users never see it.
3. Critical info only in a tooltip → invisible on touch/SR.

---

## Badges / tags / chips

- **Badge**: small status/count label, non-interactive (e.g. "New", "3", "Beta"). Height ~18–24px, font 11–12px, padding 4–8px.
- **Tag/chip**: categorization; may be removable (×) or selectable (filter chip). Removable chip's × needs `aria-label="Remove {name}"` and ≥24px hit area.
- Status colors carry meaning → also include text/icon (not color-only). Map: green=success, amber=warning, red=error/critical, blue=info, gray=neutral/default.

Top 3 mistakes:
1. Color-only status badges (colorblind-inaccessible).
2. Removable chip × too small to tap.
3. Badge text so long it breaks layout — truncate or cap counts ("99+").

---

## Avatars

- Sizes: 24/32/40/48/64px common. Always square source, `border-radius:50%` (or rounded square for orgs).
- Fallback chain: image → initials (on deterministic bg color) → generic icon. Never broken-image.
- `alt` = person's name (or `alt=""` if name is shown adjacent and avatar is decorative).
- Avatar group / stack: overlap ~ -8px, cap visible count, show "+N" overflow chip; the +N has an accessible label/list.

Top 3 mistakes:
1. No initials/icon fallback → broken image icon.
2. `alt="avatar"` (useless) instead of the name.
3. Non-square images squished into circles.

---

## Tabs

Anatomy: `[ tablist [tab][tab*selected][tab] ][ tabpanel ]`. Underline, pill, or segmented style.

- `role="tablist"` > `role="tab"` (with `aria-selected`, `aria-controls`) > `role="tabpanel"` (`aria-labelledby`).
- Keyboard: Arrow keys move between tabs; Tab moves into the panel; only the selected tab is in the tab order (`tabindex=0`), others `-1` (roving tabindex).
- Active tab: clear indicator (underline/fill) + ≥3:1 contrast, not color-only.
- ≤ ~6–7 tabs; overflow → scrollable tablist or "More" menu, not wrapping into two rows.
- Don't use tabs for sequential steps (use a stepper) or for primary nav between pages (use links/nav).

Top 3 mistakes:
1. Tabs that are really page navigation (should be routed links).
2. No roving tabindex / arrow-key support.
3. Too many tabs wrapping to a second row.

---

## Accordions

Anatomy: each item = `[ header button (expand/collapse, chevron) ][ region (content) ]`.

- Header is a `<button>` with `aria-expanded` and `aria-controls`; region has `role="region"` + `aria-labelledby`.
- Chevron rotates to indicate state; **also** change something non-color (rotation works).
- Decide: single-open (accordion) vs multi-open. Don't auto-collapse a section the user is reading.
- Hit target: full header row clickable, ≥44px tall.

Top 3 mistakes:
1. `<div>` header with click handler — not a button, no keyboard/`aria-expanded`.
2. Animating `height:auto` jankily — animate `grid-template-rows` 0fr→1fr or max-height.
3. Hiding critical content (search results, form fields) behind collapsed accordions.

---

## Toasts / notifications

- For **transient, low-priority** confirmations ("Saved", "Copied"). Not for critical errors that need action — use inline messages or a dialog.
- Position: consistent corner (top-right or bottom-center). Stack newest on top, max ~3 visible.
- Auto-dismiss 4–6s (longer if it has an action like "Undo"); **pause on hover/focus**. Provide a manual close.
- A11y: `role="status"` + `aria-live="polite"` for success/info; `role="alert"` + `aria-live="assertive"` for errors. Don't trap focus; if there's an action ("Undo"), make it keyboard-reachable and extend the timeout.

Top 3 mistakes:
1. Auto-dismissing an actionable toast before the user can click "Undo".
2. Putting critical/blocking errors in a toast that vanishes.
3. No `aria-live` → screen reader users never hear it.

---

## Dropdown menus

Anatomy: `[ trigger button ][ menu: items, separators, sections, optional submenus ]`.

- `role="menu"` + `role="menuitem"`; trigger has `aria-haspopup="menu"` + `aria-expanded`.
- Keyboard: Enter/Space/ArrowDown opens; arrows move; Enter activates; Escape closes and returns focus to trigger; type-ahead optional.
- Close on: item select, Escape, outside click, blur. Position with flip/shift to stay in viewport.
- A menu is for **commands/actions**, not form inputs — don't put text fields or checkboxes in a `role="menu"` (use a popover/dialog).

Top 3 mistakes:
1. Using a `menu` role for a list of links or form controls (wrong semantics).
2. No focus return to the trigger on close.
3. Menu clips off-screen with no flip/collision handling.

---

## Pagination

Anatomy: `[ ‹ Prev ][ 1 ][ 2 ][ … ][ current ][ … ][ N ][ Next › ]`, optionally with "showing X–Y of Z" and page-size selector.

- Current page: `aria-current="page"`, visually distinct, **not clickable as a no-op**.
- Disable (and `aria-disabled`) Prev on first page, Next on last.
- Use `<nav aria-label="Pagination">`. Each page is a real link/button with an accessible label ("Go to page 3").
- Truncate large ranges with ellipsis; always show first, last, current ±1.
- Choose pagination vs infinite scroll vs load-more per [./data-display.md](./data-display.md).

Top 3 mistakes:
1. Current page styled but still a clickable link (confusing).
2. Prev/Next not disabled at the ends.
3. Icon-only arrows with no accessible label.

---

## Cross-cutting rules

- **Build on accessible primitives** (Radix, React Aria, native elements) instead of hand-rolling trap/roving-tabindex/aria. Style those.
- **Tokens, not hardcodes**: pull color/spacing/radius/elevation from tokens — see [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md).
- **Reduced motion**: gate non-essential animation behind `@media (prefers-reduced-motion: reduce)`.
- **Touch first**: every interactive primitive must work without hover and meet 44×44px.

---

## Agent checklist

- [ ] Implement all 9 states from the states matrix for every interactive component; never signal a state by color alone.
- [ ] Use `:focus-visible` with a ≥3:1 contrast 2px ring; never `outline:none` without a replacement.
- [ ] Enforce a 44×44px minimum hit target even when the visual control is smaller.
- [ ] Allow only one `primary` button per view; reserve `destructive` styling for irreversible actions.
- [ ] Keep button/input width stable across loading and error states; disable buttons while loading and set `aria-busy`.
- [ ] Use visible top labels, never placeholders-as-labels; wire `aria-invalid` + `aria-describedby` for errors.
- [ ] Prefer native `<select>` and the `<dialog>` element / vetted primitives over hand-rolled custom controls.
- [ ] Give modals a focus trap, focus return, Escape-to-close, scroll lock, and `role="dialog"` + `aria-modal`.
- [ ] Make tabs/menus keyboard-operable with roving tabindex, arrow keys, Enter, and Escape with focus return.
- [ ] Use `aria-live` (`polite` for info, `assertive`/`alert` for errors) on toasts and pause auto-dismiss on hover/focus.
- [ ] Mark current pagination page with `aria-current="page"` and disable Prev/Next at the boundaries.
- [ ] Pull every color/spacing/radius value from tokens and gate non-essential motion behind `prefers-reduced-motion`.
