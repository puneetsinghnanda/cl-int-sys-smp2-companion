---
schema: threat-model.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md
    sha256: 2ea8104748653909b30e76525dd490d16d20f1645fa07ff4f40b533297f55d50
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "Every API request passes through `identify` and routes call requirePermission; in the MVP every request is the actor staff."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Availability, slots and today use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C3
    text: "Medical history leaves the database only for the patient's own page; a `patient` row holds identity, contact details and medical history."
    cites: ["context/data-model.md#patients"]
  - id: C4
    text: "The web app reaches the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "The new field `nextSevenDays` holds per-day date, available, booked and free counts only."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C12"]
  - id: C6
    text: "For an Inactive doctor the field is `null`, so no computation runs."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C13"]
  - id: C7
    text: "HMS-7 set the precedent that `the read-only count in a PATCH body is ignored`; nextSevenDays gets the same treatment."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/design.md#claim:C15"]
  - id: C8
    text: "The existing day-schedule route `/api/doctors/:id/slots` is unchanged and still validates its date."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C9
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: DoctorService.get (doctor detail) adds nextSevenDays
    stride: information_disclosure
    threat: "The new summary could leak patient data (names, contact details or medical history) if appointment rows are copied into it rather than counted."
    mitigation: "New control for implement: nextSevenDays carries only date, available, booked and free (C5). No appointment or patient fields are added. A unit test asserts the entry keys. Medical history stays on the patient's own page (C3)."
  - id: T-2
    component: DoctorService.get (doctor detail) adds nextSevenDays
    stride: denial_of_service
    threat: "Computing the summary on every detail read could add heavy database load if it ran one query per day or per slot."
    mitigation: "Design control: a fixed 7-day range, one appointment query for the whole range, and no computation for Inactive doctors (C6). Implement must keep the query count fixed and must not loop queries per day."
  - id: T-3
    component: DoctorService.get (doctor detail) adds nextSevenDays
    stride: spoofing
    threat: "A caller without staff rights could read doctors' schedule load."
    mitigation: "Existing control: the route passes through identify and requirePermission like every API route (C1). This is unchanged; in the MVP every request is staff. The sign-in work will replace these two functions and covers this route automatically."
  - id: T-4
    component: Shared slot-state rule in DoctorService, used by both schedule() and the new summary
    stride: tampering
    threat: "Refactoring the slot-state rule could change the day schedule's BOOKED/PAST/FREE result, showing a booked slot as free and inviting a double booking attempt."
    mitigation: "Existing control: the database's exclusion constraints still reject overlapping Scheduled appointments whatever the UI shows. New control for implement: keep the existing day-schedule tests green and add a test that the summary counts equal the schedule's states for the same date."
  - id: T-5
    component: AppointmentRepo (reads non-cancelled appointments over the 7-day range)
    stride: information_disclosure
    threat: "Reading appointments over a week may pull patient summary columns into memory and then into logs on error."
    mitigation: "Existing control: logs carry method, route and error message only, never request bodies or contact details. Implement must not log appointment rows. The repo method is reused unchanged."
  - id: T-6
    component: api DoctorDetail type gains nextSevenDays; NextDay declared beside it in api/src/types.ts
    stride: tampering
    threat: "A client could send nextSevenDays in a POST or PATCH body to try to store or override the computed values."
    mitigation: "Existing control: the doctor body reader takes only editable fields, so the HMS-7 read-only count is ignored (C7). New control for implement: a test that nextSevenDays in a PATCH body is ignored and not echoed."
  - id: T-7
    component: lib/slots and lib/time helpers (daySlots, localDay, addDays, dayBounds)
    stride: tampering
    threat: "Using UTC instead of hospital time would shift 'today' near midnight, giving wrong counts that staff act on."
    mitigation: "Existing control: the helpers take HOSPITAL_TZ from config (C2). Implement must pass the configured zone, and tests cover a UTC-vs-local midnight instant (S-1 AC1)."
  - id: T-8
    component: web DoctorDetail type gains nextSevenDays; NextDay declared beside it in web/src/api/types.ts
    stride: tampering
    threat: "A type mismatch between the api and web copies could make the page render undefined or wrong counts without an error."
    mitigation: "New control for implement: update both declarations together and make DoctorDetailPage tests use a fixture typed as the web DoctorDetail. The web app reads only through web/src/api/client.ts (C4)."
  - id: T-9
    component: DoctorDetailPage renders the Next 7 days section and reads ?date= for the day schedule
    stride: tampering
    threat: "A crafted ?date= value could inject markup or reach the slots request with an unexpected value."
    mitigation: "New control for implement: accept ?date= only when it matches YYYY-MM-DD and is a real date, else fall back to todayIn. Render it as text (React escapes it). The slots route keeps its own date validation (C8)."
  - id: T-10
    component: DoctorDetailPage renders the Next 7 days section and reads ?date= for the day schedule
    stride: repudiation
    threat: "Staff may later dispute which counts they saw when offering a day to a patient."
    mitigation: "Accepted. The section is read-only and records nothing. Bookings themselves are recorded with time and actor in appointment_event (C9), which is the audit trail that matters."
  - id: T-11
    component: DoctorService.get (doctor detail) adds nextSevenDays
    stride: elevation_of_privilege
    threat: "The write routes (POST, PATCH, availability PUT) now return nextSevenDays and could expose more than the caller is allowed."
    mitigation: "Existing control: the same requirePermission checks guard those routes (C1). The field holds counts only (C5), which the caller can already read through GET."
