---
stepsCompleted:
  - step-01-init
  - step-02-discovery
  - step-02b-vision
  - step-02c-executive-summary
  - step-03-success
  - step-04-journeys
  - step-05-domain
  - step-06-innovation
  - step-07-project-type
  - step-08-scoping
  - step-09-functional
  - step-10-nonfunctional
  - step-11-polish
inputDocuments:
  - /Users/leongwenxuan/Desktop/beaconmain/idea.md
  - /Users/leongwenxuan/Desktop/beaconmain/beacon_orchestrator_technical_plan.md
documentCounts:
  productBriefs: 1
  research: 0
  brainstorming: 0
  projectDocs: 1
workflowType: 'prd'
classification:
  projectType: web_app
  domain: proptech
  complexity: high
  projectContext: brownfield
---

# Product Requirements Document - beaconmain

**Author:** Leongwenxuan
**Date:** 2026-03-27

## Executive Summary

Beacon is a proptech web application that helps renters make better housing decisions by turning messy and incomplete rental information into one clear recommendation. Instead of making users manually search listings, cross-check building details, fill in missing facts, and compare tradeoffs themselves, Beacon gathers evidence across listing sources and the wider internet, structures that information, and ranks the strongest options.

The product is designed around a decision-support workflow rather than a listing-browsing experience. A renter provides a natural-language housing brief, Beacon searches broadly for relevant properties, enriches promising candidates with additional property and building context, and returns a recommendation surface that explains what is good, what is weak, and how the top options compare. The intended outcome is that a user can understand the best-fit properties faster and with higher confidence than they could through manual browsing.

Beacon is optimized to rank and differentiate the top three options while still surfacing as much relevant evidence as possible. Its core product value is not just discovery of listings, but completion and synthesis of incomplete housing information into a form that is immediately useful for decision-making.

### What Makes This Special

Beacon is not constrained to the information contained on a single listing page. Its key differentiator is evidence completion: if important context such as building details, layouts, or supporting property information is missing from the source listing, Beacon can retrieve that information from elsewhere on the internet and incorporate it into the recommendation.

This makes the product meaningfully different from both traditional rental portals and simple listing aggregators. Traditional portals expose fragmented, source-bounded information. Beacon instead acts as a research and synthesis layer across fragmented housing data. The user does not need to worry as much that better options or important facts are hidden elsewhere, because the system’s job is to search, reconcile, enrich, and compare on the user’s behalf.

The core insight behind the product is that rental search pain comes less from lack of supply and more from the effort required to assemble incomplete information into a trustworthy judgment. Beacon creates value by reducing that synthesis burden and converting scattered evidence into ranked, comparison-ready recommendations.

## Project Classification

This classification sets the context for the scope, requirements, and implementation decisions that follow.

- **Project Type:** Web application
- **Domain:** Proptech
- **Complexity:** High
- **Project Context:** Brownfield

## Success Criteria

### User Success

A successful user session ends with the renter receiving a clearly ranked top three shortlist that is meaningfully easier to evaluate than raw portal browsing. The user should be able to understand why each shortlisted property was selected, what its main strengths and weaknesses are, and how the options differ from one another without needing to manually stitch information across tabs and sources.

The product should save the user time, reduce uncertainty, and increase confidence in the decision process. Success is strongest when the user clicks through to at least one shortlisted property because the recommendation feels credible and worth pursuing. A further success condition is that the user can form a visual sense of the recommended property rather than relying only on sparse listing photos or text.

### Business Success

At the current stage, business success means demo success rather than commercial performance. The product is successful if it can repeatedly demonstrate an end-to-end flow that feels credible, differentiated, and more useful than manual rental browsing.

In the near term, success means the system can convincingly show that Beacon is doing real work: searching broadly, enriching incomplete property data, ranking top candidates, and producing both a recommendation and an inferred visual output that make the experience feel substantially different from a portal or static mockup.

### Technical Success

Technical success requires the orchestrator to work reliably end to end across discovery, enrichment, ranking, synthesis, and visual generation. The system must persist useful intermediate and final results safely in the database when appropriate so that evidence, candidate state, generated artifacts, and recommendation outputs are not lost across the flow.

