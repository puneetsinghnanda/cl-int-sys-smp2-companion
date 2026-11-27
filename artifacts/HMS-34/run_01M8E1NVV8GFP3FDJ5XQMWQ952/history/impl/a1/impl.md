---
schema: change.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: impl
attempt: 1
skill: implement@1.4.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md
    sha256: 49e40dd8e05c2f3d0fe0da3f188d2de68b5fc4abce7931c55896c0850ac3d324
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/split.md
    sha256: 75a7d123b0e9c6c2abede2df9d1cedfa4c8cb1c4885b7f3d7d41dbe935908603
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: f2b2e6084f36f0ed4d7e353c83d1e0b4e9bc2efc
    diff_sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
    files: []
claims:
  - id: C1
    text: "The design needs a new migration `__appointment_stale.sql` to allow STALE in the status and event CHECKs."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md#item:CO-2"]
  - id: C2
    text: "Paths under `**/migrations/**` are sensitive in this system's change limits."
    cites: ["tsl.yaml#L37"]
  - id: C3
    text: "Database changes are new SQL files only, named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-10, REQ-11, REQ-12, REQ-13]
known: [CO-2]
assumed: []
unknown:
  - {id: U-1, blocking: true}
questions:
  - id: Q-01
    blocking: true
    ask: tech lead
    text: "CO-2 needs a new file under db/migrations, which is a sensitive path (`**/migrations/**`). May the implement step add the migration <UTC timestamp>__appointment_stale.sql that replaces the appointment.status and appointment_event.event CHECK constraints to allow STALE?"
    why: "Without the migration, every STALE update and STALE event insert fails the existing CHECK constraints, so S-1 to S-4 cannot work. The change limits forbid touching a sensitive path without a question."
    evidence: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md#item:CO-2", "tsl.yaml#L37", "context/conventions.md#database-changes"]
    options:
      - "Yes, add the migration as designed (proposed default)"
      - "No, the tech lead writes the migration; implement does the rest"
      - "Stop: change the design"
    default: "Yes, add the migration as designed (proposed default)"
summary: "No product edits yet: the design's migration (CO-2) is under the sensitive path **/migrations/**, so implementation is paused on blocking question Q-01 for the tech lead."
---
# Change: HMS-34

## Summary
No product code is changed in this attempt. The design's CO-2 adds a new SQL file under
db/migrations to allow `STALE` in the appointment status and history event CHECK constraints
(C1), and `**/migrations/**` is a sensitive path in the change limits (C2). Per the rules, I
stopped before editing and raise blocking question Q-01 to the tech lead. The other components
(CO-1, CO-3 to CO-8) depend on that migration, because STALE updates and STALE events would fail
the existing CHECKs. So they are left for the next attempt, which builds the whole design in one
diff once Q-01 is answered. CO-9 (glossary) is a companion-context edit by the tech lead, not
product code.

## Changes
None in this attempt. Planned once Q-01 is answered:
- db/migrations/<UTC timestamp>__appointment_stale.sql: CO-2.
- api/src/types.ts: CO-1.
- api/src/repositories/appointmentRepo.ts: CO-3.
- api/src/services/appointmentService.ts: CO-4.
- api/src/server.ts: CO-5.
- web/src/api/types.ts: CO-6.
- web/src/lib/format.ts: CO-7.
- web/src/pages/AppointmentDetailPage.tsx: CO-8.

## Known
- CO-2: the design's migration is a new file under db/migrations, a sensitive path in the change limits (claims C1 to C3).

## Assumed
None.

## Unknown
- U-1 (blocking): may the implement step add a file under the sensitive path db/migrations for CO-2? See Q-01.

## Open questions
- U-1 (blocking) is raised by Q-01.
- Q-01 (blocking, tech lead): CO-2 needs a new file under db/migrations, which is a sensitive path (`**/migrations/**`). May the implement step add the migration <UTC timestamp>__appointment_stale.sql that replaces the appointment.status and appointment_event.event CHECK constraints to allow STALE? Proposed default: Yes, add the migration as designed.
