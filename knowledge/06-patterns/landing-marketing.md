# Landing & Marketing Page Playbook

> Purpose: A proven, section-by-section blueprint for high-converting landing and marketing pages — what each block does, the order it goes in, and the copy/visual decisions that move conversion.

**When to read this:** Building any page whose single job is to convert a visitor into a signup, demo, trial, purchase, or lead — homepage hero, product page, campaign LP, waitlist, or feature page.

---

## The one rule that governs everything

A landing page has **one job**. Decide the single conversion action before writing a word, and subordinate every section to it. If a section does not move the visitor toward that action or remove a reason not to take it, cut it.

> Don't: a homepage with 9 competing CTAs ("Sign up", "Read blog", "Contact sales", "Watch demo", "Careers"…). The visitor chooses nothing.
> Do: one primary CTA repeated, with at most one secondary ("Get started free" primary, "Book a demo" secondary).

---

## Section order (the proven skeleton)

This is the default order for a B2B SaaS / product LP. Reorder only with a reason. Every section answers the next question forming in the visitor's head.

```
┌──────────────────────────────────────────────────────────────┐
│  NAV          [logo]            Product  Pricing  Docs  [CTA] │  sticky, slim
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  HERO         H1 value prop  ........................         │  "What is this,
│               subhead .........................     [VISUAL]  │   is it for me?"
│               [ Primary CTA ]  [ Secondary ]                 │
│               ✓ no card  ✓ 2-min setup                       │
├──────────────────────────────────────────────────────────────┤
│  SOCIAL PROOF   ▪logo ▪logo ▪logo ▪logo ▪logo   "10k teams"  │  "Do others
│                                                              │   trust this?"
├──────────────────────────────────────────────────────────────┤
│  PROBLEM       Name the pain in the visitor's own words      │  "They get me"
├──────────────────────────────────────────────────────────────┤
│  SOLUTION /    3–6 benefit-led feature blocks, alternating   │  "How does it
│  FEATURES      text/visual sides                             │   solve it?"
├──────────────────────────────────────────────────────────────┤
│  HOW IT WORKS  Step 1 → Step 2 → Step 3                      │  "Is it easy?"
├──────────────────────────────────────────────────────────────┤
│  TESTIMONIALS  Quotes + faces + outcomes, case-study links   │  "Proof it works"
├──────────────────────────────────────────────────────────────┤
│  PRICING       Plans / "free to start" / link to pricing     │  "Can I afford it?"
├──────────────────────────────────────────────────────────────┤
│  FAQ           5–8 objection-killing Q&As                    │  "But what about…"
├──────────────────────────────────────────────────────────────┤
│  FINAL CTA     Restate value + [ Primary CTA ]               │  the close
├──────────────────────────────────────────────────────────────┤
│  FOOTER        nav · legal · social · trust marks            │
└──────────────────────────────────────────────────────────────┘
```

**Variants by page intent:**

| Intent | Reordered emphasis |
|---|---|
| Free product / PLG | Hero → social proof → features → how it works → final CTA. Pricing light or omitted. |
| High-ticket / sales-led | Hero → logos → problem → outcome metrics → testimonials → **book a demo** (no self-serve pricing). |
| Waitlist / pre-launch | Hero → vision → "what you get early" → email capture. No pricing, minimal proof. |
| Ecommerce product | Hero (product shot) → buy box → reviews → details → cross-sell. See [pricing-ecommerce.md](./pricing-ecommerce.md). |
| Comparison / "vs" LP | Hero → comparison table → migration story → testimonials → CTA. |

---

## Above the fold: the 5-second anatomy

The fold must, on its own, answer: **what is this, who is it for, what do I do next, why trust it.** If a stranger can't restate your value prop after 5 seconds on the fold, it fails.

