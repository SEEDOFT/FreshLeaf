# FreshLeaf — Complete Functionality Overview

> **Full-stack organic fresh produce e-commerce platform**  
> **Backend:** Laravel 11 (Filament Admin/Vendor Panels, Reverb WebSockets, MCP)  
> **Frontend:** Flutter (GetX, Pusher/Reverb, Firebase Cloud Messaging)  
> **Localization:** Khmer (km) + English (en)

---

## Table of Contents

1. [Authentication & Onboarding](#1-authentication--onboarding)
2. [User Profile Management](#2-user-profile-management)
3. [Address Management](#3-address-management)
4. [Product Catalog & Browsing](#4-product-catalog--browsing)
5. [Vendor Inventory](#5-vendor-inventory)
6. [Vendor Profiles](#6-vendor-profiles)
7. [Shopping Cart](#7-shopping-cart)
8. [Order Management](#8-order-management)
9. [Wallet System](#9-wallet-system)
10. [Payments](#10-payments)
11. [Wishlist](#11-wishlist)
12. [Ratings & Reviews](#12-ratings--reviews)
13. [AI Assistant](#13-ai-assistant)
14. [In-App Chat & Messaging](#14-in-app-chat--messaging)
15. [Push Notifications](#15-push-notifications)
16. [Admin Dashboard (Filament)](#16-admin-dashboard-filament)
17. [Vendor Dashboard (Filament)](#17-vendor-dashboard-filament)
18. [Admin Panel Resources](#18-admin-panel-resources)
19. [Vendor Panel Resources](#19-vendor-panel-resources)
20. [Vendor Payouts](#20-vendor-payouts)
21. [Localization](#21-localization)
22. [Security](#22-security)
23. [Scheduled Jobs & Background Tasks](#23-scheduled-jobs--background-tasks)
24. [AI Agent (MCP)](#24-ai-agent-mcp)
25. [Network Resilience](#25-network-resilience)
26. [Permissions](#26-permissions)

---

## 1. Authentication & Onboarding

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Splash screen with animated logo | — | `SplashView` |
| Onboarding carousel (first-launch only) | — | `OnboardingView` with page indicator |
| Login (phone/email + password) | `AuthController@login` | `LoginView` |
| Register new consumer user | `AuthController@register` | `RegisterView` |
| Register admin user (with registration key) | `AuthController@registerForAdmin` | — |
| Register vendor user | Filament Vendor `Auth/Register` page | — |
| Logout (revoke Sanctum tokens) | `AuthController@logout` | App-wide logout |
| Verify current password | `AuthController@verifyPassword` | `SecuritySettingsView` |
| Update password | `AuthController@updatePassword` | `SecuritySettingsView` |
| Set transaction PIN | `UserPinController@setPin` | `ProfilePinView` |
| Update existing PIN | `UserPinController@updatePin` | `ProfilePinView` |
| Reset PIN (without current PIN) | `UserPinController@resetPin` | `ProfilePinView` |
| Verify PIN for order payment confirmation | `UserPinController@verifyPin` | `PinVerificationView` |
| Auth middleware (redirects to login) | `EnsureActiveUserType` middleware + Sanctum | `AuthMiddleware` (GetX redirect) |
| Profile sync on first navigation | — | `ProfileSyncMiddleware` |
| Onboarding redirect middleware | — | `OnboardingMiddleware` |

---

## 2. User Profile Management

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| View profile | `ProfileController@show` | `ProfileView` / `ProfilePersonalDetailsView` |
| Update profile (name, email, phone, avatar) | `ProfileController@update` (PATCH) | `ProfilePersonalDetailsView` |
| Replace profile (full PUT) | `ProfileController@replace` | — |
| Delete / soft-delete account | `ProfileController@destroy` | Account deletion option |
| Avatar upload | Via profile update | Image picker in personal details |
| Update locale preference (km/en) | Via profile update | `SettingsView` |
| Update theme preference (light/dark/system) | Via profile update | `SettingsView` |
| Theme switching (reactive) | `ThemeManager` Livewire component | `AppSettingsController` |
| Notification toggle (enable/disable) | — | `SettingsView` |

---

## 3. Address Management

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List user delivery addresses | `AddressController@index` | `AddressesView` |
| Get single address | `AddressController@show` | `ProfileAddressEditView` |
| Create new address | `AddressController@store` | `ProfileAddressEditView` |
| Update address (PATCH) | `AddressController@update` | `ProfileAddressEditView` |
| Replace address (PUT) | `AddressController@replace` | `ProfileAddressEditView` |
| Delete address | `AddressController@destroy` | `AddressesView` (swipe to delete) |
| Current location via reverse geocode | — | `LocationRepository` (Nominatim OSM) |
| Google Maps link generation from lat/long | `AddressService` | — |

---

## 4. Product Catalog & Browsing

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List active product categories | `ProductCategoryController@index` | `HomeView` category grid |
| Get single category by slug | `ProductCategoryController@show` | `ProductListView` (filtered) |
| List vendor inventories (products) | `ProductController@index` (filterable by province, category, search) | `HomeView` + `ProductListView` |
| Get single product detail | `ProductController@show` | `ProductDetailView` |
| Product image gallery (batch images) | `VendorInventory` model | `ProductDetailHeroImageWidget` |
| Localized product names/descriptions (EN/KM) | `Product` model (localized properties) | Display based on app locale |
| Search products | `ProductController@index` with search param | `SearchView` with text input + chips |
| Search suggestions | — | `SearchView` suggestions |
| Filter by province, category, pick status | `ProductController@index` query params | `HomeView` filter chips |
| Paginated product loading (infinite scroll) | API pagination | `PaginatedListMixin` + `PaginatedListView` |
| "Picked this morning" / new / top-rated products | Category-based filtering | `HomeView` horizontal product scroll |

---

## 5. Vendor Inventory

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Create inventory listing | Filament Vendor `ProductInventoryResource` | — |
| Edit inventory (price, stock, harvest date) | Filament Vendor `ProductInventoryResource` | — |
| View own inventory list | Filament Vendor `ProductInventoryResource` | — |
| Stock adjustments (with proof image) | `InventoryAdjustment` model + `VendorInventory::adjustStock()` | — |
| Active discount management (date-ranged) | `VendorInventoryDiscount` model | — |
| Discount history tracking | `VendorInventoryDiscountHistory` model | — |
| Shelf-life / harvest date tracking | `VendorInventory` model fields | `ProductDetailView` info tiles |
| Inventory change history | `VendorInventoryHistory` model | — |
| Expiring inventory alerts (3 days) | `CheckExpiringInventory` command | — |

---

## 6. Vendor Profiles

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| View vendor profile with active products | `VendorProfileController@show` | `VendorProfileView` |
| Update business profile | Filament Vendor `BusinessProfile` page | — |
| Update financial details | Filament Vendor `FinancialDetails` page | — |
| Upload verification documents | Filament Vendor `VerificationDocs` page | — |
| Serve verification documents securely | `VerificationDocumentController@show` | — |
| Business hours, shop description | `VendorProfile` model | Displayed in `VendorProfileView` |

---

## 7. Shopping Cart

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Get active cart items with total | `CartController@index` | `CartView` / `CartPanelView` |
| Add item to cart | `CartController@store` | `ProductDetailView` add button |
| Update cart item quantity | `CartController@update` | `CartView` quantity selector |
| Remove item from cart | `CartController@destroy` | `CartView` remove action |
| Quantity validation against available stock | `CartService` | UI validation |
| Apply coupon | — | `CartView` coupon field |
| Remove coupon | — | `CartView` coupon removal |
| Checkout (convert cart to order(s)) | `CartController@checkout` | `CheckoutView` |
| Cart change history | `CartHistory` model | — |

---

## 8. Order Management

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List user orders (with filters/sorting) | `OrderController@index` | `OrdersView` (tabs, sort chips) |
| Get single order detail | `OrderController@show` | `OrderDetailView` |
| Cancel pending order | `OrderController@cancel` | `OrderDetailView` cancel action |
| Confirm receipt of delivered order | `OrderController@confirmReceipt` | `OrderDetailView` confirm action |
| Pay order with wallet (PIN required) | `OrderController@pay` + `OrderService@payWithWallet` | `OrderWalletPaymentView` |
| Batch-pay multiple orders | `OrderController@batchPay` + `OrderService@batchPayWithWallet` | Batch pay flow |
| Simulate external payment completion | `OrderController@simulateExternalPayment` | — |
| Get invoice signed URL | `OrderController@getInvoiceUrl` | `OrderDetailView` invoice link |
| Download invoice as PDF | `OrderController@downloadInvoice` + `InvoicePdfService` | Invoice PDF download |
| Auto-generate order number | `Order` model boot | Displayed in order views |
| Order status history timeline | `OrderHistory` model | `OrderDetailView` timeline |
| Automatic cancellation of unpaid orders (5 min) | `CancelUnpaidOrderJob` | — |
| Merge duplicate order items | `MergeDuplicateOrderItems` command | — |
| Vendor receives new order alert | `NewOrderAlertNotification` (broadcast + database) | Vendor Filament panel |

---

## 9. Wallet System

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List user wallets (USD + KHR) | `WalletController@index` | `WalletView` |
| Get single wallet with balance | `WalletController@show` | `WalletView` balance card |
| Get wallet history | `WalletController@history` | `WalletView` transaction list |
| List wallet transactions (filterable) | `WalletTransactionController@index` | `WalletView` |
| Create wallet transaction | `WalletTransactionController@store` | — |
| Update wallet transaction | `WalletTransactionController@update` | — |
| Delete wallet transaction | `WalletTransactionController@destroy` | — |
| Wallet top-up (preset + custom amounts) | — | `WalletTopUpView` |
| Wallet top-up payment (via saved cards) | Payment session creation | `WalletTopUpPaymentView` |
| Seed test money into wallets (CLI) | `SeedWalletMoney` command | — |
| Multi-currency support (USD/KHR) | `Currency` model, `ExchangeRate` model | Dual display throughout app |

---

## 10. Payments

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List saved payment methods | `PaymentMethodController@index` | `ProfilePaymentView` |
| Get single payment method | `PaymentMethodController@show` | `ProfilePaymentView` |
| Add new payment method (card, ABA, ACLEDA, COD, Wallet) | `PaymentMethodController@store` | `ProfilePaymentAddView` |
| Update payment method | `PaymentMethodController@update` | `ProfilePaymentAddView` |
| Delete (soft) payment method | `PaymentMethodController@destroy` | `ProfilePaymentView` |
| List payment method types | `PaymentMethodTypeController@index` | Checkout + Top-up flows |
| Get single payment method type | `PaymentMethodTypeController@show` | — |
| Payment session creation (for external gateways) | Session-based payment flow | `PaymentSessionService` |
| Payment session status polling | Payment status check endpoint | `PaymentSessionService` polling |
| External payment gateway redirect | — | `OrderExternalPaymentView` |
| QR code payment display | — | `PaymentQrView` |
| Exchange rate conversion at payment | `MoneyService::convert()` | `ExchangeRateText` widget |

---

## 11. Wishlist

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Get active wishlist items | `WishlistController@index` | `ProfileWishlistView` |
| Toggle item on/off wishlist | `WishlistController@toggle` | `ProductDetailView` / product cards |
| Optimistic UI updates | — | `WishlistController` (global GetX) |
| Wishlist change history | `WishlistHistory` model | — |

---

## 12. Ratings & Reviews

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Get ratings + average for a vendor inventory | `VendorInventoryRatingController@forVendorInventory` | `ProductDetailView` rating card |
| Create/update rating on a delivered order item | `VendorInventoryRatingController@store` | `RatingBottomSheet` (from OrderDetail) |
| Get authenticated user's all ratings | `VendorInventoryRatingController@userRatings` | User ratings view |
| Rating widget (1-5 stars) | — | `RatingStarsWidget` (shared) |

---

## 13. AI Assistant

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Create/get AI chat session | `AiChatController@createSession` | `AiAssistantView` |
| Send message, dispatch processing job | `AiChatController@storeMessage` + `ProcessAiChatMessageJob` | `AiAssistantComposerWidget` |
| Fetch chat history for a session | `AiChatController@history` | Session history loading |
| Check AI service health | `AiStatusController@check` | `AiAssistantView` status indicator |
| Multi-provider architecture (Gemini / Ollama / Zen) | `GeminiService`, `OllamaService`, `ZenService` | — |
| Web search integration (DuckDuckGo + Wikipedia) | `WebSearchService` | — |
| Streaming AI response (WebSocket chunks) | `AiMessageStarted/Chunk/Completed/Failed` events | `AiAssistantRealtimeService` (Pusher) |
| Context-aware responses (admin/vendor/consumer data) | MCP tool integration in `ProcessAiChatMessageJob` | — |
| Stop AI response generation | Via job cancellation flag | `AiAssistantView` stop button |
| Session management (switch, delete, new) | `AiChatSession` model | `AiAssistantView` session list |
| Local chat message persistence | `AiChatMessage` model | `AiChatStorageService` (GetStorage) |
| Inline product recommendations | AI response product card markup | `AiAssistantProductCardWidget` |
| Livewire AI chat (admin/vendor panels) | `AiAssistantChat` Livewire component | — |

---

## 14. In-App Chat & Messaging

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| List conversations (filterable by type/status) | `ConversationController@index` | `SupportTicketsView` |
| Start or get existing conversation (support/direct) | `ConversationController@store` | `SupportTicketsView` create ticket |
| Get single conversation details | `ConversationController@show` | `SupportChatView` header |
| Get unread message count | `ConversationController@getUnreadCount` | Badge on chat icon |
| Broadcast typing event | `ConversationController@sendTyping` + `ChatTyping` event | `SupportChatView` typing indicator |
| Get messages for a conversation (marks as read) | `MessageController@index` | `SupportChatView` message list |
| Send a message (text + optional file attachment) | `MessageController@store` | `SupportChatComposerWidget` |
| Real-time incoming messages (WebSocket) | `ChatMessageSent` event (broadcast) | `ChatRealtimeService` (Pusher) |
| Real-time typing indicator (WebSocket) | `ChatTyping` event (broadcast) | `ChatRealtimeService` |
| File/image attachment support | File upload handling in message store | `SupportChatAttachmentPreviewWidget` |
| Create support ticket (consumer/vendor to admin) | `ConversationController` support type | `SupportTicketsView` |
| Vendor order update broadcast | `VendorOrderUpdated` event | Vendor Filament panel |
| Chat notification sent to Filament admin/vendor | `ChatNotificationSent` event + `NewChatMessage` notification | — |
| Livewire chat inbox (admin/vendor panels) | `ChatInbox` Livewire component | — |

---

## 15. Push Notifications

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Register/update FCM device token | `DeviceController@store` | `NotificationService` (on startup) |
| Deactivate device token | `DeviceController@destroy` | On logout |
| Send new order notification (to consumer) | `NewOrderNotification` (Database + FCM) | Local notification display |
| Send order status update notification | `OrderStatusUpdatedNotification` (Database + FCM) | Local notification display |
| Send new order alert (to vendor) | `NewOrderAlertNotification` (database + broadcast) | Vendor Filament panel |
| Send promotion/marketing notification | `PromotionNotification` (Database + FCM) | Local notification display |
| Send inventory expiring notification (to vendor) | `InventoryExpiringNotification` (database + mail) | — |
| Base push notification with FCM | `PushNotification` base class | — |
| Custom database channel with Filament integration | `DatabaseChannel` custom channel | — |
| List in-app notifications | `NotificationController@index` | `NotificationsView` |
| Mark single notification as read | `NotificationController@markAsRead` | `NotificationsView` tap |
| Mark all notifications as read | `NotificationController@markAllAsRead` | `NotificationsView` bulk action |
| Deep linking from notification tap | — | `NotificationDetailView` / route navigation |

---

## 16. Admin Dashboard (Filament)

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Financial overview cards (total revenue, commissions) | `AdminFinancialOverview` widget | — |
| Revenue chart (30-day) | `AdminRevenueChart` widget | — |
| Exchange rate monitoring widget | `AdminExchangeRates` widget | — |
| Platform activity stats (users, vendors, orders, products) | `AdminPlatformActivity` widget | — |
| Pending vendor payouts widget | `PendingVendorPayouts` widget | — |
| Livewire dashboard overview | `DashboardOverview` Livewire component | — |

---

## 17. Vendor Dashboard (Filament)

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Financial overview (product count, today's orders, wallet) | `VendorFinancialOverview` widget | — |
| Earnings chart | `VendorEarningsChart` widget | — |
| Exchange rate display | `VendorExchangeRates` widget | — |
| Platform activity (product views, orders) | `VendorPlatformActivity` widget | — |

---

## 18. Admin Panel Resources

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| CRUD Orders | `OrderResource` (List, Create, Edit, View) | — |
| CRUD Products | `ProductResource` (List, Create, Edit, View) | — |
| CRUD Units | `UnitResource` (List, Create, Edit) | — |
| CRUD Wallets | `WalletResource` (List, Edit) | — |
| CRUD Wallet Transactions | `WalletTransactionResource` (List, Create, Edit) | — |
| Manage Exchange Rates | `ExchangeRateResource` (Manage) | — |
| Manage Commission Fees | `CommissionFeeResource` (Manage) | — |
| Application settings | `ApplicationSettings` page | — |
| Admin profile settings | `AdminProfile` page | — |
| Admin security (password, 2FA) | `AdminSecurity` page | — |
| Support chat interface | `SupportChat` page | — |
| AI assistant (admin context) | `AiAssistant` page | — |

---

## 19. Vendor Panel Resources

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| CRUD Product Inventories | `ProductInventoryResource` (List, Create, Edit, View) | — |
| View orders (assigned to vendor) | `OrderResource` (List, View) | — |
| View inventory ratings | `VendorInventoryRatingResource` (List, View) | — |
| View payouts | `PayoutResource` (List, View) | — |
| View wallets | `WalletResource` (List) | — |
| Product catalog browser | `ProductCatalog` page | — |
| Business profile settings | `BusinessProfile` page | — |
| Financial details settings | `FinancialDetails` page | — |
| Verification documents upload | `VerificationDocs` page | — |
| Vendor security (password, 2FA) | `VendorSecurity` page | — |
| Support chat interface | `SupportChat` page | — |
| AI assistant (vendor context) | `AiAssistant` page | — |

---

## 20. Vendor Payouts

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Process payout for delivered order | `VendorPayoutService@processOrder` | — |
| Automated daily payout processing | `ProcessVendorPayouts` command (daily) | — |
| Handle COD vs digital payment type | `VendorPayoutService` | — |
| Commission transfer to admin | `VendorPayoutService` | — |
| Payout status tracking | `Payout` model + `PayoutStatus` model | — |
| Payout methods configuration | `PayoutMethod` model | — |

---

## 21. Localization

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Khmer (km) language support | Translations in `lang/` | `translations_km.dart` |
| English (en) language support | Translations in `lang/` | `translations_en.dart` |
| Locale detection from Accept-Language header | `SetLocaleFromAcceptLanguage` middleware | — |
| Locale from user profile preference | Via `UserProfile` model | `AppSettingsController` |
| GetX translation orchestration | — | `AppTranslations` orchestrator |
| App-wide language switching | — | `SettingsView` language picker |

---

## 22. Security

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Sanctum token-based API authentication | `auth:sanctum` middleware | Bearer token in `ApiClient` |
| Rate-limited auth endpoints | Laravel `RateLimiter` | — |
| Active user middleware (status check) | `EnsureActiveUserType` middleware | — |
| PIN-based transaction verification | `UserPinController` verify flow | `PinVerificationView` |
| Signed routes for invoice download | Laravel signed URLs | — |
| File path traversal protection | `VerificationDocumentController` path check | — |
| Secure token storage | — | `FlutterSecureStorage` |
| Password verification before sensitive actions | `verifyPassword` endpoint | `SecuritySettingsView` |
| Soft deletes for users, addresses, payment methods | `SoftDeletes` trait | — |

---

## 23. Scheduled Jobs & Background Tasks

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Daily automated vendor payout processing | `payouts:process` (daily) | — |
| Daily expiring inventory alerts | `app:check-expiring-inventory` (daily) | — |
| Cancel unpaid orders after 5 minutes | `CancelUnpaidOrderJob` (dispatched with delay) | — |
| Process AI chat messages (queued) | `ProcessAiChatMessageJob` (ai-stream queue) | — |
| Seed test wallet money (CLI utility) | `app:seed-wallet` artisan command | — |
| Merge duplicate order items (CLI utility) | `orders:merge-duplicates` artisan command | — |
| Generate secure keys (CLI utility) | `make:key` artisan command | — |

---

## 24. AI Agent (MCP)

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Business data read-only queries | `BusinessDataServer` (`SafeDatabaseQuery`, `SafeDatabaseSchema`) | — |
| Consumer orders & products lookup | `ConsumerDataServer` (`GetMyOrdersTool`, `GetPublicProductsTool`) | — |
| Vendor orders, stock & payouts lookup | `VendorDataServer` (`GetMyVendorOrdersTool`, `GetMyStockItemsTool`, `GetMyPayoutSummaryTool`) | — |
| MCP over Web (SSE transport) | Routes at `/mcp/business-data`, `/mcp/consumer-data`, `/mcp/vendor-data` | — |

---

## 25. Network Resilience

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Internet connectivity check (socket + DNS) | — | `NetworkService` + `NetworkCheckView` |
| Offline detection and redirection | — | `NetworkCheckController` + route guard |
| Connection recovery handling | — | `NetworkCheckView` retry |

---

## 26. Permissions

| Functionality | Backend (Laravel) | Frontend (Flutter) |
|---|---|---|
| Request notification permission | — | `PermissionService` (startup + on-demand) |
| Request location permission | — | `PermissionService` (for address autofill) |

---

*Generated from codebase analysis — Backend: `FreshLeafApi/`, Frontend: `fresh_leaf/`*
