---
schema: secure-review.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/impl.md, sha256: b8571be7328f92bab36ac750d0239e386ae1fead0d6f3d638ed879b87ad36cc1}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The patient detail route is unchanged and still declares `requirePermission('patients:read')`.", cites: ["sample-system:api/src/routes/patients.ts#L28"]}
  - {id: C2, text: "The route parses the id with `parseId` before it reaches the service.", cites: ["sample-system:api/src/routes/patients.ts#L30"]}
  - {id: C3, text: "Every request passes through `identify`; routes call requirePermission.", cites: ["context/architecture.md#identity"]}
  - {id: C4, text: "The next-appointment query binds the patient id and now as parameters (`a.starts_at > $2`).", cites: ["sample-system:api/src/repositories/patientRepo.ts#L252"]}
  - {id: C5, text: "The counts query binds the patient id as a parameter (`patient_id = $1`).", cites: ["sample-system:api/src/repositories/patientRepo.ts#L259"]}
  - {id: C6, text: "The helper interpolates its `where` fragment into the SQL text; both callers pass constant strings.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L244"]}
  - {id: C7, text: "The visits object selects only appointment id, start and the doctor's `full_name`, no patient contact or medical columns.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L241"]}
  - {id: C8, text: "Logs carry method, route and error message only, never request bodies or `contact details`.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C9, text: "VisitsSummary receives only the visits and the zone, never the patient (`PAT-8`).", cites: ["sample-system:web/src/components/VisitsSummary.tsx#L8"]}
trace: [REQ-15, REQ-16, REQ-17]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - {kind: sast, report_sha256: 4e2da3a151215face8f60e91f1d942f913c7a247fe6f707c21e67c169484c67b}
  - {kind: secrets, report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3}
findings:
  - {id: SF-1, severity: minor, text: "The local helper `one()` builds SQL by interpolating its where and order arguments. Today both callers pass constants, so nothing is injectable, but a future caller could pass request data. Narrow the helper to a status/direction enum, or inline the two queries, so only bound parameters vary.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L244"]}
---
# Secure code review: HMS-26

## Summary
The overall risk is low. The change adds read-only queries behind an existing route. That route still parses its id (C2) and declares its permission (C1, C3). Every variable value is a bound parameter (C4, C5). The new response field holds only appointment ids, start instants and doctor names, with no patient contact details or medical history (C7). The web component never receives the patient object (C9). No logging was added (C8), and no dependency changed.

I attached both verifier scans (SAST and secrets) unchanged by hash; this artifact does not restate their contents.

The one finding is minor hardening. SF-1: the private helper interpolates constant SQL fragments, and narrowing it would stop a future caller from passing request data into SQL text.

## Findings
- SF-1 (minor), `sample-system:api/src/repositories/patientRepo.ts#L244`: the `one()` helper interpolates `where`/`order` strings into the SQL. Both callers pass constants today (C6). Restrict the inputs to an enum, or inline the two queries.

## Known
- SF-1: read from the changed lines. The callers at lines 252 and 253 pass only literal strings, and the values are bound as `$1`/`$2`.

## Assumed
None.

## Unknown
None.

## Open questions
None.
