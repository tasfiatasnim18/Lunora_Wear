# Solution Structure

**Project:** Lunora Wear

**Document ID:** LW-BE-002

**Version:** 1.0.0

**Status:** Approved

**Owner:** Backend Architecture

**Related Documents**

* `docs/backend/BACKEND_ARCHITECTURE.md`
* `docs/backend/DEPENDENCY_RULES.md`
* `docs/domain/BOUNDED_CONTEXT_MAP.md`

---

# 1. Purpose

This document defines the physical organization of the ASP.NET Core solution.

The structure is designed to:

* Reflect bounded contexts
* Enforce architectural boundaries
* Support independent testing
* Minimize coupling
* Improve discoverability
* Enable future module extraction

---

# 2. Solution Layout

```text
src/
├── Lunora.Api/
├── Lunora.Application/
├── Lunora.Domain/
├── Lunora.Infrastructure/
├── Lunora.SharedKernel/
├── Lunora.Contracts/
└── Lunora.ArchitectureTests/

tests/
├── Lunora.UnitTests/
├── Lunora.IntegrationTests/
├── Lunora.ContractTests/
└── Lunora.EndToEndTests/
```

---

# 3. Project Responsibilities

## Lunora.Api

Responsibilities:

* HTTP endpoints
* Authentication
* Authorization
* Middleware
* OpenAPI
* Request/response mapping

Must not contain business logic.

---

## Lunora.Application

Responsibilities:

* Commands
* Queries
* Use cases
* Validation
* Transaction orchestration
* Application services

Depends only on Domain and Shared Kernel abstractions.

---

## Lunora.Domain

Responsibilities:

* Entities
* Value objects
* Domain services
* Domain events
* Business rules

Must have no dependency on ASP.NET Core, Entity Framework Core, or external infrastructure.

---

## Lunora.Infrastructure

Responsibilities:

* Entity Framework Core
* PostgreSQL
* Redis
* File storage
* Email
* Payment gateway
* External APIs

Implements interfaces defined by inner layers.

---

## Lunora.SharedKernel

Contains cross-cutting building blocks shared across bounded contexts, such as:

* Base entity abstractions
* Result types
* Common exceptions
* Specifications (if adopted)
* Value object base classes

The Shared Kernel should remain intentionally small to avoid becoming a dumping ground.

---

## Lunora.Contracts

Contains public contracts shared across boundaries, including:

* API DTOs (where appropriate)
* Integration event contracts
* Shared enums intended for consumers

Contracts should evolve under version control and remain backward compatible where required.

---

## Lunora.ArchitectureTests

Contains automated architectural tests.

Examples:

* Domain does not reference Infrastructure.
* Application does not reference ASP.NET Core.
* Controllers remain thin.
* Modules do not violate dependency rules.

Architecture tests are part of the CI pipeline.

---

# 4. Module Structure

Each bounded context follows a consistent layout.

```text
Catalog/
├── Domain/
├── Application/
│   └── Features/
├── Infrastructure/
└── Contracts/
```

Features should be organized vertically by use case rather than horizontally by technical type.

---

# 5. Namespace Conventions

Namespaces mirror the folder structure.

Example:

```text
Lunora.Application.Catalog.Features.CreateProduct
```

Avoid namespaces that obscure feature ownership.

---

# 6. Build Dependencies

Allowed dependency flow:

```text
Api
 ↓
Application
 ↓
Domain

Infrastructure ─────► Application
Infrastructure ─────► Domain

SharedKernel ◄──── all projects
Contracts ◄──── Api / Application / Infrastructure
```

Circular dependencies are prohibited.

---

# 7. Testing Strategy

Testing mirrors the solution structure:

* Unit tests validate domain and application logic.
* Integration tests validate infrastructure.
* Contract tests validate API compatibility.
* End-to-end tests validate business workflows.

Every production project should have corresponding test coverage.

---

# 8. Evolution Strategy

New bounded contexts should be added without restructuring existing modules.

The solution should accommodate future extraction into independent services if justified by operational needs.

---

# 9. Acceptance Criteria

This document is complete when:

* Project responsibilities are defined.
* Dependency flow is documented.
* Module layout is standardized.
* Architecture tests are planned.
* Solution architecture review is complete.

---

# Next Document

**Repository Path**

`docs/backend/DEPENDENCY_RULES.md`

This document formally specifies the allowed and prohibited dependencies between projects, layers, modules, and bounded contexts, along with the automated architecture tests that enforce those rules.
