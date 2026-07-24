# Repository Path

`docs/database/POSTGRESQL_SCHEMA.md`

---

# PostgreSQL Schema Specification

**Project:** Lunora Wear

**Document ID:** LW-DB-PG-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Database Engineering

**Related Documents**

* `docs/database/PHYSICAL_DATABASE_DESIGN.md`
* `docs/database/DATABASE_DESIGN_STANDARDS.md`
* `docs/database/DATABASE_NAMING_STANDARDS.md`
* `docs/database/DATA_DICTIONARY/`

---

# 1. Purpose

This document defines the implementation standards for the PostgreSQL database schema.

It is the authoritative reference for:

* Database migrations
* Entity Framework Core mapping
* Constraints
* Indexes
* Default values
* Referential integrity

It intentionally specifies implementation rules rather than listing every table definition inline.

---

# 2. PostgreSQL Version

| Property        | Value      |
| --------------- | ---------- |
| Database        | PostgreSQL |
| Minimum Version | 17         |
| Encoding        | UTF-8      |
| Time Zone       | UTC        |

---

# 3. Primary Key Standard

All tables use a single-column primary key:

* Type: UUID v7
* Column name: `id`

Business identifiers (e.g., `order_number`, `sku`, `coupon_code`) are not primary keys and must have their own uniqueness constraints where required.

---

# 4. Common Columns

Mutable entities should include:

| Column     | Type        |
| ---------- | ----------- |
| created_at | TIMESTAMPTZ |
| updated_at | TIMESTAMPTZ |

Soft-deletable entities additionally include:

| Column     | Type        |
| ---------- | ----------- |
| deleted_at | TIMESTAMPTZ |
| deleted_by | UUID        |

Audit columns should reference `app_user` where applicable.

---

# 5. Data Type Standards

| Business Type                  | PostgreSQL Type |
| ------------------------------ | --------------- |
| Identifier                     | UUID            |
| Name                           | VARCHAR(n)      |
| Description                    | TEXT            |
| Money                          | NUMERIC(18,2)   |
| Quantity                       | INTEGER         |
| Boolean                        | BOOLEAN         |
| Timestamp                      | TIMESTAMPTZ     |
| JSON Metadata (exception only) | JSONB           |

`TEXT` should be preferred over arbitrarily large `VARCHAR` where no practical maximum length exists.

---

# 6. Constraint Standards

Every table must define:

* Primary Key
* Required foreign keys
* NOT NULL constraints
* Unique constraints where applicable
* CHECK constraints for business invariants

Examples:

* Rating between 1 and 5
* Quantity ≥ 0
* Price ≥ 0

---

# 7. Foreign Key Policy

Every foreign key must explicitly define its delete behavior.

Typical examples:

| Relationship                | Delete Action |
| --------------------------- | ------------- |
| Category → Product          | RESTRICT      |
| Product → Product Variant   | CASCADE       |
| Order → Order Item          | CASCADE       |
| User → Address              | CASCADE       |
| Product Variant → Inventory | RESTRICT      |

Delete behavior must reflect business rules rather than convenience.

---

# 8. Index Standards

Required indexes include:

* Primary keys
* Foreign keys
* Unique business identifiers
* Frequently filtered columns

Index names follow:

* `idx_<table>_<column>`
* `uq_<table>_<column>`

Composite indexes must be documented with the query pattern they support.

---

# 9. Default Values

Defaults should be explicit.

Examples:

* `created_at` → current UTC timestamp
* `status` → business-defined initial state
* Boolean flags → explicit TRUE/FALSE

Avoid relying on implicit database defaults.

---

# 10. Migration Standards

Migrations should:

* Be forward-only in production.
* Represent a single logical change.
* Be idempotent where practical for deployment tooling.
* Include rollback guidance even if automated rollback is not supported.

Naming convention:

`YYYYMMDDHHMM_<description>`

---

# 11. Seed Data

Static reference data may be seeded for:

* Roles
* Permissions
* Order statuses
* Payment statuses
* Shipment statuses

Business data (customers, products, orders) must not be inserted through schema migrations.

---

# 12. Schema Validation

Every schema change must be validated for:

* Naming compliance
* Referential integrity
* Migration success
* Performance impact
* Backward compatibility

---

# 13. Operational Readiness

Before deployment:

* All migrations reviewed.
* Backup verified.
* Restore tested.
* Performance checks completed.
* Monitoring configured.

---

# 14. Acceptance Criteria

This specification is complete when:

* All implementation standards are documented.
* Migration policy is defined.
* Constraint and index standards are approved.
* Database engineering signs off.

---

# Next Document

**Repository Path**

`docs/api/API_STANDARDS.md`

The API Standards document will define REST conventions, versioning, authentication, error handling, pagination, filtering, idempotency, validation, and response formats that every backend endpoint must follow.
