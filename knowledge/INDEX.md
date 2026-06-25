# Knowledge Base Index — Design Pro

> Purpose: The map of the entire knowledge base. Load this first, then read only the files relevant to the task (progressive disclosure — don't load everything).

**How to use this index:** Find the situation in the routing table, open the linked file(s), apply, then self-review against [05-quality/review-checklist.md](05-quality/review-checklist.md).

This repo covers **two tracks** that share the same foundations and quality bar:

- 🖥️ **Track A — Interfaces** (screens, apps, web, components) → sections **01–07**
- 🎨 **Track B — Visual & Marketing Design** (decks, social, print, email, brand — fixed-canvas) → sections **08–13**

**Shared by both:** `01-principles` (judgment), `02-foundations` (color/type/spacing/tokens), `05-quality` (contrast, accessibility, the review checklist).

---

## Reading order for a fresh task

1. **Always start:** [01-principles/decision-framework.md](01-principles/decision-framework.md) — the step-by-step method for designing anything.
2. **Visual/marketing assets only:** also set the canvas first via [08-visual-composition/format-specs.md](08-visual-composition/format-specs.md).
3. **Then load by need** using the routing table below.
4. **Always finish:** [05-quality/review-checklist.md](05-quality/review-checklist.md) — QA before declaring done.

---

## Routing table — "If the task is… read…"

### 🖥️ Track A — Interfaces / screens / apps
| If the task involves… | Read |
|---|---|
| Any new screen / "design this" / vague brief | [01-principles/decision-framework.md](01-principles/decision-framework.md) + [01-principles/design-principles.md](01-principles/design-principles.md) |
| Buttons, modals, cards, tabs, any component | [03-components/components.md](03-components/components.md) |
| Forms, inputs, validation | [03-components/forms.md](03-components/forms.md) |
| Navigation, menus, IA, mobile nav | [03-components/navigation.md](03-components/navigation.md) |
| Tables, charts, KPIs, lists, data UI | [03-components/data-display.md](03-components/data-display.md) |
| Animation, transitions, motion | [04-interaction/motion.md](04-interaction/motion.md) |
| Hover/press feedback, delight, polish | [04-interaction/microinteractions.md](04-interaction/microinteractions.md) |
| Empty / loading / error / success states | [04-interaction/states-feedback.md](04-interaction/states-feedback.md) |
| Mobile / responsive / breakpoints | [05-quality/responsive.md](05-quality/responsive.md) |
| Speed, Core Web Vitals, perceived perf | [05-quality/performance.md](05-quality/performance.md) |
| Landing / marketing page (web) | [06-patterns/landing-marketing.md](06-patterns/landing-marketing.md) |
| Dashboard / analytics / admin | [06-patterns/dashboards.md](06-patterns/dashboards.md) |
| Login / signup / onboarding | [06-patterns/auth-onboarding.md](06-patterns/auth-onboarding.md) |
| Pricing / product / cart / checkout | [06-patterns/pricing-ecommerce.md](06-patterns/pricing-ecommerce.md) |
| Choosing a stack / how to build it | [07-implementation/tech-stack.md](07-implementation/tech-stack.md) |
| Copy-paste component code | [07-implementation/recipes.md](07-implementation/recipes.md) |
| Which UI library / 3D / video / effects | [07-implementation/ecosystem.md](07-implementation/ecosystem.md) |

### 🎨 Track B — Visual & marketing design (fixed-canvas)
| If the task involves… | Read |
|---|---|
| **What size / aspect / DPI / bleed?** (any asset) | [08-visual-composition/format-specs.md](08-visual-composition/format-specs.md) |
| Arranging a static canvas, focal point, balance | [08-visual-composition/composition.md](08-visual-composition/composition.md) |
| Photos, illustration, icons, text-over-image, AI imagery | [08-visual-composition/imagery-and-icons.md](08-visual-composition/imagery-and-icons.md) |
| Brand kit / consistency across a set of assets | [08-visual-composition/brand-systems.md](08-visual-composition/brand-systems.md) |
| Presentation / deck / PPT / pitch / slides | [09-presentations/decks.md](09-presentations/decks.md) |
| Social carousel (IG / LinkedIn / TikTok) | [10-social-creatives/carousels.md](10-social-creatives/carousels.md) |
| Single social post, story, thumbnail, ad creative | [10-social-creatives/social-posts.md](10-social-creatives/social-posts.md) |
| Poster, flyer, brochure, business card, banner, event signage (print) | [11-print-and-events/print-posters.md](11-print-and-events/print-posters.md) |
| Email / newsletter design | [12-email-design/email.md](12-email-design/email.md) |
| How to render/produce it (PNG/PDF/PPTX/MP4/Express) | [13-production/production-and-tools.md](13-production/production-and-tools.md) |

### Shared (both tracks)
| If the task involves… | Read |
|---|---|
| Choosing colors / dark mode / contrast | [02-foundations/color.md](02-foundations/color.md) |
| Fonts, sizes, hierarchy, readability | [02-foundations/typography.md](02-foundations/typography.md) |
| Spacing, grids, layout, alignment | [02-foundations/layout-spacing.md](02-foundations/layout-spacing.md) |
| Tokens / theming / design system | [02-foundations/design-tokens.md](02-foundations/design-tokens.md) |
| Accessibility / a11y / WCAG / contrast | [05-quality/accessibility.md](05-quality/accessibility.md) |
| Final QA / "is this good?" | [05-quality/review-checklist.md](05-quality/review-checklist.md) |

---

## Full directory map

```
knowledge/
├── INDEX.md                 ← start here: map + routing table
│
│  ── shared foundations ──
├── 01-principles/           Laws of UX, heuristics, Gestalt + a decision framework
├── 02-foundations/          color · typography · layout/spacing · design tokens
├── 05-quality/              accessibility · responsive · performance · review checklist
│
│  ── Track A: interfaces ──
├── 03-components/           components · forms · navigation · data display
├── 04-interaction/          motion · microinteractions · states & feedback
├── 06-patterns/             landing · dashboards · auth/onboarding · pricing/ecommerce
├── 07-implementation/       tech stack · recipes (copy-paste code) · ecosystem
│
│  ── Track B: visual & marketing design ──
├── 08-visual-composition/   format-specs (dimensions cheat-sheet) · composition · imagery & icons · brand systems
├── 09-presentations/        decks (PPT / slides / pitch decks)
├── 10-social-creatives/     carousels · social posts / stories / thumbnails / ads
├── 11-print-and-events/     posters · flyers · brochures · cards · banners · event signage
├── 12-email-design/         email & newsletter design
└── 13-production/           production-and-tools (HTML→PNG/PDF/PPTX/MP4, Express/Canva, Remotion)
```

---

## The non-negotiables (apply to everything)

These run through every file; internalize them as defaults:

1. **One primary message/action per artifact.** One focal point. Make the most important thing the most prominent.
2. **8pt spacing scale** (4/8/12/16/24/32/48/64). No arbitrary values.
3. **Type**: legible minimums — 16px web body, ~24pt slide body, large for posters (read at distance), 14–16px email. Measure 45–75ch for prose.
4. **Contrast 4.5:1 text, 3:1 large/UI** — including text over images.
5. **States & completeness:** interfaces handle empty/loading/error/success/ideal; fixed-canvas assets are set at the right size/bleed/color space *before* designing.
6. **Semantic HTML + visible focus + keyboard** for interfaces; **accessible contrast + alt text** for all assets.
7. **Motion** (where it applies): only `transform`/`opacity`, 150–300ms, ease-out, with `prefers-reduced-motion` fallback.
8. **Restraint & system thinking:** one accent color, consistent radii/grids, generous whitespace, one brand family across a set. Polish over decoration.

When in doubt, optimize for clarity over cleverness.
