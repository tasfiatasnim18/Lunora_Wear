# Repository Path

`docs/04_Database/CONCEPTUAL_DATA_MODEL.md`

---

# Conceptual Data Model

**Project:** Lunora Wear

**Document ID:** LW-CDM-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Solution Architecture

**Related Documents**

* `docs/03_Domain_Model/DOMAIN_MODEL.md`
* `docs/03_Domain_Model/BOUNDED_CONTEXT_MAP.md`
* `docs/02_Information_Architecture/INFORMATION_ARCHITECTURE.md`

---

# 1. Purpose

The Conceptual Data Model (CDM) defines the major business entities of the Lunora Wear platform and the relationships between them.

This document is technology-agnostic. It describes **what** the business needs to manage, not **how** the data will be stored.

The CDM is the bridge between the Domain Model and the Logical Data Model.

---

# 2. Design Principles

The conceptual model must:

* Represent business concepts, not database tables.
* Avoid implementation details.
* Define clear ownership for each entity.
* Capture business relationships.
* Support future platform growth.

---

# 3. Core Business Entities

## Identity Context

### User

**Purpose**

Represents an authenticated person using the platform.

**Owns**

* Profile
* Authentication credentials
* Roles

**Relationships**

* One User → Many Addresses
* One User → Many Orders
* One User → One Shopping Cart
* One User → Many Reviews

---

### Role

Represents a permission group.

Examples:

* Customer
* Administrator
* Support Agent

Relationship:

Many Users ↔ Many Roles

---

## Customer Context

### Address

Represents a delivery location.

Types:

* Home
* Office
* Other

Relationship:

One User → Many Addresses

---

## Catalog Context

### Category

Purpose:

Organize products hierarchically.

Relationship:

* Parent Category → Child Categories
* Category → Many Products

Supports unlimited hierarchy.

---

### Brand

Represents the manufacturer or brand identity.

Relationship:

One Brand → Many Products

---

### Collection

Represents a curated merchandising group.

Examples:

* Summer Collection
* Eid Collection
* New Arrivals

Relationship:

Many Products ↔ Many Collections

---

### Product

Represents a sellable fashion item.

Contains:

* Name
* Description
* Brand
* Category
* Status

Relationship:

* One Product → Many Variants
* One Product → Many Images
* One Product → Many Reviews

---

### Product Variant

Represents a purchasable version of a product.

Typical attributes:

* Size
* Color
* SKU

Relationship:

One Variant → One Inventory Record

---

### Product Image

Represents product media.

Relationship:

One Product → Many Images

One image may be designated as the primary image.

---

## Inventory Context

### Inventory

Tracks stock for a single product variant.

Business Rules:

* Quantity Available ≥ 0
* Reserved Quantity ≥ 0
* Available ≥ Reserved

Relationship:

One Variant → One Inventory

---

## Shopping Context

### Shopping Cart

Represents a customer's active cart.

Relationship:

One User → One Active Cart

Contains:

Many Cart Items

---

### Cart Item

Represents one selected variant.

Relationship:

Cart → Many Cart Items

Each Cart Item references one Product Variant.

---

## Orders Context

### Order

Represents a completed purchase request.

Relationship:

One User → Many Orders

Contains:

Many Order Items

Associated With:

* Payment
* Shipment

---

### Order Item

Represents one purchased variant.

Captures a pricing snapshot at purchase time.

Relationship:

One Order → Many Order Items

---

## Payments Context

### Payment

Represents payment activity.

States include:

* Pending
* Paid
* Failed
* Refunded

Relationship:

One Order → Many Payment Attempts

---

## Shipping Context

### Shipment

Represents delivery progress.

Lifecycle:

Pending

↓

Packed

↓

Shipped

↓

Delivered

---

## Marketing Context

### Coupon

Represents promotional discounts.

Relationship:

Applied during checkout.

May apply to:

* Entire Order
* Specific Collection
* Specific Category
* Specific Product

---

## Engagement Context

### Review

Represents customer feedback.

Rules:

* Verified purchasers only.
* One review per product per order.

---

### Wishlist

Represents saved purchase intent.

Relationship:

One User ↔ Many Product Variants

---

## Content Context

### Banner

Homepage promotional content.

---

### CMS Page

Static informational content.

Examples:

* About
* Contact
* Privacy Policy
* Terms & Conditions

---

# 4. High-Level Relationship Summary

```text
User
 ├── Addresses
 ├── Cart
 │      └── Cart Items
 ├── Orders
 │      ├── Order Items
 │      ├── Payments
 │      └── Shipments
 ├── Wishlist
 └── Reviews

Category
 └── Products
        ├── Variants
        │      └── Inventory
        ├── Images
        └── Reviews

Brand
 └── Products

Collection
 └── Products
```

---

# 5. Business Invariants

The following rules must always remain true:

* Every Product belongs to one Category.
* Every Variant belongs to exactly one Product.
* Inventory is tracked at the Variant level.
* Orders are immutable after confirmation.
* Payment history is never deleted.
* Product reviews require a completed purchase.
* Users may own multiple addresses.
* Cart items must reference valid product variants.

---

# 6. Lifecycle Overview

| Entity   | Created | Updated     | Archived      |
| -------- | ------- | ----------- | ------------- |
| User     | Yes     | Yes         | Soft Delete   |
| Product  | Yes     | Yes         | Soft Delete   |
| Variant  | Yes     | Yes         | Soft Delete   |
| Order    | Yes     | Status Only | Never Deleted |
| Payment  | Yes     | Status Only | Never Deleted |
| Shipment | Yes     | Status Only | Never Deleted |
| Review   | Yes     | Moderated   | Soft Delete   |

---

# 7. Out of Scope

This conceptual model intentionally excludes:

* Table names
* Primary keys
* Foreign keys
* Data types
* Indexes
* Database normalization
* SQL implementation

These are addressed in the Logical Data Model.

---

# 8. Acceptance Criteria

This document is complete when:

* All core business entities are identified.
* Ownership is defined.
* Relationships are documented.
* Business invariants are established.
* Lifecycle expectations are captured.
* Product and engineering teams approve the conceptual model.

---

# Next Document

**Repository Path**

`docs/04_Database/LOGICAL_DATA_MODEL.md`

The Logical Data Model transforms these conceptual entities into normalized data structures, defining attributes, relationships, cardinality, candidate keys, and constraints before physical PostgreSQL implementation.
