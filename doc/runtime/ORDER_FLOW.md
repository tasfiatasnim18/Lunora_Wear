# Repository Path

`docs/runtime/ORDER_FLOW.md`

---

# Order Flow

**Project:** Lunora Wear

**Document ID:** LW-RT-012

**Version:** 1.0.0

**Status:** Approved

**Owner:** Commerce Engineering

**Related Documents**

* `docs/runtime/PAYMENT_FLOW.md`
* `docs/runtime/COMMAND_PIPELINE.md`
* `docs/runtime/DOMAIN_EVENT_FLOW.md`
* `docs/state-machines/ORDER_STATE_MACHINE.md`
* `docs/domain/DOMAIN_MODEL.md`

---

# 1. Purpose

This document defines the complete lifecycle of an order from checkout through fulfillment, delivery, returns, cancellations, and archival.

Orders represent the central business transaction within the platform.

---

# 2. Order Principles

Orders follow these principles:

* An order represents a customer commitment.
* Order history is immutable.
* State transitions are explicit.
* Every transition is auditable.
* Payment and shipment evolve independently but remain coordinated with the order.

---

# 3. Order Lifecycle

```text
Shopping Cart
      │
Checkout
      │
Order Created
      │
Payment Processing
      │
Inventory Reserved
      │
Ready for Fulfillment
      │
Picking
      │
Packing
      │
Shipment Created
      │
In Transit
      │
Delivered
      │
Completed
```

Alternative paths:

* Cancelled
* Returned
* Partially Returned
* Failed Fulfillment

---

# 4. Order Creation

Order creation performs:

* Customer validation
* Address validation
* Inventory reservation
* Pricing verification
* Coupon validation
* Tax calculation (where applicable)
* Shipping calculation
* Payment initiation

The order receives a unique business identifier before external processing begins.

---

# 5. Inventory Reservation

Inventory should be reserved before payment capture where business policy requires it.

Reservation includes:

* Product
* Variant
* Quantity
* Reservation timestamp
* Reservation expiration (if applicable)

Expired reservations are released automatically.

---

# 6. Payment Coordination

Order and payment remain separate aggregates.

Rules:

* Successful payment advances the order.
* Failed payment does not automatically delete the order.
* Payment retries may be supported according to business policy.

---

# 7. Fulfillment

Fulfillment stages include:

* Awaiting fulfillment
* Picking
* Packing
* Quality check
* Shipment creation

Operational teams may update fulfillment status without modifying the original order data.

---

# 8. Shipment Coordination

A shipment references one or more order items.

Shipments maintain independent tracking information.

Multiple shipments may fulfill a single order.

---

# 9. Delivery

Delivery confirmation may originate from:

* Shipping provider
* Internal logistics
* Manual administrative confirmation (where permitted)

Delivery events update the order lifecycle but do not alter historical records.

---

# 10. Cancellation

Cancellation may occur before defined fulfillment milestones.

Cancellation processing includes:

* Release reserved inventory
* Reverse payment where applicable
* Publish domain events
* Notify customer

Cancellation reasons should be recorded for analytics.

---

# 11. Returns

Return processing supports:

* Full return
* Partial return
* Exchange workflow (future)

Return requests require:

* Eligibility validation
* Reason capture
* Approval workflow (if applicable)

Returns remain independently auditable.

---

# 12. Audit Trail

Every order transition records:

* Previous state
* New state
* Timestamp (UTC)
* Actor
* Correlation ID
* Reason (where applicable)

Order history must be append-only.

---

# 13. Domain Events

Typical events include:

* OrderCreated
* OrderConfirmed
* InventoryReserved
* PaymentCompleted
* ShipmentCreated
* OrderDelivered
* OrderCancelled
* ReturnRequested
* ReturnApproved
* RefundCompleted

These events support downstream processes and integrations.

---

# 14. Monitoring

Operational metrics include:

* Orders created
* Checkout conversion
* Payment success rate
* Fulfillment time
* Delivery time
* Cancellation rate
* Return rate
* Average order value
* Order completion time

These metrics support operational dashboards and business reporting.

---

# 15. Security

Order operations must:

* Enforce authorization.
* Protect customer data.
* Restrict administrative actions.
* Record privileged modifications.
* Prevent unauthorized access to order history.

---

# 16. Acceptance Criteria

This document is complete when:

* Order lifecycle is documented.
* Inventory, payment, and shipment coordination are defined.
* Audit requirements are established.
* Domain events are identified.
* Monitoring and security requirements are documented.

---

# Next Document

**Repository Path**

`docs/runtime/OBSERVABILITY_FLOW.md`

This document defines the platform's observability model, including structured logging, distributed tracing, metrics, dashboards, alerting, and operational diagnostics.
