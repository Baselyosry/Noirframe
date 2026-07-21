# Review V2 — Challenge of Architecture Review Recommendations

## Scope Correction

`Agents/REVIEW.md` contains agent instructions, not architecture recommendations. This review therefore challenges the recommendations in the root `REVIEW.md`, which is the report containing findings F-001 through F-015.

## Recommendation Assessment

### F-001 — Remove V1 freeze until a full dogfood run

**Verdict:** Keep, but simplify.  
**Necessary:** Yes. A one-document demonstration cannot prove an eight-document bootstrap pipeline.  
**Overengineering risk:** Requiring many unrelated dogfood projects before V1 would be excessive.  
**Simpler alternative:** One complete unrelated project plus one controlled requirement change is sufficient for V1 evidence.  
**20-project relevance:** Yes; it establishes an evidence-first release practice.  
**Cognitive load:** Low increase, offset by avoiding premature public commitments.  
**Senior-engineer view:** Appreciated. Freezing a framework without an end-to-end trial is not credible.  
**Improved recommendation:** Replace the freeze claim with “candidate V1” until one full end-to-end dogfood run and one regeneration scenario pass.

### F-002 — Implement mechanical validation

**Verdict:** Keep, but narrow sharply.  
**Necessary:** Partially. Placeholder, ID, metadata, and link checks need repeatability; contradiction detection does not belong in a V1 validator.  
**Overengineering risk:** A generic Markdown parser, semantic inference engine, or “canonical ownership detector” is premature.  
**Simpler alternative:** Define a short manual checklist now and automate only four deterministic checks: unresolved placeholders, ID format/duplicates, referenced-ID existence, and required file presence.  
**20-project relevance:** Yes; these four checks become more valuable as project count grows.  
**Cognitive load:** Lower than the current ambiguous “mechanical validation” promise.  
**Senior-engineer view:** Appreciated if scoped; rejected if presented as a fake compiler for prose.  
**Improved recommendation:** Do not build an executable validator yet. First make validation rules precise enough to automate later and label all remaining checks as review work.

### F-003 — Expand generator prompts

**Verdict:** Keep.  
**Necessary:** Yes. Single-sentence prompts cannot reliably produce a governed artifact.  
**Overengineering risk:** Repeating the entire Framework Core in every prompt is wasteful.  
**Simpler alternative:** One shared preamble plus a compact, fixed delta format: owner, inputs, output, must-link IDs, stop conditions, and applicability rule.  
**20-project relevance:** Yes; this is the highest-leverage consistency mechanism.  
**Cognitive load:** Reduces it for users and agents because each prompt becomes predictable.  
**Senior-engineer view:** Appreciated; explicit interfaces beat hidden conventions.  
**Improved recommendation:** Standardize prompt deltas to one page or less; avoid document-specific essay prompts.

### F-004 — Centralize review finding schema

**Verdict:** Keep.  
**Necessary:** Yes, because repair safety depends on recording whether a finding needs new project truth.  
**Overengineering risk:** A separate review subsystem or many status fields is unnecessary.  
**Simpler alternative:** Put one five-column schema directly in `VALIDATION.md` and have each review prompt reference it.  
**20-project relevance:** Yes; uniform findings enable consistent handoffs.  
**Cognitive load:** Reduces duplicate prompt instructions.  
**Senior-engineer view:** Appreciated as a small, obvious standard.  
**Improved recommendation:** Require only severity, evidence, affected ID, repair safety, and action; remove nonessential review metadata.

### F-005 — Remove static validation report

**Verdict:** Keep.  
**Necessary:** Yes. A framework-local `PASS` report is stale by design and conflicts with the report being a per-run artifact.  
**Overengineering risk:** None; this is removal of confusion.  
**Simpler alternative:** Keep a short `DOGFOOD_EVIDENCE.md` if the repository needs to show tested scenarios, but do not call it the current validation report.  
**20-project relevance:** Yes; report location and freshness become more important over time.  
**Cognitive load:** Reduces ambiguity.  
**Senior-engineer view:** Strongly appreciated; stale green reports are a known anti-pattern.  
**Improved recommendation:** Define project-local report placement and retain framework-level dogfood evidence separately.

### F-006 — Add explicit applicability to templates

