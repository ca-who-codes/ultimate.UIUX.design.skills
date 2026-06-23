# Decision Framework

> Purpose: The repeatable, ordered procedure an agent runs to design any screen from scratch — from "what's the job" to "ship it" — without skipping the steps that separate amateur from world-class.

**When to read this:** Read at the start of *every* screen, component, or flow you design, and any time you're tempted to start writing markup before you've decided what the screen is *for*. This is the workflow; the reasoning behind each gate lives in [`design-principles.md`](./design-principles.md).

---

## The core loop (10 gates)

Run these in order. Do not skip ahead — each gate's output feeds the next. If you can't answer a gate, stop and resolve it before writing code.

```
(a) Job-to-be-done   → (b) The one element → (c) Hierarchy
→ (d) Layout archetype → (e) Tokens → (f) States
→ (g) Responsive → (h) Accessibility → (i) Motion → (j) Self-review
```

---

## (a) Clarify the job-to-be-done and primary action

Before any pixels, answer in one sentence each:

1. **Who** is on this screen? (state of mind, device, expertise)
2. **What** are they trying to accomplish? (the *job*, in their words)
3. **What single action** advances that job most? → this is the **primary action**.
4. **What happens next** after they do it?

> **Rule — one primary action per screen.** Every view has exactly one action you want most users to take. Name it. If you can't choose between two, the screen is doing two jobs — split it or demote one to secondary.

Write the JTBD as: *"When [situation], I want to [motivation], so I can [outcome]."* If you can't fill this in, you don't understand the screen yet — ask the user or make an explicit assumption and label it.

---

## (b) Define the single most important element per view

From the JTBD, pick the **one element** that must dominate — usually the primary action, the key piece of content, or the input that starts the job.

- This element gets the strongest hierarchy treatment (size + color + space + position).
- Everything else is explicitly ranked below it.
- **Squint test (do it now):** blur your mental image of the screen until detail disappears. The element that still stands out should be your most-important one. If something else dominates, or nothing does, fix the hierarchy before continuing. (Literally: `filter: blur(6px)` on a render, or squint at the mock.)

---

## (c) Establish hierarchy

Rank every region/element 1, 2, 3… Then assign the *minimum* hierarchy tools (size, weight, color, spacing, position — see [`design-principles.md`](./design-principles.md) §4) to make that ranking visually obvious.

- Use **one** accent color for the primary action only (Von Restorff).
- Demote secondary actions to ghost/outline/text styles.
- Use whitespace as the first emphasis tool, color as the last.

**5-second test:** Show the screen for 5 seconds, then ask (yourself, simulating the user): *What is this page? What's the main thing I can do? What did you notice first?* If the answers don't match your intended JTBD and primary action, the hierarchy is wrong. Iterate before moving on.

---

## (d) Choose a layout archetype

