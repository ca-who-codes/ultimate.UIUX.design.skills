# Design Principles

> Purpose: The canonical mental models — usability heuristics, Laws of UX, Gestalt, hierarchy, cognitive load — that every UI decision in this repo defers to.

**When to read this:** Read before designing or critiquing any screen, and whenever a layout "feels off" but you can't name why. This file gives you the vocabulary and the rule to cite. For the step-by-step process that operationalizes these principles, read [`decision-framework.md`](./decision-framework.md).

---

## How to use this file

Each principle below is paired with a **concrete UI implication** — the thing you actually change in markup or CSS. Principles without an implication are trivia; ignore them. When two principles conflict (they will), resolve in this order: **accessibility > clarity > consistency > aesthetics > novelty**. Never sacrifice the higher tier for the lower one.

---

## 1. Nielsen's 10 Usability Heuristics

The oldest, most battle-tested checklist in the field (Jakob Nielsen, 1994). Each has a one-line "apply it."

| # | Heuristic | Apply it |
|---|-----------|----------|
| 1 | **Visibility of system status** | Show state within **400ms** of any action — spinner, skeleton, progress bar, optimistic update. Never leave the user guessing. |
| 2 | **Match between system and real world** | Use the user's words, not your DB schema. "Trash" not "soft-delete"; "Send" not "Submit POST". |
| 3 | **User control and freedom** | Every destructive or navigational action needs an exit: Undo, Cancel, Back, Esc-to-close. Provide Undo over confirm dialogs where possible. |
| 4 | **Consistency and standards** | Reuse the same component for the same job everywhere. A primary button looks identical on every screen. Follow platform conventions (see Jakob's Law). |
| 5 | **Error prevention** | Disable invalid actions, constrain inputs (date pickers over free text), confirm only irreversible ones. Prevention beats good error messages. |
| 6 | **Recognition rather than recall** | Show options instead of making users remember them. Visible labels, autocomplete, recently-used lists. Don't hide actions behind memorized gestures. |
| 7 | **Flexibility and efficiency of use** | Add accelerators (keyboard shortcuts, bulk actions, saved views) that experts use and novices ignore. Progressive, not mandatory. |
| 8 | **Aesthetic and minimalist design** | Every element competes for attention. If it doesn't serve the primary task on this view, cut it or defer it (see Progressive Disclosure). |
| 9 | **Help users recognize, diagnose, recover from errors** | Plain-language errors: what happened, why, how to fix. "Card declined — check the number or try another card," not "Error 402." |
| 10 | **Help and documentation** | Make help findable and contextual (tooltips, inline hints, empty-state guidance). Best help is a UI so clear it isn't needed. |

**Agent rule:** Run a screen against these 10 before declaring it done. A violation of #1, #3, #5, or #9 is a blocking bug, not a polish item.

---

## 2. Laws of UX

Named, evidence-based laws. Each includes the concrete UI implication — what you build differently because of it.

### Hick's Law
*Decision time increases logarithmically with the number and complexity of choices.*
- **Implication:** Cap top-level navigation at **5–7 items**. Break long forms into steps. Use a single, obvious primary action per view. If a menu has >9 items, group them or add search.
- **Formula:** `RT = a + b·log₂(n+1)`. Doubling options doesn't double decision time, but more options always slow it — and add abandonment risk.

### Fitts's Law
*Time to acquire a target is a function of distance to it and its size.*
- **Implication:** Make tap/click targets **≥44×44px** (iOS) / **48×48dp** (Android), ideally **48px**. Put primary actions near where the cursor/thumb already is. Bottom-anchor mobile CTAs (thumb zone). Screen edges and corners are "infinitely large" targets — use them for high-frequency actions.
- **Formula:** `MT = a + b·log₂(D/W + 1)` — bigger `W` (target width) and smaller `D` (distance) both reduce time.

### Jakob's Law
*Users spend most of their time on other sites, so they expect yours to work the same way.*
- **Implication:** Don't reinvent conventions. Logo top-left links home. Cart top-right. Search has a magnifier icon. Underlined blue-ish text is a link. Innovate on value, not on where the close button lives.

### Miller's Law
*The average person holds ~7 (±2) items in working memory.*
- **Implication:** Chunk content. Phone numbers as `555 867 5309`, not `5558675309`. Group form fields into labeled sections of ≤5. Don't make users compare more than ~4 plans/options side by side. (Note: modern reading puts the reliable limit closer to **4** for unfamiliar items — design for 4, not 7.)

### Tesler's Law (Conservation of Complexity)
*Every system has irreducible complexity; the only question is who absorbs it.*
- **Implication:** Absorb complexity in the product, not the user. Auto-detect card type from the number. Parse "tomorrow 3pm" instead of forcing a date+time picker. Smart defaults over required fields. If you can compute it, don't ask for it.

### Postel's Law (Robustness Principle)
*Be liberal in what you accept, conservative in what you send.*
- **Implication:** Accept input in any reasonable format — phone numbers with or without spaces/dashes, dates as "12/3" or "Dec 3", pasted values with stray whitespace. Normalize silently. Output one clean canonical format.

### Doherty Threshold
*Productivity soars when system response is under **400ms**.*
- **Implication:** Target **<400ms** for any interaction feedback; **<100ms** feels instant. If real work takes longer, show progress within 400ms (skeleton/spinner) and use optimistic UI. Above ~1s, narrate ("Uploading 2 of 5"). Above ~10s, allow the user to leave and be notified.

### Aesthetic-Usability Effect
*Users perceive more attractive designs as more usable — and forgive minor flaws.*
- **Implication:** Polish (consistent spacing, type, color, motion) buys you trust and tolerance. It is not optional decoration — it measurably improves perceived and actual task success. But it never excuses a broken flow.

### Von Restorff Effect (Isolation Effect)
*The item that differs most is the one remembered.*
- **Implication:** Give the primary CTA the only saturated/filled treatment on the view; everything else is secondary/ghost/text. One highlighted plan in a pricing table. Don't make five things "pop" — then nothing does.

### Serial Position Effect
*People best recall the first (primacy) and last (recency) items in a list.*
- **Implication:** Put the most important nav items, list entries, or onboarding steps **first and last**. Bury the least important in the middle. Most-used toolbar actions go at the ends.

### Peak-End Rule
*People judge an experience by its most intense moment and its end, not the average.*
- **Implication:** Engineer a delightful peak (a smooth success animation, a thoughtful empty state) and a strong ending (clear confirmation, "what's next"). A great success screen outweighs a mediocre middle.

### Zeigarnik Effect
*Unfinished tasks occupy memory more than finished ones.*
- **Implication:** Use progress indicators, checklists, and completion meters ("Profile 60% complete") to create productive tension that pulls users to finish. Conversely, mark things "Done" decisively to release that tension.

---

## 3. Gestalt Principles

How the eye groups visual elements before the brain reads them. These govern layout grouping more than any spacing token.

| Principle | What it says | Apply it |
|-----------|--------------|----------|
| **Proximity** | Elements close together are perceived as a group. | Spacing communicates relationship. Use a tight gap (8–12px) inside a group, a large gap (32–48px) between groups. Proximity beats borders for grouping — reach for whitespace first. |
| **Similarity** | Elements that look alike are perceived as related/same type. | Make all clickable things share a visual signature (color, underline, shape). Don't style two unrelated items identically — it implies a relationship that isn't there. |
| **Closure** | The eye completes incomplete shapes. | You don't need full borders/boxes to define regions. A few aligned edges and consistent spacing imply a card. Use this to reduce visual clutter. |
| **Continuity** | The eye follows lines and curves; aligned elements read as connected. | Align everything to a grid. A clean vertical edge guides the eye down a form. Misalignment breaks the implied line and reads as an error. |
| **Common Region** | Elements inside a shared boundary are perceived as a group. | A card, a tinted background panel, or a bordered section binds its contents — even overriding proximity. Use sparingly; over-boxing creates visual noise. |
| **Figure/Ground** | The eye separates a foreground object from its background. | Ensure clear separation: shadow, elevation, contrast, or blur-backdrop for modals/overlays. Ambiguous figure/ground (low-contrast cards on busy backgrounds) feels broken. |

**Agent rule:** Before adding a border or box, ask "can proximity or alignment do this job?" Prefer whitespace and alignment over lines. Reserve borders for genuine containment.

---

## 4. Visual Hierarchy

Hierarchy = the deliberate ordering of attention. You have exactly five tools. Use the *fewest* needed.

| Tool | How to wield it | Default move |
|------|-----------------|--------------|
| **Size** | Bigger = more important. | Use a **modular type scale** (1.2–1.25 ratio). H1 ≈ 2–3× body. Don't make everything big. |
| **Weight** | Bolder = more important. | Body at **400**; emphasis at **600–700**. Avoid going below 400 for anything that must be read. |
| **Color** | Higher contrast / more saturation = more important. | One accent color carries the primary action. Mute everything secondary to a gray ramp. |
| **Spacing** | More surrounding space = more important and more isolated. | Give the hero/CTA breathing room. Whitespace is the cheapest emphasis tool you have. |
| **Position** | Top and left (LTR) get seen first; center-of-gaze gets attention. | Put the single most important element top-left or center per the F/Z scan pattern. |

**The hierarchy test:** A screen should have a clear **1 → 2 → 3** reading order. If two elements compete for "first," the design has no hierarchy. Demote one.

**Scan patterns:** Text-heavy pages get an **F-pattern** (two horizontal sweeps then a vertical scan down the left). Sparse/visual pages get a **Z-pattern**. Place key content along these paths; don't fight them.

---

## 5. Cognitive Load Reduction

Every element, choice, and word costs the user mental effort. Three load types:
- **Intrinsic** — inherent task difficulty. Reduce by chunking and progressive disclosure.
- **Extraneous** — load from *bad design* (clutter, inconsistency, jargon). Eliminate entirely; this is your job.
- **Germane** — productive load that builds understanding. Preserve it.

**Concrete reductions:**
- Default values for every field that has a sensible default.
- Show, don't make recall (recognition > recall, Heuristic #6).
- One primary action per view. Secondary actions visually recede.
- Replace free-text with constrained inputs (pickers, toggles, segmented controls) where the value space is small.
- Inline validation at field-blur, not a wall of errors at submit.
- Keep line length at **45–75 characters** (~66 ideal) so reading doesn't tax the eye.

---

## 6. Progressive Disclosure

*Show only what's needed now; reveal advanced/rare options on demand.*

- **Apply it:** Default to the 80% path. Hide the 20% behind "Advanced settings," "More options," accordions, or a second step. Two-tier menus, "Show more," and wizard steps are all progressive disclosure.
- **Don't:** Dump every option on one screen "for power users." Power users learn shortcuts; novices drown in choices (Hick's Law).
- **Don't over-hide:** If a setting is needed often, exposing it costs less than the click. Disclose the rare, surface the common.

