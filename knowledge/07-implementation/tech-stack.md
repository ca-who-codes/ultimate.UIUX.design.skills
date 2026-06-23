# Tech Stack

> Purpose: Pick the right modern web UI stack and wire it together correctly — the libraries, why each one, and the exact setup/patterns that make components fast, accessible, and maintainable.

**When to read this:** Before scaffolding any new web UI, choosing a component library, adding an animation/icon/theming dependency, or deciding "build vs. install." Pair it with [./recipes.md](./recipes.md) for copy-paste implementations.

This is the implementation backbone. For the visual rules these components must satisfy see [../03-components/components.md](../03-components/components.md); for tokens see [../02-foundations/design-tokens.md](../02-foundations/design-tokens.md); for a11y see [../05-quality/accessibility.md](../05-quality/accessibility.md); for motion see [../04-interaction/motion.md](../04-interaction/motion.md).

---

## The default recommended stack (2025–2026)

Use this unless a constraint says otherwise. Every choice is "boring, owned, accessible by default."

| Concern | Pick | Why this and not the alternative |
|---|---|---|
| Framework | **React + TypeScript** | Largest ecosystem, best headless-lib support, TS catches prop/variant bugs at author time. Use the framework your app already uses; don't introduce React into a Vue app. |
| Styling | **Tailwind CSS v4** | Utility-first = no naming bikeshed, no dead CSS, tokens live in one `@theme` block. v4 is CSS-first (no JS config), faster engine, native cascade layers. |
| Component layer | **shadcn/ui** | Not a dependency — you copy the source in and own it. Built on Radix primitives + Tailwind. Edit freely; no version-lock, no vendor lock. |
| Headless a11y primitives | **Radix UI** (default), **React Aria** (when you need more) | Unstyled, fully accessible behavior (focus, keyboard, ARIA) so you never reimplement a dialog's focus trap. |
| Animation | **Motion** (formerly Framer Motion) | Declarative `motion.*`, `AnimatePresence` for exits, layout animations, spring physics, `useReducedMotion`. The standard for React. |
| Icons | **Lucide** (`lucide-react`) | Consistent 24px grid, tree-shakeable, `stroke-width` controllable, 1400+ icons, MIT. |
| Dark mode | **next-themes** | Class strategy, no flash of wrong theme (FOUC), respects system, works in Next & plain Vite/React. |
| Variants | **cva** + **tailwind-merge** + **clsx** | `cva` declares variant→class maps with types; `clsx` joins conditionals; `tailwind-merge` resolves conflicting utilities. Together = the `cn()` utility. |
| Forms | **React Hook Form** + **Zod** (`@hookform/resolvers`) | Uncontrolled = fast, Zod schema is the single source of truth for validation + TS types. See [../03-components/forms.md](../03-components/forms.md). |

Install (Vite + React + TS already scaffolded):

```bash
npm i tailwindcss @tailwindcss/vite
npm i class-variance-authority clsx tailwind-merge lucide-react motion next-themes
npm i react-hook-form zod @hookform/resolvers
# Radix primitives are pulled in per-component by the shadcn CLI:
npx shadcn@latest init
npx shadcn@latest add button dialog dropdown-menu toast
```

---

## Tailwind CSS v4 setup essentials

v4 is **CSS-first**. There is no `tailwind.config.js` for tokens anymore — configuration lives in CSS via `@theme`. This is the single biggest change from v3.

### 1. Wire the engine

Vite (recommended): add the plugin, no PostCSS config needed.

```ts
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
});
```

### 2. One import + a `@theme` token block

