# Design Tokens

Quick reference for the Lifetime design language. Use **only** these tokens for color, typography, radius, and shadows — no arbitrary values. Standard Tailwind utilities for everything else (layout, spacing, sizing).

---

## Surfaces

| Token | Class | Use for |
|-------|-------|---------|
| p0 | `bg-p0` | Page background |
| p1 | `bg-p1` | Cards, panels, inputs |

## Grey (text & UI)

| Token | Class examples | Use for |
|-------|---------------|---------|
| g3 | `bg-g3` | Hover backgrounds (light) |
| g4 | `border-g4`, `inset-ring-g4` | Borders, dividers, subtle rings |
| g5 | `border-g5` | Stronger borders |
| g9 | `text-g9` | Muted/secondary text, placeholder |
| g10 | `text-g10` | Medium-emphasis text |
| g11 | `text-g11` | Body text (default) |
| g12 | `text-g12` | Headings, high-emphasis text |

Full scale: g0–g12 (lightest to darkest).

## Accent (blue)

| Token | Class examples | Use for |
|-------|---------------|---------|
| a3 | `bg-a3` | Light accent background |
| a9 | `bg-a9` | Primary buttons, solid accent |
| a10 | `hover:bg-a10` | Primary button hover |
| a11 | `text-a11` | Links, accent text |

Full scale: a1–a12.

## Success (green)

| Token | Class examples | Use for |
|-------|---------------|---------|
| s3 | `bg-s3` | Success background |
| s11 | `text-s11` | Success text |

Full scale: s1–s12.

## Danger (red)

| Token | Class examples | Use for |
|-------|---------------|---------|
| d3 | `bg-d3` | Danger background |
| d9 | `bg-d9` | Danger buttons (solid) |
| d10 | `hover:bg-d10` | Danger button hover |
| d11 | `text-d11` | Danger/error text |

Full scale: d1–d12.

## Warning (yellow)

| Token | Class examples | Use for |
|-------|---------------|---------|
| y3 | `bg-y3` | Warning background |
| y11 | `text-y11` | Warning text |

Full scale: y1–y12.

## Alpha (transparency)

| Token | Class | Use for |
|-------|-------|---------|
| alpha-6 | `bg-alpha-6` | Subtle overlays |
| alpha-48 | `bg-alpha-48` | Modal overlays, dimming |

Full scale: alpha-2, 4, 6, 8, 10, 12, 16, 24, 36, 48, 80.

---

## Typography

| Class | Size | Weight | Use for |
|-------|------|--------|---------|
| `text-micro` | 10px | 450 | Labels, fine print |
| `text-micro-medium` | 10px | 500 | Badge labels |
| `text-mini` | 12px | 450 | Secondary info, captions |
| `text-mini-medium` | 12px | 500 | Emphasized small text |
| `text-sm` | 13px | 450 | Body text (compact), table cells |
| `text-sm-medium` | 13px | 500 | Emphasized compact text |
| `text-regular` | 14px | 450 | Body text (default) |
| `text-regular-medium` | 14px | 500 | Headings, labels |

Pair with color: e.g. `text-sm text-g11`, `text-regular-medium text-g12`.

---

## Radius

| Class | Value | Use for |
|-------|-------|---------|
| `rounded-4` | 4px | Small elements |
| `rounded-6` | 6px | Buttons, inputs |
| `rounded-8` | 8px | Cards, panels |
| `rounded-12` | 12px | Large containers |
| `rounded-full` | 999px | Pills, badges, avatars |

Also available: `rounded-0`, `rounded-2`, `rounded-10`.

## Shadows

| Class | Use for |
|-------|---------|
| `shadow-xs` | Cards, buttons, inputs — subtle elevation |
| `shadow-md` | Dropdowns, popovers — stronger elevation |

## Animation

| Class | Value | Use for |
|-------|-------|---------|
| `duration-fast` | 120ms | Micro-interactions (hover, focus) |
| `duration-normal` | 200ms | Standard transitions |
| `duration-slow` | 300ms | Entrances, exits |
| `ease-out` | cubic-bezier(0.16, 1, 0.3, 1) | Natural deceleration |
| `ease-in-out` | cubic-bezier(0.65, 0, 0.35, 1) | Smooth both ways |

---

## Common patterns

### Buttons

```html
<!-- Primary -->
<button class="rounded-6 bg-a9 px-3 py-1.5 text-sm text-white shadow-xs hover:bg-a10 transition-colors">Label</button>

<!-- Secondary -->
<button class="rounded-6 bg-p1 px-3 py-1.5 text-sm text-g11 shadow-xs hover:bg-g3 transition-colors">Label</button>

<!-- Ghost -->
<button class="rounded-6 px-3 py-1.5 text-sm text-g11 hover:bg-g3 transition-colors">Label</button>

<!-- Danger -->
<button class="rounded-6 bg-d9 px-3 py-1.5 text-sm text-white shadow-xs hover:bg-d10 transition-colors">Delete</button>
```

### Badges

```html
<span class="inline-flex rounded-full bg-s3 px-2 py-0.5 text-micro-medium text-s11">Active</span>
<span class="inline-flex rounded-full bg-d3 px-2 py-0.5 text-micro-medium text-d11">Overdue</span>
<span class="inline-flex rounded-full bg-a3 px-2 py-0.5 text-micro-medium text-a11">Pending</span>
<span class="inline-flex rounded-full bg-y3 px-2 py-0.5 text-micro-medium text-y11">Warning</span>
```

### Card

```html
<div class="rounded-8 bg-p1 p-4 shadow-xs">Content</div>
```

### Input

```html
<input type="text" class="w-full rounded-6 bg-p1 px-3 py-1.5 text-sm text-g12 shadow-xs outline-none placeholder:text-g9 focus:ring-2 focus:ring-a9" placeholder="Search...">
```

### Table row

```html
<div class="flex items-center gap-3 border-b border-g4 px-4 py-3 text-sm text-g11">
  <span class="flex-1">Client name</span>
  <span class="text-g9">€42,500</span>
</div>
```

### Section heading

```html
<h2 class="text-regular-medium text-g12">Section title</h2>
<p class="text-sm text-g9 mt-1">Supporting description</p>
```

### Divider

```html
<div class="border-t border-g4"></div>
```

---

## Rule of thumb

Use tokens for **color, type, radius, shadows**. Use standard Tailwind for **layout, spacing, sizing** (`flex`, `grid`, `gap-*`, `p-*`, `w-*`, `max-w-*`). Deviate from the patterns freely when the UI calls for it — these are guides, not constraints.
