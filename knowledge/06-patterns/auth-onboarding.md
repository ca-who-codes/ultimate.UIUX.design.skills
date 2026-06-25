# Auth & Onboarding Playbook

> Purpose: A blueprint for sign-up, login, and onboarding — minimizing friction at the door, then guiding new users to their first taste of value (the aha moment) as fast as possible.

**When to read this:** Building login/signup screens, password flows, social auth, magic links, welcome/onboarding sequences, setup wizards, activation checklists, empty-state onboarding, or permission-priming prompts.

---

## Two jobs, one goal

Auth and onboarding are a single funnel with one goal: **get the user to value, fast.**
- **Auth** (signup/login) is a tollbooth. Its only job is to let the right person through with minimum friction. Every field, every step, every wait is tax on conversion.
- **Onboarding** is the guided path from "account created" to "I get it — this is valuable." Its job is the **aha moment**, not a feature tour.

Optimize auth for *speed-through*; optimize onboarding for *time-to-value*. Never confuse the two by front-loading onboarding work into signup.

---

## Sign-up vs login: keep them distinct but linked

They are different intents and must not be ambiguous.

- **Signup** = "I'm new, create me." Optimize for the *fewest possible fields* and the strongest reason to start.
- **Login** = "I'm back, let me in." Optimize for *speed and recognition* (remembered email, last-used method).
- Always provide a clear cross-link: signup page → "Already have an account? **Log in**"; login page → "New here? **Sign up**".
- Detect returning users where possible and default to the right screen / last-used auth method.

> Don't merge them into one ambiguous form where the user can't tell if they're creating or accessing an account. The error messages and field sets differ.

---

## Best-practice signup form anatomy

The best signup form is the shortest one that still creates a usable account. Default to **email + password** or **social/magic-link only** — collect everything else *after* the user is in.

```
┌─────────────────────────────────────────┐
│              [ logo ]                    │
│         Create your account              │ ◄ clear, outcome-y heading
│   Start free — no credit card required   │ ◄ friction-remover subhead
│                                          │
│   [  Continue with Google         ]      │ ◄ social first (fastest path)
│   [  Continue with GitHub         ]      │
│   ─────────────  or  ─────────────       │
│                                          │
│   Email                                  │
│   [ you@company.com               ]      │ ◄ inline-validated
│                                          │
│   Password                          👁    │ ◄ visibility toggle
│   [ ••••••••••                    ]      │
│   ▰▰▰▱▱  Strong enough                   │ ◄ live strength + rules met
│   ✓ 8+ chars   ✓ a number                │
│                                          │
│   [        Create account         ]      │ ◄ specific, full-width CTA
│                                          │
│   By continuing you agree to Terms·Privacy│ ◄ inline, not a checkbox wall
│   Already have an account?  Log in       │ ◄ cross-link
└─────────────────────────────────────────┘
```

**Signup rules:**
- **Minimize fields.** Email + password is the floor. Name, company, role, phone — collect later, contextually, when they're actually needed. Each removed field measurably lifts completion.
- **Lead with social auth / SSO** when your audience uses it — it's the fastest path and skips password creation entirely. Order methods by likely usage.
- **No "confirm password" field.** It nearly doubles friction; a visibility toggle solves the same problem better.
- **No "confirm email" field.** Send a verification link instead; let the user *use the product* before verifying where the model allows.
- **State the value at the top** ("Start free", "No card required") — the reason to push through the form.
- **Terms inline**, not as a separate mandatory checkbox, unless legally required.
- One clear, specific CTA: "Create account" / "Start free trial" — not "Submit".

---

## Best-practice login form anatomy

