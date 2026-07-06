# ADR-002: Base Model Strategy

**Status:** Accepted
**Date:** July 2026
**Project:** ShopSphere API
**Sprint:** Sprint 1 – Common Foundation

---

# Context

As ShopSphere grows, multiple applications such as Users, Products, Orders, Inventory, Cart, Reviews, and Payments will share common functionality.

Without a common strategy, developers may duplicate the same fields and logic across multiple models, resulting in inconsistent implementations and increased maintenance costs.

To address this, the project adopts a reusable base model strategy using Django abstract models.

---

# Decision

The project will use **abstract base models** to encapsulate reusable functionality.

Each base model will represent a single responsibility and can be combined through inheritance to create application-specific models.

Examples include:

* TimeStampedModel
* UUIDModel
* SoftDeleteModel (future)
* AuditModel (future)

Business models will inherit only the components they require.

---

# Why Abstract Models?

Django abstract models allow multiple models to share common fields and behavior without creating additional database tables.

Benefits include:

* Eliminates duplicate code
* Provides a consistent implementation across applications
* Simplifies maintenance
* Encourages reuse
* Improves readability
* Makes future enhancements easier

For example, updating the timestamp implementation in one abstract model automatically benefits every model that inherits from it.

---

# Why Composition Over Duplication?

Instead of copying the same fields into every model, the project follows the principle of composition.

For example, a Product model can inherit multiple reusable models:

* UUIDModel
* TimeStampedModel

while another model may only require:

* TimeStampedModel

This approach allows models to include only the functionality they need, resulting in a cleaner and more maintainable design.

Advantages include:

* Reduced code duplication
* Improved flexibility
* Easier testing
* Better separation of concerns
* Lower maintenance effort

---

# Why Separate Timestamp and UUID Concerns?

Each reusable feature should have a single responsibility.

TimeStampedModel is responsible only for tracking record creation and modification times.

UUIDModel is responsible only for generating secure public identifiers.

Keeping these concerns separate provides several advantages:

* Models can inherit only the required functionality.
* Future changes remain isolated.
* Each component is easier to understand and test.
* The architecture follows the Single Responsibility Principle (SRP).

For example:

* A temporary logging model may only require timestamps.
* A public-facing resource may require both UUIDs and timestamps.
* Another model may later require soft delete support without changing existing base models.

This modular approach provides greater flexibility than combining all features into a single base class.

---

# Consequences

### Positive

* Consistent model design
* Reduced duplicate code
* Easier maintenance
* Improved scalability
* Better developer experience
* Clear separation of responsibilities

### Trade-offs

* Slightly more files to maintain
* Developers must understand the inheritance hierarchy
* Good documentation is required to explain the architecture

The long-term benefits outweigh these minor costs.

---

# Future Evolution

The base model strategy is expected to grow as the project evolves.

Planned reusable components include:

* SoftDeleteModel
* AuditModel
* OwnershipModel
* SlugModel
* AddressMixin
* Money utilities
* Shared validators

These additions can be introduced without affecting existing business models because the architecture is modular and extensible.

---

# Conclusion

ShopSphere adopts abstract base models to create a clean, reusable, and maintainable architecture. By separating concerns such as timestamps, UUIDs, and future shared behaviors into independent components, the project minimizes duplication while allowing each application to compose only the functionality it requires. This strategy supports long-term scalability and provides a strong foundation for future contributors.