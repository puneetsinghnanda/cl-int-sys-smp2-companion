---
schema: change.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/design.md, sha256: 13464e602d2906d40bbc8e52cd98cf34d9fc95e4e7e761d3a538b683357627e4}
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/split.md, sha256: 0505e8c204b650fbd44f6f8a04dfa8da0e035fbf38d644498edb5957afb7a071}
  - {path: context/conventions.md, sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 4cbf5a9ac0eaba5b15514fd8f7a0c5d1b87f100b
    diff_sha256: 8ff943807476a5f35454731b3675451f86adeb88e03f8222f4cd13538149c8d9
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/components/VisitsSummary.tsx
      - web/src/lib/format.ts
      - web/src/pages/PatientDetailPage.tsx
claims:
  - {id: C1, text: "PatientService already passes its injected clock to the repo: `this.clock()`.", cites: ["sample-system:api/src/services/patientService.ts#L42"]}
  - {id: C2, text: "The existing next-appointment SQL compares with a passed instant: `p(now)`.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L129"]}
  - {id: C3, text: "The existing `NextAppointment` type holds id, starts_at and doctor_name, so visits.next and visits.last reuse it.", cites: ["sample-system:api/src/types.ts#L95"]}
  - {id: C4, text: "The appointment id in `NextAppointment` is a number, which settles the design's assumed id type.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:NextAppointment"]}
  - {id: C5, text: "The patient appointments list is capped by default: `limit = 100`.", cites: ["sample-system:api/src/repositories/patientRepo.ts#L221"]}
  - {id: C6, text: "`dateTime` prints date and time in a given zone; the new date helper sits next to it.", cites: ["sample-system:web/src/lib/format.ts#L21"]}
  - {id: C7, text: "The existing `badge-warning` style marks warnings.", cites: ["sample-system:web/src/styles.css#L81"]}
  - {id: C8, text: "The settings context defaults the zone to `Asia/Kolkata` when settings are missing.", cites: ["sample-system:web/src/hooks/useSettings.tsx#L4"]}
  - {id: C9, text: "The patient page is `PatientDetailPage`; the summary mounts after its page head, before the details list.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]}
  - {id: C10, text: "Logs never carry request bodies or `contact details`.", cites: ["context/conventions.md#logging-and-data"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
summary: "GET /api/patients/:id now returns a visits object (next Scheduled after the service clock, latest Completed, Completed/Cancelled/No-show counts over all appointments), and the patient page shows it in a new VisitsSummary between the name and the details, formatted in useSettings().timeZone."
---
# Change: HMS-26

## Summary
The change implements design.md CO-1 to CO-8 and EP-1.
- API: `PatientRepo.visits(id, now)` (CO-1) runs its own queries over all of the patient's appointments.
  - Next: the earliest SCHEDULED appointment with `starts_at > now`, joined to the doctor.
  - Last: the latest COMPLETED appointment.
  - Counts: one `count(*) FILTER` query for COMPLETED, CANCELLED and NO_SHOW.

  It does not reuse the list capped by `limit = 100` (C5). `PatientService.get` (CO-2) reads `this.clock()` once and passes it to both the appointment list and `visits` (C1, C2). `PatientDetail` gains `visits: PatientVisits` (CO-3). `PatientVisits` reuses the existing `NextAppointment` shape `{id, starts_at, doctor_name}` for next and last (C3, EP-1).
- Web: the web types mirror this (CO-4). A new `date(iso, timeZone)` helper (CO-5) prints the local date only, in the same style as `dateTime` (C6).
  - The new `VisitsSummary` (CO-6) shows the next appointment (or None booked), with its date and time linking to `/appointments/:id`, and the last visit as date · doctor (or No visits yet).
  - It also shows the three counts. When the No-show count is 2 or more, it is wrapped in the existing `badge badge-warning` (C7).
  - It receives only `visits` and the zone, so no medical history or contact details can reach it.
- `PatientDetailPage` (CO-7) reads `timeZone` from `useSettings` (CO-8, C8). It mounts the summary after the page head and before the details list (C9). Nothing else on the page changes.

Defaults taken:
- The warning threshold is a named constant `NO_SHOW_WARNING` in the component. This keeps it next to its only use, but a later change to it needs a code edit.
- The summary does not wrap the zone in the try/fallback that PatientsPage uses. This avoids hard-coding Asia/Kolkata; the settings context already supplies the default.

## Changes
- `api/src/types.ts`: new `PatientVisits` type, and `PatientDetail.visits` (CO-3, EP-1).
- `api/src/repositories/patientRepo.ts`: new `visits(id, now)`. `get` now omits `visits` from its return type (CO-1).
- `api/src/services/patientService.ts`: `get` adds `visits` using the injected clock (CO-2).
- `web/src/api/types.ts`: `PatientVisits` and `PatientDetail.visits` (CO-4).
- `web/src/lib/format.ts`: new `date` helper (CO-5).
- `web/src/components/VisitsSummary.tsx`: new component (CO-6).
- `web/src/pages/PatientDetailPage.tsx`: mounts VisitsSummary with the settings zone (CO-7, CO-8).
- Tests: none added. The design asks for no regression tests; the unit-test step covers them.

## Known
- CO-1: built in patientRepo following the existing SQL style (C2, C5).
- CO-2: uses the injected clock (C1).
- CO-3, CO-4, EP-1: the field shape follows the design, and the id type is settled by `NextAppointment` (C3, C4).
- CO-5: `date` helper next to `dateTime` (C6).
- CO-6: new component using the existing warning style (C7).
- CO-7: mounted between the page head and the details (C9).
- CO-8: zone from `useSettings` (C8).

## Assumed
None. The design's one assumption (the integer appointment id in EP-1) is settled by the existing `NextAppointment` type (C3, C4).

## Unknown
None.

## Open questions
None.
