# Phase 2: Translation Key Consolidation

## Goal
Remove ~125 unused/redundant translation keys across 3 lang files × 2 locales, consolidate auth/exchange-rate keys, fix a blade bug, and sync missing keys.

**Status:** READ-ONLY PLAN (not yet executed)

---

## Phase 1: Safe Key Removals (No Code Changes)

### 1a. `lang/en/shared.php` — Remove unused keys

**auth.login** — remove `remember`, `submit`:
```php
// BEFORE lines 12-13:
            'remember' => 'Remember me',
            'submit' => 'Sign in',
```

**auth.register** — remove `phone`, `password`, `submit`, `country`:
```php
// BEFORE lines 25-26, 29, 38:
            'phone' => 'Phone Number',
            'password' => 'Password',
            'country' => 'Country',
...
            'submit' => 'Register Store',
```

**product** — remove `plural_label`, `revenue`, `slug`, `type`, `is_active`, `is_organic`, `farming_method`, `storage_instructions`, `categorization`, `traceability`, `nutrition_key`, `nutrition_value`, `product_category`, `product_type`, `product_status`, `variant`, `quantity_in_unit`, `adjustment_detail`, `proof`, entire `farming_methods` sub-array (lines 59-60, 63, 68, 74-75, 77, 83, 86, 88, 92-93, 95-97, 100-101, 113-114, 129-133).

Also remove `image` line:
```
// BEFORE line 116:
        'image' => 'Image',
```

Wait — `image` is listed as used in the shared.php? Let me check the agent's report. The agent said `image` is NOT in the unused list for product. Let me keep it.

Actually let me list exact removals for product:

Remove line(s):
- `'plural_label' => 'Products',` (line 59)
- `'revenue' => 'Total Revenue',` (line 60)
- `'slug' => 'Slug',` (line 63)
- `'type' => 'Product Type',` (line 68)
- `'is_active' => 'Active on Store',` (line 74)
- `'is_organic' => 'Organic',` (line 75)
- `'farming_method' => 'Farming Method',` (line 77)
- `'storage_instructions' => 'Storage Instructions',` (line 83)
- `'categorization' => 'Categorization',` (line 86)
- `'traceability' => 'Traceability',` (line 88)
- `'nutrition_key' => 'Fact',` (line 92)
- `'nutrition_value' => 'Value',` (line 93)
- `'product_category' => 'Product Category',` (line 95)
- `'product_type' => 'Product Type',` (line 96)
- `'product_status' => 'Product Status',` (line 97)
- `'variant' => 'Variant',` (line 100)
- `'quantity_in_unit' => 'Quantity in Unit',` (line 101)
- `'adjustment_detail' => 'Adjustment Detail',` (line 113→after other removals)
- `'proof' => 'Proof',` (line 114→after)

Entire `farming_methods` sub-array (lines 129-133):
```php
        'farming_methods' => [
            'certified_organic' => 'Certified Organic',
            'pesticide_free' => 'Pesticide Free',
            'naturally_grown' => 'Naturally Grown',
        ],
```

**order** — remove `paid_description`, `logistics`, `user`, `vendor`, `address`, `delivery_contact_name`, `delivery_contact_phone`, `payment_method`:
```
Remove lines 138, 140, 143, 145, 150, 153-154, 149
```

**payout** — **KEEP all** (9 of 15 keys are actively used in code). The plan to remove `shared.payout.*` was based on stale analysis — the exploration found `admin_notes`, `amount`, `details`, `method`, `paid_on`, `processed_at`, `reference`, `status`, `transaction_ref` are all referenced in PHP files.

**form.fields** — remove `name`, `first_name`, `last_name`, `email`, `phone`, `note`, `password`, `password_confirmation`, `business_name`, `department`, `job_title`, `office_phone`:
```
Remove lines 189-193, 195-198, 204-206
```

**form** — remove entire `placeholders` sub-array, `inline_error`, `required`, `cancel`, `save`:
```
Remove lines 208-213, 214-217
```

**security** — remove entire section (lines 221-229)

**general** — remove entire section (lines 242-244)

