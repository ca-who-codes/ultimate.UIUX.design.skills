# CLAUDE.md

This repository is **Design Pro** — a knowledge base that gives you senior-level design judgment across **two tracks**: 🖥️ **interfaces** (screens, apps, components, design systems) and 🎨 **visual & marketing design** (decks/PPT, social carousels & posts, ads, A4/A3 posters, flyers, business cards, event banners, email/newsletters, infographics, brand kits).

**For any design work** (designing, building, styling, or reviewing anything visual), follow the operating instructions in [`AGENTS.md`](AGENTS.md) and use the knowledge base under [`knowledge/`](knowledge/INDEX.md) via progressive disclosure.

Quick start:
1. Read [`knowledge/INDEX.md`](knowledge/INDEX.md) and [`knowledge/01-principles/decision-framework.md`](knowledge/01-principles/decision-framework.md).
2. For a fixed-canvas asset (deck/social/print/email), also lock the canvas via [`knowledge/08-visual-composition/format-specs.md`](knowledge/08-visual-composition/format-specs.md) before designing.
3. Load only the references the task needs (use the index routing table).
4. Self-review against [`knowledge/05-quality/review-checklist.md`](knowledge/05-quality/review-checklist.md) before declaring done.

If this repo is installed as a Claude Code plugin, the **`ui-ux-pro` skill** activates automatically on design requests. Commands — interfaces: `/ui-build`, `/ui-review`, `/design-system`; visual/marketing: `/make-deck`, `/make-carousel`, `/make-creative`. Subagents: `ui-designer`, `frontend-implementer`, `visual-designer`, `design-reviewer`.

**Non-negotiables:** one focal point / primary action per artifact · 8pt spacing · legible type (16px web body / ~24pt slide / large-for-distance on posters / 14–16px email) · contrast 4.5:1 (incl. text over images) · interfaces handle all 5 states (empty/loading/error/success/ideal) · fixed-canvas assets set correct size/bleed/color space first · semantic HTML + visible focus + keyboard + alt text · motion only on transform/opacity with reduced-motion fallback · one brand family across a set · restraint over decoration.

**Scope note:** this repo owns the *visual design craft*. For marketing *copy, strategy, and distribution* (ad copy, SEO, CRO, deliverability), pair it with a marketing-skills library.