The system must also be responsive enough to preserve trust during the search experience. Beacon should produce the first useful shortlist signal within 60 seconds and a final recommendation within 5 minutes under normal demo conditions. It must degrade gracefully when some external sources fail or when evidence is incomplete, rather than collapsing the session. Caching and persistence should reduce repeated work where safe, especially for retrieved evidence and generated artifacts.

### Measurable Outcomes

- Users receive a ranked top 3 shortlist for a realistic rental brief.
- Each shortlisted property includes structured strengths, weaknesses, and comparison-ready evidence.
- At least one shortlisted property is compelling enough for the user to click through to the source listing.
- The system produces a final recommendation within 5 minutes under normal conditions.
- The system surfaces a first useful shortlist/progress signal within 60 seconds.
- The system successfully enriches all 3 shortlisted properties with additional evidence beyond the source listing where available.
- The system generates an inferred visual output for the recommended property as part of the MVP flow.
- Partial source failures do not prevent Beacon from returning a usable recommendation.

## Product Scope

### MVP - Minimum Viable Product

The MVP must support natural-language rental query input, broad multi-source property discovery, ranking of the top 3 candidates, enrichment of all 3 shortlisted properties, and a recommendation surface that explains why each option is strong or weak. It must also support clickthrough to the original source listing.

The MVP also requires visual generation. Beacon must infer and render a plausible visual sense of the recommended property based on available evidence such as listing images, layout information, floor plans, and related building context, even when the source listing is incomplete. Retrieved and generated images must be persisted as durable artifacts with provenance and session linkage rather than treated as transient external URLs. This is not optional for MVP because it is part of the core product proof, not just polish.

### Growth Features (Post-MVP)

Post-MVP growth should focus on improving ranking quality, increasing source coverage, deepening building-level evidence retrieval, improving confidence scoring, and making recommendation outputs more personalized. It is also the right stage for stronger caching strategies, better artifact reuse, smarter completion policies, and more refined explanation layers for tradeoffs and confidence.

Additional growth areas include better comparison UX, richer live search progress experiences, and broader support for more rental portals and external evidence sources.

### Vision (Future)

The long-term vision is a rental advisor that can search widely, complete missing housing evidence from across the internet, reason over tradeoffs with high confidence, and generate a rich, trustworthy representation of what living in a property may actually feel like. Over time, Beacon should evolve from a recommendation engine into a highly credible decision-support system that reduces the renter’s research burden to a single query and a final choice.

## User Journeys

### Journey 1: Primary User - Happy Path

A renter has a clear housing brief but is tired of doing manual portal research. They want a home near a target MRT area, within budget, and with enough surrounding context to know whether the place is worth pursuing. Their current pain is not a lack of listings, but the time and uncertainty involved in comparing fragmented and incomplete information.

They arrive in Beacon and describe what they want in natural language. Instead of forcing them through a rigid filter-first workflow, Beacon interprets the brief and begins scouting broadly across rental sources. During the live search phase, the user sees that the system is actively discovering listings, gathering supporting evidence, and narrowing toward stronger candidates. This builds confidence that the product is doing real work rather than returning a static mock result.

The turning point comes when Beacon presents a ranked top three shortlist with structured reasoning. Each property is accompanied by clear strengths, weaknesses, tradeoffs, and supporting context. The best option is not just listed, but explained. The renter can immediately see why one property ranks above another and what compromises are involved.

The journey resolves when the renter clicks through on one shortlisted property because Beacon has already done enough synthesis for them to feel confident that this option is worth pursuing. The user leaves feeling that Beacon compressed hours of scattered research into one clear decision surface.

**Capabilities revealed by this journey:**
- Natural-language query input
- Multi-source discovery
- Evidence enrichment
- Top-3 ranking
- Structured pros/cons and tradeoffs
- Clickthrough to original source listing

### Journey 2: Primary User - Edge Case / Incomplete Evidence Path

A renter enters a strong brief, but the available listings are messy. One candidate has strong photos but no layout. Another has a floor plan but incomplete neighborhood context. A third looks promising but contains conflicting facts across sources. This is the exact situation where manual browsing becomes frustrating.