### 1b. `lang/km/shared.php` — Same removals as EN

Apply the same structural changes — same keys, same nesting. Only keep the Khmer values that correspond to kept keys.

### 1c. `lang/en/admin.php` — Remove unused keys

**settings.app_settings** — remove `revenue_model`, `revenue_model_desc`, `commission_fee`, `commission_fee_helper`:
```
Remove lines 22-25
```

**auth.login** — remove `remember`, `not_having_account`, `register_here`:
```
Remove line 46, 50-51
```

**auth.register** — remove entire section (lines 53-87)

**resources.category** — remove entire section (lines 90-103)

**resources.security** — remove `success_notification`:
```
Remove line 131: 'success_notification' => 'Password updated successfully.',
```

**resources.product** — remove `add_to_store`, `adjust_stock`, entire `notifications` sub-array, entire `farming_methods` sub-array, entire `category_helpers` sub-array:
```
Remove line 379: 'add_to_store' => 'Add to Store',
Remove line 380: 'adjust_stock' => 'Adjust Stock',
Remove lines 387-390 (notifications sub-array)
Remove lines 391-395 (farming_methods sub-array)
Remove lines 353-360 (category_helpers sub-array)
```

**resources.vendor_inventory** — remove `global_listings`, `image`:
```
Remove line 401: 'global_listings' => 'Global Listings',
Remove line 403: 'image' => 'Image',
```

**support** — remove `user_name`, `send`:
```
Remove line 495: 'user_name' => 'User',
Remove line 499: 'send' => 'Send',
```

### 1d. `lang/km/admin.php` — Same removals as EN

Note: `km/admin.php` has DIFFERENT structure from EN — `category` and `security` sections appear at different positions. Map removals carefully:

- `auth.register` section lines 53-87 (same as EN)
- `auth.login.remember`, `not_having_account`, `register_here`
- `settings.app_settings.revenue_model`, `revenue_model_desc`, `commission_fee`, `commission_fee_helper`
- `resources.security.success_notification` (line 101)
- `resources.category` entire section (lines 103-116)
- `resources.product`: `category_helpers`, `farming_methods`, `notifications`, `add_to_store`, `adjust_stock`
- `resources.vendor_inventory`: `global_listings`, `image`
- `support`: `user_name`, `send`

### 1e. `lang/en/vendor.php` — Remove unused keys

**settings.vendor_security** — remove entire section (lines 29-31):
```php
'vendor_security' => [
    'label' => 'Security',
],
```

**navigation** — remove `exchange_rate` key:
```
Remove line 61: 'exchange_rate' => 'Exchange Rate',
```

### 1f. `lang/km/vendor.php` — Same removals as EN

Remove `settings.vendor_security` (lines 29-31) and `navigation.exchange_rate` (line 61).

---

## Phase 2: Auth Consolidation

### 2a. Update Admin Login PHP (`app/Filament/Admin/Pages/Auth/Login.php`)

Change all `admin.auth.login.*` → `shared.auth.login.*`:

| Line | Current | New |
|------|---------|-----|
| 33 | `__('admin.auth.login.title')` | `__('shared.auth.login.title')` |
| 39 | `__('admin.auth.login.subheading')` | `__('shared.auth.login.subheading')` |
| 49 | `__('admin.auth.login.password')` | `__('shared.auth.login.password')` |
| 62 | `__('admin.auth.login.phone')` | `__('shared.auth.login.phone')` |
| 77 | `__('admin.auth.login.pending')` | `__('shared.auth.login.pending')` |
| 93 | `__('admin.auth.login.failed')` | `__('shared.auth.login.failed')` |

### 2b. Update Admin Login Blade (`resources/views/filament/admin/pages/auth/login.blade.php`)

| Line | Current | New |
|------|---------|-----|
| 86 | `__('admin.auth.login.submit')` | `__('shared.auth.login.submit')` |

### 2c. Remove `admin.auth.*` from both EN and KM admin.php

After confirming all code references use `shared.auth.*`, delete the entire `auth` section from `en/admin.php` (lines 40-87) and `km/admin.php` (lines 40-87).

