# Repository Path

`docs/runtime/DOMAIN_EVENT_FLOW.md`

---

# Domain Event Flow

**Project:** Lunora Wear

**Document ID:** LW-RT-005

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/runtime/TRANSACTION_LIFECYCLE.md`
* `docs/backend/DOMAIN_EVENTS.md`
* `docs/backend/OUTBOX_PATTERN.md`
* `docs/domain/DOMAIN_MODEL.md`

---

# 1. Purpose

This document defines how domain events are created, collected, persisted, and transformed into integration events.

The goals are to:

* Preserve domain integrity
* Decouple business capabilities
* Support reliable integration
* Enable future microservice extraction
* Improve auditability

---

# 2. Event Categories

The platform distinguishes three event types.

## Domain Events

Represent business facts that occurred within the domain.

Examples:

* OrderPlaced
* InventoryReserved
* CouponApplied
* CustomerRegistered

Characteristics:

* Internal
* Synchronous collection
* Raised by aggregates
* Never published directly to external systems

---

## Integration Events

Represent information intended for external consumers.

Examples:

* OrderCreatedIntegrationEvent
* PaymentCompletedIntegrationEvent
* ShipmentDispatchedIntegrationEvent

Characteristics:

* Stable contract
* Versioned
* Published asynchronously
* Persisted through the Outbox

---

## Notification Events

Represent user-facing communication triggers.

Examples:

* SendWelcomeEmail
* SendOrderConfirmation
* SendShipmentNotification

Characteristics:

* Asynchronous
* Retryable
* Non-blocking
* Operational rather than domain-critical

---

# 3. Event Lifecycle

```text
Aggregate
    │
Raise Domain Event
    │
Collect During Transaction
    │
Commit Transaction
    │
Map to Integration Events
    │
Persist Outbox Messages
    │
Background Publisher
    │
External Systems
```

---

# 4. Event Creation

Only aggregates may create domain events.

Application services coordinate execution but should not manufacture domain events that represent business facts.

---

# 5. Event Collection

Aggregates maintain an internal collection of pending events.

These events remain in memory until the transaction completes successfully.

If the transaction rolls back, the pending events are discarded.

---

# 6. Event Dispatch

After a successful commit:

1. Domain events are collected.
2. Integration events are created where appropriate.
3. Integration events are written to the Outbox.
4. Background workers publish the events.

This sequence prevents publication of events for failed transactions.

---

# 7. Event Versioning

Integration events are versioned independently of internal domain events.

Rules:

* Avoid breaking changes.
* Prefer additive evolution.
* Support coexistence of multiple versions when required.
* Document deprecation timelines.

---

# 8. Event Naming

Domain events use the past tense.

Examples:

* OrderPlaced
* OrderCancelled
* InventoryAdjusted
* CustomerAddressUpdated

Names describe facts that have already occurred.

---

# 9. Event Payload

Payloads should include:

* Event identifier
* Event version
* Aggregate identifier
* Event timestamp (UTC)
* Correlation ID
* Relevant business data

Do not expose internal persistence structures or implementation-specific details.

---

# 10. Event Ordering

Ordering guarantees apply only within a single aggregate.

Consumers must not assume a global ordering across unrelated aggregates.

---

# 11. Failure Handling

If event publication fails:

* The Outbox record remains pending.
* Retry policies apply.
* Operational alerts may be raised after configured thresholds.
* Duplicate delivery must be tolerated by consumers.

Consumers should be designed to be idempotent.

---

# 12. Monitoring

Track:

* Events created
* Events published
* Retry count
* Failed publications
* Publication latency
* Dead-letter occurrences (if applicable)

These metrics should feed operational dashboards.

---

# 13. Acceptance Criteria

This document is complete when:

* Event categories are defined.
* Lifecycle is documented.
* Versioning strategy is established.
* Failure handling is specified.
* Monitoring requirements are defined.

---

# Next Document

**Repository Path**

`docs/runtime/OUTBOX_FLOW.md`

This document specifies the reliable delivery mechanism for integration events, including Outbox persistence, publisher workers, retry policies, dead-letter handling, and operational monitoring.
