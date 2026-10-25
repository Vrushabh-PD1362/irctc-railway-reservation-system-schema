# IRCTC Railway Reservation System — Database Schema Design

## Problem Statement

Design a database schema for a railway reservation system that handles trains, schedules, bookings, and seat allocation.

### Requirements

- **Stations:** Store station codes, names, cities, states, and geographical coordinates.
- **Trains:** Maintain train numbers, names, operators, and service types such as Express, Superfast, and Local.
- **Routes and schedules:** Represent ordered station stops, arrival and departure times, day offsets, and train services on specific dates.
- **Calendar exceptions:** Support added and cancelled services.
- **Coaches and berths:** Model coach types, coach sequences, berth positions, quotas, and berth layout metadata.
- **Fare calculation:** Account for classes, distance slabs, quotas, concessions, and fare snapshots at booking time.
- **Bookings and passengers:** Support PNR bookings, passenger details, travel segments, berth preferences, and ID proof types.
- **Booking status and allocation:** Track confirmed, RAC, and waitlisted positions, cancellations, and berth assignments.
- **Payments and refunds:** Record transactions, payment statuses, cancellation charges, and refund processing.
- **Boarding point changes:** Track changes to boarding stations and reflect them in coach allocation.
- **Chart preparation:** Manage allocation cut-off times and passenger charts by train run and coach.
- **Operational disruptions:** Support train cancellations, rescheduling, refunds, and rebooking.
- **Audit logging:** Record state changes with timestamps and operator identifiers.

## Solution

The database schema models the entities and relationships required to represent these railway reservation workflows. See [`irctc_railway_reservation_system_schema.md`](./irctc_railway_reservation_system_schema.md) for the complete schema and relationship cardinalities.
