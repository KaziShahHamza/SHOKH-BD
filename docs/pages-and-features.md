# Pages & Features Specification (MVP)

## 1. Global Layout Elements
- **Top Announcement Bar (Optional / Subtle):** e.g., *"Nationwide Bangladesh Delivery • Cash on Delivery Available"*.
- **Sticky Minimal Navbar:**
  - Desktop: Brand Wordmark (left), Category Links (`Watches`, `Glasses`, `T-Shirts`, `Gift`) (center), Search Trigger, Account Icon Dropdown, Cart Icon with item count badge (right).
  - **Account Icon Dropdown:**
    - *Logged Out:* `Sign In` (`/login`), `Create Account` (`/signup`), `Track Guest Order →` (`/track-order`).
    - *Logged In:* Buyer Name & BD Phone header, `My Orders` (`/account`), `Saved BD Addresses` (`/account/addresses`), `Profile & Password` (`/account/profile`), `Track an Order` (`/track-order`), `Admin Console` (`/admin`, if `ADMIN`), `Sign Out`.
  - Mobile: Hamburger Menu (left), Brand Wordmark (center), Search + Cart Icon (right). Slide-over navigation drawer with categories and the same Account/Guest Track Order links.
- **Shopping Cart (`/cart`) & Quick Slide-Over Drawer:** Dedicated `/cart` page with Navbar & Footer (plus quick slide-over bag access); displays items, variant tags, quantity +/- controls, item removal, subtotal in `৳`, free-shipping progress indicator, and `Proceed to Checkout` CTA.
- **Minimal Footer:** Brand statement, 4 category links, Track Order link, Support contact (Gmail/WhatsApp), Delivery & Return Policy links.

---

## 2. Storefront Pages

### 2.1 Landing Page (`/`)
1. **Hero Slider:** Full-bleed auto-sliding hero (4 slides with smooth crossfade, `← / →` controls, progress bars, dark gradient scrim for readability, primary CTA).
2. **Category Showcase Grid:** 4-item minimalist grid (`Watches`, `Glasses`, `T-Shirts`, `Gifts`) with clean typography overlays and subtle image scale on hover.
3. **Featured / New Arrivals:** 4-column desktop / 2-column mobile product grid with secondary image crossfade on hover, BDT price (`৳`), and quick-view / quick-add affordance.
4. **Value & Delivery Strip:** 4 minimal icon+text items (*All-Bangladesh Delivery*, *Cash on Delivery*, *Quality Checked*, *Easy Support*).
5. **Curated Spotlight / Video Banner (Optional):** Highlighting a flagship watch/gift item or brand story.

### 2.2 Products / Catalog Page (`/products` & `/products/[category]`)
- **Header:** Category title, product count, active filter chips.
- **Filter & Sort Bar:**
  - Desktop: Clean collapsible left sidebar or top filter bar (Category, Price Range in `৳`, Size/Color/Attribute, In-Stock Only) + Sort dropdown (`Newest`, `Price: Low to High`, `Price: High to Low`).
  - Mobile: Sticky bottom/top `Filter & Sort` button opening a clean slide-over drawer.
- **Product Grid:** 4-column desktop / 2-column mobile with skeleton loading states, pagination / Load More, and clean empty state.

### 2.3 Product Detail Page (`/product/[slug]`)
- **Media Gallery & Lightbox Modal:**
  - Main large slider viewport with **4 thumbnails positioned below** on both Desktop and Mobile, plus left/right slider arrows and active slide dots.
  - **Color-to-Image Sliding:** Clicking a color swatch smoothly slides the main gallery to that variant's image.
  - **Image Lightbox & Zoom Modal:** Clicking the main product image (or zoom trigger icon) opens a full-screen dark lightbox modal with **Zoom In / Zoom Out / Reset** controls, **Prev / Next (`‹` / `›`) arrow buttons**, and bottom thumbnail navigation.
  - Supports **Cloudinary images**, **external image URLs**, and **YouTube video embeds** (shown with a crisp play badge in thumbnails; clicking loads lite-YouTube player inline).
- **Purchase Panel:**
  - Breadcrumb (`Home / Category / Product`), Title, SKU, BDT Price (`৳`) + Compare-at price + `-15%` `#C8A96B` discount badge.
  - Dynamic **Variant Selector** (e.g., Size/Edition pills, Color swatches that slide the gallery).
  - Live **Stock Badge** (`In Stock`, `Only X left`, `Out of Stock`).
  - Quantity selector + `Add to Bag` (`#C8A96B` Brushed Gold) + `Order Now — Cash on Delivery` (primary `#1F1F1F` CTA) + BD Delivery info box.
- **StarTech-Style Full-Width Details Section (Below Product Hero):**
  - Tabbed full-width section (`Specification` · `Description` · `Video` · `Warranty & Delivery`) with a structured key-value Specification table, expanded multi-paragraph editorial Description, and 3 feature highlight cards.
- **Related Products:** 4-item Desktop / 2-item Mobile grid from the store catalog.

