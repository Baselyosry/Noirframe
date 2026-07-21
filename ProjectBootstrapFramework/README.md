# Project Bootstrap Framework

## Purpose

Generate a project engineering knowledge base from requirements while preserving evidence, canonical ownership, explicit uncertainty, human readability, and tool neutrality.

## Start Here

1. Read `FRAMEWORK_SPEC.md`.
2. Confirm versions in `FRAMEWORK_MANIFEST.md`.
3. Normalize `<Requirements Document>` into `TRACEABILITY.md`.
4. Follow `GENERATION_GUIDE.md` to generate the Project Knowledge Base.
5. For every document run mechanical validation, its review prompt, repair, and revalidation.
6. Use `VALIDATION_REPORT.md` to record the run and regeneration impact.

## Framework Core

- `FRAMEWORK_SPEC.md`: universal rules and ownership.
- `FRAMEWORK_MANIFEST.md`: compatibility.
- `PLACEHOLDER_SPEC.md`: template placeholders.
- `GENERATION_GUIDE.md`: intake, dependencies, and regeneration.
- `VALIDATION.md`: validation and review policy.

## Project Knowledge Base

`PROJECT.md` owns scope; `BUSINESS_RULES.md` owns behavior; `DECISIONS.md` owns accepted choices; `API.md` owns contracts; `REFERENCE.md` owns patterns; `TESTING.md` owns verification; `FEATURE_CHECKLIST.md` owns completion; `TRACEABILITY.md` owns provenance and relationships.

## Extension Rule

Do not add a Core concept because it may be useful. Add it only after real project use identifies a problem the existing framework cannot solve.