Pick a known archetype rather than inventing one (Jakob's Law). Match the archetype to the JTBD:

| JTBD pattern | Archetype | Key traits |
|--------------|-----------|------------|
| Convince / market | **Landing / marketing** | Hero with one CTA, F/Z scan flow, social proof, repeated CTA |
| Single focused task | **Centered single-column** | Max-width 480–640px, one column, minimal nav |
| Enter structured data | **Form layout** | Labeled sections ≤5 fields, single column, inline validation |
| Monitor / overview | **Dashboard** | Card grid, most-important metric top-left, scannable |
| Browse many items | **List / feed / grid** | Consistent cards, filters, pagination/infinite scroll |
| Read long content | **Article / doc** | 60–75ch measure, generous line-height, TOC |
| Operate on records | **Master-detail / table** | List or table + detail pane, bulk actions |
| Decide between options | **Pricing / comparison** | ≤4 columns, one highlighted (recommended) |

Then choose the grid: most screens are a **12-column grid**, `max-width` 1100–1280px for content, with a centered container. Mobile collapses to a single column. Default content `max-width` for prose is **65ch**.

---

## (e) Apply tokens (opinionated defaults when the user is vague)

Never use arbitrary values. Pull from tokens. When the user hasn't specified a system, **reach for these defaults — and say you're using them as a starting point** so they can override.

| Token | Default (starting point) | Notes |
|-------|--------------------------|-------|
| **Font stack** | `ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif` | Zero-load, looks native. Swap to Inter / Geist for brand. |
| **Type scale** | 12, 14, 16, 18, 20, 24, 30, 36, 48, 60 px (ratio ~1.25) | Body = 16px min. See `../02-foundations/typography.md`. |
| **Line-height** | 1.5 body, 1.2 headings | |
| **Measure** | 65ch for prose | 45–75ch acceptable. |
| **Spacing scale** | 4, 8, 12, 16, 24, 32, 48, 64, 96 px | 8px base. Never 7px/13px. See `../02-foundations/spacing.md`. |
| **Radius** | 8px default; 4px small (chips), 12–16px cards, 9999px pills | Be consistent — pick one family. |
| **Border** | 1px solid at ~12–15% foreground opacity | |
| **Shadow** | 2-layer: ambient + direct. e.g. `0 1px 2px rgb(0 0 0/.06), 0 4px 12px rgb(0 0 0/.08)` | Subtle; elevation = importance. |
| **Motion duration** | 150ms micro (hover), 200–300ms standard (panels), 400ms large | See `../06-motion/motion-principles.md`. |
| **Easing** | `cubic-bezier(0.2, 0, 0, 1)` (decelerate) for entrances | ease-out feels responsive. |
| **Color** | Neutral gray ramp + 1 brand accent + semantic (success/warn/error/info) | Verify contrast at gate (h). `../02-foundations/color.md`. |
| **Focus ring** | `2px solid` accent, `outline-offset: 2px` | Never remove without replacement. |

```css
/* Drop-in default token block — override per brand */
:root {
  --font-sans: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --space-1:4px; --space-2:8px; --space-3:12px; --space-4:16px;
  --space-6:24px; --space-8:32px; --space-12:48px; --space-16:64px;
  --radius:8px; --radius-lg:14px;
  --dur-fast:150ms; --dur:240ms; --ease:cubic-bezier(.2,0,0,1);
}
```

---

## (f) Handle ALL states

A view is not "the happy path." Design every state or it will break in production. **This is the single most-skipped step — do not skip it.**

| State | Must answer | Default move |
|-------|-------------|--------------|
| **Empty** | First-run, no data yet. | Explain what goes here + one CTA to create the first item. Never a blank void. |
| **Loading** | Data in flight. | Skeleton (preferred) or spinner, shown within 400ms. Match skeleton to final layout to avoid shift. |
| **Error** | Request/validation failed. | Plain-language cause + recovery action + retry. Preserve the user's input. |
| **Success** | Action completed. | Confirm clearly, then surface the next action (Peak-End). |
| **Partial** | Some data, some missing/loading; long lists; truncation. | Show what you have; lazy-load the rest; "Load more"; handle 1 item and 10,000 items. |

Also cover: **disabled**, **read-only / permission-denied**, **offline**, and **zero-results-after-filter** (distinct from true empty — offer "clear filters").

> **Triage rule:** if you've only drawn the happy path, you are <50% done.

---

## (g) Responsive plan

Design **mobile-first** (content priority forces clarity), then enhance up.

| Breakpoint | Width | Layout shift |
|------------|-------|--------------|
| Base (mobile) | <640px | Single column, stacked, bottom-anchored primary CTA (thumb zone), touch targets ≥48px |
| `sm` | ≥640px | Looser spacing, maybe 2-col where it helps |
| `md` | ≥768px | Multi-column begins, sidebar may appear |
| `lg` | ≥1024px | Full desktop layout, max content width caps |
| `xl` | ≥1280px | Don't let line length exceed ~75ch even on huge screens |

Rules: never hide *core* functionality on mobile (reflow, don't remove). Use `clamp()` for fluid type/spacing between breakpoints. Test at **320px** (smallest common), **768px**, **1440px**. Reflow at **400% zoom** without horizontal scroll (WCAG 1.4.10).

---

## (h) Accessibility pass

Non-negotiable. This gate can fail a "beautiful" design.

- **Contrast:** body/UI text ≥ **4.5:1**; large text (≥24px or ≥18.66px bold) and UI components/graphics ≥ **3:1**. Verify with a contrast tool, don't eyeball.
- **Semantics:** real `<button>`/`<a>`/`<nav>`/`<main>`/`<h1-6>` in logical order; one `<h1>`; no `<div onclick>`.
- **Keyboard:** every action reachable and operable by keyboard; logical tab order; visible focus ring (never `outline:none` without a replacement); Esc closes overlays; focus trapped in modals and returned on close.
- **Labels:** every input has a `<label>`; icon-only buttons have `aria-label`; images have `alt`.
- **Motion:** honor `prefers-reduced-motion`.
- **Targets:** ≥44×44px (WCAG 2.5.8 ≥24px is the floor; design for 44–48).
- **Don't rely on color alone** to convey state (add icon/text).

Full checklist → `../05-accessibility/wcag.md`.

---

## (i) Motion pass

Motion clarifies; it never decorates. Add it last, sparingly.

- Animate to show **cause/effect, spatial relationship, or state change** — not for flair.
- Durations: 150ms micro, 200–300ms standard, ≤400ms for large/entrance. Anything >400ms feels sluggish (Doherty).
- Easing: decelerate (`ease-out` / `cubic-bezier(.2,0,0,1)`) for entrances; accelerate for exits.
- Animate **transform** and **opacity** only (GPU-cheap); avoid animating layout props (width/height/top/left).
- Provide a `prefers-reduced-motion` path that reduces/disables non-essential motion.
- Stagger lists subtly (20–40ms apart) for hierarchy; don't make users wait on choreography.

Details → `../06-motion/motion-principles.md`.

---

## (j) Self-review

Before declaring done, run this gauntlet. Any failure → loop back to the named gate.

1. **Squint test** passes — the right element dominates. *(→ b/c)*
2. **5-second test** passes — JTBD and primary action are obvious. *(→ a/c)*
3. **One primary action** confirmed; secondaries recede. *(→ a)*
4. **All states** drawn: empty, loading, error, success, partial + edge cases. *(→ f)*
5. **Responsive** verified at 320 / 768 / 1440px and 400% zoom. *(→ g)*
6. **Accessibility:** contrast, keyboard, focus, labels, reduced-motion all pass. *(→ h)*
7. **Tokens only** — no arbitrary px, no off-scale colors. *(→ e)*
8. **Nielsen's 10** — no blocking heuristic violations. *(→ design-principles.md)*
9. **Consistency** — components match the rest of the system.
10. **Cut test** — remove anything that doesn't serve the JTBD. Did you?

---

## Add vs. remove triage table

When deciding whether an element earns its place, consult this. **Default bias: remove.** Clutter is the most common failure; restraint is the rarest skill.

| Situation | Verdict | Why |
|-----------|---------|-----|
| Element directly serves the primary action | **Keep** | Core to JTBD |
| Element is used by <20% of users on this view | **Defer** (progressive disclosure) | Hick's Law; surface common, hide rare |
| Two elements do the same job | **Merge / remove one** | Redundancy adds load |
| Decorative, no information or function | **Remove** | Competes for attention (Heuristic #8) |
| "Just in case" / "might be useful" | **Remove** | Speculative clutter; add it when a real need appears |
| Explains something the UI should make self-evident | **Remove the need, not just the text** | Don't Make Me Think |
| A third+ competing CTA on the view | **Demote or remove** | One primary action rule |
| Adds a real, frequent shortcut for experts | **Keep, but unobtrusive** | Flexibility (Heuristic #7) without novice cost |
| You're unsure whether it's needed | **Remove and ship** | Easier to add proven needs than to defend clutter |

**The rule of thumb:** *Subtraction is design.* If removing an element doesn't break the job, it was noise.

---

## Defaults for a vague brief (quick-start kit)

When the user says "just make it good," reach for these and state them as starting points:

- **Layout:** centered container, `max-width: 1120px`, 12-col grid, mobile-first single column.
- **Type:** system sans stack, 16px body, 1.25 scale, 1.5 line-height, 65ch measure.
- **Spacing:** 8px scale (4/8/12/16/24/32/48/64).
- **Radius:** 8px (cards 14px, pills full).
- **Color:** neutral gray ramp + one brand accent + semantic set; light + dark via tokens; verify 4.5:1.
- **Motion:** 200ms standard, `cubic-bezier(.2,0,0,1)`, transform/opacity only, reduced-motion path.
- **Components:** filled primary button (one per view), outline secondary, text tertiary; visible focus ring everywhere.
- **States:** always design empty + loading (skeleton) + error + success.

Frame every default as "I'm starting from X; tell me if you want Y" — defaults are a launchpad, not a verdict.

---

## Cross-references

- The "why" behind every gate → [`design-principles.md`](./design-principles.md).
- Tokens in depth → `../02-foundations/color.md`, `../02-foundations/typography.md`, `../02-foundations/spacing.md`.
- Component specs and states → `../04-components/`.
- Accessibility gate detail → `../05-accessibility/wcag.md`.
- Motion gate detail → `../06-motion/motion-principles.md`.

---

## Agent checklist

- [ ] Write the JTBD sentence and name the single primary action before any markup.
- [ ] Identify the one most-important element and confirm it via the squint test.
- [ ] Rank all elements and apply the minimum hierarchy tools to make the order obvious.
- [ ] Choose a named layout archetype that matches the JTBD; don't invent one.
- [ ] Use tokens only — no arbitrary px, off-scale colors, or one-off radii.
- [ ] Design empty, loading, error, success, and partial states (plus disabled/permission/offline).
- [ ] Plan mobile-first; verify at 320/768/1440px and reflow at 400% zoom.
- [ ] Pass the accessibility gate: 4.5:1 contrast, keyboard, visible focus, labels, reduced-motion.
- [ ] Add motion only to clarify cause/effect; keep ≤400ms on transform/opacity with a reduced-motion path.
- [ ] Run the self-review gauntlet; loop back to the failing gate before shipping.
- [ ] Apply the add-vs-remove triage with a default bias toward removal.
- [ ] When the brief is vague, apply the quick-start defaults and label them as overridable starting points.
