# Tailwind CSS v4 LLM Development Guidelines

You are an expert web developer specializing in Tailwind CSS v4. Follow these guidelines when writing code or providing recommendations.

## Core Principles

Tailwind CSS v4 is a complete rewrite with a new high-performance Oxide engine, CSS-first configuration, and modern web platform features. It requires modern browsers (Safari 16.4+, Chrome 111+, Firefox 128+) and is NOT compatible with older browsers.

## Installation & Setup

### Basic Setup
```css
/* styles.css */
@import "tailwindcss";
```

### With Vite
```js
// vite.config.js
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [tailwindcss()]
})
```

### Package Installation
```bash
npm install tailwindcss@next @tailwindcss/vite@next
```

## Configuration (CSS-First)

**IMPORTANT**: v4 uses CSS-first configuration, NOT JavaScript config files. Use the `@theme` directive in your CSS file:

```css
@import "tailwindcss";

@theme {
  /* Colors (use OKLCH for wider gamut) */
  --color-brand-50: oklch(0.98 0.01 142.12);
  --color-brand-500: oklch(0.64 0.15 142.12);
  --color-brand-900: oklch(0.25 0.08 142.12);
  
  /* Typography */
  --font-display: "Satoshi", "Inter", sans-serif;
  --font-body: "Inter", system-ui, sans-serif;
  
  /* Spacing */
  --spacing-18: 4.5rem;
  --spacing-72: 18rem;
  
  /* Breakpoints */
  --breakpoint-3xl: 1920px;
  --breakpoint-4xl: 2560px;
  
  /* Shadows */
  --shadow-brutal: 8px 8px 0px 0px #000;
  
  /* Animation */
  --animate-wiggle: wiggle 1s ease-in-out infinite;
}

/* Define keyframes for animations */
@keyframes wiggle {
  0%, 100% { transform: rotate(-3deg); }
  50% { transform: rotate(3deg); }
}
```

### Legacy Config Support (if needed)
```css
@import "tailwindcss";
@config "./tailwind.config.js";
```

## Breaking Changes from v3

### Import Changes
- ❌ `@tailwind base; @tailwind components; @tailwind utilities;`
- ✅ `@import "tailwindcss";`

### Removed Utilities
- `text-opacity-*` → Use `text-{color}/{opacity}` instead
- `bg-opacity-*` → Use `bg-{color}/{opacity}` instead
- `border-opacity-*` → Use `border-{color}/{opacity}` instead
- `flex-grow-*` → Use `grow-*`
- `flex-shrink-*` → Use `shrink-*`
- `decoration-slice` → Use `box-decoration-slice`

### Renamed Utilities
- `shadow-sm` → `shadow-xs`
- `rounded-sm` → `rounded-xs`
- `blur-sm` → `blur-xs`
- `bg-gradient-*` → `bg-linear-*`

### Default Behavior Changes
- **Border**: No longer defaults to gray-200, uses `currentColor`
- **Ring**: Changed from 3px blue to 1px `currentColor`
- **Outline**: Now 1px by default for consistency

## Custom Utilities

Use the `@utility` directive instead of `@layer utilities`:

```css
@utility btn {
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
  font-weight: 500;
  transition: all 0.2s;
}

@utility btn-primary {
  background: var(--color-blue-600);
  color: white;
  
  &:hover {
    background: var(--color-blue-700);
  }
}

/* Functional utilities with parameters */
@utility margin-auto {
  margin: auto;
}

@utility flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

## Custom Variants

```css
@custom-variant theme-dark {
  &:where([data-theme="dark"] *) {
    @slot;
  }
}

@custom-variant any-hover {
  @media (any-hover: hover) {
    &:hover {
      @slot;
    }
  }
}
```

## New Features in v4

### Container Queries (Built-in)
```html
<div class="@container">
  <div class="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-3">
    <div class="@min-w-64:text-lg @max-w-96:bg-blue-100">
      Responsive to container size
    </div>
  </div>
</div>
```

### 3D Transforms
```html
<div class="perspective-1000">
  <div class="rotate-x-45 rotate-y-12 scale-z-110 translate-z-24 transform-3d">
    3D transformed element
  </div>
</div>
```

### Enhanced Gradients
```html
<!-- Linear gradients with angles -->
<div class="bg-linear-45 from-blue-500 to-purple-500"></div>

<!-- Radial gradients -->
<div class="bg-radial from-red-500 via-yellow-500 to-orange-500"></div>

<!-- Conic gradients -->
<div class="bg-conic from-pink-500 via-blue-500 to-green-500"></div>
```

### Text Shadows
```html
<h1 class="text-shadow-lg text-shadow-blue-500/50">
  Text with colored shadow
</h1>
```

### Mask Utilities
```html
<div class="mask-radial mask-cover">
  <img src="image.jpg" alt="Masked image" />
</div>
```

### New Variants

#### @starting-style (for animations)
```html
<div class="opacity-100 @starting-style:opacity-0 transition-opacity">
  Animates in on mount
</div>
```

#### not-* variant
```html
<div class="bg-blue-500 not-hover:bg-gray-500">
  Blue except when hovering
</div>
```

#### nth-* variants
```html
<div class="nth-2:bg-red-500 nth-odd:bg-blue-500">
  Nth child styling
</div>
```

#### inert variant
```html
<div class="inert:opacity-50 inert:pointer-events-none">
  Styled when inert
</div>
```

#### in-* variant (like group-* but without group class)
```html
<article>
  <h2 class="in-article:text-lg">Styled when inside article</h2>
</article>
```

### Modern Color System
v4 uses OKLCH color space for wider gamut support:

```css
@theme {
  --color-brand-primary: oklch(0.7 0.15 180);
  --color-brand-accent: oklch(0.8 0.2 45);
}
```

## Multi-Theme Strategy

```css
@import "tailwindcss";

@theme {
  --color-primary: oklch(0.5 0.2 240);
  --color-background: oklch(0.98 0.01 240);
}

@layer base {
  [data-theme='dark'] {
    --color-primary: oklch(0.7 0.2 240);
    --color-background: oklch(0.15 0.02 240);
  }
  
  [data-theme='high-contrast'] {
    --color-primary: oklch(0.0 0 0);
    --color-background: oklch(1.0 0 0);
  }
}
```

## Best Practices

### 1. Use CSS Variables for Dynamic Values
```html
<div class="bg-[var(--dynamic-color)] text-[var(--dynamic-size)]">
  Dynamic styling
</div>
```

### 2. Leverage the New Color System
```html
<!-- Better contrast and vibrancy with OKLCH -->
<div class="bg-blue-500 text-white">
  Vivid colors on modern displays
</div>
```

### 3. Container Queries for True Responsive Design
```html
<div class="@container">
  <div class="p-4 @lg:p-8 @xl:p-12">
    Responds to container, not viewport
  </div>
</div>
```

### 4. Modern CSS Features
```html
<!-- Native cascade layers -->
<div class="layer-[utilities]:z-10">
  Proper layer management
</div>

<!-- Color mixing -->
<div class="bg-blue-500/50">
  Uses color-mix() under the hood
</div>
```

## Performance Optimizations

v4's Oxide engine provides:
- 5x faster full builds
- 100x faster incremental builds
- Automatic content detection
- Built-in import handling
- Native vendor prefixing

## File Organization

```
src/
├── styles/
│   ├── main.css          # @import "tailwindcss" + @theme
│   ├── components.css    # @utility definitions
│   └── utilities.css     # Additional @utility definitions
└── components/
    └── *.vue/jsx/html    # Your components
