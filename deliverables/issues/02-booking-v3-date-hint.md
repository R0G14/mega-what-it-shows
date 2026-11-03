# Booking-v3 date hint conflicts with day-first form parsing

**Reports covered: 33**

## Problem

The booking-v3 date field tells users to enter `MM/DD/YYYY`, but the form interprets submitted
dates in the tenant's day-first locale order. A user following the displayed hint can therefore
submit a date that the form reads with the month and day reversed.

## Evidence

- The export contains 412 distinct reports merged into #4471 after deduplicating repeated report
  IDs. Tenant locale and form rules classify 33 form reports as this separate hint problem.
- The reports come from the three day-first tenants using booking-v3: Alder Physio (12), Copper
  Studio (11), and Pale Harbour (10).
- Examples include reports `R-41230` and `R-41241`. Intake tags are not authoritative: 15 of
  these reports were tagged `parser`.
- The reports were received from 2026-04-17 through 2026-09-22 in the supplied export.

## Expected behavior

Make the form's displayed date format agree with the order used to parse its input, or otherwise
make the input unambiguous. Track this independently from the shared parser issue.
