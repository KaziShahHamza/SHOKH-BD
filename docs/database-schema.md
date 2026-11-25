# Database Schema & Data Model

> Designed for **MongoDB Atlas (MVP)** with clean 1:1 relational mapping for future **PostgreSQL / Supabase** migration. All primary keys are exposed as opaque `id: string` at the Service/API boundary.

---

## 1. Collections / Tables Overview

### `users`
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` (`ObjectId` / `UUID`) | Primary Key |
| `name` | `string` | Required |
| `email` | `string` | Unique, indexed, lowercase |
| `phone` | `string?` | Optional, BD format `01XXXXXXXXX` |
| `passwordHash` | `string?` | Nullable for Google-only accounts |
| `googleId` | `string?` | Unique sparse index |
| `role` | `enum('CUSTOMER', 'ADMIN')` | Default `'CUSTOMER'` |
| `isEmailVerified` | `boolean` | Default `false` (`true` for Google OAuth) |
| `createdAt` | `Date` | Indexed |
| `updatedAt` | `Date` | Auto-updated |

### `addresses` (Saved BD Addresses for Logged-In Users)
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `userId` | `string` | FK → `users.id`, indexed |
| `label` | `string` | e.g., `Home`, `Office` |
| `recipientName` | `string` | Required |
| `phone` | `string` | BD phone `01XXXXXXXXX` |
| `division` | `string` | One of 8 BD Divisions |
| `district` | `string` | One of 64 BD Districts |
| `addressLine` | `string` | House, road, area, landmark |
| `isDefault` | `boolean` | Default `false` |

### `products`
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `title` | `string` | Required, text-indexed |
| `slug` | `string` | Unique, URL-friendly |
| `category` | `enum` | `'watches' \| 'glasses' \| 'crocs' \| 'tshirts' \| 'hobby'` (indexed) |
| `description` | `string` | Markdown / plain text |
| `basePrice` | `number` | Integer in BDT (৳) |
| `compareAtPrice` | `number?` | Optional strikethrough MRP in BDT |
| `media` | `ProductMedia[]` | Images (Cloudinary/URL) & Videos (YouTube/Cloudinary URL) |
| `specs` | `Record<string, string>` | Flexible category specs (e.g., `Strap: Leather`, `UV: UV400`, `Fabric: 220 GSM Cotton`) |
| `status` | `enum('DRAFT', 'ACTIVE', 'ARCHIVED')` | Default `'ACTIVE'`, indexed |
| `isFeatured` | `boolean` | Default `false` |
| `totalStock` | `number` | Denormalized sum of active variant stocks for fast filtering |
| `createdAt` | `Date` | Indexed |
| `updatedAt` | `Date` | Auto-updated |

**`ProductMedia` Embedded/Child Structure:**
```ts
type ProductMedia = {
  id: string;
  type: 'IMAGE' | 'YOUTUBE_VIDEO' | 'VIDEO_URL';
  source: 'CLOUDINARY' | 'EXTERNAL_URL' | 'YOUTUBE';
  url: string;
  publicId?: string;     // Present only for Cloudinary uploads
  youtubeId?: string;    // Extracted 11-char ID for YOUTUBE_VIDEO
  alt?: string;
  isPrimary: boolean;
  sortOrder: number;
};
```

### `product_variants` (Flexible Variants & Stock Management)
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `productId` | `string` | FK → `products.id`, indexed |
| `sku` | `string` | Unique SKU (e.g., `TSH-BLK-L`, `WTC-SLV-01`) |
| `attributes` | `Record<string, string>` | Flexible per category (e.g., `{ size: 'L', color: 'Black' }`, `{ strap: 'Steel' }`) |
| `priceOverride` | `number?` | Optional variant-specific price in BDT |
| `stock` | `number` | Integer >= 0 (atomic updates) |
| `lowStockThreshold` | `number` | Default `5` (triggers Admin low-stock alert) |
| `imageUrl` | `string?` | Optional variant-specific thumbnail |
| `isActive` | `boolean` | Default `true` |

### `orders`
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `orderNumber` | `string` | Unique human-readable code (e.g., `SHK-2026-00412`) |
| `userId` | `string?` | Nullable (Guest Checkout supported), indexed |
| `customer` | `object` | `{ name, email?, phone }` |
| `shippingAddress` | `object` | `{ division, district, addressLine, deliveryZone: 'INSIDE_DHAKA' \| 'OUTSIDE_DHAKA', note? }` |
| `items` | `OrderItem[]` | Snapshot of `{ productId, variantId, sku, title, attributes, unitPrice, quantity, imageUrl }` |
| `subtotal` | `number` | BDT integer |
| `deliveryFee` | `number` | BDT integer (e.g., `70` Inside Dhaka, `130` Outside Dhaka, or `0` if free-shipping threshold met) |
| `discount` | `number` | Default `0` (Phase 2 coupons) |
| `total` | `number` | `subtotal + deliveryFee - discount` |
| `paymentMethod` | `enum('COD', 'BKASH', 'NAGAD', 'CARD')` | Default `'COD'` |
| `paymentStatus` | `enum('UNPAID', 'PAID', 'REFUNDED')` | Default `'UNPAID'` (auto `'PAID'` when COD order is `'DELIVERED'`) |
| `orderStatus` | `enum` | `'PENDING' \| 'CONFIRMED' \| 'SHIPPED' \| 'DELIVERED' \| 'CANCELLED'` (indexed) |
| `trackingCode` | `string?` | Optional courier tracking reference |
| `statusHistory` | `Array<{ status, note?, updatedBy?, at: Date }>` | Audit trail for order tracking timeline |
| `createdAt` | `Date` | Indexed (used for Admin income/sales charts) |
| `updatedAt` | `Date` | Auto-updated |

### `stock_movements` (Inventory Audit Log)
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `productId` | `string` | Indexed |
| `variantId` | `string` | Indexed |
| `delta` | `number` | Negative on order deduction, positive on restock or order cancellation |
| `reason` | `enum('ORDER_PLACED', 'ORDER_CANCELLED', 'ADMIN_RESTOCK', 'ADMIN_ADJUSTMENT')` | Required |
| `referenceId` | `string?` | `orderId` or `adminUserId` |
| `createdAt` | `Date` | Indexed |

### `auth_tokens` (Refresh & Password Reset)
| Field | Type | Constraints / Notes |
| :--- | :--- | :--- |
| `id` | `string` | Primary Key |
| `userId` | `string` | FK → `users.id`, indexed |
| `type` | `enum('REFRESH', 'PASSWORD_RESET')` | Indexed |
| `tokenHash` | `string` | SHA-256 hash of raw token |
| `expiresAt` | `Date` | TTL index (auto-purged in MongoDB) |
| `revokedAt` | `Date?` | Nullable |

### `store_settings` (Single-Row Config)
- `insideDhakaFee` (number, default `70`)
- `outsideDhakaFee` (number, default `130`)
- `freeShippingThreshold` (number | null, e.g., `3000`)

---

## 2. Stock Consistency Rules
1. **Order Placement:** Use atomic conditional stock decrement (`stock >= quantity`) inside a transaction (MongoDB session / Postgres transaction) so overselling cannot occur under concurrent checkouts.
2. **Order Cancellation:** Transitioning an order to `CANCELLED` automatically restores variant `stock` and logs an `ORDER_CANCELLED` entry in `stock_movements`.
3. **Denormalized `products.totalStock`:** Recalculated whenever any variant stock changes so catalog pages can filter `In Stock` instantly.

## 3. PostgreSQL / Supabase Mapping Notes
- `products.specs` and `product_variants.attributes` map cleanly to PostgreSQL `JSONB`.
- `orders.items` and `products.media` map to child tables (`order_items`, `product_media`) or `JSONB` snapshots in Postgres.
- All indexes above translate directly to standard B-tree and GIN indexes in PostgreSQL.
