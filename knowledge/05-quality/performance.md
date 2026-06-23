# Performance

> Purpose: Treat performance as a UX feature — hit Core Web Vitals, eliminate layout shift and jank, and make the interface *feel* instant through perceived-performance technique.

**When to read this:** Before shipping any view, when a page feels sluggish, and during every review. A beautiful UI that loads slowly or janks is a broken UI. Pair with [responsive.md](./responsive.md) for image strategy and [accessibility.md](./accessibility.md) for reduced-motion.

---

## Performance is UX

Users don't experience milliseconds — they experience *waiting*, *jumping*, and *lag*. The numbers below are proxies for feelings:

- **LCP** (loading) = "Is the thing I came for here yet?"
- **INP** (interactivity) = "Did the app react when I tapped?"
- **CLS** (stability) = "Why did the button move and I tapped the wrong thing?"

Every perf decision is a UX decision. The two human thresholds to memorize:

- **100ms** — feels instant. Respond to input within 100ms or it feels laggy.
- **1s** — keeps flow of thought. Beyond ~1s, users notice the wait; beyond ~10s they leave.

---

## Core Web Vitals — targets and fixes

Measured at the **75th percentile** on real users. "Good" thresholds:

| Metric | Good | Needs work | Poor | Measures |
|--------|------|-----------|------|----------|
| **LCP** (Largest Contentful Paint) | **< 2.5s** | 2.5–4s | > 4s | Time to render the largest visible element |
| **INP** (Interaction to Next Paint) | **< 200ms** | 200–500ms | > 500ms | Responsiveness across *all* interactions |
| **CLS** (Cumulative Layout Shift) | **< 0.1** | 0.1–0.25 | > 0.25 | Visual stability (unexpected movement) |

> INP replaced FID (First Input Delay) in March 2024. INP is stricter: it measures every interaction, not just the first, and counts the full input→paint cycle.

### Core Web Vital → concrete fix

| Symptom | Root cause | Fix |
|---------|-----------|-----|
| **LCP > 2.5s** | LCP image lazy-loaded / late | Set `fetchpriority="high"`, **don't** lazy-load hero, `<link rel="preload">` it |
| LCP slow | Render-blocking CSS/JS | Inline critical CSS, `defer`/`async` scripts, code-split |
| LCP slow | Slow server / no caching | Cache, CDN, edge render, reduce TTFB |
| LCP slow | Web font blocks text | `font-display: swap` + preload the font |
| **INP > 200ms** | Long JS tasks block main thread | Break up work, `requestIdleCallback`, web worker, debounce |
| INP slow | Heavy event handler | Yield with `await scheduler.yield()` / `setTimeout`, memoize |
| INP slow | Huge DOM / re-render storm | Virtualize lists, reduce nodes, avoid unnecessary re-renders |
| INP slow | Hydration blocking | Stream/partial hydrate, defer non-critical JS |
| **CLS > 0.1** | Images without dimensions | Set `width`/`height` or `aspect-ratio` |
| CLS | Web font swap reflow | Match fallback metrics (`size-adjust`), preload |
| CLS | Injected ads/banners/embeds | Reserve space with min-height placeholders |
| CLS | Content inserted above viewport | Append below or reserve the slot |
| CLS | Non-composited animations moving layout | Animate `transform`, not `top`/`width` |

Also watch **TTFB** (< 800ms) and **FCP** (< 1.8s) as upstream signals.

---

## Perceived performance — feel fast, even when you aren't

Often you can't make work faster, but you can change how waiting *feels*. This is the highest-leverage, most-overlooked area.

- **Respond to every input within 100ms** — at minimum show a pressed state, spinner, or disabled button. Silence after a tap reads as "broken."
- **Skeleton screens > spinners** for content loads. A skeleton mirrors the final layout, signals progress, and prevents the jarring spinner→content swap. Spinners are fine for short, indeterminate waits (< 1s).

```html
<!-- Skeleton placeholder that matches the real card's shape -->
<div class="card skeleton" aria-hidden="true">
  <div class="sk sk-img"></div>
  <div class="sk sk-line w-70"></div>
  <div class="sk sk-line w-40"></div>
</div>
```

```css
.sk { background: linear-gradient(90deg,#eee 25%,#f5f5f5 37%,#eee 63%);
      background-size: 400% 100%; animation: shimmer 1.4s ease infinite; border-radius:.4rem; }
@keyframes shimmer { 0%{background-position:100% 0} 100%{background-position:-100% 0} }
@media (prefers-reduced-motion: reduce){ .sk{ animation:none } }
```

