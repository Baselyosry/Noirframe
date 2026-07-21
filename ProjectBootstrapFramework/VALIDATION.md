# Validation

## Mechanical Validation

Mechanical validation is objective and blocks final output on failure.

### Structural

- Required metadata and required headings exist.
- IDs match their namespace syntax.
- Final documents contain no unresolved placeholders.

### Semantic

- Referenced IDs resolve.
- Canonical ownership is not duplicated.
- Required dependency and traceability links exist.
- Contradictory canonical statements are reported.

### Framework

- Document schema and Agent Layer versions are compatible with `FRAMEWORK_MANIFEST.md`.

## Quality Review

Quality review is an engineering critique, not a structural check. It evaluates clarity, ambiguity, duplication, ownership, unnecessary detail, implementation leakage, unsupported claims, readability, and agent usability.

Findings use `blocker`, `major`, `minor`, or `note`. Blocker and major findings require repair. Repairs may not invent facts and must be followed by all mechanical checks again.

## Validation Report

Each generation run emits one `VALIDATION_REPORT.md` from the report template. It records inputs, version checks, mechanical findings, quality findings, repairs, blocking questions, regeneration impact, and final status. It is a run artifact, not a canonical project knowledge source.
