# Draft issue: Day-first tenant dates are flipped by the shared parser

**Reports covered: 379**

## Problem

The shared date parser interprets dates month first, even for tenants whose locale writes the
day before the month. These reports describe dates being flipped when booking dates are parsed
and sent in calendar invites or reminders.

## Evidence

- The export contains 412 distinct reports merged into #4471 after deduplicating repeated report
  IDs. The tenant locale and booking-form rules classify 379 of those reports as this parser
  problem.
- The reports span 11 day-first tenants and channels: 204 app, 150 email, and 25 form reports.
- Examples include reports `R-41200` and `R-41202`. Intake tags are only guesses; 23 reports in
  this group were tagged `hint` despite matching the parser cause.
- The reports were received from 2026-04-01 through 2026-10-04 in the supplied export.

## Expected behavior

Parse a date according to the tenant's date-order contract so a day-first date cannot be read as
month first. Keep the booking-v3 form-hint issue separate; it has its own report count and
failure path.
