# System Architecture

## 1. High-Level Architecture

```text
                       ┌─────────────────────────────┐
                       │   Hostinger Domain (DNS)    │
                       └──────────────┬──────────────┘
                                      │
                ┌─────────────────────┴─────────────────────┐
                ▼                                           ▼
 ┌──────────────────────────────┐            ┌──────────────────────────────┐
 │  apps/web (Next.js App Router│  HTTPS/JSON│  apps/api (Express + TS)     │
 │  TypeScript + Tailwind CSS)  ├───────────►│  Deployed on Vercel          │
 │  Deployed on Vercel          │  Cookies   │  Controllers → Services →    │
 └──────────────┬───────────────┘            │  Repositories                │
                │                            └──────┬──────────┬──────────┬─┘
                │                                   │          │          │
     ┌──────────┴──────────┐                        ▼          ▼          ▼
     ▼                     ▼                 ┌───────────┐ ┌────────┐ ┌────────┐
┌──────────┐        ┌─────────────┐          │  MongoDB  │ │Cloudin-│ │ Gmail/ │
│ YouTube  │        │ Cloudinary  │          │   Atlas   │ │  ary   │ │ Resend │
│ Embeds   │        │ CDN Images  │          │ (Future:  │ │ Upload │ │  SMTP  │
└──────────┘        └─────────────┘          │ Supabase) │ │  API   │ └────────┘
                                             └───────────┘ └────────┘
```

## 2. Monorepo Structure

```text
E-Commerce 3/
├── AGENTS.md
├── README.md
├── design/
│   └── tokens.json              # Single source of truth for Figma & Tailwind
├── docs/
│   ├── project-plan.md
│   ├── architecture.md
│   ├── database-schema.md
│   ├── pages-and-features.md
│   ├── design-system.md
│   └── figma-workflow.md
├── packages/
│   └── shared/                  # Shared types, Zod validators, BD divisions/districts
├── apps/
│   ├── web/                     # Next.js (App Router, TS, Tailwind, Framer Motion)
│   └── api/                     # Express (TS, Zod, JWT, Repository Pattern)
└── .github/                     # Future CI/CD workflows & PR templates
```

## 3. Backend Layering & Future PostgreSQL / Supabase Migration

To make a future migration from **MongoDB Atlas** to **PostgreSQL / Supabase** clean and low-risk, `apps/api` enforces a strict 3-layer separation:

```text
HTTP Request
   │
   ▼
1. Route + Middleware (Auth, Role Guard, Zod Validation, Rate Limiter)
   │
   ▼
2. Controller (Parses HTTP req, calls Service, formats HTTP res)
   │
   ▼
3. Service (Pure business logic: pricing, BD shipping fee, stock rules, order lifecycle)
   │
   ▼
4. Repository Interface (IProductRepo, IOrderRepo, IUserRepo, IAnalyticsRepo)
   │
   ├──▶ MongoDB Implementation (MVP — Mongoose / Native Driver)
   └──▶ PostgreSQL / Supabase Implementation (Phase 2 — Prisma / Drizzle / Supabase)
```

### Migration Readiness Rules
- **IDs:** Treat all entity IDs as opaque strings (`string`) across Services, APIs, and Frontend (never leak `ObjectId` instances outside the Mongo repository).
- **Flat Relational-Friendly Documents:** Keep normalized references (`orderId`, `productId`, `variantId`, `userId`) and avoid deeply nested unbounded arrays so MongoDB collections map cleanly to PostgreSQL tables.
- **No Raw DB Queries in Services:** Aggregations (e.g., Admin income/sales charts) live behind `IAnalyticsRepository` methods so swapping MongoDB pipelines for SQL `GROUP BY` queries is isolated.

## 4. Authentication & Security Architecture
- **Tokens:** Short-lived `accessToken` (15m) + `refreshToken` (7d–30d) stored in `HTTP-only`, `Secure`, `SameSite=Lax` cookies.
- **Password Security:** `bcrypt` hashing (cost factor 12).
- **Google OAuth 2.0:** Express verifies Google ID token / OAuth callback, links or creates user account seamlessly, and issues standard HTTP-only session cookies.
- **Forgot Password Flow:** Generates cryptographically random token, stores SHA-256 hash with 30-minute expiry, emails reset link via Gmail SMTP / Resend, invalidates token on use.
- **Role-Based Access Control:** `CUSTOMER` and `ADMIN` roles enforced by Express middleware (`requireAuth`, `requireAdmin`).
- **Hardening:** Helmet headers, strict CORS (frontend origin only), Zod input validation on all routes, rate limiting on `/auth/*` and `/orders` endpoints.

## 5. Media Architecture (Cloudinary + YouTube)
- **Images:**
  - Admin can either upload directly to **Cloudinary** (via signed upload signature from `apps/api`) **or** paste an external image URL.
  - Store `{ url, publicId?, alt, isPrimary, order }` so Cloudinary-hosted assets can be cleaned up on delete while external URLs work identically.
- **Videos:**
  - Admin can paste a **YouTube URL** (standard `youtube.com/watch?v=...`, `youtu.be/...`, or `youtube.com/shorts/...`) or a Cloudinary video URL.
  - Backend extracts and validates the YouTube `videoId`.
  - Frontend uses a **lightweight facade/lite-embed** (shows thumbnail + play button first, loads YouTube iframe only on click) for zero initial page-load overhead.

## 6. Extensible Provider Abstractions (Phase 2 Ready)
- **`IPaymentProvider`:** `cod` implemented in MVP; `bkash`, `nagad`, `sslcommerz` plug in without changing order creation logic.
- **`ICourierProvider`:** Manual tracking number entry in MVP; Steadfast / Pathao / RedX API adapters plug in later.
