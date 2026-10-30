---
schema: secure-review.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/impl.md
    sha256: 26fe0697b5932aacb3488d8d1530b60856684ba8bcb1ffd38247920447b82599
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Routes call `requirePermission`; sign-in replaces identify and requirePermission only."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "SQL is kept in `api/src/repositories`."
    cites: ["context/conventions.md#code-layout"]
  - id: C4
    text: "Logs carry method, route and error message only, never `request bodies`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C5
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C6
    text: "Instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
trace: [REQ-2, REQ-4, REQ-5, REQ-6, REQ-12]
known: [SF-1]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: Is it accepted for the MVP that every caller of GET /api/patients now also sees each patient's last visit date?
    why: In the MVP every request is staff with every permission, so the new field has no access control beyond the existing list.
    evidence: ["context/architecture.md#identity"]
    options: [accept for MVP, gate behind sign-in before release]
    default: accept for MVP; same exposure as the existing Next appointment field
scans:
  - kind: sast
    report_sha256: ce84b512e5a36a50bc8b813873e19afd82a6f92a64e27eb94eedd4df91f1f24e
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The list response now carries each patient's last visit date with no access control beyond the MVP's grant-all staff actor; accept for MVP or gate behind sign-in (Q-01)."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L129"]
---
# Secure code review: HMS-4

## Summary
**Overall risk: low.** The change is read-only. It adds one SQL subquery and one display column, with no new route, input, dependency, logging or write path. The only finding, SF-1, is minor: visit dates become visible under the MVP's grant-all access model, which is an accepted architectural state rather than a defect in this diff.

Both scans from the verifier are attached by hash below. I did not read their contents in this step, and tsl records their verdicts. I found no secret in the diff.

What I checked in the diff (base `6358971`, head `2525f30`, 7 files):
- **SQL injection** (`api/src/repositories/patientRepo.ts` L128 to L129): the new subquery uses only the column reference `pg.id` and the literal `'COMPLETED'`. It takes no request input, and all existing filters are still bound parameters.
- **Data exposure** (L149 and types L85): only an ISO instant is added. `LIST_COLUMNS` is unchanged, so no medical history columns are selected, and the PAT-8 key test still pins the exact list keys.
- **Output encoding** (`web/src/pages/PatientsPage.tsx` L80): the value is rendered as React text through `startsAt`, or "-". There is no `dangerouslySetInnerHTML` and no link built from it.
- **Errors, validation and logging:** none of these change. The invalid-filter 400 is still tested, and no log statement is added.
- **Dependencies:** no package or lock file is in the diff.

## Findings
- SF-1 (minor, `sample-system:api/src/repositories/patientRepo.ts#L129`): the last visit date is exposed to every caller under the MVP's grant-all access. Fix direction: accept for the MVP, or gate the list behind sign-in once `identify` and `requirePermission` are replaced (Q-01).

## Known
- SF-1: the architecture states every MVP request is `staff` with all permissions granted. The new field inherits that, the same as the existing Next appointment field.

## Assumed
- None.

## Unknown
- U-1 (non-blocking): whether the MVP exposure of visit dates is accepted (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): Accept the MVP exposure of last visit dates? Default: accept, because it is the same exposure as Next appointment.
