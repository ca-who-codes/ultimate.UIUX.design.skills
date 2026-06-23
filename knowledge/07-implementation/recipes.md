# Recipes

> Purpose: A cookbook of copy-paste-quality, accessible UI implementations — Modal, Toast, Card, form field, skeleton, empty state, command palette, data table, scroll reveal, theme toggle, navbar, pricing card.

**When to read this:** When you need a correct, accessible implementation of a common pattern *now*. Each recipe is self-contained, production-grade, and follows the stack in [./tech-stack.md](./tech-stack.md). For the rules behind them: components [../03-components/components.md](../03-components/components.md), motion [../04-interaction/motion.md](../04-interaction/motion.md), a11y [../05-quality/accessibility.md](../05-quality/accessibility.md), forms [../03-components/forms.md](../03-components/forms.md).

All recipes assume the `cn()` helper and Tailwind v4 tokens from [./tech-stack.md](./tech-stack.md). React + TypeScript throughout.

---

## 1. Accessible Modal / Dialog

**Use when:** confirming a destructive action, focused sub-task, or short form that must interrupt. Built on Radix Dialog so focus trap, scroll lock, `Esc`, click-outside, and ARIA come free — don't hand-roll these.

```tsx
// components/ui/dialog.tsx
import * as React from "react";
import * as DialogPrimitive from "@radix-ui/react-dialog";
import { X } from "lucide-react";
import { cn } from "@/lib/utils";

export const Dialog = DialogPrimitive.Root;
export const DialogTrigger = DialogPrimitive.Trigger;
export const DialogClose = DialogPrimitive.Close;

export function DialogContent({
  className,
  children,
  ...props
}: React.ComponentProps<typeof DialogPrimitive.Content>) {
  return (
    <DialogPrimitive.Portal>
      {/* Overlay: fades; data-state hooks drive enter/exit animation. */}
      <DialogPrimitive.Overlay
        className={cn(
          "fixed inset-0 z-50 bg-black/50 backdrop-blur-sm",
          "data-[state=open]:animate-in data-[state=open]:fade-in-0",
          "data-[state=closed]:animate-out data-[state=closed]:fade-out-0",
        )}
      />
      <DialogPrimitive.Content
        // Radix moves focus in, traps it, restores on close, sets aria-modal.
        className={cn(
          "fixed left-1/2 top-1/2 z-50 w-full max-w-lg -translate-x-1/2 -translate-y-1/2",
          "rounded-[--radius] border border-border bg-bg p-6 shadow-card",
          "data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95",
          "data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95",
          "motion-reduce:data-[state=open]:animate-none motion-reduce:data-[state=closed]:animate-none",
          className,
        )}
        {...props}
      >
        {children}
        <DialogPrimitive.Close
          className="absolute right-4 top-4 rounded-sm opacity-60 transition-opacity hover:opacity-100 focus-visible:ring-2 focus-visible:ring-ring focus-visible:outline-none"
          aria-label="Close"
        >
          <X className="size-4" />
        </DialogPrimitive.Close>
      </DialogPrimitive.Content>
    </DialogPrimitive.Portal>
  );
}

// Title/Description are REQUIRED for a11y — Radix links them via aria-labelledby/describedby.
export const DialogTitle = ({ className, ...p }: React.ComponentProps<typeof DialogPrimitive.Title>) => (
  <DialogPrimitive.Title className={cn("text-lg font-semibold", className)} {...p} />
);
export const DialogDescription = ({ className, ...p }: React.ComponentProps<typeof DialogPrimitive.Description>) => (
  <DialogPrimitive.Description className={cn("mt-1 text-sm text-muted-fg", className)} {...p} />
);
export const DialogFooter = ({ className, ...p }: React.ComponentProps<"div">) => (
  <div className={cn("mt-6 flex justify-end gap-2", className)} {...p} />
);
```

Usage:

