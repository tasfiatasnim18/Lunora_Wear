# Repository Path

`docs/infrastructure/INFRASTRUCTURE_OVERVIEW.md`

---

# Infrastructure Overview

**Project:** Lunora Wear

**Document ID:** LW-INF-001

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Classification:** Architecture

**Related Documents**

* `docs/architecture/SYSTEM_CONTEXT.md`
* `docs/security/SECURITY_ARCHITECTURE.md`
* `docs/runtime/REQUEST_PIPELINE.md`
* `docs/infrastructure/DEPLOYMENT_ARCHITECTURE.md`

---

# 1. Purpose

This document provides a high-level overview of the infrastructure architecture supporting the Lunora Wear platform.

It defines the major infrastructure components, deployment boundaries, operational responsibilities, and architectural principles that guide all infrastructure decisions.

---

# 2. Objectives

The infrastructure architecture shall:

* Provide reliable application hosting.
* Support secure deployments.
* Enable horizontal scalability.
* Maintain operational visibility.
* Minimize downtime.
* Support future architectural evolution.

---

# 3. Guiding Principles

The infrastructure follows these principles:

* Infrastructure as Code.
* Immutable deployments.
* Least privilege.
* High observability.
* Automation first.
* Security by design.
* Fault isolation.
* Cost-aware scalability.

Infrastructure should be reproducible from source-controlled configuration rather than manual intervention.

---

# 4. High-Level Infrastructure

```text
                Internet
                    │
            Cloudflare DNS
                    │
          Cloudflare CDN + WAF
                    │
                HTTPS
                    │
                 Nginx
                    │
      ┌─────────────┴─────────────┐
      │                           │
 Next.js Frontend         ASP.NET Core API
                                  │
          ┌───────────────┬───────────────┐
          │               │               │
      PostgreSQL       Redis        Cloudflare R2
```

Each component has a clearly defined operational responsibility and trust boundary.

---

# 5. Core Infrastructure Components

The production environment consists of:

Edge Layer

* Cloudflare DNS
* Cloudflare CDN
* Web Application Firewall (WAF)

Application Layer

* Nginx Reverse Proxy
* Next.js Frontend
* ASP.NET Core Backend

Data Layer

* PostgreSQL
* Redis
* Cloudflare R2

Delivery Layer

* GitHub
* GitHub Actions
* Docker

Operations Layer

* Monitoring
* Logging
* Alerting
* Backup services

Each layer should remain independently maintainable.

---

# 6. Infrastructure Principles

Infrastructure should support:

* Stateless application services.
* Persistent data separation.
* Independent scaling.
* Automated deployment.
* Secure networking.
* Centralized logging.
* Health monitoring.
* Disaster recovery.

Infrastructure decisions should minimize operational complexity while supporting future growth.

---

# 7. Deployment Boundaries

Logical deployment boundaries include:

External Network

* Internet
* Cloudflare

DMZ / Edge

* Reverse proxy
* TLS termination
* WAF

Application Network

* Frontend
* Backend APIs

Internal Services

* PostgreSQL
* Redis

Managed Storage

* Cloudflare R2

Boundaries should enforce least-privilege communication.

---

# 8. Availability Strategy

Infrastructure should support:

* Automatic service restart.
* Health checks.
* Rolling deployments.
* Backup and recovery.
* Component isolation.
* Future multi-node deployments.

Single component failures should have minimized business impact.

---

# 9. Scalability Strategy

The architecture should support future scaling by enabling:

* Horizontal API scaling.
* Independent frontend scaling.
* Database optimization.
* Cache expansion.
* CDN edge caching.
* Background worker separation.

Scalability decisions should remain compatible with the Modular Monolith architecture and future service decomposition.

---

# 10. Operational Responsibilities

Platform Engineering

Responsible for:

* Infrastructure provisioning.
* Networking.
* Deployment platform.
* Monitoring.
* Capacity planning.

Application Teams

Responsible for:

* Application configuration.
* Runtime behavior.
* Health endpoints.
* Resource optimization.

Security Engineering

Responsible for:

* Infrastructure hardening.
* Security reviews.
* Access governance.
* Infrastructure compliance.

Responsibilities should remain clearly separated.

---

# 11. Acceptance Criteria

This document is complete when:

* Infrastructure layers are identified.
* Major components are documented.
* Deployment boundaries are defined.
* Operational responsibilities are assigned.
* Scalability principles are established.

---

# Next Document

**Repository Path**

`docs/infrastructure/DEPLOYMENT_ARCHITECTURE.md`

This document defines deployment topology, environment layouts, deployment units, release strategies, runtime topology, and production deployment workflows for the Lunora Wear platform.
