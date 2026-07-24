# Feature Catalog

**Project:** Lunora Wear

**Document ID:** LW-FC-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Product Management

**Last Updated:** July 2026

---

# Purpose

This document serves as the master inventory of all product features planned for the Lunora Wear platform.

It does **not** describe implementation details.

Instead, it defines:

* Product modules
* Feature ownership
* Priority
* Release planning
* Dependencies
* Related specification documents

Every feature listed here will have its own detailed specification document.

---

# Priority Legend

| Priority | Meaning                |
| -------- | ---------------------- |
| P0       | Critical for MVP       |
| P1       | Important after launch |
| P2       | Future Enhancement     |
| P3       | Long-term Vision       |

---

# Module Overview

| Module                  | Priority | Status  |
| ----------------------- | -------- | ------- |
| Customer Authentication | P0       | Planned |
| Customer Account        | P0       | Planned |
| Homepage                | P0       | Planned |
| Product Catalog         | P0       | Planned |
| Search                  | P0       | Planned |
| Collections             | P0       | Planned |
| Product Detail          | P0       | Planned |
| Wishlist                | P0       | Planned |
| Shopping Cart           | P0       | Planned |
| Checkout                | P0       | Planned |
| Payments                | P0       | Planned |
| Orders                  | P0       | Planned |
| Shipping                | P0       | Planned |
| Customer Dashboard      | P0       | Planned |
| Reviews                 | P1       | Planned |
| Coupons                 | P0       | Planned |
| Notifications           | P1       | Planned |
| Admin Dashboard         | P0       | Planned |
| Inventory               | P0       | Planned |
| Reports & Analytics     | P1       | Planned |
| CMS                     | P1       | Planned |
| Marketing               | P1       | Planned |
| Loyalty Program         | P2       | Planned |
| AI Recommendation       | P2       | Planned |
| AI Search               | P2       | Planned |
| Marketplace             | P3       | Planned |

---

# Customer Modules

## AUTH-001 Authentication

Purpose

Secure customer authentication and account management.

Includes

* Registration
* Login
* Logout
* Password Reset
* Session Management

Future

* Social Login
* Passkeys
* Multi-Factor Authentication

Specification

`docs/02_Features/AUTHENTICATION.md`

---

## HOME-001 Homepage

Purpose

Provide the primary brand experience.

Includes

* Hero Banner
* Featured Collections
* Best Sellers
* New Arrivals
* Flash Sale
* Newsletter
* Brand Story

Specification

`docs/02_Features/HOMEPAGE.md`

---

## CAT-001 Product Catalog

Purpose

Enable customers to browse products efficiently.

Includes

* Categories
* Collections
* Brands
* Filters
* Sorting
* Pagination

Specification

`docs/02_Features/PRODUCT_CATALOG.md`

---

## PROD-001 Product Details

Includes

* Image Gallery
* Zoom
* Color Variants
* Size Variants
* Size Guide
* Stock Status
* Delivery Information
* Reviews
* Related Products

Specification

`docs/02_Features/PRODUCT_DETAILS.md`

---

## SEARCH-001 Search

Includes

* Instant Search
* Suggestions
* Typo Tolerance (Future)
* Search History
* Popular Searches

Specification

`docs/02_Features/SEARCH.md`

---

## CART-001 Shopping Cart

Includes

* Add Item
* Update Quantity
* Remove Item
* Save for Later (Future)
* Coupon Support

Specification

`docs/02_Features/CART.md`

---

## CHECKOUT-001 Checkout

Includes

* Guest Checkout
* Address Selection
* Shipping Method
* Payment Method
* Order Summary
* Confirmation

Specification

`docs/02_Features/CHECKOUT.md`

---

## ORDER-001 Orders

Includes

* Order Placement
* Tracking
* Invoice
* Cancellation Rules
* Return Request

Specification

`docs/02_Features/ORDERS.md`

---

## ACCOUNT-001 Customer Account

Includes

* Dashboard
* Profile
* Addresses
* Orders
* Wishlist
* Returns
* Notifications

Specification

`docs/02_Features/CUSTOMER_ACCOUNT.md`

---

# Administration Modules

## ADMIN-001 Dashboard

Includes

* Revenue
* Sales
* Orders
* Inventory Alerts
* Customer Overview

Specification

`docs/02_Features/ADMIN_DASHBOARD.md`

---

## PRODUCT-001 Product Management

Includes

* Create Product
* Update Product
* Delete Product
* Variants
* Media
* Pricing

Specification

`docs/02_Features/PRODUCT_MANAGEMENT.md`

---

## INV-001 Inventory

Includes

* Stock Levels
* Stock Adjustment
* Purchase Entries
* Warehouse Support (Future)

Specification

`docs/02_Features/INVENTORY.md`

---

## CUSTOMER-001 Customer Management

Includes

* Customer Profiles
* Order History
* Account Status
* Notes

Specification

`docs/02_Features/CUSTOMER_MANAGEMENT.md`

---

## CMS-001 Content Management

Includes

* Homepage Sections
* Blogs
* Banners
* FAQs
* Policies

Specification

`docs/02_Features/CMS.md`

---

## REPORT-001 Analytics

Includes

* Sales Reports
* Revenue
* Top Products
* Customer Insights
* Coupon Performance

Specification

`docs/02_Features/REPORTS.md`

---

# Platform Services

## PAYMENT-001

Includes

* SSLCommerz
* bKash
* Nagad
* Card Payments

Specification

`docs/02_Features/PAYMENTS.md`

---

## SHIPPING-001

Includes

* Pathao
* RedX
* SteadFast
* Manual Delivery

Specification

`docs/02_Features/SHIPPING.md`

---

## NOTIFY-001

Includes

* Email
* SMS
* Push Notifications (Future)

Specification

`docs/02_Features/NOTIFICATIONS.md`

---

# AI Roadmap

Future AI Modules

* AI Product Recommendation
* AI Outfit Builder
* AI Search Assistant
* AI Size Recommendation
* AI Customer Support

Each AI capability will receive its own Architecture Decision Record (ADR) before implementation.

---

# Dependency Matrix

Authentication

↓

Customer Account

↓

Shopping Cart

↓

Checkout

↓

Payments

↓

Orders

↓

Shipping

↓

Notifications

---

# MVP Feature Set

The following features are mandatory before launch:

* Authentication
* Homepage
* Product Catalog
* Product Details
* Search
* Cart
* Checkout
* Payments
* Orders
* Admin Dashboard
* Inventory
* Shipping

---

# Version History

| Version | Description             |
| ------- | ----------------------- |
| 1.0.0   | Initial Feature Catalog |

---

# Next Document

`docs/02_Features/AUTHENTICATION.md`

This document defines authentication architecture, user flows, business rules, validation rules, permissions, security controls, API requirements, UI requirements, edge cases, and acceptance criteria.