```css
/* src/app.css */
@import "tailwindcss";

/* Dark mode driven by a class (so next-themes can toggle it). */
@custom-variant dark (&:where(.dark, .dark *));

@theme {
  /* Colors — define once, get bg-*, text-*, border-*, ring-* utilities free.
     Use OKLCH for perceptually-even scales (see ../02-foundations/color.md). */
  --color-bg:            oklch(1 0 0);
  --color-fg:            oklch(0.21 0.01 285);
  --color-muted:         oklch(0.97 0 0);
  --color-muted-fg:      oklch(0.55 0.01 285);
  --color-border:        oklch(0.92 0 0);
  --color-ring:          oklch(0.62 0.19 260);

  --color-primary:       oklch(0.62 0.19 260);
  --color-primary-fg:    oklch(0.98 0 0);
  --color-destructive:   oklch(0.58 0.22 27);
  --color-destructive-fg:oklch(0.98 0 0);

  /* Radii, fonts, spacing scale extensions, shadows — all tokens. */
  --radius:        0.625rem;
  --font-sans:     "Inter", ui-sans-serif, system-ui, sans-serif;
  --font-mono:     "JetBrains Mono", ui-monospace, monospace;
  --shadow-card:   0 1px 2px oklch(0 0 0 / 0.04), 0 4px 12px oklch(0 0 0 / 0.06);
}

/* Dark-mode token overrides — same variable names, swapped values. */
.dark {
  --color-bg:       oklch(0.16 0.005 285);
  --color-fg:       oklch(0.98 0 0);
  --color-muted:    oklch(0.24 0.006 285);
  --color-muted-fg: oklch(0.68 0.01 285);
  --color-border:   oklch(0.28 0.006 285);
}

/* Base layer: apply tokens to the document. */
@layer base {
  * { border-color: var(--color-border); }
  body { background: var(--color-bg); color: var(--color-fg); font-family: var(--font-sans); }
}
```

What you get for free: `--color-primary` → `bg-primary`, `text-primary`, `border-primary`, `ring-primary`, `fill-primary`, etc. `--radius` → use `rounded-[--radius]` or define `--radius-lg`/`--radius-sm` for `rounded-lg`/`rounded-sm`. **One source of truth**, light + dark, no JS.

> Migration note: if you inherit a v3 repo, `npx @tailwindcss/upgrade` handles most of it. `@tailwind base/components/utilities` becomes a single `@import "tailwindcss"`. `theme()` calls in CSS become `var(--color-*)`. Arbitrary opacity `bg-black/50` still works.

### 3. The `cn()` utility (non-negotiable)

