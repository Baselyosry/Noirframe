# Implementation Summary

## Core

### `FRAMEWORK_SPEC.md`
Core — Defines the framework constitution: layers, ownership, generation/review/repair, uncertainty, quality gates, and agent discipline.  
Depends on: design decisions. Used by: every template, prompt, validator, and Agent. Approximate size: 90 lines.

### `FRAMEWORK_MANIFEST.md`
Core — Declares V1 versions, compatibility policy, evidence rule, and current freeze status.  
Depends on: Core versioning policy. Used by: generators, validators, and Agents. Approximate size: 35 lines.

### `PLACEHOLDER_SPEC.md`
Core — Defines placeholder syntax, valid sources, and formatting restrictions.  
Depends on: framework ownership rules. Used by: templates, generators, and structural validation. Approximate size: 35 lines.

### `GENERATION_GUIDE.md`
Core — Defines requirement intake, default document order, dependency edges, regeneration impact, and applicability behavior.  
Depends on: Framework Spec. Used by: generators, CHANGE Agent, and regeneration reviews. Approximate size: 50 lines.

### `VALIDATION.md`
Core — Defines structural, semantic, framework, and quality-review checks.  
Depends on: Framework Spec and Manifest. Used by: all generators, review prompts, and validation reports. Approximate size: 35 lines.

### `README.md`
Core — Provides framework purpose, bootstrap sequence, document ownership overview, and extension rule.  
Depends on: all Core documents. Used by: human adopters and onboarding agents. Approximate size: 35 lines.

## Templates

### `templates/PROJECT.template.md`
Template — Skeleton for canonical project scope, actors, features, concepts, constraints, and non-goals.  
Depends on: Placeholder Spec and Traceability. Used by: PROJECT generator. Approximate size: 50 lines.

### `templates/BUSINESS_RULES.template.md`
Template — Skeleton for domain/flow purpose, rules, permissions, lifecycle, and uncertainty.  
Depends on: PROJECT and TRACEABILITY. Used by: BUSINESS_RULES generator. Approximate size: 25 lines.

### `templates/DECISIONS.template.md`
Template — Skeleton for accepted technical decisions with evidence, alternatives, trade-offs, and consequences.  
Depends on: PROJECT, BUSINESS_RULES, and TRACEABILITY. Used by: DECISIONS generator. Approximate size: 20 lines.

### `templates/API.template.md`
Template — Skeleton for consumer contracts, resource behavior, requests/responses, and compatibility.  
Depends on: PROJECT, BUSINESS_RULES, DECISIONS, and TRACEABILITY. Used by: API generator. Approximate size: 20 lines.

### `templates/REFERENCE.template.md`
Template — Skeleton for implementation patterns, applicability, boundaries, and error/validation guidance.  
Depends on: DECISIONS, API, and TRACEABILITY. Used by: REFERENCE generator. Approximate size: 20 lines.

### `templates/TESTING.template.md`
Template — Skeleton for testing philosophy and observable verification obligations.  
Depends on: BUSINESS_RULES, API, DECISIONS, REFERENCE, and TRACEABILITY. Used by: TESTING generator. Approximate size: 25 lines.

### `templates/FEATURE_CHECKLIST.template.md`
Template — Skeleton for traceable, verifiable completion obligations and completion gate.  
Depends on: upstream project knowledge and TRACEABILITY. Used by: FEATURE_CHECKLIST generator. Approximate size: 20 lines.

### `templates/TRACEABILITY.template.md`
Template — Skeleton for source requirements, cross-document relationships, and regeneration impact index.  
Depends on: raw requirements. Used by: TRACEABILITY generator and all downstream generators. Approximate size: 30 lines.

## Generation Prompts

### `prompts/GENERATION_PREAMBLE.md`
Prompt — Shared instruction to load inputs, preserve IDs, avoid invention, and complete review/repair.  
Depends on: Framework Core. Used by: all generators. Approximate size: 5 lines.

### `prompts/01_generate_project.md`
Prompt — Generates PROJECT-owned scope and `FEAT-*` identifiers with validation and regeneration guidance.  
Depends on: requirements, Core, PROJECT template, and initialized TRACEABILITY. Used by: PROJECT generation. Approximate size: 35 lines.

### `prompts/02_generate_business_rules.md`
Prompt — Directs evidence-based generation of `BR-*` rules without invented permissions or state changes.  
Depends on: RQ and FEAT records. Used by: BUSINESS_RULES generation. Approximate size: 5 lines.

