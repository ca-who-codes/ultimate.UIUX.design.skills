# Microinteractions

> Purpose: Define the small, single-purpose moments — a press, a toggle, a copy confirmation — that make an interface feel responsive, trustworthy, and crafted, without crossing into friction.

**When to read this:** Whenever you build a control that the user touches directly — buttons, toggles, inputs, like/save, copy, drag handles, pull-to-refresh — or any moment that needs immediate "I heard you" feedback.

A microinteraction is the smallest unit of interaction design: one job, one moment. The difference between a $10 product and a $100 product is almost entirely in these. They are also the easiest place to *over*-design — every one must earn its motion.

---

## 1. Anatomy of a microinteraction

Every microinteraction has four parts (Dan Saffer's model):

| Part | What it is | Example (copy-to-clipboard) |
|---|---|---|
| **Trigger** | What starts it — user action or system event | User clicks the copy icon |
| **Rules** | What happens & in what order | Write to clipboard → if success, show confirmation |
| **Feedback** | How the user knows it worked | Icon morphs to a checkmark, label reads "Copied!" |
| **Loops & modes** | Duration, repetition, edge states | Reverts to copy icon after 2s; disabled if nothing to copy |

Design all four. The most common failure is shipping the trigger + rules but skimping on **feedback** (the user can't tell it worked) or **loops** (the success state never reverts, or repeats annoyingly).

```
DON'T: clipboard.writeText(x)  // silent — user has no idea it worked
DO:    write → swap icon to ✓ + "Copied" → revert after 2000ms
```

---

## 2. The feedback latency budget

Feedback must begin within **100ms** of the trigger or the action feels unacknowledged. This is non-negotiable and separate from how long the *result* takes.

| Perceived | Threshold | Implication |
|---|---|---|
| Instant | < 100ms | Acknowledge the input within this window — always |
| Connected | 100ms–1s | OK for the *result*, but show acknowledgement immediately |
| Attention drifts | > 1s | Show progress/skeleton; the action is no longer "instant" |
| Context lost | > 10s | User has mentally left; needs a clear "still working" signal |

Rule: **acknowledge instantly, resolve when ready.** A button press dims at 0ms even if the network call takes 800ms. See [states-feedback.md](./states-feedback.md) for the loading side.

---

## 3. High-value microinteractions

These deliver the most felt quality per unit of effort. Build these well before anything fancy.

**Button press.** `scale: 0.97` on `:active`, ~120ms standard easing. Optionally a subtle shadow drop. On submit, transition straight into a loading state (spinner-in-button or label swap) — never let a tapped button look idle.

**Toggle / switch.** The thumb slides 150–200ms; track color cross-fades. A tiny overshoot (`cubic-bezier(0.34,1.56,0.64,1)`) sells the physical flip. State must be obvious at a glance without reading the label.

**Like / save / favorite.** The canonical delight moment. Icon pops (`scale 1 → 1.2 → 1`), color fills, optional one-shot particle burst. Make it **optimistic** — fill instantly on tap, reconcile with the server after. If the request fails, revert with a gentle shake + toast.

**Copy-to-clipboard confirmation.** Icon → checkmark morph + "Copied!" label, revert after ~2s. Without it, users click repeatedly unsure it worked. This is the highest ROI microinteraction on the web.

**Form field focus.** Border/ring animates to the accent color (120–150ms), label floats up if using floating labels, helper text appears. Focus state must be unmistakable for keyboard users (it doubles as the focus indicator — see accessibility).

**Hover reveals.** Secondary actions (edit/delete on a row, controls on a card) fade in on hover. Keep it ≤150ms. **Critical:** these must have a non-hover path on touch (see §7).

**Drag handles.** Cursor `grab` → `grabbing`, the item lifts (shadow + `scale: 1.02`), siblings slide to make room (`layout`/FLIP), a drop placeholder shows the target slot. Use spring physics so the dragged item tracks the finger/cursor naturally.

**Pull-to-refresh.** A spinner is revealed and rotates proportionally to pull distance, snaps to a spin past the threshold, then settles. Give haptic feedback at the threshold crossing on mobile.

**Optimistic UI updates.** Reflect the user's intent immediately (message appears as "sending", item appears in list, count increments) before the server confirms. Reconcile on response; roll back visibly on failure. This is the single biggest perceived-speed win in modern apps.

```
DON'T: wait for the 600ms POST before the like icon fills
DO:    fill instantly, sync in background, revert + toast on failure
```

---

## 4. Haptics on mobile

Used well, haptics make touch feel physical; overused, they're a battery-draining annoyance. Web has the Vibration API (Android-only, coarse) and richer native APIs (iOS `UIImpactFeedbackGenerator`, Capacitor/Expo Haptics).

| Event | Haptic | Notes |
|---|---|---|
| Toggle flip, segmented control | Light / selection | Confirms the discrete change |
| Successful action (save, send) | Light–medium impact | Pairs with visual success |
| Threshold crossed (pull-to-refresh, swipe-to-delete reveal) | Medium impact | Tells the user "you've armed it" |
| Error / rejected action | Notification-error (double buzz) | With a visual shake |
| Long-press activated | Medium impact | Confirms the mode change |
| Scrolling, hover, idle | **Nothing** | Never haptic on passive motion |

```ts
// Web — coarse, Android only, degrade silently elsewhere
if ("vibrate" in navigator) navigator.vibrate(10); // light tap, ~10ms
// Native (Expo): Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light)
```

Rules: keep durations short (8–15ms for taps), tie to *discrete* state changes, never to continuous motion, and never as the *only* feedback (many users disable system haptics).

---

## 5. Sound (rarely)

Default to **silent**. Sound is appropriate in a narrow set of cases: explicit messaging "sent/received" chimes, games, calls, alarms, and accessibility cues — and even then only when the surrounding context is sound-appropriate.

- Never autoplay sound on hover, page load, or generic clicks.
- Always respect system mute and provide an in-app off switch.
- Keep cues < 300ms, soft, non-startling, and consistent across the product.
- Sound should reinforce, never be the sole signal (the user may be muted or deaf).

---

## 6. Delight vs friction

The same animation can be delight or friction depending on **frequency** and **duration**.

| Delight | Friction |
|---|---|
| Happens rarely (first save, milestone, empty-state illustration) | Happens on every keystroke / every render |
| Fast (< 300ms) and skippable/interruptible | Blocks the user from proceeding |
| Reinforces a real state change | Decorative, answers no question |
| Optional polish on top of working UI | A prerequisite to using the feature |

Heuristic: **the more often an interaction repeats, the more invisible its animation should be.** A confetti burst on your one-per-year "subscription complete" is delight. The same on every "add to cart" is torture by the tenth time. Decay the delight: full effect the first time, subdued thereafter.

```
DON'T: 600ms celebratory animation on a button the user clicks 50×/session
DO:    120ms press feedback always; save the celebration for true milestones
```

---

## 7. Hover vs touch

**There is no hover on touch devices.** Any UI whose function depends on hover is broken on phones and tablets. Treat hover as progressive enhancement only.

| Desktop (hover) | Touch equivalent |
|---|---|
| Actions reveal on row hover | Always-visible action, an overflow `⋯` menu, or swipe actions |
| Tooltip on hover | Tap-to-reveal, an info `ⓘ` affordance, or inline helper text |
| Hover preview / peek | Long-press preview or a dedicated tap target |
| Hover to show drag handle | Persistent handle or long-press-to-drag |

Detect capability, don't sniff the OS:

```css
@media (hover: hover) and (pointer: fine) {
  .row .actions { opacity: 0; transition: opacity 150ms; }
  .row:hover .actions { opacity: 1; }
}
@media (hover: none) {
  .row .actions { opacity: 1; }   /* always visible on touch */
}
```

Also: touch targets ≥ 44×44px (iOS) / 48×48dp (Android). Hover affordances are often too small for fingers — size them up on touch. And never make a hover the *only* way to discover a critical action.

```
DON'T: delete button only appears on row hover (invisible & unreachable on mobile)
DO:    show it on touch, or move it into a tappable overflow menu
```

---

## 8. Personality without harming usability

Personality lives in the **edges**: empty states, success moments, error copy, loading messages, 404s, onboarding. It does **not** belong in the core task loop where it adds time.

- Add character to **moments**, not to **paths**. The checkout flow stays calm; the "order confirmed" screen can celebrate.
- Keep brand motion **consistent** — a signature easing curve or a recurring icon animation reads as craft; random flourishes read as noise.
- Never trade clarity for cuteness: a clever-but-ambiguous icon, a witty-but-vague error, or a delightful-but-slow transition all fail.
- Respect `prefers-reduced-motion` for every personality flourish (see [motion.md](./motion.md) §10) — these are exactly the effects reduced-motion users want gone.
- Test the flourish at high frequency. If it annoys you by the 5th run, subdue or remove it.

```
DON'T: a bouncy mascot animation between every step of a 6-step form
DO:    a calm form; one friendly illustration on the success screen
```

---

## 9. Common microinteractions reference

| Microinteraction | Trigger | Feedback | Timing | Haptic |
|---|---|---|---|---|
| Button press | tap / click | `scale: 0.97`, shadow dip | 100–120ms | light (optional) |
| Submit button | click | label → spinner, disable | instant ack; resolve when ready | — |
| Toggle / switch | tap | thumb slide + track color | 150–200ms, slight overshoot | selection |
| Checkbox | tap | check draws in, box fills | 150ms | light |
| Like / favorite | tap | icon pop `1→1.2→1`, color fill, optimistic | 200–250ms | light |
| Save / bookmark | tap | icon fill + brief "Saved" | 200ms; revert label 1.5s | light |
| Copy to clipboard | tap | icon→✓ morph + "Copied!" | revert at ~2000ms | light |
| Field focus | focus | ring/border accent, label float | 120–150ms | — |
| Field validation (live) | blur / debounced | inline success ✓ or error msg | 150ms fade | error: error-buzz |
| Hover reveal (desktop) | hover | secondary actions fade in | ≤150ms | n/a |
| Tooltip | hover/focus/tap | fade + 4–8px from anchor | in 120ms / out 80ms | — |
| Drag reorder | press+move | lift, shadow, siblings reflow | spring (stiff 400 / damp 30) | medium on pick-up |
| Swipe action (mobile) | swipe | action track reveals under row | tracks finger; snaps at threshold | medium at threshold |
| Pull-to-refresh | pull past threshold | spinner reveals, then spins | tracks pull; settle 300ms | medium at threshold |
| Accordion expand | tap | chevron rotate + content reveal | 200–280ms standard | — |
| Optimistic add (chat/list) | submit | item appears "pending", then confirms | instant; reconcile on response | light on success |
| Counter increment | event | number rolls/ticks up | 200–400ms | — |
| Form error on submit | submit fail | field shake + inline messages | shake 300ms (3 cycles) | error-buzz |

All timings assume the easing conventions in [motion.md](./motion.md) §3 and a feedback start within the 100ms budget.

---

## 10. Anti-patterns

```
DON'T: silent success — no visual change after a successful action
DON'T: feedback that never reverts (a "Copied!" that stays forever)
DON'T: hover-only critical actions (dead on touch)
DON'T: blocking, un-interruptible celebration animations
DON'T: haptics/sound on passive events (scroll, hover, load)
DON'T: the same heavy delight effect on a high-frequency action
DON'T: an animation that delays the user reaching the next step
DO:    acknowledge < 100ms, give clear state feedback, revert sensibly, keep it fast
```

---

## Agent checklist
- [ ] Design all four parts for each interaction: trigger, rules, feedback, loops/modes.
- [ ] Acknowledge every user action within 100ms, even if the result resolves later.
- [ ] Ship the high-ROI ones first: press, toggle, copy-confirm, field focus, optimistic updates.
- [ ] Make like/save/add optimistic — reflect intent instantly, reconcile after, revert visibly on failure.
- [ ] Ensure every "Copied!"/"Saved!" confirmation reverts after ~1.5–2s.
- [ ] Provide a non-hover path for every hover affordance; gate hover behind `@media (hover: hover)`.
- [ ] Keep touch targets ≥ 44×44px and never make a critical action hover-only.
- [ ] Use haptics only on discrete state changes and thresholds — never on passive motion; keep them 8–15ms.
- [ ] Default to silent; add sound only with consent, an off switch, and never as the sole signal.
- [ ] Put personality in edges (empty/success/error), not in the core task path; subdue effects that repeat often.
- [ ] Honor `prefers-reduced-motion` for every delight flourish.
