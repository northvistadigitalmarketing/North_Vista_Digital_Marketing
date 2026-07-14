# Client Profile — {{CUSTOMER_NAME}}

> Read/written by Boost skills. Krista appends; humans read.

## Profile

- **Client:** {{CUSTOMER_NAME}}
- **Slug:** {{CUSTOMER_SLUG}}
- **Domain:** {{DOMAIN}}
- **Provisioned:** {{PROVISIONED_DATE}}
- **Active Lines:** {{ACTIVE_LINES}}

## Business Summary

_Populated at intake._

## Brand & Voice Notes

_Populated by first content deliverable; refined over time._

## Delivery Log

| Date | Line | Deliverable | Commit | Krista Execution | Status |
|---|---|---|---|---|---|

<!-- Delivery Log schema v2 (2026-07-10):
  Commit = git SHA of the delivering commit, or "—" when the deliverable produced no git artifact.
  Krista Execution = execution_id · task_id · report_id (whichever exist, "·"-separated; "—" for
  local skill runs with no Krista conversation). Every Krista conversation execution MUST log its
  IDs here in the same turn it completes, and the same record is written to the KME on the Krista
  side — commit SHA is the join key from Krista to git; task/report IDs are the join key from git
  to Krista. A run is not "done" until its identity exists in both stores. -->