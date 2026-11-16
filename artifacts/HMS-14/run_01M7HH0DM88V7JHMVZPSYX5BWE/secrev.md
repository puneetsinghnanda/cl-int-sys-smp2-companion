---
schema: secure-review.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impl.md
    sha256: a34a17f3621e8c2ea198aa79c62757daaea0286c964804d0f596c1292edb2fcc
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new SQL columns are static text with no request input: `d.status AS doctor_status, p.status AS patient_status`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L26"]
  - id: C2
    text: "SQL stays in repositories behind services: `repositories hold the SQL`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "The book page accepts the navigation-state reason only when it is text, cut to the field limit: `typeof given === 'string' ? given.slice(0, 300) : ''`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L28"]
  - id: C4
    text: "Book again puts only the visit reason in navigation state, not the URL: `state: a.reason ? { reason: a.reason } : undefined`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L61"]
  - id: C5
    text: "Any status other than ACTIVE blocks the button: `a.doctor_status !== 'ACTIVE'`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L24"]
  - id: C6
    text: "Every request is the MVP actor and routes call `requirePermission`; the change adds no route."
    cites: ["context/architecture.md#identity"]
  - id: C7
    text: "Logs never carry request bodies: `Logs carry method, route and error message only`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "The server still refuses an Inactive doctor on booking: `this doctor is Inactive and cannot be booked`."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/impl.md#claim:C4"]
  - id: C9
    text: "The reason is rendered as React text, which escapes it: `<dd>{a.reason ?? '—'}</dd>`."
    cites: ["sample-system:web/src/pages/AppointmentDetailPage.tsx#L81"]
  - id: C10
    text: "The reason leaves the URL as decided in `Navigation state (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-09"]
trace: [REQ-6, REQ-9, REQ-10, REQ-11, REQ-12, REQ-16, REQ-17, REQ-19]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: d4c0c4d336829398efcbd0de45dc48de7bb1db64d6fd712f545f01e4c056a222
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The reason for visit in navigation state is kept in the browser's session history entry until the tab closes; it is out of the URL and logs, but not erased after use. Fix direction, if wanted: replace the history entry without state once the book page has read it."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L27"]
---
# Secure code review: HMS-14

## Summary
Overall risk is low. The change adds no route, no dependency and no new SQL input:
- The two new columns are static text inside the existing repository query (C1, C2).
- The detail response gains only the two record statuses, with no patient contact or medical fields.
- The reason for visit stays out of the URL (C4, C10) and is never logged (C7).
- The book page accepts it only as text cut to the 300-character field limit (C3), and React renders it escaped (C9).
- The disabled state is a UI hint. The server still refuses Inactive doctors and patients on booking (C8), and the new client check fails closed for any status other than ACTIVE (C5).
- Authorisation is unchanged, with the MVP staff actor and requirePermission on existing routes (C6).
- The SAST and secrets reports from the verifier are attached unchanged.

The one finding (SF-1, minor) is that the navigation state stays in session history until the tab closes.

## Findings
- SF-1 (minor): the reason in navigation state stays in the browser's session history entry until the tab closes. It is not in the URL or the logs. Fix direction: replace the history entry without state after reading it, if the product owner wants it erased. Cite: sample-system:web/src/pages/BookAppointmentPage.tsx#L27.

## Known
- SF-1: seen directly in the changed line that reads `location.state`; the behaviour follows from the navigation-state choice in A-09 (C10).

## Assumed
None

## Unknown
None

## Open questions
None
