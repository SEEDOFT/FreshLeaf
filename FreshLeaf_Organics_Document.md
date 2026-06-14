# FreshLeaf Organics — Comprehensive Project Documentation

## 1. Project Overview
**FreshLeaf Organics** is a high-impact, full-stack e-commerce platform dedicated to connecting consumers with fresh, organic produce directly from local vendors. The platform is designed for reliability, scalability, and ease of use, featuring real-time updates, AI-powered assistance, and multi-language support (English and Khmer).

- **Backend:** Laravel 11 (PHP 8.2+), Filament Admin/Vendor Panels, Reverb WebSockets, MCP (Model Context Protocol).
- **Frontend:** Flutter (Dart), GetX State Management, Pusher/Reverb Real-time integration, Firebase Cloud Messaging.
- **Key Focus:** Organic produce, vendor empowerment, consumer trust, and AI-driven user experiences.

---

## 2. User Roles & Personas

### Consumer (User)
Primary customers who browse, search, and purchase organic produce. They have access to a personalized wallet, order tracking, and an AI Shopping Assistant.

### Vendor
Local farmers and producers who list their inventory, manage stock, fulfill orders, and track earnings. They utilize a dedicated Filament Dashboard for business operations.

### Admin
Platform owners who oversee user management, verify vendors, manage financial settings (commissions, exchange rates), and provide support through real-time chat and AI monitoring.

---

## 3. Technical Architecture

### Backend (Laravel)
- **API:** RESTful API using Sanctum for secure token-based authentication.
- **Admin/Vendor Panels:** Powered by Filament for a robust, low-code management interface.
- **Real-time:** Laravel Reverb for WebSocket communication (chat, notifications, typing indicators).
- **AI Agent (MCP):** Integration of Model Context Protocol for context-aware AI tools that can query business and user data safely.
- **Scheduled Tasks:** Automated vendor payouts, inventory expiry checks, and order cleanup.

### Frontend (Flutter)
- **State Management:** GetX for reactive UI and efficient dependency injection.
- **Networking:** Secure API Client with bearer token authentication and network resilience (offline detection).
- **UI/UX:** Responsive design with support for Light/Dark themes and English/Khmer localization.
- **Notifications:** Firebase Cloud Messaging (FCM) for push notifications with deep-linking support.

---

## 4. Core Functional Modules

### 4.1. Authentication & Onboarding
- **Onboarding:** Animated splash screens and first-launch carousels.
- **Registration/Login:** Phone/Email based authentication for Consumers, Admins, and Vendors.
- **Security:** PIN-based transaction verification and session security.

### 4.2. Product Discovery & Catalog
- **Browsing:** Dynamic category grids and vendor inventory listings.
- **Search:** Text-based search with chips, suggestions, and advanced filtering (province, category).
- **Details:** Rich product views with image galleries, harvest dates, and shelf-life tracking.

### 4.3. Shopping Cart & Checkout
- **Cart Management:** Add/Remove items, quantity validation against stock, and coupon application.
- **Checkout Flow:** Address selection (with GPS reverse geocoding), payment method selection, and order placement.

### 4.4. Order Management
- **Tracking:** Real-time order status updates and historical timeline.
- **Actions:** Cancel eligible orders, confirm receipt of delivery.
- **Documents:** Generation and download of signed PDF invoices.

### 4.5. Financial System (Wallet & Payments)
- **Wallet:** Multi-currency support (USD/KHR), top-up history, and transaction records.
- **Payments:** Support for Cards, ABA, ACLEDA, COD, and Wallet payments.
- **Payouts:** Automated daily vendor payout processing with platform commission handling.

### 4.6. AI & Communication
- **AI Assistant:** Real-time streaming AI chat for consumers (Shopping Assistant), vendors (Inventory/Business help), and admins (Platform monitoring).
- **In-App Chat:** Multi-type conversation support (Support/Direct) with file attachments and typing indicators.
- **Notifications:** Push and database notifications for orders, promotions, and system alerts.

---

## 5. Project Action Items Tracking
The project currently tracks **76 action items** across various categories to ensure full delivery of the specified features.

### Action Item Categories:
1.  **Admin – User Account Management** (FL-01 to FL-05)
2.  **Admin – Product Category Management** (FL-06 to FL-09)
3.  **Admin – Product Listing Management** (FL-10 to FL-12)
4.  **Admin – Order & Revenue Monitoring** (FL-13 to FL-14)
5.  **Admin – Vendor Payout Management** (FL-15 to FL-17)
6.  **Admin – Platform Settings** (FL-18)
7.  **Admin – Support & AI** (FL-19 to FL-20)
8.  **Vendor – Profile & Onboarding** (FL-21 to FL-24)
9.  **Vendor – Product Management** (FL-25 to FL-27)
10. **Vendor – Order Fulfillment** (FL-28 to FL-29)
11. **Vendor – Earnings & Payouts** (FL-30 to FL-33)
12. **User – Authentication** (FL-34 to FL-35)
13. **User – Product Discovery** (FL-36 to FL-39)
14. **User – Cart & Checkout** (FL-40 to FL-42)
15. **User – Delivery Address Management** (FL-43 to FL-47)
16. **User – Payment Method Management** (FL-48 to FL-52)
17. **User – Order Management** (FL-53 to FL-55)
18. **User – Wallet** (FL-56 to FL-58)
19. **User – Support & Notifications** (FL-59 to FL-61)
20. **User – Profile & Settings** (FL-62 to FL-64)
21. **User – Ratings & Reviews** (FL-65 to FL-66)
22. **User – Cart & Order Enhancements** (FL-67 to FL-70)
23. **Vendor – Inventory & Promotions** (FL-71 to FL-72)
24. **Vendor – Support & AI** (FL-73 to FL-74)
25. **Admin – Configuration & Security** (FL-75 to FL-76)

---

*This document serves as the primary technical and functional reference for the FreshLeaf Organics project.*
