# Repository Path

`docs/database/DATA_DICTIONARY/ENTITY_TEMPLATE.md`

---

# Data Dictionary Entity Template

**Project:** Lunora Wear

**Document Purpose:** Standard template for documenting every database entity.

This template must be used for all entities in the Data Dictionary.

---

# Entity Information

| Property        | Value            |
| --------------- | ---------------- |
| Entity Name     |                  |
| Database Table  |                  |
| Bounded Context |                  |
| Aggregate Root  |                  |
| Owner           |                  |
| Version         |                  |
| Status          | Draft / Approved |

---

# 1. Business Purpose

Describe:

* Why this entity exists.
* What business capability it supports.
* Which workflows depend on it.

---

# 2. Relationships

| Related Entity | Relationship | Cardinality |
| -------------- | ------------ | ----------- |
|                |              |             |

---

# 3. Attributes

| Field | Type | Required | Nullable | Unique | Indexed | Sensitive | API Exposure |
| ----- | ---- | -------- | -------- | ------ | ------- | --------- | ------------ |
|       |      |          |          |        |         |           |              |

---

# 4. Field Specifications

For every attribute document:

## Field Name

### Business Purpose

...

### Validation Rules

...

### Default Value

...

### Allowed Values

...

### Example

...

### Notes

...

Repeat for every field.

---

# 5. Business Rules

List all invariant business rules.

Examples:

* SKU must be unique.
* Inventory cannot become negative.
* Orders cannot exist without at least one Order Item.

---

# 6. Lifecycle

Describe:

Creation

↓

Updates

↓

Archival

↓

Deletion

Include any soft-delete or retention rules.

---

# 7. Security Classification

| Field | Classification |
| ----- | -------------- |
|       | Public         |
|       | Internal       |
|       | Confidential   |
|       | Restricted     |

---

# 8. API Exposure

| Field | Public | Admin | Internal | Never |
| ----- | ------ | ----- | -------- | ----- |
|       |        |       |          |       |

---

# 9. Validation Summary

| Validation  | Description |
| ----------- | ----------- |
| Required    |             |
| Length      |             |
| Format      |             |
| Range       |             |
| Regex       |             |
| Cross-field |             |

---

# 10. Index Recommendations

| Index | Reason |
| ----- | ------ |
|       |        |

---

# 11. Audit Requirements

Document:

* Whether changes are audited.
* Which fields require historical tracking.
* Retention expectations.

---

# 12. Performance Considerations

Examples:

* Frequently filtered columns.
* High-write attributes.
* Candidate caching strategy.

---

# 13. Future Extensions

Document anticipated schema evolution without breaking compatibility.

Examples:

* Multi-language support
* Multi-currency support
* Regionalization
* Versioning

---

# 14. Acceptance Criteria

The entity documentation is complete when:

* Every field is documented.
* Validation is defined.
* Business rules are complete.
* Security classification is assigned.
* API exposure is defined.
* Index recommendations are documented.
* Engineering review is complete.

---

# Related Documents

* `DATABASE_DESIGN_STANDARDS.md`
* `DATABASE_NAMING_STANDARDS.md`
* `LOGICAL_DATA_MODEL.md`
* `ER_DIAGRAM.md`

---

# Change History

| Version | Date       | Author            | Summary         |
| ------- | ---------- | ----------------- | --------------- |
| 1.0.0   | YYYY-MM-DD | Architecture Team | Initial version |
