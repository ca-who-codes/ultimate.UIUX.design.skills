---
description: Generate a complete, opinionated design system (tokens + primitives) for a project from a short brief — colors, type, spacing, radius, shadows, motion, wired for Tailwind v4 + shadcn.
argument-hint: "[brand/product brief, e.g. 'fintech dashboard, trustworthy, modern' or a brand color]"
---

Generate a cohesive design system for: **$ARGUMENTS**

1. Read `knowledge/02-foundations/design-tokens.md`, `color.md`, `typography.md`, and `layout-spacing.md`.
2. Derive the system from the brief (industry, mood, brand color if given). Make deliberate choices and justify them briefly.
3. Produce:
   - **Palette** — primary, neutral ramp (50–950), and semantic (success/warning/error/info), in OKLCH or hex, for **both light and dark** modes. Verify contrast (4.5:1 text, 3:1 UI).
   - **Typography** — font choice/stack, a modular type scale (rem + Tailwind classes), line-heights, weights.
   - **Spacing** — 8pt scale; **radius** scale; **shadow/elevation** scale (layered, not harsh).
   - **Motion** — standard durations and easing curves.
   - **Token output** — a copy-paste `:root` + `.dark` CSS custom-property block and a Tailwind v4 `@theme` block, plus shadcn-compatible variable names.
4. Show a tiny applied example (a button + card) using the tokens so the system is visibly coherent.
5. Note how to extend it (component tokens, theming) and verify against `knowledge/05-quality/review-checklist.md`.

Be opinionated and specific — real values, not placeholders.
