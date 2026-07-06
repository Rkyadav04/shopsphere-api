# USER DOMAIN DESIGN

**PROJECT:** Shopsphere API
**SPRINT:** Sprint 1- Authentication 
**Author:** Ravinder Kumar
**Date:** July 2026

# DESIGN GOALS

- Flexibilty
- Security
- Maintainbility
- Extensibility
- Production Readiness

# PURPOSE

The authentication system is the foundation of the entire Shopsphere API backend. 
Every major module-- including product, inventory, orders, cart, review, payments, and administration-- will depend on the user domain.
The goal of this design is to create a flexible, maintainable, authentication system that evolves with future business requirements without requiring disruptive database migrations.

### Why Use a Custom Usern Model ?

Although Django provides a built-in User model, replacing it later in project's lifecycle is difficult and often requires complex schema migrations.

Creating a custom user model from the beginning provides several advantages:

- Full control over the authentication system.
- Ability to add business-specific fields without major migrations.
- Support for E-mail based authentication.
- Easier integration with third-party authentication provider.
- Future capabilities with multi-tenant or enterprise feature.
- Cleaner domain modeling for an e-commerce platform.

By defining the custom model before the first migration, the project avoids one of the most comman architectural problems encountered in Djnago applications.

# Why UUID Instead of Interger IDs? 

Traditional integer primary keys are predictable.

EXAMPLE:
/api/users/1
/api/users/2
/api/users/3

This makes resource enumerations easier.

Using UUIDs produces indentifiers similar to:

550e23490-ew2456-mdd84459-567340899000

Advantage:

- Public-safe identifier
- Harder to enumerator resources
- Better suited for distributed systems.
- Easier future data migrations between services.
- Comman practice for public-facing APIs.

UUIDs improves security by making object identifiers significantly less predictable.

# Why E-mail is the login identifier?

E-mail is used as the primary authentication identifier because:

- Every custmor is expected to have unique E-mail.
- User are less likely to forgot the E-mail than a username.
- Password reset and verifications workflows naturally rely on the E-mail.
- It simplfies the authentication process.

The username will remain available as a public display name but will not be used for authentication.

Configuration:

- USERNAME_FIELD = "E-mail"
- email must be unique.


# User Roles

The system will intially support:

- Admin
- Seller
- Customer

Roles will implemented using Django's \\Textchoices .

Reasons for using \\Textchoices :
- Centralized defination for valid values.
- Built-in validation
- Better readability
- IDE autocomplete 
- Easier maintenance
- Cleaner serializer and admin integration

Future roles can be added without changing the overall architecture.

EXAMPLES:

- DELIVERY_PARTNER
- SUPPORT
- MODERATOR
- MANAGER

# Soft Deletes

The project will support soft deletaion instaed of permanently removing the records.

Instead of deleting a user from the database, the record will be marked as inactive or deleted.

Benefits include:

- Preserving order history
- Maintaining audit trails 
- Supporting account recovery
- Preventing accidental data loss
- Meeting business and compliance requirements

A future implementaion may include:

- is_deleted
- deleted_at
- deleted_by

Inital implementaion will rely on Djnago's is_active field, with dedicated soft-delete fields added when required.

# User Schema (v1)
# Field | Type	| Description
id	| UUID	| Primary key
email	| EmailField	| Unique login identifier
username | CharField	| Public display name
first_name | CharField	| User first name
last_name |	CharField	| User last name
phone_number |	CharField	| Contact number
date_of_birth |	DateField	| Optional
profile_image |	ImageField	| User avatar
role |	TextChoices | CUSTOMER / SELLER / ADMIN
is_email_verified |	BooleanField	| Email verification status
is_active |	BooleanField	| Django authentication
is_staff |	BooleanField	| Admin panel access
is_superuser |	BooleanField	| Django permissions
last_login | DateTimeField	| Authentication tracking
created_at | DateTimeField	| Record creation timestamp
updated_at | DateTimeField	| Last modification timestamp

# Planned | Database | Indexes

The following fields should be indexed for performance:

Field | Reason
email |	Authentication lookup
username |	Profile lookup
role |	Filtering users by role
is_active |	Authentication and administration
created_at | Sorting and reporting

Composite indexes may be added later based on query patterns.

Examples:

- (role, is_active)
- (created_at, role)

# Future Extensibility

The user domain is designed to support future features without requiring major architectural changes.

Planned enhancements include:

- Email Verification
- Password reset
- Two-factor authentication(2FA)
- OAuth (Google, Github, Apple)
- Social login
- User profile management
- Address management
- Multiple phone numbers
- Seller verification
- Customer loyalty program
- Audit logging
- Login history
- Device management
- Account suspension
- Multi-factor authentication
- Notification preference
- API tokens
- Session management

# Engineering Principles

The authentication module should follow these principles:

- Seperations of concerns
- Business logic in services
- Thin views
- Resuable selectors
- Explicit permissions
- Testable architecture
- Environment-based configuration
- Secure defaults 
- Production-ready coding standards

# Summary

The Shopsphere authentication system is designed to be flexible, secure and maintainable from the start. By adopting a custom user model, UUID primary keys, email-based authentication, role-based authorization using TextChoices , and a scalable architecture, the project establishes a solid foundation for all the future models.

This design prioritizes long-term maintainability and aligns with production-oriented Django development practices.