---
schema: secure-review.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/impl.md
    sha256: 20681f0cdaf8c6dd6aa3000da161829ff3d487cbb97cc36ed2a30942e76c25ae
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The change adds an `onDuty` array to the existing GET /api/dashboard; no new route is added."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/impl.md#claim:C1"]
  - id: C2
    text: "The dashboard route still declares the `dashboard:read` permission."
    cites: ["sample-system:api/src/routes/dashboard.ts#L10"]
  - id: C3
    text: "In the MVP every request is the actor `staff` and every permission is granted; sign-in replaces identify and requirePermission only."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "The new weekday query passes the weekday as a bound parameter `[weekday]`, not by string concatenation."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L186"]
  - id: C5
    text: "The response row type `OnDutyRow` carries only id, name, department, counts and the next slot."
    cites: ["sample-system:api/src/types.ts#L83"]
  - id: C6
    text: "Logs never carry request bodies, `contact details` or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C7
    text: "The web renders the doctor name as React text inside a Link (`r.fullName`), so it is escaped."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L92"]
  - id: C8
    text: "onDutyToday loads each doctor's appointments one at a time with `occupyingBetween`."
    cites: ["sample-system:api/src/services/doctorService.ts#L178"]
  - id: C9
    text: "activeOnWeekday loads each doctor's blocks one at a time with `this.availability(d.id)`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L191"]
trace: [REQ-2, REQ-4, REQ-13]
known: [REQ-13]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: de4c01a3235d67bde525354569c3525499fc41a4b4061c7cea31ea18e035cffe
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "Each dashboard load (including every Period change) runs two queries per on-duty doctor, so cost grows with the number of doctors; batch blocks and appointments by doctor ids to bound it."
    cites: ["sample-system:api/src/services/doctorService.ts#L177-L178", "sample-system:api/src/repositories/doctorRepo.ts#L191"]
---
# Secure code review: HMS-23

## Summary
The overall risk is low. The change adds read-only data to the existing `GET /api/dashboard` route, which still declares `dashboard:read` (C1, C2). Under today's MVP identity, every request is `staff` (C3), so no new trust boundary appears.

What I checked:
- **SQL:** the new query binds the weekday as a parameter (C4).
- **Personal data:** the response row omits phone and email (C5), and nothing new is logged (C6).
- **Web output:** the page renders names as escaped React text (C7).

Both scans are attached by hash and unchanged. The only finding is minor: per-doctor queries on every dashboard load (SF-1).

## Findings
- **SF-1 (minor):** each dashboard load, including every Period change, runs two queries per on-duty doctor: `availability` and `occupyingBetween` (C8, C9). Cost therefore grows with the number of doctors. Fix direction: load the blocks and today's appointments for all on-duty doctors in one query each (for example, by an array of doctor ids). Cites: `sample-system:api/src/services/doctorService.ts#L177-L178`, `sample-system:api/src/repositories/doctorRepo.ts#L191`.

## Known
- **REQ-13:** the on-duty rows carry no contact details (C5). This is enforced by the `OnDutyRow` type and checked by the HMS-23 REQ-13 API test.

## Assumed
None.

## Unknown
None.

## Open questions
None.
