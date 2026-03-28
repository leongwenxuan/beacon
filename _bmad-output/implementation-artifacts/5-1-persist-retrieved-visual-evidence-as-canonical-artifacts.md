# Story 5.1: Persist Retrieved Visual Evidence as Canonical Artifacts

Status: ready-for-dev

## Story

As a renter,
I want Beacon to preserve listing and layout imagery as durable session-linked artifacts,
So that visual evidence remains available for recommendation and downstream generation.

## Acceptance Criteria

1. **Given** discovery or enrichment retrieves listing images, floor plans, or building visuals **When** Beacon ingests those files or URLs **Then** it fetches and stores the actual image bytes in Convex file storage **And** writes canonical `imageArtifacts` metadata records linked to the session and related candidate where applicable.

2. **Given** an image artifact is persisted **When** metadata is stored **Then** the record includes provenance, provider information, and `storageId`-based linkage **And** provider URLs are treated as transient inputs rather than durable application truth.

3. **Given** visual artifacts are stored **When** later workflows need them **Then** they are retrievable from canonical Convex-backed records **And** the application does not depend on unstable third-party image links remaining live.

## Tasks / Subtasks

- [ ] Define canonical `imageArtifacts` persistence for retrieved visual evidence, including `sessionId`, optional `unitId`, `kind`, `provider`, `originalUrl`, provenance, and `storageId`.
- [ ] Fetch/store retrieved image bytes into Convex file storage rather than treating provider URLs as durable truth.
- [ ] Persist canonical metadata rows for listing images, floor plans, building visuals, and other retrieved visual evidence.
- [ ] Keep retrieved-image ingestion behind server-side boundaries so provider URLs or file fetching are not handled in the client.
- [ ] Link retrieved image artifacts back to the session and, when applicable, the related candidate/unit record.
- [ ] Preserve provenance and provider-origin metadata for later recommendation, visual-generation, and operator-inspection workflows.
- [ ] Expose canonical image-artifact reads that resolve from `storageId` and metadata instead of unstable third-party URLs.
- [ ] Keep artifact ingestion compatible with later inferred-visual generation and operator inspection stories.

## Dev Notes

- Depends on Story `4.4`, which finalized the actionable recommendation output and source-linkage behavior.
- This story creates the durable visual-evidence storage layer required before inferred-visual generation can be trustworthy.
- Previous story intelligence: keep artifact linkage aligned with the same session/candidate/recommendation relationships already established in earlier epics.
- Retrieved images should become durable product artifacts, not temporary provider attachments that can disappear later.

### Architecture Guardrails

- Retrieved and generated images must be fetched and stored into Convex file storage, with metadata and provenance linked through canonical artifact records using `storageId`.
- `imageArtifacts` are the canonical metadata table for retrieved and generated visual assets.
- Provider URLs are transient inputs and must not be treated as the durable source of truth.
- Convex owns both the durable file pointer and the business metadata for image artifacts.
- Keep image ingestion and provider communication server-side only.
- Recommendation, visual-generation, and operator flows should all consume canonical artifact state from Convex.

### File Structure Notes

- `convex/imageArtifacts/mutations.ts`
- `convex/imageArtifacts/queries.ts`
- `convex/imageArtifacts/helpers.ts`
- `convex/lib/storage.ts`
- `convex/schema.ts`
- `convex/integrations/tinyfish/mappers.ts`
- `convex/orchestrator/enrichment.ts`
- `src/types/visuals.ts`

### Testing Requirements

- Verify retrieved image bytes are stored in Convex file storage and linked through canonical `imageArtifacts` rows.
- Verify artifact metadata includes provenance, provider info, and `storageId` linkage.
- Verify later reads resolve via canonical artifact state instead of raw provider URLs.
- Verify failed or invalid image retrievals do not create bogus durable artifact records.
- Run lint/typecheck after artifact-schema and storage-helper updates.

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
