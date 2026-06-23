---
description: Design and build a screen, flow, or component end-to-end using the UI/UX Design Pro knowledge base and the modern recommended stack.
argument-hint: "[what to build, e.g. 'a pricing page for a B2B SaaS' or 'a settings form']"
---

Design and build: **$ARGUMENTS**

Work through the full UI/UX Design Pro loop:

1. **Orient** — read `knowledge/INDEX.md` and `knowledge/01-principles/decision-framework.md`.
2. **Understand** — identify the job-to-be-done and the single primary action. If the brief is underspecified, apply the framework's opinionated defaults and state your assumptions; ask only if a decision genuinely blocks you.
3. **Load** the relevant references via the index routing table (the matching `06-patterns/*` playbook, `02-foundations/*`, `03-components/*`, and `07-implementation/recipes.md`).
4. **Design then build** — hierarchy → layout → tokens → components → **all five states** (empty/loading/error/success/ideal) → motion → responsive → accessibility. Match the existing repo's stack if there is one; otherwise default to React + TypeScript + Tailwind v4 + shadcn/ui + Radix + Motion + Lucide. Own styling, borrow behavior from accessible primitives.
5. **Self-review** — run the result against `knowledge/05-quality/review-checklist.md`, fix what fails, and report how it holds up (contrast, focus, states, responsive, reduced-motion).

Deliver complete, copy-pasteable code (with imports and any deps to install), plus a short note on assumptions and what the user should verify on real devices.