Beacon still performs broad discovery and shortlisting, but instead of stopping at incomplete listing data, it attempts evidence completion. It searches beyond the original listing page to retrieve building details, layout clues, and supporting context from elsewhere on the internet. When facts conflict, Beacon does not silently flatten them into false certainty. It carries uncertainty forward through confidence-aware ranking and explicit tradeoff notes.

The emotional low point in this journey is when the user realizes there is no perfect listing. The product wins by handling that honestly. Rather than pretending certainty, Beacon explains that the best options are partial or tradeoff-heavy and makes those tradeoffs legible. If visual evidence is limited, Beacon still generates an inferred visual sense of the recommended property, but frames it as a plausible synthesis grounded in partial evidence rather than a guaranteed reconstruction.

The journey resolves when the renter still receives a usable top three shortlist and one recommendation that is clear enough to act on, even though the source data was incomplete. Success here is graceful degradation: Beacon reduces confusion and preserves usefulness under imperfect conditions.

**Capabilities revealed by this journey:**
- Recursive evidence completion
- Cross-source reconciliation
- Confidence-aware ranking
- Honest uncertainty communication
- Graceful degradation under weak evidence
- Inferred visual generation from partial inputs

### Journey 3: Demo Viewer / Judge Path

A hackathon judge, technical reviewer, or early demo viewer is not only evaluating whether the product looks polished. They are also judging whether Beacon's orchestration is real and whether the product is meaningfully better than a portal clone.

They encounter Beacon through a guided demo flow. The important thing for this user is that the product visibly communicates the stages of work: intake of a natural-language brief, broad discovery, evidence gathering, narrowing, recommendation synthesis, and visual generation. They need to believe the output is the result of a system, not a hand-authored screen.

The critical moment in this journey is when the viewer sees the connection between backend intelligence and frontend output. The shortlist, recommendation rationale, tradeoffs, and inferred visuals all need to feel causally tied to the orchestrated evidence-gathering process. If the product only shows a polished final screen without process credibility, the journey fails.

The journey resolves when the viewer concludes that Beacon is both technically credible and product-differentiated. They should leave with the impression that Beacon is not just aggregating rental listings, but performing real discovery, evidence completion, and synthesis to produce a stronger rental decision experience.

**Capabilities revealed by this journey:**
- Live progress communication
- Clear linkage between orchestration and user-visible output
- Recommendation explainability
- Differentiated visual presentation
- Demo-ready reliability and end-to-end flow integrity

### Journey 4: Operator / Troubleshooting Path

The operator in the MVP is the founder or developer monitoring whether Beacon is working correctly in real runs and demos. Their pain is not rental search itself, but the complexity of a multi-stage orchestration pipeline that can fail at discovery, enrichment, ranking, persistence, or visual generation.

They need visibility into session state, candidate progression, cache hits, stored evidence, generated artifacts, and failure points. When a run behaves unexpectedly, they need to quickly understand whether the issue came from source retrieval, inconsistent data, scoring logic, or visual synthesis. They also need confidence that useful intermediate results are safely persisted in the database when appropriate, so a partial failure does not erase everything.

The critical moment in this journey happens when something goes wrong mid-run. Instead of a black box, the operator needs enough observability to diagnose what failed, what was recovered, what was cached, and whether the user-facing output can still be delivered. This determines whether Beacon can be trusted in demos and later in real use.

The journey resolves when the operator can inspect a run, understand its state, recover from issues, and confirm that the system still produced a usable result or degraded gracefully. For this user, success is operational clarity, not visual polish.

**Capabilities revealed by this journey:**
- Session/run observability
- Candidate and artifact state inspection
- Safe persistence and caching
- Failure diagnosis and recovery support
- Graceful degradation and partial completion handling

### Journey Requirements Summary

These journeys imply that Beacon needs capabilities in five major areas.

First, it needs a strong user-facing recommendation flow: natural-language intake, ranking, shortlist presentation, tradeoff explanation, and clear clickthrough to source listings.

Second, it needs a real evidence completion engine: multi-source search, cross-source enrichment, reconciliation of conflicting facts, and explicit handling of incomplete information.

Third, it needs visual synthesis capabilities as part of the core MVP: inferred image generation based on layout, listing imagery, and supporting evidence, with appropriate qualification when confidence is limited.

