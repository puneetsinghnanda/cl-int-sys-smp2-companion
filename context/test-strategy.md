---
schema: context-section.v1
section: test-strategy
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# Test strategy {#test-strategy}

## Layers {#layers}
API tests use Vitest and Supertest against a throwaway PostgreSQL 16 with a fixed clock. Web tests use
Vitest with jsdom and Testing Library. The db workspace tests the seed and the migrations.
Sources: sample-system:api/vitest.config.mts#L9-L10, sample-system:web/vitest.config.mts#L10

## Requirement IDs {#requirement-ids}
Every PRD requirement has at least one test whose name starts with its ID (DOC-1, APT-4, ...).
New requirements get new IDs and tests.
Sources: sample-system:docs/PRD.md#L250

## Database for tests {#database-for-tests}
The harness starts a temporary cluster from PostgreSQL 16 binaries or uses `HMS_TEST_DATABASE_URL`.
Without either, database tests are skipped. Tests never touch Neon.
Sources: sample-system:db/testing/pg-harness.ts#L10-L11

## How to run {#how-to-run}
`npm run lint`, `npm run typecheck` and `npm test` at the repository root; CI runs the same on every
pull request.
Sources: sample-system:package.json#L17-L19

## Gaps {#gaps}
- Web tests cover the dashboard links, the slot picker and formatting only; pages are not tested end to end.
