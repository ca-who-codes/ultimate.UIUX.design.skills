# UI/UX Ecosystem — Libraries, Tools & Sources of Craft

> Purpose: A curated, accurate map of the best component libraries, motion tools, primitives, and creative-tech projects an agent should reach for instead of reinventing.

**When to read this:** Before building a component from scratch, when the user wants a specific aesthetic ("animated", "premium", "bento", "3D"), or when choosing what to install. Prefer composing proven, accessible libraries over hand-rolling.

> Rule of thumb: **Own your styling, borrow your behavior.** Use headless primitives (Radix/React Aria) for correctness and accessibility; use copy-in component collections (shadcn/cult/ui-layouts) for speed and polish; only build from zero when nothing fits.

---

## 1. Headless / primitive layers (behavior + a11y, you style)

These solve the hard, easy-to-get-wrong parts: focus management, keyboard nav, ARIA, collisions, dismissal.

| Library | Use it for | Notes |
|---|---|---|
| **Radix UI Primitives** | Dialog, Popover, Dropdown, Tooltip, Tabs, Accordion, Select, Switch | The de-facto standard; shadcn/ui is built on it. Unstyled, accessible, composable. |
| **React Aria (Adobe)** | Same surface + complex widgets (date pickers, comboboxes, tables, drag-drop) | Most rigorous a11y; great for data-heavy apps. Hooks + components. |
| **Base UI** | Newer headless set from the Radix/MUI authors | Worth considering for greenfield. |
| **Ariakit** | Lower-level, very flexible primitives | When you need fine control. |
| **Headless UI** | Tailwind-team primitives (React/Vue) | Smaller surface; fine for simple needs. |

Non-React: **Reka UI** (Vue, formerly Radix Vue), **Bits UI** / **Melt UI** (Svelte), **Kobalte** (Solid). Plain HTML now has the native **Popover API** and **`<dialog>`** — use them for simple overlays.

See [tech-stack.md](tech-stack.md) for how these slot into the recommended stack.

---

## 2. Copy-in component collections (own the code)

Not npm dependencies — you copy components into your repo and own them. Best balance of speed + control.

| Source | What it is | Best for |
|---|---|---|
| **shadcn/ui** | Radix + Tailwind components you paste in via CLI | The default foundation. Buttons, forms, dialogs, tables, etc. |
| **cult/ui** (`nolly-studio/cult-ui`) | "Components crafted for Design Engineers. Styled with Tailwind, fully compatible with shadcn — copy and paste." MIT. | Premium **animated** React components (Framer Motion), AI-app patterns, polished blocks that drop into a shadcn project. |
| **ui-layouts** (`ui-layouts/uilayouts`) | "Not just a library — your complete front-end universe: components, effects, design tools, ready-to-use blocks." React + Tailwind + Framer Motion (+ Three.js for some effects). | Creative **effects & blocks**: image ripple/reveal, motion number, animated tabs, carousels (Embla), R3F blob, drag-and-drop, timelines. Reach here for marketing-site flair. |
| **Aceternity UI** | Flashy animated marketing components | Hero sections, backgrounds, spotlight effects. |
| **Magic UI** | Animated shadcn-compatible components | Marquees, tickers, particle/beam effects. |
| **Origin UI / Tremor** | Origin: large Tailwind component set. Tremor: dashboard/chart blocks. | Tremor is excellent for **dashboards** — see [../06-patterns/dashboards.md](../06-patterns/dashboards.md). |
| **Tailwind Plus (UI Blocks)** | Official paid marketing/app/ecommerce blocks | High-quality reference even when not purchased. |

How to choose: **shadcn/ui for the foundation → cult/ui + ui-layouts + Magic/Aceternity for animated accents.** Keep accents sparse (see motion budget in [../04-interaction/motion.md](../04-interaction/motion.md)).

---

## 3. Motion & animation

| Tool | Use it for |
|---|---|
| **Motion** (formerly Framer Motion) | The default for React animation: gestures, layout animations, `AnimatePresence` enter/exit, springs, scroll. |
| **GSAP** | Complex timelines, scroll-triggered sequences, SVG morphing, fine choreography. Framework-agnostic. |
| **Lenis** | Smooth scroll (use deliberately; respect `prefers-reduced-motion`). |
| **Native: View Transitions API + `@starting-style` + `@property`** | Page/route transitions and CSS-only enter animations with zero JS. Prefer when sufficient. |
| **Lottie / Rive** | Designer-authored vector animations (Rive is interactive + tiny runtime). |
| **Auto-Animate** | One-line list/layout transitions for simple cases. |

Always provide a reduced-motion fallback. Animate only `transform`/`opacity` for 60fps.

---

## 4. Specialized building blocks ("do not reinvent")

