# States & Feedback

> Purpose: Enforce the full lifecycle of UI states — empty, loading, error, success, partial, offline, disabled — that agents constantly forget, leaving views that only work in the happy path.

**When to read this:** Before declaring any view, list, form, or data-driven component "done." Every screen that fetches, submits, or displays variable data must handle every state below. This is the #1 source of shipped-but-broken UIs.

The single most common AI failure mode is building only the **ideal/populated** state — the view as it looks with perfect data, fast network, and no errors. Real users hit empty lists, slow networks, failed requests, and offline moments constantly. A view that ignores these isn't 80% done; it's broken for a large fraction of sessions.

---

## 1. Every view must handle these states

For any data-driven view, walk this table before shipping. If a cell is "—", justify why it can't occur.

| State | What it is | Must provide |
|---|---|---|
| **Ideal / populated** | Normal data present | The real content, designed first but not *only* |
| **Loading** | Data in flight | Skeleton (content) or spinner (<1s/unknown); no layout shift on resolve |
| **Empty** | No data — and WHICH kind of empty (see §2) | Context-appropriate copy + a CTA, never a blank box |
| **Error** | Request/action failed | Plain-language cause + a recovery action; never a dead end |
| **Partial / degraded** | Some data loaded, some failed or pending | Show what you have; mark the rest as failed/loading |
| **Success / confirmation** | Action completed | Clear, proportional confirmation; reflect the new state |
| **Offline** | No connectivity | Detect it, communicate it, queue or block writes gracefully |
| **Disabled** | Action unavailable | Explain WHY; prefer enabled-with-validation (see §8) |

```
DON'T: ship a <DataTable> that renders [] as an empty <table> with headers and no rows
DO:    branch: loading → skeleton, error → retry, empty → CTA, data → rows
```

A minimal, correct render tree:

```tsx
if (isLoading)        return <Skeleton rows={6} />;
if (error)            return <ErrorState onRetry={refetch} message={humanize(error)} />;
if (!data?.length)    return <EmptyState variant={searchQuery ? "no-results" : "first-use"} />;
return <List items={data} />;
```

---

## 2. Empty states (three different kinds)

"Empty" is not one state. Each kind needs different copy and a different CTA. Conflating them is a classic mistake.

| Variant | When | Copy tone | CTA |
|---|---|---|---|
| **First-use** (zero data ever) | Brand-new account / feature | Encouraging, explains the value | Primary action to create the first item ("Create your first project") |
| **No-results** (filter/search empty) | A query returned nothing | Helpful, suggests adjusting | "Clear filters" / "Try a different search"; show the active query |
| **Cleared** (user emptied it) | Everything done/deleted | Affirming, "all caught up" | Optional secondary action; can celebrate ("Inbox zero 🎉") |

Anatomy of a good empty state: a light illustration or icon (optional), a one-line headline, a sentence of guidance, and **exactly one clear CTA**. Never a blank region, never a lonely "No data."

```
DON'T: same "No items found" for a new user AND a no-match search
DO:    first-use → "Create your first invoice"; no-results → "No invoices match 'Q3' — clear filter"
```

```tsx
function EmptyState({ variant, query, onClearFilters, onCreate }) {
  if (variant === "no-results") return (
    <Centered icon={<SearchX />}>
      <h3>No results for “{query}”</h3>
      <p>Try a different term or clear your filters.</p>
      <Button variant="secondary" onClick={onClearFilters}>Clear filters</Button>
    </Centered>
  );
  if (variant === "cleared") return (
    <Centered icon={<CheckCircle />}><h3>All caught up</h3><p>Nothing needs your attention.</p></Centered>
  );
  return ( // first-use
    <Centered icon={<Plus />}>
      <h3>No projects yet</h3>
      <p>Projects organize your work. Create one to get started.</p>
      <Button onClick={onCreate}>Create your first project</Button>
    </Centered>
  );
}
```

---

## 3. Loading states

The goal is **perceived performance**, not just honesty. Choose the right indicator by duration and content type.

| Situation | Use | Why |
|---|---|---|
| Loading content with known shape (list, card, profile) | **Skeleton screen** | Communicates structure, reduces perceived wait, no layout shift |
| Action < 1s, or unknown/short duration | **Spinner** (small, in-context) | Cheap; skeleton would flash and feel worse |
| Inline action (button submit) | **In-button spinner / label swap** | Keeps context; disables re-submit |
| Determinate task > 2s (upload, export, multi-step) | **Progress bar/ring with %** | Sets expectations; reduces abandonment |
| Background refresh of visible data | **Stale-while-revalidate** | Show cached data instantly; update in place; subtle "refreshing" hint |
| Very fast (<300ms) | **Nothing** (or delay the spinner) | A spinner that flashes for 150ms looks like jank |

