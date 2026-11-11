# Technical and Product Decisions

## Purpose

This directory records important decisions that affect the product's architecture, database, technology stack, or development workflow.

The goal is to make decisions visible to the whole team and explain why the project is organized in a particular way. Decisions should be recorded when they are agreed upon, especially when changing them later would affect existing code or data.

## Decision Status

Use these labels when recording decisions:

- **Proposed:** A suggestion that has not been agreed upon.
- **Accepted:** The team has agreed to use it.
- **Rejected:** The team considered it and decided against it.
- **Superseded:** A newer decision has replaced it.

## Current Decision Register

| Topic | Current direction | Status |
|---|---|---|
| Repository organization | One monorepo containing backend, dashboard, booking widget, and documentation | Accepted |
| Application boundaries | Backend, dashboard, and booking widget are separate application components | Accepted |
| Backend/frontend technologies | Not selected yet | Pending |
| Database engine and migration tool | Not selected yet | Pending |
| Tenant boundary | Business is the tenant; backend and database constraints must prevent cross-business access | Accepted |
| Business locations | A business can have multiple operating locations; a location may be physical or virtual | Accepted |
| Business-user roles | Each BusinessUser has exactly one role in the initial design | Accepted |
| Staff accounts | Staff members do not have separate login accounts in the initial scope | Accepted |
| Customer records | Guest contact details are stored per booking; no global Customer account/profile | Accepted |
| Schedule model | Schedule rules and exceptions are required; exact ownership model is not finalized | Pending |
| Staff calendars | Built-in calendars are derived from staff schedules, time off, and assigned bookings | Accepted |
| Staff time off | A manager or owner records time off for a specific StaffMember | Accepted |
| Time-off categories | No separate TimeOffType entity for the initial scope | Accepted |
| External calendar integration | Excluded from the initial scope | Accepted |
| Availability | Calculate availability dynamically and revalidate when a booking is submitted | Accepted |
| Booking contents | One Booking can contain multiple BookingItems, potentially assigned to different staff members | Accepted |
| Price and duration snapshots | Preserve booked price and duration on booking records | Accepted |
| Timezone convention | Store actual appointment timestamps in UTC and use a business IANA timezone for local schedules and display | Proposed |
| Currency convention | One ISO 4217 currency code per business; preserve booked currency with the price snapshot | Proposed |
| Deployment | Components may deploy independently from the monorepo | Proposed |

## How to Record a Decision

For a significant decision, create a Markdown file in this directory using a descriptive name, for example:

`0001-use-a-monorepo.md`

Use this template:

```markdown
# [Decision title]

- Status: Proposed / Accepted / Rejected / Superseded
- Date: YYYY-MM-DD
- Decision owners: [Names or team]

## Context
What problem or question led to this decision?

## Options Considered
What realistic alternatives did the team consider?

## Decision
What did the team decide?

## Consequences
What becomes easier or harder because of this decision?

## Follow-up
Are there tasks or decisions that remain?
```

## Guidelines

- Record the reasoning, not just the final choice.
- Keep decisions focused on one topic.
- Update the status when a proposal is accepted or rejected.
- Do not silently rewrite an accepted decision. Mark it superseded and record the replacement decision.
- Small implementation details do not need an architecture decision record unless they have a meaningful long-term impact.
