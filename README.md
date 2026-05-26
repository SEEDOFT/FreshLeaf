# FreshLeaf Organics 🌿

Welcome to **FreshLeaf Organics**, a modern B2C organic vegetable marketplace designed for Cambodia. This repository serves as the **Master Controller** for the entire ecosystem, linking the backend API and the mobile application.

## 🚀 The Ecosystem
- **[FreshLeafApi](./FreshLeafApi)**: A high-performance Laravel 13 backend powered by PHP 8.5. It manages the marketplace logic, multi-vendor profiles, dual-currency pricing (USD/KHR), and a private Hybrid AI Assistant.
- **[fresh_leaf](./fresh_leaf)**: A beautiful Flutter + GetX mobile application for consumers to browse, purchase, and interact with the AI assistant.

---

## 🛠️ Getting Started (New PC Setup)

### 1. Clone the Project
This repository uses **Git Submodules**. To clone everything (including the API and Mobile app) in one command, run:

```bash
git clone --recursive https://github.com/SEEDOFT/FreshLeaf.git
```

*If you already cloned it without the submodules, run:*
```bash
git submodule init
git submodule update
```

### 2. Backend Setup (FreshLeafApi)
```bash
cd FreshLeafApi
composer install
cp .env.example .env          # Update your DB and AI settings
php artisan key:generate
php artisan migrate --seed
php ai/setup-ai.php           # Setup AI providers (Ollama, Zen, Gemini)
```

### 3. Mobile Setup (fresh_leaf)
```bash
cd fresh_leaf
flutter pub get
# Ensure your .env points to the correct API_URL
```

---

## 💎 Tech Stack & Features
- **Backend**: Laravel 13, PHP 8.5 (Property Hooks, Pipe Operator, Asymmetric Visibility).
- **Frontend**: Flutter, GetX State Management, Dio.
- **AI**: Integrated AI support with Ollama, Zen, and Gemini for operational tasks.
- **UI**: Modern Glassmorphism Design with SPA mode enabled.
- **Marketplace**: B2C model with admin commission logic, verified vendor identities, and dedicated Vendor product/order tracking panels with an audit-ready "Stock with Proof" inventory adjustment system.
- **User Engagement**: Fully localized (EN/KM) with digital wallets, AI-assisted shopping, support chat, and a robust User Wishlist.

## 📄 Project Documentation
- **[Backend Docs](./FreshLeafApi/README.md)**: Detailed API endpoints, AI architecture, and project setup.
- **[Mobile Docs](./fresh_leaf/README.md)**: App structure, state management, and build guides.

---
Developed by **SEEDOFT**.
