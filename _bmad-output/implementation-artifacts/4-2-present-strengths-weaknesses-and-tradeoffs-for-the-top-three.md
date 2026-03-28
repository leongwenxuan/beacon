# Story 4.2: Present Strengths, Weaknesses, and Tradeoffs for the Top Three

Status: ready-for-dev

## Story

As a renter,
I want Beacon to explain why each shortlisted property is strong or weak,
So that I can compare tradeoffs instead of only seeing opaque scores.

## Acceptance Criteria

1. **Given** a recommendation artifact exists **When** the renter opens the recommendation experience **Then** Beacon presents the top three shortlisted properties in a comparison-ready format **And** each property includes strengths, weaknesses, and key tradeoff notes.

2. **Given** multiple shortlisted properties satisfy different renter priorities **When** Beacon renders the comparison **Then** the UI makes the main tradeoffs legible across the top three **And** keeps the explanation anchored to persisted candidate and recommendation data.

3. **Given** the best recommendation outranks the others **When** the renter views the final output **Then** Beacon explains why that option was selected over the alternatives **And** distinguishes recommendation reasoning from raw source facts.

## Tasks / Subtasks

- [ ] Build the renter-facing recommendation route on top of the canonical `recommendationArtifacts` state produced in Story `4.1`.
- [ ] Render the ranked top three in a comparison-ready layout optimized for desktop-first review.
- [ ] Present persisted strengths, weaknesses, and tradeoff notes for each shortlisted property.
- [ ] Add explicit why-selected reasoning for the best recommendation that is clearly framed as Beacon synthesis.
- [ ] Keep comparison output anchored to persisted candidate and recommendation artifact data instead of recomputing reasoning in the client.
- [ ] Handle missing or partial explanation fields gracefully without breaking the recommendation route.
- [ ] Preserve legibility, hierarchy, and semantic structure for a comparison-heavy UI.
- [ ] Keep this story scoped to presentation of the top three; uncertainty/trust framing deepens further in Story `4.3`.

## Dev Notes

- Depends on Story `4.1`, which introduced canonical recommendation artifact persistence.
- This story is the first full renter-facing recommendation surface and should make the top-three comparison feel materially more useful than raw portal browsing.
- Previous story intelligence: use the persisted recommendation artifact as the source of truth for comparison content and avoid duplicating recommendation reasoning in client code.
- Tradeoff legibility matters more than decorative polish; the UI should help a renter decide, not just admire a ranking.

### Architecture Guardrails

- The recommendation experience should read canonical Convex state rather than duplicating recommendation truth in frontend stores.
- Keep recommendation and comparison UI under the documented route/feature boundaries for `src/app/recommendation` and `src/features/recommendation`.
- Recommendation reasoning must stay distinct from raw retrieved listing facts.
- Use accessible structure, readable hierarchy, and strong practical accessibility for core recommendation flows.
- Desktop-first presentation is the primary optimization target, but the output should remain coherent on smaller screens.
- Shared UI primitives belong in `src/components`; recommendation-domain UI belongs in `src/features/recommendation`.

### File Structure Notes

- `src/app/recommendation/[sessionId]/page.tsx`
- `src/app/recommendation/[sessionId]/loading.tsx`
- `src/app/recommendation/[sessionId]/error.tsx`
- `src/features/recommendation/components/`
- `src/features/recommendation/hooks/`
- `convex/recommendationArtifacts/queries.ts`
- `src/types/recommendation.ts`
- `src/components/`

### Testing Requirements

- Verify the recommendation route renders the persisted top three in the correct ranked order.
- Verify each candidate displays strengths, weaknesses, and tradeoff notes from canonical recommendation data.
- Verify the why-selected explanation is present for the best recommendation and is distinct from raw source facts.
- Verify partial/missing explanation fields degrade gracefully without crashing the route.
- Run lint/typecheck after recommendation-route and component updates.

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
