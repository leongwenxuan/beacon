# Story 5.2: Generate an Inferred Visual Preview for the Best Recommendation

Status: ready-for-dev

## Story

As a renter,
I want Beacon to generate a plausible visual preview of the recommended property from available evidence,
So that I can form a stronger mental model even when source imagery is incomplete.

## Acceptance Criteria

1. **Given** a best recommendation exists with usable visual or layout evidence **When** inferred-visual generation is triggered **Then** Beacon calls `fal.ai` through a server-side integration path **And** stores workflow state for the generation request in canonical session records.

2. **Given** `fal.ai` returns generated visual output **When** Beacon ingests the result **Then** it stores the generated image bytes in Convex file storage **And** links the resulting artifact to the recommendation and underlying evidence records used to produce it.

3. **Given** the available evidence is partial **When** visual generation still proceeds **Then** Beacon can create a plausible inferred preview from the best available inputs **And** preserves the distinction between generated output and retrieved listing imagery.

## Tasks / Subtasks

- [ ] Trigger inferred-visual generation from the canonical best recommendation and available visual/evidence context.
- [ ] Call `fal.ai` only through server-side Convex/integration boundaries.
- [ ] Persist visual-generation workflow state in canonical session and/or worker-run records.
- [ ] Store generated image bytes in Convex file storage and record canonical inferred-artifact metadata in `imageArtifacts`.
- [ ] Link inferred visual artifacts back to the recommendation artifact and the evidence/artifacts that informed generation.
- [ ] Preserve the distinction between retrieved imagery and generated imagery at both the artifact and workflow levels.
- [ ] Keep generation compatible with partial evidence rather than requiring perfect inputs.
- [ ] Record generation outcomes and failures for later operator inspection and graceful degradation.

## Dev Notes

- Depends on Story `5.1`, which established durable storage of retrieved visual evidence in canonical `imageArtifacts`.
- This story adds inferred generation as a grounded extension of stored evidence, not as a detached novelty output.
- Previous story intelligence: use stored `imageArtifacts`, candidate evidence state, and recommendation artifact linkage as generation inputs instead of re-fetching transient provider assets.
- Generated visuals must remain traceable back to the evidence and recommendation records that produced them.

### Architecture Guardrails

- `fal.ai` via `@fal-ai/client 1.9.5` is the required provider for inferred property visual generation.
- All provider integrations and credentials must stay server-side.
- Generated images must be fetched and stored into Convex file storage before they are attached to recommendation records.
- `imageArtifacts` should carry canonical linkage between generated outputs, recommendation artifacts, and supporting evidence.
- The system must distinguish generated/inferred outputs from directly retrieved imagery and facts.
- Workflow state should use explicit persisted statuses such as `generatingVisuals`, `ready`, `partialReady`, and `failed`.

### File Structure Notes

- `convex/integrations/fal/actions.ts`
- `convex/integrations/fal/mappers.ts`
- `convex/integrations/fal/types.ts`
- `convex/orchestrator/visuals.ts`
- `convex/workerRuns/mutations.ts`
- `convex/imageArtifacts/mutations.ts`
- `convex/recommendationArtifacts/mutations.ts`
- `src/types/visuals.ts`

### Testing Requirements

- Verify inferred-visual generation is triggered from canonical recommendation/evidence state rather than client input.
- Verify successful generated outputs are stored in Convex file storage and linked to recommendation/evidence records.
- Verify generated imagery remains explicitly distinct from retrieved imagery in canonical artifact state.
- Verify failures are recorded in workflow state without creating bogus artifact pointers.
- Run lint/typecheck after fal integration and artifact-linkage updates.

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
