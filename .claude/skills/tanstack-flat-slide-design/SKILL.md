---
name: tanstack-flat-slide-design
description: Use when designing or styling Slidev slides (or any slide UI) for a TanStack-themed talk. Applies Flat Design philosophy fused with TanStack's actual brand language — Inter typography with black-weight headlines, the teal→cyan signature gradient, per-product brand colors (Start cyan / Router emerald / Query red / Table blue / DB orange / AI pink / Form yellow / Virtual purple / etc.), zero shadows, bold color-block sections, and scale-based hover interactions. Triggers on TanStack Start / Router / Query / Table / DB / AI talks, Slidev presentation styling, "flat design" / "no shadow" / "poster look" requests, or when slides need to feel native to TanStack's brand.
---

# TanStack Flat Slide Design

A design system for Slidev decks that **feel native to TanStack** while staying disciplined under **Flat Design** rules. Use it when authoring slide layouts, component snippets, color choices, or motion for any TanStack-themed presentation.

## When to invoke

- Building or restyling a Slidev deck for a TanStack-related talk (Start / Router / Query / Table / DB / AI / Form / etc.)
- The user asks for "flat design", "poster style", "no shadow", "bold typography", "color block sections"
- A deck needs to look like it belongs to the TanStack ecosystem, not generic Slidev

If the talk is about a single TanStack product, pick that product's brand color as the deck's primary (see **Product Palette** below). The default below assumes **Start** (teal→cyan gradient).

---

## 1. Design Philosophy

**Flat. Bold. Print-poster.** No drop shadows, no bevels, no realistic gradients, no textures. Hierarchy comes from **size, color, and weight** — never from depth.

**Six core principles**

1. **Zero artificial depth.** Z-axis does not exist. Layering happens through flat color blocks.
2. **Color as structure.** Sections are defined by background color, not by lines or shadows.
3. **Typography as interface.** Inter at weight 900 with tight tracking carries hierarchy.
4. **Geometric purity.** Rectangles, circles, squares only. Consistent rounding.
5. **Snappy feedback.** Hover = scale + color shift, never shadow.
6. **Signature gradient.** The teal→cyan wordmark gradient is the one allowed expressive flourish — used sparingly for the brand-wordmark moment only.

**The Bold Factor (TanStack edition)**

- Treat each section like a flat **poster panel** with confident color blocking.
- The hero headline mimics the marketing site: massive Inter Black (`text-8xl`/`text-9xl`, `font-black`, `tracking-tighter`) with the product-brand word using the gradient text trick.
- Per-product brand colors get used as **section accents** — not as gradients, not as background washes.

---

## 2. Design Tokens (verified from tanstack.com/start/latest)

### Colors — base

| Token | Value | Tailwind | Use |
|---|---|---|---|
| `background` | `#FDFDFD` | `bg-[#FDFDFD]` | Slide canvas. Off-white, NOT pure `#FFFFFF` |
| `foreground` | `#171717` | `text-neutral-900` | Body and headline text |
| `muted` | `#F3F4F6` | `bg-gray-100` | Alt section background, code block frame |
| `muted-foreground` | `#6B7280` | `text-gray-500` | Sub-labels, captions |
| `border` | `#E5E7EB` | `border-gray-200` | Use sparingly — only inputs / dividers |

### Colors — Product Palette (each TanStack lib's brand color at 500-level)

Pick **one** as the deck's primary based on the topic. Each value is the actual brand color from the TanStack site.

| Product | Hex | Tailwind 500 | Gradient pair |
|---|---|---|---|
| **Start** | `#06B6D4` | `cyan-500` | `from-teal-500 to-cyan-500` |
| **Router** | `#10B981` | `emerald-500` | `from-emerald-500 to-green-500` |
| **Query** | `#EF4444` | `red-500` | `from-red-500 to-rose-500` |
| **Table** | `#3B82F6` | `blue-500` | `from-blue-500 to-indigo-500` |
| **DB** | `#F97316` | `orange-500` | `from-orange-500 to-amber-500` |
| **AI** | `#EC4899` | `pink-500` | `from-pink-500 to-fuchsia-500` |
| **Form** | `#EAB308` | `yellow-500` | `from-yellow-500 to-amber-500` |
| **Virtual** | `#A855F7` | `purple-500` | `from-purple-500 to-violet-500` |
| **Pacer** | `#84CC16` | `lime-500` | `from-lime-500 to-green-500` |
| **Hotkeys** | `#F43F5E` | `rose-500` | `from-rose-500 to-pink-500` |
| **Devtools** | `#000000` | `black` | (none — solid black) |
| **Store** | `#784B30` | `[#784B30]` | (none — solid brown) |