```tsx
<Dialog>
  <DialogTrigger asChild><Button variant="destructive">Delete project</Button></DialogTrigger>
  <DialogContent>
    <DialogTitle>Delete project?</DialogTitle>
    <DialogDescription>This permanently removes the project and all its data. This cannot be undone.</DialogDescription>
    <DialogFooter>
      <DialogClose asChild><Button variant="secondary">Cancel</Button></DialogClose>
      <Button variant="destructive">Delete</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

**Notes:** Always render a `DialogTitle` (use Radix `VisuallyHidden` if visually hidden) or screen readers announce nothing. The `animate-in/out` utilities come from `tailwindcss-animate` (shadcn installs it). `motion-reduce:animate-none` honours [../04-interaction/motion.md](../04-interaction/motion.md). For non-modal panels prefer a Sheet (same primitive, slides from edge). Never put a Dialog inside a Dialog — flatten the flow.

---

## 2. Toast notifications

**Use when:** confirming a background action ("Saved", "Copied"), or a recoverable error with an undo. Use **sonner** — it handles stacking, swipe-to-dismiss, timers, and the ARIA live region.

```tsx
// app root (e.g. App.tsx) — mount once
import { Toaster } from "sonner";

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <>
      {children}
      <Toaster
        position="bottom-right"
        toastOptions={{
          classNames: {
            toast: "rounded-[--radius] border border-border bg-bg text-fg shadow-card",
            description: "text-muted-fg",
            actionButton: "bg-primary text-primary-fg",
          },
        }}
      />
    </>
  );
}
```

```tsx
// anywhere — fire toasts imperatively
import { toast } from "sonner";

toast.success("Project saved");
toast.error("Couldn't save", { description: "Check your connection and retry." });

// Optimistic delete with undo:
toast("Item deleted", {
  action: { label: "Undo", onClick: () => restoreItem(id) },
  duration: 6000, // give time to undo
});

// Promise lifecycle (loading → success/error) in one call:
toast.promise(saveSettings(), {
  loading: "Saving…",
  success: "Settings saved",
  error: (e) => `Failed: ${e.message}`,
});
```

**Notes:** Toasts are for *transient, non-critical* feedback. Critical errors that block a flow belong inline (see recipe 4) or in a Dialog, never only in a toast — they auto-dismiss and screen-reader users may miss them. Keep messages short; put detail in `description`. Default 4s is fine; extend to ~6s when an `action` (undo) is offered. Don't stack more than a few; sonner collapses extras automatically.

---

## 3. Responsive Card

**Use when:** grouping related content (a setting, a summary, a list item-as-card). Compound component so consumers compose header/content/footer freely.

```tsx
// components/ui/card.tsx
import * as React from "react";
import { cn } from "@/lib/utils";

export function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      className={cn(
        "rounded-[--radius] border border-border bg-bg shadow-card",
        "transition-shadow hover:shadow-md",
        className,
      )}
      {...props}
    />
  );
}
export const CardHeader = ({ className, ...p }: React.ComponentProps<"div">) => (
  <div className={cn("flex flex-col gap-1.5 p-6", className)} {...p} />
);
export const CardTitle = ({ className, ...p }: React.ComponentProps<"h3">) => (
  <h3 className={cn("text-base font-semibold leading-none tracking-tight", className)} {...p} />
);
export const CardDescription = ({ className, ...p }: React.ComponentProps<"p">) => (
  <p className={cn("text-sm text-muted-fg", className)} {...p} />
);
export const CardContent = ({ className, ...p }: React.ComponentProps<"div">) => (
  <div className={cn("p-6 pt-0", className)} {...p} />
);
export const CardFooter = ({ className, ...p }: React.ComponentProps<"div">) => (
  <div className={cn("flex items-center gap-2 p-6 pt-0", className)} {...p} />
);
```

```tsx
// Responsive grid of cards — auto-fit, no media queries needed.
<div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
  <Card>
    <CardHeader>
      <CardTitle>Usage</CardTitle>
      <CardDescription>This billing period</CardDescription>
    </CardHeader>
    <CardContent>…</CardContent>
  </Card>