```

## Common Patterns

### Theme-aware Components
```css
@utility card {
  background: var(--color-background);
  border: 1px solid var(--color-border);
  border-radius: 0.5rem;
  padding: 1.5rem;
  box-shadow: var(--shadow-sm);
}
```

### Responsive Typography
```html
<h1 class="text-2xl @sm:text-3xl @lg:text-4xl @xl:text-5xl">
  Container-responsive heading
</h1>
```

### Animation with @starting-style
```html
<div class="translate-y-0 @starting-style:translate-y-full transition-transform duration-500">
  Slides up on mount
</div>
```

## Debugging Tips

1. **Use browser dev tools** to inspect CSS variables
2. **Check cascade layers** in dev tools
3. **Verify modern browser support** for OKLCH colors
4. **Use @reference** for CSS modules/component styles
5. **Restart dev server** after major theme changes

## Migration from v3

1. Replace `@tailwind` directives with `@import "tailwindcss"`
2. Move config from JS to CSS using `@theme`
3. Update deprecated utility classes
4. Replace `@layer utilities` with `@utility`
5. Test in modern browsers only
6. Use the official upgrade tool: `npx @tailwindcss/upgrade@next`

## Complete Utility Class Reference

### Layout

#### Display
```
block, inline-block, inline, flex, inline-flex, table, inline-table, table-caption, table-cell, table-column, table-column-group, table-footer-group, table-header-group, table-row-group, table-row, flow-root, grid, inline-grid, contents, list-item, hidden
```

#### Position
```
static, fixed, absolute, relative, sticky
```

#### Top/Right/Bottom/Left
```
inset-0, inset-x-0, inset-y-0, start-0, end-0, top-0, right-0, bottom-0, left-0
inset-px, inset-x-px, inset-y-px, start-px, end-px, top-px, right-px, bottom-px, left-px
inset-0.5, inset-1, inset-1.5, inset-2, inset-2.5, inset-3, inset-3.5, inset-4, inset-5, inset-6, inset-7, inset-8, inset-9, inset-10, inset-11, inset-12, inset-14, inset-16, inset-20, inset-24, inset-28, inset-32, inset-36, inset-40, inset-44, inset-48, inset-52, inset-56, inset-60, inset-64, inset-72, inset-80, inset-96
inset-auto, inset-1/2, inset-1/3, inset-2/3, inset-1/4, inset-2/4, inset-3/4, inset-full
```

#### Visibility & Z-Index
```
visible, invisible, collapse
z-0, z-10, z-20, z-30, z-40, z-50, z-auto
```

#### Overflow
```
overflow-auto, overflow-hidden, overflow-clip, overflow-visible, overflow-scroll
overflow-x-auto, overflow-x-hidden, overflow-x-clip, overflow-x-visible, overflow-x-scroll
overflow-y-auto, overflow-y-hidden, overflow-y-clip, overflow-y-visible, overflow-y-scroll
```

### Flexbox & Grid

#### Flex Direction
```
flex-row, flex-row-reverse, flex-col, flex-col-reverse
```

#### Flex Wrap
```
flex-wrap, flex-wrap-reverse, flex-nowrap
```

#### Flex
```
flex-1, flex-auto, flex-initial, flex-none
```

#### Grow & Shrink
```
grow, grow-0, shrink, shrink-0
```

#### Order
```
order-1, order-2, order-3, order-4, order-5, order-6, order-7, order-8, order-9, order-10, order-11, order-12, order-first, order-last, order-none
```

#### Grid Template Columns
```
grid-cols-1, grid-cols-2, grid-cols-3, grid-cols-4, grid-cols-5, grid-cols-6, grid-cols-7, grid-cols-8, grid-cols-9, grid-cols-10, grid-cols-11, grid-cols-12, grid-cols-none, grid-cols-subgrid
```

#### Grid Column Start/End
```
col-auto, col-span-1, col-span-2, col-span-3, col-span-4, col-span-5, col-span-6, col-span-7, col-span-8, col-span-9, col-span-10, col-span-11, col-span-12, col-span-full
col-start-1, col-start-2, col-start-3, col-start-4, col-start-5, col-start-6, col-start-7, col-start-8, col-start-9, col-start-10, col-start-11, col-start-12, col-start-13, col-start-auto
col-end-1, col-end-2, col-end-3, col-end-4, col-end-5, col-end-6, col-end-7, col-end-8, col-end-9, col-end-10, col-end-11, col-end-12, col-end-13, col-end-auto
```

#### Grid Template Rows
```
grid-rows-1, grid-rows-2, grid-rows-3, grid-rows-4, grid-rows-5, grid-rows-6, grid-rows-7, grid-rows-8, grid-rows-9, grid-rows-10, grid-rows-11, grid-rows-12, grid-rows-none, grid-rows-subgrid
```

#### Grid Row Start/End
```
row-auto, row-span-1, row-span-2, row-span-3, row-span-4, row-span-5, row-span-6, row-span-7, row-span-8, row-span-9, row-span-10, row-span-11, row-span-12, row-span-full
row-start-1, row-start-2, row-start-3, row-start-4, row-start-5, row-start-6, row-start-7, row-start-8, row-start-9, row-start-10, row-start-11, row-start-12, row-start-13, row-start-auto
row-end-1, row-end-2, row-end-3, row-end-4, row-end-5, row-end-6, row-end-7, row-end-8, row-end-9, row-end-10, row-end-11, row-end-12, row-end-13, row-end-auto
```

#### Gap
```
gap-0, gap-x-0, gap-y-0, gap-px, gap-x-px, gap-y-px
gap-0.5, gap-1, gap-1.5, gap-2, gap-2.5, gap-3, gap-3.5, gap-4, gap-5, gap-6, gap-7, gap-8, gap-9, gap-10, gap-11, gap-12, gap-14, gap-16, gap-20, gap-24, gap-28, gap-32, gap-36, gap-40, gap-44, gap-48, gap-52, gap-56, gap-60, gap-64, gap-72, gap-80, gap-96
```

#### Justify & Align
```
justify-normal, justify-start, justify-end, justify-center, justify-between, justify-around, justify-evenly, justify-stretch
justify-items-start, justify-items-end, justify-items-center, justify-items-stretch
justify-self-auto, justify-self-start, justify-self-end, justify-self-center, justify-self-stretch
align-items-start, align-items-end, align-items-center, align-items-baseline, align-items-stretch
align-content-normal, align-content-center, align-content-start, align-content-end, align-content-between, align-content-around, align-content-evenly, align-content-baseline, align-content-stretch
align-self-auto, align-self-start, align-self-end, align-self-center, align-self-stretch, align-self-baseline
place-content-center, place-content-start, place-content-end, place-content-between, place-content-around, place-content-evenly, place-content-baseline, place-content-stretch
place-items-start, place-items-end, place-items-center, place-items-baseline, place-items-stretch
place-self-auto, place-self-start, place-self-end, place-self-center, place-self-stretch
```

### Spacing

#### Padding
```
p-0, p-px, p-0.5, p-1, p-1.5, p-2, p-2.5, p-3, p-3.5, p-4, p-5, p-6, p-7, p-8, p-9, p-10, p-11, p-12, p-14, p-16, p-20, p-24, p-28, p-32, p-36, p-40, p-44, p-48, p-52, p-56, p-60, p-64, p-72, p-80, p-96
px-0, py-0, pl-0, pr-0, pt-0, pb-0, ps-0, pe-0
```

#### Margin
```
m-0, m-px, m-0.5, m-1, m-1.5, m-2, m-2.5, m-3, m-3.5, m-4, m-5, m-6, m-7, m-8, m-9, m-10, m-11, m-12, m-14, m-16, m-20, m-24, m-28, m-32, m-36, m-40, m-44, m-48, m-52, m-56, m-60, m-64, m-72, m-80, m-96, m-auto
mx-0, my-0, ml-0, mr-0, mt-0, mb-0, ms-0, me-0
-m-0, -m-px, -m-0.5, -m-1, -m-1.5, -m-2, -m-2.5, -m-3, -m-3.5, -m-4, -m-5, -m-6, -m-7, -m-8, -m-9, -m-10, -m-11, -m-12, -m-14, -m-16, -m-20, -m-24, -m-28, -m-32, -m-36, -m-40, -m-44, -m-48, -m-52, -m-56, -m-60, -m-64, -m-72, -m-80, -m-96
```

#### Space Between
```
space-x-0, space-x-px, space-x-0.5, space-x-1, space-x-1.5, space-x-2, space-x-2.5, space-x-3, space-x-3.5, space-x-4, space-x-5, space-x-6, space-x-7, space-x-8, space-x-9, space-x-10, space-x-11, space-x-12, space-x-14, space-x-16, space-x-20, space-x-24, space-x-28, space-x-32, space-x-36, space-x-40, space-x-44, space-x-48, space-x-52, space-x-56, space-x-60, space-x-64, space-x-72, space-x-80, space-x-96, space-x-reverse
space-y-0, space-y-px, space-y-0.5, space-y-1, space-y-1.5, space-y-2, space-y-2.5, space-y-3, space-y-3.5, space-y-4, space-y-5, space-y-6, space-y-7, space-y-8, space-y-9, space-y-10, space-y-11, space-y-12, space-y-14, space-y-16, space-y-20, space-y-24, space-y-28, space-y-32, space-y-36, space-y-40, space-y-44, space-y-48, space-y-52, space-y-56, space-y-60, space-y-64, space-y-72, space-y-80, space-y-96, space-y-reverse
```

### Sizing

#### Width
```
w-0, w-px, w-0.5, w-1, w-1.5, w-2, w-2.5, w-3, w-3.5, w-4, w-5, w-6, w-7, w-8, w-9, w-10, w-11, w-12, w-14, w-16, w-20, w-24, w-28, w-32, w-36, w-40, w-44, w-48, w-52, w-56, w-60, w-64, w-72, w-80, w-96, w-auto, w-1/2, w-1/3, w-2/3, w-1/4, w-2/4, w-3/4, w-1/5, w-2/5, w-3/5, w-4/5, w-1/6, w-2/6, w-3/6, w-4/6, w-5/6, w-1/12, w-2/12, w-3/12, w-4/12, w-5/12, w-6/12, w-7/12, w-8/12, w-9/12, w-10/12, w-11/12, w-full, w-screen, w-svw, w-lvw, w-dvw, w-min, w-max, w-fit
```

#### Height
```
h-0, h-px, h-0.5, h-1, h-1.5, h-2, h-2.5, h-3, h-3.5, h-4, h-5, h-6, h-7, h-8, h-9, h-10, h-11, h-12, h-14, h-16, h-20, h-24, h-28, h-32, h-36, h-40, h-44, h-48, h-52, h-56, h-60, h-64, h-72, h-80, h-96, h-auto, h-1/2, h-1/3, h-2/3, h-1/4, h-2/4, h-3/4, h-1/5, h-2/5, h-3/5, h-4/5, h-1/6, h-2/6, h-3/6, h-4/6, h-5/6, h-full, h-screen, h-svh, h-lvh, h-dvh, h-min, h-max, h-fit
```

#### Size (Width + Height)
```
size-0, size-px, size-0.5, size-1, size-1.5, size-2, size-2.5, size-3, size-3.5, size-4, size-5, size-6, size-7, size-8, size-9, size-10, size-11, size-12, size-14, size-16, size-20, size-24, size-28, size-32, size-36, size-40, size-44, size-48, size-52, size-56, size-60, size-64, size-72, size-80, size-96, size-auto, size-1/2, size-1/3, size-2/3, size-1/4, size-2/4, size-3/4, size-1/5, size-2/5, size-3/5, size-4/5, size-1/6, size-2/6, size-3/6, size-4/6, size-5/6, size-1/12, size-2/12, size-3/12, size-4/12, size-5/12, size-6/12, size-7/12, size-8/12, size-9/12, size-10/12, size-11/12, size-full, size-min, size-max, size-fit
```

#### Min/Max Width
```
min-w-0, min-w-full, min-w-min, min-w-max, min-w-fit
max-w-0, max-w-xs, max-w-sm, max-w-md, max-w-lg, max-w-xl, max-w-2xl, max-w-3xl, max-w-4xl, max-w-5xl, max-w-6xl, max-w-7xl, max-w-full, max-w-min, max-w-max, max-w-fit, max-w-prose, max-w-screen-sm, max-w-screen-md, max-w-screen-lg, max-w-screen-xl, max-w-screen-2xl
```

#### Min/Max Height
```
min-h-0, min-h-full, min-h-screen, min-h-svh, min-h-lvh, min-h-dvh, min-h-min, min-h-max, min-h-fit
max-h-0, max-h-px, max-h-0.5, max-h-1, max-h-1.5, max-h-2, max-h-2.5, max-h-3, max-h-3.5, max-h-4, max-h-5, max-h-6, max-h-7, max-h-8, max-h-9, max-h-10, max-h-11, max-h-12, max-h-14, max-h-16, max-h-20, max-h-24, max-h-28, max-h-32, max-h-36, max-h-40, max-h-44, max-h-48, max-h-52, max-h-56, max-h-60, max-h-64, max-h-72, max-h-80, max-h-96, max-h-full, max-h-screen, max-h-svh, max-h-lvh, max-h-dvh, max-h-min, max-h-max, max-h-fit
```

### Typography

#### Font Family
```
font-sans, font-serif, font-mono
```

#### Font Size
```
text-xs, text-sm, text-base, text-lg, text-xl, text-2xl, text-3xl, text-4xl, text-5xl, text-6xl, text-7xl, text-8xl, text-9xl
```

#### Font Weight
```
font-thin, font-extralight, font-light, font-normal, font-medium, font-semibold, font-bold, font-extrabold, font-black
```

#### Font Style
```
italic, not-italic
```

#### Font Variant Numeric
```
normal-nums, ordinal, slashed-zero, lining-nums, oldstyle-nums, proportional-nums, tabular-nums, diagonal-fractions, stacked-fractions
```

#### Font Stretch (NEW in v4)
```
font-stretch-ultra-condensed, font-stretch-extra-condensed, font-stretch-condensed, font-stretch-semi-condensed, font-stretch-normal, font-stretch-semi-expanded, font-stretch-expanded, font-stretch-extra-expanded, font-stretch-ultra-expanded
```

#### Letter Spacing
```
tracking-tighter, tracking-tight, tracking-normal, tracking-wide, tracking-wider, tracking-widest
```

#### Line Clamp
```
line-clamp-1, line-clamp-2, line-clamp-3, line-clamp-4, line-clamp-5, line-clamp-6, line-clamp-none
```

#### Line Height
```
leading-3, leading-4, leading-5, leading-6, leading-7, leading-8, leading-9, leading-10, leading-none, leading-tight, leading-snug, leading-normal, leading-relaxed, leading-loose
```

#### List Style
```
list-none, list-disc, list-decimal, list-inside, list-outside
```

#### Text Align
```
text-left, text-center, text-right, text-justify, text-start, text-end
```

#### Text Color
```
text-inherit, text-current, text-transparent, text-black, text-white
text-slate-50, text-slate-100, text-slate-200, text-slate-300, text-slate-400, text-slate-500, text-slate-600, text-slate-700, text-slate-800, text-slate-900, text-slate-950
text-gray-50, text-gray-100, text-gray-200, text-gray-300, text-gray-400, text-gray-500, text-gray-600, text-gray-700, text-gray-800, text-gray-900, text-gray-950
text-zinc-50, text-zinc-100, text-zinc-200, text-zinc-300, text-zinc-400, text-zinc-500, text-zinc-600, text-zinc-700, text-zinc-800, text-zinc-900, text-zinc-950
text-neutral-50, text-neutral-100, text-neutral-200, text-neutral-300, text-neutral-400, text-neutral-500, text-neutral-600, text-neutral-700, text-neutral-800, text-neutral-900, text-neutral-950
text-stone-50, text-stone-100, text-stone-200, text-stone-300, text-stone-400, text-stone-500, text-stone-600, text-stone-700, text-stone-800, text-stone-900, text-stone-950
text-red-50, text-red-100, text-red-200, text-red-300, text-red-400, text-red-500, text-red-600, text-red-700, text-red-800, text-red-900, text-red-950
text-orange-50, text-orange-100, text-orange-200, text-orange-300, text-orange-400, text-orange-500, text-orange-600, text-orange-700, text-orange-800, text-orange-900, text-orange-950
text-amber-50, text-amber-100, text-amber-200, text-amber-300, text-amber-400, text-amber-500, text-amber-600, text-amber-700, text-amber-800, text-amber-900, text-amber-950
text-yellow-50, text-yellow-100, text-yellow-200, text-yellow-300, text-yellow-400, text-yellow-500, text-yellow-600, text-yellow-700, text-yellow-800, text-yellow-900, text-yellow-950
text-lime-50, text-lime-100, text-lime-200, text-lime-300, text-lime-400, text-lime-500, text-lime-600, text-lime-700, text-lime-800, text-lime-900, text-lime-950
text-green-50, text-green-100, text-green-200, text-green-300, text-green-400, text-green-500, text-green-600, text-green-700, text-green-800, text-green-900, text-green-950
text-emerald-50, text-emerald-100, text-emerald-200, text-emerald-300, text-emerald-400, text-emerald-500, text-emerald-600, text-emerald-700, text-emerald-800, text-emerald-900, text-emerald-950
text-teal-50, text-teal-100, text-teal-200, text-teal-300, text-teal-400, text-teal-500, text-teal-600, text-teal-700, text-teal-800, text-teal-900, text-teal-950
text-cyan-50, text-cyan-100, text-cyan-200, text-cyan-300, text-cyan-400, text-cyan-500, text-cyan-600, text-cyan-700, text-cyan-800, text-cyan-900, text-cyan-950
text-sky-50, text-sky-100, text-sky-200, text-sky-300, text-sky-400, text-sky-500, text-sky-600, text-sky-700, text-sky-800, text-sky-900, text-sky-950
text-blue-50, text-blue-100, text-blue-200, text-blue-300, text-blue-400, text-blue-500, text-blue-600, text-blue-700, text-blue-800, text-blue-900, text-blue-950
text-indigo-50, text-indigo-100, text-indigo-200, text-indigo-300, text-indigo-400, text-indigo-500, text-indigo-600, text-indigo-700, text-indigo-800, text-indigo-900, text-indigo-950
text-violet-50, text-violet-100, text-violet-200, text-violet-300, text-violet-400, text-violet-500, text-violet-600, text-violet-700, text-violet-800, text-violet-900, text-violet-950
text-purple-50, text-purple-100, text-purple-200, text-purple-300, text-purple-400, text-purple-500, text-purple-600, text-purple-700, text-purple-800, text-purple-900, text-purple-950
text-fuchsia-50, text-fuchsia-100, text-fuchsia-200, text-fuchsia-300, text-fuchsia-400, text-fuchsia-500, text-fuchsia-600, text-fuchsia-700, text-fuchsia-800, text-fuchsia-900, text-fuchsia-950
text-pink-50, text-pink-100, text-pink-200, text-pink-300, text-pink-400, text-pink-500, text-pink-600, text-pink-700, text-pink-800, text-pink-900, text-pink-950
text-rose-50, text-rose-100, text-rose-200, text-rose-300, text-rose-400, text-rose-500, text-rose-600, text-rose-700, text-rose-800, text-rose-900, text-rose-950
```

#### Text Decoration
```
underline, overline, line-through, no-underline
decoration-solid, decoration-double, decoration-dotted, decoration-dashed, decoration-wavy
decoration-auto, decoration-from-font, decoration-0, decoration-1, decoration-2, decoration-4, decoration-8
underline-offset-auto, underline-offset-0, underline-offset-1, underline-offset-2, underline-offset-4, underline-offset-8
```

#### Text Transform
```
uppercase, lowercase, capitalize, normal-case
```

#### Text Overflow
```
truncate, text-ellipsis, text-clip
```

#### Text Shadow (NEW in v4)
```
text-shadow-sm, text-shadow, text-shadow-lg, text-shadow-xl, text-shadow-none
```

#### Overflow Wrap (NEW in v4)
```
overflow-wrap-normal, overflow-wrap-break-word, overflow-wrap-anywhere
```

#### Vertical Align
```
align-baseline, align-top, align-middle, align-bottom, align-text-top, align-text-bottom, align-sub, align-super
```

#### Whitespace
```
whitespace-normal, whitespace-nowrap, whitespace-pre, whitespace-pre-line, whitespace-pre-wrap, whitespace-break-spaces
```

#### Word Break
```
break-normal, break-words, break-all, break-keep
```

#### Hyphens
```
hyphens-none, hyphens-manual, hyphens-auto
```

#### Content
```
content-none
```

### Backgrounds

#### Background Attachment
```
bg-fixed, bg-local, bg-scroll
```

#### Background Clip
```
bg-clip-border, bg-clip-padding, bg-clip-content, bg-clip-text
```

#### Background Color
All color utilities work with `bg-` prefix (same as text colors above)

#### Background Origin
```
bg-origin-border, bg-origin-padding, bg-origin-content
```

#### Background Position
```
bg-bottom, bg-center, bg-left, bg-left-bottom, bg-left-top, bg-right, bg-right-bottom, bg-right-top, bg-top
```

#### Background Repeat
```
bg-repeat, bg-no-repeat, bg-repeat-x, bg-repeat-y, bg-repeat-round, bg-repeat-space
```

#### Background Size
```
bg-auto, bg-cover, bg-contain
```

#### Background Image
```
bg-none
bg-linear-to-t, bg-linear-to-tr, bg-linear-to-r, bg-linear-to-br, bg-linear-to-b, bg-linear-to-bl, bg-linear-to-l, bg-linear-to-tl
```

#### Enhanced Gradients (NEW in v4)
```
bg-linear-0, bg-linear-45, bg-linear-90, bg-linear-135, bg-linear-180, bg-linear-225, bg-linear-270, bg-linear-315
bg-radial, bg-radial-at-t, bg-radial-at-tr, bg-radial-at-r, bg-radial-at-br, bg-radial-at-b, bg-radial-at-bl, bg-radial-at-l, bg-radial-at-tl, bg-radial-at-c
bg-conic, bg-conic-at-t, bg-conic-at-tr, bg-conic-at-r, bg-conic-at-br, bg-conic-at-b, bg-conic-at-bl, bg-conic-at-l, bg-conic-at-tl, bg-conic-at-c
```

#### Gradient Color Stops
```
from-inherit, from-current, from-transparent, from-black, from-white, from-{color}
via-inherit, via-current, via-transparent, via-black, via-white, via-{color}
to-inherit, to-current, to-transparent, to-black, to-white, to-{color}
```

### Borders

#### Border Radius
```
rounded-none, rounded-xs, rounded-sm, rounded, rounded-md, rounded-lg, rounded-xl, rounded-2xl, rounded-3xl, rounded-full
rounded-s-none, rounded-s-xs, rounded-s-sm, rounded-s, rounded-s-md, rounded-s-lg, rounded-s-xl, rounded-s-2xl, rounded-s-3xl
rounded-e-none, rounded-e-xs, rounded-e-sm, rounded-e, rounded-e-md, rounded-e-lg, rounded-e-xl, rounded-e-2xl, rounded-e-3xl
rounded-t-none, rounded-t-xs, rounded-t-sm, rounded-t, rounded-t-md, rounded-t-lg, rounded-t-xl, rounded-t-2xl, rounded-t-3xl
rounded-r-none, rounded-r-xs, rounded-r-sm, rounded-r, rounded-r-md, rounded-r-lg, rounded-r-xl, rounded-r-2xl, rounded-r-3xl
rounded-b-none, rounded-b-xs, rounded-b-sm, rounded-b, rounded-b-md, rounded-b-lg, rounded-b-xl, rounded-b-2xl, rounded-b-3xl
rounded-l-none, rounded-l-xs, rounded-l-sm, rounded-l, rounded-l-md, rounded-l-lg, rounded-l-xl, rounded-l-2xl, rounded-l-3xl
rounded-ss-none, rounded-ss-xs, rounded-ss-sm, rounded-ss, rounded-ss-md, rounded-ss-lg, rounded-ss-xl, rounded-ss-2xl, rounded-ss-3xl
rounded-se-none, rounded-se-xs, rounded-se-sm, rounded-se, rounded-se-md, rounded-se-lg, rounded-se-xl, rounded-se-2xl, rounded-se-3xl
rounded-ee-none, rounded-ee-xs, rounded-ee-sm, rounded-ee, rounded-ee-md, rounded-ee-lg, rounded-ee-xl, rounded-ee-2xl, rounded-ee-3xl
rounded-es-none, rounded-es-xs, rounded-es-sm, rounded-es, rounded-es-md, rounded-es-lg, rounded-es-xl, rounded-es-2xl, rounded-es-3xl
rounded-tl-none, rounded-tl-xs, rounded-tl-sm, rounded-tl, rounded-tl-md, rounded-tl-lg, rounded-tl-xl, rounded-tl-2xl, rounded-tl-3xl
rounded-tr-none, rounded-tr-xs, rounded-tr-sm, rounded-tr, rounded-tr-md, rounded-tr-lg, rounded-tr-xl, rounded-tr-2xl, rounded-tr-3xl
rounded-br-none, rounded-br-xs, rounded-br-sm, rounded-br, rounded-br-md, rounded-br-lg, rounded-br-xl, rounded-br-2xl, rounded-br-3xl
rounded-bl-none, rounded-bl-xs, rounded-bl-sm, rounded-bl, rounded-bl-md, rounded-bl-lg, rounded-bl-xl, rounded-bl-2xl, rounded-bl-3xl
```

#### Border Width
```
border-0, border-2, border-4, border-8, border, border-x, border-y, border-s, border-e, border-t, border-r, border-b, border-l
```

#### Border Color
All color utilities work with `border-` prefix (same as text/bg colors)

#### Border Style
```
border-solid, border-dashed, border-dotted, border-double, border-hidden, border-none
```

#### Divide Width
```
divide-x-0, divide-x-2, divide-x-4, divide-x-8, divide-x, divide-y-0, divide-y-2, divide-y-4, divide-y-8, divide-y, divide-x-reverse, divide-y-reverse
```

#### Divide Color
All color utilities work with `divide-` prefix

#### Divide Style
```
divide-solid, divide-dashed, divide-dotted, divide-double, divide-none
```

#### Outline Width
```
outline-0, outline-1, outline-2, outline-4, outline-8
```

#### Outline Color
All color utilities work with `outline-` prefix

#### Outline Style
```
outline-none, outline, outline-dashed, outline-dotted, outline-double
```

#### Outline Offset
```
outline-offset-0, outline-offset-1, outline-offset-2, outline-offset-4, outline-offset-8
```

#### Ring Width
```
ring-0, ring-1, ring-2, ring, ring-4, ring-8, ring-inset
```

#### Ring Color
All color utilities work with `ring-` prefix

#### Ring Offset Width
```
ring-offset-0, ring-offset-1, ring-offset-2, ring-offset-4, ring-offset-8
```

#### Ring Offset Color
All color utilities work with `ring-offset-` prefix

### Effects

#### Box Shadow
```
shadow-xs, shadow-sm, shadow, shadow-md, shadow-lg, shadow-xl, shadow-2xl, shadow-inner, shadow-none
```

#### Box Shadow Color
All color utilities work with `shadow-` prefix

#### Drop Shadow (NEW colored support in v4)
```
drop-shadow-sm, drop-shadow, drop-shadow-md, drop-shadow-lg, drop-shadow-xl, drop-shadow-2xl, drop-shadow-none
```

#### Opacity
```
opacity-0, opacity-5, opacity-10, opacity-15, opacity-20, opacity-25, opacity-30, opacity-35, opacity-40, opacity-45, opacity-50, opacity-55, opacity-60, opacity-65, opacity-70, opacity-75, opacity-80, opacity-85, opacity-90, opacity-95, opacity-100
```

#### Mix Blend Mode
```
mix-blend-normal, mix-blend-multiply, mix-blend-screen, mix-blend-overlay, mix-blend-darken, mix-blend-lighten, mix-blend-color-dodge, mix-blend-color-burn, mix-blend-hard-light, mix-blend-soft-light, mix-blend-difference, mix-blend-exclusion, mix-blend-hue, mix-blend-saturation, mix-blend-color, mix-blend-luminosity, mix-blend-plus-darker, mix-blend-plus-lighter
```

#### Background Blend Mode
```
bg-blend-normal, bg-blend-multiply, bg-blend-screen, bg-blend-overlay, bg-blend-darken, bg-blend-lighten, bg-blend-color-dodge, bg-blend-color-burn, bg-blend-hard-light, bg-blend-soft-light, bg-blend-difference, bg-blend-exclusion, bg-blend-hue, bg-blend-saturation, bg-blend-color, bg-blend-luminosity
```

### Filters

#### Blur
```
blur-none, blur-xs, blur-sm, blur, blur-md, blur-lg, blur-xl, blur-2xl, blur-3xl
```

#### Brightness
```
brightness-0, brightness-50, brightness-75, brightness-90, brightness-95, brightness-100, brightness-105, brightness-110, brightness-125, brightness-150, brightness-200
```

#### Contrast
```
contrast-0, contrast-50, contrast-75, contrast-100, contrast-125, contrast-150, contrast-200
```

#### Grayscale
```
grayscale-0, grayscale
```

#### Hue Rotate
```
hue-rotate-0, hue-rotate-15, hue-rotate-30, hue-rotate-60, hue-rotate-90, hue-rotate-180, -hue-rotate-180, -hue-rotate-90, -hue-rotate-60, -hue-rotate-30, -hue-rotate-15
```

#### Invert
```
invert-0, invert
```

#### Saturate
```
saturate-0, saturate-50, saturate-100, saturate-150, saturate-200
```

#### Sepia
```
sepia-0, sepia
```

#### Backdrop Blur
```
backdrop-blur-none, backdrop-blur-xs, backdrop-blur-sm, backdrop-blur, backdrop-blur-md, backdrop-blur-lg, backdrop-blur-xl, backdrop-blur-2xl, backdrop-blur-3xl
```

#### Backdrop Brightness
```
backdrop-brightness-0, backdrop-brightness-50, backdrop-brightness-75, backdrop-brightness-90, backdrop-brightness-95, backdrop-brightness-100, backdrop-brightness-105, backdrop-brightness-110, backdrop-brightness-125, backdrop-brightness-150, backdrop-brightness-200
```

#### Backdrop Contrast
```
backdrop-contrast-0, backdrop-contrast-50, backdrop-contrast-75, backdrop-contrast-100, backdrop-contrast-125, backdrop-contrast-150, backdrop-contrast-200
```

#### Backdrop Grayscale
```
backdrop-grayscale-0, backdrop-grayscale
```

#### Backdrop Hue Rotate
```
backdrop-hue-rotate-0, backdrop-hue-rotate-15, backdrop-hue-rotate-30, backdrop-hue-rotate-60, backdrop-hue-rotate-90, backdrop-hue-rotate-180, -backdrop-hue-rotate-180, -backdrop-hue-rotate-90, -backdrop-hue-rotate-60, -backdrop-hue-rotate-30, -backdrop-hue-rotate-15
```

#### Backdrop Invert
```
backdrop-invert-0, backdrop-invert
```

#### Backdrop Saturate
```
backdrop-saturate-0, backdrop-saturate-50, backdrop-saturate-100, backdrop-saturate-150, backdrop-saturate-200
```

#### Backdrop Sepia
```
backdrop-sepia-0, backdrop-sepia
```

### Masks (NEW in v4)

#### Mask Image
```
mask-none
```

#### Mask Size
```
mask-auto, mask-cover, mask-contain
```

#### Mask Repeat
```
mask-repeat, mask-no-repeat, mask-repeat-x, mask-repeat-y, mask-repeat-round, mask-repeat-space
```

#### Mask Position
```
mask-bottom, mask-center, mask-left, mask-left-bottom, mask-left-top, mask-right, mask-right-bottom, mask-right-top, mask-top
```

#### Mask Origin
```
mask-origin-border, mask-origin-padding, mask-origin-content
```

#### Mask Clip
```
mask-clip-border, mask-clip-padding, mask-clip-content
```

#### Mask Composite
```
mask-composite-add, mask-composite-subtract, mask-composite-intersect, mask-composite-exclude
```

#### Mask Mode
```
mask-mode-match-source, mask-mode-luminance, mask-mode-alpha
```

#### Mask Type
```
mask-type-luminance, mask-type-alpha
```

### Tables

#### Border Collapse
```
border-collapse, border-separate
```

#### Border Spacing
```
border-spacing-0, border-spacing-px, border-spacing-0.5, border-spacing-1, border-spacing-1.5, border-spacing-2, border-spacing-2.5, border-spacing-3, border-spacing-3.5, border-spacing-4, border-spacing-5, border-spacing-6, border-spacing-7, border-spacing-8, border-spacing-9, border-spacing-10, border-spacing-11, border-spacing-12, border-spacing-14, border-spacing-16, border-spacing-20, border-spacing-24, border-spacing-28, border-spacing-32, border-spacing-36, border-spacing-40, border-spacing-44, border-spacing-48, border-spacing-52, border-spacing-56, border-spacing-60, border-spacing-64, border-spacing-72, border-spacing-80, border-spacing-96
border-spacing-x-0, border-spacing-y-0 (and all other spacing values)
```

#### Table Layout
```
table-auto, table-fixed
```

#### Caption Side
```
caption-top, caption-bottom
```

### Transforms

#### Scale
```
scale-0, scale-50, scale-75, scale-90, scale-95, scale-100, scale-105, scale-110, scale-125, scale-150
scale-x-0, scale-x-50, scale-x-75, scale-x-90, scale-x-95, scale-x-100, scale-x-105, scale-x-110, scale-x-125, scale-x-150
scale-y-0, scale-y-50, scale-y-75, scale-y-90, scale-y-95, scale-y-100, scale-y-105, scale-y-110, scale-y-125, scale-y-150
```

#### Scale Z (NEW in v4)
```
scale-z-0, scale-z-50, scale-z-75, scale-z-90, scale-z-95, scale-z-100, scale-z-105, scale-z-110, scale-z-125, scale-z-150
```

#### Rotate
```
rotate-0, rotate-1, rotate-2, rotate-3, rotate-6, rotate-12, rotate-45, rotate-90, rotate-180, -rotate-180, -rotate-90, -rotate-45, -rotate-12, -rotate-6, -rotate-3, -rotate-2, -rotate-1
```

#### Rotate X/Y (NEW in v4)
```
rotate-x-0, rotate-x-1, rotate-x-2, rotate-x-3, rotate-x-6, rotate-x-12, rotate-x-45, rotate-x-90, rotate-x-180, -rotate-x-180, -rotate-x-90, -rotate-x-45, -rotate-x-12, -rotate-x-6, -rotate-x-3, -rotate-x-2, -rotate-x-1
rotate-y-0, rotate-y-1, rotate-y-2, rotate-y-3, rotate-y-6, rotate-y-12, rotate-y-45, rotate-y-90, rotate-y-180, -rotate-y-180, -rotate-y-90, -rotate-y-45, -rotate-y-12, -rotate-y-6, -rotate-y-3, -rotate-y-2, -rotate-y-1
```

#### Translate
```
translate-x-0, translate-x-px, translate-x-0.5, translate-x-1, translate-x-1.5, translate-x-2, translate-x-2.5, translate-x-3, translate-x-3.5, translate-x-4, translate-x-5, translate-x-6, translate-x-7, translate-x-8, translate-x-9, translate-x-10, translate-x-11, translate-x-12, translate-x-14, translate-x-16, translate-x-20, translate-x-24, translate-x-28, translate-x-32, translate-x-36, translate-x-40, translate-x-44, translate-x-48, translate-x-52, translate-x-56, translate-x-60, translate-x-64, translate-x-72, translate-x-80, translate-x-96, translate-x-1/2, translate-x-1/3, translate-x-2/3, translate-x-1/4, translate-x-2/4, translate-x-3/4, translate-x-full
translate-y-0, translate-y-px, translate-y-0.5, translate-y-1, translate-y-1.5, translate-y-2, translate-y-2.5, translate-y-3, translate-y-3.5, translate-y-4, translate-y-5, translate-y-6, translate-y-7, translate-y-8, translate-y-9, translate-y-10, translate-y-11, translate-y-12, translate-y-14, translate-y-16, translate-y-20, translate-y-24, translate-y-28, translate-y-32, translate-y-36, translate-y-40, translate-y-44, translate-y-48, translate-y-52, translate-y-56, translate-y-60, translate-y-64, translate-y-72, translate-y-80, translate-y-96, translate-y-1/2, translate-y-1/3, translate-y-2/3, translate-y-1/4, translate-y-2/4, translate-y-3/4, translate-y-full
```

#### Translate Z (NEW in v4)
```
translate-z-0, translate-z-px, translate-z-0.5, translate-z-1, translate-z-1.5, translate-z-2, translate-z-2.5, translate-z-3, translate-z-3.5, translate-z-4, translate-z-5, translate-z-6, translate-z-7, translate-z-8, translate-z-9, translate-z-10, translate-z-11, translate-z-12, translate-z-14, translate-z-16, translate-z-20, translate-z-24, translate-z-28, translate-z-32, translate-z-36, translate-z-40, translate-z-44, translate-z-48, translate-z-52, translate-z-56, translate-z-60, translate-z-64, translate-z-72, translate-z-80, translate-z-96
```

#### Skew
```
skew-x-0, skew-x-1, skew-x-2, skew-x-3, skew-x-6, skew-x-12, -skew-x-12, -skew-x-6, -skew-x-3, -skew-x-2, -skew-x-1
skew-y-0, skew-y-1, skew-y-2, skew-y-3, skew-y-6, skew-y-12, -skew-y-12, -skew-y-6, -skew-y-3, -skew-y-2, -skew-y-1
```

#### Transform Origin
```
origin-center, origin-top, origin-top-right, origin-right, origin-bottom-right, origin-bottom, origin-bottom-left, origin-left, origin-top-left
```

#### Transform Style (NEW in v4)
```
transform-style-flat, transform-style-preserve-3d
```

#### Perspective (NEW in v4)
```
perspective-none, perspective-250, perspective-500, perspective-750, perspective-1000, perspective-distant
```

#### Perspective Origin (NEW in v4)
```
perspective-origin-center, perspective-origin-top, perspective-origin-top-right, perspective-origin-right, perspective-origin-bottom-right, perspective-origin-bottom, perspective-origin-bottom-left, perspective-origin-left, perspective-origin-top-left
```

### Interactivity

#### Accent Color
```
accent-auto, accent-inherit, accent-current, accent-transparent, accent-black, accent-white
accent-{color} (all color utilities work with accent- prefix)
```

#### Appearance
```
appearance-none, appearance-auto
```

#### Cursor
```
cursor-auto, cursor-default, cursor-pointer, cursor-wait, cursor-text, cursor-move, cursor-help, cursor-not-allowed, cursor-none, cursor-context-menu, cursor-progress, cursor-cell, cursor-crosshair, cursor-vertical-text, cursor-alias, cursor-copy, cursor-no-drop, cursor-grab, cursor-grabbing, cursor-all-scroll, cursor-col-resize, cursor-row-resize, cursor-n-resize, cursor-e-resize, cursor-s-resize, cursor-w-resize, cursor-ne-resize, cursor-nw-resize, cursor-se-resize, cursor-sw-resize, cursor-ew-resize, cursor-ns-resize, cursor-nesw-resize, cursor-nwse-resize, cursor-zoom-in, cursor-zoom-out
```

#### Caret Color
All color utilities work with `caret-` prefix

#### Pointer Events
```
pointer-events-none, pointer-events-auto
```

#### Resize
```
resize-none, resize, resize-y, resize-x
```

#### Scroll Behavior
```
scroll-auto, scroll-smooth
```

#### Scroll Margin
```
scroll-m-0, scroll-m-px, scroll-m-0.5, scroll-m-1, scroll-m-1.5, scroll-m-2, scroll-m-2.5, scroll-m-3, scroll-m-3.5, scroll-m-4, scroll-m-5, scroll-m-6, scroll-m-7, scroll-m-8, scroll-m-9, scroll-m-10, scroll-m-11, scroll-m-12, scroll-m-14, scroll-m-16, scroll-m-20, scroll-m-24, scroll-m-28, scroll-m-32, scroll-m-36, scroll-m-40, scroll-m-44, scroll-m-48, scroll-m-52, scroll-m-56, scroll-m-60, scroll-m-64, scroll-m-72, scroll-m-80, scroll-m-96
scroll-mx-0, scroll-my-0, scroll-ms-0, scroll-me-0, scroll-mt-0, scroll-mr-0, scroll-mb-0, scroll-ml-0 (and all spacing values)
```

#### Scroll Padding
```
scroll-p-0, scroll-p-px, scroll-p-0.5, scroll-p-1, scroll-p-1.5, scroll-p-2, scroll-p-2.5, scroll-p-3, scroll-p-3.5, scroll-p-4, scroll-p-5, scroll-p-6, scroll-p-7, scroll-p-8, scroll-p-9, scroll-p-10, scroll-p-11, scroll-p-12, scroll-p-14, scroll-p-16, scroll-p-20, scroll-p-24, scroll-p-28, scroll-p-32, scroll-p-36, scroll-p-40, scroll-p-44, scroll-p-48, scroll-p-52, scroll-p-56, scroll-p-60, scroll-p-64, scroll-p-72, scroll-p-80, scroll-p-96
scroll-px-0, scroll-py-0, scroll-ps-0, scroll-pe-0, scroll-pt-0, scroll-pr-0, scroll-pb-0, scroll-pl-0 (and all spacing values)
```

#### Scroll Snap Align
```
snap-start, snap-end, snap-center, snap-align-none
```

#### Scroll Snap Stop
```
snap-normal, snap-always
```

#### Scroll Snap Type
```
snap-none, snap-x, snap-y, snap-both, snap-mandatory, snap-proximity
```

#### Touch Action
```
touch-auto, touch-none, touch-pan-x, touch-pan-left, touch-pan-right, touch-pan-y, touch-pan-up, touch-pan-down, touch-pinch-zoom, touch-manipulation
```

#### User Select
```
select-none, select-text, select-all, select-auto
```

#### Will Change
```
will-change-auto, will-change-scroll, will-change-contents, will-change-transform
```

### SVG

#### Fill
```
fill-none, fill-inherit, fill-current, fill-transparent, fill-black, fill-white
fill-{color} (all color utilities work with fill- prefix)
```

#### Stroke
```
stroke-none, stroke-inherit, stroke-current, stroke-transparent, stroke-black, stroke-white
stroke-{color} (all color utilities work with stroke- prefix)
```

#### Stroke Width
```
stroke-0, stroke-1, stroke-2
```

### Accessibility

#### Screen Readers
```
sr-only, not-sr-only
```

#### Forced Color Adjust
```
forced-color-adjust-auto, forced-color-adjust-none
```

### Container Queries (NEW in v4)

#### Container Type
```
@container, @container-normal, @container-size, @container-inline-size
```

#### Container Query Variants
```
@xs:, @sm:, @md:, @lg:, @xl:, @2xl:, @3xl:, @4xl:, @5xl:, @6xl:, @7xl:
@min-w-0:, @min-w-xs:, @min-w-sm:, @min-w-md:, @min-w-lg:, @min-w-xl:, @min-w-2xl:, @min-w-3xl:, @min-w-4xl:, @min-w-5xl:, @min-w-6xl:, @min-w-7xl:
@max-w-xs:, @max-w-sm:, @max-w-md:, @max-w-lg:, @max-w-xl:, @max-w-2xl:, @max-w-3xl:, @max-w-4xl:, @max-w-5xl:, @max-w-6xl:, @max-w-7xl:
@min-h-0:, @min-h-xs:, @min-h-sm:, @min-h-md:, @min-h-lg:, @min-h-xl:, @min-h-2xl:, @min-h-3xl:, @min-h-4xl:, @min-h-5xl:, @min-h-6xl:, @min-h-7xl:
@max-h-xs:, @max-h-sm:, @max-h-md:, @max-h-lg:, @max-h-xl:, @max-h-2xl:, @max-h-3xl:, @max-h-4xl:, @max-h-5xl:, @max-h-6xl:, @max-h-7xl:
```

### Responsive Design

#### Breakpoint Variants
```
sm:, md:, lg:, xl:, 2xl:
max-sm:, max-md:, max-lg:, max-xl:, max-2xl:
```

### State Variants

#### Hover, Focus, etc.
```
hover:, focus:, focus-within:, focus-visible:, active:, visited:, target:, disabled:, enabled:, checked:, indeterminate:, default:, required:, valid:, invalid:, in-range:, out-of-range:, placeholder-shown:, autofill:, read-only:
```

#### Group States
```
group-hover:, group-focus:, group-focus-within:, group-focus-visible:, group-active:, group-visited:, group-target:, group-disabled:, group-enabled:, group-checked:, group-indeterminate:, group-default:, group-required:, group-valid:, group-invalid:, group-in-range:, group-out-of-range:, group-placeholder-shown:, group-autofill:, group-read-only:
```

#### Peer States
```
peer-hover:, peer-focus:, peer-focus-within:, peer-focus-visible:, peer-active:, peer-visited:, peer-target:, peer-disabled:, peer-enabled:, peer-checked:, peer-indeterminate:, peer-default:, peer-required:, peer-valid:, peer-invalid:, peer-in-range:, peer-out-of-range:, peer-placeholder-shown:, peer-autofill:, peer-read-only:
```

#### NEW Variants in v4
```
@starting-style:, not-*, nth-*, in-*, inert:, open: (for popovers)
```

### Media Queries

#### Dark Mode
```
dark:
```

#### Motion
```
motion-safe:, motion-reduce:
```

#### Contrast
```
contrast-more:, contrast-less:
```

#### Print
```
print:
```

#### Orientation
```
portrait:, landscape:
```

### Content
```
content-none, content-['text']
```

### Arbitrary Values
You can use arbitrary values with square brackets for any property:
```
w-[123px], h-[456px], text-[#bada55], bg-[url('...')], top-[117px], left-[344px]
m-[12px], p-[24px], grid-cols-[200px_minmax(900px,_1fr)_100px]
```

### Arbitrary Properties
```
[mask-type:luminance], [tab-size:4], [@supports(backdrop-filter:blur(0))]:bg-white/75
```

### Arbitrary Variants
```
[&:nth-child(3)]:, [&:hover]:, [&_p]:, [@media(min-width:600px)]:
[@supports(backdrop-filter:blur(0))]:, [data-theme="dark"]:
```

## Common v4 Pitfalls to Avoid

### Don't Use These Deprecated Patterns
- ❌ `@tailwind base; @tailwind components; @tailwind utilities;`
- ❌ `text-opacity-50` → Use `text-white/50` instead
- ❌ `bg-opacity-25` → Use `bg-blue-500/25` instead
- ❌ `border` without color (now uses currentColor, not gray-200)
- ❌ `ring` without explicit width (now 1px, was 3px)
- ❌ `@layer utilities` → Use `@utility` instead
- ❌ JavaScript config for new projects → Use CSS `@theme`

### Modern v4 Alternatives
- ✅ `@import "tailwindcss";`
- ✅ `text-white/50` for semi-transparent text
- ✅ `bg-blue-500/25` for semi-transparent backgrounds
- ✅ `border border-gray-200` for explicit border color
- ✅ `ring-3` for 3px ring width
- ✅ `@utility` for custom utilities
- ✅ `@theme` for configuration

## Performance Best Practices

### Leverage v4's Performance Features
- Use automatic content detection (no manual `content` config needed)
- Prefer container queries over viewport queries for true component responsiveness
- Use CSS variables from `@theme` for dynamic styling
- Leverage the new OKLCH color system for better color consistency

### Efficient Class Usage
```html
<!-- Good: Use size-* for square elements -->
<div class="size-12"></div>

<!-- Instead of: -->
<div class="w-12 h-12"></div>

<!-- Good: Use container queries for component responsiveness -->
<div class="@container">
  <div class="p-4 @lg:p-8">Content</div>
</div>

<!-- Good: Use CSS variables for dynamic values -->
<div class="bg-[var(--theme-primary)] text-[var(--theme-on-primary)]">
  Dynamic theming
</div>
```

## Framework Integration

### Svelte 5
```svelte
<script>
  let { variant = "primary", children } = $props();
  
  const buttonClasses = $derived(() => [
    'px-4 py-2 rounded-md font-medium transition-colors',
    variant === 'primary' 
      ? 'bg-brand text-white hover:bg-brand/90'
      : 'bg-gray-100 text-gray-900 hover:bg-gray-200'
  ].join(' '));
</script>

<button class={buttonClasses}>
  {@render children()}
</button>

<style>
@import "tailwindcss";

@theme {
  --color-brand: oklch(0.5 0.2 240);
}
</style>
```

## Debugging & Development

### Browser DevTools Tips
1. **Inspect CSS Variables**: Check `:root` in DevTools to see all theme variables
2. **Check Cascade Layers**: Use the Layers panel to understand style precedence
3. **Verify OKLCH Support**: Test colors in modern browsers vs fallbacks
4. **Container Query Debugging**: Use the @container panel in DevTools

### Common Debug Commands
```bash
# Check if utilities are being generated
npx tailwindcss --watch --content "./src/**/*.{html,js,jsx,ts,tsx}"

# Verify v4 installation
npm list tailwindcss

# Check for conflicting PostCSS plugins
npm list | grep postcss
```

### VS Code Extensions
- Tailwind CSS IntelliSense (updated for v4)
- PostCSS Language Support

## v3 to v4 Migration Checklist

### Pre-Migration
- [ ] Backup your project
- [ ] Ensure Node.js 20+ is installed
- [ ] Check browser support requirements (Safari 16.4+, Chrome 111+, Firefox 128+)

### Automated Migration
```bash
npx @tailwindcss/upgrade@next
```

### Manual Verification
- [ ] Replace `@tailwind` directives with `@import "tailwindcss"`
- [ ] Move `tailwind.config.js` content to CSS `@theme`
- [ ] Update deprecated utility classes
- [ ] Test container queries functionality
- [ ] Verify custom component styles
- [ ] Check 3D transform support
- [ ] Test mask utilities if used
- [ ] Validate color consistency (OKLCH vs RGB)

### Testing
- [ ] Test in all target browsers
- [ ] Verify responsive design still works
- [ ] Check dark mode functionality
- [ ] Test custom animations
- [ ] Validate accessibility features

## Production-Ready Component Patterns

### Modern Card Component
```html
<div class="@container group">
  <article class="
    bg-white dark:bg-gray-900 
    rounded-xl shadow-sm border border-gray-200 dark:border-gray-800
    overflow-hidden transition-all duration-200
    hover:shadow-md hover:-translate-y-0.5
    @sm:flex @sm:items-center
  ">
    <div class="@sm:flex-shrink-0">
      <img class="h-48 w-full object-cover @sm:h-full @sm:w-48" src="..." alt="...">
    </div>
    <div class="p-6 @sm:p-8">
      <h3 class="text-lg font-semibold text-gray-900 dark:text-white @lg:text-xl">
        Card Title
      </h3>
      <p class="mt-2 text-gray-600 dark:text-gray-300 @lg:text-lg">
        Card description that adapts to container size.
      </p>
    </div>
  </article>
</div>
```

### 3D Interactive Button
```html
<button class="
  relative px-6 py-3 bg-blue-600 text-white font-medium rounded-lg
  transform-3d perspective-1000 
  transition-all duration-200
  hover:rotate-x-12 hover:scale-105 hover:shadow-xl
  active:scale-95 active:rotate-x-6
  focus:outline-none focus:ring-2 focus:ring-blue-500/50
">
  3D Button
</button>
```

Remember: Tailwind CSS v4 is designed for modern browsers and modern development workflows. Embrace the new CSS-first approach and leverage the powerful new features for better performance and developer experience.
