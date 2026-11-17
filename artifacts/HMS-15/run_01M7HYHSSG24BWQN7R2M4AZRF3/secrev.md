---
schema: secure-review.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: secrev
attempt: 1
skill: secure-code-review@1.2.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/impl.md
    sha256: 8d0cdda0228533bf20acfae2614ed1e7051e24f67db59538f0a060c00e87d78b
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Every API request passes `identify` and routes call requirePermission; the change adds no route."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "The patient detail route already guards reads with `requirePermission`."
    cites: ["sample-system:api/src/routes/patients.ts#L28"]
  - id: C3
    text: "Logs carry method, route and error message only, never `request bodies` or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "The new one-patient query binds the patient id and now as parameters `$1` and `$2`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L157"]
  - id: C5
    text: "The list passes now through its parameter helper `p(now)`, so no value is spliced into SQL."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L138"]
  - id: C6
    text: "The shared fragment is built by the private `nextAppointmentSql` with string interpolation of its two arguments."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L83"]
  - id: C7
    text: "The new field is the existing `NextAppointment` shape (id, start, doctor name), with no medical history."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/impl.md#claim:C4"]
trace: [REQ-4, REQ-11]
known: [SF-1]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 62daed9e618ef5c0ae9aa300ed286f31dc53b3b858676a5b225286ef81f13196
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "nextAppointmentSql interpolates its two string arguments into SQL; today every caller passes a literal or a $n placeholder, but a future caller passing a value would open SQL injection — keep it module-private and accept only placeholder strings (or type them as such)."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L87"]
---
# Secure code review: HMS-15

## Summary
The overall risk is low. The change adds no route, no dependency, no logging and no new input parsing. The detail, create and update endpoints keep their existing `requirePermission` guards (C1, C2).

Data passes into SQL only as bound parameters (C4, C5). The new response field carries only an appointment id, start time and doctor name, and no medical history (C7, C3). On the web, React renders the doctor name as escaped text inside a router `Link` to `/appointments/{id}`.

The only finding is a minor hardening note: the shared SQL fragment is assembled by string interpolation (SF-1). The SAST and secrets scans from the verifier are attached by hash.

## Findings
- SF-1 (minor), `sample-system:api/src/repositories/patientRepo.ts#L87`: `nextAppointmentSql` interpolates its arguments into SQL. All current callers pass the code literal `pg.id`, `$1`/`$2`, or the placeholder from `p(now)`, so nothing is exploitable today. Fix direction: keep the function private to the module, and restrict its arguments to placeholder or column-reference strings so that no caller can pass a value.

## Known
- SF-1: read from the changed code at head (C4, C5, C6).

## Assumed
None.

## Unknown
None.

## Open questions
None.
