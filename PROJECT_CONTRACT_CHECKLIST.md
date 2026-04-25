## FreshLeaf Mobile <-> FreshLeafApi Contract Checklist

Audit date: 2026-04-25

### Project Focus
- **B2C Marketplace**: Consumers (Mobile App) buy from verified Vendors (Web App).
- **Revenue Model**: Commission-based fees for platform admins.

### Matched

| Area | Mobile endpoint(s) | Backend coverage |
|---|---|---|
| User auth | `/user/auth/register`, `/user/auth/login`, `/user/auth/logout`, `/user/auth/password/verify`, `/user/auth/password/update` | Present in `routes/api.php` under `v1/user/auth/*` |
| Profile | `/user/profile` | Present (`GET`, `PUT`, `PATCH`, `DELETE`) |
| Pricing | `product.activePrice` (USD), `product.activePriceKhr` (KHR) | PHP 8.5 Hooks implemented in `ProductVariant` |
| Discounts | `product.discountPercentage` | Scalable `ProductDiscount` system implemented |
| Exchange Rates | `ExchangeRate::getRate('USD', 'KHR')` | Admin Panel & History tracking implemented |
| AI Assistant | Hybrid Search (Method B) | Local Llama.cpp + Web Search Bridge implemented |

### Mismatch (path mismatch)
... (keep existing mismatches) ...

| Area | Mobile endpoint(s) | Backend route(s) | Impact |
|---|---|---|---|
| PIN | `/user/auth/pin/set`, `/user/auth/pin/update`, `/user/auth/pin/verify`, `/user/auth/pin/reset` | `v1/user/pin/set`, `v1/user/pin/update`, `v1/user/pin/verify`, `v1/user/pin/reset` | PIN flows can fail with 404 unless backend has hidden compatibility routes |

### Missing on backend (referenced in mobile constants/services)

| Area | Mobile endpoint(s) | Seen in mobile usage |
|---|---|---|
| Payment sessions | `/user/wallets/top-up/sessions`, `/user/checkout/sessions`, `/user/payments/sessions/{id}` | Used by `PaymentSessionService` |
| Product extras | `/user/products/categories`, `/user/products/search` | Declared in constants |
| Cart | `/user/cart`, `/user/cart/items/{itemId}`, `/user/cart/apply-coupon`, `/user/cart/remove-coupon` | Declared in constants |
| Orders | `/user/orders`, `/user/orders/{id}`, `/user/orders/{id}/cancel` | Declared in constants |
| Wishlist | `/user/wishlist`, `/user/wishlist/items/{id}` | Declared in constants |
| AI suggestions | `/ai/suggestions` | Declared in constants |

### Recommended resolution order

1. Fix PIN path mismatch first (high user impact, low change size).
2. Decide source of truth for payment sessions:
   - Implement missing API routes/controllers in backend, or
   - Remove/replace mobile usage if feature is not live.
3. Decide if cart/orders/wishlist/categories/search are:
   - Planned but not implemented yet, or
   - Legacy constants to remove.
4. Add a lightweight CI contract check (shared endpoint manifest) to prevent drift.
