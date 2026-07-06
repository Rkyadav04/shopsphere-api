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

# Shared Models

The Common app provides reusable abstract models that encapsulate functionality shared across multiple applications. These models reduce code duplication, promote consistency, and provide a solid foundation for the entire ShopSphere backend.

Instead of repeating common fields in every model, applications inherit only the features they require.

---

# TimeStampedModel

## Purpose

`TimeStampedModel` provides automatic timestamp tracking for database records. Every model that inherits from it automatically records when the object was created and when it was last updated.

This eliminates the need to manually define timestamp fields in every application.

## Fields

| Field      | Type          | Description                                                |
| ---------- | ------------- | ---------------------------------------------------------- |
| created_at | DateTimeField | Stores the date and time when the record is created.       |
| updated_at | DateTimeField | Stores the date and time when the record is last modified. |

## Why Abstract?

`TimeStampedModel` is implemented as an abstract model because it is not intended to create its own database table.

Benefits include:

* Eliminates duplicate timestamp fields.
* Promotes consistency across applications.
* Simplifies maintenance.
* Keeps business models clean.
* Supports automatic auditing.

Every business model that requires timestamps can simply inherit from `TimeStampedModel`.

---

# UUIDModel

## Purpose

`UUIDModel` provides a UUID-based primary key for models that require secure, non-sequential identifiers.

Instead of predictable integer IDs, each record receives a universally unique identifier.

Example:

```text
550e8400-e29b-41d4-a716-446655440000
```

## Why UUIDs Are Separated

UUID functionality is intentionally separated from `TimeStampedModel` because each reusable model should have a single responsibility.

Advantages include:

* Models inherit only the functionality they require.
* Better adherence to the Single Responsibility Principle (SRP).
* Easier maintenance and testing.
* Greater flexibility for future models.

For example:

* Some models may require timestamps only.
* Some models may require UUIDs only.
* Others may inherit both models.

Keeping these concerns separate produces a cleaner and more modular architecture.

---

# SoftDeleteModel

## Purpose

`SoftDeleteModel` will provide logical deletion instead of permanently removing records from the database.

Rather than deleting data, records will be marked as inactive while remaining available for auditing and recovery.

## Planned Fields

| Field      | Purpose                                        |
| ---------- | ---------------------------------------------- |
| is_deleted | Indicates whether the record has been deleted. |
| deleted_at | Stores the deletion timestamp.                 |
| deleted_by | Stores the user responsible for the deletion.  |

## Future Implementation

The initial sprint focuses on timestamp functionality only.

Soft deletion will be implemented in a future sprint when the application begins managing business-critical data such as products, orders, customers, and payments.

Benefits include:

* Prevents accidental data loss.
* Preserves historical records.
* Supports audit requirements.
* Enables record restoration.
* Improves long-term maintainability.

---

# Inheritance Strategy

The project follows a composable inheritance strategy where models inherit only the reusable functionality they require.

```text
                    models.Model
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
TimeStampedModel     UUIDModel     SoftDeleteModel
        │                │                │
        └────────────────┼────────────────┘
                         │
                         ▼
                    Business Models
      (User, Product, Category, Order, Review)
```

This approach promotes:

* Separation of concerns
* Code reusability
* Low coupling
* High cohesion
* Easier maintenance
* Future extensibility



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
