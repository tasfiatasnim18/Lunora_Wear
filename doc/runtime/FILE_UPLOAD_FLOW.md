# Repository Path

`docs/runtime/FILE_UPLOAD_FLOW.md`

---

# File Upload Flow

**Project:** Lunora Wear

**Document ID:** LW-RT-010

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Related Documents**

* `docs/architecture/FILE_STORAGE_ARCHITECTURE.md`
* `docs/security/FILE_UPLOAD_SECURITY.md`
* `docs/runtime/REQUEST_PIPELINE.md`
* `docs/backend/FILE_STORAGE.md`

---

# 1. Purpose

This document defines the lifecycle of file uploads throughout the Lunora Wear platform.

Supported uploads include:

* Product images
* Category images
* Brand logos
* Customer profile photos
* Return request evidence
* Marketing assets
* Future document uploads

The upload pipeline prioritizes security, scalability, and consistency.

---

# 2. Upload Principles

Every uploaded file is considered **untrusted** until it successfully completes the validation and processing pipeline.

The platform should:

* Validate uploads before persistence.
* Store binary content outside the application server.
* Record metadata separately from file contents.
* Prevent direct execution of uploaded content.
* Preserve auditability.

---

# 3. Upload Pipeline

```text
Client
    │
HTTPS Upload
    │
Authentication
    │
Authorization
    │
Request Validation
    │
File Validation
    │
Temporary Storage
    │
Security Checks
    │
Image Processing
    │
Object Storage
    │
Metadata Persistence
    │
CDN Delivery
```

---

# 4. Request Validation

Before accepting a file:

* Verify authentication (where required).
* Verify authorization.
* Validate request size.
* Validate request content type.
* Enforce endpoint-specific limits.

Requests exceeding configured limits are rejected immediately.

---

# 5. File Validation

Validation includes:

* File extension
* MIME type
* Actual file signature (magic bytes)
* Maximum file size
* Minimum file size
* Image dimensions (where applicable)

File extensions alone must never be trusted.

---

# 6. Temporary Storage

Files are initially written to temporary storage.

Temporary files should:

* Have unique names.
* Be inaccessible to end users.
* Be automatically cleaned up after processing.
* Have a defined retention period.

---

# 7. Security Checks

Processing may include:

* Malware scanning integration
* Corruption detection
* Image decoding validation
* Archive inspection (if archive uploads are supported)

Files failing security checks are rejected and logged.

---

# 8. Image Processing

For supported image types:

* Correct orientation using metadata where appropriate.
* Generate predefined derivative sizes (e.g., thumbnail, medium, large).
* Compress using approved quality settings.
* Strip unnecessary metadata unless business requirements dictate otherwise.

Original files may be retained according to retention policy.

---

# 9. Object Storage

Approved files are stored in Cloudflare R2.

Object keys should:

* Be globally unique.
* Avoid exposing internal identifiers unnecessarily.
* Follow a documented naming convention.
* Support future lifecycle policies.

Binary files should never be stored directly in the relational database except where explicitly justified.

---

# 10. Metadata Persistence

The database stores metadata only, such as:

* File identifier
* Owner identifier
* Storage key
* Original filename
* Content type
* Size
* Dimensions (if applicable)
* Upload timestamp
* Processing status

Metadata serves as the authoritative reference for uploaded assets.

---

# 11. CDN Delivery

Public assets are delivered through Cloudflare CDN.

Recommendations:

* Use immutable URLs for versioned assets.
* Apply appropriate cache-control headers.
* Invalidate cached assets only when necessary.

Private assets should require authenticated access and should not be publicly cacheable.

---

# 12. Failure Handling

If processing fails:

* Remove temporary files.
* Do not persist incomplete metadata.
* Return a standardized error response.
* Log the failure with a correlation ID.

Partially processed uploads must not become visible to users.

---

# 13. Monitoring

Operational metrics include:

* Upload success rate
* Upload failure rate
* Average processing time
* Malware scan failures
* Image processing duration
* Storage utilization
* CDN cache hit rate for media

---

# 14. Security

The platform must:

* Reject executable content where not explicitly supported.
* Prevent path traversal attacks.
* Prevent filename collisions.
* Validate all uploaded content.
* Encrypt transport using HTTPS.
* Apply least-privilege access to object storage.

Administrative upload operations should be audited.

---

# 15. Acceptance Criteria

This document is complete when:

* Upload lifecycle is defined.
* Validation requirements are documented.
* Processing pipeline is specified.
* Storage strategy is established.
* Monitoring and security requirements are documented.

---

# Next Document

**Repository Path**

`docs/runtime/PAYMENT_FLOW.md`

This document defines the end-to-end payment lifecycle, including payment initiation, authorization, capture, webhook handling, reconciliation, refunds, failure recovery, and audit requirements.
