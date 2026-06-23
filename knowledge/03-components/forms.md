# Forms

> Purpose: Compose forms that are fast to complete and hard to get wrong — layout, labels, validation timing, error wording, and accessibility.

**When to read this:** Before building any form — sign-up, checkout, settings, multi-step wizard — or when fixing validation that fires too early, error messages users can't act on, or forms screen readers can't navigate.

For the primitives this composes (input, select, checkbox, button) see [./components.md](./components.md). For a11y mechanics (label association, focus management) see [../05-quality/accessibility.md](../05-quality/accessibility.md). For spacing/sizing tokens see [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md).

---

## Layout: single column wins

- **One column, top to bottom.** Multi-column forms make users zig-zag, miss fields, and misread which label belongs to which input. Eye-tracking and completion studies consistently favor single column.
- **Exception**: short, logically paired fields can share a row — `City / State / ZIP`, `Expiry / CVC`, `First / Last`. Keep these to fields the user reads as one unit, and let them stack on narrow viewports.
- Max field width should match expected content: a ZIP field shouldn't be as wide as a street address. Width is an affordance.
- Form container max-width ~480–640px for readability; full-width fields inside.

```
Do                          Don't
[ Full name            ]    [ First ] [ Last ] [ Email ]
[ Email                ]    [ Phone ] [ Company ]  ← scattered, zig-zag
[ Phone                ]
```

---

## Label placement: top labels

- **Top-aligned labels** are the default and fastest to scan (label directly above field, 4–6px gap). They work on mobile, allow long labels without truncation, and keep a clean left edge.
- Avoid **left-aligned (in-line) labels** unless space-constrained and labels are short — they slow scanning and break on mobile.
- **Never** float the label as a placeholder-only (see "Placeholders are NOT labels").
- Label: 14px, medium weight, associated with the field (`<label htmlFor>` or wrapping). Clicking the label focuses the field.

---

## Input sizing & vertical rhythm

| Property | Value |
|---|---|
| Field height | 40–44px (touch-comfortable) |
| Font size | **≥16px on mobile** (prevents iOS zoom) |
| Gap label→field | 4–6px |
| Gap between fields | 16–24px |
| Gap between sections | 32–40px + heading/divider |

Consistent heights and spacing make the form feel calm and reduce errors.

---

## Grouping & sections