Fourth, it needs a credible live orchestration surface that makes search progress and evidence gathering visible enough for users and demo viewers to trust the process.

Fifth, it needs operator tooling and persistence infrastructure: session state tracking, artifact storage, caching, observability, and failure handling so the orchestrator can be monitored and trusted in practice.

## Domain-Specific Requirements

### Compliance & Regulatory

- No heavy industry-specific compliance regime is treated as a core MVP driver for Beacon.
- The product should focus on trust, transparency, and clear distinction between sourced facts and inferred outputs rather than formal regulatory workflows.

### Technical Constraints

- Rental source data is often incomplete, inconsistent, and uneven in quality, so the system must be designed around partial evidence rather than assuming complete listings.
- The product must distinguish clearly between retrieved facts, inferred facts, and unknowns.
- Generated visual outputs must be explicitly presented as inferred previews rather than exact reconstructions.
- Caching and persistence must avoid unsafe reuse of stale evidence that would degrade recommendation quality.
- The orchestrator must preserve usefulness even when one or more evidence retrieval paths fail.

### Integration Requirements

- Integration with rental listing sources for candidate discovery and listing detail retrieval.
- Integration with broader web retrieval workflows for building, layout, and contextual evidence beyond the original listing page.
- Integration with a persistent database and state layer for sessions, candidates, evidence, artifacts, and recommendation outputs.
- Integration with durable file storage for retrieved listing images, floor plans, and generated visual outputs, with canonical metadata and provenance linked back to session and unit records.
- Integration with a visual synthesis pipeline for inferred image generation.

### Risk Mitigations

- Track provenance for important fields so recommendation outputs can be grounded in identifiable evidence sources.
- Maintain explicit state for retrieved, inferred, and unknown information to prevent false certainty.
- Use confidence-aware ranking and recommendation language when evidence is incomplete or conflicting.
- Add freshness controls for cached evidence so outdated source data does not silently distort results.
- Degrade gracefully when source retrieval fails, evidence is sparse, or visual generation confidence is weak.

## Innovation & Novel Patterns

### Detected Innovation Areas

Beacon combines three capabilities that are usually separate in rental search products: multi-source discovery, recursive evidence completion, and inferred visual synthesis. The product is not simply showing listings better. It is attempting to transform incomplete rental data into a recommendation-ready artifact by actively searching for missing context and synthesizing it into a ranked decision output.

The first innovation area is evidence completion as a product behavior. Traditional rental portals expose whatever information the listing page already contains. Beacon treats missing information as a solvable problem. If layout, building details, or contextual signals are absent from the original listing, the system attempts to retrieve supporting evidence from other sources and incorporate that into the ranking and presentation flow.

The second innovation area is orchestrated rental decision support. Instead of a user manually browsing, comparing, and interpreting fragmented information, Beacon uses an orchestrated agent workflow to search broadly, shortlist intelligently, fill gaps, and generate a final recommendation. This shifts the product interaction from browsing to delegating research.

The third innovation area is visual inference as part of recommendation quality. Beacon does not treat generated visuals as standalone novelty. It uses partial evidence such as listing images, floor plans, and layout clues to infer what the property may feel like, making the recommendation more legible and useful even when source materials are incomplete.

### Market Context & Competitive Landscape

Most existing rental experiences optimize for search and browsing, not evidence synthesis. Listing portals help users filter inventory, but they generally leave comparison, cross-source verification, missing-information recovery, and tradeoff analysis to the renter. Some AI search experiences may summarize listings, but fewer make cross-source evidence completion and inferred visual understanding core parts of the product behavior.

Beacon is differentiated if it can credibly show that it does more than aggregate or summarize. Its competitive edge comes from acting like an active rental research layer: gathering, reconciling, enriching, ranking, and visualizing, rather than just presenting raw inventory more attractively.

### Validation Approach

The innovation should be validated at the product-behavior level, not just by technical novelty claims.

- Validate whether users perceive the top 3 shortlist as materially more decision-useful than manual portal browsing.
- Validate whether evidence completion improves trust and usefulness when listings are incomplete.
- Validate whether the inferred visual output helps users understand a property better rather than feeling decorative or misleading.
- Validate whether demo viewers can clearly see that Beacon is doing real orchestration work instead of producing a static output.

