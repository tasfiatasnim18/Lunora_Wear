# Information Architecture (IA)

**Project:** Lunora Wear

**Document ID:** LW-IA-001

**Version:** 1.0.0

**Status:** Draft

**Priority:** Critical

**Owner:** Product & UX

---

# 1. Purpose

The Information Architecture (IA) defines how information, content, products, and navigation are organized across the Lunora Wear platform.

Its goals are to:

* Make products easy to discover.
* Reduce navigation friction.
* Improve conversion.
* Support SEO.
* Ensure scalability as the product catalog grows.

This document is the foundation for navigation design, page hierarchy, URL structure, search, filtering, breadcrumbs, and internal linking.

---

# 2. Design Principles

The information architecture must follow these principles:

* Customer-first navigation
* Three-click rule for key tasks
* Mobile-first organization
* Predictable hierarchy
* Consistent terminology
* SEO-friendly URLs
* Scalable taxonomy

---

# 3. Primary Navigation

## Public Navigation

* Home
* New Arrivals
* Men
* Women
* Collections
* Sale
* About
* Contact

Right-side utilities:

* Search
* Wishlist
* Cart
* Account

---

# 4. Site Hierarchy

```
Home
│
├── New Arrivals
├── Men
│   ├── T-Shirts
│   ├── Shirts
│   ├── Polo
│   ├── Pants
│   ├── Jeans
│   ├── Jackets
│   └── Accessories
│
├── Women
│   ├── Tops
│   ├── Dresses
│   ├── Bottoms
│   ├── Outerwear
│   └── Accessories
│
├── Collections
├── Sale
├── About
├── Contact
└── Customer Account
```

---

# 5. Product Taxonomy

Every product belongs to:

* Category
* Optional Subcategory
* Collection
* Brand
* Season
* Gender
* Tags

Example:

Category → Men

Subcategory → T-Shirts

Collection → Summer 2027

Tags:

* Oversized
* Cotton
* Casual
* Black

---

# 6. URL Strategy

Examples:

```
/

```

Homepage

```
/men

```

Category

```
/men/t-shirts

```

Subcategory

```
/collections/summer-2027

```

Collection

```
/product/classic-oversized-black-tee

```

Product

```
/account/orders

```

Customer Area

```
/admin/products

```

Admin Area

Rules:

* Lowercase only
* Hyphen-separated words
* Stable URLs
* Human-readable
* Canonical URLs

---

# 7. Search Architecture

Search should support:

* Product name
* SKU
* Category
* Collection
* Brand
* Tags

Future enhancements:

* Synonyms
* Typo tolerance
* Natural language search
* AI search assistant

---

# 8. Filtering Strategy

Global filters include:

* Category
* Size
* Color
* Price
* Availability
* Collection
* Brand

Future filters:

* Material
* Fit
* Sleeve type
* Occasion

---

# 9. Sorting Options

* Featured
* Newest
* Price: Low to High
* Price: High to Low
* Best Selling
* Customer Rating

---

# 10. Breadcrumbs

Example:

```
Home
→ Men
→ T-Shirts
→ Classic Oversized Tee
```

Breadcrumbs are required for:

* SEO
* Navigation
* Context

---

# 11. Footer Architecture

Sections:

* Shop
* Customer Service
* Company
* Policies
* Social Links
* Newsletter

---

# 12. Customer Account Structure

```
Account
├── Dashboard
├── Orders
├── Wishlist
├── Addresses
├── Profile
├── Password
└── Logout
```

---

# 13. Admin Structure

```
Dashboard
├── Products
├── Categories
├── Inventory
├── Orders
├── Customers
├── Promotions
├── CMS
├── Reports
└── Settings
```

---

# 14. SEO Structure

Every indexable page should include:

* Unique title
* Meta description
* Canonical URL
* Open Graph metadata
* Structured data (where applicable)

---

# 15. Scalability Considerations

The IA must support future additions without restructuring core navigation, including:

* Kids category
* Wholesale portal
* Marketplace
* Gift cards
* Loyalty program
* International storefronts

---

# 16. Acceptance Criteria

The IA is complete when:

* Navigation hierarchy is approved.
* URL conventions are finalized.
* Taxonomy supports current and future catalog growth.
* Search and filter strategy is defined.
* Account and admin structures are validated.
* SEO requirements are documented.

---

# Next Document

**Path**

`docs/01_Product_Management/USER_FLOWS.md`

This document will define end-to-end user journeys (registration, browsing, checkout, returns, account management, and administration) that connect the Information Architecture with UI design, API contracts, and QA scenarios.