- Group related fields under a clear **section heading** ("Shipping address", "Payment"). Use `<fieldset>` + `<legend>` for groups, especially radio/checkbox groups (legend is the group's accessible name).
- Order fields in the sequence users think (name → email → password; not email → name → confirm-password → name-again).
- Keep each section short; long forms feel shorter when chunked. Consider multi-step for >~10 fields.

---

## Smart defaults

- Pre-fill what you can infer safely: country from locale/IP, today's date, the most common option.
- Don't pre-select choices that have consequences (don't pre-tick "subscribe me", don't pre-pick the expensive plan) — dark-pattern and erodes trust.
- Remember prior input (autofill, saved addresses). Make "same as billing" a one-click copy.

---

## Inline validation: timing is everything

The single biggest UX lever in forms. Get the timing right:

| Phase | Behavior |
|---|---|
| **First interaction (typing)** | Do NOT validate. Validating an email "as you type" before the user finishes flags `j`, `j@`, `j@x` as errors — punishing, useless. |
| **On blur (first time)** | Validate when the field loses focus. This is the moment to show an error if the value is invalid/incomplete. |
| **After first error, on change** | Once a field has shown an error, **re-validate on every keystroke** so the error clears the instant it's fixed — giving live positive feedback. |
| **On submit** | Validate everything; focus the first invalid field; surface an error summary. |

Rule of thumb: **validate on blur, re-validate on change after the first error, never validate while typing the first time.** Positive confirmation (green check) is fine to show on successful blur but use sparingly.

```tsx
// Validate on blur; after an error exists, re-validate on change
const [touched, setTouched] = useState(false);
const [error, setError] = useState<string | null>(null);

function validate(value: string) {
  if (!value) return "Email is required.";
  if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(value)) return "Enter a valid email, like name@example.com.";
  return null;
}

<input
  type="email"
  inputMode="email"
  autoComplete="email"
  aria-invalid={!!error}
  aria-describedby={error ? "email-error" : "email-help"}
  onBlur={(e) => { setTouched(true); setError(validate(e.target.value)); }}
  onChange={(e) => { if (touched) setError(validate(e.target.value)); }}
/>
```

---

## Error message wording

Error text must be **specific, actionable, human, and adjacent to the field**.

| Bad | Good |
|---|---|
| "Invalid input" | "Enter a valid email, like name@example.com." |
| "Error" | "Password must be at least 8 characters and include a number." |
| "This field is required" (generic) | "Enter your card's 3-digit security code." |
| "Something went wrong" | "We couldn't process that card. Check the number or try another." |

Rules:
- Say **what's wrong and how to fix it**, not just that it's wrong.
- Place the message **directly below the field**, in red, with a non-color cue (icon + text).
- Blame the system, not the user ("We couldn't…"), and never use shame ("You failed to…").
- Keep it short, plain language, no error codes in the primary message.

---

## Required vs optional marking

- If **most** fields are required, mark the **optional** ones with "(optional)" and skip the asterisks.
- If **most** are optional, mark required fields. Don't mark everything.
- The asterisk `*` alone is ambiguous and inaccessible — pair it with a legend ("* required") or just write "(optional)" / "(required)" in text.
- Set the native `required` attribute and `aria-required="true"` for SR users.

---

## Help text

- Persistent hint **below the label / above-or-below the field**, in muted text, for format or context ("We'll never share this", "8+ characters").
- Distinct from error text (help = neutral/gray, error = red). Both can be referenced via `aria-describedby` (space-separated ids).
- Keep it one short line; if you need a paragraph, the field is too complex — redesign.

---

## Placeholders are NOT labels

- Placeholder text disappears on input → users forget what the field is, can't review, and re-check by deleting.
- Low contrast placeholders fail accessibility; using them as the only label fails SR users entirely.
- Placeholders also get mistaken for pre-filled values.
- **Use a visible top label always.** Use placeholder only for an *example format* ("name@example.com", "MM/YY") — and even then, prefer help text.

```
Don't                          Do
[ Email address    ]  (label   [Email address]
   only as placeholder)        [ name@example.com ]   ← placeholder is an example, label is above
```

---

## Autofill / autocomplete attributes

Set `autocomplete` tokens so browsers/password managers fill correctly — a huge speed/error win:

| Field | `autocomplete` | also set |
|---|---|---|
| Email | `email` | `inputmode="email"`, `type="email"` |
| Name | `name` (or `given-name`/`family-name`) | |
| New password | `new-password` | |
| Current password | `current-password` | |
| One-time code | `one-time-code` | `inputmode="numeric"` |
| Street | `address-line1` / `address-line2` | |
| City / State / ZIP | `address-level2` / `address-level1` / `postal-code` | |
| Country | `country` / `country-name` | |
| Phone | `tel` | `type="tel"`, `inputmode="tel"` |
| Credit card | `cc-number`, `cc-exp`, `cc-csc`, `cc-name` | `inputmode="numeric"` |

Never disable autocomplete for these out of habit.

---

## Input types & mobile keyboards

Pick `type` and `inputmode` so mobile shows the right keyboard:

| Data | `type` | `inputmode` |
|---|---|---|
| Email | `email` | `email` |
| Phone | `tel` | `tel` |
| Numbers (codes, qty) | `text` | `numeric` (digits only, no spinner) |
| Decimal amounts | `text` | `decimal` |
| URL | `url` | `url` |
| Search | `search` | `search` |
| Date | `date` (native picker) or split inputs | |

Note: `type="number"` is poor for codes/phones (allows `e`, spinners, locale issues) — use `type="text"` + `inputmode="numeric"` + `pattern`.

---

## Multi-step forms & progress

- Split long forms (>~10 fields, or distinct stages like checkout) into steps. Fewer fields per screen = less intimidating, higher completion.
- Show a **progress indicator**: "Step 2 of 4" + a stepper with labels. Mark completed/current/upcoming states.
- Let users go **back without losing data**; persist between steps. Validate each step on its "Next".
- Put the lightest/easiest step first to build momentum; ask for sensitive info (payment) late.
- Keep a single logical chunk per step; don't fragment one address across three screens.

---

## Disabled vs error submit buttons

- **Don't disable the submit button** to "prevent" submission of an incomplete form. A disabled button gives no feedback about *what's* missing and is unreachable by keyboard/SR for explanation.
- **Prefer**: keep submit enabled → on click, validate → focus first invalid field + show errors + error summary.
- Acceptable to disable only briefly: during the in-flight request (with a loading spinner) to prevent double-submit.

---

## Success states

- On successful submit: clear, immediate confirmation. Inline ("Profile saved") for in-page saves; a confirmation screen/route for transactions (order placed) with next steps.
- Don't bury success in a toast that vanishes for consequential actions — give a persistent confirmation users can screenshot/reference.
- Reset the form or navigate forward; don't leave the user staring at the just-submitted form unsure if it worked.

---

## Accessibility

- **Label association**: every input has a programmatic label (`<label htmlFor>` / wrapping / `aria-label` as last resort). Test: clicking the label focuses the field.
- **`aria-invalid="true"`** on fields in error.
- **`aria-describedby`** points to help text and/or error message ids (space-separated).
- **Error summary** at top of form on submit (`role="alert"` or focusable heading) listing each error as a link that focuses its field — essential for long forms and SR users.
- **Focus management on submit**: move focus to the error summary or the first invalid field.
- **`<fieldset>`/`<legend>`** for radio/checkbox groups and logical sections.
- **Don't convey errors by color only** — icon + text always.
- Announce async results via `aria-live` (saving → saved → error).

---

## Complete annotated form example

```tsx
<form noValidate onSubmit={handleSubmit} aria-labelledby="form-title">
  <h1 id="form-title">Create your account</h1>

  {/* Error summary — rendered only after a failed submit, focused on appearance */}
  {submitAttempted && errors.length > 0 && (
    <div role="alert" tabIndex={-1} ref={summaryRef}
         className="rounded-md border border-red-300 bg-red-50 p-4">
      <h2 className="text-sm font-semibold text-red-800">
        Please fix {errors.length} {errors.length === 1 ? "error" : "errors"}:
      </h2>
      <ul className="mt-2 list-disc pl-5 text-sm text-red-700">
        {errors.map((e) => (
          <li key={e.field}>
            <a href={`#${e.field}`} className="underline">{e.message}</a>
          </li>
        ))}
      </ul>
    </div>
  )}

  {/* Top label, help text, single column */}
  <div className="mt-6">
    <label htmlFor="email" className="block text-sm font-medium">Email address</label>
    <p id="email-help" className="text-xs text-gray-500">We'll send a verification link here.</p>
    <input
      id="email" name="email" type="email" inputMode="email"
      autoComplete="email" required aria-required="true"
      aria-invalid={!!fieldErrors.email}
      aria-describedby={fieldErrors.email ? "email-error email-help" : "email-help"}
      placeholder="name@example.com"  /* example format, NOT the label */
      className="mt-1 h-11 w-full rounded-md border px-3 text-base
                 focus-visible:ring-2 focus-visible:ring-blue-500
                 aria-[invalid=true]:border-red-500"
      onBlur={validateField} onChange={revalidateIfTouched}
    />
    {fieldErrors.email && (
      <p id="email-error" className="mt-1 flex items-center gap-1 text-sm text-red-600">
        <AlertIcon className="h-4 w-4" aria-hidden /> {fieldErrors.email}
      </p>
    )}
  </div>

  {/* New password — correct autocomplete token for password managers */}
  <div className="mt-6">
    <label htmlFor="password" className="block text-sm font-medium">Password</label>
    <p id="password-help" className="text-xs text-gray-500">At least 8 characters, including a number.</p>
    <input
      id="password" name="password" type="password"
      autoComplete="new-password" required
      aria-describedby="password-help"
      className="mt-1 h-11 w-full rounded-md border px-3 text-base"
    />
  </div>

  {/* Grouped consent — checkbox for staged opt-in, NOT pre-checked */}
  <fieldset className="mt-6">
    <legend className="text-sm font-medium">Preferences</legend>
    <label className="mt-2 flex items-start gap-2">
      <input type="checkbox" name="newsletter" className="mt-0.5 h-5 w-5" />
      <span className="text-sm">Send me product updates (optional)</span>
    </label>
  </fieldset>

  {/* Submit stays ENABLED; disabled only while submitting to stop double-submit */}
  <button
    type="submit"
    aria-busy={isSubmitting}
    disabled={isSubmitting}
    className="mt-8 inline-flex h-11 w-full items-center justify-center gap-2
               rounded-md bg-blue-600 px-4 font-medium text-white
               hover:bg-blue-700 disabled:opacity-50"
  >
    {isSubmitting && <Spinner className="h-4 w-4 animate-spin" aria-hidden />}
    {isSubmitting ? "Creating account…" : "Create account"}
  </button>
