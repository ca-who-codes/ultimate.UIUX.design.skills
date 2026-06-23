# CLAUDE.md

This repository is **UI/UX Design Pro** — a knowledge base that gives you senior-level UI/UX design judgment.

**For any interface work** (designing, building, styling, or reviewing UI), follow the operating instructions in [`AGENTS.md`](AGENTS.md) and use the knowledge base under [`knowledge/`](knowledge/INDEX.md) via progressive disclosure.

Quick start:
1. Read [`knowledge/INDEX.md`](knowledge/INDEX.md) and [`knowledge/01-principles/decision-framework.md`](knowledge/01-principles/decision-framework.md).
2. Load only the references the task needs (use the index routing table).
3. Self-review against [`knowledge/05-quality/review-checklist.md`](knowledge/05-quality/review-checklist.md) before declaring done.

If this repo is installed as a Claude Code plugin, the **`ui-ux-pro` skill** activates automatically on UI/UX requests, and these commands are available: `/ui-build`, `/ui-review`, `/design-system`. Subagents: `ui-designer`, `frontend-implementer`, `design-reviewer`.

**Non-negotiables:** one primary action per screen · 8pt spacing · 16px body / ~1.5 line-height · contrast 4.5:1 · handle all 5 states (empty/loading/error/success/ideal) · semantic HTML + visible focus + keyboard · motion only on transform/opacity with reduced-motion fallback · restraint over decoration.
