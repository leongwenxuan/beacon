# Story 2.3: Persist Discovery Telemetry and Raw Candidates with Source Linkage

Status: ready-for-dev

## Story

As a renter,
I want discovered properties and the way Beacon found them to be stored against my session,
So that the system can build a shortlist from real source-backed candidates and improve its discovery behavior over time.

## Acceptance Criteria

1. **Given** discovery returns candidate properties from one or more sources **When** Beacon receives those results **Then** it stores candidate records in Convex linked to the owning `searchSession` **And** each candidate retains its source listing reference, provider-origin metadata, and the source plan or run that produced it.

2. **Given** a discovery attempt completes or fails **When** Beacon persists run outputs **Then** it stores the executed `entryUrl`, `appliedUrl`, filter plan, execution mode, navigation telemetry, and result counts **And** that state can later be inspected for debugging recall, speed, and browser struggle.

3. **Given** a candidate is persisted **When** required listing facts are incomplete **Then** Beacon still stores the candidate in a usable partial form **And** marks missing or unknown fields explicitly instead of fabricating completeness.

## Tasks / Subtasks

- [ ] Define the canonical raw-candidate persistence shape in `unitRecords`, including session linkage, source linkage, listing reference, and discovery-run provenance.
- [ ] Validate TinyFish discovery outputs at the external boundary before merging them into canonical Convex state.
- [ ] Persist discovery telemetry to `workerRuns`, including `entryUrl`, `appliedUrl`, filter plan, execution mode, navigation telemetry, and result counts.
- [ ] Implement idempotent candidate upsert logic so the same source/listing does not create uncontrolled duplicate canonical rows.
- [ ] Preserve provider-origin metadata and the source plan/run that produced each candidate for later operator inspection and ranking lineage.
- [ ] Persist incomplete candidates in usable partial form with explicit unknown/missing fields.
- [ ] Update session-level progress metadata or counters so live session surfaces can show discovery progress without reparsing raw logs.
- [ ] Add activity-log records for candidate persistence and discovery completion/degradation events.
- [ ] Keep this story focused on persistence and provenance; ranking/normalization belongs to Story `2.4`.

## Dev Notes

- Depends on Story `2.2`, which established direct-url-first execution, telemetry capture, and degraded fallback behavior.
- This story is where discovery output becomes canonical application data instead of transient integration output.
- Previous story intelligence: preserve the exact telemetry and worker-run vocabulary from `2.2` so operator surfaces can later explain browser struggle and degraded sources cleanly.
- Candidate persistence should stay honest to incomplete data; explicit unknowns are better than false completeness because Epic 3 depends on gap-aware evidence modeling.

### Architecture Guardrails

- Convex owns canonical workflow state and persistence; provider payloads must be mapped into internal domain models before storage.
- `unitRecords` are the canonical candidate entities for discovery, ranking, enrichment, and recommendation.
- Discovery telemetry must be persisted so navigation struggle and degraded runs are inspectable later.
- Use camelCase field names, typed validators, and domain-oriented mutations such as `unitRecords.upsertFromDiscovery`.
- Distinguish incomplete or unknown fields explicitly instead of fabricating values for convenience.
- Keep provider-specific mapping logic inside integration boundaries and backend helpers, not in frontend code.

### File Structure Notes

- `convex/unitRecords/mutations.ts`
- `convex/unitRecords/queries.ts`
- `convex/workerRuns/mutations.ts`
- `convex/workerRuns/helpers.ts`
- `convex/activityLogs/mutations.ts`
- `convex/integrations/tinyfish/mappers.ts`
- `convex/lib/validation.ts`
- `convex/schema.ts`

### Testing Requirements

- Verify successful discovery writes canonical candidates linked to the correct session, source, and worker run.
- Verify executed URL/mode/telemetry metadata persists for both successful and degraded discovery attempts.
- Verify incomplete discovery payloads still persist in usable partial form with explicit unknown fields.
- Verify repeated discovery of the same listing does not create uncontrolled duplicate canonical candidate rows.
- Run lint/typecheck after schema, validator, and persistence updates.

### References

- `_bmad-output/planning-artifacts/epics.md`
- `_bmad-output/planning-artifacts/prd.md`
- `_bmad-output/planning-artifacts/architecture.md`

## Dev Agent Record

### Agent Model Used

_(fill on completion)_

### Debug Log References

_(fill on completion)_

### Completion Notes List

- _(empty)_

### File List

- _(empty)_
