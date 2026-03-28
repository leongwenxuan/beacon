# Story 4.4: Support Source Clickthrough and No-Perfect-Match Outcomes

Status: ready-for-dev

## Story

As a renter,
I want to click through to source listings and still get a useful recommendation when no option is perfect,
So that Beacon remains actionable in real housing search conditions.

## Acceptance Criteria

1. **Given** a shortlisted property appears in the recommendation output **When** the renter chooses to inspect it further **Then** Beacon provides clickthrough access to the original source listing **And** preserves the mapping between the displayed recommendation entry and its source record.

2. **Given** no shortlisted property is a perfect match **When** Beacon produces the recommendation **Then** it still returns the best available option and the top-three comparison **And** frames the output as a tradeoff-aware recommendation rather than a false exact match.

3. **Given** source or evidence quality is uneven across the shortlist **When** the renter uses the final output **Then** Beacon remains actionable and understandable **And** does not collapse the recommendation surface simply because the data is imperfect.

## Tasks / Subtasks

- [ ] Expose original source-listing clickthrough controls for each shortlisted recommendation entry using canonical source linkage from candidate/unit state.
- [ ] Keep the mapping between rendered recommendation entries and source records stable and testable.
- [ ] Add no-perfect-match framing so Beacon can recommend the best available tradeoff-aware option without implying exact fit.
- [ ] Preserve top-three comparison output even when evidence quality is uneven or none of the options is ideal.
- [ ] Keep clickthrough and tradeoff framing anchored to canonical recommendation and candidate state.
- [ ] Handle missing/stale source links gracefully instead of breaking recommendation usability.
- [ ] Ensure the final recommendation route remains coherent when the shortlist has mixed evidence quality.
- [ ] Keep this renter-facing output actionable and understandable without leaking operator-only diagnostics.

## Dev Notes

- Depends on Story `4.3`, which added evidence strength, uncertainty, and workflow credibility framing to the recommendation experience.
- This story is the final renter-actionability layer for Epic 4: Beacon should help users move from comparison to next step, even under imperfect conditions.
- Previous story intelligence: preserve the trust/uncertainty semantics from `4.3` so clickthrough and no-perfect-match messaging stay honest.
- “No perfect match” is not a failure mode; it is a core product behavior that should remain useful and explicit.

### Architecture Guardrails

- Each shortlisted property must include linkage to its original source listing.
- Recommendation outputs must remain usable when evidence is partial or uneven; graceful degradation is a core requirement.
- Renter-facing recommendation output should remain grounded in canonical Convex state and source linkage rather than ad hoc client wiring.
- Keep route separation and recommendation-domain UI boundaries intact under `src/app/recommendation` and `src/features/recommendation`.
- Avoid collapsing the final recommendation surface simply because some data is imperfect.
- User-facing messages should be tradeoff-aware and honest rather than implying precision Beacon does not have.

### File Structure Notes

- `src/app/recommendation/[sessionId]/page.tsx`
- `src/features/recommendation/components/`
- `src/features/recommendation/lib/`
- `convex/recommendationArtifacts/queries.ts`
- `convex/unitRecords/queries.ts`
- `src/types/recommendation.ts`
- `tests/e2e/recommendation-flow.spec.ts`

### Testing Requirements

- Verify each recommendation entry links to the correct original source listing.
- Verify no-perfect-match scenarios still render a best option and the full top-three comparison.
- Verify uneven evidence quality does not collapse the recommendation route into failure UI.
- Verify stale or missing source-link states degrade gracefully without breaking the page.
- Run lint/typecheck after clickthrough and final recommendation handling updates.

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
