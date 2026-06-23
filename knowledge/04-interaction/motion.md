# Motion & Animation

> Purpose: Define how to use motion in UI so it communicates meaning, runs at 60fps, and never feels gratuitous, slow, or accessibility-hostile.

**When to read this:** Any time you add a transition, transform, page change, modal, list reveal, scroll effect, or "make it feel alive" request. Read before reaching for a library.

Motion is a functional layer, not a decorative one. Every animation in a world-class interface answers one of three questions: *Where did this come from? Where did it go? What is the relationship between these two things?* If an animation answers none of them, delete it.

---

## 1. Core principles

| Principle | Rule | Why |
|---|---|---|
| Motion has meaning | Animate to show causality, continuity, or hierarchy — never to decorate | Decorative motion becomes noise; users tune it out or get annoyed |
| Fast by default | UI motion is 100–300ms; nothing crosses 500ms | Beyond ~400ms feedback feels laggy; the user has moved on mentally |
| Transform & opacity only | Animate `transform` and `opacity`; never `width/height/top/left/margin` | Only transform/opacity are GPU-composited — the rest trigger layout/paint and drop frames |
| Natural easing | Entrances `ease-out`, exits `ease-in`, moves use the standard curve | Linear motion reads as mechanical and cheap |
| Respect reduced motion | Provide a reduced fallback for `prefers-reduced-motion` — MANDATORY | Motion triggers vestibular disorders, nausea, and migraines for real users |
| Interruptible | Animations must be cancelable / re-targetable mid-flight | A modal that ignores a second click feels broken |
| One focal motion | Don't animate five things at full intensity at once | Competing motion has no hierarchy; the eye doesn't know where to look |

**Do / Don't**

```
DON'T: bounce a button label on every render "for delight"
DO:    animate a checkmark drawing in after a save succeeds (it reports state)

DON'T: slide a panel in over 600ms with a fancy overshoot
DO:    slide it in over 250ms ease-out so the user can start reading immediately

DON'T: fade in the entire page on every navigation
DO:    animate only the elements that actually changed
```

---

## 2. Duration guidelines

Match duration to **distance traveled** and **surface size**. A 16px icon toggle should be faster than a full-screen sheet.

| Tier | Range | Use for |
|---|---|---|
| Micro | 100–150ms | Hover, focus ring, button press, toggle, icon swap, tooltip, color change |
| Standard | 200–300ms | Dropdowns, popovers, accordion, tab switch, small card expand, toast in |
| Large | 300–500ms | Modals, full sheets, page/route transitions, drawer, hero reveal |
| Never | > 500ms | Nothing in core UI. Reserve only for ambient/marketing loops |

Rules of thumb:
- **Exits are faster than entrances** (typically 0.7–0.8×). Getting out of the way should feel instant; arriving can have a beat. Entrance 250ms → exit ~180ms.
- **Larger surface = longer duration**, but cap at 500ms. A 1200px sheet at 300ms; a 40px chip at 120ms.
- **Mobile slightly longer than desktop** for the same element (touch surfaces are bigger, motion is more visible) — but still under the cap.
- **Distance scales duration sub-linearly.** Don't make a 2× longer slide take 2× the time; ~1.3× is enough.

```
DON'T: every transition = 300ms regardless of what's moving
DO:    micro 120ms, standard 240ms, modal 320ms — tier it
```

---

## 3. Easing

Easing is what separates "animated" from "cheap." The curve carries as much meaning as the duration.

| Token | cubic-bezier | Use for |
|---|---|---|
| `ease-out` (decelerate) | `cubic-bezier(0, 0, 0.2, 1)` | Entrances — element flies in fast, settles gently |
| `ease-in` (accelerate) | `cubic-bezier(0.4, 0, 1, 1)` | Exits — element eases out then leaves quickly |
| `standard` | `cubic-bezier(0.4, 0, 0.2, 1)` | On-screen moves, color/size changes, reflows |
| `emphasized` | `cubic-bezier(0.2, 0, 0, 1)` | Hero / large expressive transitions (Material 3 style) |
| `spring-ish` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful overshoot for toggles, likes, adds — use sparingly |
| `linear` | `linear` | ONLY spinners, marquees, progress bars, looping ambient motion |

**Why ease-out for entrances:** the element arrives quickly (grabs attention) and decelerates into place (feels physical, settled). The opposite — ease-in entrance — makes things crawl onscreen then snap, which reads as broken.

**Why ease-in for exits:** the element lingers briefly then accelerates away, mirroring real-world departure. An ease-out exit feels like it's being yanked.

