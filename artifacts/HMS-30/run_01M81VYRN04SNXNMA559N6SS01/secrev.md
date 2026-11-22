---
schema: secure-review.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/impl.md
    sha256: b03701e7048b60a921f2709374b064311f7ced727534936ab87c31ece1c13f20
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The list route still declares `requirePermission('appointments:read')`; no new route was added."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C2
    text: "Routes call `requirePermission`, and in the MVP every permission is granted to the staff actor."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The list and the counts share one parameterised WHERE builder, `listWhere`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L78"]
  - id: C4
    text: "The counts query interpolates only that built clause and passes values as parameters, grouping by `GROUP BY a.status`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L95"]
  - id: C5
    text: "The status filter is still validated against the enum with `f.oneOf` before any query runs."
    cites: ["sample-system:api/src/services/appointmentService.ts#L40"]
  - id: C6
    text: "Each count link is built on the fixed in-app path `/appointments?`, so no open redirect is possible."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L22"]
  - id: C7
    text: "Logs carry method, route and error message only, `never request bodies`; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
trace: [REQ-1, REQ-4, REQ-5, REQ-7]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - {kind: sast, report_sha256: 003560778edaef3de02e31149411fcf68b06e90454ffb04ac4d198d95465036e}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "Unfiltered list calls add a full-table GROUP BY; acceptable now, add an index or require a filter if the table grows."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L95"]
---
# Secure code review: HMS-30

## Summary
Overall risk is low. The change adds one read-only aggregate to an existing authorised route. It adds no new route, input, dependency or logging. The SQL stays parameterised through the shared `listWhere` builder (C3, C4). Status input is still checked against the enum (C5). The response adds only four integers per status and no contact details or medical history. The web links are built on a fixed in-app path from the current query string, and React escapes the rendered text (C6). The SAST and secrets scans from the implement step's verifier run are attached unchanged. The only finding is SF-1, minor: an extra full-table aggregate when no filters are given.

## Findings
- SF-1 (minor): an unfiltered list now also runs a GROUP BY over all appointments. This is a small, repeatable extra load. It matches the cost of the existing total count, so the fix is to keep it and revisit with an index or a required filter if the table grows. Cite: sample-system:api/src/repositories/appointmentRepo.ts#L95.

Checked with no issue:
- Authorisation: the list route still declares `appointments:read`, and no route was added (C1, C2).
- Injection: values are passed as `$n` parameters; only the generated clause is interpolated (C3, C4).
- Input validation: the status and id parsing is unchanged (C5).
- Output and data exposure: `status_counts` holds integers only; no personal data or medical history.
- Client: the `Link` href is always `/appointments?…`, built with URLSearchParams (C6).
- Logging and errors: no new logging; errors still go through the shared handler (C7).
- Dependencies: none added or changed.

## Known
- SF-1: read directly from the changed query at head; its cost matches the existing count query.

## Assumed
None

## Unknown
None

## Open questions
None
