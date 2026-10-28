---
schema: secure-review.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impl.md
    sha256: 0137364324d03689efa60e79f71467d9f328f737017200b453c89dfae4bf98ec
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Routes declare a permission with `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "Unknown ids answer `404` in the JSON error format."
    cites: ["context/conventions.md#errors"]
  - id: C5
    text: "The change adds `visitSummary` to AppointmentRepo and `visit_summary` to the patient response."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/impl.md#summary"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do you accept, as residual risk for HMS-5, that the new counts are readable by any caller until sign-in replaces `identify` and `requirePermission`?"
    why: In the MVP every permission is granted (C2), so the existing route permission does not restrict who sees the counts. The threat-model review raised the same point (threat F-1).
    evidence: ["context/architecture.md#identity"]
    options: ["Accept until sign-in lands", "Hold the change until sign-in"]
    default: "Accept. The counts are less sensitive than the medical history already on the same page."
scans:
  - kind: sast
    report_sha256: 87081a866a73e1e7c9060a9736f053f3db13dcf155db80c090262839b4c2e494
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The visit counts reach callers through the existing GET /api/patients/:id permission, which grants everything in the MVP (C2). Fix direction: none in this change; keep the counts behind the patient-read permission when sign-in lands."
    cites: ["sample-system:api/src/services/patientService.ts#L46"]
---
# Secure code review: HMS-5

## Summary
The overall risk is low. The change is read-only and adds no route, no input field, no dependency and no
logging (C3, C5). The most important point is SF-1 (minor): the counts inherit the patient-read
permission, which grants everything until sign-in is added (C2). This is residual risk, not a defect in
this diff, and is raised for acceptance in Q-01.

Scans, attached unchanged:
- **SAST:** 4 hits repo-wide, none in the 6 changed files. Two are in CI workflow files, one in
  `api/src/lib/codes.ts` and one in `api/src/middleware/errorHandler.ts`, all existing code outside this change.
- **Secrets:** 0 results.

Manual review of the diff:
- **Input validation:** the count query takes only the patient id. The route has already parsed that id,
  and the query binds it as `$1`. The status values `COMPLETED` and `NO_SHOW` are SQL literals, not taken from input.
- **Authorisation:** no new route. The counts ride on `GET /api/patients/:id`, which keeps its
  `requirePermission` (C1).
- **Output encoding:** the page renders the counts as React text nodes, which React escapes.
- **Error leakage:** an unknown patient still returns 404 before the count runs (C4).
- **Personal data in logs:** none added (C3).
- **Dependencies:** no change.

## Findings
- SF-1 (minor) — the counts are exposed under the MVP grant-all permission. Cite: sample-system:api/src/services/patientService.ts#L46. No code fix needed now. Revisit when sign-in lands.

## Known
- REQ-1, REQ-7, REQ-8: the page edits add only escaped numeric text. The medical history and list code are untouched, so no new disclosure.
- REQ-4, REQ-5, REQ-6: the status filter uses constants in a parameterised query, and zero defaults come from `count(*) FILTER`. There is no injection surface.

## Assumed
- REQ-2, REQ-3: the review assumes the `visit_summary` field shape as built (C5). A rename (change Q-01) does not change the security picture.
- REQ-9: the all-time count means one aggregate per patient read. This is a performance point (threat-model Q-01), not a security one.

## Unknown
No item is unknown. Both scans are attached, and no instruction-like text or credentials were found in the inputs.

## Open questions
- Q-01 (non-blocking, tech lead): accept that the counts are readable by any caller until sign-in lands? Default: accept.
