# Pricing & Commerce Playbook

> Purpose: A blueprint for pricing pages and commerce flows — pricing-table anatomy, psychological pricing, product listing and detail pages, and a checkout that minimizes abandonment.

**When to read this:** Building a SaaS pricing page, a plan comparison, a product listing grid (PLP), a product detail page (PDP), a cart, or a checkout flow.

---

## Part 1 — Pricing pages

### The job of a pricing page

A pricing page's job is **not** to list prices — it's to guide the visitor to the *right* plan with confidence and minimum hesitation. The enemy is decision paralysis. Every choice you add is a chance for the visitor to choose *nothing*.

### Pricing table anatomy

```
        Monthly  ◯───●  Annual  (save 20%)        ◄ billing toggle, annual default
 ┌─────────────┐ ┌═══════════════┐ ┌─────────────┐
 │  Starter    │ ║  Pro  ★ Best  ║ │ Enterprise  │
 │             │ ║    value      ║ │             │  ◄ recommended plan
 │   $19       │ ║    $49        ║ │   Custom    │     highlighted (badge,
 │   /mo       │ ║    /mo        ║ │             │     color, scale, lift)
 │             │ ║               ║ │             │
 │ [Start free]│ ║[ Start free ]║ │[Contact us] │  ◄ recommended CTA = solid;
 │             │ ║               ║ │             │     others ghost
 │ ✓ 3 projects│ ║ ✓ Unlimited   ║ │ ✓ Everything│
 │ ✓ 1 seat    │ ║ ✓ 10 seats    ║ │ ✓ SSO/SAML  │  ◄ features framed as
 │ ✓ Email supp│ ║ ✓ Priority    ║ │ ✓ SLA       │     "everything in X, plus"
 │ ─ No API    │ ║ ✓ API access  ║ │ ✓ Dedicated │
 └─────────────┘ └═══════════════┘ └─────────────┘
```

**Anatomy rules:**
- **3-tier rule.** Three plans is the sweet spot — enough for good/better/best framing, few enough to avoid paralysis. Two feels thin; four+ overwhelms. If you have more, group them or hide niche tiers behind "see all plans."
- **Highlight the recommended plan.** Make the target plan unmissable: a "Most popular" / "Best value" badge, accent color, slight scale-up, elevation. This is the single most effective lever on a pricing page — most users pick the one you point to.
- **Plan names signal audience,** not size: "Starter / Pro / Enterprise" or "Personal / Team / Business." Help the visitor self-identify.
- **Price is the largest element** in each card. Show the billing period clearly (`/mo`, `/user/mo`).
- **CTA per plan,** with the recommended plan's CTA solid/primary and others ghost. Consistent verb across plans.
- **Features as cumulative:** "Everything in Starter, plus…" reduces reading load and makes upgrades feel additive.
- **Show what's excluded** too (greyed/✕) so the upgrade reason is visible — but don't make lower tiers feel punitive.

### Annual / monthly toggle

- Offer both; **default to annual** and show the savings explicitly ("Save 20%" / "2 months free"). Annual lifts LTV and reduces churn.
- Show the *effective* monthly price on annual ("$49/mo billed annually") so the number stays comparable, plus the annual total.
- Make the toggle obvious and the price update instant and animated.

### Anchor pricing & psychology

| Tactic | How | Why it works |
|---|---|---|
| **Anchoring** | Show a high tier (Enterprise/"Custom") so middle looks reasonable | First number seen sets the reference; mid-tier feels like a deal |
| **Decoy effect** | Make the target plan obviously better-value than its neighbor | A slightly-worse nearby option pushes choice to the target |
| **Charm pricing** | $49 not $50; $19 not $20 | Left-digit effect — reads meaningfully cheaper |
| **Price framing** | "$1.60/day" or "/seat/mo" for big totals | Shrinks the perceived cost to a trivial unit |
| **Annual savings** | "$490/yr (save $98)" | Loss-aversion: not saving feels like losing |
| **Free tier / trial** | "Start free" removes entry risk | Lowers commitment; gets users to value first |

