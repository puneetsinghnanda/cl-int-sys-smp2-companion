---
schema: secure-review.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: secrev
attempt: 1
skill: secure-code-review@1.1.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md
    sha256: 4a00b08471ecde2b4d9db5168ee234536a1669de0d906f565501f522bd4ec217
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The new route declares the `appointments:read` permission, as decided at design."
    cites: ["sample-system:api/src/routes/specializations.ts#L19", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-04"]
  - id: C2
    text: "Every request passes through `identify`, and in the MVP every permission is granted, so the route check takes effect only once sign-in lands."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The path id is read with `parseId`, before any query runs."
    cites: ["sample-system:api/src/routes/specializations.ts#L21"]
  - id: C4
    text: "`parseId` accepts only a positive integer of up to ten digits and otherwise throws a 400."
    cites: ["sample-system:api/src/lib/codes.ts#L21-L22"]
  - id: C5
    text: "The new query binds the specialization id as a parameter: `d.specialization_id = $1`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L175"]
  - id: C6
    text: "The new query selects only id, name, department name and slot length (`dp.name AS department_name`), so doctor phone and email are never loaded."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L172"]
  - id: C7
    text: "Each entry is built field by field, ending with `departmentName: doctor.departmentName` and the slot, so no other column reaches the response."
    cites: ["sample-system:api/src/services/doctorService.ts#L196"]
  - id: C8
    text: "An Inactive specialization gives the fixed message `specialization is Inactive`, which leaks no internal detail."
    cites: ["sample-system:api/src/services/doctorService.ts#L175"]
  - id: C9
    text: "`refStatus` interpolates the table name, but only from the literal union `'department' | 'specialization'`; the id stays a parameter."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L182-L183"]
  - id: C10
    text: "Logs carry method, route and error message only, `never request bodies`, contact details or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C11
    text: "For each Active doctor the service runs its own availability query and its own `occupyingBetween` query."
    cites: ["sample-system:api/src/services/doctorService.ts#L184-L187"]
  - id: C12
    text: "The verifier's `sast` step passed with exit code 0 on the implement change."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.checks.json#L127-L130"]
  - id: C13
    text: "The verifier's `secrets` step passed with exit code 0 on the implement change."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.checks.json#L133-L137"]
  - id: C14
    text: "The change touches four API files and no package manifest, ending with `api/src/types.ts`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md#L60-L63"]
trace: [REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-4, S-5]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: 74399446950f21c4dec97223c5ddf04782ef815df4d06488fcafa927ca3b06d7
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "The unpaged endpoint runs two queries per Active doctor, so a large specialization makes each request cost grow with its doctor count; batch the availability and appointment reads into one query each."
    cites: ["sample-system:api/src/services/doctorService.ts#L184"]
---
# Secure code review: HMS-11

## Summary
The overall risk is low. The change adds one read-only route, `GET /api/specializations/:id/earliest-slots`, plus the service method, repository query and type behind it. No dependency changes (C14).

- **Authorisation:** the route declares `appointments:read` (C1). In the MVP every permission is still granted, so the check takes effect only once sign-in lands (C2). That is existing system behaviour and not a weakness this change adds.
- **Input validation:** the path id goes through `parseId`, which accepts only a positive integer and otherwise answers 400 before any query runs (C3, C4).
- **Injection:** the new query binds the id as `$1` (C5). The existing `refStatus` interpolates only a fixed table name (C9).
- **Data exposure:** the query never loads phone or email, and the entry is built field by field (C6, C7).
- **Error leakage:** the errors use fixed messages (C8).
- **Logging:** the change adds no logging, so the logging rule is unaffected (C10).
- **Scans:** the verifier's SAST and secrets steps both passed (C12, C13). Their reports are attached by hash below.

The most important finding is SF-1 (minor). The endpoint is unpaged and runs two queries per Active doctor (C11), so its cost grows with the size of the specialization.

## Findings
- SF-1 (minor): the unpaged endpoint runs two queries per Active doctor, so a large specialization makes each request cost grow with its doctor count. Fix direction: batch the availability and appointment reads into one query each. Cite: sample-system:api/src/services/doctorService.ts#L184.

## Known
- S-4: the unknown-id and Inactive errors are standard 404 and 400 responses with fixed messages, and the malformed id is rejected by `parseId` (C3, C4, C8).
- S-5: no doctor contact data and no patient data can reach an entry, because the query does not select them and the entry is mapped explicitly (C6, C7).

## Assumed
None.

## Unknown
None.

## Open questions
None.
