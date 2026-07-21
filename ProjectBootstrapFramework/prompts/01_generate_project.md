# Generate PROJECT.md

## Purpose

Create the canonical project-scope document. It owns `FEAT-*` identifiers and answers what the project is, who it serves, what it includes, what constrains it, and what is explicitly out of scope.

## Inputs

- Requirements document
- `FRAMEWORK_SPEC.md`
- `FRAMEWORK_MANIFEST.md`
- `PLACEHOLDER_SPEC.md`
- `GENERATION_GUIDE.md`
- `VALIDATION.md`
- `templates/PROJECT.template.md`
- initialized `TRACEABILITY.md`

## Required Behavior

Follow `GENERATION_PREAMBLE.md` when it exists. Extract confirmed project scope only. Normalize each supported capability into a stable `FEAT-*` identifier. Link features to their `RQ-*` sources in `TRACEABILITY.md`.

Do not create business rules, technical decisions, API contracts, data models, permissions, workflows, authentication, payment behavior, or implementation architecture. Refer to a blocking question or visible uncertainty when requirements are insufficient.

## Validation

- Each `FEAT-*` has an `RQ-*` source.
- Every included section is supported by requirements or labeled uncertainty.
- No statement is owned by a downstream document.
- No final placeholder remains.

## Regeneration

Regenerate all of `PROJECT.md` after a scope change. Preserve IDs for unchanged features. Regenerate only affected downstream documents according to `GENERATION_GUIDE.md`.
