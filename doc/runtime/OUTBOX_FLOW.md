# Repository Path

`docs/runtime/OUTBOX_FLOW.md`

---

# Outbox Flow

**Project:** Lunora Wear

**Document ID:** LW-RT-006

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/runtime/DOMAIN_EVENT_FLOW.md`
* `docs/runtime/TRANSACTION_LIFECYCLE.md`
* `docs/backend/OUTBOX_PATTERN.md`
* `docs/runtime/FAILURE_RECOVERY.md`

---

# 1. Purpose

This document defines how integration events are delivered reliably using the Outbox Pattern.

Objectives:

* Eliminate lost events
* Prevent inconsistent state
* Support retries
* Enable future asynchronous integrations
* Improve operational observability

The Outbox Pattern guarantees that business state changes and integration event persistence occur atomically.

---

# 2. Why an Outbox Exists

Without an Outbox:

```text
Save Order
      ✓

Publish Event
      ✗

Result:
Database updated
Other systems never notified
```

With an Outbox:

```text
Save Order
      ✓

Save Outbox Record
      ✓

Commit Transaction
      ✓

Background Publisher
      ↓

Publish Event
```

The event cannot be permanently lost due to temporary infrastructure failures after the database commit.

---

# 3. High-Level Flow

```text
HTTP Request
      │
Command Handler
      │
Aggregate Raises Domain Event
      │
Transaction Begins
      │
Persist Aggregate
      │
Create Integration Event
      │
Persist Outbox Record
      │
Commit Transaction
      │
Background Publisher
      │
External Provider
      │
Mark Published
```

---

# 4. Outbox Record

Each Outbox entry should contain:

* Unique identifier
* Event identifier
* Event type
* Event version
* Aggregate identifier
* Aggregate type
* Correlation ID
* Causation ID (if applicable)
* Serialized payload
* Created timestamp (UTC)
* Publish status
* Publish attempts
* Last attempt timestamp
* Published timestamp (when successful)
* Error summary (latest failure only)

Sensitive data should not be stored unless required and approved.

---

# 5. Publisher Worker

The publisher runs independently of API requests.

Responsibilities:

* Poll pending Outbox records
* Publish integration events
* Handle transient failures
* Update publish status
* Record metrics
* Respect retry policies

Multiple workers may execute concurrently provided duplicate publication is prevented or safely tolerated.

---

# 6. Delivery Guarantees

The platform targets:

**At-Least-Once Delivery**

Implications:

* Duplicate delivery is possible.
* Consumers must be idempotent.
* Event identifiers should be globally unique.

Exactly-once delivery is not assumed.

---

# 7. Retry Strategy

Retry only transient failures.

Examples:

* Temporary network interruption
* Provider timeout
* Rate limiting
* Temporary service unavailability

Do not automatically retry:

* Invalid payload
* Unsupported event version
* Permanent authorization failure
* Business contract violations

Retry policy parameters should be configurable rather than hard-coded.

---

# 8. Failure Handling

After exceeding retry thresholds:

* Mark the record as failed.
* Preserve the payload.
* Raise operational alerts.
* Enable manual replay after investigation.

The original business transaction remains committed.

---

# 9. Replay

Replay capabilities should support:

* Single event replay
* Batch replay
* Replay by date range
* Replay by event type
* Replay by aggregate identifier

Replay operations must be authenticated, authorized, and fully audited.

---

# 10. Ordering

Ordering guarantees exist only within the same aggregate where supported by the publishing implementation.

Consumers must not depend on global ordering across unrelated aggregates.

---

# 11. Monitoring

Operational metrics include:

* Pending Outbox records
* Publish throughput
* Success rate
* Retry rate
* Failed publications
* Oldest pending record age
* Average publish latency

Thresholds should trigger alerts before failures accumulate significantly.

---

# 12. Operational Runbook

Support personnel should be able to:

* View pending events
* View failed events
* Retry failed publications
* Pause publishing during incidents
* Resume publishing safely
* Audit publication history

Administrative actions must be logged.

---

# 13. Security

Integration events must:

* Contain only required data.
* Avoid secrets.
* Avoid internal implementation details.
* Respect data classification policies.
* Be encrypted in transit.

Access to Outbox management interfaces must be restricted.

---

# 14. Acceptance Criteria

This document is complete when:

* Reliable delivery strategy is defined.
* Retry behavior is documented.
* Replay process is specified.
* Monitoring requirements are established.
* Operational controls are documented.

---

# Next Document

**Repository Path**

`docs/runtime/AUTHENTICATION_FLOW.md`

This document defines the complete authentication lifecycle, including login, token issuance, refresh token rotation, logout, session management, and security controls.
