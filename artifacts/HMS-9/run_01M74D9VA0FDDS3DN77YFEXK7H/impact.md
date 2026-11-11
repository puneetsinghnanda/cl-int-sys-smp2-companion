---
schema: impact.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: impact
attempt: 1
skill: impact-analysis@1.3.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md
    sha256: d91af6d2d33b27873ad261064f8454e09c4904922e83e8ea0b13ffb56bc031e8
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
claims:
  - id: C1
    text: "The doctor detail route is declared in `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C2
    text: "The one-day slots route used by the day schedule is declared in `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C3
    text: "The doctor page UI route is registered in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/doctors/:id"]
  - id: C4
    text: "The doctor page is rendered by `DoctorDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C5
    text: "Doctor rules, including the day schedule, live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C6
    text: "One local day's slots are produced by `daySlots`, which the 7-day counts reuse so they match the day schedule."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/slots.ts:daySlots"]
  - id: C7
    text: "Availability, slots and today use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C8
    text: "Weekly availability is stored in `availability_block` rows in local wall-clock time."
    cites: ["context/data-model.md#doctors"]
  - id: C9
    text: "Appointment status includes `CANCELLED`, which does not take a slot."
    cites: ["context/data-model.md#appointments"]
  - id: C10
    text: "The counts are added to the doctor detail response, following the HMS-7 `noShowsLast30Days` precedent."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-05"]
  - id: C11
    text: "`DoctorFormPage` and BookAppointmentPage also call the doctor detail route, so they receive the added field and ignore it."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-05"]
  - id: C12
    text: "The web app reaches the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C13
    text: "The doctor page's current local date comes from `todayIn`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-05"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/api-inventory.json#ui:/doctors/:id"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db"]
apis:
  - id: http:GET:/api/doctors/:id
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-05"]
  - id: http:GET:/api/doctors/:id/slots
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
tables:
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor"]
  - id: table:availability_block
    change: existing
    cites: ["context/generated/db-schema.json#table:availability_block"]
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment"]
answers:
  - id: A-05
    kind: question
    about: Q-01
    answer: "Extend doctor detail (proposed default)"
    decides: [http:GET:/api/doctors/:id, module:sample-system:api, C10, C11]
---
# Impact analysis: HMS-9

## Summary
HMS-9 changes the doctor page in the web module and the doctor detail API in the api module. No table or migration changes. As decided in A-05, `GET /api/doctors/:id` gains a next-7-days summary that the server computes. This follows the pattern HMS-7 used for the No-shows count. The summary reuses the same slot logic as the day schedule (`daySlots` and the schedule rule in `DoctorService`), so the counts match it (REQ-4). The web doctor page adds the "Next 7 days" section and reads `?date=` to set the day schedule's date (REQ-7, REQ-8).

The main risks:
- **Drift between the summary and the day schedule.** Two code paths could disagree about past or cancelled slots; sharing one helper avoids that.
- **Hospital-time "today".** The summary must use HOSPITAL_TZ, not UTC.
- **Other callers.** DoctorFormPage and BookAppointmentPage also call the doctor detail route and will receive the extra field.

## Impacted modules
- module:sample-system:api, **changed**. DoctorService builds the next-7-days summary, and the doctor detail type gains the field.
- module:sample-system:web, **changed**. DoctorDetailPage gets the new section, the `?date=` handling and the client type.
- module:sample-system:db, **existing**. No migration.
- http:GET:/api/doctors/:id, **changed**. The response gains the next-7-days summary (A-05). It is skipped or empty for Inactive doctors, per REQ-9; the exact shape is decided in design.
- http:GET:/api/doctors/:id/slots, **existing**. It is unchanged and still serves the day schedule, including when the date comes from `?date=`.
- table:doctor, **existing**. Read for the status and slot length.
- table:availability_block, **existing**. Read for the weekly availability.
- table:appointment, **existing**. Read for appointments that take slots (status other than CANCELLED).

Indirect effects:
- C11: DoctorFormPage and BookAppointmentPage call the same doctor detail route and get the extra field. They need no change, but their tests and fixtures may need the new field in the client type.
- C12: the client type in `web/src/api/client.ts` and its DoctorDetail type change with the response.

## Known
- REQ-1 to REQ-3, REQ-5, REQ-6, REQ-9: the new section on DoctorDetailPage (C3, C4). The data comes from the changed doctor detail route (C10).
- REQ-2: the days are taken in HOSPITAL_TZ (C7, C13).
- REQ-4: the counts reuse `daySlots` and the day-schedule rule in DoctorService (C5, C6). Cancelled appointments don't take a slot (C9).
- REQ-7, REQ-8: web-only. DoctorDetailPage reads `?date=`, and the day schedule keeps using the existing slots route (C2).
- REQ-10: the rest of the page and the slots route are unchanged. The added response field is the only contract change.

## Assumed
None.

## Unknown
None.

## Open questions
None.

Answered in the IDE:
- A-05 (eng-3c51), Q-01, where the counts come from: "Extend doctor detail (proposed default)". Decides http:GET:/api/doctors/:id (changed), module:sample-system:api (changed), C10 and C11. No mismatch with the facts.