A successful validation outcome is that users feel Beacon meaningfully reduces research effort and increases confidence, while demo viewers perceive the system as a new interaction pattern rather than a portal clone with AI summaries.

### Risk Mitigation

The main innovation risk is overclaiming novelty when the user experience still feels like a dressed-up listing portal. To reduce this risk, the product must make the evidence completion and recommendation logic clearly visible in the final experience.

A second risk is that inferred visuals may feel gimmicky or untrustworthy. This should be mitigated by grounding image generation in retrieved evidence, labeling outputs as inferred, and ensuring the recommendation remains valuable even when visual confidence is weaker than desired.

A third risk is that the orchestration complexity becomes impressive technically but unclear in product value. The mitigation is to keep the product centered on a simple user promise: Beacon reduces rental research burden by returning a clear, evidence-backed recommendation with ranked alternatives.

## Web App Specific Requirements

### Project-Type Overview

Beacon should be implemented as a single-page web application optimized for an app-like decision-support experience rather than a document-style or SEO-driven website. The product's core interaction is dynamic, stateful, and session-based: the user submits a rental brief, watches search and enrichment progress, and receives a ranked recommendation output with supporting evidence and inferred visuals.

The primary experience is desktop-first because rental comparison, map context, side-by-side reasoning, and visual review are easier to present clearly on larger screens. Mobile web should remain usable, but it is a secondary experience for the MVP rather than the primary product surface.

### Technical Architecture Considerations

Because Beacon's value depends on live orchestration visibility, the frontend should support progressive updates to session state, candidate ranking, and evidence enrichment as the backend works. Real-time or near-real-time session updates are therefore a core product-type requirement, not a cosmetic enhancement.

The application should be designed around persistent session state and reactive UI updates. The frontend must handle partial results, evolving candidate quality, generated artifacts, and degraded states without forcing full page reloads. It should also distinguish clearly between retrieved evidence, inferred outputs, and unknown information so that the interface does not imply false certainty.

Since SEO is not a core requirement, the frontend architecture can prioritize responsiveness, interactivity, and session continuity over search-indexable page rendering. That makes a SPA-oriented architecture the best fit for the product behavior we've defined.

### Browser Matrix

The MVP should target modern desktop browsers first, especially current Chrome and Safari, with reasonable support for current Edge. Mobile web should be functional enough to review recommendations, but the primary workflow should be optimized for desktop interaction and larger-screen comparison.

### Responsive Design

The layout should be responsive, but not all desktop behaviors need to collapse perfectly into a mobile-first pattern. The recommendation artifact, ranked shortlist, live progress surface, and visual outputs should remain legible and useful on smaller screens, while desktop remains the primary optimization target.

### Performance Targets

The frontend should surface the first meaningful progress or shortlist signal within 60 seconds and remain responsive throughout the session. The interface should avoid blocking states during long-running orchestration steps and should reveal system progress incrementally so users understand that work is ongoing.

### Accessibility Level

Beacon should meet a strong practical accessibility baseline for MVP. Core flows should support keyboard navigation, semantic structure, readable contrast, accessible labeling, and understandable status messaging. Full formal audit depth is not required for MVP, but avoid shipping a UI that is only visually polished and operationally inaccessible.

### Implementation Considerations

The UI should be built around two connected surfaces: a live scouting/progress experience and a final recommendation experience. Both surfaces must share session state and present continuity between "Beacon is working" and "Beacon has concluded." This is important because the product's credibility depends on the user perceiving the recommendation as the output of a real search and synthesis process.

The frontend should also support confidence-aware presentation. When evidence is weak, conflicting, or inferred, the interface should communicate that cleanly rather than flattening all outputs into equally certain facts. This is especially important for generated visual outputs, which must be helpful without overstating precision.

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach:** problem-solving MVP with demo-grade polish on the core loop.
**Resource Requirements:** minimum 1 strong full-stack or AI engineer can prototype it, but a more realistic MVP team is 1 full-stack product engineer, 1 orchestration or backend engineer, and optionally 1 design or frontend-heavy contributor for the recommendation experience.

The MVP should prove one thing clearly: a renter can give one brief and get a top-3, evidence-backed, comparison-ready recommendation with an inferred visual sense of the best property.

