---
schema: secure-review.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/impl.md
    sha256: 2d2c9c19b6546cddf013f0ee6fe1041dfc59d388f1933e82c049e2c70345a311
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "Every API request passes through `identify` and routes call requirePermission; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "The doctor detail route requires `doctors:read`."
    cites: ["sample-system:api/src/routes/doctors.ts#L30"]
  - id: C3
    text: "The day-schedule slots route requires `appointments:read`."
    cites: ["sample-system:api/src/routes/doctors.ts#L64"]
  - id: C4
    text: "The summary is computed from appointments read by `occupyingBetween`, but only counts leave the service."
    cites: ["sample-system:api/src/services/doctorService.ts#L174"]
  - id: C5
    text: "Each entry returned holds only date, `available`, booked and free."
    cites: ["sample-system:api/src/services/doctorService.ts#L180"]
  - id: C6
    text: "The page reads ?date= only through `validDay`, which accepts a real YYYY-MM-DD day and otherwise returns null."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L30"]
  - id: C7
    text: "Logs carry `method, route and error message` only; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "The change adds no new route; the summary rides on the existing doctor detail `nextSevenDays` field."
    cites: ["sample-system:api/src/services/doctorService.ts#L75"]
  - id: C9
    text: "The appointment query for the summary is parameterised, using `$1` placeholders for the doctor and range."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L117"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: [{id: U-1, blocking: false}]
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "When sign-in splits roles, should the per-day booked/free counts on the doctor detail need appointments:read, like the day schedule, or stay under doctors:read, like the HMS-7 No-shows count?"
    why: "In the MVP every permission is granted (C1), so nothing changes today; after sign-in, a doctors:read-only user would see appointment-derived load (C2, C3)."
    evidence: ["sample-system:api/src/routes/doctors.ts#L30", "sample-system:api/src/routes/doctors.ts#L64", "context/architecture.md#identity"]
    options: ["Keep doctors:read (aggregate counts only, as HMS-7 does)", "Return null unless the caller also has appointments:read"]
    default: "Keep doctors:read (aggregate counts only, as HMS-7 does)"
scans:
  - kind: sast
    report_sha256: 77c61cefa93770e71567f9512bce62c9e9c1cd075c74b2cef4c3102e5b28d91b
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "Booking counts are served under doctors:read while the day schedule needs appointments:read; settle Q-01 before roles split."
    cites: ["sample-system:api/src/routes/doctors.ts#L30", "sample-system:api/src/services/doctorService.ts#L75"]
---
# Secure code review: HMS-9

## Summary
The overall risk is low. The SAST and secrets scans from the verifier both passed, and are attached by hash. The change adds no route, dependency, logging or SQL.

It reuses the parameterised appointment query (C9) and returns only aggregate counts per day (C4, C5). On the web side, `?date=` passes through the strict `validDay` check before use (C6), and is rendered as text.

The one finding is SF-1, a permission-scope question that has no effect in the MVP, because every permission is granted (C1). It becomes relevant once sign-in splits roles, and it is raised as Q-01.

## Findings
- **SF-1 (minor)** at `sample-system:api/src/routes/doctors.ts#L30` and `api/src/services/doctorService.ts#L75`. Booking counts derived from appointments are served under `doctors:read` (C2), while the day schedule uses `appointments:read` (C3). Fix direction: settle Q-01. If the stricter permission is chosen, return `nextSevenDays: null` to callers without `appointments:read`.

Checked, with no finding:
- **Input validation:** `?date=` (C6). The server route takes no new input.
- **Output encoding:** the dates and counts are rendered as React text.
- **Error leakage:** no new error paths; an unknown doctor is still 404.
- **Logging:** none added (C7).
- **Dependencies:** no change.
- **Write bodies:** `nextSevenDays` is ignored on PATCH (covered by an impl test).

## Known
- REQ-1 to REQ-10: the changed code for every requirement was read at head. The scans are attached unchanged.

## Assumed
None.

## Unknown
- U-1: Which permission should guard the summary once roles exist. Raised as Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): When sign-in splits roles, should the counts need `appointments:read`, or stay under `doctors:read` like HMS-7's No-shows count? Default: keep `doctors:read`.
