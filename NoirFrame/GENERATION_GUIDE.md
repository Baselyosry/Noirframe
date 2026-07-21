# Generation Guide

## Requirement Intake

Normalize each source requirement into an `RQ-*` entry in `TRACEABILITY.md`. Record a source locator and one status: `confirmed`, `blocking_question`, `provisional_assumption`, `user_approved_assumption`, or `derived_conclusion`.

## Default Pipeline

1. Initialize the `RQ-*` requirement register in `TRACEABILITY.md`.
2. Generate `PROJECT.md`.
3. Generate `BUSINESS_RULES.md`.
4. Generate `DECISIONS.md`.
5. Generate `API.md`.
6. Generate `REFERENCE.md`.
7. Generate `TESTING.md`.
8. Generate `FEATURE_CHECKLIST.md`.
9. Enrich and finalize `TRACEABILITY.md`.

This is an onboarding sequence, not a strict single-parent chain.

## Dependency Edge List

- `PROJECT.md` depends on normalized requirements.
- `BUSINESS_RULES.md` depends on requirements and `PROJECT.md`.
- `DECISIONS.md` depends on requirements, project constraints, and confirmed decisions.
- `API.md` depends on applicable project scope, rules, and decisions.
- `REFERENCE.md` depends on applicable decisions and contracts.
- `TESTING.md` depends on rules, contracts, decisions, and reference patterns.
- `FEATURE_CHECKLIST.md` depends on all applicable upstream obligations.
- `TRACEABILITY.md` indexes all applicable IDs and relationships.

## Regeneration Impact

| Change class | Regenerate |
|---|---|
| scope | `PROJECT.md`, affected rules, traceability |
| behavior | rules, affected API/testing/checklist, traceability |
| technical decision | decisions, affected API/reference/testing/checklist |
| contract | API, affected reference/testing/checklist |
| wording only | changed document only unless canonical IDs or claims change |

Full regeneration is required when a major framework version changes or normalized requirements are fundamentally replaced.

## Not Applicable

When requirements do not establish a concern, retain the document schema and write `Not Applicable` with evidence. Never invent an API, implementation style, data store, identity model, or workflow merely to complete a section.