**Rule:** psychology serves clarity, never deception. Hidden fees, fake "was $99" anchors, or countdowns that reset destroy trust and convert once. Be honest; be clear.

### Reducing decision paralysis

- **Recommend a default** ("Most popular") so the undecided have a safe choice.
- **Limit tiers** to 3 (±1). More plans = more abandonment.
- **Feature-comparison table** *below* the cards for detail-seekers — let scanners decide from cards, let comparers dig in.
- **Answer pricing objections inline:** an FAQ ("Can I change plans? Is there a contract? What counts as a seat?") right under the table.
- **Show a clear "talk to us"** path for Enterprise so big buyers don't bounce on "Custom."
- **Free trial / money-back** reverses risk: "14-day free trial · no card · cancel anytime."

### Feature comparison table

For products where the difference between plans is detailed, add a full comparison below the cards.

```
                          Starter   Pro      Enterprise
─────────────────────────────────────────────────────────
 Projects                 3         ∞        ∞
 Team members             1         10       Unlimited
 API access               —         ✓        ✓
 SSO / SAML               —         —        ✓
 Support                  Email     Priority Dedicated
 SLA                      —         —        99.9%
 [ row CTAs repeat here ]
```

**Rules:** sticky header row + sticky plan column on scroll; group rows by category; repeat CTAs at the bottom; use ✓/— with text (not color alone). Keep it scannable — the cards make the decision, the table confirms it.

### Pricing mistakes

> Don't show 5+ equal-weight plans with no recommendation. Paralysis → no purchase.
> Don't hide all pricing behind "Contact sales" for a self-serve product. Buyers assume expensive and leave.
> Don't bury the annual toggle or hide that annual is cheaper.
> Don't use fake anchors or resetting countdowns. One-time conversion, permanent trust loss.
> Don't make lower tiers feel punishing — frame upgrades as gains, not the removal of dignity.

---

## Part 2 — Product listing (PLP) & cards

### Product card anatomy

```
┌──────────────────┐
│                  │
│   [ IMAGE ]      │ ◄ consistent ratio, hover = 2nd image
│            ♡     │ ◄ wishlist (optional)
├──────────────────┤
│ Brand            │
│ Product name     │ ◄ truncate to 2 lines, consistent
│ ★★★★☆ (128)      │ ◄ rating + count (social proof)
│ $49  $̶6̶9̶  -29%   │ ◄ price; sale price first, strike original
│ ● ● ●            │ ◄ variant swatches (optional)
│ [ Add to cart ]  │ ◄ appears on hover or always
└──────────────────┘
```

**PLP grid rules:**
- **Consistent image aspect ratio** across all cards — ragged heights destroy scannability. Pad/crop to one ratio.
- **Show price, rating, and a sale flag** on the card — the three things shoppers filter on visually.
- **2–4 columns** on desktop, 2 on mobile. Don't cram 6 tiny cards.
- **Sale price first, original struck through** with the discount — frames the saving.
- **Fast filtering & sorting** (price, rating, popularity, new) in a left rail or top bar, with applied filters as removable chips and the result count visible.
- **Quick-add / quick-view** on hover speeds purchase without leaving the grid.
- **Lazy-load images**, reserve space (no layout shift), show skeletons. See [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md).

---

## Part 3 — Product detail page (PDP)

The PDP is where the purchase decision is made. It must answer: is this right for me, can I trust it, how do I buy.

