# Domain Model

**Project:** Lunora Wear

**Document ID:** LW-DM-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Solution Architecture

**Related Documents**

* Project Charter
* ADR-000
* Product Requirements Document
* Information Architecture
* User Flows

---

# 1. Purpose

The Domain Model defines the core business entities of the Lunora Wear platform, their responsibilities, ownership, and relationships.

This document represents the business language of the system and serves as the foundation for:

* Database Design
* API Design
* Backend Architecture
* Event Modeling
* Future microservice boundaries

It intentionally describes business concepts rather than database tables or implementation details.

---

# 2. Bounded Contexts

The platform is organized into the following business domains:

1. Identity & Access
2. Catalog
3. Inventory
4. Pricing & Promotions
5. Shopping
6. Orders
7. Payments
8. Shipping
9. Customer
10. Content Management
11. Reporting & Analytics
12. Notifications

Each context owns its own business rules and data.

---

# 3. Core Domain Entities

## User

Represents any authenticated person interacting with the platform.

Attributes (conceptual):

* User ID
* Name
* Email
* Mobile Number
* Status
* Roles

Relationships:

* Owns Orders
* Owns Addresses
* Owns Wishlist
* Owns Cart
* Owns Reviews

---

## Product

Represents a sellable fashion item.

Attributes:

* Product ID
* Name
* Slug
* Description
* Brand
* Status

Relationships:

* Belongs to Category
* Has Variants
* Has Images
* Has Reviews
* Belongs to Collections

---

## Product Variant

Represents a purchasable variation of a product.

Examples:

* Size
* Color

Attributes:

* Variant ID
* SKU
* Size
* Color
* Price Override (optional)
* Barcode (future)

Relationships:

* Belongs to Product
* Has Inventory

---

## Category

Represents the product hierarchy.

Relationships:

* Parent Category (optional)
* Child Categories
* Products

Supports unlimited nesting.

---

## Collection

Represents curated product groupings.

Examples:

* Summer Collection
* Eid Collection
* New Arrivals

Collections do not replace categories.

A product may belong to multiple collections.

---

## Inventory

Tracks stock availability for each product variant.

Attributes:

* Quantity Available
* Reserved Quantity
* Reorder Threshold

Business Rules:

* Inventory cannot become negative.
* Reservations occur during checkout.
* Final deduction occurs after successful order placement.

---

## Shopping Cart

Represents the customer's temporary purchase intent.

Relationships:

* Customer
* Cart Items

Rules:

* Guest carts supported.
* Logged-in carts synchronized across devices (future).

---

## Cart Item

Represents one selected variant inside the cart.

Contains:

* Variant
* Quantity
* Unit Price
* Discount Snapshot

---

## Order

Represents a confirmed purchase.

Relationships:

* Customer
* Order Items
* Payment
* Shipment

Orders are immutable after confirmation except through defined business processes (e.g., cancellation, return).

---

## Order Item

Represents an individual purchased product variant.

Contains:

* Variant
* Quantity
* Unit Price
* Discount
* Tax (future)
* Total

---

## Payment

Represents a financial transaction associated with an order.

States:

* Pending
* Authorized
* Paid
* Failed
* Refunded
* Partially Refunded (future)

Each order may have one or more payment attempts.

---

## Shipment

Represents the delivery lifecycle.

States:

* Pending
* Packed
* Shipped
* Out for Delivery
* Delivered
* Returned

---

## Coupon

Represents promotional discounts.

Types:

* Percentage
* Fixed Amount
* Free Shipping (future)

Rules:

* Configurable validity.
* Usage limits.
* Eligibility conditions.

---

## Address

Represents a customer delivery location.

Relationships:

* Customer

Types:

* Home
* Office
* Other

---

## Review

Represents customer feedback on purchased products.

Rules:

* Verified purchasers only.
* One review per product per order.

---

## Notification

Represents customer communication.

Channels:

* Email
* SMS
* Push (future)

Types:

* Order Confirmation
* Shipping Update
* Password Reset
* Promotional Campaign

---

# 4. High-Level Relationships

```text
User
 ├── Addresses
 ├── Cart
 │     └── Cart Items
 ├── Orders
 │     ├── Order Items
 │     ├── Payment
 │     └── Shipment
 ├── Wishlist
 └── Reviews

Category
 └── Products
        ├── Variants
        │      └── Inventory
        ├── Images
        └── Reviews

Collections
 └── Products
```

---

# 5. Aggregate Boundaries

The following entities form aggregates in the Domain-Driven Design (DDD) sense:

| Aggregate Root | Contains           |
| -------------- | ------------------ |
| User           | Addresses          |
| Product        | Variants, Images   |
| Cart           | Cart Items         |
| Order          | Order Items        |
| Category       | Child Categories   |
| Collection     | Product References |

All modifications should occur through the aggregate root to maintain business consistency.

---

# 6. Domain Events

Key business events include:

* UserRegistered
* UserLoggedIn
* ProductPublished
* InventoryAdjusted
* ItemAddedToCart
* CartCheckedOut
* OrderPlaced
* PaymentSucceeded
* PaymentFailed
* ShipmentCreated
* ShipmentDelivered
* ReviewSubmitted

These events will support future integrations and eventual microservice decomposition.

---

# 7. Ubiquitous Language

To ensure consistency across business and engineering teams:

* "Product" = sellable fashion item.
* "Variant" = specific size/color combination.
* "Collection" = curated grouping.
* "Category" = hierarchical classification.
* "Order" = confirmed purchase.
* "Cart" = temporary purchase intent.
* "Inventory" = available stock.
* "Shipment" = delivery process.

These terms must be used consistently in documentation, APIs, code, and UI.

---

# 8. Design Principles

* Keep entities focused on business responsibilities.
* Avoid duplication across bounded contexts.
* Preserve aggregate consistency.
* Prefer explicit business terminology over technical jargon.
* Design for future service extraction without premature complexity.

---

# 9. Acceptance Criteria

This Domain Model is complete when:

* Core business entities are identified.
* Relationships are documented.
* Aggregate boundaries are defined.
* Domain events are listed.
* Ubiquitous language is established.
* Product and engineering teams approve the model.

---

# Next Document

**Path**

`docs/09_Database/CONCEPTUAL_DATA_MODEL.md`

The next document translates these business entities into a conceptual data model, which will then evolve into the logical schema, ER diagrams, indexing strategy, constraints, and physical PostgreSQL database design.