</div>
```

**Notes:** Don't make the whole card a `<button>`/`<a>` if it contains other interactive elements (nested-interactive a11y violation). Instead, stretch a single primary link with a pseudo-element: put `<a className="after:absolute after:inset-0">` on the title and `relative` on the card, so the whole card is clickable but inner buttons still work. For an `auto-fit` grid without breakpoints: `grid-cols-[repeat(auto-fit,minmax(16rem,1fr))]`.

---

## 4. Form field (label + error + ARIA wiring)

**Use when:** any text input. Wires `<label htmlFor>`, `aria-invalid`, `aria-describedby` → error, and `role="alert"` so errors are announced. Pairs with React Hook Form + Zod ([../03-components/forms.md](../03-components/forms.md)).

```tsx
// components/ui/field.tsx
import * as React from "react";
import { cn } from "@/lib/utils";

interface FieldProps extends React.ComponentProps<"input"> {
  label: string;
  error?: string;
  hint?: string;
}

export const Field = React.forwardRef<HTMLInputElement, FieldProps>(
  ({ id, label, error, hint, className, required, ...props }, ref) => {
    const autoId = React.useId();
    const fieldId = id ?? autoId;
    const errorId = `${fieldId}-error`;
    const hintId = `${fieldId}-hint`;

    return (
      <div className="flex flex-col gap-1.5">
        <label htmlFor={fieldId} className="text-sm font-medium">
          {label}
          {required && <span className="ml-0.5 text-destructive" aria-hidden="true">*</span>}
        </label>

        <input
          ref={ref}
          id={fieldId}
          required={required}
          aria-invalid={error ? true : undefined}
          // Link BOTH hint and error so SR reads them; omit when absent.
          aria-describedby={cn(hint && hintId, error && errorId) || undefined}
          className={cn(
            "h-10 rounded-[--radius] border bg-bg px-3 text-sm outline-none transition-colors",
            "placeholder:text-muted-fg",
            "focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-1 focus-visible:ring-offset-bg",
            "disabled:cursor-not-allowed disabled:opacity-50",
            error ? "border-destructive focus-visible:ring-destructive" : "border-border",
            className,
          )}
          {...props}
        />

        {hint && !error && (
          <p id={hintId} className="text-xs text-muted-fg">{hint}</p>
        )}
        {error && (
          // role="alert" → announced immediately; icon + text, never color alone.
          <p id={errorId} role="alert" className="flex items-center gap-1 text-xs text-destructive">
            <svg viewBox="0 0 16 16" className="size-3.5 shrink-0" fill="currentColor" aria-hidden="true">
              <path d="M8 1a7 7 0 1 0 0 14A7 7 0 0 0 8 1Zm0 3a1 1 0 0 1 1 1v3a1 1 0 1 1-2 0V5a1 1 0 0 1 1-1Zm0 7a1 1 0 1 1 0-2 1 1 0 0 1 0 2Z" />
            </svg>
            {error}
          </p>
        )}
      </div>
    );
  },
);
Field.displayName = "Field";
```

```tsx
// With React Hook Form + Zod
const schema = z.object({ email: z.string().email("Enter a valid email") });
const { register, handleSubmit, formState: { errors } } = useForm({ resolver: zodResolver(schema) });

<form onSubmit={handleSubmit(onSubmit)}>
  <Field label="Email" type="email" required hint="We'll never share it." error={errors.email?.message} {...register("email")} />
</form>
```

**Notes:** Error message states *what to do*, not "Invalid" ("Enter a valid email" not "Error"). Never rely on red border alone — the icon + text covers colorblind users. Validate on blur / submit, not every keystroke (see [../03-components/forms.md](../03-components/forms.md)). `useId()` guarantees unique, SSR-safe ids.

---

## 5. Skeleton loader

**Use when:** content takes >~300ms to load and you know its shape. A skeleton that matches the final layout prevents layout shift (CLS) and feels faster than a spinner.

```tsx
// components/ui/skeleton.tsx
import { cn } from "@/lib/utils";

export function Skeleton({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      // aria-hidden: it's decorative; announce loading on the region instead.
      aria-hidden="true"
      className={cn("animate-pulse rounded-md bg-muted motion-reduce:animate-none", className)}
      {...props}
    />
  );
}

