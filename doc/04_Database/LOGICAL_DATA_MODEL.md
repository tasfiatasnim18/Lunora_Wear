# Logical Data Model

**Project:** Lunora Wear

**Document ID:** LW-LDM-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Solution Architecture

**Related Documents**

* `docs/domain/DOMAIN_MODEL.md`
* `docs/domain/BOUNDED_CONTEXT_MAP.md`
* `docs/database/CONCEPTUAL_DATA_MODEL.md`

---

# 1. Purpose

The Logical Data Model (LDM) transforms the business-oriented Conceptual Data Model into a structured representation suitable for relational database design.

Unlike the Conceptual Data Model, the Logical Data Model introduces:

* Logical entities
* Attributes
* Candidate keys
* Relationships
* Cardinality
* Constraints
* Normalization

It remains independent of PostgreSQL-specific implementation details.

---

# 2. Design Goals

The Logical Data Model must:

* Eliminate unnecessary redundancy.
* Preserve business integrity.
* Support high read and write performance.
* Scale to millions of products and customers.
* Enable future feature expansion without structural redesign.

---

# 3. Entity Inventory

The platform consists of the following logical entities:

## Identity

* User
* Role
* Permission
* UserRole
* RefreshToken
* PasswordResetToken

---

## Customer

* Address
* Wishlist
* WishlistItem

---

## Catalog

* Category
* Brand
* Collection
* Product
* ProductVariant
* ProductImage
* ProductCollection

---

## Inventory

* Inventory
* InventoryAdjustment

---

## Shopping

* Cart
* CartItem

---

## Orders

* Order
* OrderItem
* OrderStatusHistory

---

## Payments

* Payment
* PaymentTransaction

---

## Shipping

* Shipment
* ShipmentTrackingEvent

---

## Marketing

* Coupon
* CouponUsage

---

## Engagement

* Review

---

## Content

* Banner
* CmsPage

---

# 4. Relationships

## User

One User

→ Many Addresses

→ One Active Cart

→ Many Orders

→ Many Reviews

→ Many Wishlist Items

---

## Product

One Product

→ Many Variants

→ Many Images

→ Many Reviews

→ Many Collections

---

## Product Variant

One Variant

→ One Inventory

→ Many Cart Items

→ Many Order Items

---

## Category

One Category

→ Many Products

One Category

→ Many Child Categories

---

## Order

One Order

→ Many Order Items

→ Many Payments

→ One Shipment

---

# 5. Candidate Attributes

## User

* UserId
* FullName
* Email
* MobileNumber
* PasswordHash
* Status
* CreatedAt
* UpdatedAt

---

## Product

* ProductId
* Name
* Slug
* Description
* BrandId
* CategoryId
* Status
* PublishedAt

---

## Product Variant

* VariantId
* ProductId
* SKU
* Size
* Color
* Price
* CostPrice
* Barcode
* Weight

---

## Inventory

* VariantId
* QuantityAvailable
* QuantityReserved
* ReorderLevel

---

## Order

* OrderId
* OrderNumber
* UserId
* OrderStatus
* PaymentStatus
* ShippingStatus
* TotalAmount

---

## Payment

* PaymentId
* OrderId
* Provider
* TransactionReference
* Amount
* Status

---

# 6. Cardinality

| Relationship        | Cardinality |
| ------------------- | ----------- |
| User → Address      | 1 : N       |
| User → Order        | 1 : N       |
| User → Cart         | 1 : 1       |
| Product → Variant   | 1 : N       |
| Product → Image     | 1 : N       |
| Product → Review    | 1 : N       |
| Category → Product  | 1 : N       |
| Order → OrderItem   | 1 : N       |
| Order → Payment     | 1 : N       |
| Variant → Inventory | 1 : 1       |

---

# 7. Candidate Keys

Examples:

* User.Email
* User.MobileNumber
* Product.Slug
* ProductVariant.SKU
* Order.OrderNumber
* Payment.TransactionReference
* Coupon.Code

These keys must remain unique within their respective domains.

---

# 8. Integrity Constraints

Examples include:

* Every Product must reference a valid Category.
* Every Variant must reference a valid Product.
* Every Inventory record must reference one Variant.
* Every OrderItem must reference one Variant.
* Every Payment must reference one Order.
* Every Shipment must reference one Order.

---

# 9. Normalization

The model targets Third Normal Form (3NF):

* Eliminate duplicate data.
* Separate repeating groups.
* Preserve referential integrity.
* Allow selective denormalization only when justified by performance requirements.

---

# 10. Audit Requirements

The following entities require auditing:

* User
* Product
* Inventory
* Order
* Payment
* Shipment
* Coupon

Audit records should capture:

* Action
* Timestamp
* Actor
* Previous Value
* New Value

---

# 11. Acceptance Criteria

The Logical Data Model is complete when:

* All entities are defined.
* Relationships are documented.
* Cardinality is established.
* Candidate keys are identified.
* Integrity constraints are approved.
* Ready for physical database design.

---

# Next Document

**Repository Path**

`docs/database/ER_DIAGRAM.md`

The ER Diagram will visualize all logical entities, their relationships, cardinality, and ownership, serving as the reference for PostgreSQL schema creation and Entity Framework Core model generation.