```
DON'T: use `ease` (the CSS default cubic-bezier(0.25,0.1,0.25,1)) for everything
DO:    pick ease-out / ease-in / standard deliberately per direction

DON'T: linear for a panel slide (mechanical, robotic)
DO:    linear only for the spinner and the indeterminate progress bar
```

---

## 4. Spring physics vs duration-based

Two models. Know when to reach for each.

**Duration-based (cubic-bezier + ms):** deterministic, predictable, easy to coordinate. Use for the vast majority of UI: fades, slides, color, opacity, anything that must finish at a known time.

**Spring physics (stiffness/damping/mass):** motion is defined by physics, not a clock. Naturally interruptible and re-targetable — if the target changes mid-flight, the spring redirects smoothly. Best for **draggable** elements, **gesture-driven** UI, and anything that must respond to a moving target (a sheet following a finger, a re-ordering list).

```tsx
// Framer Motion spring — great for drag / gesture / interruptible
const spring = { type: "spring", stiffness: 400, damping: 30, mass: 1 };

// Tuning guide:
// stiffness ↑ = snappier, reaches target faster
// damping   ↑ = less bounce (damping ≈ 30+ ≈ no overshoot at stiffness 400)
// mass      ↑ = heavier, slower, more momentum
// "bouncy"   : stiffness 300, damping 15
// "snappy"   : stiffness 500, damping 35
// "gentle"   : stiffness 150, damping 25
```

| Choose | When |
|---|---|
| Duration-based | Fades, page transitions, modals, toasts, coordinated sequences, anything timed |
| Spring | Drag, swipe, pull, reorder, shared-layout, "follows my input" interactions |

```
DON'T: spring a tooltip with stiffness 120 damping 8 — it wobbles for 800ms
DO:    spring drag handles; use a 240ms ease-out for the tooltip
```

---

## 5. The 12 principles, applied to UI

The classic Disney principles — only the ones that matter for screens:

- **Easing (slow in / slow out):** never linear for UI moves. This is principle #1 and the single biggest quality lever. (See §3.)
- **Anticipation:** a tiny wind-up before a big move — a button dips 2px before launching a sheet, a list item lifts before it drags. Keep it under 80ms or it reads as lag.
- **Follow-through & overlap:** elements don't all stop at once. A modal lands at 300ms; its content fades in starting at 150ms. Trailing children settle slightly after the container.
- **Staggering (secondary action):** reveal list items 30–50ms apart, not all at once. Creates a readable cascade and implies order. Cap total stagger at ~300ms — don't make item 12 wait a full second.
- **Squash & stretch:** subtle scale on press (`scale: 0.97`) and release. Communicates physical touch. Never literal cartoon squashing.
- **Arcs:** objects that travel a distance can follow a slight curve rather than a straight line (FLIP/shared-layout transitions do this naturally). Optional polish.
- **Secondary action:** the checkmark that draws while the row also turns green — supporting motion that reinforces the primary one without competing.
- **Timing:** covered in §2. The right duration *is* the principle.
- **Exaggeration:** a controlled overshoot on a success toggle (`cubic-bezier(0.34,1.56,0.64,1)`) — confident, not silly.
- **Staging:** direct the eye. Dim/blur the background when a modal enters so attention lands on one focal element.

```
DON'T: stagger 40 search results at 50ms each (2s of waiting)
DO:    stagger the first ~8 visible items; render the rest instantly
```

---

## 6. Enter / exit transitions

The hardest part is **exit** — most implementations only animate entrance, then the element pops out. Use `AnimatePresence` (Framer Motion) or `@starting-style` + transitions (CSS) so exits animate too.

```tsx
// Framer Motion — enter AND exit
import { AnimatePresence, motion } from "framer-motion";

<AnimatePresence mode="wait">
  {open && (
    <motion.div
      initial={{ opacity: 0, y: 8 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: 8 }}
      transition={{ duration: 0.24, ease: [0, 0, 0.2, 1] }}  // ease-out
    />
  )}
</AnimatePresence>
```

```css
/* Pure CSS enter+exit with @starting-style (no JS) */
.popover {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 200ms cubic-bezier(0.4,0,0.2,1),
              transform 200ms cubic-bezier(0.4,0,0.2,1);
}
@starting-style {        /* the FROM state when first rendered */
  .popover { opacity: 0; transform: translateY(8px); }
}
.popover[hidden] { opacity: 0; transform: translateY(8px); } /* exit */
```

Direction conventions: things enter **from where they conceptually live** — a dropdown drops down, a bottom sheet rises, a toast slides from its anchor edge. Don't fade everything from `scale: 0` center-screen.

---

## 7. Layout animations

