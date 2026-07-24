# Repository Path

`docs/database/DATA_DICTIONARY/IDENTITY.md`

---

# Identity Data Dictionary

**Project:** Lunora Wear

**Document ID:** LW-DD-ID-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Identity Domain

**Bounded Context:** Identity

**Related Documents**

* `docs/domain/DOMAIN_MODEL.md`
* `docs/database/DATABASE_DESIGN_STANDARDS.md`
* `docs/database/DATABASE_NAMING_STANDARDS.md`
* `docs/database/LOGICAL_DATA_MODEL.md`

---

# Purpose

This document defines the logical data structures for the Identity context.

It is the authoritative source for:

* Entity Framework Core entities
* Validation rules
* API contracts
* Security reviews
* Database implementation

---

# Entity: app_user

## Business Purpose

Represents an authenticated person who can access the Lunora Wear platform.

A user may be:

* Customer
* Administrator
* Customer Support
* Operations Staff (future)

---

## Table Ownership

Identity Context

---

## Candidate Attributes

| Field           | Type        | Required | Unique | Indexed | Sensitive    | API   |
| --------------- | ----------- | -------- | ------ | ------- | ------------ | ----- |
| id              | UUID        | Yes      | Yes    | PK      | Internal     | No    |
| full_name       | String(100) | Yes      | No     | No      | Internal     | Yes   |
| email           | String(255) | Yes      | Yes    | Yes     | Confidential | Yes   |
| mobile_number   | String(20)  | Yes      | Yes    | Yes     | Confidential | Yes   |
| password_hash   | String      | Yes      | No     | No      | Restricted   | Never |
| status          | Enum        | Yes      | No     | Yes     | Internal     | Yes   |
| email_verified  | Boolean     | Yes      | No     | No      | Internal     | Yes   |
| mobile_verified | Boolean     | Yes      | No     | No      | Internal     | Yes   |
| last_login_at   | Timestamp   | No       | No     | Yes     | Internal     | No    |
| created_at      | Timestamp   | Yes      | No     | Yes     | Internal     | No    |
| updated_at      | Timestamp   | Yes      | No     | No      | Internal     | No    |
| deleted_at      | Timestamp   | No       | No     | No      | Internal     | No    |

---

## Business Rules

* Email must be unique.
* Mobile number must be unique.
* Passwords are stored only as hashes.
* Password hashes are never exposed through APIs.
* Deleted users cannot authenticate.
* Disabled users cannot authenticate.

---

## Validation Rules

### full_name

* Required
* 2–100 characters
* Unicode supported
* Trim leading/trailing whitespace

---

### email

* RFC-compliant email format
* Lowercase before persistence
* Maximum 255 characters

---

### mobile_number

* Bangladesh format for MVP
* E.164 format for future international support

---

### password_hash

* Generated only by the authentication service
* Never accepted directly from client requests

---

# Entity: role

## Purpose

Defines a collection of permissions.

Typical values:

* Customer
* Admin
* Support

---

## Candidate Attributes

| Field       | Type        | Required |
| ----------- | ----------- | -------- |
| id          | UUID        | Yes      |
| name        | String(50)  | Yes      |
| description | String(255) | No       |
| created_at  | Timestamp   | Yes      |

---

## Rules

* Role names must be unique.
* Roles cannot be deleted while assigned.

---

# Entity: permission

## Purpose

Represents a fine-grained authorization capability.

Examples:

* product.read
* product.create
* order.cancel
* user.manage

---

## Candidate Attributes

| Field       | Type        | Required |
| ----------- | ----------- | -------- |
| id          | UUID        | Yes      |
| code        | String(100) | Yes      |
| description | String(255) | No       |

---

## Rules

* Permission codes must be globally unique.
* Codes follow `<resource>.<action>` format.

---

# Entity: user_role

## Purpose

Associates users with roles.

Relationship:

Many Users ↔ Many Roles

---

## Candidate Attributes

| Field       | Type      |
| ----------- | --------- |
| user_id     | UUID      |
| role_id     | UUID      |
| assigned_at | Timestamp |

---

## Rules

* Duplicate assignments are prohibited.
* Every assignment references valid User and Role entities.

---

# Entity: refresh_token

## Purpose

Maintains long-lived authentication sessions.

---

## Candidate Attributes

| Field      | Type      | Sensitive  |
| ---------- | --------- | ---------- |
| id         | UUID      | Internal   |
| user_id    | UUID      | Internal   |
| token_hash | String    | Restricted |
| expires_at | Timestamp | Internal   |
| revoked_at | Timestamp | Internal   |
| created_at | Timestamp | Internal   |

---

## Rules

* Tokens are stored as hashes.
* Expired tokens are unusable.
* Revoked tokens cannot be reactivated.
* Rotation is mandatory after refresh.

---

# Entity: password_reset_token

## Purpose

Supports secure password recovery.

---

## Candidate Attributes

| Field       | Type      |
| ----------- | --------- |
| id          | UUID      |
| user_id     | UUID      |
| token_hash  | String    |
| expires_at  | Timestamp |
| consumed_at | Timestamp |
| created_at  | Timestamp |

---

## Rules

* Single use only.
* Short expiration period.
* Stored as hashes.
* Invalid after password change.

---

# Index Recommendations

| Entity               | Suggested Index |
| -------------------- | --------------- |
| app_user             | email           |
| app_user             | mobile_number   |
| app_user             | status          |
| refresh_token        | user_id         |
| refresh_token        | expires_at      |
| password_reset_token | expires_at      |

---

# API Exposure Matrix

| Field         | Public API | Internal API | Never |
| ------------- | ---------- | ------------ | ----- |
| full_name     | ✔          | ✔            |       |
| email         | ✔          | ✔            |       |
| mobile_number | ✔          | ✔            |       |
| password_hash |            |              | ✔     |
| token_hash    |            |              | ✔     |

---

# Acceptance Criteria

This document is complete when:

* Every Identity entity is documented.
* Every field has validation rules.
* Sensitive data is classified.
* API exposure is defined.
* Security review is completed.

---

# Next Document

**Repository Path**

`docs/database/DATA_DICTIONARY/CATALOG.md`

The Catalog Data Dictionary will define the complete data model for:

* Category
* Brand
* Collection
* Product
* Product Variant
* Product Image
* Product Collection

including field definitions, business rules, validation, indexing strategy, and ownership.