```
┌─────────────────────────────────────────┐
│              [ logo ]                    │
│            Welcome back                  │
│                                          │
│   [  Continue with Google         ]      │ ◄ last-used method highlighted
│   ─────────────  or  ─────────────       │
│                                          │
│   Email                                  │
│   [ you@company.com               ]      │ ◄ prefilled if remembered
│                                          │
│   Password                          👁    │
│   [ ••••••••••                    ]      │
│                  Forgot password?        │ ◄ rescue link, always visible
│                                          │
│   [           Log in              ]      │
│                                          │
│   New here?  Create an account           │
└─────────────────────────────────────────┘
```

**Login rules:**
- **Remember the email** and the last-used method; show "Continue as you@…" for returning sessions.
- **"Forgot password?" must be visible** next to the password field — never hidden. Locked-out users are a churn risk.
- **Don't reveal which field was wrong** for security ("Email or password is incorrect" — not "no account with this email"), but do make recovery one click away.
- Support password managers: correct `autocomplete` attributes, real `<input type="password">`, no JS that blocks paste.
- Offer the same auth methods as signup, in a consistent order.

---

## Passwords: rules, strength, visibility

- **Show a password-visibility toggle (👁).** It's the single highest-impact fix for password friction and error rates. Default masked; let users reveal.
- **State requirements upfront and validate live** — show rules met (✓ 8+ chars, ✓ a number) as the user types, not in an error after submit. See inline validation below.
- **Modern password rules:** require length (8–12+ min), not arbitrary composition gymnastics. Forced symbol/uppercase/number cocktails produce *weaker*, more-forgotten passwords. Length + a blocklist of common passwords beats complexity theater.
- **Don't block paste.** Password-manager users paste; blocking it punishes your most secure users.
- Offer a **strength meter** as guidance, not a gate, unless the password is genuinely weak.

> Don't enforce "must contain 1 uppercase, 1 number, 1 symbol, no repeated chars, change every 30 days." This is discredited; it lowers real-world security.

---

## Magic links & passwordless

For many products, the lowest-friction auth is no password at all.
- **Magic link:** enter email → click link in inbox → in. Zero password to create, remember, or leak. Ideal for low-frequency-login products.
- **Trade-off:** adds an email round-trip (inbox-switch friction) and depends on deliverability. Best as an *option* alongside password/social, or as the primary method for products where logins are infrequent.
- **OTP code** (6-digit) is a magic-link variant that keeps the user on the same tab — better for mobile.
- Always show a clear "we sent a link to you@… — check your inbox" confirmation with a resend option and a "wrong email?" escape.
- **Passkeys** are the emerging best path: phishing-resistant, no shared secret. Offer where supported, with a fallback.

---

## Inline validation

Validate at the right moment, in the right place. Bad validation timing is a top cause of form abandonment.

| Moment | Behavior |
|---|---|
| **While typing** | Show *positive* progress only (✓ rules met). Don't yell "invalid email" mid-typing. |
| **On blur (leave field)** | Validate format/availability; show error if wrong. |
| **On submit** | Catch anything remaining; focus the first error. |