When an element changes size/position because content changed (list reorder, accordion, filtering, grid reflow), animate the *layout change* — don't let it jump. Use FLIP (First-Last-Invert-Play): measure before, measure after, invert with a transform, play to zero.

```tsx
// Framer Motion does FLIP for you with `layout`
<motion.div layout transition={{ duration: 0.3, ease: [0.4,0,0.2,1] }}>
  {items.map(i => <motion.div layout key={i.id}>{i.label}</motion.div>)}
</motion.div>
```

```js
// Native View Transitions API — animate DOM changes across a swap
if (document.startViewTransition) {
  document.startViewTransition(() => updateDOM());  // browser tweens before↔after
} else {
  updateDOM(); // graceful fallback, no animation
}
```

```
DON'T: reorder a list and let rows teleport to new positions
DO:    `layout` (or View Transitions) so rows slide to their new slots
```

---

## 8. Scroll-triggered animation

Powerful and frequently abused. Rules:

- **Reveal-on-scroll once, subtly:** fade + 12–20px rise as a section enters the viewport. Trigger at ~15% visibility, animate once, never re-hide.
- **Never hijack the scroll.** No scroll-jacking, no forcing the user through a "scroll experience." It breaks expectations and accessibility.
- **Use `IntersectionObserver`**, not scroll-event listeners (which fire constantly and jank).
- **Respect performance:** scroll-linked animation (progress tied to scroll position) is expensive. Prefer the native `animation-timeline: scroll()` / `view()` (CSS Scroll-Driven Animations) where supported — it runs off the main thread.
- **Always honor reduced motion** — scroll reveals are a top vestibular offender.

```tsx
// Reveal once via Framer Motion
<motion.section
  initial={{ opacity: 0, y: 16 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, amount: 0.15 }}
  transition={{ duration: 0.4, ease: [0,0,0.2,1] }}
/>
```

```css
/* Native scroll-driven, off-main-thread, with reduced-motion guard */
@media (prefers-reduced-motion: no-preference) {
  .reveal {
    animation: fade-up linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 30%;
  }
}
@keyframes fade-up { from { opacity: 0; translate: 0 20px; } to { opacity: 1; translate: 0 0; } }
```

For heavy timeline/scrubbing work (pinned sections, complex sequencing) **GSAP + ScrollTrigger** is the production tool — but budget it carefully and ship a static fallback.

---

## 9. What to animate (the 60fps rule)

The compositor can animate `transform` and `opacity` without touching layout or paint. Everything else risks dropped frames.

| Animate (cheap, GPU) | Avoid (triggers layout/paint) | Use instead |
|---|---|---|
| `transform: translate / scale / rotate` | `top / left / right / bottom` | `transform: translate()` |
| `opacity` | `width / height` | `transform: scaleX/scaleY()` (+ correct transform-origin) |
| `filter` (sparingly) | `margin / padding` | layout shift via transform |
| | `box-shadow` (paint-heavy) | animate an overlaid pseudo-element's `opacity` |
| | `background-color` on huge areas | acceptable on small elements |

```
DON'T: transition: left 300ms;     /* janks — recalculates layout every frame */
DO:    transition: transform 300ms; transform: translateX(40px);
```

Tips: add `will-change: transform` *only* right before an animation and remove it after (it costs memory). Promote a layer deliberately, not globally. Test on a mid-tier Android, not your M-series laptop.

---

## 10. prefers-reduced-motion (MANDATORY)

You must ship a reduced-motion path. "Reduced" does **not** mean "no feedback" — it means no large positional/scaling motion. Keep opacity/color cross-fades; kill slides, parallax, and big scale/spring effects.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
/* Then re-enable gentle opacity fades where they aid comprehension. */
```

```tsx
// Framer Motion — respects the OS setting globally
import { MotionConfig } from "framer-motion";
<MotionConfig reducedMotion="user">{children}</MotionConfig>

// Or branch manually:
import { useReducedMotion } from "framer-motion";
const reduce = useReducedMotion();
const variants = reduce
  ? { initial: { opacity: 0 }, animate: { opacity: 1 } }            // fade only
  : { initial: { opacity: 0, y: 16 }, animate: { opacity: 1, y: 0 } }; // fade + move