| Need | Reach for |
|---|---|
| Data tables / grids | **TanStack Table** (headless) → style yourself; AG Grid for enterprise. |
| Long lists | **TanStack Virtual** / react-virtuoso (virtualization). |
| Charts | **Recharts** (simple), **visx**/**D3** (custom), **Tremor** (dashboard), **Nivo**. See [../03-components/data-display.md](../03-components/data-display.md). |
| Command palette | **cmdk**. |
| Date/time | React Aria DatePicker, react-day-picker (shadcn calendar). |
| Forms | **React Hook Form** + **Zod** (validation). See [../03-components/forms.md](../03-components/forms.md). |
| Drag & drop | **dnd-kit**. |
| Toasts | **Sonner**. |
| Icons | **Lucide** (default), Phosphor, Radix Icons, Heroicons. |
| Dark mode | **next-themes**. |
| Carousels | **Embla**. |
| Rich text | **Tiptap**, Lexical. |
| Tables→PDF / export | as needed; keep out of the render path. |

---

## 5. Creative & advanced surfaces (when the brief is ambitious)

The references below expand what "UI" can mean — reach for them when the product calls for motion graphics, 3D, or signal-driven interfaces.

| Project | What it is | When to use |
|---|---|---|
| **Remotion** (`remotion-dev/remotion`) | "Make videos programmatically with React." React + Canvas/SVG/WebGL → real `.mp4`. | Data-driven video, animated reports, personalized media, automated social/marketing video, motion-design rendered from your design tokens. |
| **SuperSplat** (`playcanvas/supersplat`) | Free browser-based **3D Gaussian Splat editor** (TypeScript + WebGL/WebGPU on PlayCanvas). Inspect, edit, optimize, publish splats. | Photoreal 3D scenes/products on the web; immersive hero experiences; capture-to-web 3D. Pair with R3F for embedding. |
| **React Three Fiber + drei** | React renderer for Three.js | Interactive 3D, product configurators, WebGL hero scenes. |
| **RuView** (`ruvnet/RuView`) | Open-source **WiFi-sensing** platform — "turns commodity WiFi signals into real-time spatial intelligence… without a single pixel of video." Presence, vital signs, pose, fall detection (Rust/Python/ESP32 + ML). | Ambient / camera-free interfaces: presence-aware UIs, privacy-first smart-home and health dashboards, spatial/IoT control panels. Informs UX for sensor-driven, screenless or glanceable interfaces. |

These are *capability expanders*, not defaults. Most product UI never needs them — but when a brief says "immersive", "video", "3D", or "ambient/sensor-driven", this is where to look.

---

## 6. Design intelligence & skill sources (meta)

This repo stands on the shoulders of the broader "AI design skill" movement. Related, complementary sources:

- **ui-ux-pro-max-skill** (`nextlevelbuilder/ui-ux-pro-max-skill`) — "An AI skill that provides design intelligence for building professional UI/UX across platforms." Ships a Design-System Generator plus large libraries of industry reasoning rules, UI styles, color palettes, font pairings, and UX guidelines. A strong companion for *generating* opinionated design systems on demand.
- **StringTune Skill Hub** (`string-tune.fiddle.digital/skill-hub`) — an attribute-driven, "CSS-first, JS-light" interaction library philosophy ("No JS until you truly need it"). Good reminder that the most performant interaction is often the one with the least JavaScript.

This knowledge base is the *judgment layer* (principles, foundations, QA); the libraries above are the *materials*. Use both.

---

## 7. Inspiration & references (where to look when stuck)

When you need a visual target, mentally reference these standards of craft: Linear, Vercel, Stripe, Raycast, Arc, Family, Things, Superhuman, Mobbin (pattern library), Godly / Land-book / Refero (gallery sites), and the Tailwind/shadcn showcases. Match their level of polish: tight spacing, restrained palette, purposeful motion, obsessive state handling.

---

## Agent checklist

- [ ] Did I check for an existing primitive/component before building from scratch?
- [ ] Am I using a headless a11y primitive (Radix/React Aria) for any overlay, menu, or widget?
- [ ] Is shadcn/ui the foundation, with animated accents (cult/ui, ui-layouts, Magic UI) used *sparingly*?
- [ ] Did I pick the right specialized lib (TanStack Table/Virtual, cmdk, RHF+Zod, Sonner, Lucide) instead of hand-rolling?
- [ ] For animation, am I on Motion/GSAP/native View Transitions — and animating only transform/opacity with a reduced-motion fallback?
- [ ] Am I only reaching for 3D/video/sensor tech (R3F, SuperSplat, Remotion, RuView) when the brief actually demands it?
- [ ] Did I keep dependencies justified — every install earns its weight?
- [ ] Did I reference a known standard of craft (Linear/Stripe/Vercel-class) as the quality bar?
