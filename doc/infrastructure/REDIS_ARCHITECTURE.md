# Repository Path

`docs/infrastructure/REDIS_ARCHITECTURE.md`

---

# Redis Architecture

**Project:** Lunora Wear

**Document ID:** LW-INF-010

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Classification:** Infrastructure Architecture

**Related Documents**

* `docs/infrastructure/POSTGRESQL_ARCHITECTURE.md`
* `docs/infrastructure/DOCKER_ARCHITECTURE.md`
* `docs/security/SESSION_MANAGEMENT.md`
* `docs/runtime/CACHING_STRATEGY.md`
* `docs/data/DATA_MODEL.md`

---

# 1. Purpose

This document defines the Redis architecture for the Lunora Wear platform.

It specifies caching strategies, key design, expiration policies, session storage, distributed locking considerations, operational monitoring, and governance.

---

# 2. Objectives

The Redis architecture shall:

* Reduce application latency.
* Offload repetitive database queries.
* Improve scalability.
* Support session and temporary data storage.
* Enable efficient cache invalidation.
* Maintain graceful degradation during failures.

---

# 3. Guiding Principles

The platform follows these principles:

* Redis is a cache, not the source of truth.
* Cached data must be disposable.
* Expiration should be explicit.
* Minimize memory fragmentation.
* Avoid unnecessary persistence.
* Cache invalidation must be predictable.

The application must continue operating if Redis becomes temporarily unavailable.

---

# 4. High-Level Architecture

```text
          Client
             │
      ASP.NET Core API
             │
      Cache Lookup (Redis)
       ┌───────────────┐
       │ Cache Hit     │
       │ Return Value  │
       └───────────────┘
             │
       Cache Miss
             │
        PostgreSQL
             │
      Store in Redis
             │
        Return Result
```

Redis serves as a performance optimization layer positioned between the application and the database.

---

# 5. Primary Use Cases

Redis may be used for:

* Product catalog caching.
* Category navigation caching.
* Frequently accessed configuration.
* Rate limiting counters.
* Temporary verification codes.
* Session storage (if adopted).
* Shopping cart caching (optional).
* Distributed locks (where justified).

Redis should not store permanent business records.

---

# 6. Key Naming Strategy

Keys should follow a consistent namespace.

Examples:

```text
catalog:product:{id}

catalog:category:{id}

user:session:{sessionId}

cart:{customerId}

config:homepage

rate-limit:{ip}

verification:{email}
```

Key names should be human-readable and logically grouped.

---

# 7. Expiration Strategy

Every temporary key should have a defined Time-To-Live (TTL).

Example guidance:

| Data Type          | Typical TTL         |
| ------------------ | ------------------- |
| Product cache      | 10–30 minutes       |
| Category cache     | 30–60 minutes       |
| Session data       | Application-defined |
| Verification code  | 5–10 minutes        |
| Rate limit counter | 1–15 minutes        |
| Temporary locks    | Seconds to minutes  |

TTL values should reflect business requirements and data freshness.

---

# 8. Cache Invalidation

Cache invalidation should occur when:

* Product details change.
* Inventory changes.
* Prices change.
* Categories are updated.
* Promotional content changes.
* Configuration changes.

Invalidation should be event-driven where practical.

---

# 9. Persistence Strategy

Redis persistence should be configured based on workload.

Possible approaches:

* No persistence for ephemeral cache.
* Snapshot persistence where operationally beneficial.
* Append-only persistence only if required for specific workloads.

Persistence requirements should align with business recovery objectives.

---

# 10. Memory Management

Redis should enforce:

* Maximum memory allocation.
* Eviction policies.
* Memory monitoring.
* Key expiration cleanup.

Eviction policies should prioritize removal of disposable cache entries rather than critical operational data.

---

# 11. High Availability

Future evolution may include:

* Redis replication.
* Sentinel.
* Redis Cluster.
* Read replicas.
* Automatic failover.

Application logic should remain compatible with both standalone and clustered Redis deployments.

---

# 12. Monitoring

Operational monitoring includes:

* Memory utilization.
* Cache hit ratio.
* Cache miss ratio.
* Evictions.
* Expired keys.
* Slow commands.
* Client connections.
* Replication health (future).

Monitoring thresholds should generate alerts before service degradation occurs.

---

# 13. Governance

Platform Engineering

Responsible for:

* Redis deployment.
* Runtime configuration.
* Capacity planning.
* Operational monitoring.

Security Engineering

Responsible for:

* Access control.
* Authentication configuration.
* Network restrictions.
* Secret management.

Application Teams

Responsible for:

* Cache design.
* TTL selection.
* Invalidation logic.
* Performance optimization.

---

# 14. Acceptance Criteria

This document is complete when:

* Cache architecture is documented.
* Key naming strategy is defined.
* Expiration and invalidation policies are established.
* Monitoring requirements are specified.
* Governance responsibilities are assigned.

---

# Next Document

**Repository Path**

`docs/infrastructure/CONFIGURATION_MANAGEMENT.md`

This document defines the configuration management architecture, including environment variables, application settings, secrets handling, configuration versioning, runtime configuration, and governance for the Lunora Wear platform.