### `prompts/03_generate_decisions.md`
Prompt — Directs evidence-based generation of accepted `ADR-*` decisions.  
Depends on: requirements and stakeholder decisions. Used by: DECISIONS generation. Approximate size: 5 lines.

### `prompts/04_generate_api.md`
Prompt — Directs generation of supported `API-*` consumer contracts or evidence-based inapplicability.  
Depends on: scope, rules, decisions. Used by: API generation. Approximate size: 5 lines.

### `prompts/05_generate_reference.md`
Prompt — Directs generation of `REF-*` implementation patterns without selecting unsupported technologies.  
Depends on: decisions and contracts. Used by: REFERENCE generation. Approximate size: 5 lines.

### `prompts/06_generate_testing.md`
Prompt — Directs generation of observable `TEST-*` obligations without selecting a test runner.  
Depends on: rules, contracts, decisions, and patterns. Used by: TESTING generation. Approximate size: 5 lines.

### `prompts/07_generate_feature_checklist.md`
Prompt — Directs generation of `FC-*` completion checks linked to upstream owners.  
Depends on: all applicable upstream obligations. Used by: FEATURE_CHECKLIST generation. Approximate size: 5 lines.

### `prompts/08_generate_traceability.md`
Prompt — Initializes RQ provenance and enriches evidence-based relationships without creating project facts.  
Depends on: requirements and generated document IDs. Used by: TRACEABILITY generation. Approximate size: 5 lines.

## Review Prompts

### `prompts/reviews/PROJECT.review.md`
Prompt — Reviews project scope for clarity, ownership, invention, and repair safety using a finding table.  
Depends on: requirements, PROJECT, TRACEABILITY, and Core. Used by: PROJECT quality review. Approximate size: 15 lines.

### `prompts/reviews/BUSINESS_RULES.review.md`
Prompt — Reviews rule evidence, uncertainty, duplication, and unsupported behavioral detail.  
Depends on: BUSINESS_RULES and upstream IDs. Used by: BUSINESS_RULES quality review. Approximate size: 5 lines.

### `prompts/reviews/DECISIONS.review.md`
Prompt — Reviews whether ADRs reflect accepted choices rather than recommendations.  
Depends on: DECISIONS and decision evidence. Used by: DECISIONS quality review. Approximate size: 5 lines.

### `prompts/reviews/API.review.md`
Prompt — Reviews contract clarity, unsupported protocol detail, compatibility, and ownership.  
Depends on: API and upstream rules. Used by: API quality review. Approximate size: 5 lines.

### `prompts/reviews/REFERENCE.review.md`
Prompt — Reviews patterns for evidence, unnecessary prescription, duplication, and readability.  
Depends on: REFERENCE and upstream decisions/contracts. Used by: REFERENCE quality review. Approximate size: 5 lines.

### `prompts/reviews/TESTING.review.md`
Prompt — Reviews verification obligations for unsupported cases and test-framework invention.  
Depends on: TESTING and source obligations. Used by: TESTING quality review. Approximate size: 5 lines.

### `prompts/reviews/FEATURE_CHECKLIST.review.md`
Prompt — Reviews whether completion checks are verifiable, owned, and evidence-based.  
Depends on: FEATURE_CHECKLIST and upstream IDs. Used by: checklist quality review. Approximate size: 5 lines.

### `prompts/reviews/TRACEABILITY.review.md`
Prompt — Reviews provenance, links, fabricated relationships, and impact records.  
Depends on: TRACEABILITY and generated IDs. Used by: traceability quality review. Approximate size: 5 lines.

## Agent Layer

### `Agents/SYSTEM.md`
Agent — Defines tool-neutral behavior, canonical-ID authority, stop conditions, and single-writer discipline.  
Depends on: Framework Core. Used by: every coding/review agent. Approximate size: 10 lines.

### `Agents/IMPLEMENT.md`
Agent — Defines the knowledge-base reading set and completion expectations for implementation work.  
Depends on: project documents and validation report. Used by: implementation agents. Approximate size: 5 lines.

### `Agents/REVIEW.md`
Agent — Defines ID-addressed review behavior and separation of validation from quality critique.  
Depends on: canonical owner, reference, checklist, and testing documents. Used by: review agents. Approximate size: 5 lines.