```
┌───────────────────────────┬──────────────────────────┐
│  ┌─────────────────────┐  │  Brand                   │
│  │                     │  │  Product Name            │ ◄ BUY BOX (sticky
│  │   MAIN IMAGE        │  │  ★★★★☆ 4.6 (128 reviews) │    on scroll)
│  │   (zoomable)        │  │                          │
│  │                     │  │  $49.00  $̶6̶9̶  Save 29%   │
│  └─────────────────────┘  │                          │
│  ▢ ▢ ▢ ▢  ◄ thumbnails    │  Color:  ● ● ●            │ ◄ variant pickers
│                           │  Size:   [S][M][L][XL]   │
│  GALLERY                  │                          │
│                           │  Qty [ 1 ▾]              │
│                           │  [   Add to cart    ]    │ ◄ primary action
│                           │  [   Buy now        ]    │ ◄ express checkout
│                           │  ✓ Free shipping over $50│ ◄ trust / reassurance
│                           │  ✓ 30-day returns        │
│                           │  🔒 Secure checkout       │
├───────────────────────────┴──────────────────────────┤
│  Description · Details · Specs · Shipping (tabs)      │
├───────────────────────────────────────────────────────┤
│  ★ Reviews (128)  — rating breakdown, photos, sortable│
├───────────────────────────────────────────────────────┤
│  You might also like  [card][card][card][card]        │ ◄ cross-sell
└───────────────────────────────────────────────────────┘
```

**PDP rules:**
- **Gallery:** multiple real images, zoom, and scale/context shots. Image quality *is* the conversion driver in commerce. Include video where it helps.
- **Buy box** (right column) holds name, rating, price, variant pickers, quantity, and CTAs — and **stays sticky** as the user scrolls the details.
- **Variant selection is unmissable** and shows availability per variant (greyed if out of stock). Reflect the selected variant in the gallery and price.
- **Primary CTA** ("Add to cart") is the highest-contrast element; offer an express "Buy now" / wallet button for impulse buys.
- **Trust cluster near the CTA:** free-shipping threshold, returns policy, secure-checkout mark, stock status. These reverse purchase risk at the decision point.
- **Reviews** with rating distribution, customer photos, sort/filter, and verified-purchase badges. Reviews are the #1 trust signal in commerce — surface them prominently.
- **Honest stock/urgency** ("Only 3 left") only when *true*.
- **Cross-sell / "frequently bought with"** below the fold, never above the buy box.

---

## Part 4 — Cart & checkout

Checkout is where the money is won or lost. The average cart-abandonment rate is ~70%; most of it is self-inflicted friction. Every step, field, and surprise is a leak.

### Checkout flow step list

A lean, proven flow:

```
1. CART
   - Line items: thumbnail, name, variant, qty (editable), price, remove
   - Order summary: subtotal, shipping est., tax est., TOTAL
   - Trust: secure badge, accepted payments
   - [ Checkout ]   and   [ Continue shopping ]

2. IDENTITY  ← offer GUEST CHECKOUT first; login optional
   - Email (for receipt + order tracking)
   - [ Continue as guest ]   ·   [ Log in ]   ·   [ Create account ]

3. SHIPPING
   - Address (with autocomplete/autofill)
   - Shipping method + cost + ETA (shown, not hidden)

4. PAYMENT
   - Express wallets up top (Apple/Google Pay, PayPal, Shop Pay)
   - Card fields (autofill-friendly, formatted)
   - Billing = shipping by default

5. REVIEW & PLACE ORDER
   - Final summary, edit links per section
   - [ Place order ]  — total restated on the button

6. CONFIRMATION
   - Order #, ETA, what happens next, receipt emailed
   - Account-creation offer (post-purchase, optional)
```

### Checkout rules

