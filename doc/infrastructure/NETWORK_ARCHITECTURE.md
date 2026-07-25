# Repository Path

`docs/infrastructure/NETWORK_ARCHITECTURE.md`

---

# Network Architecture

**Project:** Lunora Wear

**Document ID:** LW-INF-003

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Classification:** Infrastructure Architecture

**Related Documents**

* `docs/infrastructure/INFRASTRUCTURE_OVERVIEW.md`
* `docs/infrastructure/DEPLOYMENT_ARCHITECTURE.md`
* `docs/security/TRUST_BOUNDARIES.md`
* `docs/security/API_SECURITY.md`
* `docs/infrastructure/DNS_ARCHITECTURE.md`

---

# 1. Purpose

This document defines the logical network architecture for the Lunora Wear platform.

It specifies network zones, communication paths, trust boundaries, ingress and egress controls, firewall rules, and service connectivity.

---

# 2. Objectives

The network architecture shall:

* Minimize attack surface.
* Enforce network segmentation.
* Restrict unnecessary communication.
* Support secure service interaction.
* Enable future horizontal scaling.
* Maintain operational simplicity.

---

# 3. Guiding Principles

The platform follows these principles:

* Default deny.
* Least privilege.
* Secure by default.
* Explicit communication paths.
* Layered defense.
* Network observability.

Every permitted network connection should have a documented business purpose.

---

# 4. Logical Network Topology

```text id="qv84na"
                 Internet
                     │
             Cloudflare Edge
                     │
              HTTPS (443)
                     │
          Ubuntu Host Firewall
                     │
                Nginx Reverse Proxy
             ┌──────────┴──────────┐
             │                     │
      Next.js Frontend      ASP.NET Core API
                                    │
                    ┌───────────────┴──────────────┐
                    │                              │
               PostgreSQL                     Redis
                    │
            Cloudflare R2 (HTTPS)
```

Only documented communication paths shall be permitted.

---

# 5. Network Zones

## Public Zone

Accessible from the Internet.

Components:

* Cloudflare DNS
* Cloudflare CDN
* Cloudflare WAF

---

## Edge Zone

Handles incoming traffic.

Components:

* Nginx

Responsibilities:

* TLS termination
* Reverse proxy
* Request routing
* Security headers

---

## Application Zone

Components:

* Next.js
* ASP.NET Core

Responsibilities:

* Business logic
* Authentication
* API processing

This zone should not be directly reachable from the Internet except through approved entry points.

---

## Data Zone

Components:

* PostgreSQL
* Redis

Responsibilities:

* Persistent storage
* Caching

The Data Zone shall never accept direct public connections.

---

## External Managed Services

Examples:

* Cloudflare R2
* Email providers
* Payment gateways
* Shipping providers

Connections shall use authenticated HTTPS.

---

# 6. Allowed Communication Matrix

| Source       | Destination   | Protocol        | Purpose                  |
| ------------ | ------------- | --------------- | ------------------------ |
| Internet     | Cloudflare    | HTTPS           | Customer requests        |
| Cloudflare   | Nginx         | HTTPS           | Reverse proxy            |
| Nginx        | Next.js       | HTTP (internal) | Frontend routing         |
| Nginx        | ASP.NET Core  | HTTP (internal) | API routing              |
| ASP.NET Core | PostgreSQL    | TCP             | Data access              |
| ASP.NET Core | Redis         | TCP             | Cache access             |
| ASP.NET Core | Cloudflare R2 | HTTPS           | Object storage           |
| ASP.NET Core | External APIs | HTTPS           | Third-party integrations |

Any communication not explicitly listed should be considered unauthorized.

---

# 7. Ingress Controls

Approved ingress points include:

* HTTPS (443)
* SSH for administration (restricted)
* Monitoring endpoints where explicitly approved

Unused ports should remain closed.

---

# 8. Egress Controls

Outbound traffic should be limited to:

* Cloudflare services
* Email providers
* Payment gateways
* Shipping providers
* Software update repositories
* Monitoring services

Egress traffic should be logged where practical.

---

# 9. Firewall Strategy

Firewall rules should:

* Deny by default.
* Permit only documented services.
* Restrict administrative access.
* Log denied connections where appropriate.
* Support future network segmentation.

Firewall configuration should be version controlled.

---

# 10. Future Network Evolution

The logical network should remain valid as the platform evolves to:

* Multiple application servers.
* Dedicated database server.
* Dedicated Redis server.
* Background worker nodes.
* Container orchestration.
* Multi-region deployment.

Physical topology may change without altering logical trust boundaries.

---

# 11. Monitoring

Operational monitoring includes:

* Connection failures.
* Unexpected inbound traffic.
* Unexpected outbound traffic.
* Firewall events.
* TLS failures.
* Latency between services.

Network monitoring should support incident investigations.

---

# 12. Governance

Platform Engineering

Responsible for:

* Network configuration.
* Firewall management.
* Connectivity validation.

Security Engineering

Responsible for:

* Network security reviews.
* Trust boundary validation.
* Firewall policy approval.

Application Teams

Responsible for:

* Documenting required communication paths.
* Minimizing unnecessary network dependencies.

---

# 13. Acceptance Criteria

This document is complete when:

* Network zones are defined.
* Communication paths are documented.
* Firewall strategy is established.
* Ingress and egress controls are specified.
* Governance responsibilities are assigned.

---

# Next Document

**Repository Path**

`docs/infrastructure/DNS_ARCHITECTURE.md`

This document defines domain architecture, DNS zones, record strategy, subdomain management, DNS security, Cloudflare integration, certificate strategy, and lifecycle governance for the Lunora Wear platform.
