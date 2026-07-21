# Framework Extraction

## Reusable Method

- Separate project scope, behavior, technical decisions, contracts, patterns, tests, and completion obligations.
- Preserve one canonical owner per fact.
- Use stable identifiers and evidence-based traceability.
- Label uncertainty and stop for blocking gaps.
- Validate mechanically, review quality, repair without inventing facts, then revalidate.

## Project-Specific Content

Reference-project entities, roles, technical stack, integrations, workflows, operational policies, and examples are excluded from Framework Core, templates, prompts, and Agent Layer.

## Placeholder Sources

Every placeholder is filled from requirements, an explicitly named upstream canonical document, or a visible user-approved assumption. No template supplies project defaults.

## Simplified V1

Dependency, regeneration, and generation-flow rules are consolidated in `GENERATION_GUIDE.md`; validation and review rules are consolidated in `VALIDATION.md`; static agent instructions live in `Agents/`.
