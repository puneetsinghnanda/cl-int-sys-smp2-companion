---
schema: impact.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/req.md
    sha256: 7163b98904b9c026b5fe2fca4f8b92bd8e20975a6e468d30003d1f0f96c67a46
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
  - path: context/generated/db-schema.json
    sha256: f047583e5b452649edd65cf48083feaad4c664531202fba78652d06b415749f5
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The patient page is the web route `/patients/:id`, declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/patients/:id"]
  - id: C2
    text: "The page component is `PatientDetailPage` in `web/src/pages/PatientDetailPage.tsx`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C3
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "The API is layered into routes, services and `repositories` that hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "One patient is served by `GET /api/patients/:id`, defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C6
    text: "Appointments are listed by `GET /api/appointments`, defined in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C7
    text: "The `appointment` table has a `appointment.status` column and a `appointment.patient_id` column."
    cites: ["context/generated/db-schema.json#table:appointment"]
  - id: C8
    text: "Appointment status is SCHEDULED, `COMPLETED`, CANCELLED or `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C10
    text: "Appointment SQL lives in `AppointmentRepo` in `api/src/repositories/appointmentRepo.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C11
    text: "An appointment becomes Completed through `/api/appointments/:id/complete`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
  - id: C12
    text: "An appointment becomes No-show through `/api/appointments/:id/no-show`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
  - id: C13
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-9]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the Completed and No-show counts come from the API (as part of `GET /api/patients/:id` or a new endpoint), or be computed in `PatientDetailPage` from the appointments it already loads?"
    why: The facts do not show which call the patient page uses to load its appointments, or whether that list is complete. This decides whether the API changes at all.
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
    options: ["Add counts to the GET /api/patients/:id response", "Add a new read-only endpoint for the counts", "Compute counts in the web page from the loaded appointments"]
    default: "Add the counts to the GET /api/patients/:id response, computed in SQL in the repository layer, so they do not depend on how much of the list the page loads."
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage", "context/generated/api-inventory.json#ui:/patients/:id"]
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment"]
apis:
  - id: http:GET:/api/patients/:id
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: http:GET:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: http:POST:/api/appointments/:id/complete
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]
  - id: http:POST:/api/appointments/:id/no-show
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/data-model.md#patients"]
---
# Impact analysis: HMS-5

## Summary
HMS-5 adds a read-only Visit summary to the patient page (`/patients/:id`, `PatientDetailPage`).
It shows counts of the patient's COMPLETED and NO_SHOW appointments. The web module changes in every
option. The API module and `GET /api/patients/:id` are assumed to change to return the two counts,
counted in SQL from `appointment.status` by `patient_id`. No table, column or migration changes,
because the existing status column holds everything needed. The main risks are counting from a partial
appointments list if the counts are computed in the browser (Q-01), and accidentally changing the
existing patient response or appointments list, which REQ-7 and REQ-8 forbid. There are no background
jobs to affect.

## Impacted modules
- module:sample-system:web — changed: `PatientDetailPage` renders the Visit summary above the appointments list (REQ-1 to REQ-8).
- module:sample-system:api — changed (assumed): the patient read path returns the Completed and No-show counts (REQ-2 to REQ-6, REQ-9).
- module:sample-system:db — existing: no migration; the counts read existing columns.
- http:GET:/api/patients/:id — changed (assumed): the response gains the two counts. Existing fields stay as they are (REQ-8).
- http:GET:/api/appointments — existing: may feed the page's appointments list, which must stay unchanged (REQ-7).
- http:POST:/api/appointments/:id/complete — existing: an indirect effect, because closing an appointment as Completed raises the Completed count.
- http:POST:/api/appointments/:id/no-show — existing: an indirect effect, because closing an appointment as No-show raises the No-show count.
- table:appointment — existing: read by `patient_id` and `status`. Scheduled and Cancelled rows are excluded (REQ-4, REQ-5).
- table:patient — existing: read as today. Medical history still leaves the database only for this page (REQ-8).

## Known
- REQ-1: the Visit summary goes on `PatientDetailPage`, route `/patients/:id` (C1, C2), so module:sample-system:web changes.
- REQ-7: the appointments list on that page stays as it is. http:GET:/api/appointments is existing (C6).
- REQ-8: medical history and the rest of the page stay as they are. table:patient is read as today (C9).

## Assumed
- REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-9: the counts are assumed to be computed in SQL over table:appointment (`status`, `patient_id`; C7, C8). They would sit in the repository layer of module:sample-system:api (C4, C10) and be returned by http:GET:/api/patients/:id (C5). Basis: the web app gets all data through the API client (C3), and counting on the server covers the whole history (REQ-9) whatever the list loads. module:sample-system:db and table:appointment stay existing. The writes http:POST:/api/appointments/:id/complete and http:POST:/api/appointments/:id/no-show (C11, C12) change the counts indirectly. Confirm via Q-01.

## Unknown
No item is unknown. The facts do not show which call `PatientDetailPage` uses to load appointments. That gap is carried as non-blocking question Q-01, and the assumed API change stands until it is answered.

## Open questions
- Q-01 (non-blocking, tech lead): return the counts from `GET /api/patients/:id`, from a new endpoint, or compute them in the page? Default: add them to the `GET /api/patients/:id` response, computed in SQL.
