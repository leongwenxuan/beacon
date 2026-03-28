# Story 2.4: Normalize Candidate Sets and Produce an Evolving Ranked Top-3 Shortlist

Status: ready-for-dev

## Story

As a renter,
I want Beacon to merge overlapping listings and surface a ranked top-three shortlist from discovered candidates,
So that I can see the strongest current options before deeper enrichment is finished.

## Acceptance Criteria

1. **Given** multiple discovered candidates may refer to the same property or unit **When** Beacon processes the candidate set **Then** it applies deterministic normalization and duplicate-detection rules **And** consolidates overlapping candidates into a single canonical unit record where confidence is sufficient.

2. **Given** duplicate confidence is weak or ambiguous **When** Beacon cannot safely merge candidates **Then** it keeps them separate **And** preserves enough state for later refinement instead of making an unsafe merge.

3. **Given** a session has a normalized candidate set with enough information for initial scoring **When** shortlist generation runs **Then** Beacon ranks candidates against the renter's stated priorities and persists a top-three shortlist linked to the session **And** each shortlisted property includes linkage to its original source listing.

4. **Given** new discovery results or better candidate data arrive **When** Beacon recomputes shortlist quality **Then** it can update the ranked top-three shortlist in persisted state **And** preserves continuity so the renter sees that the shortlist is evolving rather than resetting arbitrarily.

## Tasks / Subtasks

- [ ] Implement deterministic normalization rules for overlapping candidate records using source ids, listing identifiers, URLs, and other safe matching signals.
- [ ] Persist duplicate/merge confidence so Beacon can distinguish safe merges from ambiguous overlaps.
- [ ] Keep ambiguous candidates separate when confidence is not sufficient for a canonical merge.
- [ ] Build shortlist scoring against normalized renter priorities from the session intent.
- [ ] Persist the ranked top-three shortlist in canonical Convex state with stable linkage to the underlying candidate/unit records and source listings.
- [ ] Support shortlist recomputation when additional discovery results or stronger candidate data arrive later in the session.
- [ ] Preserve shortlist continuity/versioning so live progress surfaces can show the shortlist evolving instead of resetting.
- [ ] Update session status and progress metadata so Epic 3 can hand off directly into evidence completion from canonical shortlist state.
- [ ] Keep ranking and normalization logic server-side in Convex/orchestrator helpers, not in client code.

## Dev Notes

- Depends on Story `2.3`, which persisted raw candidates, discovery telemetry, and source linkage into canonical Convex state.
- This story produces the first true recommendation-shaped output in the workflow: an evolving ranked top three backed by real candidates.
- Previous story intelligence: preserve candidate provenance and source-linkage from `2.3` so shortlist entries remain traceable back to raw discovery state.
- Unsafe merges are worse than duplicate candidates; keep ambiguity explicit so Epic 3 can refine with more evidence.

### Architecture Guardrails

- Convex remains the single source of truth for candidate normalization, ranking, shortlist state, and session workflow transitions.
- `unitRecords` are the canonical source candidate entities; shortlist state should link back to them rather than duplicating detached client objects.
- Ranking must be grounded in renter priorities captured in the session intent, not arbitrary client-side heuristics.
- Each shortlisted property must keep linkage to its original source listing.
- Partial or evolving state should stay visible and persisted so the shortlist can improve over time without breaking continuity.
- Keep normalization, duplicate detection, and scoring logic in backend/orchestrator helpers under the documented project structure.

### File Structure Notes

- `convex/unitRecords/mutations.ts`
- `convex/unitRecords/queries.ts`
- `convex/orchestrator/discovery.ts`
- `convex/orchestrator/helpers.ts`
- `convex/lib/scoring.ts`
- `convex/searchSessions/mutations.ts`
- `convex/searchSessions/queries.ts`
- `src/types/search.ts`

### Testing Requirements

- Verify strong duplicate candidates normalize into one canonical candidate/unit record.
- Verify ambiguous overlap keeps candidates separate instead of forcing an unsafe merge.
- Verify shortlist scoring persists an ordered top three linked to the correct source listings.
- Verify shortlist recomputation updates canonical shortlist state without collapsing continuity.
- Run lint/typecheck after normalization and scoring logic changes.

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
