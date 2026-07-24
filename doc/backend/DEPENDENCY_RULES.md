# Repository Path

`docs/backend/DEPENDENCY_RULES.md`

---

# Dependency Rules

**Project:** Lunora Wear

**Document ID:** LW-BE-003

**Version:** 1.0.0

**Status:** Approved

**Owner:** Solution Architecture

**Related Documents**

* `docs/backend/BACKEND_ARCHITECTURE.md`
* `docs/backend/SOLUTION_STRUCTURE.md`
* `docs/domain/BOUNDED_CONTEXT_MAP.md`
* `docs/decisions/ADR-001_MODULAR_MONOLITH.md`

---

# 1. Purpose

This document defines the dependency rules governing the backend architecture.

Its objectives are to:

* Preserve Clean Architecture.
* Prevent architectural drift.
* Reduce coupling.
* Enable independent module evolution.
* Support future service extraction.

These rules are mandatory and should be enforced through automated architecture tests.

---

# 2. Dependency Principles

The backend follows these principles:

* Dependencies point inward.
* Business logic is independent of infrastructure.
* Modules communicate through explicit contracts.
* Cross-cutting concerns are centralized.
* Circular dependencies are prohibited.

---

# 3. Layer Dependency Matrix

| From           | May Depend On                                | Must Not Depend On                                        |
| -------------- | -------------------------------------------- | --------------------------------------------------------- |
| API            | Application, Contracts                       | Infrastructure implementation details, Domain persistence |
| Application    | Domain, SharedKernel, Contracts              | API, Infrastructure frameworks                            |
| Domain         | SharedKernel                                 | Application, API, Infrastructure                          |
| Infrastructure | Domain, Application, SharedKernel, Contracts | API                                                       |

The Domain layer must remain framework-independent.

---

# 4. Module Boundaries

Each bounded context owns:

* Domain model
* Application logic
* Persistence abstractions
* Business rules
* Internal events

Modules must not access another module's persistence implementation directly.

---

# 5. Cross-Module Communication

Preferred mechanisms:

1. Public application interfaces
2. Published integration events
3. Shared contracts (where appropriate)

Avoid:

* Direct database access across modules
* Repository access across modules
* Shared mutable state

---

# 6. Infrastructure Isolation

Infrastructure implementations must remain replaceable.

Examples:

* PostgreSQL → Repository implementation
* Redis → Cache implementation
* Cloudflare R2 → File storage adapter
* Payment Gateway → Payment provider adapter

Business logic should depend on abstractions rather than concrete technologies.

---

# 7. Dependency Inversion

High-level policies define interfaces.

Low-level implementations satisfy those interfaces.

Example:

```text id="qj3s8v"
Application
    │
    ▼
IProductRepository
    ▲
    │
Infrastructure (EF Core)
```

The interface belongs to the inner layer; the implementation belongs to the outer layer.

---

# 8. Shared Kernel Rules

The Shared Kernel may contain:

* Base entity abstractions
* Value object base classes
* Result types
* Common exceptions
* Shared utilities with broad applicability

It must not become a catch-all library.

Domain-specific logic belongs within its owning bounded context.

---

# 9. Contracts

The Contracts project contains:

* Public DTOs (where shared)
* Integration event schemas
* Versioned API contracts

Contracts should avoid references to infrastructure concerns.

---

# 10. Prohibited Dependencies

The following are not allowed:

* Domain → Entity Framework Core
* Domain → ASP.NET Core
* Domain → Redis
* Domain → HTTP
* Domain → Logging framework
* Application → Controllers
* Controllers → Repositories
* One bounded context → Another context's database implementation

Violations should fail architecture tests.

---

# 11. Architecture Test Rules

Automated tests should verify:

* Dependency direction.
* Layer isolation.
* Namespace conventions.
* Feature organization.
* Absence of circular references.
* Controller thinness.
* Infrastructure isolation.

These tests are part of continuous integration.

---

# 12. Exceptions

Any exception to these rules requires:

* Documented justification.
* Architecture review.
* Associated ADR.
* Defined expiration or review date if temporary.

---

# 13. Acceptance Criteria

This document is complete when:

* Dependency rules are explicit.
* Layer interactions are defined.
* Prohibited dependencies are documented.
* Architecture tests reflect these rules.
* Solution Architecture approves the dependency model.

---

# Next Document

**Repository Path**

`docs/backend/REQUEST_PIPELINE.md`

This document describes the end-to-end lifecycle of an HTTP request—from network ingress through middleware, authentication, authorization, validation, application execution, persistence, response mapping, and observability.
