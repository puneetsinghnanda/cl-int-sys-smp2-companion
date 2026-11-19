---
schema: secure-review.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: secrev
attempt: 1
skill: secure-code-review@1.3.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/impl.md
    sha256: c9ae830fc32ea02998373070619455888e04b1ce4e45956966e15701d01cc409
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The change adds no route; it extends the existing patient detail route in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C2
    text: "Every route declares a permission with `requirePermission`, and the patient detail route keeps its existing guard."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The new count query binds the patient id and the clock as parameters (`patient_id = $1`), with no string-built SQL."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L246"]
  - id: C4
    text: "The last-visit query selects only id, start and `d.full_name AS doctor_name`: no reason, cancellation reason or patient medical columns."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L252"]
  - id: C5
    text: "Logs must never carry request bodies, `contact details` or medical history; the change adds no logging."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C6
    text: "The web app reaches the API only through `web/src/api/client.ts`; the change adds no new client call."
    cites: ["context/architecture.md#components"]
  - id: C7
    text: "The doctor name is rendered as a React text child (`{lastVisit.doctorName}`), so it is escaped, not injected as HTML."
    cites: ["sample-system:web/src/components/VisitSummary.tsx#L28"]
  - id: C8
    text: "The appointment link is built from the numeric `lastVisit.appointmentId` only."
    cites: ["sample-system:web/src/components/VisitSummary.tsx#L28"]
trace: [REQ-13, REQ-14]
known: [REQ-13, REQ-14]
assumed: []
unknown: []
questions: []
scans:
  - kind: sast
    report_sha256: d2e90c4a830eb3c5acc584a732ae6cdea825628dc81313c1dfd75e3fbba69f85
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings: []
---
# Secure code review: HMS-24

## Summary
Overall risk is low and there are no findings in the changed code. The change adds a read-only
field to an existing, permission-guarded route (C1, C2). It adds no new route, client call (C6),
logging (C5) or dependency. Both new queries are fully parameterised (C3). They read only
appointment status, start, id and the doctor's name, so no reason, cancellation reason or medical
history can leave through the summary (C4). That supports REQ-13. On the web side the doctor's name
is rendered as escaped text (C7). The only link target is built from the numeric appointment id (C8).

Scans are attached by hash, unchanged. The SAST report holds 4 hits, all in files this change does
not touch: two mutable action tags in `.github/workflows/ci.yml`, a non-literal RegExp in
`api/src/lib/codes.ts`, and a format-string warning in `api/src/middleware/errorHandler.ts`. They
are pre-existing and out of scope for HMS-24 (REQ-14 keeps everything else unchanged), so they
are not raised as findings here. The secrets report is attached as given, and no secret value
appears in the changed files.

## Findings
None in the changed code.

## Known
- REQ-13: the new queries and the rendered summary carry no reason, cancellation reason or medical history (C4, C7).
- REQ-14: no route, permission, client call, log or dependency changes. The existing guard and conventions still apply (C1, C2, C5, C6).

## Assumed
None.

## Unknown
None.

## Open questions
None.
