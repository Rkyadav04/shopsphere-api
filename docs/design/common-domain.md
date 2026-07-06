# Common Domain Design

**Project:** ShopSphere API
**Sprint:** Sprint 1 – Common Foundation
**Author:** Ravinder Kumar
**Date:** July 2026
**Status:** Draft v1

---

# Purpose

The Common app serves as the shared foundation for the entire ShopSphere API. It contains reusable models, utilities, validators, helper functions, and mixins that can be used across multiple applications.

Instead of duplicating common functionality in every app, reusable components are placed in the Common app to improve maintainability, consistency, and scalability.

---

# Why the Common App Exists

As a backend application grows, many features require the same functionality. Writing the same code repeatedly increases maintenance effort and introduces inconsistencies.

The Common app solves this problem by centralizing shared functionality.

### Benefits

* Reduces code duplication
* Promotes code reusability
* Improves maintainability
* Encourages consistent development practices
* Simplifies future feature development

---

# Responsibilities

The Common app is responsible for providing reusable building blocks that can be shared across the project.

Examples include:

* Abstract base models
* Shared validators
* Utility functions
* Mixins
* Constants
* Custom exceptions
* Helper methods

The Common app should remain independent of business logic.

---

# What Belongs Here

The following components belong inside the Common app:

* Abstract models
* Timestamp functionality
* UUID utilities
* Soft delete functionality
* Common validators
* Slug generation
* File upload utilities
* Helper functions
* Shared constants
* Common exceptions
* Pagination helpers
* Permission helpers

These components should be reusable by multiple applications.

---

# What Does NOT Belong Here

Business-specific logic should never be placed inside the Common app.

Examples include:

* User authentication
* Product management
* Order processing
* Cart functionality
* Inventory management
* Payment processing
* Review management
* Email verification

Each application should contain its own business logic while depending on the Common app for reusable components.

---

# Base Models

The Common app will provide reusable abstract models that other applications can inherit from.

## TimeStampedModel

### Purpose

Automatically stores creation and modification timestamps.

### Fields

| Field      | Type          | Purpose                |
| ---------- | ------------- | ---------------------- |
| created_at | DateTimeField | Record creation time   |
| updated_at | DateTimeField | Last modification time |

### Benefits

* Automatic auditing
* Consistent timestamps
* Reduced duplicate code
* Useful for reporting and debugging

Example:

```python
class Product(TimeStampedModel):
    ...
```

---

# Future Base Models

## UUIDModel

### Purpose

Provide UUID primary keys for models requiring secure public identifiers.

### Benefits

* Harder to guess IDs
* Prevents resource enumeration
* Better suited for distributed systems
* Consistent identifier format

---

## SoftDeleteModel

### Purpose

Allow records to be marked as deleted instead of permanently removing them.

### Future Fields

* is_deleted
* deleted_at
* deleted_by

### Benefits

* Preserves historical records
* Supports data recovery
* Improves auditing
* Prevents accidental data loss

---

# Future Extensions

As ShopSphere grows, additional reusable models may be introduced.

## AuditModel

Tracks who created, updated, or deleted a record.

Example fields:

* created_by
* updated_by
* deleted_by

Useful for administrative auditing.

---

## OwnershipModel

Provides ownership relationships.

Example:

* Product owner
* Store owner
* Review author

---

## SlugModel

Automatically generates SEO-friendly slugs.

Example:

```
gaming-laptop
wireless-headphones
iphone-17-pro
```

Useful for products and categories.

---

## AddressMixin

Reusable address fields.

Example:

* street
* city
* state
* postal_code
* country

Can be reused by:

* Users
* Sellers
* Shipping addresses
* Billing addresses

---

## Money Utilities

Future utilities for handling money consistently.

Examples:

* Currency formatting
* Price validation
* Tax calculation
* Discount calculation
* Decimal rounding

Using centralized utilities ensures consistent financial calculations throughout the project.

---

## Validators

Reusable validators may include:

* Email validation
* Phone number validation
* Username validation
* Password strength validation
* Image validation
* File extension validation
* Slug validation
* Postal code validation

Keeping validators in one place improves consistency and reduces duplicate validation logic.

---

# Engineering Principles

The Common app follows these principles:

* Reusability
* Separation of Concerns
* Low Coupling
* High Cohesion
* Scalability
* Maintainability
* Consistency

Every component should be generic enough to be reused across multiple applications without depending on business-specific logic.

---

# Summary

The Common app provides the shared foundation for the ShopSphere API. By centralizing reusable models, utilities, validators, and helper classes, it reduces code duplication and improves maintainability. All business-specific logic remains within individual applications, while the Common app provides reusable building blocks that support the entire backend architecture.
