---
schema: secure-review.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: secrev
attempt: 1
skill: secure-code-review@1.2.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/impl.md, sha256: 603d12bce385e4b5a6b74b10ce66a08c29fab4a69f57d6b55fba5d9ab524da1a}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The new count is a fixed SQL sub-select with no user input, bound to the server instant `$3`: `status = 'SCHEDULED' AND starts_at < $3`.", cites: ["sample-system:api/src/services/dashboardService.ts#L58"]}
  - {id: C2, text: "The bound `$3` is the server clock value `now`, passed as a query parameter.", cites: ["sample-system:api/src/services/dashboardService.ts#L59"]}
  - {id: C3, text: "The list filter accepts only the literal value `'true'` through `f.oneOf`, so other values are rejected as a field error.", cites: ["sample-system:api/src/services/appointmentService.ts#L40"]}
  - {id: C4, text: "The filter instant comes from the server, `startedBefore: started ? this.clock()`, never from the client.", cites: ["sample-system:api/src/services/appointmentService.ts#L50"]}
  - {id: C5, text: "The repository binds the new bound as a numbered parameter: `add('a.starts_at < ?', f.startedBefore)`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L75"]}
  - {id: C6, text: "The `add` helper pushes the value into `params.push(value)` and puts only a placeholder in the SQL.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L67"]}
  - {id: C7, text: "The list stays paginated: `LIMIT` and OFFSET are bound parameters.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L82"]}
  - {id: C8, text: "The appointment list route still declares `appointments:read`.", cites: ["sample-system:api/src/routes/appointments.ts#L12"]}
  - {id: C9, text: "The dashboard route still declares `dashboard:read`.", cites: ["sample-system:api/src/routes/dashboard.ts#L10"]}
  - {id: C10, text: "In the MVP every request is the actor `staff` and every permission is granted.", cites: ["context/architecture.md#identity"]}
  - {id: C11, text: "Logs carry method, route and error message only, `never request bodies`.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C12, text: "Invalid input is 400 with one message per field (`ValidationError`).", cites: ["context/conventions.md#errors"]}
  - {id: C13, text: "The web page passes only whitelisted URL keys to the API, now including `'started'`.", cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L14"]}
  - {id: C14, text: "The new count, like the existing Upcoming count (`starts_at > $3`), has no lower time bound.", cites: ["sample-system:api/src/services/dashboardService.ts#L54"]}
trace: [REQ-2, REQ-6, REQ-10, REQ-11]
known: [REQ-2, REQ-6, REQ-10, REQ-11]
assumed: []
unknown: []
questions: []
scans:
  - {kind: sast, report_sha256: 908efb4a4bcecfc5d36ebda494bc1100c7e0ddef0810a24584ca6ad5b11c8f52}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - {id: SF-1, severity: minor, text: "The awaiting_outcome count has no lower bound on starts_at, so every dashboard load counts all past Scheduled rows; cost grows with uncompleted history. Same shape as the existing Upcoming count; watch it, or bound it if the table grows.", cites: ["sample-system:api/src/services/dashboardService.ts#L58"]}
---
# Secure code review: HMS-21

## Summary
The change carries low security risk. It adds one count to `GET /api/dashboard` and one optional filter to `GET /api/appointments`. Neither adds a route, a permission, a dependency, a log line or a new response field with personal data.

The only finding is SF-1 (minor), a performance point rather than a weakness: the new count has no lower time bound. The threat model already accepted this risk and asked the tech lead about it (Q-05).

The SAST and secrets scans from the verifier are attached by hash, unchanged.

## Findings
- SF-1 (minor), `sample-system:api/src/services/dashboardService.ts#L58`: the `awaiting_outcome` sub-select has no lower bound on `starts_at`, so each dashboard load counts every past Scheduled row (C14). The existing Upcoming count has the same shape. Fix direction: keep watching it. If outcomes are often left unrecorded and the table grows, add a lower bound or an index on (status, starts_at).

What was checked and found sound:
- **Injection.** The count has no user input and binds the server's `now` as a parameter (C1, C2). The list filter takes only the literal `true` (C3). The filter instant is the server's clock, never a client value (C4), and it is bound through the existing parameterised helper (C5, C6). The list stays paginated (C7).
- **Authorisation.** Both routes still declare their read permission (C8, C9). In the MVP every request is staff with every permission granted (C10). The change neither widens nor narrows this.
- **Errors.** An invalid `started` value gives the standard 400 field error, which echoes no input (C12).
- **Logging and data.** No new log lines (C11). The dashboard returns a number. The list rows are unchanged, so no contact details or medical history are added.
- **Web.** `started` is passed through the existing URL whitelist (C13). React renders the count as text.
- **Dependencies.** No package or lock file changed.

## Known
- REQ-2: the count is a fixed, parameterised query (C1, C2).
- REQ-6: the list filter is validated and server-bound (C3 to C7).
- REQ-10, REQ-11: no new fields or logs carry contact details or medical history (C11).

## Assumed
None.

## Unknown
None.

## Open questions
None. The unbounded-count question is already open as the threat step's Q-05 for the tech lead.
