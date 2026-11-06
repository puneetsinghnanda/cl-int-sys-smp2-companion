---
schema: secure-review.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impl.md
    sha256: 64eabc5d66a4d1a05ab0755635b9faf8985e6940de2426991f73b10a90101aad
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The service validates status against a fixed list extended only by `NOT_CANCELLED`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L36"]
  - id: C2
    text: "The NOT_CANCELLED branch pushes the fixed SQL text `a.status <> 'CANCELLED'` and takes no request value."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L72"]
  - id: C3
    text: "The repository filter type is widened to `AppointmentStatus | 'NOT_CANCELLED'`, so only these values type-check into the query builder."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L44"]
  - id: C4
    text: "The list route keeps `requirePermission('appointments:read')`; the change adds no route."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C5
    text: "In the MVP every request is the actor `staff`, so authorisation is unchanged by this ticket."
    cites: ["context/architecture.md#identity"]
  - id: C6
    text: "Picker renders the doctor label as React text, `{selectedLabel}`, so it is escaped on output."
    cites: ["sample-system:web/src/components/Picker.tsx#L27"]
  - id: C7
    text: "Logs carry method, route and error message only, `never request bodies`; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "Invalid input is answered as a 400 `ValidationError`, so an unknown status does not reach SQL."
    cites: ["context/conventions.md#errors"]
  - id: C9
    text: "The list page derives the doctor label only from `doctor.data`; a failed lookup is not shown to the user."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L39"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`; the change adds no new client or dependency."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-14]
known: [SF-1, SF-2]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 3ac4e55643ebe6f40af6962dc83283ca40defc7689d48bed4e2722e2f28c854c
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "A failed doctor lookup (unknown or malformed doctorId in the URL) is silently ignored: the list stays filtered but the chooser shows no name and no error; show the lookup error or the list's ValidationError next to the chooser."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L39"]
  - id: SF-2
    severity: minor
    text: "The URL doctorId is converted with Number() and sent to GET /api/doctors/:id without a client-side integer check; the API still rejects bad ids, but guarding with Number.isInteger before the lookup avoids a needless failing request."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L30"]
---
# Secure code review: HMS-8

## Summary
Overall risk is low. The SAST and secrets reports from the verifier are attached by hash. The change adds no route, dependency, logging or secret (C4, C7, C10). The new status value goes through the fixed allowed list and a 400 for anything else (C1, C8). The NOT_CANCELLED branch uses fixed SQL text with no request value, and every other value stays a bound parameter (C2, C3). The doctor label is rendered as escaped React text (C6). Authorisation is unchanged (C4, C5). The two findings are minor robustness points on the list page, not exploitable weaknesses.

## Findings
- SF-1 (minor): a failed doctor lookup is silently ignored, so staff see a filtered list with no doctor name or error; show the error (sample-system:web/src/pages/AppointmentsPage.tsx#L39, C9).
- SF-2 (minor): doctorId from the URL is not checked as an integer before the lookup; the API rejects it, but a client-side guard avoids the failing call (sample-system:web/src/pages/AppointmentsPage.tsx#L30).

## Known
- SF-1: read from the changed line; only `doctor.data` feeds the label (C9).
- SF-2: read from the changed line; the API's own id check remains the control.

## Assumed
None.

## Unknown
None.

## Open questions
None.