---

## 7. "Don't Make Me Think" (Krug's Law)

*The interface should be self-evident — usable without conscious thought.*

- **Apply it:** A user landing cold should answer, within ~3 seconds: *What is this? What can I do here? Where do I start?* If they have to think, you've failed.
- **Tactics:** Obvious clickable affordances. Conventional layouts (Jakob's Law). Descriptive labels ("Download invoice PDF" not "Click here"). No ambiguous icons without labels. Self-evident > self-explanatory > requires-explanation — always aim for the first tier.

---

## 8. Signifiers & Affordances

(Don Norman.) An **affordance** is what an object lets you do; a **signifier** is the perceivable cue that tells you it's there.

- A button *affords* clicking; its raised look / fill / cursor change is the *signifier*.
- **Apply it:** Make interactivity perceivable. Clickable things must look clickable — fill, border, underline, shadow, or `cursor: pointer`. Don't strip all signifiers in pursuit of "clean" (the flat-design trap: buttons indistinguishable from text).
- **False affordances are bugs:** non-interactive things styled like buttons, or text that looks like a link but isn't. Every signifier must be honest.
- **State signifiers:** hover, focus, active, disabled, loading, and selected each need a distinct, perceivable treatment.

```css
/* Honest button signifiers */
.btn { cursor: pointer; }
.btn:hover  { /* perceptible change: lighten/darken ~8% */ }
.btn:focus-visible { outline: 2px solid var(--focus-ring); outline-offset: 2px; }
.btn:active { transform: translateY(1px); }
.btn:disabled { opacity: .5; cursor: not-allowed; }
```

---

## 9. Feedback Loops

Every user action must produce a perceivable system reaction. No silent actions, ever.

| Action | Required feedback | Timing |
|--------|-------------------|--------|
| Button press | Visual state change (`:active`) | Immediate (<100ms) |
| Form submit | Loading state on the button, then success/error | Loading <400ms; result ASAP |
| Async load | Skeleton or spinner | Appears <400ms |
| Destructive op | Confirmation OR optimistic + Undo toast | Undo visible ≥5s |
| Long process | Progress bar + step text | Updates every <1s |
| Validation | Inline message at field | On blur / debounce |

**Loop anatomy:** *trigger → feedback → result → next-action cue.* If any link is missing, the user feels lost. The "next-action cue" is the most forgotten — after success, always answer "what now?"

---

## Cross-references

- Operationalize all of this with [`decision-framework.md`](./decision-framework.md) — the per-screen workflow.
- Color contrast and semantic color → `../02-foundations/color.md`.
- Type scale and line length → `../02-foundations/typography.md`.
- Spacing scale (8px system) → `../02-foundations/spacing.md`.
- Motion durations and easing → `../06-motion/motion-principles.md`.
- Component states and patterns → `../04-components/`.
- Full WCAG pass → `../05-accessibility/wcag.md`.

---

## Agent checklist

- [ ] Run the screen against Nielsen's 10; treat violations of #1/#3/#5/#9 as blocking bugs.
- [ ] Confirm exactly one primary action per view; everything else visually recedes (Von Restorff).
- [ ] Verify a clear 1→2→3 reading order using only as many hierarchy tools as needed.
- [ ] Check every interactive target is ≥44–48px and reachable in the thumb/cursor zone (Fitts's).
- [ ] Cap primary navigation at 5–7 items; chunk anything longer (Hick's, Miller's).
- [ ] Ensure every action returns feedback within 400ms (Doherty); no silent actions.
- [ ] Group with proximity and alignment before reaching for borders or boxes (Gestalt).
- [ ] Confirm clickable things look clickable and non-clickable things don't (honest signifiers).
- [ ] Provide an exit/undo for every destructive or navigational action (Heuristic #3).
- [ ] Absorb complexity in the system, not the user — default, parse, or compute instead of asking (Tesler's).
- [ ] Engineer a strong success/end state with a clear "what's next" (Peak-End).
- [ ] Replace jargon with the user's real-world words (Heuristic #2, Jakob's).