### MVP Feature Set (Phase 1)

**Core User Journeys Supported:**
- Primary renter happy path
- Primary renter incomplete-evidence path
- Demo viewer or judge path
- Lightweight operator or troubleshooting path

**Must-Have Capabilities:**
- Natural-language rental brief input
- Broad multi-source discovery
- Top-3 shortlist generation
- Enrichment of all shortlisted candidates
- Structured strengths, weaknesses, and tradeoffs
- Clear best recommendation with clickthrough to source listing
- Live or near-real-time progress updates
- Persistence of session, candidate, evidence, and artifact state
- Confidence-aware handling of incomplete or conflicting data
- Inferred visual generation for the recommended property
- Graceful degradation when some sources or enrichment paths fail

### Post-MVP Features

**Phase 2 (Post-MVP):**
- Better comparison UX and richer recommendation presentation
- Stronger confidence scoring and provenance display
- Broader source coverage
- Deeper building- and neighborhood-level retrieval
- Improved caching and reuse of evidence and artifacts
- More refined visual generation quality
- Better operator tooling and observability

**Phase 3 (Expansion):**
- Personalized ranking and memory of renter preferences
- Saved searches, alerts, and repeat session workflows
- Broader market coverage and more property categories
- Stronger building intelligence and richer layout understanding
- More advanced "what living here feels like" synthesis
- Platformization beyond a single demo-grade flow

### Risk Mitigation Strategy

**Technical Risks:** The riskiest part is getting the full chain to work together reliably across discovery, enrichment, ranking, persistence, and visual inference. The mitigation is to optimize for one strong end-to-end path first, keep source count limited, and prefer reliability over breadth in MVP.

**Market Risks:** The biggest market risk is that Beacon feels like a portal wrapper instead of a genuinely better decision product. The mitigation is to make recommendation quality, evidence completion, and top-3 differentiation obviously better than manual browsing.

**Resource Risks:** The main resource risk is trying to build too much breadth too early. The mitigation is to keep MVP constrained to a desktop-first web app, a limited source set, a strong top-3 flow, and one credible visual generation path.

## Functional Requirements

### Query Intake & Search Setup

- FR1: Renters can describe their rental needs in natural language.
- FR2: Renters can submit a rental brief that includes location, budget, property preferences, and lifestyle priorities.
- FR3: The system can interpret a renter's brief into a structured search intent.
- FR4: The system can create a search session for each submitted rental brief.
- FR5: The system can preserve the state of an in-progress search session.
- FR6: Renters can revisit the current state of an active search session.

### Property Discovery & Candidate Selection

- FR7: The system can discover candidate rental properties from multiple sources.
- FR8: The system can consolidate discovered candidates into a shared shortlist for a search session.
- FR9: The system can identify and remove duplicate or overlapping candidate properties.
- FR10: The system can narrow discovered candidates into a ranked top-three shortlist.
- FR11: The system can update the shortlist as better evidence becomes available.
- FR12: The system can associate each shortlisted property with its original source listing.

### Evidence Enrichment & Completion

- FR13: The system can collect property details beyond the original listing page.
- FR14: The system can gather building-level and neighborhood-level context for shortlisted properties.
- FR15: The system can retrieve supporting evidence from sources beyond the original listing page when important information is missing.
- FR16: The system can identify missing information for shortlisted properties.
- FR17: The system can continue enriching shortlisted properties until they are sufficiently complete for recommendation.
- FR18: The system can reconcile conflicting information gathered from different sources.
- FR19: The system can retain provenance for important evidence used in recommendations.
- FR20: The system can distinguish between retrieved facts, inferred outputs, and unknown information.

### Recommendation & Comparison

- FR21: The system can rank shortlisted properties against the renter's stated priorities.
- FR22: The system can identify a best overall recommendation from the shortlisted properties.
- FR23: The system can present the top three shortlisted properties in a comparison-ready format.
- FR24: The system can explain why each shortlisted property was selected.
- FR25: The system can identify strengths and weaknesses for each shortlisted property.
- FR26: The system can present tradeoffs across shortlisted properties.
- FR27: The system can communicate uncertainty or incomplete evidence within recommendation outputs.
- FR28: Renters can access the original source listing for a shortlisted property.
- FR29: The system can return a usable recommendation even when no perfect match exists.