### `Agents/TEST.md`
Agent — Defines rule/contract-driven test review behavior without choosing project behavior.  
Depends on: BUSINESS_RULES, API, TESTING, and TRACEABILITY. Used by: test agents. Approximate size: 5 lines.

### `Agents/CHANGE.md`
Agent — Defines change-impact analysis, stable-ID preservation, and regeneration reporting.  
Depends on: GENERATION_GUIDE and changed canonical owners. Used by: change-management agents. Approximate size: 5 lines.

## Validation

### `validation/VALIDATION_REPORT.template.md`
Validation — Template for run inputs, check outcomes, findings, repairs, blocking questions, and impact.  
Depends on: VALIDATION.md. Used by: project-local generation runs. Approximate size: 30 lines.

### `validation/VALIDATION_REPORT.md`
Validation — Current framework freeze report documenting repository-level checks and dogfood result.  
Depends on: implemented framework and Work Queue dogfood. Used by: V1 evidence review. Approximate size: 35 lines.

## Supporting Framework Evidence

### `ANALYSIS.md`
Supporting — Records reusable documentation methodology extracted from the golden reference set.  
Depends on: supplied golden documents. Used by: framework design and audit readers. Approximate size: 10 lines.

### `FRAMEWORK_EXTRACTION.md`
Supporting — Separates reusable method, excluded project content, placeholder sources, and simplified V1 choices.  
Depends on: golden-document analysis and architecture decisions. Used by: framework maintainers. Approximate size: 20 lines.

### `REVIEW.md`
Supporting — Principal-architect review identifying V1 implementation defects and recommendations.  
Depends on: full framework inspection. Used by: maintainers before release decisions. Approximate size: 300 lines.

### `REVIEW_V2.md`
Supporting — Challenges each recommendation in REVIEW.md and narrows the V1 correction set.  
Depends on: REVIEW.md and current implementation. Used by: architecture decision-makers. Approximate size: 250 lines.

### `FINAL_VERDICT.md`
Supporting — Compares design, implementation, and reviews to define V1 readiness and priorities.  
Depends on: original design, implementation, REVIEW.md, and REVIEW_V2.md. Used by: release planning. Approximate size: 220 lines.

## Process and Dogfood Support

### `docs/superpowers/specs/2026-07-21-project-bootstrap-framework-design.md`
Supporting — Original approved architecture design used to guide planning and implementation.  
Depends on: user-approved design discussion. Used by: plan and review comparison. Approximate size: 140 lines.

### `docs/superpowers/plans/2026-07-21-project-bootstrap-framework.md`
Supporting — Detailed implementation plan, file structure, validation commands, and vertical-slice tasks.  
Depends on: architecture design and review adjustments. Used by: implementation execution. Approximate size: 620 lines.

### `docs/dogfood/work-queue/Requirements.md`
Supporting — Unrelated sample requirements for dogfooding the first vertical slice.  
Depends on: none. Used by: Work Queue traceability and PROJECT generation. Approximate size: 25 lines.

### `docs/dogfood/work-queue/TRACEABILITY.md`
Supporting — Normalized sample RQ register and links to sample features.  
Depends on: Work Queue requirements. Used by: dogfood PROJECT generation and validation. Approximate size: 35 lines.

### `docs/dogfood/work-queue/PROJECT.md`
Supporting — Generated sample project scope with features and non-goals.  
Depends on: Work Queue requirements and traceability. Used by: dogfood validation/review. Approximate size: 65 lines.

### `docs/dogfood/work-queue/VALIDATION_REPORT.md`
Supporting — Records structural/semantic review, repair, and regeneration impact for the PROJECT slice.  
Depends on: Work Queue PROJECT and traceability. Used by: dogfood evidence. Approximate size: 30 lines.

## Canvas Review Artifacts

### `~/.cursor/projects/Users-philosopher-Projects-MdAgentGenerator/canvases/framework-v1-architecture-review.canvas.tsx`
Supporting — Interactive V1 architecture critique with scores, findings, and simplification recommendations.  
Depends on: early design specification. Used by: architecture review discussion. Approximate size: 350 lines.

### `~/.cursor/projects/Users-philosopher-Projects-MdAgentGenerator/canvases/framework-readiness-review.canvas.tsx`
Supporting — Interactive readiness assessment with component verdicts and final V1 architecture recommendation.  
Depends on: implementation design and prior review. Used by: architecture approval discussion. Approximate size: 400 lines.
