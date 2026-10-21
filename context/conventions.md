---
schema: context-section.v1
section: conventions
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# Conventions {#conventions}

## Code layout {#code-layout}
TypeScript strict everywhere. One router per resource in `api/src/routes`, mounted with a literal
path; rules in `api/src/services`; SQL in `api/src/repositories`; shared helpers in `api/src/lib`.
Sources: sample-system:api/src/app.ts#L26-L32

## Errors {#errors}
API errors are JSON `{ "error": { "code", "message", "fields"? } }`. Invalid input is 400 with one
message per field (`ValidationError`), unknown ids 404, conflicts 409.
Sources: sample-system:api/src/lib/errors.ts#L1-L25

## Lists {#lists}
Lists answer `{ items, page, page_size, total }`, 20 per page.
Sources: sample-system:api/src/config.ts#L41

## Database changes {#database-changes}
New SQL files only, never edits to merged ones. Names start with a UTC timestamp
(`YYYYMMDDTHHMMSS__what.sql`). The runner applies pending files in name order, one transaction each,
under an advisory lock.
Sources: sample-system:db/scripts/migrate.ts#L11-L16

## Logging and data {#logging-and-data}
Logs carry method, route and error message only, never request bodies, contact details or medical
history.
Sources: sample-system:api/src/middleware/errorHandler.ts#L19-L20

## Open points {#open-points}
- Branch naming and commit message rules beyond the `tsl/` run branches are not written down.