---
# Threat model: HMS-9

## Components
- **DoctorService.get (doctor detail) adds nextSevenDays.** Reads the doctor, availability and non-cancelled appointments, and returns the date, available, booked and free values for each day.
- **Shared slot-state rule in DoctorService, used by both schedule() and the new summary.** Classifies slots as BOOKED, PAST or FREE.
- **AppointmentRepo (reads non-cancelled appointments over the 7-day range).** Reads appointment rows, including patient summary columns.
- **api DoctorDetail type gains nextSevenDays; NextDay declared beside it in api/src/types.ts.** The response contract.
- **lib/slots and lib/time helpers (daySlots, localDay, addDays, dayBounds).** Work out the hospital-time days and slots.
- **web DoctorDetail type gains nextSevenDays; NextDay declared beside it in web/src/api/types.ts.** The client contract.
- **DoctorDetailPage renders the Next 7 days section and reads ?date= for the day schedule.** Renders the counts and reads the URL's date parameter.

## Threats
- **T-1, information disclosure** (DoctorService.get). The summary could leak patient data. Mitigation (new): the summary holds counts only, and a test asserts the keys.
- **T-2, denial of service** (DoctorService.get). The summary could add heavy database load. Mitigation (design): a fixed 7-day range, one query, and no work for Inactive doctors.
- **T-3, spoofing** (DoctorService.get). A caller without staff rights could read the data. Mitigation (existing): identify and requirePermission, unchanged.
- **T-4, tampering** (shared slot-state rule). The refactor could change the day schedule's states. Mitigation: the database's exclusion constraints still apply, the existing tests stay green, and a new test checks that the summary equals the schedule.
- **T-5, information disclosure** (AppointmentRepo). Appointment rows could reach the logs. Mitigation (existing): the logging rules; implement must not log the rows.
- **T-6, tampering** (api DoctorDetail type). A client could send nextSevenDays in a write body. Mitigation: the existing editable-field reader, plus a new PATCH test.
- **T-7, tampering** (lib/slots and lib/time helpers). Using UTC would shift "today". Mitigation (existing): HOSPITAL_TZ, plus a midnight test.
- **T-8, tampering** (web DoctorDetail type). The api and web types could drift. Mitigation (new): update both together and use a typed fixture.
- **T-9, tampering** (DoctorDetailPage). A crafted `?date=` value. Mitigation (new): strict YYYY-MM-DD validation with a fallback to today, text rendering, and the route's own validation.
- **T-10, repudiation** (DoctorDetailPage). Staff could dispute which counts they saw. Accepted: the section is read-only, and bookings are audited in appointment_event.
- **T-11, elevation of privilege** (DoctorService.get). The write responses carry the field. Mitigation (existing): the same permission checks apply, and the field holds counts only.

## Known
- REQ-1 to REQ-10: every component the design names is modelled. The controls rest on C1 to C9.

## Assumed
None.

## Unknown
None.

## Open questions
None.