```

```
DON'T: ignore the media query and ship parallax to everyone
DON'T: turn ALL feedback off (a save with zero feedback is worse)
DO:    swap big motion for cross-fades; keep state changes legible
```

---

## 11. Recipes

### Fade-in-up
```tsx
const fadeInUp = {
  initial: { opacity: 0, y: 16 },
  animate: { opacity: 1, y: 0 },
  transition: { duration: 0.24, ease: [0, 0, 0.2, 1] }, // ease-out
};
<motion.div {...fadeInUp} />
```

### Stagger children
```tsx
const container = {
  animate: { transition: { staggerChildren: 0.04, delayChildren: 0.05 } },
};
const item = {
  initial: { opacity: 0, y: 12 },
  animate: { opacity: 1, y: 0, transition: { duration: 0.22, ease: [0,0,0.2,1] } },
};
<motion.ul variants={container} initial="initial" animate="animate">
  {rows.map(r => <motion.li key={r.id} variants={item}>{r.label}</motion.li>)}
</motion.ul>
// Stagger ONLY the visible window; cap total cascade < 300ms.
```

### Modal enter / exit (with backdrop + reduced motion)
```tsx
<AnimatePresence>
  {open && (
    <>
      <motion.div className="backdrop"
        initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }}
        transition={{ duration: 0.2 }} onClick={close} />
      <motion.div className="modal" role="dialog" aria-modal="true"
        initial={{ opacity: 0, scale: 0.96, y: 8 }}
        animate={{ opacity: 1, scale: 1,    y: 0 }}
        exit={{    opacity: 0, scale: 0.96, y: 8 }}
        transition={{ duration: 0.28, ease: [0.2, 0, 0, 1] }} />  {/* emphasized */}
    </>
  )}
</AnimatePresence>
```

### Shared layout (element morphs between two places)
```tsx
// Same layoutId in both states → Framer tweens position+size between them.
{!expanded && <motion.div layoutId="card" onClick={open} />}
{expanded  && <motion.div layoutId="card" className="fullscreen" />}
// transition={{ type: "spring", stiffness: 350, damping: 35 }}
```

### Press feedback (squash)
```css
.btn { transition: transform 120ms cubic-bezier(0.4,0,0.2,1); }
.btn:active { transform: scale(0.97); }
```

---

## 12. Easing & duration cheat-sheet

| Element | Duration | Easing | Notes |
|---|---|---|---|
| Button / icon press | 100–120ms | standard `(0.4,0,0.2,1)` | `scale: 0.97` on active |
| Hover / focus ring | 120–150ms | standard | color + ring opacity |
| Toggle / switch | 150–200ms | spring-ish `(0.34,1.56,0.64,1)` | small overshoot OK |
| Tooltip in | 120ms | ease-out `(0,0,0.2,1)` | out 80ms ease-in |
| Dropdown / popover | 180–220ms | ease-out | originate from anchor |
| Accordion / expand | 200–280ms | standard | use `layout`, not height |
| Tab content swap | 200ms | ease-out | cross-fade + 8px slide |
| Toast in / out | in 250ms / out 180ms | out ease-out / in ease-in | slide from edge |
| Modal / dialog | 280–320ms | emphasized `(0.2,0,0,1)` | backdrop 200ms fade |
| Bottom sheet / drawer | 300–360ms | ease-out / spring | spring if drag-dismiss |
| Page / route transition | 300–400ms | emphasized | animate changed regions only |
| List item stagger | 30–50ms apart | per-item ease-out | cap cascade < 300ms |
| Spinner / marquee | continuous | **linear** | the only correct linear use |

Related: [microinteractions.md](./microinteractions.md) for the trigger→feedback anatomy of these moments, and [states-feedback.md](./states-feedback.md) for when motion is actually reporting a state change.

---

## Agent checklist
- [ ] Confirm each animation answers where-from / where-to / what-relationship — else remove it.
- [ ] Keep UI durations 100–300ms; cap everything at 500ms; make exits ~0.7–0.8× the entrance.
- [ ] Use ease-out for entrances, ease-in for exits, `cubic-bezier(0.4,0,0.2,1)` for on-screen moves; linear only for spinners/marquees.
- [ ] Animate only `transform` and `opacity`; never `width/height/top/left/margin/box-shadow`.
- [ ] Reach for springs on drag/gesture/interruptible motion; duration curves for everything timed.
- [ ] Animate exits, not just entrances (`AnimatePresence` or `@starting-style` + `[hidden]`).
- [ ] Use `layout` / View Transitions for reflows so elements slide instead of teleporting.
- [ ] Trigger scroll reveals once via `IntersectionObserver` or native scroll-driven CSS; never scroll-jack.
- [ ] Cap list stagger at ~300ms total and only stagger the visible window.
- [ ] Ship a `prefers-reduced-motion: reduce` fallback that swaps big motion for cross-fades — never zero feedback.
- [ ] Add `will-change` only just-in-time and remove it; verify 60fps on a mid-tier device.
