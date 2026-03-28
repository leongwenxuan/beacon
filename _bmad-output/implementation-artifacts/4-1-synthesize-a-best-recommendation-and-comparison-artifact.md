# Story 4.1: Synthesize a Best Recommendation and Comparison Artifact

Status: ready-for-dev

## Story

As a renter,
I want Beacon to turn enriched shortlisted candidates into one ranked recommendation artifact,
So that I can evaluate the best options without manually stitching evidence together.

## Acceptance Criteria

1. **Given** a session has a shortlist with enough evidence for synthesis **When** recommendation generation runs **Then** Beacon creates a persisted recommendation artifact in Convex for the session **And** identifies one best overall recommendation plus the remaining ranked shortlist entries.

2. **Given** the recommendation artifact is created **When** it is stored **Then** it remains linked to the underlying session, shortlisted units, and important evidence references **And** can be re-read by renter-facing and operator-facing surfaces from canonical state.

3. **Given** evidence quality is imperfect **When** synthesis still has enough signal to proceed **Then** Beacon can create a usable recommendation artifact without waiting for perfect completeness **And** preserves the state needed for later partial-ready presentation.

## Tasks / Subtasks

- [ ] Define the canonical `recommendationArtifacts` persistence shape, including session linkage, ranked candidates, best selection, and evidence lineage.
- [ ] Build server-side recommendation synthesis on top of the enriched and conflict-aware candidate state from Epic 3.
- [ ] Persist the best overall recommendation plus the ranked top-three comparison artifact in Convex.
- [ ] Keep recommendation artifacts linked to the source session, shortlisted candidates, and relevant evidence/provenance references.
- [ ] Support partial-ready recommendation synthesis when there is enough trustworthy signal even if evidence remains incomplete.
- [ ] Persist synthesis lifecycle/workflow state so later renter and operator surfaces can inspect how recommendation output was produced.
- [ ] Keep recommendation reads canonical and queryable by both renter-facing and operator-facing surfaces.
- [ ] Ensure reruns or later synthesis improvements can update or version recommendation output without losing traceability.

## Dev Notes

- Depends on Story `3.4`, which established safe reuse and partial-failure preservation for evidence state.
- This story is the boundary where Beacon stops being only a workflow engine and starts producing a canonical recommendation artifact.
- Previous story intelligence: preserve the evidence-quality, uncertainty, and partial-ready semantics from Epic 3 so recommendation synthesis does not flatten uncertainty into fake certainty.
- Recommendation artifacts should be treated as durable application state, not transient derived UI-only objects.

### Architecture Guardrails

- Convex remains the canonical source of truth for recommendation artifacts and their relationship to sessions, candidates, and evidence.
- Recommendation and comparison output must stay linked to the session, shortlisted units, and important evidence references.
- Use asynchronous workflow execution with persisted status transitions rather than hidden request-response only logic.
- Recommendation synthesis should consume canonical candidate/evidence state from Convex instead of duplicate client-side or temporary stores.
- Partial-ready output is allowed when evidence is imperfect but still useful; do not block on impossible completeness.
- Keep provider/model integrations server-side and validate external outputs before merging them into canonical recommendation state.

### File Structure Notes

- `convex/recommendationArtifacts/mutations.ts`
- `convex/recommendationArtifacts/queries.ts`
- `convex/recommendationArtifacts/helpers.ts`
- `convex/orchestrator/synthesis.ts`
- `convex/orchestrator/helpers.ts`
- `convex/searchSessions/mutations.ts`
- `convex/lib/provenance.ts`
- `src/types/recommendation.ts`

### Testing Requirements

- Verify synthesis persists one best recommendation and the ordered ranked shortlist in canonical state.
- Verify recommendation artifacts remain linked to the session, candidates, and evidence references.
- Verify partial-ready recommendation artifacts can still be created when signal is sufficient but evidence is incomplete.
- Verify recommendation artifact reads are stable for both renter-facing and operator-facing consumers.
- Run lint/typecheck after synthesis and artifact-schema updates.

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
