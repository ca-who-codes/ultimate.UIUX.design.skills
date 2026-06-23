# Design Brief Template

> Copy this, fill it in, and hand it to the agent (or `/ui-build`). The more you specify, the less the agent has to assume. Anything left blank, the agent fills with the opinionated defaults in `knowledge/01-principles/decision-framework.md` — and will state the assumptions it made.

## 1. What & why
- **What are we building?** (e.g., pricing page, settings form, analytics dashboard)
- **Primary goal of the screen:** the one thing the user should be able to do.
- **Primary action / CTA:** the single most important button or outcome.
- **Success metric (optional):** how we'd know it works (conversion, task time, etc.).

## 2. Audience & context
- **Who uses it?** (expertise level, role)
- **Where/when?** (mobile on the go, desktop at work, both)
- **Frequency:** first-time vs daily power user.
- **Emotional tone:** e.g., calm/trustworthy, energetic, premium, playful, serious.

## 3. Brand & visual direction
- **Brand color(s) / existing tokens:** (hex/OKLCH, or "none — pick for me")
- **Reference products to match in quality:** (e.g., Linear, Stripe, Notion)
- **Light / dark / both:**
- **Must-keep constraints:** (existing components, logo, fonts)

## 4. Content
- **Key sections / fields / data:** what must appear.
- **Real copy (if available):** headlines, labels — or "write placeholder copy."
- **Imagery / illustration / 3D / video:** any, and source.

## 5. Scope & stack
- **Build it, or just design/spec it?**
- **Stack:** (existing repo's stack, or "use the recommended default")
- **States to handle:** confirm empty / loading / error / success / ideal are all in scope (they should be).
- **Responsive targets:** mobile-first 320px → desktop (default), or specific.
- **Accessibility bar:** WCAG 2.2 AA (default).

## 6. Out of scope / notes
- Anything explicitly NOT to do, deadlines, or open questions.

---

### Minimal version (paste this if you're in a hurry)

```
Build: <what>
Goal / primary action: <…>
Audience & tone: <…>
Brand color: <hex or "pick one">
Stack: <existing, or "recommended default">
Design + build, all states, responsive, WCAG AA.
```