When picked, that color becomes `--primary`, used for: buttons, link accents, the wordmark gradient (with its pair), and one "hero color block" section.

### Typography

**Family: `Inter`** — primary, with `JetBrains Mono` for code.

| Role | Size | Weight | Tracking | Tailwind |
|---|---|---|---|---|
| Hero wordmark | `text-8xl` / `96px` | `font-black` (900) | `tracking-tighter` (`-0.05em`) | `text-8xl font-black tracking-tighter` |
| H1 slide title | `text-5xl` / `48px` | `font-extrabold` (800) | `tracking-tight` | `text-5xl font-extrabold tracking-tight` |
| H2 section | `text-3xl` / `30px` | `font-bold` (700) | `tracking-tight` | `text-3xl font-bold tracking-tight` |
| Body | `text-base` / `16px` | `font-normal` (400) | normal | `text-base` |
| Sub-label / kicker | `text-xs` / `12px` | `font-semibold` (600) | `tracking-widest uppercase` | `text-xs font-semibold tracking-widest uppercase` |
| Code | `text-sm` | `font-normal` | normal | `font-mono text-sm` |

**The wordmark recipe** (signature element, use once per deck — title slide only):

```html
<h1 class="text-8xl font-black tracking-tighter text-neutral-900">
  TANSTACK
  <span class="inline-block text-transparent bg-clip-text bg-gradient-to-r from-teal-500 to-cyan-500">
    START
  </span>
</h1>
```

Swap the gradient pair for any other product (see palette table above).

### Radius, borders, shadows

- **Radius**: `rounded-lg` (8px) — confirmed from TanStack buttons. Use consistently. `rounded-full` only for tags/badges.
- **Borders**: default `0`. Inputs/active tabs use `border-2`. Outline buttons use `border-4` for bold expression.
- **Shadows**: **`shadow-none` everywhere.** Non-negotiable.
- **Blur**: none. No backdrop blur.
- **Gradients**: forbidden on buttons/cards. Allowed only on:
  1. The brand wordmark (background-clip text trick).
  2. Subtle directional background decoration (e.g., `from-gray-50 to-transparent`) — never colorful.

---

## 3. Component Recipes (Slidev-ready)

All examples use Tailwind/UnoCSS class strings that Slidev supports out of the box. Drop into `slides.md` directly.

### 3.1 Title slide (hero)

```markdown
---
layout: cover
background: '#FDFDFD'
class: 'text-neutral-900'
---

<div class="flex flex-col items-center justify-center h-full gap-8">
  <h1 class="text-8xl font-black tracking-tighter leading-none">
    TANSTACK
    <span class="inline-block text-transparent bg-clip-text bg-gradient-to-r from-teal-500 to-cyan-500">
      START
    </span>
  </h1>
  <p class="text-2xl font-medium text-neutral-700 max-w-2xl text-center">
    クライアントファーストという選択
  </p>
  <div class="mt-12 px-6 py-3 bg-cyan-500 text-white rounded-lg font-medium text-base">
    Get Started
  </div>
</div>
```

### 3.2 Section divider (poster panel)

A full-bleed color block panel. Use as a transition between major sections.

```markdown
---
layout: section
class: 'bg-cyan-500 text-white'
---

# <span class="text-7xl font-black tracking-tighter">01. Client-first とは</span>

<p class="mt-6 text-xl font-medium opacity-90">Server-first 全盛の時代へのカウンター</p>
```

Rotate background colors across sections (`bg-cyan-500` → `bg-neutral-900` → `bg-amber-500`) to give the deck a magazine feel.

### 3.3 Content slide with kicker

```markdown
---
layout: default
---

<p class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">01 / Philosophy</p>

# <span class="text-5xl font-extrabold tracking-tight">Router が起点になる</span>

<p class="text-lg text-neutral-600 mt-4 max-w-3xl">
  TanStack Start は Router をクライアント側に据え、SSR は「ルートごとに選ぶオプション」として扱う。
</p>
```

### 3.4 Cards — color-block style

No border, no shadow, solid pastel background. Hover scales by 1.02.