---

## Phase 3: Widget Exchange Rate Dedup

### 3a. Add exchange rate keys to `shared.php`

Add a `widgets` section to `lang/en/shared.php` and `lang/km/shared.php` with the 6 exchange rate keys:

```php
'widgets' => [
    'usd_to_khr_label' => 'USD → KHR',
    'khr_to_usd_label' => 'KHR → USD',
    'usd_to_khr' => ':rate KHR',
    'khr_to_usd' => ':rate USD',
    'usd_to_khr_desc' => '1 USD in KHR',
    'khr_to_usd_desc' => '1 KHR in USD',
],
```

Insert before the `'lightbox'` section in both files.

For KM, use the same Khmer translations from `km/admin.php` or `km/vendor.php` (they're identical values).

### 3b. Update `AdminStatsOverview.php`

Change lines 77-90:
```php
// FROM:
__('admin.widgets.usd_to_khr_label'),
__('admin.widgets.usd_to_khr', ['rate' => number_format($usdToKhr, 0)]),
__('admin.widgets.usd_to_khr_desc')
__('admin.widgets.khr_to_usd_label'),
__('admin.widgets.khr_to_usd', ['rate' => number_format($khrToUsd, 4)]),
__('admin.widgets.khr_to_usd_desc')

// TO:
__('shared.widgets.usd_to_khr_label'),
__('shared.widgets.usd_to_khr', ['rate' => number_format($usdToKhr, 0)]),
__('shared.widgets.usd_to_khr_desc')
__('shared.widgets.khr_to_usd_label'),
__('shared.widgets.khr_to_usd', ['rate' => number_format($khrToUsd, 4)]),
__('shared.widgets.khr_to_usd_desc')
```

Keep the `platform_fee_label` and `platform_fee_desc` as `admin.widgets.*` — those are admin-specific.

### 3c. Update `VendorStatsOverview.php`

Change lines 105-118 similarly — replace `vendor.widgets.*` with `shared.widgets.*` for the exchange rate keys.

### 3d. Remove exchange rate keys from `admin.php`

Remove these 6 keys from the `en/admin.php` widgets section:
```php
'usd_to_khr_label' => 'USD → KHR',
'khr_to_usd_label' => 'KHR → USD',
'usd_to_khr' => ':rate KHR',
'khr_to_usd' => ':rate USD',
'usd_to_khr_desc' => '1 USD in KHR',
'khr_to_usd_desc' => '1 KHR in USD',
```

Do the same for `km/admin.php`.

### 3e. Remove exchange rate keys from `vendor.php`

Remove the same 6 keys from `en/vendor.php` and `km/vendor.php`.

---

## Phase 4 & 5: Product & Order Keys — No Changes

As determined by exploration, admin/vendor panels use separate namespaces intentionally. Leave `admin.resources.product.*` and `shared.product.*` as-is. Leave `admin.resources.order.*` and `shared.order.*` as-is.

---

## Phase 6: Bug Fix

**File:** `resources/views/filament/vendor/pages/vendor-security.blade.php`

Change line 8:
```php
// FROM:
{{ __('shared.other.save_changes') }}
// TO:
{{ __('shared.profile.save_changes') }}
```

---

## Phase 7: Key Sync

**File:** `lang/en/vendor.php`

Add `exchange_rate_desc` to the `widgets` section (alphabetically after `earnings_chart_label` or after `khr_to_usd_desc`):
```php
'exchange_rate_desc' => 'Current USD to KHR rate',
```

This key already exists in `km/vendor.php` line 77 but is missing from `en/vendor.php`.

---

## Verification

After all edits, run:
1. `vendor/bin/pint --format agent` — fix any code style issues
2. `php artisan route:list --except-vendor` — confirm routes still work
3. Manually visit:
   - Admin login page (auth keys still resolve)
   - Admin dashboard (widget exchange rate stat cards work)
   - Vendor dashboard (widget exchange rate stat cards work)
   - Vendor security page (save_changes button resolves)
4. Open admin + vendor stat overview pages and confirm no `__(key)` fallback strings appear
