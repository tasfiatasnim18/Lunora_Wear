# Repository Path

`docs/api/ERROR_MODEL.md`

---

# API Error Model

**Project:** Lunora Wear

**Document ID:** LW-API-ERR-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/api/API_DESIGN_PHILOSOPHY.md`
* `docs/api/API_STANDARDS.md`
* `docs/security/SECURITY_STANDARDS.md`

---

# 1. Purpose

This document defines the standard error response format used by every API exposed by the Lunora Wear platform.

Its objectives are to:

* Provide a predictable contract for API consumers.
* Support debugging and troubleshooting.
* Enable localization without changing response structure.
* Improve observability through correlation identifiers.
* Standardize validation and business-rule errors.

---

# 2. Design Principles

Every error response must be:

* Machine-readable
* Human-readable
* Consistent
* Traceable
* Secure

Error payloads must never expose implementation details such as SQL statements, stack traces, connection strings, or file paths.

---

# 3. Canonical Error Response

Every error response follows this structure:

```json
{
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "The requested order could not be found.",
    "correlationId": "9c2b4c77-5c3e-4f84-a0cf-5d2f8e1b4d2e",
    "details": [],
    "documentation": "/docs/errors/ORDER_NOT_FOUND"
  }
}
```

---

# 4. Response Fields

| Field         | Required | Description                           |
| ------------- | -------- | ------------------------------------- |
| code          | Yes      | Stable machine-readable identifier    |
| message       | Yes      | Human-readable summary                |
| correlationId | Yes      | Request trace identifier              |
| details       | No       | Validation or contextual details      |
| documentation | No       | Reference to troubleshooting guidance |

---

# 5. Error Categories

| Category       | Example                      |
| -------------- | ---------------------------- |
| Validation     | Missing required field       |
| Authentication | Invalid access token         |
| Authorization  | Insufficient permissions     |
| Resource       | Product not found            |
| Business Rule  | Coupon expired               |
| Conflict       | Duplicate email              |
| Rate Limiting  | Too many requests            |
| Infrastructure | Temporary dependency failure |
| Internal       | Unexpected server error      |

---

# 6. Validation Errors

Validation failures return a structured list of field-level issues.

Example:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "One or more validation errors occurred.",
    "correlationId": "9c2b4c77-5c3e-4f84-a0cf-5d2f8e1b4d2e",
    "details": [
      {
        "field": "email",
        "code": "INVALID_FORMAT",
        "message": "Email address is not valid."
      },
      {
        "field": "password",
        "code": "MIN_LENGTH",
        "message": "Password must contain at least 12 characters."
      }
    ]
  }
}
```

---

# 7. Business Rule Errors

Business rule violations should use specific error codes.

Examples:

* `COUPON_EXPIRED`
* `INSUFFICIENT_STOCK`
* `ORDER_ALREADY_CANCELLED`
* `PAYMENT_ALREADY_CAPTURED`

These errors are distinct from validation failures because the request format is valid but the requested action cannot be completed.

---

# 8. Correlation IDs

Every request receives a unique correlation identifier.

Requirements:

* Included in logs.
* Returned in error responses.
* Propagated across internal service calls where applicable.
* Accepted from trusted upstream systems when appropriate.

---

# 9. Localization

The `code` field is stable across languages.

The `message` field may be localized based on client preferences or future internationalization requirements.

---

# 10. Logging Requirements

Server logs should include:

* Correlation ID
* Error code
* Request path
* Authenticated user identifier (if available)
* Timestamp

Sensitive information must be excluded from logs.

---

# 11. HTTP Status Mapping

| HTTP Status | Typical Error Codes     |
| ----------- | ----------------------- |
| 400         | VALIDATION_FAILED       |
| 401         | INVALID_TOKEN           |
| 403         | ACCESS_DENIED           |
| 404         | RESOURCE_NOT_FOUND      |
| 409         | DUPLICATE_RESOURCE      |
| 422         | BUSINESS_RULE_VIOLATION |
| 429         | RATE_LIMIT_EXCEEDED     |
| 500         | INTERNAL_SERVER_ERROR   |
| 503         | SERVICE_UNAVAILABLE     |

Specific domain error codes should refine these generic categories.

---

# 12. Error Code Conventions

Error codes use uppercase snake case.

Examples:

* PRODUCT_NOT_FOUND
* EMAIL_ALREADY_EXISTS
* INVALID_COUPON
* CART_IS_EMPTY
* INVENTORY_UNAVAILABLE

Codes are treated as part of the public API contract and must remain stable.

---

# 13. Security Considerations

Never disclose:

* Stack traces
* SQL queries
* Database schema
* Internal IP addresses
* Authentication secrets
* Encryption keys

Internal diagnostic information belongs in logs, not API responses.

---

# 14. Acceptance Criteria

This document is complete when:

* Every API uses the standard error format.
* Error codes follow the documented conventions.
* Validation errors include field-level details.
* Correlation IDs are consistently returned.
* Security review confirms no sensitive implementation details are exposed.

---

# Next Document

**Repository Path**

`docs/api/PAGINATION_AND_FILTERING.md`

This document defines the canonical approach to pagination, filtering, sorting, searching, and field selection for collection endpoints across the platform.
