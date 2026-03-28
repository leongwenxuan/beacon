# Story 3.2: Enrich Shortlisted Candidates with External Supporting Evidence

Status: ready-for-dev

## Story

As a renter,
I want Beacon to gather building, layout, and neighborhood context beyond the source listing,
So that shortlisted properties become decision-useful even when original listings are incomplete.

## Acceptance Criteria

1. **Given** a shortlist exists for a session **When** enrichment is launched **Then** Beacon creates persisted enrichment workflow records for shortlisted candidates **And** uses external retrieval paths beyond the original listing page to gather supporting evidence.

2. **Given** new supporting evidence is found **When** Beacon stores enrichment outputs **Then** candidate records are updated with additional property, building, or contextual facts **And** the original source references and provenance are preserved alongside newly retrieved information.

3. **Given** one candidate remains sparse after initial enrichment **When** Beacon determines key recommendation inputs are still missing **Then** the system can continue enrichment iteratively within bounded workflow rules **And** records the candidate's remaining gaps explicitly rather than silently stopping.

## Tasks / Subtasks

- [ ] Launch enrichment runs from the canonical shortlist state produced in Story `2.4`.
- [ ] Create persisted enrichment `workerRuns` or equivalent workflow records linked to the session and shortlisted candidates.
- [ ] Use external retrieval paths beyond the original listing page through server-side integration boundaries.
- [ ] Merge new property, building, layout, and contextual facts into canonical candidate state while preserving existing provenance.
- [ ] Keep enrichment bounded through explicit iteration rules, pass limits, and/or time-budget rules.
- [ ] Persist remaining enrichment gaps explicitly after each pass so the system can decide what still needs work.
- [ ] Record enrichment lifecycle events to activity logs so operator tooling can later inspect what was attempted and what remained incomplete.
- [ ] Keep enrichment output compatible with later conflict reconciliation and safe-reuse logic.
- [ ] Avoid silent enrichment termination when important recommendation inputs remain missing.

## Dev Notes

- Depends on Story `3.1`, which established evidence-state, provenance, and explicit unknown/gap modeling.
- This story is about improving candidate completeness beyond listing-only facts, not about final recommendation synthesis.
- Previous story intelligence: reuse the evidence/provenance helper layer from `3.1` so enrichment writes remain consistent with canonical evidence semantics.
- Bounded iterative enrichment matters more than naive depth; this workflow should stop deliberately, not drift indefinitely.

### Architecture Guardrails

- Use persisted `workerRuns` and session state to model enrichment progress; long-running work must not hide in untracked async behavior.
- Keep TinyFish and other retrieval integrations behind server-side Convex boundaries.
- Preserve original source references and provenance instead of overwriting earlier evidence blindly.
- Missing recommendation-critical facts should remain explicit after each pass if they are still unresolved.
- Convex remains the canonical store for candidate evidence state, workflow state, and enrichment outputs.
- External retrieval data must be validated at the boundary before it becomes canonical state.

### File Structure Notes

- `convex/orchestrator/enrichment.ts`
- `convex/orchestrator/helpers.ts`
- `convex/workerRuns/mutations.ts`
- `convex/activityLogs/mutations.ts`
- `convex/unitRecords/mutations.ts`
- `convex/integrations/tinyfish/actions.ts`
- `convex/integrations/tinyfish/mappers.ts`
- `convex/lib/evidence.ts`

### Testing Requirements

- Verify enrichment launches from the persisted shortlist and creates candidate-linked workflow records.
- Verify newly retrieved evidence merges into canonical candidate state without losing prior provenance.
- Verify bounded iterative enrichment records remaining gaps instead of silently stopping.
- Verify enrichment activity is inspectable via worker-run/activity-log state.
- Run lint/typecheck after orchestrator and candidate-write updates.

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