Every component takes a `className` and merges it. `clsx` handles conditionals; `tailwind-merge` makes the *last* conflicting utility win so consumer overrides actually apply (`<Button className="bg-red-500">` beats the variant's `bg-primary`).

```ts
// src/lib/utils.ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

### 4. Variant-driven components with `cva`

`cva` turns a base + variant map into a typed function. Declare every variant once; `VariantProps` extracts the prop types. This is how shadcn/ui builds every component and how you should too.

```ts
import { cva, type VariantProps } from "class-variance-authority";

const badge = cva(
  "inline-flex items-center rounded-full border px-2.5 py-0.5 text-xs font-medium",
  {
    variants: {
      tone: {
        neutral:  "bg-muted text-muted-fg border-transparent",
        success:  "bg-emerald-500/15 text-emerald-700 dark:text-emerald-400 border-transparent",
        warning:  "bg-amber-500/15 text-amber-700 dark:text-amber-400 border-transparent",
        danger:   "bg-destructive/15 text-destructive border-transparent",
      },
    },
    defaultVariants: { tone: "neutral" },
  },
);
type BadgeProps = React.ComponentProps<"span"> & VariantProps<typeof badge>;
```

---

## shadcn/ui philosophy — you own the code

shadcn/ui is **not an npm package you depend on**. The CLI copies component source files into *your* repo (`components/ui/*`). Consequences:

- **No version lock, no breaking upgrades.** The code is yours; it never updates under you.
- **Edit freely.** Change a default variant, add a size, rip out a prop — it's just your file.
- **It's a starting point, not a black box.** Built on Radix + Tailwind + cva, so it follows exactly the patterns above.
- **Trade-off:** you maintain it. There's no `npm update` to get upstream fixes — you re-run `add` or patch by hand.

**Use shadcn/ui when:** you want production-quality, accessible primitives fast and you're on React + Tailwind. This is the default.

**Build custom when:** the interaction is genuinely novel (no Radix/Aria primitive fits), or you need a radically different DOM/animation that fighting the generated code would cost more than writing fresh. Even then, wrap a headless primitive — don't hand-roll focus traps.

**Don't:** install shadcn *and* another full styled component kit (MUI, Mantine, Chakra). Pick one styling philosophy. Mixing two design systems doubles bundle and halves consistency.

---

## Headless vs. styled libraries — when to reach for each

| Type | Examples | Reach for it when |
|---|---|---|
| **Headless / unstyled (behavior only)** | Radix UI, React Aria (Components), Headless UI, Ark UI, TanStack (Table/Virtual) | You want full visual control + correct a11y/keyboard behavior. **Default for any custom design system.** You bring the CSS. |
| **Styled, copy-in** | shadcn/ui (React), Reka UI / Nuxt UI (Vue), Bits UI / Melt (Svelte) | You want headless behavior *plus* a sensible default look you can edit. The pragmatic default. |
| **Styled, dependency** | MUI, Mantine, Chakra, Ant Design | Internal tools / admin dashboards where speed > pixel-perfect brand, and you accept their design language. Hard to make "not look like the library." |

Rule of thumb: **brand-critical surfaces → headless or copy-in; internal CRUD → a styled kit is fine.** Radix is the default behavior layer; reach for **React Aria** specifically when you need richer interactions Radix doesn't cover — date pickers, comboboxes with async, collections, drag-and-drop, internationalized number/date formatting, or fine-grained focus management.

---

## Accessibility you get for free with Radix / React Aria

Reimplementing these correctly is a multi-week trap. The primitives ship them:

- **Focus management** — focus trap inside dialogs/popovers, focus restored to the trigger on close, focus moved to the first focusable on open.
- **Keyboard interaction** — `Esc` to close, arrow-key roving tabindex in menus/tabs/radio groups, `Home`/`End`, type-ahead in selects, `Enter`/`Space` activation.
- **ARIA wiring** — `role`, `aria-expanded`, `aria-controls`, `aria-haspopup`, `aria-selected`, `aria-checked`, `aria-modal`, `aria-labelledby`/`aria-describedby` linked automatically.
- **Dismissal & layering** — click-outside, scroll-lock on body, `Esc` bubbling, correct stacking with portals, pointer/focus return.
- **Screen-reader announcements** — live regions for toasts/alerts, `VisuallyHidden` for labels.
- **Collision-aware positioning** — popovers/tooltips/menus flip and shift to stay in viewport (`@radix-ui/react-popper` / Floating UI under the hood).

If you build a dialog/menu/combobox/tooltip without one of these primitives, assume you've shipped an a11y bug. See [../05-quality/accessibility.md](../05-quality/accessibility.md) for the acceptance bar.

---

## Folder structure, naming, composition

```
src/
  components/
    ui/                # Owned primitives (shadcn-style): button.tsx, dialog.tsx, input.tsx…
    composed/          # App-level compositions built FROM ui/: data-table.tsx, page-header.tsx
  features/            # Feature-scoped UI + logic: features/billing/pricing-card.tsx
  lib/
    utils.ts           # cn(), formatters, small pure helpers
  hooks/               # useMediaQuery, useDebounce, useClickOutside…
  app.css              # @import "tailwindcss" + @theme tokens
```

Naming: files `kebab-case.tsx`, components `PascalCase`, hooks `useCamelCase`, variant fns lowercase (`buttonVariants`). Keep one primitive per file; export the component + its `variants` fn so consumers can reuse classes.

### Composition patterns you should use

**Compound components** — related parts share implicit state via context. Mirrors HTML structure, lets consumers reorder/omit parts:

```tsx
<Card>
  <Card.Header>
    <Card.Title>Plan</Card.Title>
    <Card.Description>Billed monthly</Card.Description>
  </Card.Header>
  <Card.Content>…</Card.Content>
  <Card.Footer><Button>Upgrade</Button></Card.Footer>
</Card>
```

**`asChild` / Slot** — Radix's `Slot` (also `@radix-ui/react-slot`) merges a component's props/behavior onto *your* child element instead of rendering its own wrapper. Lets a `Button` *become* an `<a>` or a router `<Link>` with zero wrapper div and full styling:

```tsx
<Button asChild>
  <Link to="/pricing">View pricing</Link>
</Button>
```

**Slots for layout flexibility** — accept `startContent`/`endContent`/`action` render props (or named children) so the same component handles icons, badges, and trailing buttons without a prop explosion.

Prefer **composition over configuration**: a `<Card.Footer>` you can fill with anything beats a `footerButtons={[…]}` array prop.

---

## Framework-agnostic alternatives (brief)

You don't have to use React. Same philosophy, different runtime:

- **Vue** — **Reka UI** (formerly Radix Vue) for headless primitives; **Nuxt UI** / **shadcn-vue** for copy-in styled components. Tailwind v4 works identically.
- **Svelte** — **Bits UI** + **Melt UI** (the headless builder layer) for behavior; **shadcn-svelte** for copy-in. Svelte 5 runes-friendly.
- **Solid** — **Kobalte** (headless) + **shadcn/ui Solid ports**.
- **Plain web / no framework** — modern CSS goes far: the native **Popover API** (`popover` attr + `popovertarget`), `<dialog>` element with `showModal()`, CSS anchor positioning, `@starting-style` for enter animations, and `:has()` for state. Use these before reaching for a library on simple sites.

Tailwind v4, cva, clsx, and tailwind-merge are framework-agnostic — they work in all of the above.

---

## Do NOT reinvent these — install the go-to instead

These are deceptively hard (a11y, edge cases, i18n, performance). Always install:

| Need | Go-to library | Notes |
|---|---|---|
| **Date / date-range picker** | React Aria `DatePicker`, or `react-day-picker` (shadcn Calendar) | Time zones, locales, keyboard nav, range logic. Never hand-roll. |
| **Combobox / autocomplete / async select** | `cmdk`, React Aria `ComboBox`, or Downshift | Filtering, async, keyboard, ARIA listbox. |
| **Command palette** | **cmdk** | The standard. See recipe in [./recipes.md](./recipes.md). |
| **Data table (sort/filter/page/group)** | **TanStack Table** (headless) | You own markup; it owns logic. Pair with TanStack Virtual for big sets. |
| **List / grid virtualization** | **TanStack Virtual** (or `react-virtuoso`) | Render only visible rows. Mandatory past ~hundreds of rows. |
| **Drag and drop** | **dnd-kit** | Accessible, keyboard-operable, sortable/sensors. Avoid `react-dnd` for new work. |
| **Charts** | **Recharts** (simple), **visx** / **Observable Plot** (custom), **ECharts** (dense/realtime) | Don't draw SVG charts by hand. See [../03-components/data-display.md](../03-components/data-display.md). |
| **Forms + validation** | **React Hook Form** + **Zod** | Schema = types + validation. |
| **Toast / notifications** | **sonner** (or Radix Toast) | Stacking, swipe, a11y live region handled. |
| **Tooltip / popover positioning** | Radix, or **Floating UI** directly | Collision flipping, never clipped by overflow. |
| **Carousel** | **Embla** | Accessible, no jank, headless. |
| **Rich text editor** | **Tiptap** (ProseMirror) or **Lexical** | Never build a contenteditable editor from scratch. |
| **Animation primitives** | **Motion** | `AnimatePresence`, layout, springs. |
| **Markdown rendering** | `react-markdown` + `rehype`/`remark` | Sanitize untrusted input. |

If it's on this list, installing is the *senior* call, not the lazy one.

---

## Complete annotated example: a production-quality Button

Everything above in one file. `cva` for variants/sizes, `cn()` for override-safe merging, Radix `Slot` for `asChild`, a built-in loading state that preserves width, and proper disabled/focus handling. This is `components/ui/button.tsx`.

```tsx
import * as React from "react";
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import { Loader2 } from "lucide-react";
import { cn } from "@/lib/utils";

// 1) Variant map — one source of truth for every visual permutation.
//    Base classes cover layout + focus ring + disabled + (svg) icon sizing.
const buttonVariants = cva(
  [
    "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-[--radius]",
    "text-sm font-medium transition-colors outline-none select-none",
    // Keyboard-only focus ring (see ../05-quality/accessibility.md):
    "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-bg",
    // Disabled is uniform across variants:
    "disabled:pointer-events-none disabled:opacity-50",
    // Auto-size any Lucide icon child, and stop pointer events on it:
    "[&_svg]:size-4 [&_svg]:shrink-0 [&_svg]:pointer-events-none",
  ],
  {
    variants: {
      variant: {
        primary:     "bg-primary text-primary-fg shadow-sm hover:bg-primary/90 active:bg-primary/95",
        secondary:   "border border-border bg-bg hover:bg-muted active:bg-muted/80",
        ghost:       "hover:bg-muted active:bg-muted/80",
        destructive: "bg-destructive text-destructive-fg shadow-sm hover:bg-destructive/90",
        link:        "text-primary underline-offset-4 hover:underline",
      },
      size: {
        sm:   "h-8 px-3 text-xs",
        md:   "h-10 px-4",            // 40px — default; pad hit area to 44 on touch
        lg:   "h-12 px-6 text-base",
        icon: "size-10",             // square icon-only; requires aria-label
      },
    },
    defaultVariants: { variant: "primary", size: "md" },
  },
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  /** Render onto the child element (e.g. an <a> or router <Link>) instead of a <button>. */
  asChild?: boolean;
  /** Show a spinner, disable interaction, keep width stable. */
  loading?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, loading = false, disabled, children, ...props }, ref) => {
    // 2) Slot lets <Button asChild><Link/></Button> work with zero wrapper div.
    //    Note: when asChild is set you can't also inject a spinner sibling,
    //    so loading + asChild are mutually exclusive by design.
    const Comp = asChild ? Slot : "button";

    return (
      <Comp
        ref={ref}
        // 3) cn(): variant classes first, consumer className LAST so it wins.
        className={cn(buttonVariants({ variant, size }), className)}
        // 4) Disable on loading; expose busy state to AT.
        disabled={disabled || loading}
        aria-busy={loading || undefined}
        {...props}
      >
        {loading && !asChild && (
          // Spinner replaces nothing — it sits inline; width stays stable because
          // the label is still rendered. For label-swap, hide children instead.
          <Loader2 className="animate-spin" aria-hidden="true" />
        )}
        {children}
      </Comp>
    );
  },
);
Button.displayName = "Button";

