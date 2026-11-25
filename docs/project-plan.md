# SHOKH — Project Plan & Roadmap

## 1. Vision
Build **SHOKH**, a fast, reliable, modern minimalist online store for Bangladesh selling five distinct product categories (**Watches, Glasses, Crocs, T-Shirts, Hobby Products**). Start lean with an MVP optimized for conversion and operational ease (COD + Admin Panel + Stock Management), while keeping the codebase cleanly architected to scale into digital payments, courier APIs, and PostgreSQL/Supabase.

## 2. Target Market & Operational Context
- **Geography:** All 64 districts / 8 divisions of Bangladesh.
- **Currency:** Bangladeshi Taka (`৳` / BDT).
- **Language:** English (MVP).
- **Order Flow:** Guest Checkout + Logged-in Checkout with BD phone validation (`01XXXXXXXXX`) and 2-tier delivery fees (Inside Dhaka vs. Outside Dhaka + configurable free-shipping threshold).
- **Media Bandwidth Strategy:** Cloudinary for optimized WebP/AVIF product photography + YouTube embeds for product showcase videos.

## 3. Phased Delivery Roadmap

### Phase 0: Foundation & UI/UX Design (Current)
- [x] Initialize Git repository with branch protection workflow (`main` + feature branches).
- [x] Author `AGENTS.md`, `docs/*.md`, and synced `design/tokens.json`.
- [ ] Design System Sheet (Desktop + Mobile Figma-pasteable code) → Review & Approve.
- [ ] Storefront Pages in Figma (Desktop + Mobile):
  1. Landing Page (`/`)
  2. Products / Catalog Page (`/products`, `/products/[category]`)
  3. Product Detail Page (`/product/[slug]`)
  4. Cart Drawer, Checkout (`/checkout`), Order Confirmation & Tracking (`/order/[orderNumber]`)
  5. Auth Pages (`/login`, `/signup`, `/forgot-password`, `/reset-password`) & Account (`/account`)
  6. Admin Panel (`/admin`, `/admin/products`, `/admin/orders`)

### Phase 1: MVP Build (After Design Approval)
1. **Monorepo Bootstrap:** `apps/web` (Next.js), `apps/api` (Express), `packages/shared`, Tailwind synced to `design/tokens.json`.
2. **Backend Core:** MongoDB Atlas connection, Repository Pattern interfaces & Mongo implementations, Auth (JWT cookies, Google OAuth, Forgot Password email), Product/Variant CRUD + Stock Management, Cloudinary signature/upload + URL validation, Order placement with atomic stock deduction, Admin Analytics endpoints.
3. **Storefront UI:** Implement all approved pages with subtle transitions, responsive mobile-first layouts, SEO metadata, and YouTube lazy-embed player.
4. **Admin Panel UI:** Protected `/admin` dashboard with Recharts/Chart analytics (Income, Sales, Delivered, Cancelled), Product & Variant Stock Manager, Cloudinary + YouTube URL media inputs, and Order Status pipeline.
5. **Testing & Launch:** End-to-end verification, Vercel deployment for `web` + `api`, Hostinger DNS binding, Gmail SMTP verification.

### Phase 2: Growth & Expansion (Post-MVP)
- **Digital Payments:** bKash, Nagad, and SSLCommerz / Card payments via `PaymentProvider` abstraction.
- **Courier Integrations:** Steadfast / Pathao / RedX API integration for automated parcel booking & live tracking.
- **Database Migration (Optional):** Swap MongoDB repositories to PostgreSQL / Supabase repositories when relational analytics or Supabase features are needed.
- **Growth Features:** Discount coupons, product reviews & ratings, wishlist, Bangla language toggle, SMS order alerts.
- **Agentic Tooling:** Add workspace skills & subagents for automated code review, E2E testing, and design-parity checks.

## 4. Infrastructure & Cost Plan (MVP)
| Component | Service | Plan / Role |
| :--- | :--- | :--- |
| Frontend (`apps/web`) | Vercel | Free / Hobby tier (zero sleep, global CDN) |
| Backend (`apps/api`) | Vercel (Serverless Express) | Free / Hobby tier (zero sleep, always responsive) |
| Database | MongoDB Atlas | Free M0 Cluster (Singapore region `ap-southeast-1` for low BD latency) |
| Images | Cloudinary | Free tier (auto WebP/AVIF, responsive transforms) |
| Videos | YouTube (Unlisted/Public embeds) | Free, zero bandwidth cost |
| Domain DNS | Hostinger | Existing Shared Business plan domain |
| Business Email & Transactional | Gmail / Resend | Gmail for business inbox; Resend or Gmail SMTP for transactional emails |
