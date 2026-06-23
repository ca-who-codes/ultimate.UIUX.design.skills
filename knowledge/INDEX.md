# Knowledge Base Index — UI/UX Design Pro

> Purpose: The map of the entire knowledge base. Load this first, then read only the files relevant to the task (progressive disclosure — don't load everything).

**How to use this index:** Find the situation in the routing table, open the linked file(s), apply, then self-review against [05-quality/review-checklist.md](05-quality/review-checklist.md).

---

## Reading order for a fresh task

1. **Always start:** [01-principles/decision-framework.md](01-principles/decision-framework.md) — the step-by-step method for designing any screen.
2. **Then load by need** using the routing table below.
3. **Always finish:** [05-quality/review-checklist.md](05-quality/review-checklist.md) — QA before declaring done.

---

## Routing table — "If the task is… read…"

| If the task involves… | Read |
|---|---|
| Any new screen / "design this" / vague brief | [01-principles/decision-framework.md](01-principles/decision-framework.md) + [01-principles/design-principles.md](01-principles/design-principles.md) |
| Choosing colors / dark mode / contrast | [02-foundations/color.md](02-foundations/color.md) |
| Fonts, sizes, hierarchy, readability | [02-foundations/typography.md](02-foundations/typography.md) |
| Spacing, grids, page layout, alignment | [02-foundations/layout-spacing.md](02-foundations/layout-spacing.md) |
| Setting up tokens / theming / a design system | [02-foundations/design-tokens.md](02-foundations/design-tokens.md) |
| Buttons, modals, cards, tabs, any component | [03-components/components.md](03-components/components.md) |
| Forms, inputs, validation | [03-components/forms.md](03-components/forms.md) |
| Navigation, menus, IA, mobile nav | [03-components/navigation.md](03-components/navigation.md) |
| Tables, charts, KPIs, lists, data UI | [03-components/data-display.md](03-components/data-display.md) |
| Animation, transitions, motion | [04-interaction/motion.md](04-interaction/motion.md) |
| Hover/press feedback, delight, polish | [04-interaction/microinteractions.md](04-interaction/microinteractions.md) |
| Empty / loading / error / success states | [04-interaction/states-feedback.md](04-interaction/states-feedback.md) |
| Accessibility / a11y / WCAG / keyboard | [05-quality/accessibility.md](05-quality/accessibility.md) |
| Mobile / responsive / breakpoints | [05-quality/responsive.md](05-quality/responsive.md) |
| Speed, Core Web Vitals, perceived perf | [05-quality/performance.md](05-quality/performance.md) |
| Final QA / "is this good?" | [05-quality/review-checklist.md](05-quality/review-checklist.md) |
| Landing / marketing / homepage | [06-patterns/landing-marketing.md](06-patterns/landing-marketing.md) |
| Dashboard / analytics / admin | [06-patterns/dashboards.md](06-patterns/dashboards.md) |
| Login / signup / onboarding | [06-patterns/auth-onboarding.md](06-patterns/auth-onboarding.md) |
| Pricing / product / cart / checkout | [06-patterns/pricing-ecommerce.md](06-patterns/pricing-ecommerce.md) |
| Choosing a stack / how to build it | [07-implementation/tech-stack.md](07-implementation/tech-stack.md) |
| Copy-paste component code | [07-implementation/recipes.md](07-implementation/recipes.md) |
| Which library to use / 3D / video / effects | [07-implementation/ecosystem.md](07-implementation/ecosystem.md) |

---

## Full directory map

```
knowledge/
├── 01-principles/         The "why" — judgment that drives every decision
│   ├── design-principles.md      Nielsen heuristics, Laws of UX, Gestalt, hierarchy
│   └── decision-framework.md     Step-by-step method + opinionated defaults
├── 02-foundations/        The visual language
│   ├── color.md                  Palettes, OKLCH, contrast, dark mode
│   ├── typography.md             Type scale, line-height, measure, pairing
│   ├── layout-spacing.md         8pt grid, breakpoints, grid vs flex, archetypes
│   └── design-tokens.md          3-tier tokens, CSS vars, Tailwind v4 + shadcn wiring
├── 03-components/         The building blocks
│   ├── components.md             Anatomy + states for every core component
│   ├── forms.md                  Layout, validation, error UX, a11y
│   ├── navigation.md             Nav patterns, IA, mobile nav
│   └── data-display.md           Tables, charts, KPIs, lists, empty states
├── 04-interaction/        How it moves & responds
│   ├── motion.md                 Durations, easing, what-to-animate, reduced-motion
│   ├── microinteractions.md      Trigger→feedback, delight without friction
│   └── states-feedback.md        Empty/loading/error/success/partial lifecycle
├── 05-quality/            The bar
│   ├── accessibility.md          WCAG 2.2 AA, semantics, ARIA, keyboard, focus
│   ├── responsive.md             Mobile-first, breakpoints, touch, fluid
│   ├── performance.md            Core Web Vitals, perceived perf, no jank
│   └── review-checklist.md       Master QA checklist (run before "done")
├── 06-patterns/           Page-level playbooks
│   ├── landing-marketing.md      High-converting marketing pages
│   ├── dashboards.md             Data/app dashboards
│   ├── auth-onboarding.md        Signup/login/onboarding
│   └── pricing-ecommerce.md      Pricing tables, PDP, cart, checkout
└── 07-implementation/     How to ship it
    ├── tech-stack.md             React + Tailwind v4 + shadcn + Radix + Motion
    ├── recipes.md                Copy-paste accessible component code
    └── ecosystem.md              Best libraries, motion, 3D/video, inspiration
```

---

## The non-negotiables (apply to everything)

These appear across many files; internalize them as defaults:

1. **One primary action per screen.** Make the most important thing the most prominent.
2. **8pt spacing scale.** 4, 8, 12, 16, 24, 32, 48, 64. No arbitrary values.
3. **Type: 16px body min, line-height ~1.5, measure 45–75ch.**
4. **Contrast: 4.5:1 text, 3:1 large/UI.** Never ship failing contrast.
5. **Every view handles 5 states:** empty, loading, error, success, ideal. Agents forget this — don't.
6. **Semantic HTML + visible focus + keyboard operable.** Accessibility is not optional.
7. **Animate only transform/opacity, 150–300ms, ease-out, with `prefers-reduced-motion` fallback.**
8. **Restraint.** One accent color, consistent radii, generous whitespace. Polish > decoration.

When in doubt, optimize for clarity over cleverness.