```html
<div class="grid grid-cols-3 gap-6 mt-8">
  <div class="bg-cyan-50 p-8 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-cyan-100">
    <div class="text-xs font-semibold tracking-widest uppercase text-cyan-600 mb-3">Router</div>
    <h3 class="text-xl font-bold text-neutral-900">Type-safe routing</h3>
    <p class="text-sm text-neutral-600 mt-2">params / search / loader まで貫通する型</p>
  </div>
  <div class="bg-amber-50 p-8 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-amber-100">
    <div class="text-xs font-semibold tracking-widest uppercase text-amber-600 mb-3">Runtime</div>
    <h3 class="text-xl font-bold text-neutral-900">Minimal layer</h3>
    <p class="text-sm text-neutral-600 mt-2">React の上に薄いランタイム</p>
  </div>
  <div class="bg-emerald-50 p-8 rounded-lg transition-all duration-200 hover:scale-[1.02] hover:bg-emerald-100">
    <div class="text-xs font-semibold tracking-widest uppercase text-emerald-600 mb-3">DX</div>
    <h3 class="text-xl font-bold text-neutral-900">Instant HMR</h3>
    <p class="text-sm text-neutral-600 mt-2">Vite ベースで開発が速い</p>
  </div>
</div>
```

### 3.5 Buttons

```html
<!-- Primary -->
<a class="inline-flex px-6 py-3 bg-cyan-500 text-white rounded-lg font-medium text-base transition-all duration-200 hover:bg-cyan-600 hover:scale-105">
  Get Started
</a>

<!-- Outline (bold border) -->
<a class="inline-flex px-6 py-3 border-4 border-cyan-500 text-cyan-500 rounded-lg font-semibold text-base transition-all duration-200 hover:bg-cyan-500 hover:text-white">
  Read the docs
</a>

<!-- Secondary -->
<a class="inline-flex px-6 py-3 bg-gray-100 text-neutral-900 rounded-lg font-medium text-base transition-all duration-200 hover:bg-gray-200 hover:scale-105">
  Learn more
</a>
```

### 3.6 Stat block (TanStack-style metrics row)

```html
<div class="grid grid-cols-4 gap-8 mt-12 border-t-2 border-neutral-200 pt-8">
  <div>
    <div class="text-4xl font-black tracking-tight text-cyan-500">437M</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">NPM Downloads</div>
  </div>
  <div>
    <div class="text-4xl font-black tracking-tight text-emerald-500">14k</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">GitHub Stars</div>
  </div>
  <div>
    <div class="text-4xl font-black tracking-tight text-amber-500">719</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">Contributors</div>
  </div>
  <div>
    <div class="text-4xl font-black tracking-tight text-purple-500">49k</div>
    <div class="text-xs font-semibold tracking-widest uppercase text-neutral-500 mt-2">Dependents</div>
  </div>
</div>
```

Each stat number uses a DIFFERENT brand color — that's the "multi-color stat numbers" bold move.

### 3.7 Code block — flat treatment

Slidev defaults are fine; just wrap in a muted card to keep flatness.

````markdown
```tsx {2,5-9|all}{lines:true}
export const Route = createFileRoute('/canvas')({
  ssr: false,                              // ← client-only route
  loader: async () => {
    const saved = localStorage.getItem('canvas-state')
    return {
      Tools: await getDrawingTools({
        data: { savedState: saved },
      }),
    }
  },
  component: CanvasPage,
})
```
````

Wrap in `<div class="bg-gray-50 p-6 rounded-lg">…</div>` if you want a flat frame.

### 3.8 Section background — alternating panel pattern

Recommend rotating these `class:` values on `layout: default` slides for rhythm:

| Order | Background | Foreground | Use |
|---|---|---|---|
| 1 | `bg-white text-neutral-900` | white panel | Regular content |
| 2 | `bg-gray-100 text-neutral-900` | gray panel | Secondary content / quotes |
| 3 | `bg-cyan-500 text-white` | brand color block | Section opener / statement |
| 4 | `bg-neutral-900 text-white` | dark panel | Code-heavy or dramatic claim |
| 5 | `bg-amber-500 text-neutral-900` | accent color block | CTA / closing |

Use this as the deck's "rhythm map" so adjacent slides never share the same panel color.

### 3.9 Background decoration (poster shapes)

Subtle, low-opacity geometric shapes positioned absolutely. Use on title and section slides.

```html
<div class="absolute top-20 right-20 w-64 h-64 rounded-full bg-cyan-500/5"></div>
<div class="absolute bottom-10 left-10 w-32 h-32 rounded-lg bg-amber-500/10 rotate-12"></div>
```

Opacity must stay at `/5` to `/10`. Never higher — they must whisper, not shout.

---

## 4. Motion

- Default transition: `transition-all duration-200`
- Larger transformations: `duration-300`
- **Hover patterns** (snappy, immediate):
  - Buttons: `hover:scale-105 hover:bg-cyan-600`
  - Cards: `hover:scale-[1.02] hover:bg-cyan-100`
  - Outline buttons: `hover:bg-cyan-500 hover:text-white` (fill effect)
  - Icons in cards: `group-hover:scale-110`
