# Repository Path

`docs/database/DATABASE_DESIGN_STANDARDS.md`

---

# Database Design Standards

**Project:** Lunora Wear

**Document ID:** LW-DB-STD-002

**Version:** 1.0.0

**Status:** Approved

**Owner:** Solution Architecture

**Related Documents**

* `docs/database/DATABASE_NAMING_STANDARDS.md`
* `docs/database/CONCEPTUAL_DATA_MODEL.md`
* `docs/database/LOGICAL_DATA_MODEL.md`
* `docs/database/ER_DIAGRAM.md`

---

# 1. Purpose

This document defines the mandatory database design principles for the Lunora Wear platform.

These standards ensure:

* Consistency
* Data integrity
* Performance
* Security
* Scalability
* Maintainability

All database objects must conform to these standards.

---

# 2. Database Platform

Primary Database Engine:

**PostgreSQL 17+**

Designs must use PostgreSQL capabilities where they provide clear operational or performance benefits while avoiding unnecessary vendor lock-in.

---

# 3. Design Principles

Every schema must follow these principles:

* Normalize first, optimize second.
* Protect data integrity through constraints.
* Prefer explicit relationships.
* Minimize nullable fields.
* Design for scalability.
* Avoid duplicated business data.
* Keep transactions short.
* Make destructive actions explicit.

---

# 4. Primary Keys

Every table uses:

* UUID (preferred) **or**
* BIGINT generated identity

The project must use one strategy consistently.

For distributed systems and future service extraction, **UUID v7** is the preferred long-term strategy.

---

# 5. Foreign Keys

Every relationship must be enforced using foreign keys unless there is a documented exception approved through an ADR.

Foreign keys should define explicit delete behavior:

* RESTRICT
* CASCADE
* SET NULL

No implicit defaults.

---

# 6. Soft Delete Strategy

Business entities should use soft deletion when historical information is valuable.

Standard columns:

* deleted_at
* deleted_by

Entities that represent financial or legal records (such as orders and payments) must never be physically deleted during normal operations.

---

# 7. Audit Strategy

Critical entities require auditing.

Minimum audit information:

* Who performed the action
* When it occurred
* What changed
* Previous value
* New value

Auditing should be append-only.

---

# 8. Timestamp Standard

Unless a documented exception exists, mutable entities include:

* created_at
* updated_at

All timestamps must use UTC.

Application code is responsible for local timezone presentation.

---

# 9. Monetary Values

Money must never use floating-point types.

Recommended:

* NUMERIC(18,2)

Future multi-currency support should separate:

* Amount
* Currency Code

---

# 10. Enumerations

Business states should use:

* Lookup tables **or**
* Strongly typed application enums mapped consistently

Magic numbers are prohibited.

---

# 11. Indexing Strategy

Indexes should be created only for demonstrated query patterns.

Typical candidates:

* Foreign keys
* Slugs
* SKUs
* Order numbers
* Email
* Mobile number
* Created timestamps

Composite indexes must be supported by workload analysis.

---

# 12. Constraints

Use database constraints wherever possible.

Examples:

* Unique email
* Positive prices
* Rating between 1 and 5
* Non-negative inventory

Business rules should not rely solely on application code.

---

# 13. Transactions

Transactions should be:

* Atomic
* Short-lived
* Retry-safe where appropriate

Inventory reservation, payment recording, and order creation must be transactionally consistent.

---

# 14. Concurrency

Use optimistic concurrency for business entities where appropriate.

Examples:

* Product
* Inventory
* Customer Profile

Prevent lost updates through versioning or equivalent mechanisms.

---

# 15. Data Retention

| Entity                | Retention Policy                                 |
| --------------------- | ------------------------------------------------ |
| Orders                | Permanent                                        |
| Payments              | Permanent                                        |
| Audit Logs            | Configurable (minimum business retention period) |
| Password Reset Tokens | Expire automatically                             |
| Refresh Tokens        | Expire automatically                             |
| Shopping Carts        | Configurable cleanup                             |
| Notifications         | Configurable archival                            |

Retention periods must comply with applicable legal and business requirements.

---

# 16. Security

Sensitive data must be protected.

Examples:

Restricted:

* Password hashes
* Refresh tokens
* Reset tokens

Confidential:

* Email
* Mobile number
* Address

Public:

* Product information
* Categories
* Collections

Sensitive information must never appear in logs.

---

# 17. Performance Guidelines

The schema should support:

* Fast product browsing
* Efficient filtering
* Low-latency checkout
* High-volume order processing

Avoid excessive joins in high-frequency customer-facing queries.

Materialized views or read models may be introduced after performance analysis.

---

# 18. Backup & Recovery

The production database must support:

* Automated backups
* Point-in-time recovery
* Disaster recovery testing

Recovery procedures should be documented separately.

---

# 19. Acceptance Criteria

This standard is complete when:

* Database design principles are documented.
* Integrity requirements are defined.
* Performance expectations are established.
* Security standards are documented.
* Engineering leadership approves the document.

---

# Next Document

**Repository Path**

`docs/database/DATA_DICTIONARY/IDENTITY.md`

The next document begins the detailed data dictionary, starting with the Identity context. It will define every attribute of:

* `app_user`
* `role`
* `permission`
* `user_role`
* `refresh_token`
* `password_reset_token`

including validation, ownership, indexing, API exposure, sensitivity classification, and business rules.
