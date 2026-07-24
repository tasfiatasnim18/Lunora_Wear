# Repository Path

`docs/runtime/REQUEST_PIPELINE.md`

---

# HTTP Request Pipeline

**Project:** Lunora Wear

**Document ID:** LW-RT-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/backend/BACKEND_ARCHITECTURE.md`
* `docs/api/API_STANDARDS.md`
* `docs/security/SECURITY_ARCHITECTURE.md`
* `docs/runtime/AUTHENTICATION_FLOW.md`
* `docs/runtime/AUTHORIZATION_FLOW.md`

---

# 1. Purpose

This document defines the lifecycle of every HTTP request entering the Lunora Wear platform.

The request pipeline provides:

* Predictable execution
* Centralized cross-cutting concerns
* Security
* Validation
* Observability
* Error handling

Every endpoint follows the same high-level pipeline.

---

# 2. Pipeline Overview

```text
Client
    │
HTTPS
    │
Reverse Proxy (Nginx)
    │
ASP.NET Core
    │
Middleware
    │
Routing
    │
Authentication
    │
Authorization
    │
Endpoint Binding
    │
Validation
    │
Application Handler
    │
Domain Model
    │
Infrastructure
    │
Database / External Systems
    │
Response Mapping
    │
Response Middleware
    │
Client
```

---

# 3. Reverse Proxy

Responsibilities:

* TLS termination
* Compression
* Static asset routing
* Request forwarding
* Security headers
* Request size limits

The reverse proxy should not implement business logic.

---

# 4. Middleware Execution

Typical middleware order:

1. Correlation ID
2. Request logging
3. Exception handling
4. Security headers
5. Authentication
6. Authorization
7. Rate limiting
8. Routing
9. Endpoint execution

Middleware order must be documented and tested.

---

# 5. Authentication

Responsibilities:

* Validate JWT
* Validate token lifetime
* Load user identity
* Attach principal

Failure terminates the pipeline with `401 Unauthorized`.

---

# 6. Authorization

Responsibilities:

* Evaluate permissions
* Verify roles
* Apply resource-based authorization where required

Failure terminates the pipeline with `403 Forbidden`.

---

# 7. Request Binding

Responsibilities:

* Deserialize JSON
* Bind route values
* Bind query parameters
* Bind headers
* Bind uploaded files

Binding errors return structured validation responses.

---

# 8. Validation

Validation occurs before business logic.

Validation includes:

* Required fields
* Formats
* Length
* Range
* Cross-field rules

Business invariants remain the responsibility of the Domain layer.

---

# 9. Application Execution

The Application layer:

* Starts the use case
* Coordinates collaborators
* Manages transactions
* Publishes domain events
* Maps results

Application code should not contain infrastructure-specific behavior.

---

# 10. Domain Execution

The Domain layer:

* Applies business rules
* Updates aggregates
* Produces domain events
* Preserves invariants

The Domain must remain independent of HTTP and persistence concerns.

---

# 11. Infrastructure

Infrastructure responsibilities include:

* Database persistence
* Cache interaction
* External providers
* File storage
* Messaging adapters

Infrastructure exceptions should be translated into domain-appropriate application errors.

---

# 12. Response Mapping

Responses should be mapped from application models to API contracts.

No persistence entities should be returned directly to API consumers.

---

# 13. Error Handling

Unhandled exceptions are converted into the standard API error model.

The pipeline must:

* Log the error
* Attach the correlation ID
* Return a sanitized response
* Avoid exposing implementation details

---

# 14. Observability

Each request should generate:

* Correlation ID
* Structured logs
* Timing metrics
* Trace information
* Error metrics (when applicable)

Observability must be consistent across all endpoints.

---

# 15. Acceptance Criteria

The request pipeline is complete when:

* Execution order is defined.
* Cross-cutting concerns are centralized.
* Error handling is standardized.
* Authentication and authorization are integrated.
* Observability is built into every request.

---

# Next Document

**Repository Path**

`docs/runtime/COMMAND_PIPELINE.md`

This document defines the lifecycle of command execution, including validation, transaction management, domain event publication, outbox processing, and post-commit actions.
