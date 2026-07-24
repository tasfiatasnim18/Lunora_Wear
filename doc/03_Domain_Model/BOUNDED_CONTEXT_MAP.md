# Repository Path

`docs/03_Domain_Model/BOUNDED_CONTEXT_MAP.md`

---

# Bounded Context Map

**Project:** Lunora Wear

**Document ID:** LW-BCM-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Solution Architecture

**Related Documents**

* `docs/00_Project_Charter/PROJECT_CHARTER.md`
* `docs/00_Architecture/ADR-000_FOUNDATIONAL_ARCHITECTURE_DECISIONS.md`
* `docs/01_Product_Management/PRODUCT_REQUIREMENTS_DOCUMENT.md`
* `docs/03_Domain_Model/DOMAIN_MODEL.md`

---

# 1. Purpose

The Bounded Context Map defines how the business domains of the Lunora Wear platform are divided into independent responsibilities.

Its objectives are to:

* Prevent tight coupling.
* Define ownership boundaries.
* Support modular development.
* Simplify future migration to microservices.
* Improve maintainability.

A bounded context owns its business rules, data, APIs, and domain events.

---

# 2. Context Overview

The platform consists of the following bounded contexts:

| Context       | Responsibility                               |
| ------------- | -------------------------------------------- |
| Identity      | Authentication, authorization, user identity |
| Customer      | Profiles, addresses, preferences             |
| Catalog       | Products, categories, collections, brands    |
| Inventory     | Stock management and availability            |
| Pricing       | Product pricing, discounts, promotions       |
| Shopping      | Cart and wishlist                            |
| Orders        | Order lifecycle                              |
| Payments      | Payment processing                           |
| Shipping      | Shipment lifecycle                           |
| Content       | Homepage, banners, CMS                       |
| Notifications | Email, SMS, push notifications               |
| Analytics     | Reporting and business insights              |

---

# 3. Context Relationships

```text
Identity
    │
    ▼
Customer
    │
    ▼
Shopping
    │
    ▼
Orders
   ├────────► Payments
   ├────────► Shipping
   └────────► Notifications

Catalog
   │
   ├────────► Inventory
   └────────► Pricing

Analytics
▲
│
All Contexts publish business events
```

---

# 4. Context Responsibilities

## Identity Context

Owns:

* Registration
* Login
* Password reset
* Roles
* Permissions
* Refresh tokens

Does **not** own:

* Customer profile
* Orders
* Product data

---

## Catalog Context

Owns:

* Products
* Categories
* Collections
* Brands
* Product media
* Product variants

Publishes events such as:

* ProductCreated
* ProductUpdated
* ProductPublished

---

## Inventory Context

Owns:

* Stock quantity
* Stock reservations
* Stock adjustments
* Low stock alerts

Consumes:

* ProductCreated

Publishes:

* InventoryAdjusted
* InventoryReserved
* InventoryReleased

---

## Shopping Context

Owns:

* Cart
* Cart Items
* Wishlist

Consumes:

* Product information
* Inventory availability

Publishes:

* ItemAddedToCart
* CartCheckedOut

---

## Orders Context

Owns:

* Orders
* Order Items
* Order status
* Returns (future)

Consumes:

* Customer
* Cart
* Inventory
* Pricing

Publishes:

* OrderPlaced
* OrderCancelled
* OrderCompleted

---

## Payments Context

Owns:

* Payment attempts
* Transactions
* Refunds

Consumes:

* Orders

Publishes:

* PaymentSucceeded
* PaymentFailed
* RefundIssued

---

## Shipping Context

Owns:

* Shipment
* Courier integration
* Tracking

Consumes:

* Orders

Publishes:

* ShipmentCreated
* ShipmentDelivered

---

## Notifications Context

Owns:

* Email
* SMS
* Push notifications

Consumes business events from every other context.

---

## Analytics Context

Consumes business events from all contexts.

Owns:

* Dashboards
* KPIs
* Reports
* Metrics

---

# 5. Context Communication

Current Architecture:

* Direct service calls (within the modular monolith)
* Shared transaction boundaries where appropriate

Future Evolution:

* Event-driven communication
* Asynchronous messaging
* Independent deployment

---

# 6. Context Ownership Principles

Each context:

* Owns its business rules.
* Owns its persistence model.
* Exposes only approved interfaces.
* Must not directly modify another context's data.

---

# 7. Future Service Extraction Candidates

When scaling beyond the modular monolith, the following contexts are expected to become independent services:

1. Search
2. Notifications
3. Payments
4. Analytics
5. Recommendation Engine

---

# 8. Acceptance Criteria

This document is complete when:

* Every major business capability belongs to exactly one bounded context.
* Context interactions are documented.
* Ownership boundaries are approved.
* Future extraction strategy is defined.

---

# Next Document

**Repository Path**

`docs/04_Database/CONCEPTUAL_DATA_MODEL.md`

The next document converts the business domain into a conceptual data model that will guide the logical schema, ER diagrams, constraints, and the physical PostgreSQL implementation.
