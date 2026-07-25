# Repository Path

`docs/runtime/CACHING_FLOW.md`

---

# Caching Flow

**Project:** Lunora Wear

**Document ID:** LW-RT-009

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/backend/CACHING.md`
* `docs/runtime/QUERY_PIPELINE.md`
* `docs/runtime/COMMAND_PIPELINE.md`
* `docs/architecture/CACHING_STRATEGY.md`

---

# 1. Purpose

This document defines how caching is used throughout the Lunora Wear platform to improve response time, reduce database load, and increase scalability while preserving data consistency.

Caching is a performance optimization and must never become the source of truth.

---

# 2. Objectives

The caching strategy aims to:

* Reduce response latency
* Improve throughput
* Protect the database
* Support horizontal scaling
* Minimize unnecessary network traffic

Correctness always takes priority over cache hit rate.

---

# 3. Cache Layers

The platform uses multiple cache layers.

## Browser Cache

Used for:

* Images
* Fonts
* CSS
* JavaScript
* Static assets

Controlled through HTTP cache headers.

---

## CDN Cache

Cloudflare caches:

* Product images
* Static files
* Public assets

Benefits:

* Global edge delivery
* Reduced origin traffic
* Faster page loads

---

## Application Memory Cache

Suitable for:

* Configuration
* Feature flags
* Static lookup data

Application memory cache should not be relied upon for shared state across multiple instances.

---

## Redis Distributed Cache

Used for:

* Product detail cache
* Category cache
* Search filters
* Session-related metadata (where applicable)
* Frequently accessed read models

Redis acts as the primary distributed cache.

---

# 4. Cache Flow

```text
Client
    │
CDN
    │
Application
    │
Memory Cache
    │
Redis
    │
Database
```

Each lower layer is consulted only when the previous layer cannot satisfy the request.

---

# 5. Cache Population

Preferred strategy:

Cache-aside

Workflow:

1. Request data.
2. Check cache.
3. Cache miss.
4. Load from database.
5. Store in cache.
6. Return response.

Applications should not assume cached data always exists.

---

# 6. Cache Invalidation

Cache invalidation occurs after successful state changes.

Examples:

* Product updated
* Inventory adjusted
* Category modified
* Promotion activated
* Product image replaced

Invalidation should occur after transaction commit.

---

# 7. Cache Keys

Cache keys should:

* Be deterministic
* Include entity identifiers where applicable
* Support versioning when necessary
* Avoid sensitive information

Example naming convention:

```text
product:12345
category:summer
search:keyword=shirt&page=1
```

---

# 8. Time-to-Live (TTL)

Different data types may have different TTL values.

Examples:

* Product catalog: medium TTL
* Configuration: long TTL
* Inventory availability: short TTL

TTL values should be configurable through application settings rather than hard-coded.

---

# 9. Cache Consistency

The platform prioritizes:

* Strong consistency for transactional operations
* Eventual consistency for non-critical read models

Users should not observe stale data for operations that immediately affect their own workflow, such as viewing their cart after adding an item.

---

# 10. Cache Stampede Protection

To avoid excessive database load during cache expiration:

* Use request coalescing where appropriate.
* Refresh popular entries proactively when justified.
* Apply jitter to expiration times to reduce synchronized expirations.

---

# 11. Monitoring

Metrics include:

* Cache hit rate
* Cache miss rate
* Average lookup latency
* Redis availability
* Eviction count
* Memory utilization

These metrics support capacity planning and tuning.

---

# 12. Failure Handling

If Redis becomes unavailable:

* Continue serving requests directly from the database where feasible.
* Degrade gracefully.
* Record operational alerts.
* Avoid failing user requests solely because of cache unavailability.

Caching failures should not corrupt business data.

---

# 13. Security

Caches must not contain:

* Plain-text passwords
* Private cryptographic keys
* Sensitive secrets

Cached user-specific data must respect authorization boundaries and expiration policies.

---

# 14. Acceptance Criteria

This document is complete when:

* Cache layers are defined.
* Population strategy is documented.
* Invalidation rules are established.
* Monitoring requirements are documented.
* Failure behavior is specified.

---

# Next Document

**Repository Path**

`docs/runtime/FILE_UPLOAD_FLOW.md`

This document defines the lifecycle of file uploads, including validation, malware scanning integration points, object storage, image processing, metadata persistence, and secure content delivery.
