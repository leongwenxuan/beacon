# Story 5.3: Present Inferred Visuals with Clear Labeling and Evidence Context

Status: ready-for-dev

## Story

As a renter,
I want generated property visuals to be shown with honest labeling and context,
So that they help me understand the recommendation without misleading me into thinking they are exact reconstructions.

## Acceptance Criteria

1. **Given** an inferred visual artifact exists for the recommendation **When** the renter views the recommendation experience **Then** Beacon presents the generated visual as part of the recommendation flow **And** labels it clearly as inferred rather than exact.

2. **Given** both retrieved imagery and generated visuals may exist **When** Beacon renders them **Then** the UI distinguishes source imagery from inferred previews **And** keeps the relationship to evidence and provenance understandable.

3. **Given** the generated visual is derived from limited evidence **When** it is shown **Then** Beacon communicates that it is a grounded synthesis rather than a guaranteed reconstruction **And** preserves trust in the broader recommendation artifact.

## Tasks / Subtasks

- [ ] Render inferred visuals from canonical `imageArtifacts` and recommendation linkage in the renter-facing recommendation experience.
- [ ] Add explicit “inferred” labeling and supporting context that makes clear the preview is not an exact reconstruction.
- [ ] Distinguish retrieved/source imagery from generated previews in layout, copy, and metadata presentation.
- [ ] Surface enough provenance/evidence context for the renter to understand what grounded the inferred output.
- [ ] Keep visual presentation anchored to canonical artifact state and recommendation linkage rather than temporary provider response data.
- [ ] Preserve accessibility and readable hierarchy so inferred-vs-retrieved distinctions are understandable to all users.
- [ ] Keep the recommendation experience trustworthy when inferred visuals are derived from limited evidence.
- [ ] Prepare the UI for the degraded/missing-visual path addressed in Story `5.4`.

## Dev Notes

- Depends on Story `5.2`, which generated and persisted inferred visual artifacts linked to the best recommendation.
- This story is about honest presentation and trust, not about generation/storage mechanics.
- Previous story intelligence: use canonical artifact linkage and evidence lineage from `5.2` so labeling and context are grounded in real persisted state.
- The visual should help with understanding, not overclaim precision or authenticity.

### Architecture Guardrails

- The implementation must distinguish `retrieved`, `inferred`, and `unknown` data explicitly in stored state and UI presentation.
- Generated visuals must be explicitly presented as inferred rather than exact reconstructions.
- Frontend trust depends on backend transparency; the relationship between generated visuals and their evidence should remain understandable.
- Recommendation and visual UI should consume canonical Convex-backed artifact state, not provider URLs as truth.
- Keep provider logic server-side and presentation logic inside the documented recommendation/visual feature boundaries.
- Practical accessibility still applies to live and final recommendation experiences.

### File Structure Notes

- `src/app/recommendation/[sessionId]/page.tsx`
- `src/features/recommendation/components/`
- `src/features/visuals/components/`
- `src/features/visuals/lib/`
- `convex/imageArtifacts/queries.ts`
- `convex/recommendationArtifacts/queries.ts`
- `src/types/visuals.ts`

### Testing Requirements

- Verify inferred visuals are clearly labeled as inferred in the recommendation experience.
- Verify retrieved/source imagery and generated previews are visually and semantically distinct.
- Verify provenance/evidence context is understandable without exposing raw internal ids or provider internals.
- Verify the recommendation UI remains trustworthy and coherent when inferred visuals are grounded in limited evidence.
- Run lint/typecheck after visual presentation and query updates.

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
