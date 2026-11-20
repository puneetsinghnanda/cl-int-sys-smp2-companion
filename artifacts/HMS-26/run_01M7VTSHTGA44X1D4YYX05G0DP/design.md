---
schema: design.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: design
attempt: 2
skill: design@1.9.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/split.md, sha256: 0505e8c204b650fbd44f6f8a04dfa8da0e035fbf38d644498edb5957afb7a071}
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md, sha256: 9a1f1abb91af2282ee0e1cec2d11c2c78175ade5af0865cba4f1e1ec883e3ea7}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The API is layered: routes, services hold the rules, `repositories` hold the SQL.", cites: ["context/architecture.md#components"]}
  - {id: C2, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C3, text: "Instants are stored in `UTC`.", cites: ["context/architecture.md#time-and-slots"]}
  - {id: C4, text: "The patient detail route exists in `api/src/routes/patients.ts`.", cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]}
  - {id: C5, text: "Patient SQL lives in `api/src/repositories/patientRepo.ts`.", cites: ["context/data-model.md#patients"]}
  - {id: C6, text: "`PatientService` is declared in api/src/services/patientService.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]}
  - {id: C7, text: "The API response type `PatientDetail` is declared in api/src/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]}
  - {id: C8, text: "The web type `PatientDetail` is declared in web/src/api/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]}
  - {id: C9, text: "`PatientDetailPage` is declared in web/src/pages/PatientDetailPage.tsx.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]}
  - {id: C10, text: "`dateTime` is declared in web/src/lib/format.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]}
  - {id: C11, text: "`useSettings` is declared in web/src/hooks/useSettings.tsx.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]}
  - {id: C12, text: "The appointment page route is `/appointments/:id` in the web app.", cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]}
  - {id: C13, text: "The existing patient appointments list is capped (`limit = 100`), so the summary uses its own queries over all appointments.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-02"]}
  - {id: C14, text: "Last visit uses a new `date` helper in web/src/lib/format.ts.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-03"]}
  - {id: C15, text: "The display zone comes from `useSettings().timeZone`.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01"]}
  - {id: C16, text: "Medical history appears only on the patient's own page, never in lists, the `dashboard` or logs.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C17, text: "The `AppointmentTable` component already renders appointment tables.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]}
  - {id: C18, text: "PatientService passes its injected clock to the repo: `this.clock()`.", cites: ["sample-system:api/src/services/patientService.ts#L42"]}
  - {id: C19, text: "The existing next-appointment SQL compares with the passed instant: `p(now)`.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L129"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8]
assumed: [EP-1]
unknown: []
questions: []
resolves:
  - {id: F-1, how: "The next-appointment query takes the service's injected clock as a now parameter instead of the database now()."}
components:
  - {id: CO-1, name: "patientRepo visit queries", module: "module:sample-system:api", change: changed, cites: ["context/data-model.md#patients", "sample-system:api/src/repositories/patientRepo.ts#L129", "runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-02"]}
  - {id: CO-2, name: "PatientService detail adds visits", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "sample-system:api/src/services/patientService.ts#L42", "runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-02"]}
  - {id: CO-3, name: "API PatientDetail gains visits", module: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]}
  - {id: CO-4, name: "Web PatientDetail gains visits", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]}
  - {id: CO-5, name: "format.ts date helper", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-03"]}
  - {id: CO-6, name: "VisitsSummary component", module: "module:sample-system:web", change: new, cites: ["context/generated/module-map.json#module:sample-system:web"]}
  - {id: CO-7, name: "PatientDetailPage mounts summary", module: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]}
  - {id: CO-8, name: "useSettings timeZone", module: "module:sample-system:web", change: existing, cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings", "runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01"]}
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients/:id
    change: changed
    fields:
      - {name: visits, type: object, nullable: false, format: "PatientVisits object, always present", example: "{next: null, last: null, completed: 0, cancelled: 0, no_show: 0}", status: known}
      - {name: visits.next, type: object, nullable: true, format: "earliest SCHEDULED appointment with starts_at > now (the service clock); null when none", example: "{id: 42, starts_at: \"2026-03-09T19:00:00.000Z\", doctor_name: \"<doctor name>\"}", status: known}
      - {name: visits.next.id, type: integer, nullable: false, format: "appointment row id, used in the /appointments/:id link", example: "42", status: assumed}
      - {name: visits.next.starts_at, type: string, nullable: false, format: "ISO 8601 instant in UTC (YYYY-MM-DDTHH:mm:ss.sssZ)", example: "2026-03-09T19:00:00.000Z", status: known}
      - {name: visits.next.doctor_name, type: string, nullable: false, format: "doctor's display name", example: "<doctor name>", status: known}
      - {name: visits.last, type: object, nullable: true, format: "latest COMPLETED appointment by starts_at; null when none; same shape as visits.next", example: "{id: 17, starts_at: \"2026-03-09T20:00:00.000Z\", doctor_name: \"<doctor name>\"}", status: known}
      - {name: visits.last.id, type: integer, nullable: false, format: "appointment row id", example: "17", status: assumed}
      - {name: visits.last.starts_at, type: string, nullable: false, format: "ISO 8601 instant in UTC (YYYY-MM-DDTHH:mm:ss.sssZ)", example: "2026-03-09T20:00:00.000Z", status: known}
      - {name: visits.last.doctor_name, type: string, nullable: false, format: "doctor's display name", example: "<doctor name>", status: known}
      - {name: visits.completed, type: integer, nullable: false, format: "count of COMPLETED appointments, any date, 0 or more", example: "2", status: known}
      - {name: visits.cancelled, type: integer, nullable: false, format: "count of CANCELLED appointments, any date, 0 or more", example: "1", status: known}
      - {name: visits.no_show, type: integer, nullable: false, format: "count of NO_SHOW appointments, any date, 0 or more", example: "1", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-02"]
answers:
  - {id: A-02, kind: question, about: Q-01, answer: "visits on patient detail (the proposed default), but compute next, last and the three counts with their own queries over all of the patient's appointments, not from the 100-row appointments list (patientRepo.appointments has limit = 100), so AC4 holds for any patient.", decides: [CO-1, CO-2, EP-1, C13]}
  - {id: A-03, kind: question, about: Q-02, answer: "New `date` helper (proposed default)", decides: [CO-5, C14]}
---
# Design: HMS-26

## Overview
The patient detail response (`GET /api/patients/:id`, EP-1) gains a `visits` object computed on the
server, so the patient page needs no extra request (A-02). Following the layering (C1), patientRepo
(CO-1) runs its own queries over all of the patient's appointments:
- the earliest SCHEDULED appointment with `starts_at > now`, joined to its doctor;
- the latest COMPLETED appointment;
- a count per status for COMPLETED, CANCELLED and NO_SHOW.

These queries do not reuse the capped appointment list (C13). PatientService (CO-2) adds the result
to the detail. It passes its injected clock as `now`, the same way it already calls the repo with
`this.clock()` (C18), and the existing SCHEDULED query compares with `p(now)` (C19). The API and web
`PatientDetail` types (CO-3, CO-4) gain a `PatientVisits` type. In the web app, a new VisitsSummary
component (CO-6) renders the summary, and the patient page (CO-7) mounts it between the name and the
details.

Instants travel as ISO 8601 UTC (C3). The web app formats them in `useSettings().timeZone` (CO-8,
A-01). The next appointment uses the existing `dateTime`. The last visit uses a new `date` helper
(CO-5, A-03) that prints only the local date. The next appointment's date and time link to
`/appointments/:id` (C12). When `no_show` is 2 or more, the No-show count gets a warning style.

Resolves F-1: The next-appointment query takes the service's injected clock as a now parameter instead of the database now().

Trade-offs:
- Server-side visits instead of computing them in the web app: each patient detail read runs one
  more set of queries, but the counts stay correct for any number of appointments (A-02).
- "After now" uses the service's injected clock (C18), passed to SQL as a parameter (C19), not the
  database clock. Tests can fix "now" (S-1 AC1), and the comparison works on UTC instants in any
  hospital zone.
- The warning style is a CSS class on the No-show count inside VisitsSummary, chosen by default.
  It is cheap and local, and it does not reuse a shared badge, so other pages do not change
  (REQ-18).
- VisitsSummary receives only `visits` and the zone, never the patient object, so medical history
  and contact details cannot reach it (REQ-15, REQ-16, C16).

## Components
- CO-1 (changed): patientRepo visit queries in `api/src/repositories/patientRepo.ts`. They find the next Scheduled appointment after the passed `now`, the latest Completed appointment and the counts by status, over all of the patient's appointments.
- CO-2 (changed): the PatientService detail read adds `visits` and passes `this.clock()` as `now`.
- CO-3 (changed): the API type `PatientDetail` gains `visits: PatientVisits`.
- CO-4 (changed): the web type `PatientDetail` gains `visits: PatientVisits`.
- CO-5 (changed): `web/src/lib/format.ts` gains `date(iso, timeZone)`, which prints the local date only.
- CO-6 (new): `web/src/components/VisitsSummary.tsx` shows:
  - the next appointment, or None booked;
  - the last visit, or No visits yet;
  - the three counts, with the warning when the No-show count is 2 or more.
- CO-7 (changed): PatientDetailPage renders VisitsSummary below the name and above the details. The rest of the page is unchanged.
- CO-8 (existing): `useSettings` supplies `timeZone`.
- EP-1 (changed): `GET /api/patients/:id` adds these fields:
  - `visits` (object, not null).
  - `visits.next` (object or null): the earliest SCHEDULED appointment with `starts_at > now` (the service clock). Its fields:
    - `id` (integer, appointment id, assumed; see Assumed);
    - `starts_at` (ISO 8601 UTC string, for example `2026-03-09T19:00:00.000Z`);
    - `doctor_name` (string).
  - `visits.last` (object or null): the latest COMPLETED appointment, with the same fields.
  - `visits.completed`, `visits.cancelled` and `visits.no_show` (integers, 0 or more, any date).

Story mapping:
- S-1: CO-1, CO-2, EP-1, CO-6, CO-8.
- S-2: CO-1, CO-2, EP-1, CO-5, CO-6.
- S-3: CO-1, CO-2, EP-1, CO-6.
- S-4: CO-6, CO-7.
- S-5: CO-7. No other page or component changes.
- S-6: CO-5, CO-6, CO-8.

## Known
- CO-1: the patient SQL file is proven by C5 and C19. The user decided the queries (A-02).
- CO-2, CO-3, CO-4, CO-7, CO-8: module-map facts prove each symbol (C6, C7, C8, C9, C11). The clock usage is proven by C18.
- CO-5: `dateTime` proves the file exists (C10). The user decided the helper (A-03).
- CO-6: a new file in the web module, next to the existing components (C17).

## Assumed
- EP-1: the route exists (C4), and the user decided the field shape (A-02). One part is assumed: `visits.next.id` and `visits.last.id` are the appointment row id as an integer, the same id the existing `/appointments/:id` page uses (C12). The basis is that display codes are derived from row ids and are not stored (context/data-model.md#codes). Implement must confirm this against the existing appointment types.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (eng-dffd), Q-01: how the API delivers the summary. Answer: a `visits` object on the patient detail, computed by its own queries over all appointments, not from the 100-row list. Decides CO-1, CO-2, EP-1 and C13. No mismatch.
- A-03 (eng-dffd), Q-02: how the last-visit date is formatted. Answer: a new `date` helper. Decides CO-5 and C14. No mismatch.
