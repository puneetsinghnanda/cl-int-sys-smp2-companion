---
schema: secure-review.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/impl.md
    sha256: 51eee980084e44b77b1b62655028e5caf19eb3b25ffe99e828772b7c8a3082a5
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new list condition is added through the parameterised helper as `a.starts_at < ?`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L75"]
  - id: C2
    text: "The helper replaces the placeholder with a numbered bind parameter `$${params.length}`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L68"]
  - id: C3
    text: "The service accepts `started` only through `f.oneOf('started', ['true'] as const)`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L40"]
  - id: C4
    text: "The bound passed to the repository is the server clock, `this.clock()`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L50"]
  - id: C5
    text: "The dashboard count uses a fixed SQL text with the server now bound as `starts_at < $3`."
    cites: ["sample-system:api/src/services/dashboardService.ts#L58"]
  - id: C6
    text: "The appointments list route declares `appointments:read`."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C7
    text: "The dashboard route declares `dashboard:read`."
    cites: ["sample-system:api/src/routes/dashboard.ts#L10"]
  - id: C8
    text: "List rows are mapped by `toSummary`, which returns ids, codes, names, times and status only."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L28"]
  - id: C9
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C10
    text: "Logs never carry request bodies, `contact details` or medical data."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C11
    text: "Routes call `requirePermission`; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C12
    text: "The tile link carries only fixed values, `started: 'true'`."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L64"]
trace: [REQ-1, REQ-8, REQ-12, REQ-13]
known: [REQ-1, REQ-8, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 3477f0046ed46ec999b4cb936de656653459c72682bea2c898dc48a701ac89a4
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings: []
---
# Secure code review: HMS-21

## Summary
Overall risk is low. There is no finding attributable to this change. The SAST and secrets scans from the verifier are attached by hash and unchanged; tsl holds their results.

What was checked in the changed code:
- **Injection:** the new list condition goes through the existing parameterised helper (C1, C2), and the dashboard count is fixed SQL with the bound value passed as a parameter (C5). No user text reaches SQL.
- **Input validation:** `started` is accepted only as the literal `true`; any other value is a 400 field error (C3, C9).
- **Tampering with "now":** the bound comes from the server clock, never from the client (C4).
- **Authorisation:** no route was added. The list and dashboard routes keep their existing permissions (C6, C7). In the MVP every permission is granted to staff (C11); that is existing and unchanged by this ticket.
- **Data exposure:** the started list reuses `toSummary`, so rows carry no contact details or medical history (C8). The dashboard gains only a count.
- **Logging and errors:** no logging was added, so the logging rule holds (C10). Errors use the existing field-error shape (C9).
- **Web:** the tile link is built from fixed values only (C12); React escapes rendered text.
- **Dependencies:** no dependency or lock file changed.

## Findings
None.

## Known
- REQ-1: the count is a fixed, parameterised query (C5).
- REQ-8: the started filter is validated and bound to the server clock (C1 to C4).
- REQ-12: the started list carries no contact details (C8).
- REQ-13: the started list and dashboard carry no medical history (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.