// Compose to mirror the real component's geometry:
export function CardSkeleton() {
  return (
    <div className="rounded-[--radius] border border-border p-6" aria-busy="true" aria-live="polite">
      <span className="sr-only">Loading…</span>
      <Skeleton className="h-4 w-1/3" />
      <Skeleton className="mt-3 h-3 w-full" />
      <Skeleton className="mt-2 h-3 w-5/6" />
      <Skeleton className="mt-6 h-9 w-24" />
    </div>
  );
}
```

**Notes:** Match the skeleton to the real layout's *dimensions* so swapping in content causes zero shift. Put `aria-busy="true"` + an `sr-only` "Loading" on the container (the skeleton bars themselves are `aria-hidden`). `motion-reduce:animate-none` kills the pulse for users who opted out. Don't skeleton tiny/instant content — a flash is worse than nothing. For unknown shapes or <300ms, a spinner is fine.

---

## 6. Empty state

**Use when:** a list/table/search has no items — first-run, no results, or cleared. A good empty state explains *why it's empty* and gives the *next action*.

```tsx
// components/ui/empty-state.tsx
import * as React from "react";
import { cn } from "@/lib/utils";

interface EmptyStateProps {
  icon?: React.ReactNode;
  title: string;
  description?: string;
  action?: React.ReactNode;
  className?: string;
}

export function EmptyState({ icon, title, description, action, className }: EmptyStateProps) {
  return (
    <div className={cn("flex flex-col items-center justify-center px-6 py-16 text-center", className)}>
      {icon && (
        <div className="mb-4 grid size-12 place-items-center rounded-full bg-muted text-muted-fg [&_svg]:size-6">
          {icon}
        </div>
      )}
      <h3 className="text-base font-semibold">{title}</h3>
      {description && <p className="mt-1 max-w-sm text-sm text-muted-fg">{description}</p>}
      {action && <div className="mt-6">{action}</div>}
    </div>
  );
}
```

```tsx
import { Inbox, Search } from "lucide-react";

// First-run (no data yet) — drive the primary action:
<EmptyState icon={<Inbox />} title="No projects yet"
  description="Create your first project to get started."
  action={<Button>New project</Button>} />

// No search results — offer a way out, not a dead end:
<EmptyState icon={<Search />} title="No results for “acme”"
  description="Try a different term or clear filters."
  action={<Button variant="secondary" onClick={clearFilters}>Clear filters</Button>} />
```

**Notes:** Distinguish the three cases — *first-run* (teach + primary CTA), *no results* (suggest a fix / clear filters), *error* (retry button, not an empty state). Never show a blank region or a bare "No data." The icon is decorative (`aria-hidden` via the svg). Keep copy human and specific to the context.

---

## 7. Command palette (cmdk)

**Use when:** power-user navigation/actions via `⌘K`. Use **cmdk** — it gives filtering, keyboard nav, grouping, and ARIA listbox semantics. Combine with a Dialog for the overlay.

```tsx
// components/command-menu.tsx
import * as React from "react";
import { Command } from "cmdk";
import { Dialog, DialogContent, DialogTitle } from "@/components/ui/dialog";
import { VisuallyHidden } from "@radix-ui/react-visually-hidden";
import { useNavigate } from "react-router-dom";
import { Home, Settings, FileText } from "lucide-react";

export function CommandMenu() {
  const [open, setOpen] = React.useState(false);
  const navigate = useNavigate();

  // ⌘K / Ctrl+K toggles.
  React.useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "k" && (e.metaKey || e.ctrlKey)) {
        e.preventDefault();
        setOpen((o) => !o);
      }
    };
    document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, []);

  const go = (path: string) => {
    setOpen(false);
    navigate(path);
  };

  return (
    <Dialog open={open} onOpenChange={setOpen}>
      <DialogContent className="overflow-hidden p-0">
        <VisuallyHidden><DialogTitle>Command menu</DialogTitle></VisuallyHidden>
        <Command className="[&_[cmdk-input]]:h-12 [&_[cmdk-input]]:w-full [&_[cmdk-input]]:bg-transparent [&_[cmdk-input]]:px-4 [&_[cmdk-input]]:text-sm [&_[cmdk-input]]:outline-none">
          <Command.Input placeholder="Type a command or search…" className="border-b border-border" />
          <Command.List className="max-h-80 overflow-y-auto p-2">
            <Command.Empty className="py-6 text-center text-sm text-muted-fg">No results found.</Command.Empty>
            <Command.Group heading="Navigation" className="[&_[cmdk-group-heading]]:px-2 [&_[cmdk-group-heading]]:py-1.5 [&_[cmdk-group-heading]]:text-xs [&_[cmdk-group-heading]]:text-muted-fg">
              <Item onSelect={() => go("/")} icon={<Home />}>Home</Item>
              <Item onSelect={() => go("/docs")} icon={<FileText />}>Docs</Item>
              <Item onSelect={() => go("/settings")} icon={<Settings />}>Settings</Item>
            </Command.Group>
          </Command.List>
        </Command>
      </DialogContent>
    </Dialog>
  );
}

