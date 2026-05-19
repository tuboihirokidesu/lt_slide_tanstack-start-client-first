# TanStack Product Brand Palette (verified)

Sampled from `tanstack.com` sidebar product indicators (2026-05-19).
All values are the exact `oklch()` reported by the site, with the closest Tailwind v3 token and a hex approximation for convenience.

| Product | Status badge | oklch (source) | Tailwind | Hex (approx) | Gradient pair (suggestion) |
|---|---|---|---|---|---|
| **Start** | RC | `oklch(0.715 0.143 215.221)` | `cyan-500` | `#06B6D4` | `from-teal-500 to-cyan-500` |
| **Router** | — | `oklch(0.696 0.17 162.48)` | `emerald-500` | `#10B981` | `from-emerald-500 to-green-500` |
| **Query** | — | `oklch(0.637 0.237 25.331)` | `red-500` | `#EF4444` | `from-red-500 to-rose-500` |
| **Table** | — | `oklch(0.623 0.214 259.815)` | `blue-500` | `#3B82F6` | `from-blue-500 to-indigo-500` |
| **DB** | BETA | `oklch(0.705 0.213 47.604)` | `orange-500` | `#F97316` | `from-orange-500 to-amber-500` |
| **AI** | ALPHA | `oklch(0.656 0.241 354.308)` | `pink-500` | `#EC4899` | `from-pink-500 to-fuchsia-500` |
| **Form** | NEW | `oklch(0.795 0.184 86.047)` | `yellow-500` | `#EAB308` | `from-yellow-500 to-amber-500` |
| **Virtual** | — | `oklch(0.627 0.265 303.9)` | `purple-500` | `#A855F7` | `from-purple-500 to-violet-500` |
| **Pacer** | BETA | `oklch(0.648 0.2 131.684)` | `lime-500` | `#84CC16` | `from-lime-500 to-green-500` |
| **Hotkeys** | ALPHA | `oklch(0.645 0.246 16.439)` | `rose-500` | `#F43F5E` | `from-rose-500 to-pink-500` |
| **Store** | ALPHA | `rgb(120, 75, 48)` | `[#784B30]` | `#784B30` | — |
| **Devtools** | ALPHA | `rgb(0, 0, 0)` | `black` | `#000000` | — |
| **CLI** | ALPHA | `oklch(0.585 0.233 277.117)` | `indigo-500` | `#6366F1` | `from-indigo-500 to-violet-500` |
| **Intent** | ALPHA | `oklch(0.685 0.169 237.323)` | `sky-500` | `#0EA5E9` | `from-sky-500 to-cyan-500` |

## Verified site tokens

- **Body bg**: `rgb(253, 253, 253)` → `#FDFDFD`
- **Body text**: `rgb(23, 23, 23)` → `#171717` (neutral-900)
- **Body font**: `Inter, ui-sans-serif, system-ui, sans-serif, ...`
- **Hero "TANSTACK" wordmark**: Inter 900, `96px`, `letter-spacing: -4.8px` (= `-0.05em`)
- **Hero "START" wordmark**: same font/size, with `bg-clip-text` + `from-teal-500 to-cyan-500` gradient
- **Get Started button**: `bg-cyan-500`, `text-white`, `font-medium`, `rounded-lg` (8px), padding `8px 16px`
