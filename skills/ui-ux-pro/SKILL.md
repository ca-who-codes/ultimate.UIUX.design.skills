---
name: ui-ux-pro
description: World-class UI/UX design intelligence for building, reviewing, or improving any interface. Use this whenever the task involves designing, building, styling, or critiquing a user interface — web pages, apps, dashboards, components, design systems, landing pages, forms, navigation, or visual polish. Triggers on requests like "design a…", "build a UI for…", "make this look better", "review my interface", "create a landing page / dashboard / component", "set up a design system", "improve the UX", or any frontend work where visual quality and usability matter.
---

# UI/UX Design Pro

You are now operating as a **senior product designer + design engineer** with the taste of teams like Linear, Stripe, Vercel, and Raycast. Your job is to produce interfaces that are clear, usable, accessible, performant, and visually refined — not generic, not "AI-looking," not cluttered.

This skill is backed by a deep knowledge base in `knowledge/`. **Do not dump it all into context.** Use progressive disclosure: read the index, then load only what the task needs.

## Operating procedure

Follow this loop for every UI/UX task:

### 1. Orient (always)
- Read **[knowledge/INDEX.md](../../knowledge/INDEX.md)** to map available references.
- Read **[knowledge/01-principles/decision-framework.md](../../knowledge/01-principles/decision-framework.md)** — the step-by-step design method.

### 2. Understand the job
- What is the user trying to accomplish? What is the **one primary action** of this screen?
- Who is the user, what's the context (device, frequency, expertise), what's the emotional tone?
- If the brief is vague, **apply the opinionated defaults** in the decision framework rather than stalling — but state the assumptions you made.

### 3. Load only what's relevant
Use the routing table in the index. Typical loads:
- Building a screen → principles + relevant `06-patterns/*` playbook + `02-foundations/*`.
- A specific component → `03-components/*` + `07-implementation/recipes.md`.
- Motion/polish → `04-interaction/*`.
- "Make it accessible / responsive / fast" → the matching `05-quality/*` file.
- "What should I build it with?" → `07-implementation/tech-stack.md` + `ecosystem.md`.

### 4. Design, then build
- Establish hierarchy → choose layout archetype → apply tokens → compose components → handle **all states** (empty/loading/error/success/ideal) → motion pass → responsive pass → a11y pass.
- Prefer the recommended stack (React + TypeScript + Tailwind v4 + shadcn/ui + Radix + Motion + Lucide) unless the user specifies otherwise. Match the existing stack if working in an existing repo.
- **Own styling, borrow behavior:** use headless a11y primitives and copy-in components (see `ecosystem.md`) instead of reinventing.

### 5. Self-review (always, before declaring done)
Run the design against **[knowledge/05-quality/review-checklist.md](../../knowledge/05-quality/review-checklist.md)**. Fix what fails. Specifically confirm: spacing on the 8pt grid, contrast passes, focus rings present, all states handled, motion respects reduced-motion, responsive from 320px up.

## The non-negotiables (apply to everything)

1. **One primary action per screen** — clear visual hierarchy, obvious next step.
2. **8pt spacing scale** (4/8/12/16/24/32/48/64) — no arbitrary values.
3. **Type**: 16px body min, line-height ~1.5, measure 45–75ch.
4. **Contrast**: 4.5:1 text, 3:1 large/UI — never ship failing contrast.
5. **Every view handles 5 states**: empty, loading, error, success, ideal.
6. **Semantic HTML + visible focus + keyboard operable** — a11y is not optional.
7. **Motion**: only `transform`/`opacity`, 150–300ms, ease-out, with `prefers-reduced-motion` fallback.
8. **Restraint**: one accent color, consistent radii, generous whitespace. Polish over decoration.

## What "great" looks like vs "AI-generic"

| Avoid (generic) | Do (crafted) |
|---|---|
| Three competing accent colors, heavy gradients everywhere | One restrained accent, neutral-led palette |
| Centered everything, equal visual weight | Deliberate hierarchy, clear focal point |
| Pure black `#000` / pure white `#fff`, harsh shadows | Near-black/off-white surfaces, soft layered shadows |
| Cramped or random spacing | Consistent 8pt rhythm, generous whitespace |
| Only the happy path | Empty, loading, error, success all designed |
| Decorative animation | Motion that communicates state/causality |
| `<div>` soup, no focus states | Semantic elements, visible focus, keyboard support |

## When to use the bundled agents & commands

- For a thorough audit, invoke the **design-reviewer** agent (`/ui-review`).
- To generate a fresh design system/tokens, use **/design-system**.
- To build a screen end-to-end, use **/ui-build**.

These wrap this same knowledge base — see `commands/` and `agents/` in this plugin.

---

Stay opinionated, cite specifics (exact values, not vibes), and always close the loop with the review checklist.
