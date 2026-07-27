# Repository Path

`docs/runtime/COMMAND_PIPELINE.md`

---

# Command Pipeline

**Project:** Lunora Wear

**Document ID:** LW-RT-002

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/runtime/REQUEST_PIPELINE.md`
* `docs/runtime/TRANSACTION_LIFECYCLE.md`
* `docs/backend/CQRS_GUIDELINES.md`
* `docs/backend/DOMAIN_EVENTS.md`
* `docs/backend/OUTBOX_PATTERN.md`

---

# 1. Purpose

This document defines the lifecycle of command execution for all operations that modify system state.

Examples include:

* Create Product
* Update Inventory
* Add to Cart
* Place Order
* Cancel Order
* Apply Coupon

Every command follows the same execution model to ensure consistency, transactional integrity, and observability.

---

# 2. Command Principles

Commands:

* Represent an intention to change state.
* Are task-oriented rather than CRUD-oriented.
* Execute exactly one business use case.
* Return success, failure, or minimal output.
* Must be idempotent where duplicate execution is possible.

Commands should not be used for data retrieval beyond what is required to complete the operation.

---

# 3. Pipeline Overview

```text
HTTP Request
    │
Authentication
    │
Authorization
    │
Command Binding
    │
Input Validation
    │
Business Validation
    │
Transaction Start
    │
Domain Execution
    │
Persist Changes
    │
Collect Domain Events
    │
Commit Transaction
    │
Write Outbox Messages
    │
Post-Commit Actions
    │
Response
```

---

# 4. Command Components

Each command consists of:

* Command object
* Validator
* Handler
* Domain model interaction
* Result mapping

Supporting services may be injected through abstractions.

---

# 5. Validation Stages

Validation occurs in two stages.

## Input Validation

Examples:

* Required fields
* Length
* Format
* Numeric ranges
* Enumeration values

Input validation should fail fast before business logic executes.

---

## Business Validation

Examples:

* Product exists.
* Coupon is active.
* Stock is available.
* Customer account is active.
* Order is cancellable.

Business validation belongs to the domain or application layer, depending on whether it enforces an invariant or coordinates external state.

---

# 6. Transaction Boundary

A single command executes within one application transaction.

The transaction begins immediately before domain execution and ends after all persistence operations succeed.

External network calls should not occur while the database transaction remains open unless explicitly justified.

---

# 7. Domain Interaction

The handler invokes domain entities, aggregates, and services to enforce business rules.

Domain entities are responsible for:

* Maintaining invariants
* Raising domain events
* Protecting aggregate consistency

Application handlers coordinate but do not duplicate business logic.

---

# 8. Persistence

Repositories persist aggregate changes.

Persistence should occur only after successful domain execution.

Repositories should not contain business rules.

---

# 9. Domain Events

Aggregates may emit domain events during execution.

Examples:

* OrderPlaced
* InventoryReserved
* PaymentCaptured
* ShipmentCreated

Events are collected during the transaction and published only after a successful commit.

---

# 10. Outbox Integration

After committing the transaction:

* Integration events are written to the Outbox.
* Background workers publish events to external systems.
* Failed publications are retried according to the retry policy.

This guarantees reliable event delivery without compromising transactional consistency.

---

# 11. Post-Commit Actions

Typical post-commit activities include:

* Send confirmation email
* Dispatch notifications
* Update search index
* Refresh cache
* Trigger analytics events

Post-commit work must not affect the success of the original command.

---

# 12. Error Handling

Failures are categorized as:

* Validation failures
* Business rule violations
* Concurrency conflicts
* Infrastructure failures
* Unexpected exceptions

Errors are translated into the standard API error model and logged with the request correlation ID.

---

# 13. Observability

Each command execution records:

* Command name
* Duration
* Success or failure
* Transaction outcome
* Retry count (if applicable)
* Correlation ID

Metrics should support operational dashboards and alerting.

---

# 14. Acceptance Criteria

This document is complete when:

* Command lifecycle is fully defined.
* Transaction boundaries are explicit.
* Domain event handling is documented.
* Outbox integration is specified.
* Observability requirements are defined.

---

# Next Document

**Repository Path**

`docs/runtime/QUERY_PIPELINE.md`

This document defines the execution model for read operations, including query handling, caching, pagination, projections, authorization, and response optimization.