**Verdict:** Keep.  
**Necessary:** Yes. Forced `API-001`, `ADR-001`, and `REF-001` directly pressure agents to invent facts.  
**Overengineering risk:** An elaborate applicability state machine is unnecessary.  
**Simpler alternative:** Add one `## Applicability` section with `Applicable` or `Not Applicable — evidence: <RQ-* or upstream ID>`, then make item sections conditional.  
**20-project relevance:** Yes; not every project has the same technical/documentation surface.  
**Cognitive load:** Slight increase in templates, substantial reduction in generation ambiguity.  
**Senior-engineer view:** Appreciated as honest modeling of optional concerns.  
**Improved recommendation:** Apply this only to DECISIONS, API, and REFERENCE first; BUSINESS_RULES, TESTING, and CHECKLIST remain mandatory but may have empty/limited sections.

### F-007 — Formalize traceability lifecycle

**Verdict:** Keep, but avoid a complex lifecycle model.  
**Necessary:** Yes. The register must be available before downstream documents; links appear later.  
**Overengineering risk:** State-machine fields, append-only enforcement, or multiple traceability files would be excessive.  
**Simpler alternative:** Define two headings: `Requirement Register` is created first; `Derived Links` is progressively updated. The same file remains valid throughout.  
**20-project relevance:** Yes; this prevents pipeline ambiguity without adding tools.  
**Cognitive load:** Low increase, high clarity gain.  
**Senior-engineer view:** Appreciated if it remains this simple.  
**Improved recommendation:** Do not call it a two-phase artifact; call it an initialized document with progressively completed sections.

### F-008 — Reduce regeneration promise to document-level impact

**Verdict:** Keep.  
**Necessary:** Yes. Current traceability does not support automatic ID-level regeneration.  
**Overengineering risk:** ID-level rebuild orchestration is premature without an executor.  
**Simpler alternative:** Use IDs to identify sections for review, but define V1 regeneration as “regenerate named documents and inspect linked IDs.”  
**20-project relevance:** Yes; document-level impact remains useful even with many projects.  
**Cognitive load:** Reduces false precision and operational complexity.  
**Senior-engineer view:** Appreciated; honest scope is preferable to architecture theater.  
**Improved recommendation:** Keep the impact table, add add/change/deprecate ID semantics, and postpone automatic minimal rebuilds.

### F-009 — Define exact metadata schema and source fingerprint

**Verdict:** Keep, but reduce source-fingerprint ambition.  
**Necessary:** An exact metadata schema is necessary. A content hash for every PDF/DOCX is not.  
**Overengineering risk:** Computing canonical hashes across converted documents introduces tool-specific complexity and false change signals.  
**Simpler alternative:** Require `requirements_source` as a path/URI and `requirements_revision` as a human-supplied version/date/commit identifier. Make the revision optional when unavailable.  
**20-project relevance:** Yes; provenance matters when documents evolve.  
**Cognitive load:** Small, manageable increase.  
**Senior-engineer view:** Appreciated if it avoids invented hashing infrastructure.  
**Improved recommendation:** Specify metadata fields once and use them verbatim in every template; no automatic fingerprinting in V1.

### F-010 — Tighten deterministic prose policy

**Verdict:** Keep, but reject any attempt at byte-identical prose.  
**Necessary:** Stable facts, IDs, headings, and links need deterministic behavior.  
**Overengineering risk:** Mandating identical prose from generative systems is impractical and would harm readability.  
**Simpler alternative:** Define determinism as stable claims and structure; require reviewers to treat explanatory wording as non-authoritative.  
**20-project relevance:** Yes; stable anchors make reviews and migrations reliable.  
**Cognitive load:** Low; it clarifies what must remain stable.  
**Senior-engineer view:** Appreciated if realistic.  
**Improved recommendation:** Keep the existing “claims versus prose” distinction, but explicitly state that headings, IDs, metadata, and canonical statements are deterministic invariants.

### F-011 — Expand Agent instructions

**Verdict:** Keep, but merge.  
**Necessary:** Yes. IMPLEMENT omitting decisions and traceability is a real reliability defect.  
**Overengineering risk:** Five long agent documents repeating document-loading rules would recreate the prompt-drift problem.  
**Simpler alternative:** Put discovery, compatibility, applicability, freshness, stop, and handoff rules in `SYSTEM.md`; keep four role documents as short reading-order deltas.  
**20-project relevance:** Yes; portable agent behavior becomes more valuable over time.  
**Cognitive load:** Reduces it by centralizing common behavior.  
**Senior-engineer view:** Appreciated; shared policy plus focused roles is conventional and maintainable.  
**Improved recommendation:** Do not add more agent categories until repeated workflows show that the five roles are insufficient.

