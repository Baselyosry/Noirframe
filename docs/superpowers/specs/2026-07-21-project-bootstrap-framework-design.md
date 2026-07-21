# Project Bootstrap Framework — Design

## Purpose

Create a reusable, domain-neutral engineering operating system that transforms a requirements document into a human-readable, internally consistent engineering knowledge base. The framework defines how project documentation is generated, validated, traced, and consumed; it never supplies project facts, architecture, entities, workflows, permissions, authentication, or payments that the requirements do not establish.

## Architecture

The framework has three independently versioned layers.

### Layer 1: Framework Core (static)

The Framework Core is the constitution and execution model. It is not regenerated per project. It includes the framework specification, manifest, placeholder language, generation flow, dependency graph, validation model, regeneration model, templates, and generator prompts.

`FRAMEWORK_MANIFEST.md` is the version authority. It declares the framework version, supported document-schema versions, placeholder-language version, validation-rule version, generator-pipeline version, and compatible Agent Layer versions. A structured manifest may later mirror the Markdown document, but cannot become a second source of truth.

### Layer 2: Project Knowledge Base (generated)

Each project receives a knowledge base containing:

- `PROJECT.md`
- `BUSINESS_RULES.md`
- `DECISIONS.md`
- `API.md`
- `REFERENCE.md`
- `TESTING.md`
- `FEATURE_CHECKLIST.md`
- `TRACEABILITY.md`

The first seven documents are the project’s canonical engineering knowledge. `TRACEABILITY.md` is a derived control-plane artifact that records provenance and relations across it. It must never invent project meaning.

### Layer 3: Agent Layer (static)

The Agent Layer is a reusable set of project-neutral instructions, including `SYSTEM.md`, `IMPLEMENT_FEATURE.md`, `REVIEW_FEATURE.md`, `CREATE_MODULE.md`, `GENERATE_API.md`, `WRITE_TESTS.md`, `FIX_BUG.md`, and `REFACTOR.md`.

Agents consume the generated Project Knowledge Base and validate work against it. They do not encode project-specific facts. They must stop and surface missing, contradictory, or unresolved blocking information rather than infer it.

## Canonical Ownership and Identifiers

Every factual statement has exactly one canonical owner. Other documents reference its identifier rather than restating it as an independently authoritative fact.

| Namespace | Canonical owner | Meaning |
|---|---|---|
| `RQ-*` | `TRACEABILITY.md` | Normalized requirement and source provenance |
| `FEAT-*` | `PROJECT.md` | Project capability or scoped feature |
| `BR-*` | `BUSINESS_RULES.md` | Behavioral rule |
| `ADR-*` | `DECISIONS.md` | Accepted architecture or technical decision |
| `API-*` | `API.md` | Consumer-facing API contract or convention |
| `REF-*` | `REFERENCE.md` | Implementation pattern |
| `TEST-*` | `TESTING.md` | Test obligation |
| `FC-*` | `FEATURE_CHECKLIST.md` | Completion obligation |

Identifiers are immutable, never renumbered, and never reused. Retired entries remain addressable and are marked deprecated, with a successor ID when one exists.

The traceability identifier registry is the central allocator. Generators request valid IDs in their namespace; they do not independently choose them. This prevents collisions and makes partial regeneration stable.

## Metadata

Every generated document begins with a concise, human-readable YAML metadata block containing:

- document ID and document-schema version
- framework version
- generation status (`draft`, `approved`, or `regenerated`)
- generation timestamp
- requirement IDs and source locators used
- upstream document IDs and dependency IDs
- regeneration triggers, partial scope, and full-regeneration conditions

Metadata is machine-checkable but must remain understandable without tooling.

## Generation and Dependency Model

The default initial pipeline is:

`Requirements → PROJECT → BUSINESS_RULES → DECISIONS → API → REFERENCE → TESTING → FEATURE_CHECKLIST`

The actual model is a directed acyclic graph. Each document can depend on multiple applicable upstream sources. For example, API conventions can derive from project scope, business rules, and accepted decisions; testing obligations can derive from rules, contracts, decisions, and implementation patterns.

`TRACEABILITY.md` is initialized first with the normalized requirement registry and source locators, enriched after every document generation, and finalized at the end. It tracks links as `confirmed`, `derived`, `assumption`, or `unresolved`.

## Universal Generation Contract

Every generator inherits this contract:

1. **Load inputs:** requirements, required upstream documents, Framework Core rules, placeholder specification, manifest, and its generator prompt.
2. **Validate inputs:** required artifacts exist, versions are compatible, the dependency graph is satisfied, and no blocking validation error exists.
3. **Extract owned information only:** generate only facts owned by the target document. Record uncertainty instead of moving facts across ownership boundaries or guessing.
4. **Generate draft:** populate the template, allocate stable IDs from the registry, write metadata, and preserve canonical ownership.
5. **Self-validate:** check framework rules, placeholders, dependencies, ownership, traceability, and target-document quality gates.
6. **Emit output:** write the document; enrich traceability; update the validation report; and update regeneration metadata.

The contract is deterministic in policy and structure: identical approved inputs, versions, and resolved assumptions must lead to the same claims, ownership, identifiers, and validation outcome. Natural-language phrasing need not be byte-identical.

## Assumption Contract

No undocumented assumption may appear in project documentation. Every assumption has an ID, source context, affected artifacts, owner, and one classification:

- **Blocking assumption:** generation stops until a user resolves it.
- **Temporary assumption:** generation continues with the assumption visibly recorded and validation status downgraded.
- **Derived assumption:** logically follows from confirmed project information and includes explicit reasoning.
- **Explicit user assumption:** was approved directly by the user and includes approval provenance.

An assumption can never silently become a confirmed requirement or accepted decision.

## Quality Gates

A generator marks a document successful only after all three independent gates pass:

1. **Structural:** template complete, metadata complete, identifiers valid.
2. **Semantic:** no contradictions, canonical ownership preserved, dependencies satisfied, and required traceability links present.
3. **Framework:** compatible with the manifest, Agent Layer, and validation-rule version.

Critical failures block generation. Examples include invalid IDs, missing mandatory upstream artifacts, unknown references, unresolved blocking assumptions, duplicate canonical ownership, incompatible versions, and absent required downstream obligations. Warnings are recorded but do not block output.

`validation/VALIDATION_REPORT.md` summarizes errors and warnings, including missing traceability, broken dependencies, invalid placeholders, duplicate ownership, contradictions, and missing downstream artifacts.

## Regeneration

Every document declares:

- triggering upstream change classes
- safe partial-regeneration boundary
- conditions requiring full regeneration

The dependency graph and traceability links compute the smallest affected set. Full regeneration is required for framework/document-schema/placeholder incompatibility, an identifier-registry failure, or a change that invalidates the project’s foundational scope.

## Writing and Human-Factor Standards

Generated documents are human-first. They use concise prose, clear headings, explicit source identifiers, cross-references, and explicit uncertainty. Structured metadata supports tooling without replacing engineering explanation.

The framework optimizes for correctness, consistency, traceability, controlled regeneration, and maintainability—not novelty or creative completion. Missing information must block generation or be explicitly recorded according to the Assumption Contract.

## Non-Goals

The framework does not:

- invent product scope, business rules, architecture, entities, workflows, access control, authentication, payment, or integration details;
- replace stakeholder decisions with model assumptions;
- force an API, database, frontend, or implementation style when the requirements do not establish one;
- make an unresolved requirement appear implemented or approved.