function Item({ icon, children, onSelect }: { icon: React.ReactNode; children: React.ReactNode; onSelect: () => void }) {
  return (
    <Command.Item
      onSelect={onSelect}
      className="flex cursor-pointer items-center gap-2 rounded-md px-2 py-2 text-sm aria-selected:bg-muted [&_svg]:size-4 [&_svg]:text-muted-fg"
    >
      {icon}
      {children}
    </Command.Item>
  );
}
```

**Notes:** cmdk manages the active-item `aria-selected` and arrow/Enter keys; you just style `aria-selected:bg-muted`. Wrap in a Dialog so focus trap + scroll lock + `Esc` are handled. Always render a `DialogTitle` (visually hidden here). Surface the `⌘K` hint somewhere discoverable. Use `Command.Group` to cluster actions; debounce expensive async filtering yourself.

---

## 8. Data table with sorting (TanStack Table)

**Use when:** tabular data needing sort/filter/pagination. Use **TanStack Table** (headless — it owns logic, you own markup). Add **TanStack Virtual** past a few hundred rows. See [../03-components/data-display.md](../03-components/data-display.md).

```tsx
// components/data-table.tsx
import * as React from "react";
import {
  flexRender, getCoreRowModel, getSortedRowModel, useReactTable,
  type ColumnDef, type SortingState,
} from "@tanstack/react-table";
import { ChevronsUpDown, ChevronUp, ChevronDown } from "lucide-react";
import { cn } from "@/lib/utils";

