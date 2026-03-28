# Story 3.3: Reconcile Conflicting Facts and Persist Confidence-Aware Candidate State

Status: ready-for-dev

## Story

As a renter,
I want Beacon to handle conflicting facts across sources transparently,
So that the shortlist becomes more trustworthy instead of flattening disagreement into false certainty.

## Acceptance Criteria

1. **Given** two or more sources disagree on an important fact for a candidate **When** Beacon evaluates the conflict **Then** it applies explicit reconciliation rules or confidence-aware selection logic **And** preserves enough provenance to explain which fact was chosen or why uncertainty remains.

2. **Given** conflict resolution cannot safely produce a single authoritative answer **When** the candidate is updated **Then** Beacon stores the field in a partial or uncertain state **And** avoids presenting the result as fully resolved truth.

3. **Given** candidate records have reconciled outputs **When** later ranking or recommendation workflows consume them **Then** they can account for evidence strength and conflict state **And** continue operating without requiring every field to be perfectly complete.

## Tasks / Subtasks

- [ ] Detect conflicting facts across listing and enrichment sources for important candidate fields.
- [ ] Define explicit reconciliation rules or confidence-aware selection logic in shared backend helpers.
- [ ] Preserve provenance for both chosen and unresolved conflicting values so later recommendation and operator flows can explain the outcome.
- [ ] Persist partial/uncertain candidate state when no safe single answer exists.
- [ ] Keep downstream consumers able to read conflict-aware candidate data without failing when uncertainty remains.
- [ ] Align reconciliation output with the evidence-state model from Story `3.1` and the enrichment outputs from Story `3.2`.
- [ ] Ensure ranking and later recommendation flows can see confidence/uncertainty state rather than only flattened final values.
- [ ] Record conflict-resolution or unresolved-conflict milestones in activity/workflow state where needed for operator visibility.

## Dev Notes

- Depends on Story `3.2`, which enriched shortlisted candidates with additional external evidence.
- This story is the safeguard against false certainty once multiple evidence sources exist for the same candidate fact.
- Previous story intelligence: preserve the evidence/provenance semantics from `3.1` and the enrichment lineage from `3.2`; reconciliation should not erase how the system learned something.
- A field staying uncertain is acceptable; a confident but wrong flattening is not.

### Architecture Guardrails

- Provenance and confidence modeling are first-class architectural concerns and must remain visible in canonical state.
- Recommendation quality depends on evidence completeness and trustworthiness, not just having one scalar value everywhere.
- Keep reconciliation logic in shared backend helpers so multiple workflows do not invent divergent conflict rules.
- Distinguish clearly between chosen retrieved facts, inferred outputs, and unresolved/unknown states.
- Convex remains the single source of truth for reconciled candidate state and downstream workflow reads.
- Recommendation and operator surfaces must be able to consume conflict-aware state without reimplementing reconciliation in the client.

### File Structure Notes

- `convex/lib/evidence.ts`
- `convex/lib/provenance.ts`
- `convex/lib/scoring.ts`
- `convex/unitRecords/mutations.ts`
- `convex/unitRecords/queries.ts`
- `convex/orchestrator/enrichment.ts`
- `src/types/evidence.ts`

### Testing Requirements

- Verify conflicting facts from multiple sources trigger deterministic reconciliation or explicit uncertain state.
- Verify unresolved conflicts preserve provenance and do not collapse into false certainty.
- Verify downstream consumers can read conflict-aware candidate state without requiring perfect completeness.
- Verify confidence/uncertainty state remains available for later ranking and recommendation workflows.
- Run lint/typecheck after helper and schema updates.

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