### F-012 — Relax identifier use for explanatory prose

**Verdict:** Keep.  
**Necessary:** Yes. IDs should serve traceability and change management, not decorate every paragraph.  
**Overengineering risk:** A separate “ID eligibility framework” would be unnecessary.  
**Simpler alternative:** State one rule: assign IDs only to items that may be independently traced, changed, tested, or checked; explanatory text remains unnumbered.  
**20-project relevance:** Yes; prevents identifier inflation as knowledge bases grow.  
**Cognitive load:** Substantially reduces authoring burden.  
**Senior-engineer view:** Appreciated; senior engineers resist meaningless identifiers.  
**Improved recommendation:** Keep IDs for requirements, features, rules, ADRs, API contracts, test obligations, and checklist obligations, but allow document-level reference guidance and philosophy without IDs.

### F-013 — Bound quality-review loops

**Verdict:** Keep.  
**Necessary:** Yes. Unbounded subjective repair loops are a predictable agent failure mode.  
**Overengineering risk:** A workflow engine, reviewer voting, or many exception states is not needed.  
**Simpler alternative:** Allow one repair/review pass. If a blocker or major finding remains, stop with a blocking question or human-approved exception.  
**20-project relevance:** Yes; bounded review protects throughput across many projects.  
**Cognitive load:** Reduces it by making termination explicit.  
**Senior-engineer view:** Appreciated; this is a practical review discipline.  
**Improved recommendation:** Add this rule to `VALIDATION.md`, not every prompt.

### F-014 — Downgrade unsupported PASS claims

**Verdict:** Keep.  
**Necessary:** Yes. Audit evidence must match performed checks.  
**Overengineering risk:** None.  
**Simpler alternative:** Report checks as `performed`, `not performed`, or `blocked`; reserve `PASS` for evidenced checks only.  
**20-project relevance:** Yes; trust in reports compounds across projects.  
**Cognitive load:** Slightly more honest reporting, less future debugging.  
**Senior-engineer view:** Strongly appreciated.  
**Improved recommendation:** Make evidence links mandatory only for blocker/major claims, not every minor check.

### F-015 — Add more template guidance

**Verdict:** Keep, but limit it.  
**Necessary:** Partially. Current templates are too skeletal for consistent high-quality output.  
**Overengineering risk:** Turning templates into long manuals recreates the documentation bulk the framework is meant to avoid.  
**Simpler alternative:** Add one short italic “include when” and “do not include” note under each required heading, or move such guidance into generator deltas.  
**20-project relevance:** Yes; minimal guidance prevents drift across many agents and projects.  
**Cognitive load:** Small increase, significant reduction in ambiguity.  
**Senior-engineer view:** Appreciated if templates remain skimmable.  
**Improved recommendation:** Keep templates structural; place nuanced judgment in generator/review prompts.

## Rejected or Narrowed Ideas

- **Do not build a generic semantic validator in V1.** Markdown prose cannot be reliably contradiction-checked without a much larger system.
- **Do not require byte-identical generated prose.** Stable canonical claims are sufficient.
- **Do not require full downstream traceability for every requirement.** Mandatory links should remain proportional to applicability.
- **Do not add a central identifier allocator, plugin architecture, structured manifest, or automated regeneration executor in V1.**
- **Do not split traceability into multiple top-level documents.** Two headings in one document solve the lifecycle issue.

## Prioritized V1 Correction Set

1. Complete one end-to-end dogfood run with all eight documents and one regeneration change.
2. Align templates with applicability and define a single metadata schema.
3. Replace one-sentence generator prompts with a shared contract plus compact standard deltas.
4. Centralize the review finding schema and bound review/repair to one iteration.
5. Make validation reports evidence-based, project-local, and explicit about checks not performed.
6. Move common agent behavior into `SYSTEM.md`; correct the role-specific reading sets.

## Final Assessment

Most recommendations in `REVIEW.md` address genuine implementation gaps and remain useful after 20 projects. The weak versions are the ones that imply a future runtime system: generic semantic validation, automatic ID-level regeneration, byte-identical prose, and elaborate traceability lifecycle machinery. The smallest durable V1 is a disciplined Markdown protocol with clear contracts, honest manual validation, complete dogfooding, and progressive automation only when repeated use supplies evidence.
