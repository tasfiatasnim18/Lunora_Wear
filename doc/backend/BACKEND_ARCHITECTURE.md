# Repository Path

`docs/backend/BACKEND_ARCHITECTURE.md`

---

# Backend Architecture

**Project:** Lunora Wear

**Document ID:** LW-BE-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Backend Architecture

**Related Documents**

* `docs/architecture/ARCHITECTURE_OVERVIEW.md`
* `docs/domain/DOMAIN_MODEL.md`
* `docs/api/API_STANDARDS.md`
* `docs/database/PHYSICAL_DATABASE_DESIGN.md`

---

# 1. Purpose

This document defines the architecture of the Lunora Wear backend.

It specifies:

* Architectural style
* Layer responsibilities
* Dependency rules
* Communication patterns
* Extension points
* Evolution strategy

It does not define business features or endpoint behavior.

---

# 2. Technology Stack

| Layer                 | Technology                                     |
| --------------------- | ---------------------------------------------- |
| Runtime               | ASP.NET Core                                   |
| Language              | C#                                             |
| ORM                   | Entity Framework Core                          |
| Database              | PostgreSQL                                     |
| Cache                 | Redis                                          |
| Background Processing | Hosted Services (future scheduler if required) |
| Object Storage        | Cloudflare R2                                  |
| Authentication        | JWT + Refresh Tokens                           |

Technology choices are governed by Architecture Decision Records (ADRs).

---

# 3. Architectural Style

The backend follows:

* Modular Monolith
* Clean Architecture
* Domain-Driven Design
* Dependency Injection
* Repository Pattern (where appropriate)
* CQRS for complex read/write separation
* Event-driven integration through the Outbox Pattern

---

# 4. Layered Architecture

The backend is organized into four logical layers:

### Presentation

Responsibilities:

* HTTP endpoints
* Request binding
* Authentication
* Authorization
* Response mapping

No business logic.

---

### Application

Responsibilities:

* Use cases
* Command handling
* Query handling
* Validation
* Transaction orchestration

Coordinates domain objects but does not contain persistence details.

---

### Domain

Responsibilities:

* Business entities
* Value objects
* Domain services
* Business rules
* Domain events

The domain must remain independent of infrastructure.

---

### Infrastructure

Responsibilities:

* Database access
* External integrations
* Caching
* File storage
* Email
* Payment providers
* Logging implementations

Infrastructure depends on abstractions defined by inner layers.

---

# 5. Dependency Rule

Dependencies always point inward.

```text
Presentation
      ↓
Application
      ↓
Domain

Infrastructure ─────► Domain
Infrastructure ─────► Application
```

The Domain layer must not reference:

* ASP.NET Core
* Entity Framework Core
* Redis
* PostgreSQL
* HTTP
* Logging frameworks

---

# 6. Module Organization

Modules should align with bounded contexts.

Examples:

* Identity
* Catalog
* Inventory
* Shopping
* Orders
* Payments
* Shipping
* Marketing

Each module owns:

* Commands
* Queries
* Domain model
* Persistence abstractions
* Application services

---

# 7. Request Flow

Typical request lifecycle:

1. HTTP request
2. Authentication
3. Authorization
4. Validation
5. Application use case
6. Domain execution
7. Persistence
8. Event publication (if applicable)
9. Response mapping
10. HTTP response

Cross-cutting concerns should be centralized.

---

# 8. Cross-Cutting Concerns

Centralized concerns include:

* Logging
* Validation
* Authorization
* Exception handling
* Metrics
* Tracing
* Configuration

Business modules should not duplicate these capabilities.

---

# 9. Transaction Management

Application services define transaction boundaries.

Transactions should:

* Be short-lived
* Be atomic
* Avoid external network calls while open

Long-running workflows should use asynchronous coordination where appropriate.

---

# 10. Background Processing

Background processing is responsible for:

* Email delivery
* Notification dispatch
* Scheduled cleanup
* Outbox processing
* Retry handling

Background workers should be idempotent.

---

# 11. Scalability

The backend should support:

* Horizontal application scaling
* Stateless request processing
* Independent cache scaling
* Future service extraction by bounded context

Scalability decisions should be guided by operational metrics.

---

# 12. Quality Attributes

The architecture prioritizes:

* Maintainability
* Testability
* Reliability
* Security
* Performance
* Evolvability

Trade-offs should be documented in ADRs.

---

# 13. Acceptance Criteria

This document is complete when:

* Architectural style is documented.
* Layer responsibilities are defined.
* Dependency rules are explicit.
* Module boundaries align with bounded contexts.
* Backend architecture review is complete.

---

# Next Document

**Repository Path**

`docs/backend/SOLUTION_STRUCTURE.md`

This document defines the physical organization of the ASP.NET Core solution, including projects, assemblies, namespaces, module boundaries, and build dependencies.
