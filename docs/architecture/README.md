# System Architecture

## Overview

The Booking Platform is a multi-tenant appointment-booking system. It is planned as a monorepo containing three application components:

- **Backend:** Provides the API, authentication, authorization, booking logic, availability calculations, and database access.
- **Dashboard:** Allows business owners and managers to configure their business and manage services, staff, schedules, and bookings.
- **Booking Widget:** Provides the customer-facing booking experience and can be embedded into an external business website.

The applications will share one backend and one database. The dashboard and booking widget will not connect directly to the database.

## High-Level Architecture

```text
Business Owner / Manager                 Customer
          |                                  |
          v                                  v
     Dashboard                         Booking Widget
          |                                  |
          +---------------+------------------+
                          |
                          v
                    Backend API
                          |
                          v
                     Database
```

## Application Responsibilities

### Backend

The backend is responsible for:

- Authentication and business-user authorization.
- Enforcing business and location data isolation.
- Managing businesses, locations, staff members, and services.
- Managing recurring schedules, schedule exceptions, and staff time off.
- Calculating availability from schedules, service duration, buffers, and existing bookings.
- Creating, confirming, rescheduling, and cancelling bookings.
- Providing API responses to the dashboard and booking widget.

### Dashboard

The dashboard is the private management interface for business owners and managers. It is responsible for:

- Business and location settings.
- Business-user access and roles.
- Staff and service configuration.
- Staff and location calendars.
- Schedule and time-off management.
- Viewing and managing bookings.

Staff members do not need their own dashboard accounts in the initial product scope.

### Booking Widget

The booking widget is the public customer-facing interface. It is responsible for:

- Displaying bookable services and service providers.
- Showing available appointment times.
- Collecting guest contact details and booking notes.
- Submitting booking requests to the backend.
- Providing customers with a secure way to cancel or reschedule a booking, subject to the business's policies.

The widget should be embeddable into websites that are not hosted by this platform.

## Main Data Ownership Boundaries

- A **Business** is the tenant boundary for data isolation.
- A **Location** is an operating branch or unit belonging to a business. A location may be physical or virtual.
- Staff, services, schedules, time off, and bookings are associated with the appropriate business and location.
- Business users belong to one business and have one role within that business in the initial design.
- Guest contact details are stored with each booking. A global customer account or cross-business customer profile is not part of the initial scope.

## Booking Flow (High Level)

1. A customer opens a business's booking widget.
2. The widget requests the business's available services and relevant staff from the backend.
3. The customer selects a service, provider (where applicable), and appointment time.
4. The backend calculates or revalidates availability.
5. The customer submits their contact details and booking request.
6. The backend validates the request and creates the booking and its booking item(s).
7. The dashboard displays the booking to the business.

The backend must revalidate availability when a booking is submitted. Availability shown in the widget may become outdated if another customer books the same time first.

## Deployment Approach

The repository is planned as a monorepo, but the applications may be built and deployed independently:

- Backend: application hosting, potentially using a container.
- Dashboard: frontend hosting.
- Booking widget: static hosting or a content-delivery network, depending on the final implementation.
- Database: a managed relational database service.

The hosting providers, deployment pipeline, container strategy, and production topology have not yet been selected.

## Current Architecture Decisions

- Use one repository for the product's applications and documentation.
- Keep the backend, dashboard, and booking widget as separate application components.
- Use one shared backend API and database for the dashboard and widget.
- Do not include third-party calendar synchronization in the initial scope.
- Provide built-in calendars using staff schedules, time off, and assigned bookings.

## Decisions Still to Be Finalized

- Technology stack for each application.
- Database engine and data-access approach.
- Whether location and staff availability use one generic schedule model or separate schedule models.
- Whether staff availability must always fit within location opening hours, or whether the platform supports explicit overrides.
- Authentication provider and session/token strategy.
- Hosting providers, environments, and deployment workflow.

See [`../decisions/README.md`](../decisions/README.md) for the decision register.
