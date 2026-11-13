---
schema: threat-model.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md
    sha256: 385aa8939befa99e8d415e886be5a4faac02d8447880d56dc1a4477012f582fb
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Every API request passes through `identify`, and routes call requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "GET /api/appointments/:id requires `appointments:read`."
    cites: ["sample-system:api/src/routes/appointments.ts#L28"]
  - id: C4
    text: "The appointment write routes require `appointments:write`."
    cites: ["sample-system:api/src/routes/appointments.ts#L36"]
  - id: C5
    text: "The doctor lookup is a parameterised query `WHERE d.id = $1`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L100"]
  - id: C6
    text: "Logs carry `method, route and error message only`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C7
    text: "`Medical history` leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C8
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "A doctor's availability blocks have `no overlaps per weekday`."
    cites: ["context/data-model.md#doctors"]
  - id: C10
    text: "Availability uses the configured hospital zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C11
    text: "An unknown appointment id is answered with `notFound`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L56"]
  - id: C12
    text: "Doctors with appointments are `made Inactive, never deleted`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C13
    text: "The code base uses `TypeScript strict everywhere`."
    cites: ["context/conventions.md#code-layout"]
  - id: C14
    text: "After booking, the booking page uses only the returned `appt.id`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#claim:C27"]
  - id: C15
    text: "The `AppointmentDetailPage notices` render two fixed texts chosen by two flags."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-7"]
  - id: C16
    text: "The shared `SUMMARY_SELECT` stays unchanged in the design."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-5"]
  - id: C17
    text: "The appointment id from the path is parsed with `parseId` before use."
    cites: ["sample-system:api/src/routes/appointments.ts#L30"]
  - id: C18
    text: "Reschedule already reads the appointment's doctor with a non-null assertion: `(await new DoctorRepo(tx).get(current.doctor_id))!`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L95"]
  - id: C19
    text: "The DOC-7 flags on the doctor page come from `DoctorService.get`, which the design leaves existing."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/design.md#item:CO-8"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12, T-13, T-14]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: AppointmentService.get (computes doctorStatus and outsideAvailability)
    stride: information_disclosure
    threat: "The detail response could expose more of the doctor record than the page needs, such as the doctor's phone or email, if the whole DoctorSummary is spread into it."
    mitigation: "New control the implement step must build: copy only the doctor's `status` into `doctorStatus`, and never spread the DoctorSummary into the response. A unit test asserts the response carries no other doctor fields."
  - id: T-2
    component: AppointmentService.get (computes doctorStatus and outsideAvailability)
    stride: denial_of_service
    threat: "Each detail read, and each book, reschedule, cancel, complete and no-show response, now makes extra database reads, which adds load under heavy use."
    mitigation: "Existing controls: the doctor read is a by-id parameterised query (C5), and the availability read returns a doctor's weekly blocks, which cannot overlap per weekday (C9). The availability read is skipped unless the appointment is Scheduled and has not started. No new loop or unbounded query is added."
  - id: T-3
    component: AppointmentService.get (computes doctorStatus and outsideAvailability)
    stride: spoofing
    threat: "A caller could read an appointment's detail, now with the doctor's status, without being a real staff member."
    mitigation: "Existing control: every request passes through identify, and the route requires appointments:read (C1, C3). In the MVP every request is the staff actor (C2). This known gap is not widened by this change and is closed when sign-in replaces identify."
  - id: T-4
    component: AppointmentService.get (computes doctorStatus and outsideAvailability)
    stride: information_disclosure
    threat: "If the appointment's doctor row were missing, an unhandled error could surface internal details to the caller or the logs."
    mitigation: "Existing controls: doctors with appointments are never deleted (C12), so the lookup is handled like reschedule's non-null read (C18). Errors are logged with method, route and message only (C6), and an unknown appointment id still returns notFound (C11)."
  - id: T-5
    component: AppointmentDetail type (API)
    stride: tampering
    threat: "The API and web copies of AppointmentDetail could drift, so the page reads a missing or misnamed field and shows or hides a notice wrongly."
    mitigation: "Existing control: TypeScript strict in both workspaces (C13). New control the implement step must build: an API test asserting doctorStatus and outsideAvailability on GET /api/appointments/:id, with the web type updated to the same names."
  - id: T-6
    component: DoctorRepo.get and DoctorRepo.availability
    stride: tampering
    threat: "A crafted appointment or doctor id could alter the SQL that reads the doctor or its availability."
    mitigation: "Existing controls: the path id is parsed with parseId (C17), and the doctor lookups are parameterised queries (C5). The doctor id used comes from the stored appointment row, not from the request."
  - id: T-7
    component: withinAvailability (DOC-7 rule)
    stride: tampering
    threat: "A wrong or changed hospital zone would make the flag disagree with the doctor's real local availability."
    mitigation: "Existing control: the zone comes from configuration (C10), as the doctor page uses it, and the rule function is reused unchanged."
  - id: T-8
    component: AppointmentRepo.detail, SUMMARY_SELECT and AppointmentSummary
    stride: information_disclosure
    threat: "Editing the shared summary query to add the new fields would push doctor data into every list, dashboard and patient response, and widen what lists expose."
    mitigation: "Existing controls: the design keeps SUMMARY_SELECT unchanged (C16), and medical history never leaves the database outside the patient's own page (C7)."
  - id: T-9
    component: AppointmentDetail type (web)
    stride: tampering
    threat: "A client could alter the flags it receives to hide a notice or make one appear."
    mitigation: "Existing control: the flags only drive display. Every change to an appointment still goes through the write routes, which require appointments:write (C4) and the server's own Scheduled checks, so altering a flag changes no data."
  - id: T-10
    component: AppointmentDetailPage notices
    stride: tampering
    threat: "Rendering request-controlled text inside the notices could inject markup into the page."
    mitigation: "Design control: the notices render two fixed texts chosen by two flags (C15), and no appointment, patient or doctor text goes into them. The implement step must keep the texts as fixed strings."
  - id: T-11
    component: AppointmentDetailPage notices
    stride: repudiation
    threat: "Staff could act on a notice, by cancelling or rescheduling, with no trace of who did it."
    mitigation: "Existing control: the notices only point to existing actions, and every change is recorded in appointment_event with time and actor (C8)."
  - id: T-12
    component: DoctorService.get DOC-7 flags and DoctorDetailPage
    stride: tampering
    threat: "Changing the shared rule for the appointment page could silently change which appointments the doctor page flags."
    mitigation: "Design control: DoctorService.get stays existing (C19), and withinAvailability is reused without edits (T-7). The implement step must not modify either."
  - id: T-13
    component: BookAppointmentPage
    stride: information_disclosure
    threat: "The booking response now carries the doctor's status and the availability flag, which the booking page could display or keep without need."
    mitigation: "Existing control: the booking page uses only the returned appointment id to navigate (C14). The two fields carry no personal data."
  - id: T-14
    component: AppointmentService.get (computes doctorStatus and outsideAvailability)
    stride: elevation_of_privilege
    threat: "A new field or code path could need, or bypass, a permission it does not check."
    mitigation: "Existing control: no new route or permission is added. The fields ride on routes that already require appointments:read or appointments:write (C3, C4)."
