---
schema: impact.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: impact
attempt: 2
skill: impact-analysis@1.3.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md
    sha256: 9020c22eea6b4e433f7a024deb321a42e4042b10165a1d67058dd3d9e7f1fc2c
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
  - path: context/generated/db-schema.json
    sha256: d0487757e73c20b1eaced48f38b101f87348f97927b76d4b7bd46fc6f5f985ff
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
resolves:
  - id: F-1
    how: "C24 and C25 record that doctor rows carry phone and email, and REQ-14 now maps to them."
claims:
  - id: C1
    text: "The new route is not in the facts; it is the endpoint REQ-1 asks for, `earliest-slots` under the specializations path."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-1"]
  - id: C2
    text: "The specializations routes are served from `api/src/routes/specializations.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/specializations"]
  - id: C3
    text: "That file defines the router `specializationsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/specializations.ts:specializationsRouter"]
  - id: C4
    text: "Its existing list, create, rename and delete handlers come from the shared `referenceHandlers`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/reference.ts:referenceHandlers"]
  - id: C5
    text: "The day schedule of one doctor and date is served from `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C6
    text: "The day schedule and its slot states are built in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C7
    text: "A doctor's slots for one hospital-local day come from `daySlots`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: C8
    text: "Hospital-local day bounds come from `dayBounds`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C9
    text: "Calendar days are stepped with `addDays`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays"]
  - id: C10
    text: "Doctor rows, filters and availability are read through `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C11
    text: "Appointments that occupy slots are read through `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C12
    text: "Appointment summaries are built from `SUMMARY_SELECT`, which joins the patient row, so reusing them carries patient data the new entries must not expose (REQ-15)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
  - id: C13
    text: "Unknown ids answer through `notFound`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/errors.ts:notFound"]
  - id: C14
    text: "Bad input answers through `badRequest`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/errors.ts:badRequest"]
  - id: C15
    text: "Every route declares its permission with `requirePermission`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/auth/identity.ts:requirePermission"]
  - id: C16
    text: "Doctor codes are produced by `formatCode`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/codes.ts:formatCode"]
  - id: C17
    text: "Slots and today use the hospital zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C18
    text: "Double-booking is guarded in the database by `exclusion constraints`; a read-only endpoint does not touch them."
    cites: ["context/architecture.md#double-booking"]
  - id: C19
    text: "The web app reaches the API only through `web/src/api/client.ts`; no web change is in scope (REQ-16)."
    cites: ["context/architecture.md#components"]
  - id: C20
    text: "Doctors link to their specialization through `specialization_id`, so no new column is needed."
    cites: ["context/generated/db-schema.json#column:doctor.specialization_id"]
  - id: C21
    text: "Weekly availability is stored as `availability_block` rows."
    cites: ["context/data-model.md#doctors"]
  - id: C22
    text: "Appointment status includes `CANCELLED`, which frees the slot."
    cites: ["context/data-model.md#appointments"]
  - id: C23
    text: "Design waits for a refreshed context pack: the user chose `Refresh the context pack first` because the pack is behind sample-system main."
    cites: ["runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-02"]
  - id: C24
    text: "Doctor rows carry the contact column `phone`; rows read through DoctorRepo include it, so the new entries must leave it out (REQ-14)."
    cites: ["context/generated/db-schema.json#column:doctor.phone"]
  - id: C25
    text: "Doctor rows carry the contact column `email`; rows read through DoctorRepo include it, so the new entries must leave it out (REQ-14)."
    cites: ["context/generated/db-schema.json#column:doctor.email"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-1"]
  - id: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:web", "artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-16"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#column:doctor.specialization_id"]
apis:
  - id: http:GET:/api/specializations/:id/earliest-slots
    change: new
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-1"]
  - id: http:GET:/api/doctors/:id/slots
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots", "artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-6"]
  - id: http:GET:/api/specializations
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/specializations"]
  - id: http:POST:/api/specializations
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/specializations"]
  - id: http:PATCH:/api/specializations/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/specializations/:id"]
  - id: http:DELETE:/api/specializations/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:DELETE:/api/specializations/:id"]
  - id: http:POST:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments", "artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#item:REQ-16"]
tables:
  - id: table:specialization
    change: existing
    cites: ["context/data-model.md#reference-lists"]
  - id: table:department
    change: existing
    cites: ["context/data-model.md#reference-lists"]
  - id: table:doctor
    change: existing
    cites: ["context/data-model.md#doctors", "context/generated/db-schema.json#column:doctor.phone", "context/generated/db-schema.json#column:doctor.email"]
  - id: table:availability_block
    change: existing
    cites: ["context/data-model.md#doctors"]
  - id: table:appointment
    change: existing
    cites: ["context/data-model.md#appointments"]
  - id: table:patient
    change: existing
    cites: ["context/data-model.md#patients", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
answers:
  - id: A-02
    kind: question
    about: Q-01
    answer: "Refresh the context pack first"
    decides: [C23]
---
# Impact analysis: HMS-11

## Summary
HMS-11 adds one read-only route, `GET /api/specializations/:id/earliest-slots`, to the `api` module. It joins the existing specializations router (C2, C3) and reuses the day-schedule slot rule built in `DoctorService` (C6, C7), so the new answer and the day schedule cannot disagree. Nothing is written: every table is only read, no migration is needed (C20), and the web and db modules are untouched. The main risks are (1) re-deriving the slot rule instead of sharing it; (2) leaking personal data: doctor rows carry phone and email (C24, C25), and appointment summaries join the patient row (C12); and (3) regressing the other specializations routes on the same router. Design waits for a refreshed context pack (C23, A-02).

## Impacted modules
- module:sample-system:api: changed (new route, service logic and reads).
- module:sample-system:web: existing (no page changes in this story).
- module:sample-system:db: existing (no migration).
- http:GET:/api/specializations/:id/earliest-slots: new.
- http:GET:/api/doctors/:id/slots: existing (its FREE rule is reused; its response must not change).
- http:GET:/api/specializations: existing (same router).
- http:POST:/api/specializations: existing (same router).
- http:PATCH:/api/specializations/:id: existing (same router).
- http:DELETE:/api/specializations/:id: existing (same router).
- http:POST:/api/appointments: existing (booking rules unchanged).
- table:specialization: existing (read for existence and Inactive status).
- table:department: existing (read for the department name).
- table:doctor: existing (read by specialization and status; its phone and email columns must not reach the response).
- table:availability_block: existing (read for the slot grid).
- table:appointment: existing (read for BOOKED slots; Cancelled ones free the slot, C22).
- table:patient: existing (joined by `SUMMARY_SELECT` if summaries are reused; never exposed).

## Known
- REQ-1 to REQ-17: each maps to the api module items above, using the cited facts.
- REQ-6 ties to http:GET:/api/doctors/:id/slots and C6, C7.
- REQ-12 and REQ-13 tie to C13 and C14.
- REQ-14 (no contact details) ties to C24 and C25: doctor phone and email come with DoctorRepo rows and must be left out.
- REQ-15 (no medical history) ties to C12 and table:patient.
- REQ-16 ties to the existing routes and the web module.

Resolved review findings:
- F-1: C24 and C25 record that doctor rows carry phone and email, and REQ-14 now maps to them.

## Assumed
None.

## Unknown
None.

## Open questions
None.

Answered in the IDE:
- A-02 (this run): Q-01, should design treat current main as the source of truth, given the pack is 1 commit behind (E-CTX-10, the HMS-9 merge touching `doctorService.ts`)? "Refresh the context pack first". Decides C23: design starts only after the context pack is rebuilt against main.
