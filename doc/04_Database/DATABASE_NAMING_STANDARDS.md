# Repository Path

`docs/database/DATABASE_NAMING_STANDARDS.md`

---

# Database Naming Standards

**Project:** Lunora Wear

**Document ID:** LW-DB-STD-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Solution Architecture

**Related Documents**

* `docs/database/LOGICAL_DATA_MODEL.md`
* `docs/database/ER_DIAGRAM.md`
* `docs/database/DATA_DICTIONARY.md`

---

# 1. Purpose

This document establishes the official database naming conventions for the Lunora Wear platform.

These standards ensure:

* Consistency
* Readability
* Maintainability
* Predictable schema evolution
* Easier onboarding
* Cleaner SQL
* Better ORM compatibility

These rules are mandatory for every database object.

---

# 2. General Principles

Database names must be:

* Clear
* Singular where appropriate for entities
* Lowercase
* Snake_case
* English only
* Free of abbreviations unless universally accepted

---

# 3. Table Names

Use singular nouns.

✅ Correct

```text
user
product
order
shipment
review
```

❌ Incorrect

```text
users
tbl_products
ProductMaster
```

---

# 4. Primary Keys

Every table uses:

```text
id
```

Never:

```text
user_id
product_pk
pk_product
```

Example:

```text
product
--------
id
name
slug
```

---

# 5. Foreign Keys

Foreign keys follow:

```text
<referenced_entity>_id
```

Examples

```text
product_id
category_id
user_id
order_id
variant_id
coupon_id
```

---

# 6. Timestamp Columns

Every mutable entity should include:

```text
created_at
updated_at
```

Soft-deletable entities additionally include:

```text
deleted_at
```

---

# 7. Boolean Columns

Boolean names should read naturally.

Examples

```text
is_active
is_verified
is_featured
is_deleted
is_default
```

Avoid

```text
active
deleted
verified
```

---

# 8. Status Columns

Prefer enumerated status values.

Examples

```text
status
payment_status
shipment_status
order_status
```

Avoid multiple boolean flags for state management.

---

# 9. Monetary Values

Always store monetary amounts using decimal types.

Preferred names:

```text
price
cost_price
discount_amount
subtotal
tax_amount
shipping_fee
total_amount
```

Currency should be stored separately if multi-currency support is introduced.

---

# 10. Quantity Columns

Use descriptive names:

```text
quantity
available_quantity
reserved_quantity
minimum_stock_level
```

---

# 11. Slugs

SEO-friendly entities use:

```text
slug
```

Examples:

* Product
* Category
* Collection
* CMS Page

---

# 12. Audit Columns

Where applicable:

```text
created_by
updated_by
deleted_by
```

These reference the `user` entity.

---

# 13. Index Naming

Pattern:

```text
idx_<table>_<column>
```

Examples:

```text
idx_product_slug
idx_order_user_id
idx_variant_sku
```

Composite indexes:

```text
idx_product_category_status
```

---

# 14. Unique Constraints

Pattern:

```text
uq_<table>_<column>
```

Examples:

```text
uq_user_email
uq_user_mobile_number
uq_product_slug
uq_variant_sku
```

---

# 15. Foreign Key Constraints

Pattern:

```text
fk_<child_table>_<parent_table>
```

Examples:

```text
fk_product_category
fk_order_user
fk_inventory_product_variant
```

---

# 16. Check Constraints

Pattern:

```text
chk_<table>_<rule>
```

Examples:

```text
chk_inventory_non_negative
chk_review_rating_range
chk_price_positive
```

---

# 17. Junction Tables

For many-to-many relationships:

Use:

```text
product_collection
user_role
wishlist_item
```

Alphabetical ordering is preferred unless business terminology dictates otherwise.

---

# 18. Enum Values

Store meaningful values.

Good

```text
Pending
Confirmed
Packed
Delivered
Cancelled
Returned
```

Avoid

```text
1
2
3
4
5
```

unless implemented through lookup tables or well-defined enum mappings.

---

# 19. Reserved Words

Avoid database reserved keywords.

Examples to avoid:

* user (if conflicting with the target DB)
* order
* group

If required, use explicit alternatives such as:

* app_user
* customer_order

These decisions will be finalized during physical schema design based on PostgreSQL best practices.

---

# 20. Migration Naming

Migration names follow:

```text
YYYYMMDDHHMM_<description>
```

Examples

```text
202607250930_create_product_table
202607251015_add_coupon_usage
```

---

# 21. Examples

### Product

```text
app_product
------------
id
category_id
brand_id
name
slug
description
status
created_at
updated_at
```

### Order

```text
customer_order
----------------
id
user_id
order_number
payment_status
shipment_status
total_amount
created_at
updated_at
```

---

# 22. Acceptance Criteria

This standard is complete when:

* All naming rules are documented.
* Reserved-word strategy is defined.
* Constraint naming is standardized.
* Index naming is standardized.
* Migration naming is standardized.
* Engineering team approves the conventions.

---

# Next Document

**Repository Path**

`docs/database/DATA_DICTIONARY.md`

The Data Dictionary will apply these naming standards to every entity and attribute, providing business definitions, validation rules, data sensitivity classifications, ownership, and usage notes.