</form>
```

Annotations: single column · top labels · help text via `aria-describedby` · placeholder is an *example* not the label · `autocomplete="new-password"` for managers · checkbox not pre-checked · submit enabled (only disabled in-flight) · error summary with deep links · `aria-invalid`/`aria-describedby` wired · `noValidate` so we control validation timing.

---

## Agent checklist

- [ ] Lay forms out in a single column; only pair short logically-grouped fields on one row.
- [ ] Use visible top labels associated with every field; never use placeholders as labels.
- [ ] Validate on blur, re-validate on change only after the first error, and never while typing the first time.
- [ ] Write error messages that state what's wrong and how to fix it, placed directly below the field with an icon.
- [ ] Mark the minority case (optional or required) explicitly; don't rely on a bare asterisk.
- [ ] Set correct `type`, `inputmode`, and `autocomplete` tokens so mobile keyboards and password managers work.
- [ ] Keep the submit button enabled; validate on click, focus the first invalid field, and show an error summary.
- [ ] Disable submit only during the in-flight request, with a spinner and `aria-busy`, to prevent double-submit.
- [ ] Wire `aria-invalid`, `aria-describedby`, `<fieldset>/<legend>`, and an error summary with deep links for a11y.
- [ ] Provide a clear, persistent success state and don't pre-check consequential options.
- [ ] For long forms, split into steps with a labeled progress indicator and preserve data across back/forward.
- [ ] Use ≥16px input font on mobile and consistent heights/spacing throughout.
