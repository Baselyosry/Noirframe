# Project Bootstrap Framework Specification

## Purpose

This framework turns a supplied requirements document into a coherent engineering knowledge base. It defines the process and quality standard; it never supplies project scope, entities, architecture, workflows, permissions, authentication, payments, integrations, or other project facts.

## Architecture

The framework has three layers:

1. **Framework Core** — static, versioned methodology and reusable assets.
2. **Project Knowledge Base** — generated project truth: `PROJECT.md`, `BUSINESS_RULES.md`, `DECISIONS.md`, `API.md`, `REFERENCE.md`, `TESTING.md`, `FEATURE_CHECKLIST.md`, and `TRACEABILITY.md`.
3. **Agent Layer** — static, tool-neutral operating instructions that tell an agent how to consume the Project Knowledge Base.

## Canonical Ownership

Every project fact has exactly one canonical owner. Other documents may link to its ID but must not redefine it.

| ID namespace | Owner | Owns |
|---|---|---|
| `RQ-*` | `TRACEABILITY.md` | Normalized requirement and provenance |
| `FEAT-*` | `PROJECT.md` | Scope and capability |
| `BR-*` | `BUSINESS_RULES.md` | Behavior and business constraints |
| `ADR-*` | `DECISIONS.md` | Accepted technical decision |
| `API-*` | `API.md` | Consumer-facing contract |
| `REF-*` | `REFERENCE.md` | Implementation pattern |
| `TEST-*` | `TESTING.md` | Verification obligation |
| `FC-*` | `FEATURE_CHECKLIST.md` | Completion obligation |

IDs are immutable, never reused, and preserved on regeneration. The owning document mints its own IDs; validation detects invalid or duplicate IDs.

## Universal Generation Contract

Every generator follows this lifecycle:

1. Load the requirements, Framework Core, target template, and required upstream documents.
2. Validate required inputs, versions, dependencies, and blocking findings.
3. Extract only information owned by the target document.
4. Generate a draft with stable IDs and stable metadata.
5. Run mechanical validation.
6. Run the target document's quality review.
7. Repair findings only when repair needs no new project fact.
8. Re-run mechanical validation.
9. Emit the final document, traceability update, regeneration impact, and validation report.

The policy is deterministic: equivalent approved inputs produce equivalent claims, ownership, identifiers, and validation outcomes. Prose may vary without changing meaning.

## Document Review and Repair

Mechanical validation checks objective correctness. Quality review checks clarity, ambiguity, duplication, ownership, scope leakage, unsupported detail, readability, and maintainability.

Repairs may remove duplication, simplify prose, relocate a statement to its owner, or resolve a mechanically provable contradiction. Repairs must preserve IDs and traceability. A repair must not invent project facts. Missing stakeholder information becomes a blocking question or a visible uncertainty record. Any repair is followed by full mechanical revalidation.

## Assumption Contract

Uncertainty is always explicit and carries a source locator, affected IDs, and status:

- **blocking question** — generation stops until resolved;
- **provisional assumption** — generation may continue only with visible provisional status;
- **user-approved assumption** — directly approved by a stakeholder;
- **derived conclusion** — follows from confirmed evidence and states that evidence.

No uncertainty may silently become a confirmed requirement or accepted decision.

## Metadata

Generated documents use human-readable YAML metadata containing document ID, schema version, framework version, upstream documents, and source requirement IDs. Run timestamps, hashes, and validation outcomes belong in `VALIDATION_REPORT.md`, not in canonical documents.

## Quality Gates

A final document passes only when all gates pass:

1. **Structural:** required metadata and sections exist; IDs are valid; final output contains no unresolved placeholders.
2. **Semantic:** references resolve; ownership is unique; required dependency and traceability links exist; no contradiction is found.
3. **Framework:** document and Agent Layer versions are compatible with `FRAMEWORK_MANIFEST.md`.
4. **Quality review:** no blocker or major finding remains; the document is clear, concise, and does not invent project knowledge.

## Writing Standards

Write for engineers first: concise prose, clear headings, stable IDs, explicit sources, and no duplicated authority. Use `Not Applicable` only with evidence that the concern is absent from requirements. Do not fill an absent concern with a default architecture or workflow.

## Multi-Agent Discipline

Only one agent may modify a canonical artifact in a change set. Other agents contribute ID-addressed findings through the validation report. Agent instructions must be tool-neutral and must stop on missing or contradictory project truth.

## Non-Goals

The framework does not replace stakeholder decisions, create a runtime generation engine, require a particular IDE, or add plugin infrastructure before evidence from real projects demonstrates the need.
