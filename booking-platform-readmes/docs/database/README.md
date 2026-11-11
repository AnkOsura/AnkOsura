# Database Design

## Purpose

This document records the initial database design direction for the Booking Platform. The entity list and relationships are being developed before the technology stack and physical schema are finalized.

The database must support multiple businesses while preventing one business from accessing another business's data.

## Core Entity Groups

### Business and Locations

- **Business:** The tenant and organization using the platform. It owns its locations, business users, and operational data.
- **Location:** A branch or operating unit belonging to a business. It can represent a physical or virtual operating location and has its own staff, services, schedules, and bookings.

### Management Access

- **BusinessUser:** A person who can sign in to manage one business.
- **Role:** Defines the user's responsibility, such as Owner or Manager.
- **Permission:** Represents an action that can be allowed or restricted.
- **RolePermission:** Connects roles to permissions.

Each BusinessUser has exactly one role in the initial design. A separate BusinessUserRole junction entity is therefore not planned unless the requirements change.

Whether roles and permissions need database tables or can initially be represented by fixed application-defined roles is still to be decided.

### Staff and Services

- **StaffMember:** An employee or service provider managed by the business. Staff members do not need individual login accounts in the initial scope.
- **Service:** A bookable service offered at a location.
- **StaffService:** Connects staff members to the services they are qualified or allowed to provide.
- **ServiceCategory:** Groups related services.
- **ServiceVariant:** Represents a separately selectable variation of a service, if required.
- **ServiceAddon:** Represents an optional extra that can be added to a service.

The exact need for ServiceVariant and ServiceAddon should be confirmed against the first version of the product requirements before implementation.

### Scheduling and Availability

- **ScheduleRule:** Defines recurring availability, such as a staff member's normal working periods or a location's normal opening hours.
- **ScheduleException:** Records a date-specific change to normal availability, such as a holiday closure or special opening hours.
- **TimeOff:** Records a period when a specific staff member is unavailable. It belongs to a StaffMember and may include an optional reason and the BusinessUser who created the record.

A separate TimeOffType entity is not planned for the initial version. A simple optional reason is sufficient unless businesses later need configurable leave categories.

The final relationship between schedules and their owners (Location or StaffMember) is still to be decided. Built-in staff calendars can be generated from schedule rules, exceptions, time off, and assigned bookings; a separate Calendar entity is not currently required.

### Bookings

- **Booking:** Represents the customer's overall appointment and stores guest contact information and booking-level details.
- **BookingItem:** Represents an individual service within a booking and identifies the assigned staff member.
- **BookingItemAddon:** Connects selected add-ons to a booked service, if add-ons are included in the initial release.
- **BookingPolicy:** Defines booking, cancellation, and rescheduling rules at the appropriate business or location scope.
- **BookingManagementToken:** Provides a secure way for a customer to manage a booking through a link, if this is implemented as a separate entity rather than fields on Booking.
- **AuditLog:** Records important actions and changes for traceability.

A booking may contain multiple services, and different staff members may handle different booking items within the same booking.

## Important Relationships

The following is a high-level guide. The physical ERD will define the final foreign keys and cardinalities.

```text
Business 1 ───< Location
Business 1 ───< BusinessUser
BusinessUser >─── 1 Role
Role >───< Permission       (through RolePermission)

Location 1 ───< StaffMember
Location 1 ───< Service
StaffMember >───< Service   (through StaffService)

StaffMember 1 ───< TimeOff
Booking 1 ───< BookingItem
BookingItem >─── 1 Service
BookingItem >─── 1 StaffMember
```

## Tenant Isolation and Data Integrity

Business isolation is a core requirement, not just an application convention.

The implementation should:

- Associate tenant-owned records with a `business_id`, directly or through a clearly enforced parent relationship.
- Scope all backend queries to the authenticated BusinessUser's business.
- Verify that related records belong to the same business before creating or updating relationships.
- Use foreign keys and, where appropriate, composite foreign keys or unique constraints to prevent cross-business references.
- Avoid trusting a `business_id` supplied by a public client or dashboard request without authorization checks.
- Add automated tests proving that a user from Business A cannot read or modify Business B's data.

Locations within the same business may share visibility where appropriate, while remaining distinguishable for location-specific services, staff, schedules, and bookings.

## Date, Time, Timezone, and Currency Conventions

These are the proposed conventions for the initial design and should be documented in the physical schema and application code:

- Store actual appointment start and end timestamps in UTC.
- Store an IANA timezone identifier for each business, such as `Asia/Dhaka`, and use it to interpret local business hours and display appointment times.
- Store recurring schedule times as local wall-clock times interpreted in the business timezone, rather than treating them as UTC timestamps.
- Use one ISO 4217 currency code per business, such as `BDT`.
- Preserve the booked price and duration on BookingItem as snapshots so later service edits do not rewrite historical bookings.

The final database types and exact timestamp strategy depend on the selected database engine.

## Booking Data Considerations

- Customer details are stored on each Booking. A global Customer entity and customer account system are out of scope for the initial version.
- A BookingItem should preserve the service name or reference, assigned staff member, duration, and price as they were when booked, as appropriate to the final schema.
- Availability is calculated from schedules, exceptions, time off, service duration, buffers, and existing bookings. Individual available slots do not need to be stored as permanent records.
- The backend must revalidate availability when creating or rescheduling a booking to prevent conflicting appointments.

## Initial Scope Exclusions

The initial database design does not include:

- Payments, invoices, or refunds.
- Customer accounts or a cross-business customer database.
- Staff login accounts.
- External calendar integration or calendar-event mappings.
- Rooms, chairs, equipment, or other bookable resources.
- Employee-submitted time-off requests and approval workflows.
- Notification delivery history.

## Migrations and Testing

Database migrations will be added after the database engine and migration tooling are selected. The initial migrations are expected to cover at least:

1. Businesses and locations.
2. Business users and access-control structures.
3. Staff members and services.
4. Scheduling rules, exceptions, and time off.
5. Bookings and booking items.

Tests should cover foreign-key integrity, required uniqueness rules, booking validation, and tenant isolation.