- **Optimistic UI** — update the UI immediately on action, assuming success; reconcile/rollback if the server disagrees. A "like" should fill instantly, not after a round-trip.
- **Stale-while-revalidate** — show cached data instantly, refresh in the background.
- **Progressive loading** — render the shell + above-the-fold first, stream the rest. Prioritize what the user sees.
- **Prefetch on intent** — preload the next route on link hover/touchstart so navigation feels instant.
- **Show progress, not just motion** — a determinate bar with real progress beats an infinite spinner for long tasks.

---

## Preventing layout shift (CLS = 0)

Layout shift is the most *visible* perf failure — content jumps under the user's finger. Eliminate it at the source:

- **Always set image/video dimensions.** `width`/`height` attributes (browser computes `aspect-ratio`) or CSS `aspect-ratio: 16/9`.

```css
img, video { aspect-ratio: attr(width) / attr(height); height: auto; max-width: 100%; }
.media { aspect-ratio: 16 / 9; } /* reserves the box before the asset loads */
```

- **Reserve space for anything async** — embeds, ads, dynamically injected banners get a `min-height`.
- **Fonts: `font-display: swap` + matched fallback metrics** so the swap from fallback to web font doesn't reflow:

```css
@font-face {
  font-family: "Inter"; src: url(/inter.woff2) format("woff2");
  font-display: swap;
}
/* Fallback tuned to occupy the same space → near-zero swap shift */
@font-face {
  font-family: "Inter-fallback"; src: local("Arial");
  size-adjust: 107%; ascent-override: 90%; descent-override: 22%; line-gap-override: 0%;
}
body { font-family: "Inter", "Inter-fallback", sans-serif; }
```

- **Never insert content above existing content** once the page is interactive (cookie bars, notifications) — overlay it or reserve the slot.
- **Animate transform/opacity only** (see below) so motion never reflows the page.

---

## Image optimization

Images are usually the heaviest bytes on a page and the most common LCP element.

- **Right size:** serve via `srcset`/`sizes` so a phone never downloads a 2000px file (full pattern in [responsive.md](./responsive.md)).
- **Modern formats:** AVIF (smallest) → WebP → JPEG/PNG fallback via `<picture>`. AVIF/WebP cut 30–70% off JPEG.
- **LCP hero:** preload + `fetchpriority="high"`, and **never** `loading="lazy"` it.

```html
<link rel="preload" as="image" href="hero.avif" fetchpriority="high">
```

- **Below the fold:** `loading="lazy" decoding="async"`.
- **Compress:** target quality ~75–80; strip metadata. SVG for icons/logos (and minify it).
- **Dimensions always set** to prevent CLS.

---

## Font loading strategy

Fonts block text rendering and cause swap shift. Discipline:

- **`woff2` only** (best compression, universal support).
- **Subset** to the characters/weights you use; drop unused glyph ranges and weights.
- **Self-host** critical fonts (a third-party CDN adds a connection + DNS + TLS to your critical path).
- **Preload** the fonts used above the fold: `<link rel="preload" as="font" type="font/woff2" crossorigin href="/inter.woff2">`.
- **`font-display: swap`** so text shows immediately in the fallback (no invisible-text FOIT).
- **Match fallback metrics** (`size-adjust`, `ascent-override`) to kill swap-induced CLS (snippet above).
- Limit to 1–2 families and the weights you actually render; variable fonts can replace many static weights with one file.

---

## Code splitting & lazy loading

Ship less JS to start; load the rest on demand.

- **Route-based splitting:** each route is its own chunk — don't make the homepage download the settings page's code.
- **Component-level lazy load** for heavy, below-the-fold, or interaction-gated UI (modals, charts, rich editors, maps):

```js
const Chart = React.lazy(() => import("./Chart"));   // loaded only when rendered
// <Suspense fallback={<Skeleton/>}><Chart/></Suspense>
```

- **Defer non-critical scripts:** `<script defer>` for app code, `async` for independent analytics. Nothing render-blocking in `<head>` except critical CSS.
- **Tree-shake** — import only what you use (`import { debounce } from 'lodash-es'`, not the whole lib).
- **Lazy-load offscreen images/iframes** natively (`loading="lazy"`).
- **Preload/prefetch intelligently:** `rel="preload"` for must-have-now, `rel="prefetch"` for likely-next-route.

---

## The cost of animations — transform & opacity only

Animations run cheaply *only* when they stay off the main thread and skip layout/paint. The browser pipeline is **Layout → Paint → Composite**. Animating geometric properties forces Layout (reflow) every frame — instant jank.