### Visual Understanding & Inferred Property Representation

- FR30: The system can collect visual evidence for shortlisted properties and persist it as session-linked artifacts.
- FR31: The system can use available visual and layout evidence to infer a plausible representation of a recommended property.
- FR32: The system can present an inferred visual output as part of the recommendation experience.
- FR33: The system can indicate that generated visuals are inferred rather than exact reconstructions.
- FR34: The system can produce a recommendation even when visual evidence is weak or incomplete.
- FR35: The system can associate inferred visual outputs with the property evidence, provenance, and persisted artifact records used to generate them.

### Live Search Experience & Session Feedback

- FR36: Renters can see that Beacon is actively working on their search session.
- FR37: Renters can view progress as properties are discovered, enriched, and narrowed.
- FR38: Renters can see evolving candidate quality during an in-progress session.
- FR39: The system can surface partial results before the final recommendation is complete.
- FR40: The system can preserve continuity between live search progress and the final recommendation experience.

### Persistence, Operations & Recovery

- FR41: The system can persist candidate, evidence, recommendation, and artifact state for a session.
- FR42: The system can reuse previously gathered evidence when it is safe and appropriate to do so.
- FR43: Operators can inspect the state of a search session and its shortlisted candidates.
- FR44: Operators can inspect generated artifacts and stored evidence associated with a session.
- FR45: Operators can identify when a session has partially failed or degraded.
- FR46: The system can continue delivering a usable output when one or more evidence retrieval paths fail.
- FR47: The system can preserve partial progress when a full search workflow does not complete successfully.

### Trust, Transparency & Product Credibility

- FR48: Renters can understand whether a recommendation is based on strong, partial, or uncertain evidence.
- FR49: Demo viewers can observe that the recommendation is the result of a real multi-stage workflow rather than a static output.
- FR50: The system can present recommendation outputs in a way that reflects how evidence was gathered, enriched, and synthesized.

## Non-Functional Requirements

### Performance

- NFR1: The system shall surface an initial progress signal or early shortlist indication within 60 seconds for a typical rental query under normal operating conditions.
- NFR2: The system shall produce a final recommendation within 5 minutes for a typical rental query under normal demo conditions.
- NFR3: The user interface shall remain responsive while long-running discovery, enrichment, and visual synthesis tasks are in progress.
- NFR4: The system shall support progressive result updates so users can receive partial value before a session is fully complete.

### Security

- NFR5: The system shall protect stored session data, candidate data, evidence records, and generated artifacts from unauthorized access.
- NFR6: The system shall restrict operational and troubleshooting visibility to authorized operators only.
- NFR7: The system shall preserve the integrity of provenance, confidence, and recommendation data so that outputs cannot be silently corrupted or misrepresented.
- NFR8: The system shall protect data in transit and at rest using industry-standard security practices appropriate for a web application MVP.

### Scalability

- NFR9: The system shall support multiple concurrent search sessions without causing unacceptable degradation to the core recommendation flow.
- NFR10: The system shall degrade gracefully when demand exceeds ideal processing capacity, prioritizing usable partial results over total failure.
- NFR11: The system shall support incremental expansion of source coverage, session volume, and artifact storage without requiring a full product redesign.

### Accessibility

- NFR12: Core user flows shall support keyboard-accessible interaction.
- NFR13: The interface shall use semantic structure, accessible labels, and status messaging sufficient for assistive technology support in core flows.
- NFR14: Recommendation, progress, and comparison content shall remain readable through adequate contrast, hierarchy, and text clarity.
- NFR15: Accessibility support shall apply to both the live search experience and the final recommendation experience.

### Integration

- NFR16: The system shall tolerate partial unreliability from external listing and evidence sources without collapsing the full session.
- NFR17: The system shall maintain clear separation between external source data, inferred data, and internally synthesized recommendation outputs.
- NFR18: The system shall support persistence and retrieval of session, candidate, evidence, and visual artifact data, including durable storage references for retrieved and generated images, across the end-to-end workflow.
- NFR19: The system shall detect and handle stale or incomplete external evidence in a way that reduces the risk of distorted recommendations.
