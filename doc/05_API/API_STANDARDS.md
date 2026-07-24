# Repository Path

`docs/api/API_STANDARDS.md`

---

# API Standards

**Project:** Lunora Wear

**Document ID:** LW-API-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/product/FEATURE_CATALOG.md`
* `docs/database/POSTGRESQL_SCHEMA.md`
* `docs/security/SECURITY_STANDARDS.md`

---

# 1. Purpose

This document defines the standards that every HTTP API exposed by Lunora Wear must follow.

The goals are:

* Consistency
* Predictability
* Security
* Maintainability
* Backward compatibility
* Developer experience

These rules apply to:

* Public APIs
* Customer APIs
* Admin APIs
* Internal service APIs

---

# 2. Architectural Style

Primary API style:

**REST**

Characteristics:

* Resource-oriented
* Stateless
* Cache-aware
* JSON request/response
* HTTPS only

GraphQL or gRPC may be introduced later for specific use cases but are outside the MVP scope.

---

# 3. Base URL

Example:

```text
https://api.lunorawear.com/v1/
```

Version is part of the URL for externally consumed APIs.

---

# 4. Resource Naming

Use plural nouns.

Examples:

```text
GET /products
GET /categories
GET /orders
GET /users
```

Avoid verbs in resource names.

Good:

```text
POST /orders
```

Avoid:

```text
POST /createOrder
```

---

# 5. HTTP Methods

| Method | Purpose                                            |
| ------ | -------------------------------------------------- |
| GET    | Retrieve                                           |
| POST   | Create                                             |
| PUT    | Full replacement                                   |
| PATCH  | Partial update                                     |
| DELETE | Remove or deactivate (according to business rules) |

Methods must be idempotent where defined by the HTTP specification.

---

# 6. Status Codes

| Code | Meaning                 |
| ---- | ----------------------- |
| 200  | Success                 |
| 201  | Created                 |
| 202  | Accepted                |
| 204  | No Content              |
| 400  | Validation Error        |
| 401  | Unauthorized            |
| 403  | Forbidden               |
| 404  | Not Found               |
| 409  | Conflict                |
| 422  | Business Rule Violation |
| 429  | Too Many Requests       |
| 500  | Internal Server Error   |
| 503  | Service Unavailable     |

Avoid returning `200 OK` for failed operations.

---

# 7. JSON Conventions

Property names use:

```json
{
  "productId": "...",
  "productName": "...",
  "createdAt": "..."
}
```

API payloads use **camelCase**.

Database naming remains **snake_case**.

Mapping between the two occurs in the application layer.

---

# 8. Request Validation

Validation occurs before business logic.

Validation includes:

* Required fields
* Type validation
* Length
* Range
* Format
* Cross-field validation

Invalid requests return structured validation errors.

---

# 9. Response Envelope

Successful responses follow a consistent structure.

Example:

```json
{
  "data": {
    "...": "..."
  },
  "meta": {
    "...": "..."
  }
}
```

Error responses follow the standard error model defined separately.

---

# 10. Pagination

Collection endpoints must support pagination.

Recommended parameters:

```text
?page=1&pageSize=20
```

Response metadata includes:

* page
* pageSize
* totalItems
* totalPages

Cursor-based pagination may be introduced for high-volume resources.

---

# 11. Filtering

Filtering should use query parameters.

Example:

```text
GET /products?category=hoodies&brand=lunora
```

Filtering syntax must be consistent across all endpoints.

---

# 12. Sorting

Example:

```text
?sort=price
?sort=-createdAt
```

A leading `-` indicates descending order.

---

# 13. Authentication

Protected endpoints require JWT access tokens.

Authentication details are documented separately.

Refresh tokens must never be transmitted as query parameters.

---

# 14. Authorization

Authorization is role- and permission-based.

Access decisions are evaluated after authentication and before business logic.

---

# 15. Idempotency

Sensitive POST operations (for example, payment initiation) should support idempotency keys to prevent accidental duplicate processing.

---

# 16. Error Handling

Every error response must include:

* Machine-readable error code
* Human-readable message
* Correlation identifier
* Validation details (when applicable)

Error payload structure is defined in `ERROR_MODEL.md`.

---

# 17. Versioning

External APIs use URI versioning.

Example:

```text
/v1/products
```

Breaking changes require a new major API version.

---

# 18. Deprecation

Deprecated endpoints must:

* Be documented.
* Include a deprecation notice.
* Provide a migration path.
* Remain available for the agreed support period.

---

# 19. Observability

Every request should support:

* Correlation ID
* Structured logging
* Metrics
* Distributed tracing (future)

Sensitive data must never be logged.

---

# 20. Acceptance Criteria

This document is complete when:

* HTTP conventions are defined.
* Resource naming is standardized.
* Versioning policy is documented.
* Response conventions are established.
* Engineering governance approves the standards.

---

# Next Document

**Repository Path**

`docs/api/ERROR_MODEL.md`

The Error Model document will define the canonical error response format, error codes, validation payloads, localization strategy, correlation identifiers, and troubleshooting guidance used across every API.