| Animate this (compositor-only, cheap) | Never animate this (forces layout/paint) |
|---------------------------------------|------------------------------------------|
| `transform` (translate/scale/rotate) | `top` / `left` / `right` / `bottom` |
| `opacity` | `width` / `height` / `margin` / `padding` |
| `filter` (mostly composited) | `box-shadow` (paint-heavy — fake with a layered element) |

```css
/* DON'T — animating left triggers layout every frame → jank */
.menu { transition: left .3s; }  .menu.open { left: 0; }

/* DO — transform is GPU-composited, smooth 60fps */
.menu { transform: translateX(-100%); transition: transform .3s ease; }
.menu.open { transform: translateX(0); }
```

- Hint the compositor with `will-change: transform` on elements about to animate — but **remove it after**; leaving it on wastes GPU memory.
- Keep frames under **~16ms** (60fps) — under ~8ms for 120Hz. Profile with DevTools Performance.
- Honor `prefers-reduced-motion` (see [accessibility.md](./accessibility.md)).

---

## Avoiding jank

Jank = dropped frames during scroll, type, drag, or animation. Keep the main thread free.

- **Debounce** input that triggers expensive work (search-as-you-type, resize): run after the user pauses.

```js
const debounce = (fn, ms=300) => { let t; return (...a)=>{ clearTimeout(t); t=setTimeout(()=>fn(...a),ms); }; };
input.addEventListener("input", debounce(search, 300));
```

- **Throttle** continuous events (scroll, mousemove, drag): cap to one run per frame (`requestAnimationFrame`).
- **Virtualize long lists** — render only the visible rows + a buffer (react-window / TanStack Virtual / `content-visibility: auto`). A 10,000-row table renders ~20 nodes.

```css
.list-item { content-visibility: auto; contain-intrinsic-size: 0 64px; } /* skip offscreen render */
```

- **Batch DOM reads/writes** — never interleave reads (`offsetWidth`) and writes in a loop; that thrashes layout. Read all, then write all.
- **Move heavy compute to a Web Worker** so the main thread stays responsive.
- **Passive listeners** for scroll/touch: `addEventListener('scroll', fn, { passive: true })`.

---

## Bundle & asset budgets

Set hard limits and fail the build when exceeded — perf decays silently without budgets.

| Asset | Budget (compressed) | Notes |
|-------|--------------------|-------|
| Initial JS (critical path) | **< 170KB** gzip/brotli | The biggest lever on INP/TTI |
| Initial CSS | < 60KB | Inline critical, defer the rest |
| Total page weight (initial) | < 1–1.5MB | Mobile data + battery |
| Largest single image | < 200KB | Compress + modern format |
| Web fonts | < 100KB total | woff2, subset, ≤ 2 families |
| Time to Interactive (mid-tier mobile) | < 3.5s | Test on throttled 4G + 4× CPU |
| DOM nodes | < ~1500 | Bloated DOM hurts INP and memory |

- Test on a **throttled mid-tier device** (DevTools: 4× CPU slowdown, Slow 4G), not your dev machine.
- Track real users with field data (CrUX / RUM), not just lab (Lighthouse). Lab finds issues; field proves them.

---

## Agent checklist

- [ ] Hit **LCP < 2.5s, INP < 200ms, CLS < 0.1** at p75; verify in Lighthouse *and* field data.
- [ ] Make the LCP element fast: **preload + `fetchpriority="high"`**, never lazy-load the hero.
- [ ] Respond to every interaction within **100ms**; show a pressed/loading state immediately.
- [ ] Use **skeletons** for content loads and **optimistic UI** for actions; prefetch the next route on intent.
- [ ] Drive **CLS to 0**: set image/media dimensions or `aspect-ratio`, reserve space for async content, match font fallback metrics.
- [ ] Serve **AVIF/WebP**, right-sized via `srcset`, compressed (~75–80 quality), lazy below the fold.
- [ ] Self-host, subset, and **preload woff2** fonts with `font-display: swap`.
- [ ] **Code-split by route** and lazy-load heavy/offscreen components; `defer` non-critical JS.
- [ ] Animate **`transform`/`opacity` only**; never animate `width`/`top`/`left`; keep frames < 16ms.
- [ ] **Debounce/throttle** expensive handlers and **virtualize** long lists.
- [ ] Enforce **bundle budgets** (< 170KB initial JS) and fail the build on regression.
- [ ] Profile on a **throttled mid-tier mobile**, not your laptop.
