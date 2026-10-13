# IRCTC (Railway Reservation System) — Database Schema Design

## Overview

This project presents a relational database schema for a Railway Reservation System, designed to manage stations, trains, routes, schedules, coach and berth allocation, passenger bookings, fares, payments, and railway operations.

## Key Features

- Station, city, and state management with geo-coordinates
- Train, operator, and service type management
- Route planning with ordered station stops and day offsets
- Train schedules and calendar exceptions
- Coach types and coach sequence management
- Berth layouts, positions, and seat allocation
- Fare calculation based on classes, distance slabs, and quotas
- PNR bookings and passenger management
- Booking status tracking with RAC and waitlist positions
- Passenger berth allocation and queue management
- Payment processing and transaction tracking
- Cancellation charges based on time before departure
- Refund processing and refund status tracking
- Boarding-point changes and history
- Chart preparation and final passenger charts per coach
- Operational disruptions, including train cancellation and rescheduling
- Affected booking resolution through refunds or rebooking
- Audit logging with timestamps and operator identifiers

## Database Concepts Demonstrated

- Primary keys and foreign keys
- One-to-many and many-to-many relationships
- Junction tables
- Relational database normalization
- Unique constraints and data integrity
- Fare snapshots for historical consistency
- Status and lifecycle management
- Historical data tracking
- Transaction-sensitive seat allocation and queue management
- Audit logging

## Project File

- `irctc_railway_reservation_schema.md` — Complete schema definitions and relationship cardinalities.

## Purpose

This project demonstrates relational database design skills, including entity identification, relationship modeling, constraint planning, reservation lifecycle management, fare calculation, seat allocation, and operational exception handling.