**Rules:**
- Error messages are **specific and actionable**: "Email is already registered — log in?" not "Invalid input."
- Place errors **next to the field**, not in a top banner the user has scrolled past.
- Validate email *availability* on blur where possible, so the user isn't told "already taken" only after filling the whole form.
- Never clear the form on error. Preserve every entered value.
- Use color *and* icon/text for errors (don't rely on red alone — accessibility). See [../05-quality/accessibility.md](../05-quality/accessibility.md).

> Don't fire a red "invalid email" the instant the user types the first character. Validate format on blur, not on every keystroke.

---

## Reducing friction (the master checklist)

Every point of friction is a leak. Audit ruthlessly:
- **Cut fields.** Can this field be collected later, inferred, or dropped? Default to dropping.
- **Defer verification.** Let users into the product before email verification where the model allows; verify before sensitive actions, not at the door.
- **Social/SSO first.** One click beats a form.
- **Autofocus the first field.** Don't make the user click to start typing.
- **Smart defaults & autocomplete.** Detect country, prefill from OAuth profile, correct `autocomplete` tags.
- **No CAPTCHA unless you're under attack.** It's pure friction; use invisible/risk-based checks first.
- **Mobile keyboards:** `type="email"`, `inputmode`, `autocapitalize="off"` for emails/usernames.
- **One thing per screen** on mobile — don't cram signup, profile, and team-invite into one giant form.

---

## Onboarding: design for the aha moment

Onboarding's north star is the **activation moment** — the specific action after which a user "gets it" and is dramatically more likely to retain. Identify it, then design the entire onboarding to reach it as fast as possible.

| Product | Aha / activation moment |
|---|---|
| Collaboration tool | Invite a teammate; first shared doc edited |
| Analytics | Connect a data source; see real numbers |
| Design tool | Create/import the first real project |
| Social network | Follow N people; first feed populated |
| Communication | Send the first message |
| Dev tool | First successful build / first API call |
| E-commerce SaaS | First product added; first order received |

**Rule:** map the shortest path from signup to that moment, and strip everything that isn't on it. Don't make users configure settings, read a tour, or fill a profile *before* experiencing value.

> Don't open with a 6-slide feature carousel before the user has done anything. Tours of features the user hasn't earned context for are skipped and forgotten. Show value, then teach.

---

## Onboarding patterns

| Pattern | Best for | Notes |
|---|---|---|
| **Empty-state onboarding** | Most products | Teach inside the real UI via populated-by-doing empty states. Lowest friction, highest stick. |
| **Setup wizard** | Genuinely sequential setup (connect account, configure) | Only when steps are truly required and ordered. Show progress. |
| **Checklist / getting-started** | Multi-step activation | A visible, finite list of value-driving steps with progress. |
| **Interactive walkthrough** | Complex UIs | Do-it-with-me, not watch-me. Let users act; coach contextually. |
| **Progressive disclosure** | Feature-rich products | Reveal advanced features as users grow into them, not all at once. |
| **Templates / sample data** | Blank-canvas products | Start from a template so the user sees a filled-in result immediately. |

### Empty-state-driven onboarding (the default)

The best onboarding often isn't a separate flow at all — it's **empty states that teach by inviting action.** Each empty zone explains what goes there and offers the one action to fill it.

```
┌───────────────────────────────────────────┐
│   Your projects                            │
│                                            │
│        📁  No projects yet                 │
│                                            │
│   Projects keep your work organized.       │
│   Create your first one to get started.    │
│                                            │
│        [ + Create a project ]              │ ◄ the activating action
│        or  start from a template           │
└───────────────────────────────────────────┘
```

This beats modal tours: it's contextual, dismissible by *doing*, and the lesson sticks because the user performed it. See [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md).

### Setup wizards & checklists

When setup genuinely requires several ordered steps, use a wizard with visible progress; otherwise prefer a non-blocking checklist.

```
Setup checklist                          3 of 5 done  ▰▰▰▱▱
─────────────────────────────────────────────────────────
 ✓  Create your account
 ✓  Verify your email
 ✓  Create your first project
 ▢  Invite a teammate            →   ◄ next best action highlighted
 ▢  Connect your data source     →
                                         [ Skip for now ]
```

**Checklist rules:**
- Keep it short (3–5 steps) and ordered by value, activating step first.
- Show **progress** — partial completion is a powerful motivator (endowed-progress effect: pre-check "Create account" so they start at 1/5, not 0/5).
- Each step links directly to the action. Never make the user hunt for where to do it.
- Make it **dismissible/skippable** — never trap the user. Power users and returning users should be able to dump it.
- Celebrate completion (brief, earned — not confetti spam).

---

## Welcome screens

A welcome screen is worth showing only if it *does work*, not just greets.
- **Good welcome:** a single question that personalizes the product ("What are you here to do?" → tailor the next screen). Or one input that produces immediate value.
- **Bad welcome:** a "Welcome to ProductX! We're so excited!" dead-end with a "Get started" button that just dismisses it. That's a speed bump.
- If you collect role/use-case here, *use it* to branch onboarding — otherwise don't ask.
- Keep it to one screen, one decision. The user wants to reach the product, not read a greeting card.

---

## Permission priming

Never fire a native OS permission prompt (notifications, location, camera, contacts) cold. A denied permission is often permanent and hard to recover.

**The priming pattern:** show your *own* in-app explanation first, asking only after the user understands the value. Trigger the real OS prompt only when they opt in — and ideally at the *moment of need*, not at launch.

```
┌─────────────────────────────────────────┐
│   🔔  Stay on top of replies             │
│                                          │
│   Turn on notifications and we'll ping   │
│   you when someone responds — never for  │
│   anything else.                         │
│                                          │
│   [ Maybe later ]    [ Enable alerts ]   │ ◄ "Enable" triggers OS prompt
└─────────────────────────────────────────┘
```

**Rules:**
- **Prime before prompting.** Your soft ask is recoverable; the OS hard ask often isn't.
- **Ask in context, at the moment of value** ("Enable notifications so you hear back about *this* message"), not on first launch in a batch.
- **Explain the benefit to the user**, not your need ("never miss a reply", not "we'd like to send you marketing").
- **Respect "Maybe later."** Re-ask later, in context — don't nag immediately.
- Request the *minimum* permissions, only when actually needed.

---

## Auth & onboarding mistakes (the catalog)

> Don't pile signup fields (name, company, role, phone, how-did-you-hear). Email + password or social only; collect the rest later.

> Don't use "confirm password" / "confirm email" fields. A visibility toggle and a verification link do the job with less friction.

> Don't enforce arcane password-complexity rules or block paste. Require length, use a common-password blocklist, allow password managers.

> Don't validate aggressively on every keystroke. Positive feedback while typing; errors on blur and submit.

> Don't hide "Forgot password?" Locked-out users churn; keep recovery one click away.

> Don't reveal which credential was wrong on login. Generic message + easy recovery.

> Don't open onboarding with a feature-tour carousel before the user has done anything.

> Don't force profile completion or settings configuration before the user reaches value.

> Don't fire cold OS permission prompts at launch. Prime in-app first, ask in context.

> Don't trap users in a non-skippable wizard or an undismissable checklist.

> Don't clear the form on error or lose entered data. Preserve everything.

---

## Related playbooks

- [dashboards.md](./dashboards.md) — the first-run dashboard onboarding fills.
- [landing-marketing.md](./landing-marketing.md) — the CTA that delivered the user here.
- [pricing-ecommerce.md](./pricing-ecommerce.md) — checkout auth (guest vs account).
- [../04-interaction/states-feedback.md](../04-interaction/states-feedback.md) — empty-state-driven onboarding.
- [../03-components/forms.md](../03-components/forms.md) — field design and validation timing.
- [../05-quality/accessibility.md](../05-quality/accessibility.md) — accessible errors, labels, focus order.

---

## Agent checklist

- [ ] Strip signup to email + password or social/magic-link; defer all other fields to after entry.
- [ ] Lead with social auth / SSO ordered by likely usage; drop confirm-password and confirm-email.
- [ ] Add a password visibility toggle and show live, positive validation of password rules.
- [ ] Require password length over composition complexity; allow paste and password managers.
- [ ] Keep "Forgot password?" visible on login and remember the user's email and last-used method.
- [ ] Validate inline: positive while typing, errors on blur and submit, specific and field-adjacent.
- [ ] Use a generic login-error message but make recovery one click away.
- [ ] Identify the product's aha moment and design the shortest signup-to-value path to it.
- [ ] Prefer empty-state-driven onboarding over feature-tour carousels; teach by doing.
- [ ] Use a short, skippable, progress-showing checklist with the activating step first (pre-checked).
- [ ] Prime permissions in-app before firing OS prompts; ask in context at the moment of value.
- [ ] Never clear forms on error, trap users in wizards, or block users before they reach value.
