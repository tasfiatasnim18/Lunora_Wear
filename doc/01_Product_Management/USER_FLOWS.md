# User Flows

**Project:** Lunora Wear

**Document ID:** LW-UF-001

**Version:** 1.0.0

**Status:** Draft

**Owner:** Product & UX

**Related Documents**

* Information Architecture
* Product Requirements Document
* Authentication Feature Specification

---

# Purpose

This document defines the primary end-to-end user journeys across the Lunora Wear platform.

Each flow describes:

* User goal
* Entry point
* Happy path
* Alternate paths
* Error conditions
* Completion criteria

These flows serve as the foundation for UX design, API sequencing, QA scenarios, and implementation planning.

---

# UF-001: Customer Registration

## Goal

Allow a new customer to create an account securely.

### Entry Point

* "Create Account" button
* Checkout (optional prompt)

### Happy Path

1. Open registration page.
2. Enter name.
3. Enter mobile number.
4. (Optional) Enter email.
5. Create password.
6. Accept Terms & Conditions.
7. Submit form.
8. Account is created.
9. Customer is automatically signed in.
10. Redirect to homepage or intended destination.

### Alternate Paths

* Mobile already registered.
* Email already exists.
* Weak password.
* Required fields missing.

### Success Criteria

* Account created.
* User authenticated.
* Audit event recorded.

---

# UF-002: Customer Login

## Goal

Authenticate an existing customer.

### Happy Path

1. Open login page.
2. Enter email or mobile.
3. Enter password.
4. Credentials validated.
5. Access token issued.
6. Refresh token issued.
7. Redirect to previous page or dashboard.

### Error Conditions

* Invalid credentials.
* Locked account.
* Disabled account.
* Too many attempts.

### Success Criteria

* Secure session established.
* Last login timestamp updated.
* Audit event recorded.

---

# UF-003: Product Discovery

## Goal

Help customers quickly find relevant products.

### Happy Path

1. Visit homepage.
2. Browse featured sections or navigation.
3. Open category.
4. Apply filters.
5. Sort results.
6. Open product details.

### Alternate Paths

* Use search instead of browsing.
* Open collection page.
* View related products.

### Success Criteria

Customer reaches the desired product page efficiently.

---

# UF-004: Add to Cart

## Goal

Place a product into the shopping cart.

### Happy Path

1. Open product page.
2. Select size.
3. Select color.
4. Choose quantity.
5. Click "Add to Cart".
6. Mini cart updates.
7. Cart count refreshes.

### Validation

* Size required (if applicable).
* Color required (if applicable).
* Quantity available.

### Error Conditions

* Out of stock.
* Invalid variant.
* Quantity exceeds stock.

### Success Criteria

Selected variant appears in the cart with accurate pricing.

---

# UF-005: Guest Checkout

## Goal

Allow a customer to purchase without creating an account.

### Happy Path

1. Review cart.
2. Proceed to checkout.
3. Enter shipping information.
4. Select shipping method.
5. Select payment method.
6. Review order.
7. Confirm purchase.
8. Payment processed.
9. Order created.
10. Confirmation displayed.

### Alternate Paths

* Customer chooses to sign in.
* Customer creates an account during checkout.

### Success Criteria

Order successfully created and ready for fulfillment.

---

# UF-006: Registered Customer Checkout

## Goal

Provide a faster checkout experience.

### Happy Path

1. Customer signs in.
2. Cart is restored.
3. Saved address selected.
4. Shipping selected.
5. Payment selected.
6. Order confirmed.
7. Payment processed.
8. Order placed.

### Benefits

* Saved addresses.
* Faster checkout.
* Order history linked automatically.

---

# UF-007: Order Tracking

## Goal

Allow customers to monitor order status.

### Happy Path

1. Sign in.
2. Open "My Orders".
3. Select an order.
4. View current status.
5. View shipment information.
6. Access invoice.

### Status Examples

* Pending
* Confirmed
* Packed
* Shipped
* Out for Delivery
* Delivered
* Cancelled
* Returned

---

# UF-008: Password Reset

## Goal

Recover account access securely.

### Happy Path

1. Click "Forgot Password".
2. Enter email or mobile.
3. Receive verification.
4. Verify identity.
5. Set a new password.
6. Existing sessions revoked.
7. User signs in again.

### Error Conditions

* Invalid code.
* Expired code.
* Too many attempts.

---

# UF-009: Product Review

## Goal

Allow verified customers to review purchased products.

### Happy Path

1. Open delivered order.
2. Select purchased product.
3. Rate product.
4. Write review.
5. Submit review.
6. Review awaits moderation (if enabled).

### Rules

* Only verified purchasers may review.
* One review per product per order.

---

# UF-010: Admin Product Management

## Goal

Maintain an accurate product catalog.

### Happy Path

1. Admin signs in.
2. Open Products.
3. Create or edit product.
4. Upload media.
5. Configure variants.
6. Set pricing.
7. Publish.

### Success Criteria

* Product is searchable.
* Inventory synchronized.
* Audit event recorded.

---

# Cross-Flow Principles

All user flows must:

* Be mobile-first.
* Minimize unnecessary steps.
* Preserve user context after authentication.
* Handle validation gracefully.
* Provide clear feedback for success and failure.
* Generate appropriate analytics and audit events.

---

# Future Flows

The following journeys will be added in later releases:

* Return & Refund Request
* Exchange Request
* Wishlist Synchronization
* Loyalty Rewards
* Gift Card Redemption
* AI Size Recommendation
* AI Outfit Builder
* Wholesale Ordering
* Marketplace Seller Onboarding

---

# Acceptance Criteria

This document is complete when:

* Core customer journeys are documented.
* Admin workflows are defined.
* Error paths are identified.
* Success criteria exist for each flow.
* Flows are approved by Product, UX, and Engineering.

---

# Next Document

**Path**

`docs/02_Features/HOMEPAGE.md`

The Homepage Feature Specification will define the storefront's landing experience, merchandising zones, content hierarchy, responsive behavior, personalization strategy, analytics events, SEO requirements, and acceptance criteria.