---
# Threat model: HMS-13

## Components
- AppointmentService.get (computes doctorStatus and outsideAvailability): reads the appointment, the doctor's status and weekly availability, and returns two non-personal fields.
- AppointmentDetail type (API): the response contract, with the doctor's status and a boolean flag.
- DoctorRepo.get and DoctorRepo.availability: read doctor rows and availability blocks by id.
- withinAvailability (DOC-7 rule): pure time logic in the hospital zone.
- AppointmentRepo.detail, SUMMARY_SELECT and AppointmentSummary: the shared appointment read, unchanged.
- AppointmentDetail type (web): the client mirror of the contract.
- AppointmentDetailPage notices: two fixed texts shown above the actions.
- DoctorService.get DOC-7 flags and DoctorDetailPage: unchanged readers of the same rule.
- BookAppointmentPage: an unchanged consumer of the booking response.

No new data class is introduced. The new fields hold the doctor's status and a boolean, and neither is personal data.

## Threats
- T-1 (information_disclosure, AppointmentService.get): over-sharing the doctor record. A new control copies only `status`, and a test checks there are no extra fields.
- T-2 (denial_of_service, AppointmentService.get): extra reads per response. They are by-id and bounded (C5, C9), and the availability read is skipped when the rule does not apply.
- T-3 (spoofing, AppointmentService.get): unauthenticated reads. Covered by identify and appointments:read (C1, C3). The MVP staff-only gap (C2) is not widened.
- T-4 (information_disclosure, AppointmentService.get): leaking internals on a missing doctor. Doctors are never deleted (C12), it is handled like reschedule (C18), and logs stay minimal (C6).
- T-5 (tampering, AppointmentDetail type (API)): API/web type drift. TypeScript strict (C13) plus a new API test on both fields.
- T-6 (tampering, DoctorRepo.get and DoctorRepo.availability): SQL alteration via an id. parseId (C17) and parameterised queries (C5).
- T-7 (tampering, withinAvailability): a wrong zone. The zone comes from configuration (C10), and the rule is reused unchanged.
- T-8 (information_disclosure, AppointmentRepo.detail, SUMMARY_SELECT and AppointmentSummary): fields leaking into lists. SUMMARY_SELECT is unchanged (C16), and medical history is never in lists (C7).
- T-9 (tampering, AppointmentDetail type (web)): client-altered flags. Display only; writes are still enforced on the server (C4).
- T-10 (tampering, AppointmentDetailPage notices): markup injection. The notices are fixed texts only (C15).
- T-11 (repudiation, AppointmentDetailPage notices): untraced follow-up actions. appointment_event records every change (C8).
- T-12 (tampering, DoctorService.get DOC-7 flags and DoctorDetailPage): changing the doctor page's flags. Both pieces stay unedited (C19).
- T-13 (information_disclosure, BookAppointmentPage): unneeded fields in the booking response. The page uses only the id (C14), and the fields are not personal data.
- T-14 (elevation_of_privilege, AppointmentService.get): a path missing its permission check. No new route or permission; the existing checks apply (C3, C4).

## Known
- T-1, T-5: real gaps that new controls close, and the implement step must build them.
- T-2, T-3, T-4, T-6, T-7, T-8, T-9, T-11, T-13, T-14: covered by existing, cited controls.
- T-10, T-12: covered by design choices the implement step must keep.

## Assumed
None.

## Unknown
None.

## Open questions
None.