- Slidev `v-click` transitions: keep at default `slide-left` or `fade`. Never `flip` or anything that implies depth.

---

## 5. Iconography

- Library: **`lucide` Slidev shortcut** (`<carbon:... />` / `<mdi:... />` via UnoCSS preset-icons, or use lucide-react in components).
- Slidev syntax for icons inline:
  ```html
  <carbon:rocket class="text-cyan-500 inline w-6 h-6" />
  <mdi:lightning-bolt class="text-amber-500 w-8 h-8" />
  ```
- Stroke: `2px` to `2.5px` for emphasis.
- Treatment in cards: wrap in a solid white circle with the brand-color icon inside, `h-14 w-14`, no shadow.

---

## 6. Accessibility

- **Focus**: high-contrast solid outline — `focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-cyan-500`.
- **Contrast**: White on `cyan-500` and `emerald-500` passes WCAG AA. Avoid white text on `yellow-500` or `amber-500` — use `text-neutral-900` instead.
- **Click animations**: respect `prefers-reduced-motion` if exporting to web. Slidev handles this for built-in transitions.

---

## 7. Deck-level Slidev frontmatter (canonical)

Drop this at the top of `slides.md`:

```yaml
---
title: 'TanStack Start — クライアントファーストという選択'
theme: default
layout: cover
background: '#FDFDFD'
class: text-neutral-900
drawings:
  persist: false
transition: slide-left
mdc: true
highlighter: shiki
fonts:
  sans: 'Inter'
  mono: 'JetBrains Mono'
  weights: '400,500,600,700,800,900'
colorSchema: 'light'
---
```

The `weights: '400,500,600,700,800,900'` line is mandatory — without it, `font-black` (900) silently falls back and the wordmark looks weak.

Add this `<style>` block on the first slide for tight reset:

```html
<style>
  .slidev-layout {
    padding: 3rem 4rem;
    background: #FDFDFD;
    color: #171717;
  }
  .slidev-layout h1, .slidev-layout h2, .slidev-layout h3 { letter-spacing: -0.02em; }
  .slidev-layout code {
    background: #F3F4F6;
    padding: 0.15rem 0.4rem;
    border-radius: 0.375rem;
    font-size: 0.9em;
  }
</style>
```

---

## 8. Anti-patterns

| Don't | Why |
|---|---|
| `shadow-md`, `shadow-lg`, `drop-shadow-*` | Breaks flatness. Always `shadow-none`. |
| `bg-gradient-to-r` on buttons or cards | Gradients are reserved for the brand wordmark only. |
| Pure `#FFFFFF` background | Use `#FDFDFD` — what the TanStack site actually uses. |
| `font-bold` for the hero wordmark | Must be `font-black` (900) with `tracking-tighter`. |
| `border-2` borders everywhere | Flat design uses color blocks, not lines. Reserve `border-2`/`border-4` for inputs and outline buttons. |
| Material-style elevated cards | Use solid color blocks instead. |
| Translucent backdrop-blur surfaces | No glass-morphism. Flat means flat. |
| Pastel-everything palette | The deck must have at least one full-bleed brand color section. |
| Mixed font families | Inter only. Mono only inside `<code>` / fenced blocks. |
| Reusing the same panel color on adjacent slides | Rotate per section 3.8 to give the deck rhythm. |

---

## 9. Decision tree — which product color to use

```
Is the talk about a single TanStack product?
├─ YES → use that product's brand color as primary (Section 2 palette)
│         The wordmark gradient uses the listed gradient-pair.
└─ NO (multi-product / ecosystem talk)
   └─ primary = TanStack Start cyan
   └─ use each product's brand color as a SECTION accent
      (rotate Router → Query → Table → … per slide group)
```

---

## 10. Quick checklist before shipping a slide

- [ ] No `shadow-*` classes anywhere
- [ ] Background is `#FDFDFD` or a solid brand block
- [ ] At least one slide uses `bg-{brand}-500 text-white` full bleed
- [ ] Hero uses `font-black tracking-tighter` with the gradient wordmark trick
- [ ] All cards have `transition-all duration-200 hover:scale-[1.02]`
- [ ] Fonts loaded: Inter (incl. 900) + JetBrains Mono
- [ ] Per-section colors rotate (no two adjacent slides share the same panel color)
- [ ] Stats / metrics use **multiple brand colors** (not all the same)
- [ ] At least one decorative absolute-positioned shape on the title slide
- [ ] No gradients except the brand wordmark and (optionally) one neutral background fade

---

## References

See `references/` for product-specific palette swatches and longer component snippets if added later.
