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
| 0 | **Design System Sheet** (Colors, Fonts, Buttons, Inputs, Badges, Cards) | — | ✅ | ✅ | `Approved` | Synced 1:1 with `design/tokens.json` — Minimal Luxe (`#F8F9FA`, `#1F1F1F`, `#C8A96B`, `#6E7C7C`), `00-design-system-*.svg`, `00-global-components.svg` (`Gifts` nav, no footer COD boxes) |
| 0b | **Navbar Account Dropdown** (Logged-out + Logged-in + Guest Track Order) | — | ✅ | ✅ | `Approved` | Desktop dropdown + Mobile menu drawer (`00b-navbar-account-dropdown.svg`, `Gifts` nav) |
| 1 | **Landing Page** | `/` | ✅ | ✅ | `Approved` | 4-slide auto-sliding hero, 4-category showcase (`Watches`, `Glasses`, `T-Shirts`, `Gifts`), New Arrivals, 1:1 Global Footer (`01-landing-*.svg`) |
| 2 | **Products / Catalog Page** | `/products` | ✅ | ✅ | `Approved` | Category tabs (`All`, `Watches`, `Glasses`, `T-Shirts`, `Gifts`), Filters, Sort, 4-col Desktop / 2-col Mobile grid (`02-products-*.svg`) |
| 3 | **Product Detail Page** | `/product/[slug]` | ✅ | ✅ | `Approved` | Bottom thumbnails, color-to-image sliding, StarTech-style Specification & Description tabs, YouTube embed, 1:1 Global Footer (`03-product-detail-*.svg`) |
| 3c | **Product Image Lightbox & Zoom Modal** | `/product/[slug]` (modal) | ✅ | ✅ | `Approved` | Click main image to open; Zoom In/Out (`−` / `+` / Reset) + Prev/Next (`‹` / `›`) arrows + thumbnail strip (`03c-product-image-modal.svg`) |
| 4a | **Checkout Page** | `/checkout` | ✅ | ✅ | `Approved` | Guest + Logged-in, BD Divisions/Districts, 2-tier fee, COD summary (`04a-checkout-*.svg`) |
| 4b | **Shopping Cart Page** | `/cart` | ✅ | ✅ | `Approved` | Full page with Navbar, Footer, free-shipping bar, steppers & summary (`04b-cart-*.svg`) |
| 4c | **Order Tracking & Guest Lookup** | `/track-order`, `/order/[id]` | ✅ | ✅ | `Approved` | Order Number + BD Phone lookup card + 4-step COD timeline (`04c-order-tracking-*.svg`) |
| 5a | **Login Page** | `/login` | ✅ | ✅ | `Approved` | Google OAuth + Email/Password (`05a-login-*.svg`) |
| 5b | **Signup Page** | `/signup` | ✅ | ✅ | `Approved` | Google OAuth + Name, BD Phone, Email, Password (`05b-signup-*.svg`) |
| 5c | **Forgot Password Page** | `/forgot-password` | ✅ | ✅ | `Approved` | Email reset link request (`05c-forgot-password-*.svg`) |
| 5d | **Reset Password Page** | `/reset-password` | ✅ | ✅ | `Approved` | New password + confirm form (`05d-reset-password-*.svg`) |
| 6a | **Account — Orders** | `/account` | ✅ | ✅ | `Approved` | Order History & tracking links (`06a-account-orders-*.svg`) |
| 6b | **Account — BD Addresses** | `/account/addresses` | ✅ | ✅ | `Approved` | Saved BD addresses + Add/Edit (`06b-account-addresses-*.svg`) |
| 6c | **Account — Profile** | `/account/profile` | ✅ | ✅ | `Approved` | Personal details & change password (`06c-account-profile-*.svg`) |
| 7a | **Admin — Dashboard Overview** | `/admin` | ✅ | ✅ | `Approved` | Income/Sales/Delivered/Cancelled/Returned KPIs + COD Cash Flow + Top 5 Products (`07a-admin-dashboard-*.svg`) |
| 7b | **Admin — Product & Stock Manager** | `/admin/products` | ✅ | ✅ | `Approved` | Product table, Variants, Stock, Cloudinary + YouTube URLs (`07b-admin-products-*.svg`) |
| 7c | **Admin — Order Manager** | `/admin/orders` | ✅ | ✅ | `Approved` | COD pipeline, Returned status, 1-Click Print COD Slip (`07c-admin-orders-*.svg`) |
| 7d | **Admin — Customers & COD Risk** | `/admin/customers` | ✅ | ✅ | `Approved` | Customer order/return stats & COD Risk Flag (`07d-admin-customers-*.svg`) |
| 7e | **Admin — Store & Delivery Settings** | `/admin/settings` | ✅ | ✅ | `Approved` | Dhaka/Outside fees, free-shipping threshold, announcement bar (`07e-admin-settings-*.svg`) |

**Status Legend:** `Pending` → `In Figma` → `Revisions` → `Approved` → `Built`
