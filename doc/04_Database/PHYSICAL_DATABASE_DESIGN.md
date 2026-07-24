# Repository Path

`docs/database/PHYSICAL_DATABASE_DESIGN.md`

---

# Physical Database Design

**Project:** Lunora Wear

**Document ID:** LW-DB-PHY-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Database Architecture

**Related Documents**

* `docs/database/LOGICAL_DATA_MODEL.md`
* `docs/database/ER_DIAGRAM.md`
* `docs/database/DATABASE_DESIGN_STANDARDS.md`
* `docs/database/DATABASE_NAMING_STANDARDS.md`
* `docs/database/DATA_DICTIONARY/`

---

# 1. Purpose

This document defines how the logical data model will be implemented within PostgreSQL.

It covers:

* Physical storage strategy
* Key selection
* Indexing
* Constraints
* Concurrency
* Performance
* Maintenance
* Scalability

SQL implementation details are documented separately in `POSTGRESQL_SCHEMA.md`.

---

# 2. Database Platform

| Property      | Value                                                         |
| ------------- | ------------------------------------------------------------- |
| Database      | PostgreSQL                                                    |
| Version       | 17+                                                           |
| Character Set | UTF-8                                                         |
| Collation     | Default locale unless business requirements dictate otherwise |
| Time Zone     | UTC                                                           |

---

# 3. Schema Organization

Initially, the application will use a single application schema (for example, `public` or a dedicated application schema such as `lunora`, based on deployment standards).

Future expansion may introduce separate schemas for:

* Identity
* Catalog
* Orders
* Analytics
* Reporting

Schema boundaries should align with bounded contexts where practical.

---

# 4. Identifier Strategy

Preferred identifier:

* UUID v7

Benefits:

* Globally unique
* Ordered for better index locality
* Suitable for future service decomposition

Business identifiers remain separate from primary keys.

Examples:

* Order Number
* SKU
* Coupon Code

---

# 5. Storage Strategy

High-write entities:

* Shopping Cart
* Inventory
* Refresh Token

High-read entities:

* Product
* Category
* Brand

Mixed workload:

* Orders
* Payments
* Shipment

Storage tuning should reflect expected access patterns.

---

# 6. Indexing Strategy

Every index must support a known query pattern.

Typical indexes include:

* Primary keys
* Foreign keys
* Product slug
* Product SKU
* Email
* Mobile number
* Order number
* Created timestamp

Composite indexes require workload justification.

Unused indexes should be reviewed periodically.

---

# 7. Constraint Strategy

Use database constraints whenever feasible.

Examples:

* Unique email
* Unique SKU
* Positive prices
* Rating range (1–5)
* Non-negative inventory

Application validation complements—not replaces—database constraints.

---

# 8. Transaction Strategy

Critical workflows requiring atomic transactions include:

* Checkout
* Inventory reservation
* Order placement
* Payment recording
* Refund processing

Long-running business processes should be coordinated at the application level rather than through long-lived database transactions.

---

# 9. Concurrency Strategy

Adopt optimistic concurrency where conflicts are expected but infrequent.

Potential candidates:

* Product
* Inventory
* Customer Profile

Inventory adjustments must prevent overselling while maintaining throughput.

---

# 10. Soft Delete Policy

Use soft deletion for entities where recovery or auditability is important.

Examples:

* Product
* Customer Account
* Address

Do not soft delete immutable business records such as completed orders or finalized payments. Instead, use explicit status transitions.

---

# 11. Audit Strategy

Critical entities should generate audit records for:

* Create
* Update
* Delete (logical or physical where permitted)

Audit records should capture:

* Actor
* Timestamp
* Action
* Changed fields (or sufficient detail to reconstruct changes)

---

# 12. Backup and Recovery

Production environments must support:

* Automated scheduled backups
* Point-in-time recovery (PITR)
* Periodic restore validation
* Documented recovery procedures

Recovery objectives (RPO/RTO) should be defined in operational documentation.

---

# 13. Performance Considerations

Design targets:

* Efficient product search
* Fast catalog browsing
* Low-latency checkout
* Responsive order history

Performance tuning should be driven by measured workloads rather than assumptions.

---

# 14. Growth Strategy

The physical design should support:

* Millions of products
* Millions of customers
* Tens of millions of orders
* Historical reporting
* International expansion

Scaling decisions should be deferred until operational metrics justify them.

---

# 15. Security Considerations

Sensitive data protections include:

* Hashed credentials
* Restricted token storage
* Principle of least privilege
* Encrypted connections
* Controlled administrative access

Database roles and permissions are documented separately in the security section.

---

# 16. Operational Maintenance

Routine maintenance includes:

* Index monitoring
* Statistics updates
* Backup verification
* Capacity planning
* Slow query review
* Vacuum and storage maintenance appropriate for PostgreSQL

Maintenance procedures should be automated where practical.

---

# 17. Acceptance Criteria

This document is complete when:

* Physical implementation strategy is documented.
* Storage and indexing guidance is defined.
* Concurrency approach is approved.
* Operational considerations are documented.
* Database engineering review is complete.

---

# Next Document

**Repository Path**

`docs/database/POSTGRESQL_SCHEMA.md`

This document will translate the approved physical design into implementation-ready PostgreSQL DDL, including tables, constraints, indexes, defaults, and migration guidance.
