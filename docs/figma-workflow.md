# Figma Design Workflow & Approval Tracker

## 1. How We Design Before Coding
1. **Desktop + Mobile Mandatory:** Every page or section delivered for Figma MUST include **both Desktop (`1440px`) and Mobile (`390px`)** views.
2. **Design System First:** We start by finalizing the **Design System Sheet** in Figma so colors, typography, buttons, inputs, badges, and cards are locked in `design/tokens.json` and `docs/design-system.md`.
3. **Iteration Loop per Page:**
   - **Step A (Generate):** Agent provides clean, self-contained HTML/Tailwind code for Desktop (`1440px`) and Mobile (`390px`) sections/pages.
   - **Step B (Visualize & Edit in Figma):** User pastes the code onto the Figma canvas, inspects layout/prototypes, and edits directly in Figma if desired.
   - **Step C (Feedback Sync):** User shares back updated PNG screenshots or Tailwind code for any changed sections.
   - **Step D (Sync & Approve):** Agent updates `design/tokens.json` (if tokens changed), updates the page spec, and marks the page **Approved** below.
4. **Build Gate:** Production Next.js pages are built **only after** their Figma designs reach `Approved` status.

---

## 2. Page Design Status Tracker

| # | Deliverable / Page | Route | Desktop (`1440px`) | Mobile (`390px`) | Status | Notes |
| :- | :--- | :--- | :---: | :---: | :--- | :--- |
| 0 | **Design System Sheet** (Colors, Fonts, Buttons, Inputs, Badges, Cards) | — | ✅ | ✅ | `Approved` | Synced 1:1 with `design/tokens.json` (`00-design-system-*.svg`) |
| 1 | **Landing Page** | `/` | ✅ | ✅ | `Approved` | Hero, Bento Categories, Featured, Video Spotlight, Footer (`01-landing-*.svg`) |
| 2 | **Products / Catalog Page** | `/products` | ✅ | ✅ | `Approved` | Category tabs, Filters, Sort, 4-col Desktop / 2-col Mobile grid (`02-products-*.svg`) |
| 3 | **Product Detail Page** | `/product/[slug]` | ✅ | ✅ | `Approved` | Gallery + YouTube embed, Variants, Stock, COD CTA (`03-product-detail-*.svg`) |
| 4 | **Cart Drawer, Checkout & Order Tracking** | `/checkout`, `/order/[id]` | 🟡 | 🟡 | `In Figma` | Guest + Logged-in, BD Divisions/Districts, 2-tier fee, Timeline (`04-checkout-and-cart-*.svg`) |
| 5 | **Auth Pages & User Account** | `/login`, `/signup`, `/forgot-password`, `/account` | ⬜ | ⬜ | `Pending` | Google OAuth, Forgot Password, Saved Addresses, Orders |
| 6 | **Admin Panel** | `/admin`, `/admin/products`, `/admin/orders` | ⬜ | ⬜ | `Pending` | Income/Sales/Delivered/Cancelled charts, Stock & Media URL/YouTube manager |

**Status Legend:** `Pending` → `In Figma` → `Revisions` → `Approved` → `Built`
