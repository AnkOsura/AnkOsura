# Product Overview

## What We Are Building

The Booking Platform is a multi-tenant appointment-booking product for businesses that provide services by appointment. Examples may include salons, barbershops, and clinics.

A business can configure its services and service providers, define operating hours and availability, and let customers book appointments through an embeddable booking widget or through the business dashboard.

## Product Goals

- Give each business a way to manage its appointment operations.
- Let customers book services without needing to create an account.
- Support multiple businesses while keeping their data isolated.
- Support businesses with multiple operating locations.
- Provide built-in calendars for individual service providers and locations.
- Keep the initial product focused on appointment scheduling rather than becoming a full business-management suite.

## Users

### Business Owners

Owners configure the business, manage locations, invite or create management users, configure services and staff, and oversee bookings.

### Business Managers

Managers use the permissions assigned to their role to manage day-to-day operations, such as services, staff schedules, time off, and bookings.

### Customers

Customers book as guests through the booking widget. They provide contact details for each booking and may use a secure link to cancel or reschedule, subject to the business's policies.

### Staff Members

Staff members are service-provider records managed by owners or managers. They do not need separate login accounts in the initial version.

## Core Product Capabilities

### Business and Location Setup

- A business is the tenant using the platform.
- A business may operate multiple locations.
- A location represents an operating unit and may be physical or virtual.
- Locations can have their own staff, services, schedules, and bookings.
- The initial design uses one business timezone and one currency per business.

### Staff and Services

- Businesses can create and manage staff-member records.
- Businesses can create services offered at their locations.
- A service can be provided by more than one staff member.
- A staff member can provide more than one service.
- Service categories, variants, and add-ons are being considered; their exact MVP scope remains to be finalized.

### Scheduling and Calendars

- Businesses can define recurring location opening hours and staff working hours.
- Schedules may contain multiple working periods in one day, allowing breaks between periods.
- Businesses can make date-specific schedule changes, such as holidays or special opening hours.
- Managers can record time off for individual staff members.
- The platform calculates availability from schedules, exceptions, time off, service duration, buffers, and existing bookings.
- Individual available slots are not intended to be stored as permanent records.
- The dashboard provides built-in staff calendars and a way to view bookings across a location.

### Booking Management

- Customers can book one or more services in a single booking.
- Different staff members may handle different services within the same booking.
- Customers book as guests; contact details are stored with the booking.
- Business users can create and manage bookings from the dashboard.
- Customers can cancel or reschedule through a secure link, subject to business policies.
- Booked prices and durations are preserved even if the service is edited later.

## Initial Booking Lifecycle

The working lifecycle is:

```text
Pending → Confirmed → Completed
              |
              ├── Cancelled
              └── Rescheduled
```

The exact status transitions and how a reschedule is represented in the database will be finalized during booking design. No-show tracking is not currently planned for the initial version.

## Initial Product Scope

The first release focuses on:

- Multi-tenant business setup.
- Multiple locations per business.
- Business owner and manager accounts with role-based access.
- Staff and service configuration.
- Location and staff schedules.
- Schedule exceptions and staff time off.
- Availability calculation.
- Guest booking through an embeddable widget.
- Booking management through the dashboard.
- Customer cancellation and rescheduling through secure links.
- Basic audit records for important changes.

## Explicitly Out of Scope for the Initial Version

- Online payments, invoices, and refunds.
- Customer accounts, CRM, or cross-business customer history.
- Staff login accounts or a staff self-service portal.
- External calendar synchronization, including Google Calendar integration.
- Rooms, chairs, equipment, or other bookable resources.
- Employee-submitted leave requests and approval workflows.
- Advanced reporting and analytics.
- Notification delivery history and reminder automation.

These features may be considered later, but they should not expand the initial implementation without an explicit product decision.

## Product Constraints and Principles

- Each business must only access its own data.
- A business may access appropriate data across its own locations.
- Public widget requests must not be allowed to bypass booking validation or tenant boundaries.
- Availability must be revalidated by the backend when a booking is submitted.
- The platform should not store more customer information than is needed to manage a booking.
- Features should remain focused on appointment scheduling in the initial release.

## Open Product Questions

- Must staff availability always fit within location opening hours?
- Can a customer select multiple services that are performed by different staff members in one checkout flow?
- Are service variants and add-ons required for the MVP?
- Which booking and cancellation policies can businesses configure?
- What exact rules govern booking confirmation, rescheduling, and cancellation?
