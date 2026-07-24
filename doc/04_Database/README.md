# Repository Path

`docs/database/DATA_DICTIONARY/README.md`

---

# Data Dictionary

**Project:** Lunora Wear

**Document ID:** LW-DD-000

**Version:** 1.0.0

**Status:** Draft

**Owner:** Database Architecture

---

# Purpose

The Data Dictionary is the authoritative reference for every logical entity and attribute used by the Lunora Wear platform.

It standardizes:

* Field definitions
* Business meaning
* Validation
* Data ownership
* Sensitivity classification
* Default values
* Lifecycle
* Index recommendations

This documentation is shared across Product, Engineering, QA, DevOps, Security, and Analytics teams.

---

# Data Dictionary Modules

| Module    | Repository Path                              |
| --------- | -------------------------------------------- |
| Identity  | `docs/database/DATA_DICTIONARY/IDENTITY.md`  |
| Customer  | `docs/database/DATA_DICTIONARY/CUSTOMER.md`  |
| Catalog   | `docs/database/DATA_DICTIONARY/CATALOG.md`   |
| Inventory | `docs/database/DATA_DICTIONARY/INVENTORY.md` |
| Shopping  | `docs/database/DATA_DICTIONARY/SHOPPING.md`  |
| Orders    | `docs/database/DATA_DICTIONARY/ORDERS.md`    |
| Payments  | `docs/database/DATA_DICTIONARY/PAYMENTS.md`  |
| Shipping  | `docs/database/DATA_DICTIONARY/SHIPPING.md`  |
| Marketing | `docs/database/DATA_DICTIONARY/MARKETING.md` |
| Content   | `docs/database/DATA_DICTIONARY/CONTENT.md`   |
| Analytics | `docs/database/DATA_DICTIONARY/ANALYTICS.md` |

---

# Field Definition Standard

Every field in every entity will be documented using the following template.

| Property         | Description                                   |
| ---------------- | --------------------------------------------- |
| Field Name       | Official database column name                 |
| Business Name    | Human-readable name                           |
| Description      | Business purpose                              |
| Data Type        | Logical data type                             |
| Nullable         | Yes / No                                      |
| Required         | Yes / No                                      |
| Default Value    | Default if applicable                         |
| Validation Rules | Business validation                           |
| Example          | Example value                                 |
| Sensitive        | Public / Internal / Confidential / Restricted |
| Indexed          | Yes / No                                      |
| Unique           | Yes / No                                      |
| Searchable       | Yes / No                                      |
| API Exposure     | Public / Internal / Never                     |
| Audit Required   | Yes / No                                      |

---

# Sensitivity Levels

| Level        | Meaning                                                   |
| ------------ | --------------------------------------------------------- |
| Public       | Safe for public display                                   |
| Internal     | Internal application data                                 |
| Confidential | Business-sensitive information                            |
| Restricted   | Personally identifiable or security-sensitive information |

---

# Naming Convention

All field names must follow the rules defined in:

`docs/database/DATABASE_NAMING_STANDARDS.md`

---

# Ownership

Every module is owned by exactly one bounded context.

Examples:

* Identity → Identity Context
* Orders → Orders Context
* Catalog → Catalog Context

Cross-context modifications are prohibited except through approved interfaces.

---

# Change Management

Every change to a field requires:

* Updated documentation
* Schema migration (if applicable)
* API review (if exposed)
* Backward compatibility assessment
* Approval from Engineering

---

# Acceptance Criteria

The Data Dictionary is complete when:

* Every entity has documented attributes.
* Every field has a business definition.
* Validation rules are defined.
* Sensitivity classifications are assigned.
* Ownership is documented.
* Product and Engineering approve the definitions.

---

# Next Document

**Repository Path**

`docs/database/DATA_DICTIONARY/IDENTITY.md`

This document defines the data dictionary for the Identity context, including `app_user`, `role`, `permission`, `user_role`, `refresh_token`, and `password_reset_token` entities.