**Skeleton > spinner for content.** A spinner is a void that says "wait"; a skeleton says "your content looks like *this*, it's almost here." Match skeleton shapes to the real layout so there's **zero layout shift** when data arrives.

**Avoid spinner flashes:** delay showing any spinner by ~200–300ms; if data arrives first, the user never sees a flicker. Likewise, once a skeleton/spinner shows, keep it up for a minimum (~300–500ms) to avoid a jarring flash.

**Optimistic updates** (covered in [microinteractions.md](./microinteractions.md) §3): for user-initiated writes, skip the loading state entirely — show the result immediately and reconcile in the background.

**Stale-while-revalidate:** never blank out already-visible content to refetch it. Keep the old data on screen, fetch underneath, swap in place. Libraries like SWR / TanStack Query do this by default — don't fight it.

```tsx
// Skeleton matched to the real card → no layout shift on resolve
function CardSkeleton() {
  return (
    <div className="card animate-pulse">
      <div className="h-10 w-10 rounded-full bg-muted" />
      <div className="mt-3 h-4 w-3/4 rounded bg-muted" />
      <div className="mt-2 h-4 w-1/2 rounded bg-muted" />
    </div>
  );
}
```

```
DON'T: replace a full page of loaded data with a centered spinner to refresh it
DO:    keep the data visible, refetch underneath, show a subtle top-bar progress
```

---

## 4. Error states

The rule: **never dead-end.** Every error must tell the user *what happened, in plain language* and *what they can do next*. Choose the surface by scope and recoverability.

| Surface | Use for | Recovery |
|---|---|---|
| **Inline** (next to the field/section) | Validation, a single failed field, a partial failure | Fix-in-place; the rest of the page works |
| **Toast** | Transient, non-blocking failures (a save retry, a background sync) | "Retry" action in the toast; auto-dismiss after ~6s |
| **Full-page / boundary** | The whole view can't render (fetch failed, 500, crash) | "Try again" button, link home, support path |
| **Banner** | Persistent degraded condition affecting the page | Stays until resolved; explains the limitation |

Writing errors:
- Say what happened in human terms: "Couldn't save your changes — you're offline" not "Error 0x80004005".
- Say what to do next: a button (Retry), an instruction (Check your connection), or a path (Contact support with code X).
- Never blame the user; never expose stack traces/IDs except as a copyable support code.
- Preserve their work — never clear a form because submission failed.

```tsx
function ErrorState({ message, onRetry }) {
  return (
    <Centered icon={<AlertTriangle />} role="alert">
      <h3>Something went wrong</h3>
      <p>{message ?? "We couldn’t load this. Please try again."}</p>
      <Button onClick={onRetry}>Try again</Button>
    </Centered>
  );
}
// Wrap views in an error boundary so a render crash → recoverable UI, not a white screen.
```

```
DON'T: toast "Error" and leave the user staring at a blank page
DON'T: clear the form on a failed submit
DO:    keep their input, explain the cause, offer Retry inline
```

---

## 5. Success / confirmation feedback

Confirmation must be **proportional** to the action's weight and **reflect the new state**.

| Action weight | Confirmation |
|---|---|
| Trivial / reversible (toggle, like, edit field) | The state change itself is the confirmation; optionally a brief inline ✓ |
| Standard (save, send, add) | Toast or inline "Saved" + the UI reflecting the change | 
| Significant / irreversible (delete, purchase, publish) | Explicit success screen/modal, often with next steps; precede with a confirm step |

Principles:
- **Reflect, don't just announce.** After "Saved", the data should *visibly* be saved (button reverts to default, "edited" badge clears). A toast alone, with no state change, feels hollow.
- Keep success feedback brief and non-blocking for common actions (toast ~3s, auto-dismiss).
- For destructive/irreversible actions, prefer an **undo** affordance over a pre-confirm dialog where feasible ("Deleted. Undo" toast beats "Are you sure?") — it's faster and safer.
- Don't celebrate routine actions (see [microinteractions.md](./microinteractions.md) §6).

```
DON'T: "Saved!" toast while the form still shows the old/dirty state
DO:    persist, clear the dirty indicator, then (optionally) a quiet "Saved" toast
```

---

## 6. Partial / degraded states

Real systems load in pieces. When some data arrives and some fails or lags, **show what you have** and mark the rest — don't block the whole view on the slowest/most-broken part.

- Render each independent region with its own state. A failed "recommendations" widget shouldn't blank the whole dashboard.
- Mark degraded regions explicitly: "Couldn't load comments — Retry" inside an otherwise-working post.
- For paginated/infinite lists: show loaded items, a loading row at the bottom, and an error row with retry if the next page fails — keep prior items intact.
- Feature-detect and degrade gracefully: if a non-critical capability is missing, hide/disable that piece, keep the core working.

