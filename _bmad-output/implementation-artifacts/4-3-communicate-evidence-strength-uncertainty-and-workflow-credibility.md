# Story 4.3: Communicate Evidence Strength, Uncertainty, and Workflow Credibility

Status: ready-for-dev

## Story

As a renter or demo viewer,
I want Beacon to show how strong or incomplete the evidence is and that the result came from a real workflow,
So that I can trust the output without assuming false precision.

## Acceptance Criteria

1. **Given** the recommendation experience is rendered **When** evidence is strong, partial, conflicting, or uncertain **Then** Beacon communicates that evidence strength explicitly in the UI **And** avoids presenting uncertain outputs as equally certain facts.

2. **Given** the recommendation was produced through multi-stage workflow execution **When** the result is displayed **Then** the output reflects the lineage of discovery, enrichment, and synthesis **And** makes it clear that Beacon performed real orchestration rather than serving a static mock result.

3. **Given** the recommendation includes inferred or synthesized content **When** the renter or demo viewer reviews it **Then** Beacon distinguishes those outputs from directly retrieved facts **And** keeps trust and explainability central to the experience.

## Tasks / Subtasks

- [ ] Add evidence-strength and uncertainty presentation to the recommendation experience using canonical evidence/recommendation state.
- [ ] Distinguish retrieved facts, inferred outputs, and unknown/missing information in renter-facing recommendation UI.
- [ ] Surface workflow lineage so users can see the recommendation came from discovery, enrichment, and synthesis rather than a static final screen.
- [ ] Add partial-ready/degraded recommendation messaging where appropriate without collapsing usable output.
- [ ] Keep uncertainty/trust messaging anchored to canonical session, evidence, and recommendation artifact data.
- [ ] Avoid fake precision or overconfident language when evidence quality is partial or conflicting.
- [ ] Keep renter-safe workflow credibility messaging distinct from deeper operator-only diagnostics.
- [ ] Ensure recommendation trust indicators stay readable and accessible, not buried in dense internal detail.

## Dev Notes

- Depends on Story `4.2`, which built the comparison-ready top-three recommendation experience.
- This story is how Beacon proves it is doing real multi-stage work and remains honest about evidence quality.
- Previous story intelligence: preserve the same recommendation artifact and evidence model from earlier stories so trust messaging is grounded in canonical state rather than UI-only interpretation.
- Workflow credibility is a product requirement, not just observability polish; the recommendation experience should show enough lineage to feel causally connected to the orchestration.

### Architecture Guardrails

- Trust depends on clear distinction between retrieved facts, inferred outputs, and unknown information.
- Frontend trust depends on backend transparency; enough session progress and output lineage must be visible for users and demo viewers to believe the recommendation reflects real work.
- Recommendation and workflow lineage output should read canonical Convex state rather than recomputing credibility client-side.
- Keep operator-level diagnostics separate from renter-safe workflow summaries.
- Avoid false precision when evidence is partial, conflicting, or uncertain.
- Use accessible labels, readable hierarchy, and status messaging sufficient for core recommendation flows.

### File Structure Notes

- `src/app/recommendation/[sessionId]/page.tsx`
- `src/features/recommendation/components/`
- `src/features/recommendation/lib/`
- `convex/recommendationArtifacts/queries.ts`
- `convex/searchSessions/queries.ts`
- `convex/activityLogs/queries.ts`
- `convex/workerRuns/queries.ts`
- `src/types/recommendation.ts`

### Testing Requirements

- Verify evidence-strength and uncertainty states are visible in the renter recommendation experience.
- Verify inferred/synthesized content is clearly distinguished from directly retrieved facts.
- Verify workflow-lineage messaging reflects persisted discovery/enrichment/synthesis progress rather than static copy.
- Verify partial-ready or degraded recommendation states remain usable and honest.
- Run lint/typecheck after trust/lineage UI and query updates.

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
