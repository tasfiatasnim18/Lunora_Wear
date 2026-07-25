# Repository Path

`docs/infrastructure/CLOUDFLARE_R2.md`

---

# Cloudflare R2 Architecture

**Project:** Lunora Wear

**Document ID:** LW-INF-008

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Classification:** Infrastructure Architecture

**Related Documents**

* `docs/infrastructure/CLOUDFLARE_ARCHITECTURE.md`
* `docs/infrastructure/NETWORK_ARCHITECTURE.md`
* `docs/security/FILE_UPLOAD_SECURITY.md`
* `docs/security/DATA_CLASSIFICATION.md`
* `docs/infrastructure/POSTGRESQL_ARCHITECTURE.md`

---

# 1. Purpose

This document defines the Cloudflare R2 architecture for the Lunora Wear platform.

It specifies bucket organization, object lifecycle, storage strategy, access control, upload workflows, integration with the application platform, and operational governance.

---

# 2. Objectives

The object storage architecture shall:

* Provide highly durable storage.
* Support scalable asset management.
* Optimize global content delivery.
* Secure uploaded objects.
* Separate object storage from transactional data.
* Simplify lifecycle management.

---

# 3. Guiding Principles

The platform follows these principles:

* Object storage is not a database.
* Immutable objects where practical.
* Metadata stored separately from binary objects.
* Least-privilege access.
* Secure-by-default configuration.
* Lifecycle-driven storage management.

---

# 4. High-Level Architecture

```text
                User
                  │
           HTTPS Upload
                  │
           ASP.NET Core API
                  │
        Validation & Authorization
                  │
          Cloudflare R2 Bucket
                  │
      Cloudflare Edge CDN Delivery
                  │
               End User
```

Binary content should be stored in R2, while business metadata remains in PostgreSQL.

---

# 5. Storage Responsibilities

Cloudflare R2 stores:

* Product images
* Brand logos
* Category banners
* Marketing assets
* User profile images
* Uploaded documents (where applicable)
* Static downloadable files

R2 should not store:

* Transactional records
* Business logic data
* Session information
* Application configuration

---

# 6. Bucket Organization

Recommended structure:

```text
lunora-production/

├── products/
│   ├── original/
│   ├── thumbnail/
│   └── gallery/
│
├── categories/
│
├── brands/
│
├── users/
│
├── marketing/
│
├── documents/
│
└── backups/
```

Object prefixes should reflect business domains rather than implementation details.

---

# 7. Object Naming Strategy

Object names should:

* Be globally unique.
* Avoid user-supplied filenames.
* Include logical grouping prefixes.
* Support efficient retrieval.
* Remain immutable after publication.

Example:

```text
products/
    2026/
        07/
            8f4a2d91-image.webp
```

---

# 8. Upload Workflow

```text
Client
   │
Authentication
   │
Upload Request
   │
Validation
   │
Virus/Malware Scan (future)
   │
Upload to R2
   │
Metadata Saved in PostgreSQL
   │
Success Response
```

The application should validate uploads before storage.

---

# 9. Metadata Strategy

Binary object:

Stored in Cloudflare R2.

Metadata stored in PostgreSQL includes:

* Object identifier
* Business entity reference
* MIME type
* Size
* Upload timestamp
* Owner
* Storage path
* Version (if applicable)

This separation simplifies querying while keeping object storage efficient.

---

# 10. Access Control

Access should be controlled through:

* Application authorization.
* Signed URLs where required.
* Least-privilege API credentials.
* Bucket-level permissions.
* Audit logging.

Direct public write access shall never be permitted.

---

# 11. Lifecycle Management

Lifecycle policies should support:

* Temporary uploads.
* Archived assets.
* Soft deletion workflows.
* Permanent deletion.
* Storage cleanup.
* Version retention (if enabled).

Lifecycle rules should align with business retention requirements.

---

# 12. Monitoring

Operational monitoring includes:

* Storage utilization.
* Upload failures.
* Download latency.
* Unauthorized access attempts.
* Object count growth.
* Error rates.

Monitoring should integrate with the platform observability stack.

---

# 13. Future Evolution

The architecture should support:

* Multiple buckets by environment.
* Cross-region replication (if adopted).
* Object versioning.
* Automated image optimization.
* CDN optimization.
* Signed download URLs.
* Direct browser uploads using temporary credentials.

Future enhancements should not require changes to application business logic.

---

# 14. Governance

Platform Engineering

Responsible for:

* Bucket configuration.
* Storage lifecycle.
* Access management.
* Operational monitoring.

Security Engineering

Responsible for:

* Credential governance.
* Access reviews.
* Data protection policies.
* Audit requirements.

Application Teams

Responsible for:

* Upload validation.
* Metadata integrity.
* Business ownership of stored assets.

---

# 15. Acceptance Criteria

This document is complete when:

* Bucket organization is documented.
* Upload workflow is defined.
* Metadata strategy is established.
* Access control is specified.
* Lifecycle management is documented.
* Governance responsibilities are assigned.

---

# Next Document

**Repository Path**

`docs/infrastructure/POSTGRESQL_ARCHITECTURE.md`

This document defines the PostgreSQL architecture, including database topology, schema organization, indexing strategy, transaction management, backup approach, replication readiness, performance optimization, and governance for the Lunora Wear platform.