```
┌───────────────────────────────────────────────────────────┐
│                                                           │
│   Ship 3× faster without            ┌──────────────────┐  │
│   breaking production         ◄H1   │                  │  │
│                                     │   PRODUCT UI     │  │
│   Catch bugs before they ship.      │   (real, not     │  │
│   CI that explains failures   ◄sub  │    stock photo)  │  │
│   in plain English.                 │                  │  │
│                                     └──────────────────┘  │
│   [ Start free → ]  [ Watch demo ]  ◄ primary + secondary │
│                                                           │
│   ✓ No credit card   ✓ 5-min setup  ◄ friction-removers   │
│   ──────────────────────────────────                      │
│   Trusted by  ▪ACME ▪Globex ▪Initech ◄ proof strip        │
└───────────────────────────────────────────────────────────┘
```

**Fold checklist (all must be present):**
1. **Headline** — the value prop, benefit-led, scannable in one breath.
2. **Subhead** — one sentence that adds the *how* or the *who*, not a restatement.
3. **Primary CTA** — high contrast, action verb, visible without scrolling.
4. **Friction-removers** — micro-reassurance under the CTA ("No card", "Cancel anytime").
5. **Hero visual** — shows the product or the outcome, not a generic illustration.
6. **A whisper of proof** — a logo strip or "10,000+ teams" tucked at the fold edge.

> Don't bury the CTA below a 700px-tall hero illustration on mobile. The button must be reachable in the first viewport on a 667px-tall phone.

---

## The value-prop headline

The headline is 80% of the page's work. It should communicate **outcome**, not category. Lead with what changes for the user.

> Don't: "The leading cloud-native CI/CD orchestration platform" (category jargon, zero outcome).
> Do: "Ship 3× faster without breaking production" (outcome + pain avoided).

### Headline formula table

| Formula | Template | Example |
|---|---|---|
| Outcome + timeframe | "[Achieve X] in [time]" | "Launch a store in 10 minutes" |
| End pain | "[Do X] without [pain]" | "Send invoices without chasing payments" |
| Desire + audience | "[Outcome] for [who]" | "Bookkeeping built for freelancers" |
| Transformation | "From [before] to [after]" | "From spreadsheet chaos to one dashboard" |
| Quantified benefit | "[Number]× / %[metric]" | "Cut support tickets by 40%" |
| Job-to-be-done | "The [tool] that [verb]s your [job]" | "The inbox that sorts itself" |
| Provocative question | "What if [pain] just… stopped?" | "What if onboarding took one day?" |

**Headline rules:** ≤ 10 words ideal, ≤ 12 max. Concrete nouns over adjectives. One idea, not three clauses. No "revolutionary / next-gen / cutting-edge" — they signal nothing and read as filler. Front-load the strongest word so it survives F-pattern skimming.

**Subhead rules:** Expands the headline with the mechanism or the audience. "Here's how" or "Here's who." Keep to 1–2 lines (≤ 25 words). If the headline is the promise, the subhead is the proof-of-plausibility.

---

## Benefit-led vs feature-led copy

Features describe the product. Benefits describe the user's better life. Lead with benefit, then *back it* with the feature so the claim is credible. Pure benefit feels like fluff; pure feature is a spec sheet.

| Feature-led (weak) | Benefit-led (strong) |
|---|---|
| "256-bit AES encryption" | "Your data stays yours — bank-grade encryption, always on" |
| "Real-time collaboration" | "Edit together, see every change the instant it happens" |
| "Automated reporting" | "Walk into Monday with the report already written" |
| "99.99% uptime SLA" | "It's up when you need it — 26 seconds of downtime a year, max" |

**The pattern:** `Benefit headline → one-line elaboration → the feature that earns it`. The feature is the receipt, not the pitch.

---

## CTA best practices

The CTA is where intent becomes action. Three levers: **specificity, contrast, repetition.**

**Specificity** — the label should describe what happens next and imply value. Generic verbs leak conversion.

| Weak | Strong |
|---|---|
| "Submit" | "Get my free report" |
| "Sign up" | "Start free — no card" |
| "Learn more" | "See it in 2 minutes" |
| "Click here" | "Create my first project" |

First-person ("Start **my** trial") often beats second-person — it reads as the user's own decision.

