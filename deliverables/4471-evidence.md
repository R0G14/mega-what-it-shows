# #4471 export analysis

**Snapshot:** exported 2026-10-04; not a live view.
**Source:** the supplied `support-kb/src/lib/data/reports-4471.json`, tenant JSON files,
`desk-notes.json`, and the `causes.ts`, `CauseTable.svelte`, and `QueueTimeline.svelte` logic.

## Count reconciliation

- Raw export rows: 427.
- Duplicate report IDs: 15 extra rows across 15 IDs; all duplicate copies match and none has
  conflicting data.
- Unique reports after the same ID deduplication used by `CauseTable`: 412.
- Unique reports with `mergedInto === 4471`: 412; no other or unmerged reports remain.
- `groupByCause` produces exactly two groups, based on the tenant's locale and booking form:

| Cause | Reports | Tenants | Channels | Intake tags | Received |
|---|---:|---:|---|---|---|
| `parser` | 379 | 11 | app 204, email 150, form 25 | parser 356, hint 23 | 2026-04-01–2026-10-04 |
| `hint` | 33 | 3 | form 33 | hint 18, parser 15 | 2026-04-17–2026-09-22 |

The 33 hint reports are distributed across Alder Physio (12), Copper Studio (11), and Pale
Harbour (10). All three use booking-v3 in day-first locales. All other day-first tenants use
booking-v2 and fall into the parser group.

## Interpretation and checks

- `causeOf` classifies a day-first tenant's report as `hint` only when it came through a
  booking-v3 form; all other applicable reports are `parser`. This is why intake tags cannot be
  used as cause counts.
- The shared parser uses month-first interpretation for every date. The booking-v3 hint is
  `MM/DD/YYYY`, while the form reads date order using the day-first tenant locale: a distinct
  failure path, not a duplicate of the shared parser problem.
- `CauseTable` sorts groups by count. It therefore assigns #4471 to `parser`, marks the 33-report
  `hint` group as not belonging to that ticket, and notes five one-day-move IDs under `parser`.
  The five IDs in `desk-notes.json` are R-41202, R-41209, R-41236, R-41344, and R-41620.
- `QueueTimeline` yields 27 weeks from 2026-03-30 through 2026-10-04. It shows a bulk 189-report
  merge in the week of 2026-06-29, then continuing receipts through the last week in the export.

## Proposed tickets

1. Shared date parser flips dates for day-first tenants — 379 reports.
2. Booking-v3 date hint conflicts with day-first form parsing — 33 reports.

The report bodies are not copied into these draft deliverables; the draft evidence uses report
IDs and aggregated counts.
