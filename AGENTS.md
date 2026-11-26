# AGENTS.md — Persistent Project Memory & Rules

> Read this file first in every new chat or subagent session. It is the single source of truth for how this project is designed, architected, and developed.

## 1. Project Identity
- **Brand Name:** **SHOKH** (meaning *passion / hobby* — styled as clean uppercase `SHOKH` wordmark).
- **What:** Local Bangladesh-based minimalist e-commerce store selling **Watches, Glasses, Crocs, T-Shirts, and Hobby Products**.
- **Design Aesthetic:** Clean Monochrome Minimalist (Uniqlo / COS inspired) — white/off-white surfaces, deep charcoal (`#111111`) primary accent, subtle hairline borders (`#E5E5E5`), semantic colors reserved strictly for stock & order statuses.
- **Locale & Currency:** English UI, Bangladeshi Taka (**BDT / ৳**), nationwide Bangladesh delivery.
- **MVP Payment:** Cash on Delivery (**COD**) only. Phase 2: **bKash, Nagad, SSLCommerz / Card**.
- **Checkout:** Both **Guest Checkout** and **Logged-in Checkout** (saved BD addresses, order history, order tracking).
- **Shipping Rules:** 2-tier flat rate (**Inside Dhaka** vs. **Outside Dhaka**) with Division & District dropdowns + configurable free-shipping threshold.

## 2. Tech Stack & Hosting
- **Monorepo Layout (to be scaffolded after Figma designs are approved):**
  - `apps/web` — Next.js (App Router, TypeScript, Tailwind CSS, Framer Motion / Motion for subtle transitions).
  - `apps/api` — Node.js + Express (TypeScript, Zod validation), deployed on **Vercel** (zero sleep, fast cold starts).
  - `packages/shared` — Shared TypeScript types, Zod schemas, Bangladesh Divisions/Districts list, constants.
- **Database:** **MongoDB Atlas** (free tier) in MVP, with planned future migration to **PostgreSQL / Supabase**.
  - **Mandatory Rule:** Every database operation MUST go through the **Repository Pattern** (`controllers -> services -> repositories`). Services and controllers must NEVER import Mongoose models or MongoDB queries directly.
- **Authentication:** Custom JWT Auth in Express (`HTTP-only` access + refresh cookies, `bcrypt` password hashing, Google OAuth 2.0, Forgot/Reset Password email tokens, role-based `CUSTOMER` / `ADMIN` guards).
- **Email:** Gmail SMTP (Nodemailer) or Resend for Forgot Password and Order Confirmation emails.
- **Media:**
  - Product images & short clips: **Cloudinary** upload **AND** direct URL input.
  - Product videos: **YouTube URL** input embedded cleanly on product pages for bandwidth efficiency.
- **Hosting & Domain:** Both Next.js and Express API deployed on **Vercel**. Custom domain DNS managed on **Hostinger** (Shared Business plan). Business email via **Gmail**.

## 3. Current Project Phase & Strict Gate
- **Current Phase:** **Phase 0 — Planning & UI/UX Design in Figma**.
- **Strict Rule:** Do **NOT** scaffold or build Next.js/Express application pages until the corresponding Figma designs are explicitly **Approved** by the user in `docs/figma-workflow.md`.

## 4. Figma-First UI/UX Workflow (Mandatory Rules)
1. **Desktop + Mobile Always:** Whenever delivering Figma-pasteable code for any page or section, **ALWAYS provide BOTH Desktop (`1440px`) and Mobile (`390px`) views**.
2. **Native SVG Copy-Paste Format:** Deliver Figma designs as **pure `<svg>...</svg>` code** (no `<foreignObject>`) directly in chat AND saved under `design/figma/*.svg` so the user can copy-paste (`Ctrl + V`) straight onto the Figma Web canvas with zero plugins.
2. **Design System Sync:** `design/tokens.json` and `docs/design-system.md` are the single source of truth synced 1:1 with the Figma Design System layer (colors, typography, spacing, radii, borders, motion).
   - If the user modifies any color, font, spacing, or component style in Figma and shares back updated PNGs or Tailwind code, **update `design/tokens.json` and `docs/design-system.md` first**, then update affected pages/components.
3. **Iteration Loop:**
   - Agent generates self-contained Desktop + Mobile section/page code for the user to paste onto the Figma canvas.
   - User previews, prototypes, and edits in Figma if needed.
   - User shares updated PNGs or Tailwind code back.
   - Agent updates specs/code to match and marks the page `Approved` in `docs/figma-workflow.md`.

## 5. Git & Engineering Rules
- **NEVER commit directly to `main`.**
- **Keep active branches minimal (max 1–2):** Use `design/figma-ui` for all Phase 0 Figma designs (do **not** create a separate branch per page). For Phase 1+, use one milestone branch at a time (`feat/*`, `fix/*`, `docs/*`), merge via PR to `main`, and **delete old branches** locally and on GitHub. See [Git Workflow](./docs/git-workflow.md).
- Use **Conventional Commits** (`feat:`, `design:`, `docs:`, `fix:`, `refactor:`, `test:`, `chore:`) and **Git Tags** (`v0.1.0-docs`, `v0.2.0-figma`, `v1.0.0-mvp`) on `main` for milestones.
- Keep animations **subtle and restrained** (`150ms–250ms`, ease-out curves) — never heavy parallax or distracting effects.
- Preserve existing comments and documentation unless explicitly asked to change them.

## 6. Future Skills & Subagents (As Project Grows)
When transitioning to Phase 1 (Implementation), create dedicated workspace skills/subagents for:
- **Code Reviewer Subagent:** Enforces TypeScript strictness, Repository Pattern compliance, security (auth/XSS/CSRF/rate-limiting), and zero direct DB leaks.
- **QA & Testing Subagent:** Runs API integration tests, stock deduction/restoration concurrency tests, and Playwright E2E checkout flows.
- **Design Parity Skill:** Compares implemented Next.js UI against `design/tokens.json` and approved Figma screenshots.

## 7. Quick Reference to Docs
- [Project Plan & Roadmap](./docs/project-plan.md)
- [System Architecture](./docs/architecture.md)
- [Database Schema](./docs/database-schema.md)
- [Pages & Features](./docs/pages-and-features.md)
- [Design System](./docs/design-system.md)
- [Design Tokens JSON](./design/tokens.json)
- [Figma Workflow & Tracker](./docs/figma-workflow.md)
- [Git Workflow, PRs & Tags](./docs/git-workflow.md)