### 2.4 Checkout (`/checkout`) & Order Tracking (`/order/[orderNumber]`)
- **Checkout Mode:** Works seamlessly for **Guests** (no login wall) AND **Logged-in Users** (auto-fills saved default address + lets them pick from saved addresses or save a new one).
- **Form Fields:**
  - Full Name, BD Phone Number (`01XXXXXXXXX` validation), Email (optional for guests, used for order confirmation email).
  - **Division** dropdown (8 divisions) → cascades to **District** dropdown (64 districts).
  - Full Delivery Address (house, road, area/thana) + optional Delivery Note.
- **Shipping & Payment Summary:**
  - Auto-calculates **Inside Dhaka** vs. **Outside Dhaka** delivery fee (or `৳0` if free-shipping threshold is met).
  - Payment Method: **Cash on Delivery (COD)** selected by default (disabled "Coming Soon" badges optional or omitted cleanly until Phase 2).
- **Order Confirmation & Tracking (`/order/[orderNumber]`):**
  - Displays Order Number, confirmation summary, BD shipping address, items, total payable on delivery, and a visual status timeline (`Pending → Confirmed → Shipped → Delivered`).
  - Also accessible via `/track-order` using `Order Number + Phone Number` for guests.

---

## 3. Authentication & Customer Account

### 3.1 Auth Pages (`/login`, `/signup`, `/forgot-password`, `/reset-password`)
- Minimal centered card layout (max-width `420px`) with crisp borders.
- **Continue with Google** button at top + divider + Email/Password form.
- Inline Zod validation errors, show/hide password toggle, loading states.
- Forgot Password sends a 30-minute reset link via email; Reset Password sets a new hashed password and redirects to login.

### 3.2 User Account (`/account`)
- **Orders Tab:** List of past orders with status pills, totals in `৳`, and direct link to order tracking timeline.
- **Saved Addresses Tab:** Add/edit/delete BD addresses and set default shipping address.
- **Profile Tab:** Update name, phone, or change password.

---

## 4. Protected Admin Panel (`/admin`)
Accessible only to users with `role === 'ADMIN'`. Clean monochrome dashboard layout with collapsible sidebar (desktop) and drawer menu (mobile).

### 4.1 Dashboard Overview (`/admin`)
- **Date Range Filter:** Today, Last 7 Days, Last 30 Days, This Month, All Time.
- **KPI Summary Cards:**
  - Total Income (`৳` — delivered revenue)
  - Total Sales (total order count)
  - Delivered Orders (count + completion rate)
  - Cancelled & Returned Orders (counts + rates)
- **Analytics Charts:**
  - Revenue & Sales Trend Chart (Line/Area chart over time).
  - Order Status Breakdown Chart (Pending, Confirmed, Shipped, Delivered, Cancelled, Returned).
  - **COD Cash Flow Breakdown:** Cash Collected (`Delivered`) vs. Cash in Transit (`Confirmed + Shipped`) vs. Lost/Returned Value (`Cancelled + Returned`).
  - **Top 5 Best-Selling Products:** Ranked horizontal bar chart by revenue (`৳`) or units sold.
- **Low-Stock Alert Table:** Variants at or below `lowStockThreshold` with quick `Restock` inline action.

### 4.2 Product & Stock Manager (`/admin/products`)
- Searchable, category-filterable product table showing thumbnail, title, category, price (`৳`), total stock badge, status (`ACTIVE` / `DRAFT` / `ARCHIVED`), and quick actions.
- **Create / Edit Product Form:**
  - Basic info (Title, Slug, Category, Description, Base Price, Compare-at Price, Featured toggle, Status).
  - **Media Manager:**
    - Upload image/video directly to **Cloudinary** (drag-and-drop).
    - **Direct URL Input** for external images.
    - **YouTube Video URL Input** (auto-parses video ID and displays live thumbnail preview).
    - Drag/reorder media and set primary image.
  - **Flexible Variants & Stock Management:**
    - Add/remove variants with custom attributes (Size, Color, Strap, etc.), SKU, optional price override, stock quantity, and low-stock threshold.
    - Quick stock adjustment controls (`+` / `-` with reason logged to `stock_movements`).
  - **Category Specs Builder:** Key-value rows (e.g., `Water Resistance: 3ATM`).

### 4.3 Order Manager (`/admin/orders`)
- Filter by status (`Pending`, `Confirmed`, `Shipped`, `Delivered`, `Cancelled`, `Returned`), delivery zone, or search by Order Number / Phone.
- Order detail drawer/page showing full BD address, customer phone (with COD risk-flag warning if flagged), line items, **1-Click Printable COD Packing Slip / Invoice** (`@media print`), and **Status Update Controls** (`Pending → Confirmed → Shipped → Delivered`, or `Cancelled` / `Returned` with automatic stock restoration).
- Optional courier tracking code input.

### 4.4 Customer Directory & COD Risk Flag (`/admin/customers`)
- Aggregated view of registered customers and guest phone numbers: Total Orders, Delivered, Cancelled/Returned, Total Spent (`৳`).
- **Flag COD Risk** toggle + note for repeat-cancellation/return phone numbers (surfaced as a warning badge on new orders).

### 4.5 Store & Delivery Settings (`/admin/settings`)
- Live configuration of **Inside Dhaka fee (`৳ 70`)**, **Outside Dhaka fee (`৳ 130`)**, **Free-shipping threshold (`৳ 3,000`)**, and **Announcement Bar text**.