**Contrast** — the primary CTA must be the single highest-contrast element in its viewport. One accent color, reserved for primary actions only. Secondary CTAs are ghost/outline buttons so the hierarchy is unmistakable. See [03-components/buttons.md](../03-components/components.md) for button anatomy and states.

**Repetition** — repeat the *same* primary CTA at natural decision points: hero, after features, after testimonials, and the final CTA. A visitor convinced at section 4 shouldn't have to scroll back up. Keep wording consistent so it reads as one offer, not four.

> Don't make the secondary CTA compete visually with the primary. "Book a demo" as a second solid button splits the click. Make it a text link or ghost button.

> Don't gate the primary action behind a long form on the LP. The CTA's job is the *click*; collect fields on the next step.

---

## Trust signals (and where they go)

Trust is built cumulatively. Distribute proof so a reason-to-doubt is answered the moment it arises.

| Trust signal | Best placement | Why |
|---|---|---|
| Customer logos | Just below fold + footer | Borrowed credibility, instant |
| Quantified usage ("10k teams") | Fold edge, near CTA | Bandwagon effect at decision point |
| Testimonials w/ name + face + role | After features | Specificity = believability |
| Outcome metrics ("cut churn 22%") | In/around testimonials | Proof the benefit is real |
| Security/compliance badges (SOC2, GDPR) | Near signup + footer | Kills the "is my data safe?" doubt |
| Star ratings / review counts (G2, Trustpilot) | Fold or near pricing | Third-party validation |
| Money-back / "cancel anytime" | Under CTA | Reverses purchase risk |
| Real founder/team photo | About / problem section | Humanizes, especially for SMB |

**Rules:** Real names, real faces, real numbers. Stock-photo "testimonials" and unnamed quotes ("— a happy customer") destroy trust faster than no testimonial. If you can link a quote to a case study, do — it makes the claim auditable.

---

## Scanning patterns: F and Z

People don't read pages; they scan them. Lay out for the eye-path.

**F-pattern** — applies to text-dense, content-heavy sections (feature lists, blog-style LPs). The eye sweeps the top line, drops, sweeps a shorter line, then scans down the left edge.
- Put the most important words at the **start of headlines and the top-left**.
- Front-load list items and bullet starts with the keyword.
- Left-align body copy; ragged-right is fine, justified is not.

```
F-PATTERN                        Z-PATTERN
████████████████  ◄ top sweep    [logo] ──────────► [CTA]   ◄ top bar
███████                              ╲
██████████  ◄ second sweep            ╲  diagonal of attention
███                                    ╲
██                                      ▼
██  ◄ left-edge scan              [hero text] ──────► [VISUAL]
██                                                       │
                                  [ Primary CTA ] ◄──────┘  ◄ bottom close
```

**Z-pattern** — applies to sparse, hero-style layouts with little text. Eye goes top-left → top-right → diagonal → bottom-left → bottom-right. Place logo top-left, nav-CTA top-right, value prop on the diagonal, primary CTA at the bottom-right terminus.

**Rule of thumb:** simple/visual fold → design for Z; dense/textual section → design for F. Either way, the primary CTA sits where the eye-path ends.

---

## Hero visual choices

The hero image is a promise. Choose by what best proves the value.

| Visual type | Use when | Avoid when |
|---|---|---|
| **Product screenshot/UI** | The product *is* the value (SaaS, tools) | UI is ugly or pre-MVP — fake it cleanly instead |
| **Annotated UI** | One feature needs spotlighting | The annotation clutters the shot |
| **Short looping demo (muted, autoplay)** | The magic is in the motion/interaction | It hurts load time on mobile — lazy-load + poster |
| **Outcome visualization** (dashboard "after") | Selling a result, not an interface | You can't make the result concrete |
| **Hero photo of person using it** | Lifestyle/consumer, emotional pull | B2B technical buyer wants the product, not a model |
| **Abstract illustration** | Brand-led, concept is intangible | It says nothing — the default failure mode |