export function DataTable<T>({ columns, data }: { columns: ColumnDef<T, any>[]; data: T[] }) {
  const [sorting, setSorting] = React.useState<SortingState>([]);
  const table = useReactTable({
    data, columns,
    state: { sorting },
    onSortingChange: setSorting,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
  });

  return (
    <div className="overflow-x-auto rounded-[--radius] border border-border">
      <table className="w-full text-sm">
        <thead className="border-b border-border bg-muted/50">
          {table.getHeaderGroups().map((hg) => (
            <tr key={hg.id}>
              {hg.headers.map((header) => {
                const sortable = header.column.getCanSort();
                const dir = header.column.getIsSorted(); // 'asc' | 'desc' | false
                return (
                  <th key={header.id} className="px-4 py-2.5 text-left font-medium"
                      aria-sort={dir === "asc" ? "ascending" : dir === "desc" ? "descending" : undefined}>
                    {sortable ? (
                      <button
                        onClick={header.column.getToggleSortingHandler()}
                        className="inline-flex items-center gap-1 rounded hover:text-fg focus-visible:ring-2 focus-visible:ring-ring focus-visible:outline-none"
                      >
                        {flexRender(header.column.columnDef.header, header.getContext())}
                        {dir === "asc" ? <ChevronUp className="size-3.5" />
                          : dir === "desc" ? <ChevronDown className="size-3.5" />
                          : <ChevronsUpDown className="size-3.5 text-muted-fg" />}
                      </button>
                    ) : flexRender(header.column.columnDef.header, header.getContext())}
                  </th>
                );
              })}
            </tr>
          ))}
        </thead>
        <tbody>
          {table.getRowModel().rows.map((row) => (
            <tr key={row.id} className="border-b border-border last:border-0 hover:bg-muted/40">
              {row.getVisibleCells().map((cell) => (
                <td key={cell.id} className="px-4 py-2.5 tabular-nums">
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

```tsx
const columns: ColumnDef<User, any>[] = [
  { accessorKey: "name", header: "Name" },
  { accessorKey: "email", header: "Email" },
  { accessorKey: "createdAt", header: "Joined", cell: (c) => formatDate(c.getValue()) },
];
<DataTable columns={columns} data={users} />
```

**Notes:** `aria-sort` on the `<th>` is what makes sorting accessible — screen readers announce the column's sort state. Header sort control is a real `<button>` so it's keyboard-operable. Use `tabular-nums` on numeric columns to keep digits aligned. For large datasets wrap `<tbody>` rows in TanStack Virtual and keep a fixed row height. Right-align numbers, left-align text (see [../03-components/data-display.md](../03-components/data-display.md)).

---

## 9. Fade-in-up on scroll (reduced-motion guarded)

**Use when:** revealing sections as they enter the viewport on a marketing page. Triggers once, respects `prefers-reduced-motion` (renders instantly, no movement, for users who opted out). Uses Motion's `useReducedMotion`.

```tsx
// components/reveal.tsx
import * as React from "react";
import { motion, useReducedMotion } from "motion/react";

export function Reveal({
  children,
  delay = 0,
  className,
}: { children: React.ReactNode; delay?: number; className?: string }) {
  const reduce = useReducedMotion();

  // Reduced motion → render plainly, no transform, no fade movement.
  if (reduce) return <div className={className}>{children}</div>;

  return (
    <motion.div
      className={className}
      initial={{ opacity: 0, y: 16 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, margin: "0px 0px -10% 0px" }} // fire once, slightly before fully in view
      transition={{ duration: 0.4, delay, ease: [0.22, 1, 0.36, 1] }} // ease-out
    >
      {children}
    </motion.div>
  );
}
```

```tsx
<Reveal>          <h2>Built for speed</h2></Reveal>
<Reveal delay={0.1}><p>Ship faster with…</p></Reveal>
```

Pure-CSS alternative (no JS, native scroll-driven animation — degrades gracefully where unsupported):

```css
@media (prefers-reduced-motion: no-preference) {
  .reveal {
    animation: reveal linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 40%;
  }
  @keyframes reveal { from { opacity: 0; translate: 0 16px; } to { opacity: 1; translate: 0 0; } }
}
```

**Notes:** `viewport={{ once: true }}` means it animates a single time — re-animating on every scroll is nauseating. Only `opacity` + `y` (transform) are animated, both GPU-cheap (see [../04-interaction/motion.md](../04-interaction/motion.md)). Never gate content visibility on the animation — if JS fails, content must still be there. Don't scroll-jack. Keep `y` distance small (12–24px); large slides feel sluggish.

---

## 10. Theme toggle (next-themes)

**Use when:** light/dark/system switching with no flash of the wrong theme. next-themes persists the choice and sets `.dark` on `<html>` before paint.

```tsx
// app root
import { ThemeProvider } from "next-themes";

// Vite/React: wrap your app
<ThemeProvider attribute="class" defaultTheme="system" enableSystem disableTransitionOnChange>
  <App />
</ThemeProvider>
```

```tsx
// components/theme-toggle.tsx
import * as React from "react";
import { useTheme } from "next-themes";
import { Moon, Sun } from "lucide-react";
import { Button } from "@/components/ui/button";

export function ThemeToggle() {
  const { setTheme, resolvedTheme } = useTheme();
  const [mounted, setMounted] = React.useState(false);

  // Avoid hydration mismatch: don't render theme-dependent UI until mounted.
  React.useEffect(() => setMounted(true), []);
  if (!mounted) return <Button variant="ghost" size="icon" aria-label="Toggle theme" />;

  const isDark = resolvedTheme === "dark";
  return (
    <Button
      variant="ghost"
      size="icon"
      aria-label={`Switch to ${isDark ? "light" : "dark"} mode`}
      onClick={() => setTheme(isDark ? "light" : "dark")}
    >
      {isDark ? <Sun className="size-4" /> : <Moon className="size-4" />}
    </Button>
  );
}
```

For Next.js App Router, add `suppressHydrationWarning` to `<html>`. The `@custom-variant dark` in your CSS ([./tech-stack.md](./tech-stack.md)) is what makes `dark:` utilities respond to the class.

**Notes:** `disableTransitionOnChange` prevents every transitioning element from animating during the swap (jarring). The mounted-guard avoids a hydration mismatch since the server can't know the user's stored preference. `aria-label` updates to describe the *action*. Provide a 3-way (light/dark/system) menu for power users if desired — keep system as default.

---

## 11. Sticky responsive navbar with mobile sheet

**Use when:** top-level site/app navigation that collapses to a hamburger + slide-in sheet on mobile. Sheet uses Radix Dialog (focus trap, `Esc`, scroll lock free).

```tsx
// components/navbar.tsx
import * as React from "react";
import * as Dialog from "@radix-ui/react-dialog";
import { Menu, X } from "lucide-react";
import { Link } from "react-router-dom";
import { Button } from "@/components/ui/button";
import { ThemeToggle } from "@/components/theme-toggle";
import { cn } from "@/lib/utils";

const links = [
  { href: "/features", label: "Features" },
  { href: "/pricing", label: "Pricing" },
  { href: "/docs", label: "Docs" },
];

export function Navbar() {
  const [open, setOpen] = React.useState(false);
  return (
    <header className="sticky top-0 z-40 w-full border-b border-border bg-bg/80 backdrop-blur supports-[backdrop-filter]:bg-bg/60">
      <nav className="mx-auto flex h-14 max-w-6xl items-center justify-between px-4" aria-label="Main">
        <Link to="/" className="font-semibold">Acme</Link>

        {/* Desktop links */}
        <ul className="hidden items-center gap-1 md:flex">
          {links.map((l) => (
            <li key={l.href}>
              <Link to={l.href} className="rounded-md px-3 py-2 text-sm text-muted-fg transition-colors hover:text-fg hover:bg-muted">
                {l.label}
              </Link>
            </li>
          ))}
        </ul>

        <div className="flex items-center gap-2">
          <ThemeToggle />
          <Button className="hidden md:inline-flex" size="sm">Sign in</Button>

          {/* Mobile menu */}
          <Dialog.Root open={open} onOpenChange={setOpen}>
            <Dialog.Trigger asChild>
              <Button variant="ghost" size="icon" className="md:hidden" aria-label="Open menu"><Menu /></Button>
            </Dialog.Trigger>
            <Dialog.Portal>
              <Dialog.Overlay className="fixed inset-0 z-50 bg-black/50 data-[state=open]:animate-in data-[state=open]:fade-in-0" />
              <Dialog.Content
                className={cn(
                  "fixed inset-y-0 right-0 z-50 w-3/4 max-w-xs border-l border-border bg-bg p-6 shadow-card",
                  "data-[state=open]:animate-in data-[state=open]:slide-in-from-right",
                  "data-[state=closed]:animate-out data-[state=closed]:slide-out-to-right",
                  "motion-reduce:animate-none",
                )}
              >
                <Dialog.Title className="sr-only">Navigation</Dialog.Title>
                <div className="mb-6 flex justify-end">
                  <Dialog.Close asChild><Button variant="ghost" size="icon" aria-label="Close menu"><X /></Button></Dialog.Close>
                </div>
                <ul className="flex flex-col gap-1">
                  {links.map((l) => (
                    <li key={l.href}>
                      <Link to={l.href} onClick={() => setOpen(false)} className="block rounded-md px-3 py-2.5 text-sm hover:bg-muted">
                        {l.label}
                      </Link>
                    </li>
                  ))}
                </ul>
                <Button className="mt-4 w-full">Sign in</Button>
              </Dialog.Content>
            </Dialog.Portal>
          </Dialog.Root>
        </div>
      </nav>
    </header>
  );
}
```

**Notes:** `sticky top-0` + `backdrop-blur` with a translucent bg is the modern frosted nav; the `supports-[backdrop-filter]` guard avoids a fully transparent bar where blur is unsupported. The mobile sheet is a Dialog, so it traps focus and closes on `Esc`/outside — close it on link click too (`onClick={() => setOpen(false)}`). Mark active link with `aria-current="page"`. Hamburger and close buttons need `aria-label`s. See [../03-components/navigation.md](../03-components/navigation.md).

---

## 12. Pricing card

**Use when:** presenting plan tiers. Highlights the recommended plan, lists features clearly, single CTA per card. Accessible feature list with check icons.

```tsx
// components/pricing-card.tsx
import { Check } from "lucide-react";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

interface Plan {
  name: string;
  price: string;
  period?: string;
  description: string;
  features: string[];
  cta: string;
  featured?: boolean;
}

export function PricingCard({ plan }: { plan: Plan }) {
  return (
    <Card className={cn("relative flex flex-col", plan.featured && "border-primary ring-1 ring-primary shadow-md")}>
      {plan.featured && (
        <span className="absolute -top-3 left-1/2 -translate-x-1/2 rounded-full bg-primary px-3 py-0.5 text-xs font-medium text-primary-fg">
          Most popular
        </span>
      )}
      <CardHeader>
        <h3 className="text-lg font-semibold">{plan.name}</h3>
        <p className="text-sm text-muted-fg">{plan.description}</p>
        <p className="mt-2 flex items-baseline gap-1">
          <span className="text-3xl font-bold tabular-nums">{plan.price}</span>
          {plan.period && <span className="text-sm text-muted-fg">/{plan.period}</span>}
        </p>
      </CardHeader>
      <CardContent className="flex-1">
        <ul className="space-y-2.5 text-sm">
          {plan.features.map((f) => (
            <li key={f} className="flex items-start gap-2">
              <Check className="mt-0.5 size-4 shrink-0 text-primary" aria-hidden="true" />
              <span>{f}</span>
            </li>
          ))}
        </ul>
      </CardContent>
      <CardFooter>
        <Button className="w-full" variant={plan.featured ? "primary" : "secondary"}>{plan.cta}</Button>
      </CardFooter>
    </Card>
  );
}
```

```tsx
<div className="grid gap-6 md:grid-cols-3">
  <PricingCard plan={{ name: "Starter", price: "$0", period: "mo", description: "For trying things out.",
    features: ["Up to 3 projects", "Community support"], cta: "Start free" }} />
  <PricingCard plan={{ name: "Pro", price: "$19", period: "mo", description: "For growing teams.", featured: true,
    features: ["Unlimited projects", "Priority support", "Advanced analytics"], cta: "Start trial" }} />
  <PricingCard plan={{ name: "Enterprise", price: "Custom", description: "For large orgs.",
    features: ["SSO & SAML", "Dedicated support", "SLA"], cta: "Contact sales" }} />
</div>
```

**Notes:** One clear CTA per card; the featured plan gets `variant="primary"`, others `secondary`, so visual hierarchy points to the recommended tier. The check icon is decorative (`aria-hidden`) — the feature text carries meaning. `tabular-nums` keeps prices aligned across cards. `flex flex-col` + `flex-1` on content pins all footers/CTAs to the same baseline even with uneven feature lists. Don't bury the price; lead with it.

---

## Agent checklist
- [ ] Build dialogs, sheets, and command palettes on Radix Dialog / cmdk — never hand-roll focus trap, scroll lock, `Esc`, or click-outside.
- [ ] Every form field wires `htmlFor`, `aria-invalid`, `aria-describedby`→error, and `role="alert"`; error copy says what to fix, never color-only.
- [ ] Toasts only for transient, non-critical feedback (sonner); critical/blocking errors go inline or in a dialog.
- [ ] Skeletons mirror final layout dimensions (no CLS), carry `aria-busy` + `sr-only` "Loading", and stop pulsing under `motion-reduce`.
- [ ] Empty states distinguish first-run vs. no-results vs. error and always offer the next action.
- [ ] Data tables expose `aria-sort` on sortable headers and make the sort control a real keyboard-operable `<button>`.
- [ ] Scroll reveals fire once, animate only opacity+transform, and short-circuit to instant render under `prefers-reduced-motion`.
- [ ] Theme toggle guards against hydration mismatch (mounted check), uses `disableTransitionOnChange`, and labels the action.
- [ ] Mobile nav sheet closes on link click + `Esc`; hamburger/close/icon-only buttons have `aria-label`; active link gets `aria-current`.
- [ ] Cards/pricing avoid nested-interactive violations, keep one primary CTA, and use `tabular-nums` for aligned numbers.
- [ ] Every recipe routes `className` through `cn()` and pulls colors/radius from `@theme` tokens so it re-themes in dark mode automatically.