```
DON'T: one fetch fails → entire dashboard shows a single error page
DO:    that widget shows its own error+retry; the rest of the dashboard works
```

---

## 7. Offline states

Detect connectivity, communicate it, and handle writes deliberately.

- **Detect:** `navigator.onLine` + `online`/`offline` events (treat as a hint, not gospel; confirm with a request when it matters).
- **Communicate:** a persistent, unobtrusive banner ("You're offline — changes will sync when you reconnect"), not a blocking modal.
- **Reads:** serve cached data (service worker / query cache) and label it as potentially stale.
- **Writes:** queue them and replay on reconnect (optimistic + outbox), or clearly disable write actions with an explanation. Never let a write silently vanish.
- **Recovery:** when back online, sync, resolve the banner, and confirm ("Back online — synced").

```tsx
const online = useOnline(); // wraps navigator.onLine + online/offline events
{!online && <Banner tone="warning">You’re offline. Changes will sync when you reconnect.</Banner>}
```

```
DON'T: let a tap on "Send" do nothing (and lose the message) while offline
DO:    queue it, show "Pending", flush on reconnect — or disable Send with a reason
```

---

## 8. Disabled states

Disabled controls are a usability trap: a greyed-out button that gives no reason is a dead end. The user knows *what* they can't do but not *why* — so they can't fix it.

Order of preference:
1. **Enabled-with-validation (best).** Let the user click; on click, surface the specific blocker ("Add at least one recipient before sending"). This teaches and unblocks in one step.
2. **Disabled with a visible reason.** If you must disable, show the reason inline or in a tooltip *that's reachable on touch too* — a hover-only tooltip on a disabled control is invisible on mobile and often un-focusable.
3. **Hide entirely.** If the action is irrelevant in this context (not just temporarily blocked), remove it rather than disabling.

Accessibility: a truly disabled control (`disabled`) isn't focusable and announces nothing — screen-reader and keyboard users hit a wall. Prefer `aria-disabled="true"` on a still-focusable element when you need to explain the reason, or use option 1.

```
DON'T: <button disabled> with no hint why → user is stuck
DO:    enabled button → on click, show "Enter a valid email to continue"
DO:    or aria-disabled + an always-visible reason next to it
```

| Pattern | Use when | Caveat |
|---|---|---|
| Enabled + validate on submit | Most forms / gated actions | Requires good inline error messaging |
| `aria-disabled` + visible reason | Action genuinely blocked, reason explainable | Reason must be visible on touch, not hover-only |
| `disabled` attribute | Truly inert, reason obvious from context | Not focusable; no SR announcement |
| Hidden | Action irrelevant in this context | Don't hide things users expect to find |

---

## 9. Putting it together — a checklist render

```tsx
function View() {
  const { data, error, isLoading, isFetching, refetch } = useQuery(...);
  const online = useOnline();

  return (
    <>
      {!online && <OfflineBanner />}
      {isFetching && !isLoading && <TopProgressBar />}        {/* stale-while-revalidate */}
      {isLoading        ? <Skeleton />
        : error         ? <ErrorState onRetry={refetch} message={humanize(error)} />
        : !data?.length ? <EmptyState variant={query ? "no-results" : "first-use"} />
        :                 <List items={data} onFailedRow={...} /> /* partial handled per-row */}
    </>
  );
}
```

Related reading: [microinteractions.md](./microinteractions.md) for optimistic updates and confirmation moments, [motion.md](./motion.md) for how skeletons, toasts, and state transitions should animate.

---

## Agent checklist
- [ ] For every data view, explicitly handle: ideal, loading, empty, error, partial, success, offline, disabled.
- [ ] Split "empty" into first-use, no-results, and cleared — each with distinct copy and a CTA; never a blank box.
- [ ] Use skeletons (matched to real layout, zero layout shift) for content; spinners only for <1s or unknown waits.
- [ ] Delay spinners ~200–300ms to avoid flashes; keep stale data visible while revalidating.
- [ ] Show determinate progress for any task over ~2s.
- [ ] Make every error state recoverable: plain-language cause + a retry/next action; never dead-end; preserve user input.
- [ ] Pick the error surface by scope: inline (field) / toast (transient) / full-page (whole view).
- [ ] Make success feedback proportional and reflect the new state; prefer "Undo" toasts over pre-confirm dialogs.
- [ ] Render partial/degraded regions independently — one failed widget must not blank the page.
- [ ] Detect offline, show a non-blocking banner, and queue or clearly block writes — never silently drop them.
- [ ] Prefer enabled-with-validation over disabled; if disabled, show the reason visibly (reachable on touch).
- [ ] Wrap views in an error boundary so a render crash degrades to a recoverable UI, not a white screen.
