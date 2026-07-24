# Repository Path

`docs/architecture/CONTEXT_DIAGRAM.md`

---

# System Context

**Project:** Lunora Wear

**Document ID:** LW-ARCH-002

**Version:** 1.0.0

**Status:** Approved

**Owner:** Solution Architecture

**Related Documents**

* `docs/architecture/ARCHITECTURE_OVERVIEW.md`
* `docs/architecture/CONTAINER_DIAGRAM.md`
* `docs/security/SECURITY_ARCHITECTURE.md`

---

# 1. Purpose

The System Context diagram defines the highest-level view of the Lunora Wear platform.

It answers:

* Who uses the system?
* Which external systems interact with it?
* What trust boundaries exist?
* What information flows across those boundaries?

Implementation details are intentionally omitted.

---

# 2. System Scope

The Lunora Wear platform provides:

* Online product discovery
* Shopping cart
* Checkout
* Order management
* Customer account management
* Administrative operations
* Inventory management
* Promotions and marketing

The platform is responsible for business logic, while payments, messaging, and logistics rely on external providers.

---

# 3. Primary Actors

## Customer

Responsibilities:

* Browse products
* Place orders
* Track shipments
* Manage profile
* Submit reviews

---

## Administrator

Responsibilities:

* Manage catalog
* Manage inventory
* Process orders
* Configure promotions
* Review reports

---

## Customer Support

Responsibilities:

* Resolve customer issues
* Review order history
* Process approved returns
* Assist with account management

---

# 4. External Systems

## Payment Gateway

Purpose:

* Authorize payments
* Capture payments
* Process refunds
* Send payment notifications

---

## Shipping Provider

Purpose:

* Create shipments
* Retrieve tracking events
* Estimate delivery

---

## Email Service

Purpose:

* Account verification
* Password reset
* Order confirmation
* Marketing communication

---

## SMS Provider

Purpose:

* One-time passwords
* Delivery notifications
* Security alerts

---

## Object Storage

Purpose:

* Product images
* Marketing assets
* Customer uploads

---

## CDN

Purpose:

* Global content delivery
* Static asset caching
* Performance optimization

---

# 5. Trust Boundaries

The architecture distinguishes the following trust zones:

### Public Internet

* Customer browser
* Mobile applications

---

### Application Boundary

* Web applications
* API layer
* Authentication

---

### Internal Services

* Business logic
* Database
* Cache
* Background processing

---

### External Provider Boundary

* Payment provider
* Shipping provider
* Email provider
* SMS provider

Every crossing of a trust boundary requires authentication, authorization, validation, and logging appropriate to the risk.

---

# 6. High-Level Interaction Flow

1. Customer accesses the web application.
2. Web application communicates with the API.
3. API executes business logic.
4. Business logic accesses the database and cache.
5. External providers are contacted when required.
6. Results are returned to the client.

Long-running processes are delegated to background workers where appropriate.

---

# 7. Responsibilities

| Component         | Primary Responsibility       |
| ----------------- | ---------------------------- |
| Customer          | Commerce interactions        |
| Admin             | Platform management          |
| API               | Business capability exposure |
| Database          | Persistent storage           |
| Cache             | Performance optimization     |
| Payment Gateway   | Payment processing           |
| Shipping Provider | Fulfillment support          |
| Email Service     | Transactional messaging      |
| CDN               | Static content delivery      |

---

# 8. Non-Goals

The context diagram does not describe:

* Internal code structure
* Database schema
* Deployment topology
* API endpoints
* UI layouts

Those concerns are addressed in lower-level architecture documents.

---

# 9. Quality Attributes

The context architecture should support:

* Security
* Availability
* Scalability
* Maintainability
* Clear system boundaries

---

# 10. Acceptance Criteria

This document is complete when:

* All external actors are identified.
* External integrations are documented.
* Trust boundaries are defined.
* System responsibilities are clearly separated.
* Architecture review is complete.

---

# Next Document

**Repository Path**

`docs/architecture/CONTAINER_DIAGRAM.md`

This document decomposes the platform into deployable runtime containers, defining the responsibilities and interactions of the web applications, backend API, database, cache, background workers, object storage, and external integrations.
