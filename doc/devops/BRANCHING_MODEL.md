# Repository Path

`docs/devops/BRANCHING_MODEL.md`

---

# Branching Model

**Project:** Lunora Wear

**Document ID:** LW-DEV-004

**Version:** 1.0.0

**Status:** Approved

**Owner:** Platform Engineering

**Classification:** DevOps Architecture

**Related Documents**

* `docs/devops/GIT_STRATEGY.md`
* `docs/devops/VERSIONING_STRATEGY.md`
* `docs/devops/CI_ARCHITECTURE.md`
* `docs/devops/RELEASE_MANAGEMENT.md`
* `docs/devops/DEPLOYMENT_PIPELINE.md`

---

# 1. Purpose

This document defines the branching model for the Lunora Wear platform.

It establishes branch types, naming conventions, merge policies, lifecycle rules, release workflows, and governance to support reliable, automated software delivery.

---

# 2. Objectives

The branching model shall:

* Keep the main branch deployable.
* Encourage frequent integration.
* Reduce merge conflicts.
* Support continuous delivery.
* Simplify release management.
* Improve repository consistency.

---

# 3. Guiding Principles

The platform follows these principles:

* Trunk-based development.
* Short-lived feature branches.
* Frequent merges.
* Automated validation.
* Peer review before merge.
* Small incremental changes.

Long-lived development branches should be avoided.

---

# 4. Branch Types

| Branch      | Purpose                         |
| ----------- | ------------------------------- |
| `main`      | Production-ready code           |
| `feature/*` | New features                    |
| `bugfix/*`  | Non-critical bug fixes          |
| `hotfix/*`  | Critical production fixes       |
| `release/*` | Temporary release stabilization |

Release branches should exist only when necessary.

---

# 5. Branch Lifecycle

```text id="7y8e4b"
main
 │
 ├──── feature/cart-discount
 │          │
 │          ▼
 │      Pull Request
 │          │
 │          ▼
 ├──────── Merge
 │
 ├──── feature/payment
 │
 ├──── bugfix/login
 │
 └──── hotfix/checkout
```

Feature branches should be deleted after successful merge.

---

# 6. Naming Conventions

Branch names should be descriptive and lowercase.

Examples:

```text id="k4w91r"
feature/product-search

feature/order-history

feature/admin-dashboard

bugfix/payment-timeout

bugfix/cart-calculation

hotfix/login-error

release/v1.3.0
```

Branch names should describe business functionality rather than developer-specific tasks.

---

# 7. Merge Policy

Every merge into `main` requires:

* Pull Request.
* Successful CI pipeline.
* Required approvals.
* Passing quality gates.
* Resolved conversations.
* Updated documentation (where applicable).

Direct commits to `main` are prohibited.

---

# 8. Hotfix Workflow

Critical production issues follow:

```text id="d8m3sa"
Production Issue
        │
Create hotfix/*
        │
Implement Fix
        │
Run CI Pipeline
        │
Review
        │
Merge to main
        │
Deploy
        │
Tag Release
```

Hotfixes should be limited to urgent production issues and kept as small as possible.

---

# 9. Release Branches

Release branches may be created when:

* Coordinating major releases.
* Stabilizing complex changes.
* Preparing scheduled deployments.

Routine releases should merge directly from `main` whenever practical.

---

# 10. Branch Protection

The `main` branch shall enforce:

* Pull Request approval.
* Required status checks.
* No force pushes.
* No direct commits.
* Required conversation resolution.
* Required CODEOWNERS review (where applicable).

Protection rules should be managed by Platform Engineering.

---

# 11. Governance

Platform Engineering

Responsible for:

* Branch protection.
* Repository configuration.
* Workflow automation.

Development Teams

Responsible for:

* Branch creation.
* Pull Requests.
* Timely merges.
* Branch cleanup.

Security Engineering

Responsible for:

* Reviewing critical security-related changes.
* Repository access governance.

---

# 12. Acceptance Criteria

This document is complete when:

* Branch types are defined.
* Naming conventions are documented.
* Merge policies are established.
* Hotfix workflow is specified.
* Branch protection rules are identified.
* Governance responsibilities are assigned.

---

# Next Document

**Repository Path**

`docs/devops/VERSIONING_STRATEGY.md`

This document defines the versioning strategy for the Lunora Wear platform, including semantic versioning, release numbering, tagging conventions, compatibility rules, release cadence, and governance.