**Rules:** Real product over stock art, every time. If you must illustrate, make it specific to *your* product's story. Provide a static poster for any video and lazy-load below-fold media. Never let the hero visual push the CTA below the first viewport on mobile.

---

## Mobile considerations

Most landing traffic is mobile. Design the fold mobile-first.
- Headline + subhead + one CTA must fit the first viewport (~640px tall). Stack visual *below* the CTA, not above it.
- Single-column everything. Side-by-side feature blocks stack vertically.
- Tap targets ≥ 44px; CTA full-width or near-full-width.
- Sticky bottom CTA bar is a strong mobile pattern — keeps the action one tap away through the whole scroll.
- Logo strips wrap or scroll; don't shrink logos to illegible.

---

## Conversion mistakes (the catalog)

> Don't open with a carousel/slider hero. They auto-advance past the message, tank engagement, and split the value prop across slides nobody waits for. Pick one message.

> Don't use a generic headline that names your category instead of the visitor's outcome. "An all-in-one platform" is not a value prop.

> Don't stack five different CTAs with equal weight. Decide the one action; make everything else secondary or a link.

> Don't hide pricing entirely if you're self-serve. "Contact us for pricing" on a $20/mo tool kills conversion — buyers assume "expensive" and bounce.

> Don't put the contact form's 11 fields on the LP. Each extra field drops completion. Ask for email; collect the rest after the click.

> Don't use fake urgency ("3 spots left!") that resets on refresh. It's detectable and burns trust permanently.

> Don't ship a hero video that autoplays with sound or blocks first paint. Muted, poster-framed, lazy-loaded — or it costs more conversions than it earns.

> Don't write testimonials without names and faces. Anonymous praise reads as invented.

> Don't make the visitor hunt for "what is this." If the fold needs a scroll to explain the product, the headline failed.

> Don't neglect the final CTA. The most motivated readers reach the bottom — give them the button there, not just a footer.

---

## Copy voice & microcopy

- Speak to "you," singular. The page is a 1:1 conversation.
- Active voice, present tense. "You ship faster," not "Faster shipping is enabled."
- Concrete numbers beat adjectives. "Set up in 5 minutes" > "Quick setup."
- Match the visitor's vocabulary, not your internal jargon. If they say "payroll run," don't say "compensation event."
- Microcopy under CTAs reverses risk: "No credit card · Cancel anytime · 14-day trial." See [04-interaction/microcopy.md](../05-quality/review-checklist.md).

---

## Related playbooks

- [pricing-ecommerce.md](./pricing-ecommerce.md) — pricing tables, product pages, checkout.
- [auth-onboarding.md](./auth-onboarding.md) — what happens after the CTA click.
- [dashboards.md](./dashboards.md) — the product surface you're driving signups to.
- [../03-components/components.md](../03-components/components.md) — CTA button states and contrast.
- [../02-foundations/typography.md](../02-foundations/typography.md) — headline scale and hierarchy.

---

## Agent checklist

- [ ] Define the single conversion action before designing; subordinate every section to it.
- [ ] Ensure the fold answers what/who/why-trust/next-step within 5 seconds, CTA visible without scroll.
- [ ] Write a benefit-led headline ≤ 12 words using a formula from the table; no category jargon.
- [ ] Make the primary CTA the highest-contrast element in every viewport and repeat it 3–4×.
- [ ] Pair every feature claim with the benefit it delivers and a proof point.
- [ ] Distribute trust signals (logos near fold, named testimonials after features, security near signup).
- [ ] Order sections hero → proof → problem → solution → how → testimonials → pricing → FAQ → final CTA.
- [ ] Use a real product/outcome visual, lazy-loaded, never pushing the mobile CTA below the fold.
- [ ] Lay text-dense sections for F-pattern and sparse hero for Z-pattern, CTA at the eye-path terminus.
- [ ] Strip the LP form to the minimum fields; collect the rest after the click.
- [ ] Kill conversion-killers: carousels, fake urgency, anonymous quotes, equal-weight CTAs.
- [ ] Verify the fold, single-column stack, and 44px tap targets on a 640px-tall mobile viewport.
