---
schema: secure-review.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/impl.md
    sha256: 307cbd9b69244e89b5711f193ecc2892cec5e4ff5e2c48f7553f8625f7587629
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The weekday filter value reaches SQL only as a bound parameter through `p(f.weekday)`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L95"]
  - id: C2
    text: "The service accepts weekday only from a fixed list of seven strings via `oneOf` before it reaches the repository."
    cites: ["sample-system:api/src/services/doctorService.ts#L47"]
  - id: C3
    text: "The weekdays query passes the doctor ids as one array parameter (`ANY($1)`), not as interpolated text."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L151-L152"]
  - id: C4
    text: "The changed list route still declares `requirePermission('doctors:read')`; no route was added."
    cites: ["sample-system:api/src/routes/doctors.ts#L14"]
  - id: C5
    text: "Routes guard access with `requirePermission`; in the MVP every request is staff."
    cites: ["context/architecture.md#identity"]
  - id: C6
    text: "Logs carry method, route and error message only, `never request bodies`, contact details or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C7
    text: "The Works on cell renders a computed string as React text (`worksOn(d.weekdays)`), not as HTML."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L98"]
  - id: C8
    text: "A rejected weekday yields the fixed message `must be one of` the allowed values, without echoing the input."
    cites: ["sample-system:api/src/lib/validate.ts#L65"]
  - id: C9
    text: "The change is limited to the doctor list code, starting with `DoctorFilter`, and adds no dependency."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/impl.md#claim:C1"]
trace: [REQ-4, REQ-6, REQ-7, REQ-9, REQ-10]
known: [REQ-4, REQ-6, REQ-7, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: f2552014a47af75a7f12073101dbb44a9ab4d6a75786706911c0df126f72a288
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings: []
---
# Secure code review: HMS-28

## Summary
Overall risk is low, and there are no findings.

The only new untrusted input is the `weekday` query parameter. It is checked against a fixed list of seven values (C2) and reaches the database only as a bound parameter (C1). The new weekdays query also uses a single array parameter (C3).

No route was added: the changed list route keeps its read permission (C4, C5). Output is rendered as React text (C7). A rejected value gets a fixed error message that does not echo the input (C8). Nothing new is logged, which keeps to the logging rule (C6). No dependency changed (C9).

The SAST and secrets scans from the verifier are attached by hash, unchanged. Because I attached the reports by hash and did not read them, this artifact does not restate what they found.

## Findings
None. Checks made on the diff:
- input validation: C2;
- SQL injection: C1, C3;
- authorisation on the changed route: C4, C5;
- output encoding: C7;
- error leakage: C8;
- logging of personal data: C6;
- dependency changes: C9.

## Known
- REQ-4: the new Works on filter input is allow-listed to "1" to "7" (C2), and a rejected value does not reflect the input (C8).
- REQ-6: the weekday filter is a bound SQL parameter (C1).
- REQ-7: the combined filters keep the existing parameterised WHERE clause, and the route keeps `requirePermission('doctors:read')` (C1, C4).
- REQ-9: the page count uses the same parameterised clause; the weekdays query binds the id list as one array parameter (C3).
- REQ-10: the weekday in the page address is rendered and sent as plain text and values (C7); no new log line was added (C6).

## Assumed
None

## Unknown
None

## Open questions
None