export { Button, buttonVariants };
```

Usage:

```tsx
<Button>Save</Button>
<Button variant="secondary" size="sm">Cancel</Button>
<Button variant="destructive" loading>Deleting…</Button>
<Button variant="ghost" size="icon" aria-label="Open menu"><Menu /></Button>
<Button asChild><Link to="/pricing">Pricing</Link></Button>
```

Why this is "production quality": single source of variants (no drifting class strings), override-safe (`tailwind-merge` lets consumers patch), polymorphic without wrapper divs (`asChild`), keyboard-focus ring only (`:focus-visible`), loading state that disables + announces (`aria-busy`) without layout shift, and tokens (`--radius`, `bg-primary`, `ring-ring`) so it re-themes instantly in dark mode. It satisfies the states matrix in [../03-components/components.md](../03-components/components.md).

---

## Agent checklist
- [ ] Default to React + TypeScript + Tailwind v4 + shadcn/ui (Radix) unless the host app dictates otherwise — match the existing framework, never bolt a second one on.
- [ ] Tailwind v4: one `@import "tailwindcss"`, define ALL tokens in a single `@theme` block, drive dark mode by `.dark` class overrides — no `tailwind.config.js` for design tokens.
- [ ] Ship the `cn()` helper (`clsx` + `tailwind-merge`) and route every component's `className` through it so consumer overrides win.
- [ ] Build variant-driven components with `cva` + `VariantProps`; export the variants fn alongside the component.
- [ ] Treat shadcn/ui as owned source you edit — not a dependency; don't mix it with a second styled kit (MUI/Mantine/Chakra).
- [ ] For any dialog/menu/combobox/tooltip/tabs, wrap a headless primitive (Radix default; React Aria for date/combobox/DnD/i18n) — never hand-roll focus traps, keyboard nav, or ARIA.
- [ ] Use `asChild`/Slot for polymorphism, compound components for structured UI, and composition over giant config-prop APIs.
- [ ] Do NOT reinvent date pickers, comboboxes, virtualization, drag-drop, tables, charts, toasts, or rich-text — install the named go-to.
- [ ] Add Motion for animation (with `useReducedMotion`), Lucide for icons, next-themes for dark mode, React Hook Form + Zod for forms.
- [ ] Verify the produced component satisfies the states matrix and a11y bar in [../03-components/components.md](../03-components/components.md) and [../05-quality/accessibility.md](../05-quality/accessibility.md).
