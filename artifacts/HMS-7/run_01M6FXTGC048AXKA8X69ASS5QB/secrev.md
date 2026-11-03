---
schema: secure-review.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md
    sha256: 62586154589c341a2efe7ee5696ec741d288b17b601ec7084da8a07b98468299
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes declare a permission with `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Logs carry method, route and error message only, never request `bodies`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "SQL lives in `api/src/repositories`."
    cites: ["context/conventions.md#code-layout"]
  - id: C5
    text: "The change adds the repository query `countByDoctor`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md#summary"]
  - id: C6
    text: "The page builds the link with `URLSearchParams`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md#summary"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
questions: []
scans:
  - {kind: sast, report_sha256: d1a30b3cdb2733bda5bd4371fdea3d156e8786002ba386d74fff554df8c4ecd2}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - id: SF-1
    severity: minor
    text: "The No-show count is served under the existing doctor read permission, which in the MVP every request has; when sign-in lands, decide whether attendance figures need their own permission."
    cites: ["sample-system:api/src/services/doctorService.ts#L76"]
  - id: SF-2
    severity: minor
    text: "Every doctor detail read, including the responses of create, update and availability changes, now runs one extra count query; it is indexed and bounded to one doctor, so keep it that way and do not widen it to a list."
    cites: ["sample-system:api/src/services/doctorService.ts#L73"]
---
# Secure code review: HMS-7

## Summary
Overall risk is low. The change adds one read-only aggregate (a count and two dates) to an existing
read route; it adds no route, no write path, no dependency and no logging. The verifier's SAST scan
has 4 results in the repository, none in a changed file; the secrets scan has no results. My reading
of the diff found two minor points and no blocker or major issue. The most important is SF-1: the
count rides on the doctor read permission, which in the MVP every request has, so its exposure is the
MVP's identity model rather than something this change introduces.

Checked and fine:
- Input validation and injection: the new count query passes doctor id, status and both bounds as bind parameters (`sample-system:api/src/repositories/appointmentRepo.ts#L141`); no string building.
- Authorisation: no new route; GET /api/doctors/:id keeps its requirePermission; the unknown-id 404 still happens before the count (`sample-system:api/src/services/doctorService.ts#L71`).
- Output encoding: the link query is built with URLSearchParams (`sample-system:web/src/pages/DoctorDetailPage.tsx#L14`) and the count renders as React text (`sample-system:web/src/pages/DoctorDetailPage.tsx#L72`).
- Personal data: the new fields hold an integer and two dates; no patient fields, nothing logged.
- Write path: the read-only fields are not read by PATCH, so they cannot be set by a client (covered by the unit test for PATCH).
- Dependencies: none added or changed.

## Findings
- SF-1 (minor): count exposed under the existing doctor read permission; revisit when sign-in is added — `sample-system:api/src/services/doctorService.ts#L76`.
- SF-2 (minor): one extra indexed count per doctor detail read; keep it per doctor — `sample-system:api/src/services/doctorService.ts#L73`.

## Known
- REQ-1 to REQ-8: the code serving them was read at head; no security weakness beyond SF-1 and SF-2.

## Assumed
None.

## Unknown
None.

## Open questions
None.