- **Offer guest checkout — prominently.** Forcing account creation is the single biggest abandonment cause. Let guests buy; offer account creation *after* purchase (when they'd just enter a password).
- **Minimize steps and fields.** Combine where sensible (single-page or accordion checkout). Drop optional fields. Each field is friction.
- **Show progress.** A step indicator (`Cart → Shipping → Payment → Done`) tells users how much is left and reduces anxiety. For single-page, show the sections.
- **Address autofill / autocomplete** (Google Places-style) — typing an address is slow and error-prone. Auto-detect country; format postal codes per country.
- **No surprise costs.** Show shipping and tax *early* (cart or first step), not as a shock on the final screen. Unexpected fees are the #1 stated reason for abandonment.
- **Express checkout up top.** Apple/Google Pay, PayPal, Shop Pay — one-tap paths skip the whole form. Put them above the manual fields.
- **Inline, recoverable errors.** Validate cards and addresses inline; never clear the form; explain exactly what's wrong and how to fix it (declined card → "try another card", not a dead-end).
- **Trust badges at the point of payment:** secure-checkout lock, accepted card logos, money-back/returns. They calm the moment of handing over card details.
- **Persist the cart** across sessions and devices. A user who left should find their cart intact.
- **Keep the user in checkout.** No nav distractions, no header menu pulling them out. A minimal header (logo + secure badge) on checkout pages.
- **Total on the button.** "Place order — $58.40" beats a bare "Place order"; the user confirms what they pay.
- **Mobile-first:** correct input types (`numeric` for card, `tel` for phone), big tap targets, wallet buttons that shine on mobile.

### Cart-abandonment reducers (the catalog of fixes)

| Abandonment cause | Fix |
|---|---|
| Forced account creation | Guest checkout, account offered post-purchase |
| Surprise shipping/fees at the end | Show costs early; free-shipping threshold with progress ("$8 to free shipping") |
| Long / multi-page checkout | Fewer steps/fields; single-page or accordion; autofill |
| No trust at payment | Security badges, returns policy, recognized payment logos |
| Can't see total/what's left | Step indicator + always-visible order summary |
| Errors that dead-end | Inline recovery, card-decline guidance, preserved input |
| Lost cart on return | Persistent cross-device cart; abandonment email with the cart restored |
| Limited payment options | Wallets + PayPal + card; buy-now-pay-later where relevant |
| Slow / janky checkout | Optimistic UI, instant validation, no full reloads |

### Checkout mistakes

> Don't force account creation before purchase. Guest checkout first; account after.
> Don't reveal shipping/tax only on the final step. Surface costs early.
> Don't bury express-pay below a long card form. Wallets up top.
> Don't dead-end on a declined card or a validation error. Recover inline, preserve input.
> Don't keep the full site nav in checkout — it leaks users out of the funnel.
> Don't make the user re-enter billing if it equals shipping. Default them equal.
> Don't hide the running total. Show the order summary throughout and put the total on the place-order button.

---

## Related playbooks

- [landing-marketing.md](./landing-marketing.md) — the page that drives traffic to pricing/PDP.
- [auth-onboarding.md](./auth-onboarding.md) — guest-vs-account decisions and post-purchase account creation.
- [dashboards.md](./dashboards.md) — the post-purchase account/admin surface.
- [../03-components/components.md](../03-components/components.md) — CTA contrast and states for Add-to-cart / Place-order.
- [../03-components/forms.md](../03-components/forms.md) — checkout field design, autofill, validation timing.
- [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md) — PLP image loading, checkout submission states.

---

## Agent checklist

- [ ] Use 3 pricing tiers (±1) and visibly highlight one recommended plan with badge, color, and scale.
- [ ] Offer annual/monthly toggle defaulting to annual with explicit savings; show effective monthly price.
- [ ] Apply honest pricing psychology (anchor tier, charm pricing, free trial) — never fake anchors or fees.
- [ ] Put a feature-comparison table and a pricing FAQ below the cards for detail-seekers.
- [ ] Build product cards on one image aspect ratio with price, rating, and sale flag; lazy-load images.
- [ ] Give the PDP a multi-image zoomable gallery and a sticky buy box with clear variant selection.
- [ ] Cluster trust signals (shipping, returns, secure checkout, stock) right beside the buy CTA.
- [ ] Surface reviews with rating breakdown, photos, and verified badges prominently on the PDP.
- [ ] Offer guest checkout prominently; defer account creation to post-purchase.
- [ ] Minimize checkout steps/fields, show a progress indicator, and enable address autofill.
- [ ] Show shipping and tax early, put express wallets up top, and recover from errors inline.
- [ ] Persist the cart, keep nav out of checkout, and restate the total on the place-order button.
