# Story 3.1: Model Evidence Completeness, Provenance, and Unknowns

Status: ready-for-dev

## Story

As a renter,
I want Beacon to track what it knows, what it inferred, and what is still missing for each candidate,
So that later recommendations can be trustworthy instead of pretending certainty.

## Acceptance Criteria

1. **Given** candidate records exist for a session **When** Beacon prepares them for enrichment **Then** each candidate supports explicit evidence-state fields that distinguish `retrieved`, `inferred`, and `unknown` data **And** retains provenance metadata for important facts that may influence ranking or explanation.

2. **Given** a candidate has incomplete property information **When** Beacon evaluates it **Then** missing information is represented explicitly in persisted state **And** the system can identify what enrichment work remains before recommendation synthesis.

3. **Given** evidence-state modeling is in place **When** downstream workflows read candidate data **Then** they can rely on canonical Convex state instead of duplicating ad hoc completeness logic elsewhere **And** recommendation and operator surfaces can later expose evidence quality consistently.

## Tasks / Subtasks

- [ ] Extend canonical candidate/unit state to distinguish `retrieved`, `inferred`, and `unknown` values for important recommendation-impacting fields.
- [ ] Persist provenance metadata for facts that influence ranking, explanation, or trust-oriented UI.
- [ ] Introduce an explicit gap/completeness model so Beacon can tell what evidence is still missing for each candidate.
- [ ] Add shared evidence/provenance helper logic in Convex so completeness rules are not reimplemented in multiple workflows.
- [ ] Keep candidate evidence state queryable by enrichment, recommendation, and operator surfaces without additional translation layers.
- [ ] Ensure field-level unknowns are explicit rather than implied by nullish/fabricated defaults.
- [ ] Align the candidate read/write model with later uncertainty communication and evidence-strength UI requirements.
- [ ] Preserve compatibility with earlier shortlist/ranking state from Epic 2 while deepening the candidate schema for Epic 3+.

## Dev Notes

- Depends on Story `2.4`, which produced the evolving ranked top-three shortlist and canonical unit/candidate state that Epic 3 now enriches.
- This story is the foundation for trustworthy recommendation output, evidence-aware ranking, and operator traceability.
- Previous story intelligence: keep shortlist linkage and canonical `unitRecords` from Epic 2 intact so evidence modeling extends existing records rather than inventing a second candidate truth model.
- Unknown data must be first-class state; later workflows should be able to distinguish “missing”, “inferred”, and “retrieved” without guessing.

### Architecture Guardrails

- The implementation must distinguish `retrieved`, `inferred`, and `unknown` data explicitly in stored state and UI presentation.
- Convex remains the single source of truth for workflow state, candidate evidence state, and provenance metadata.
- Provenance and confidence modeling are first-class architectural concerns and must not be deferred to ad hoc UI logic.
- Use shared backend helpers for evidence/completeness logic rather than duplicating rules in orchestrator, recommendation, and operator code.
- Keep application-controlled field names camelCase and typed at the boundary.
- Recommendation and operator surfaces should consume canonical evidence state from Convex, not re-derive trust semantics in the client.

### File Structure Notes

- `convex/schema.ts`
- `convex/unitRecords/mutations.ts`
- `convex/unitRecords/queries.ts`
- `convex/lib/evidence.ts`
- `convex/lib/provenance.ts`
- `convex/lib/validation.ts`
- `src/types/evidence.ts`
- `src/types/search.ts`

### Testing Requirements

- Verify candidate records can persist field-level `retrieved`, `inferred`, and `unknown` evidence states.
- Verify important facts retain provenance metadata after persistence and updates.
- Verify enrichment-gap/completeness metadata is queryable by downstream workflows.
- Verify downstream readers can consume canonical evidence state without reimplementing completeness rules.
- Run lint/typecheck after schema/helper updates.

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
