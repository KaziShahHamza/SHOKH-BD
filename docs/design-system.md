# SHOKH — Design System Specification (Synced with Figma)

> **Single Source of Truth:** Every token below mirrors [`design/tokens.json`](../design/tokens.json) and maps 1:1 to our **Figma Variables & Text Styles**.
> Whenever a style is updated in Figma, update `design/tokens.json` and this file first so the codebase and Figma stay permanently in sync.

---

## 1. Visual Philosophy
- **Aesthetic:** Clean Monochrome Minimalist (COS / Uniqlo inspired).
- **Hierarchy via Space & Typography:** Generous whitespace, sharp-to-subtle corners (`2px–4px`), `1px` hairline borders (`#E5E5E5`) instead of heavy drop shadows.
- **Product-First:** Neutral `#F7F7F7` image containers let Watches, Glasses, Crocs, T-Shirts, and Hobby Products stand out cleanly.
- **Currency Formatting:** Always use `৳` prefix with `font-variant-numeric: tabular-nums` (e.g., `৳ 1,850`).

---

## 2. Color Tokens (Figma Variable Collection: `Color`)

| Figma Variable | Hex | Usage |
| :--- | :--- | :--- |
| `color/bg/primary` | `#FFFFFF` | Main page canvas |
| `color/bg/surface` | `#F7F7F7` | Product photo containers, admin cards, subtle panels |
| `color/bg/subtle` | `#F1F1F1` | Hover fills, skeletons, muted tags |
| `color/bg/inverse` | `#111111` | Announcement bar, primary buttons, dark footer option |
| `color/text/primary` | `#111111` | Headings, product titles, prices |
| `color/text/secondary` | `#525252` | Body copy, form labels, secondary nav |
| `color/text/muted` | `#8A8A8A` | Placeholders, timestamps, disabled labels |
| `color/text/inverse` | `#FFFFFF` | Text on `#111111` surfaces |
| `color/border/subtle` | `#E5E5E5` | Dividers, card outlines, table rows |
| `color/border/strong` | `#D4D4D4` | Form inputs, hover borders |
| `color/border/focus` | `#111111` | Active focus outlines, selected variant pills |
| `color/accent/primary` | `#111111` | Primary CTA fill |
| `color/accent/primaryHover` | `#2A2A2A` | Primary CTA hover |
| `color/semantic/success` | `#15803D` (bg `#F0FDF4`) | `In Stock`, `Delivered` |
| `color/semantic/warning` | `#B45309` (bg `#FFFBEB`) | `Low Stock`, `Pending`, `Shipped` |
| `color/semantic/danger` | `#B91C1C` (bg `#FEF2F2`) | `Out of Stock`, `Cancelled`, validation errors |
| `color/semantic/info` | `#1D4ED8` (bg `#EFF6FF`) | `Confirmed`, analytics accent |

---

## 3. Typography (Figma Text Styles)
- **Primary Font:** `Inter` (loaded via `next/font/google`).
- **Mono Font:** `JetBrains Mono` (Order IDs like `SHK-2026-00412`, SKUs).

| Figma Style | Size / Line Height | Weight | Tracking | Usage |
| :--- | :--- | :--- | :--- | :--- |
| `Text/Display` | `48px / 1.08` (Mobile `32px / 1.12`) | 500 | `-0.03em` | Hero headlines |
| `Text/H1` | `32px / 1.20` | 500 | `-0.02em` | Page titles |
| `Text/H2` | `24px / 1.25` | 500 | `-0.015em` | Section titles |
| `Text/H3` | `18px / 1.35` | 500 | `-0.01em` | Card / modal headers |
| `Text/Body-Lg` | `16px / 1.60` | 400 | `0em` | Hero subcopy, descriptions |
| `Text/Body` | `14px / 1.60` | 400 | `0em` | Default UI text, prices, forms |
| `Text/Caption` | `12px / 1.40` | 400 | `0.01em` | Helper text, stock labels, metadata |
| `Text/Overline` | `11px / 1.30` | 500 | `0.10em` | Category tags, section eyebrows (UPPERCASE) |

---

## 4. Layout & Grid (Figma Frames)
- **Desktop Frame Width:** `1440px` (Content max-width `1280px`, `12` columns, `24px` gutter, `80px` outer margin).
- **Mobile Frame Width:** `390px` (`4` columns, `16px` gutter, `16px` outer margin).
- **Section Vertical Rhythm:** `80px` desktop / `48px` mobile.

---

## 5. Core Components (Figma Component Sheet)

### Buttons (`radius: 2px` or `4px`, height `44px` default / `36px` sm / `48px` lg)
- **Primary:** Background `#111111`, Text `#FFFFFF`, Hover `#2A2A2A`.
- **Secondary / Outline:** Background `#FFFFFF`, Border `1px solid #111111`, Text `#111111`, Hover bg `#F7F7F7`.
- **Ghost:** Transparent bg, Text `#111111`, Hover bg `#F1F1F1`.
- **Disabled:** `opacity: 0.4`, `cursor: not-allowed`.

### Inputs & Selects (height `44px`, `radius: 2px`)
- Default: `bg: #FFFFFF`, `border: 1px solid #D4D4D4`, `padding: 0 12px`, `font-size: 14px`.
- Focus: `border-color: #111111`, subtle `1px` ring.
- Error: `border-color: #B91C1C`, caption error text below in `#B91C1C`.

### Product Card
- Aspect ratio `4:5` photo container on `#F7F7F7` surface (no drop shadow).
- Top-left optional minimal badge (`NEW`, `SOLD OUT`, or `-15%`).
- Below image (`12px` top gap): Category overline (`11px`, muted), Product Title (`14px`, `#111111`, single-line truncate), Price in `৳` (`14px`, medium, `tabular-nums`, optional muted strikethrough compare-at price).
- Hover: Smooth `220ms` crossfade to secondary product photo + subtle quick-add bar.

### Status Badges (`radius: 2px`, `11px` uppercase medium)
- `In Stock` / `Delivered`: `#15803D` on `#F0FDF4`
- `Low Stock` / `Pending` / `Shipped`: `#B45309` on `#FFFBEB`
- `Out of Stock` / `Cancelled`: `#B91C1C` on `#FEF2F2`
- `Confirmed`: `#1D4ED8` on `#EFF6FF`

---

## 6. Motion & Transitions (Subtle Only)
- **Standard Curve:** `cubic-bezier(0.22, 1, 0.36, 1)`
- **Durations:** Fast `150ms` (hovers, variant pills), Base `220ms` (image crossfade, accordions), Slow `320ms` (Cart/Filter slide-over drawers).
- **Guideline:** Never use bouncy springs, heavy parallax, or scroll-jacking. Respect `prefers-reduced-motion`.
