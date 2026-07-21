# Project Bootstrap Framework V1 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a domain-neutral documentation operating system that derives and maintains a project engineering knowledge base from requirements without inventing project facts.

**Architecture:** The deliverable is a static `ProjectBootstrapFramework/` package with six normative Framework Core documents, non-normative golden-project analysis, generic templates, generation/review prompts, static agent instructions, and validation assets. Every generated project document follows one bounded lifecycle: intake → draft → mechanical validation → quality review → repair → mechanical revalidation → final output.

**Tech Stack:** Markdown, YAML front matter, repository-native validation commands (`git diff --check`, `rg`), no runtime dependencies or framework-specific code.

## Global Constraints

- Framework Core, Project Knowledge Base, and Agent Layer contain zero Dar Al-Funoon business terminology, product scope, vendors, roles, workflows, payment rules, or architecture defaults.
- A project fact has exactly one canonical owner; other artifacts link to canonical IDs instead of restating authority.
- Requirements gaps are classified as a blocking question, provisional assumption, user-approved assumption, or derived conclusion; the framework never silently creates project truth.
- Documents are human-first Markdown and tool-neutral; no Framework Core or Agent Layer instruction may depend on Cursor, Claude Code, or another specific IDE.
- Generated document metadata contains stable semantic values only: document/schema/framework version, upstream document IDs, and source revision. Run timestamps, hashes, and pass/fail results belong in `VALIDATION_REPORT.md`.
- IDs are minted by their owning document namespace and preserved on regeneration. Validation, not a central allocator, detects duplicates and broken references.
- Traceability always links applicable `RQ-*` entries to `FEAT-*` and `BR-*`; links to `API-*`, `TEST-*`, and `FC-*` are required only when the requirement creates that artifact type.
- No git commit is created unless the user explicitly requests one.

---

## Final V1 File Structure

```text
ProjectBootstrapFramework/
├── README.md
├── FRAMEWORK_SPEC.md
├── FRAMEWORK_MANIFEST.md
├── PLACEHOLDER_SPEC.md
├── GENERATION_GUIDE.md
├── VALIDATION.md
├── ANALYSIS.md
├── FRAMEWORK_EXTRACTION.md
├── templates/
│   ├── PROJECT.template.md
│   ├── BUSINESS_RULES.template.md
│   ├── DECISIONS.template.md
│   ├── API.template.md
│   ├── REFERENCE.template.md
│   ├── TESTING.template.md
│   ├── FEATURE_CHECKLIST.template.md
│   └── TRACEABILITY.template.md
├── prompts/
│   ├── GENERATION_PREAMBLE.md
│   ├── 01_generate_project.md
│   ├── 02_generate_business_rules.md
│   ├── 03_generate_decisions.md
│   ├── 04_generate_api.md
│   ├── 05_generate_reference.md
│   ├── 06_generate_testing.md
│   ├── 07_generate_feature_checklist.md
│   ├── 08_generate_traceability.md
│   └── reviews/
│       ├── PROJECT.review.md
│       ├── BUSINESS_RULES.review.md
│       ├── DECISIONS.review.md
│       ├── API.review.md
│       ├── REFERENCE.review.md
│       ├── TESTING.review.md
│       ├── FEATURE_CHECKLIST.review.md
│       └── TRACEABILITY.review.md
├── Agents/
│   ├── SYSTEM.md
│   ├── IMPLEMENT.md
│   ├── REVIEW.md
│   ├── TEST.md
│   └── CHANGE.md
└── validation/
    └── VALIDATION_REPORT.template.md
```

`architecture.md`, `dependency-graph.md`, and `regeneration.md` are intentionally not separate Core authorities. Their responsibilities are merged into `GENERATION_GUIDE.md`; this prevents conflicting graph and regeneration rules. `ANALYSIS.md` and `FRAMEWORK_EXTRACTION.md` are non-normative reverse-engineering evidence, not Framework Core policy.

### Task 1: Establish Core Constitution, Versioning, and Golden-Project Evidence

**Files:**
- Create: `ProjectBootstrapFramework/FRAMEWORK_SPEC.md`
- Create: `ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md`
- Create: `ProjectBootstrapFramework/ANALYSIS.md`
- Create: `ProjectBootstrapFramework/FRAMEWORK_EXTRACTION.md`
- Test: `ProjectBootstrapFramework/FRAMEWORK_SPEC.md` and `ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md`

**Interfaces:**
- Consumes: `/Users/philosopher/Documents/Obsidian/TriStack/Dar-AL Funoon/MD Files For Better Agent/{AGENTS,PROJECT,BUSINESS_RULES,DECISIONS,API,REFERENCE,TESTING,FEATURE_CHECKLIST,PROMPTS}.md`
- Produces: the normative rules that all later framework documents reference as `FRAMEWORK_SPEC §<section>` and the compatibility declaration `FRAMEWORK_MANIFEST v1.0.0`.

- [ ] **Step 1: Write the failing Core completeness check**

Run:

```bash
rg -q "^## Universal Generation Contract$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Document Review and Repair$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Assumption Contract$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Quality Gates$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^framework_version: 1.0.0$" "ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md"
```

Expected: fail because the Core documents do not exist.

- [ ] **Step 2: Create `FRAMEWORK_SPEC.md` with the universal lifecycle**

Write the document with these normative sections:

```markdown
## Universal Generation Contract
1. Load inputs.
2. Validate inputs.
3. Extract information owned by the target document only.
4. Generate a draft.
5. Run mechanical validation.
6. Run a document-specific quality review.
7. Repair only review findings that do not require invented project facts.
8. Re-run mechanical validation.
9. Emit the final document, traceability updates, regeneration impact, and validation report.

## Document Review and Repair
Quality review evaluates clarity, ambiguity, duplication, ownership, scope leakage,
readability, and whether the document invents project knowledge. A repair may remove
duplication, improve wording, relocate information to its owner, or reconcile a
mechanically provable contradiction. A repair must not create missing facts. A finding
that requires missing stakeholder information becomes a blocking question or a visibly
recorded provisional assumption.
```

Also define: purpose; Core/Knowledge Base/Agent Layer boundaries; canonical ownership namespaces `RQ`, `FEAT`, `BR`, `ADR`, `API`, `REF`, `TEST`, `FC`; source-of-truth precedence; uncertainty vocabulary; stable metadata; structural/semantic/framework gates; non-goals; human-first writing rules; one artifact writer per multi-agent handoff.

- [ ] **Step 3: Create `FRAMEWORK_MANIFEST.md`**

Use this machine-readable-but-human-readable front matter:

```yaml
framework_version: 1.0.0
core_schema_version: 1.0
placeholder_language_version: 1.0
validation_rules_version: 1.0
agent_layer_version: 1.0
supported_project_document_schema_versions:
  PROJECT: 1.0
  BUSINESS_RULES: 1.0
  DECISIONS: 1.0
  API: 1.0
  REFERENCE: 1.0
  TESTING: 1.0
  FEATURE_CHECKLIST: 1.0
  TRACEABILITY: 1.0
```

Below it, define semantic-version compatibility: patch releases are compatible, minor releases add optional behavior, and major releases require a documented project migration. State that the Markdown manifest is canonical and a structured manifest is deferred until an executor needs it.

- [ ] **Step 4: Create evidence documents without importing domain facts into Core**

`ANALYSIS.md` must analyze the nine supplied golden documents by purpose, responsibility, writing style, repeated patterns, dependencies, assumption handling, validation philosophy, and generation/review patterns. It must identify `AGENTS.md` and `PROMPTS.md` as evidence for the static Agent Layer, not as project knowledge-base outputs.

`FRAMEWORK_EXTRACTION.md` must separate reusable method, Dar Al-Funoon-specific content, placeholders, and source provenance. It must explicitly map the final V1 simplified Core: architecture/dependency/regeneration concerns are merged into `GENERATION_GUIDE.md`; generic agent rules become static `Agents/` assets.

- [ ] **Step 5: Run Core checks**

Run:

```bash
rg -q "^## Universal Generation Contract$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Document Review and Repair$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Assumption Contract$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^## Quality Gates$" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" && \
rg -q "^framework_version: 1.0.0$" "ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md" && \
! rg -i "museum\|booking\|hyperpay\|dar al-funoon" "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" "ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md"
```

Expected: exit code `0`.

### Task 2: Define Placeholder Grammar, Generation/Regeneration Rules, and Validation Policy

**Files:**
- Create: `ProjectBootstrapFramework/PLACEHOLDER_SPEC.md`
- Create: `ProjectBootstrapFramework/GENERATION_GUIDE.md`
- Create: `ProjectBootstrapFramework/VALIDATION.md`
- Create: `ProjectBootstrapFramework/validation/VALIDATION_REPORT.template.md`
- Test: the four files above

**Interfaces:**
- Consumes: `FRAMEWORK_SPEC.md`, `FRAMEWORK_MANIFEST.md`
- Produces: the placeholder syntax used by every template; the only canonical dependency/impact rules; validation rule IDs and report structure used by prompts and review prompts.

- [ ] **Step 1: Write the failing documentation contract check**

Run:

```bash
rg -q "^## Placeholder Syntax$" "ProjectBootstrapFramework/PLACEHOLDER_SPEC.md" && \
rg -q "^## Requirement Intake$" "ProjectBootstrapFramework/GENERATION_GUIDE.md" && \
rg -q "^## Dependency Edge List$" "ProjectBootstrapFramework/GENERATION_GUIDE.md" && \
rg -q "^## Mechanical Validation$" "ProjectBootstrapFramework/VALIDATION.md" && \
rg -q "^## Quality Review Findings$" "ProjectBootstrapFramework/validation/VALIDATION_REPORT.template.md"
```

Expected: fail because these files do not exist.

- [ ] **Step 2: Create `PLACEHOLDER_SPEC.md`**

Define angle-bracket placeholders with Title Case tokens and optional qualified fields:

```markdown
<Project Name>
<Primary Entity>
<Role>
<Business Rule: BR-001>
<API Resource>
<Requirement: RQ-001>
```

Specify that placeholders express template slots only, never defaults; every placeholder has an allowed source (`Requirements`, a named upstream canonical document, or an explicit user-approved assumption); unresolved placeholders are validation errors in final documents; placeholder nesting is forbidden; IDs are not placeholders.

- [ ] **Step 3: Create `GENERATION_GUIDE.md`**

Include:

```markdown
## Requirement Intake
Normalize each source requirement into RQ-* with a source locator and one status:
confirmed, blocking_question, provisional_assumption, user_approved_assumption,
or derived_conclusion.

## Default Pipeline
1. Initialize TRACEABILITY.md with the requirement register.
2. Generate PROJECT.md.
3. Generate BUSINESS_RULES.md.
4. Generate DECISIONS.md.
5. Generate API.md.
6. Generate REFERENCE.md.
7. Generate TESTING.md.
8. Generate FEATURE_CHECKLIST.md.
9. Enrich and finalize TRACEABILITY.md.
```

Define the explicit dependency edge list and change classification:

```markdown
scope change → PROJECT, affected BUSINESS_RULES, TRACEABILITY
behavior change → BUSINESS_RULES, affected API/TESTING/CHECKLIST, TRACEABILITY
technical decision change → DECISIONS, affected API/REFERENCE/TESTING/CHECKLIST
contract change → API, affected REFERENCE/TESTING/CHECKLIST
writing-only change → changed document only, unless IDs or canonical statements change
```

State safe partial regeneration and full regeneration conditions. Include `Not Applicable` rules: a document retains its schema and says why a section is inapplicable; it never invents an API, database, auth, or architecture to fill a template.

- [ ] **Step 4: Create `VALIDATION.md` and the report template**

Define three mechanical gates:

```markdown
Structural: required metadata and headings exist; IDs are syntactically valid; no unresolved placeholders.
Semantic: references resolve; canonical ownership is not duplicated; required dependency and traceability edges exist; no direct contradiction is found.
Framework: document and Agent Layer versions satisfy FRAMEWORK_MANIFEST.md.
```

Define quality review separately from mechanical validation. Quality findings use `blocker`, `major`, `minor`, or `note`; `blocker` and `major` require repair before final output. A repair always triggers all three mechanical gates again.

`VALIDATION_REPORT.template.md` must contain sections for run inputs, version checks, mechanical failures, quality review findings, repairs applied, blocking questions, affected regeneration scope, and final status. It is run output, not a project knowledge-base authority.

- [ ] **Step 5: Run policy checks**

Run:

```bash
rg -q "^## Placeholder Syntax$" "ProjectBootstrapFramework/PLACEHOLDER_SPEC.md" && \
rg -q "^## Requirement Intake$" "ProjectBootstrapFramework/GENERATION_GUIDE.md" && \
rg -q "^## Dependency Edge List$" "ProjectBootstrapFramework/GENERATION_GUIDE.md" && \
rg -q "^## Mechanical Validation$" "ProjectBootstrapFramework/VALIDATION.md" && \
rg -q "^## Quality Review Findings$" "ProjectBootstrapFramework/validation/VALIDATION_REPORT.template.md" && \
! rg -i "museum\|booking\|hyperpay\|dar al-funoon" "ProjectBootstrapFramework/PLACEHOLDER_SPEC.md" "ProjectBootstrapFramework/GENERATION_GUIDE.md" "ProjectBootstrapFramework/VALIDATION.md"
```

Expected: exit code `0`.

### Task 3: Build the Eight Generic Project Knowledge-Base Templates

**Files:**
- Create: `ProjectBootstrapFramework/templates/PROJECT.template.md`
- Create: `ProjectBootstrapFramework/templates/BUSINESS_RULES.template.md`
- Create: `ProjectBootstrapFramework/templates/DECISIONS.template.md`
- Create: `ProjectBootstrapFramework/templates/API.template.md`
- Create: `ProjectBootstrapFramework/templates/REFERENCE.template.md`
- Create: `ProjectBootstrapFramework/templates/TESTING.template.md`
- Create: `ProjectBootstrapFramework/templates/FEATURE_CHECKLIST.template.md`
- Create: `ProjectBootstrapFramework/templates/TRACEABILITY.template.md`
- Test: all eight templates

**Interfaces:**
- Consumes: `FRAMEWORK_SPEC.md`, `PLACEHOLDER_SPEC.md`, `GENERATION_GUIDE.md`
- Produces: the only source skeletons that generators populate, each with stable metadata and canonical ID conventions.

- [ ] **Step 1: Write the failing template coverage check**

Run:

```bash
for file in PROJECT BUSINESS_RULES DECISIONS API REFERENCE TESTING FEATURE_CHECKLIST TRACEABILITY; do
  test -f "ProjectBootstrapFramework/templates/${file}.template.md" || exit 1
  rg -q "^---$" "ProjectBootstrapFramework/templates/${file}.template.md" || exit 1
done
```

Expected: fail because templates do not exist.

- [ ] **Step 2: Create the metadata contract in every template**

Use this exact stable metadata shape, replacing values with placeholders:

```yaml
---
document_id: <Document ID>
document_schema_version: 1.0
framework_version: <Framework Version>
upstream_documents:
  - <Upstream Document ID>
source_requirements:
  - <Requirement: RQ-001>
---
```

Do not include automatic timestamps, “approved” status, content hashes, or generated-run status.

- [ ] **Step 3: Create document-specific section skeletons**

Use the following required ownership boundaries:

```markdown
PROJECT: Overview; Vision; Goals; Actors; Features (FEAT-*); Domain Concepts; Scope; Non-Goals; Constraints.
BUSINESS_RULES: one domain/flow section per owned rule; Purpose; Rules (BR-*); Permissions; Relationships; Lifecycle; State Changes; Edge Cases; Assumptions.
DECISIONS: one ADR-* per accepted technical decision; Decision; Context; Evidence; Alternatives; Trade-offs; Consequences.
API: API philosophy; conventions; access model only if requirements establish it; response/error contracts; resource contracts (API-*); compatibility rules.
REFERENCE: implementation patterns (REF-*); construction order; boundaries; error/validation/testing patterns only when supported by upstream documents.
TESTING: philosophy; test obligations (TEST-*); rule/contract coverage; deterministic testing; test organization; quality criteria.
FEATURE_CHECKLIST: planning; applicable architecture/contract/behavior/security/test/documentation checks (FC-*); completion gate.
TRACEABILITY: requirement register (RQ-*); relationship table; unresolved items; impact index; canonical owner index.
```

Mark optional sections with `## Optional — Include Only When Supported by Upstream Evidence`; no template can imply an implementation architecture.

- [ ] **Step 4: Run template validation**

Run:

```bash
for file in PROJECT BUSINESS_RULES DECISIONS API REFERENCE TESTING FEATURE_CHECKLIST TRACEABILITY; do
  rg -q "document_schema_version: 1.0" "ProjectBootstrapFramework/templates/${file}.template.md" || exit 1
done
! rg -i "museum\|booking\|hyperpay\|dar al-funoon\|content editor" "ProjectBootstrapFramework/templates"
```

Expected: exit code `0`.

### Task 4: Create Shared Generator Prompts and Per-Document Quality Review Prompts

**Files:**
- Create: `ProjectBootstrapFramework/prompts/GENERATION_PREAMBLE.md`
- Create: `ProjectBootstrapFramework/prompts/01_generate_project.md`
- Create: `ProjectBootstrapFramework/prompts/02_generate_business_rules.md`
- Create: `ProjectBootstrapFramework/prompts/03_generate_decisions.md`
- Create: `ProjectBootstrapFramework/prompts/04_generate_api.md`
- Create: `ProjectBootstrapFramework/prompts/05_generate_reference.md`
- Create: `ProjectBootstrapFramework/prompts/06_generate_testing.md`
- Create: `ProjectBootstrapFramework/prompts/07_generate_feature_checklist.md`
- Create: `ProjectBootstrapFramework/prompts/08_generate_traceability.md`
- Create: `ProjectBootstrapFramework/prompts/reviews/{PROJECT,BUSINESS_RULES,DECISIONS,API,REFERENCE,TESTING,FEATURE_CHECKLIST,TRACEABILITY}.review.md`
- Test: all prompt files

**Interfaces:**
- Consumes: all six Framework Core documents and the templates from Task 3.
- Produces: target-specific generation deltas and review findings with severity, source IDs, remediation action, and an explicit “requires project fact” flag.

- [ ] **Step 1: Write the failing prompt inventory check**

Run:

```bash
test -f "ProjectBootstrapFramework/prompts/GENERATION_PREAMBLE.md" && \
test "$(rg --files "ProjectBootstrapFramework/prompts" -g "*.review.md" | wc -l | tr -d ' ')" = "8" && \
test "$(rg --files "ProjectBootstrapFramework/prompts" -g "0*_generate_*.md" | wc -l | tr -d ' ')" = "8"
```

Expected: fail because prompts do not exist.

- [ ] **Step 2: Create `GENERATION_PREAMBLE.md`**

The preamble must instruct every generator to:

```markdown
Read FRAMEWORK_SPEC.md, FRAMEWORK_MANIFEST.md, PLACEHOLDER_SPEC.md,
GENERATION_GUIDE.md, VALIDATION.md, the target template, requirements, and
listed upstream documents. Follow the Universal Generation Contract. Generate
only target-owned information. Preserve existing stable IDs on regeneration.
Stop for blocking questions. Do not invent project facts, architecture, access
control, workflows, integrations, or technical choices. Emit draft → validate
→ quality review → repair → revalidate → final output.
```

- [ ] **Step 3: Create eight generator deltas**

Each prompt contains only: purpose, target-owned information, required upstream sources, stable-ID namespace, extraction algorithm, target-specific validation, forbidden assumptions, failure conditions, and regeneration behavior.

For example, `02_generate_business_rules.md` must state:

```markdown
Own BR-* only. Derive behavior from confirmed FEAT-* and RQ-* records.
Do not create a permission, state transition, lifecycle, or validation rule
unless a requirement or explicit user-approved assumption supports it. Record
an unresolved requirement instead of completing the rule. Link every applicable
BR-* to its RQ-* and FEAT-* sources in TRACEABILITY.md.
```

`08_generate_traceability.md` must initialize `RQ-*` before `PROJECT.md`, then only enrich links after each later document; it must not create domain facts.

- [ ] **Step 4: Create eight review prompts**

Every review prompt must require findings in this exact form:

```markdown
| Severity | Finding | Evidence | Affected canonical ID | Repair allowed without new fact? | Required action |
|---|---|---|---|---|---|
| major | <finding> | <section or ID> | <ID> | yes/no | <repair or blocking question> |
```

Review criteria must cover single responsibility, canonical ownership, duplication, clarity, ambiguity, scope leakage, unsupported technical detail, invented project knowledge, human readability, agent readability, and open-source-reference quality.

Document-specific reviews additionally check:

```markdown
PROJECT: scope versus implementation leakage.
BUSINESS_RULES: rule/assumption distinction and behavior completeness.
DECISIONS: accepted choice versus unmade recommendation.
API: contract versus implementation detail.
REFERENCE: pattern guidance versus project-specific claim.
TESTING: obligation coverage without test-framework invention.
FEATURE_CHECKLIST: verifiable completion conditions without duplicated rules.
TRACEABILITY: evidence-based edges without fabricated relationships.
```

- [ ] **Step 5: Run prompt checks**

Run:

```bash
rg -q "Universal Generation Contract" "ProjectBootstrapFramework/prompts/GENERATION_PREAMBLE.md" && \
for file in ProjectBootstrapFramework/prompts/reviews/*.review.md; do
  rg -q "Repair allowed without new fact" "$file" || exit 1
done && \
! rg -i "museum\|booking\|hyperpay\|dar al-funoon" "ProjectBootstrapFramework/prompts"
```

Expected: exit code `0`.

### Task 5: Build the Static, Tool-Neutral Agent Layer and Framework Onboarding

**Files:**
- Create: `ProjectBootstrapFramework/Agents/SYSTEM.md`
- Create: `ProjectBootstrapFramework/Agents/IMPLEMENT.md`
- Create: `ProjectBootstrapFramework/Agents/REVIEW.md`
- Create: `ProjectBootstrapFramework/Agents/TEST.md`
- Create: `ProjectBootstrapFramework/Agents/CHANGE.md`
- Create: `ProjectBootstrapFramework/README.md`
- Test: all agent files and README

**Interfaces:**
- Consumes: `FRAMEWORK_SPEC.md`, `FRAMEWORK_MANIFEST.md`, `GENERATION_GUIDE.md`, `VALIDATION.md`
- Produces: reusable instructions that name document inputs and validation obligations but never prescribe a project domain or IDE vendor.

- [ ] **Step 1: Write the failing portability check**

Run:

```bash
for file in SYSTEM IMPLEMENT REVIEW TEST CHANGE; do
  test -f "ProjectBootstrapFramework/Agents/${file}.md" || exit 1
done
rg -q "^## Bootstrap a New Project$" "ProjectBootstrapFramework/README.md"
```

Expected: fail because the Agent Layer and README do not exist.

- [ ] **Step 2: Create the five Agent Layer documents**

`SYSTEM.md` defines universal behavior:

```markdown
Load the applicable Project Knowledge Base documents before acting. Treat their
canonical IDs as authoritative. Stop when required project truth is absent or
contradictory. Do not infer domain facts. Record findings with IDs and hand off
only through the validation report. This instruction is tool-neutral.
```

`IMPLEMENT.md`, `REVIEW.md`, `TEST.md`, and `CHANGE.md` each name a minimal reading order and completion check:

```markdown
IMPLEMENT: PROJECT → BUSINESS_RULES → API → REFERENCE → FEATURE_CHECKLIST.
REVIEW: applicable canonical owner → REFERENCE → FEATURE_CHECKLIST → TESTING.
TEST: BUSINESS_RULES → API → TESTING → TRACEABILITY.
CHANGE: changed canonical owner → GENERATION_GUIDE impact rules → affected documents → VALIDATION_REPORT.
```

All five include the single-writer rule: one agent may modify a canonical artifact per change set; other agents submit ID-addressed findings rather than concurrent edits.

- [ ] **Step 3: Create `README.md`**

Document the six Core files, eight generated knowledge-base documents, Agent Layer, requirement intake, default generation lifecycle including quality review/repair, validation report, regeneration, common mistakes, version upgrades, and a “first project in 30 minutes” walkthrough that uses only generic names such as `<Requirements Document>` and `<Project Name>`.

- [ ] **Step 4: Run onboarding and portability checks**

Run:

```bash
rg -q "tool-neutral" "ProjectBootstrapFramework/Agents/SYSTEM.md" && \
rg -q "one agent may modify a canonical artifact" "ProjectBootstrapFramework/Agents/IMPLEMENT.md" && \
rg -q "^## Bootstrap a New Project$" "ProjectBootstrapFramework/README.md" && \
! rg -i "cursor\|claude code\|museum\|booking\|hyperpay\|dar al-funoon" "ProjectBootstrapFramework/Agents" "ProjectBootstrapFramework/README.md"
```

Expected: exit code `0`.

### Task 6: Freeze V1 and Run Repository-Level Quality Review

**Files:**
- Modify: `ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md`
- Create: `ProjectBootstrapFramework/validation/VALIDATION_REPORT.md`
- Test: all files under `ProjectBootstrapFramework/`

**Interfaces:**
- Consumes: every framework file from Tasks 1–5.
- Produces: V1 freeze declaration and a current validation report that proves final structure, no golden-project leakage, prompt/review coverage, and no duplicate authority.

- [ ] **Step 1: Create the failing final inventory check**

Run:

```bash
test "$(rg --files "ProjectBootstrapFramework/templates" -g "*.template.md" | wc -l | tr -d ' ')" = "8" && \
test "$(rg --files "ProjectBootstrapFramework/prompts/reviews" -g "*.review.md" | wc -l | tr -d ' ')" = "8" && \
test "$(rg --files "ProjectBootstrapFramework/Agents" -g "*.md" | wc -l | tr -d ' ')" = "5" && \
test -f "ProjectBootstrapFramework/validation/VALIDATION_REPORT.md"
```

Expected: fail until the report exists.

- [ ] **Step 2: Produce `VALIDATION_REPORT.md`**

Use the report template to record:

```markdown
Final status: PASS
Framework version: 1.0.0
Structural validation: PASS
Semantic validation: PASS
Framework compatibility: PASS
Quality review: PASS
Blocking questions: none
Known future work: structured executor and plugin architecture are deferred by design.
```

Document any real warning found during review instead of claiming a clean report. Do not include a generated timestamp in this static repository report; it is a checked-in V1 freeze artifact.

- [ ] **Step 3: Add V1 freeze language to the manifest**

Append:

```markdown
## V1 Freeze
Version 1.0.0 is frozen after repository validation. Future architecture changes
require evidence from real project generation, a compatibility assessment, and an
updated validation report. The framework does not add new Core concepts merely to
anticipate hypothetical use cases.
```

- [ ] **Step 4: Run final mechanical checks**

Run:

```bash
git diff --check && \
test "$(rg --files "ProjectBootstrapFramework/templates" -g "*.template.md" | wc -l | tr -d ' ')" = "8" && \
test "$(rg --files "ProjectBootstrapFramework/prompts/reviews" -g "*.review.md" | wc -l | tr -d ' ')" = "8" && \
test "$(rg --files "ProjectBootstrapFramework/Agents" -g "*.md" | wc -l | tr -d ' ')" = "5" && \
rg -q "Final status: PASS" "ProjectBootstrapFramework/validation/VALIDATION_REPORT.md" && \
! rg -i "dar al-funoon\|hyperpay\|content editor\|store & bookings manager" \
  "ProjectBootstrapFramework/FRAMEWORK_SPEC.md" \
  "ProjectBootstrapFramework/FRAMEWORK_MANIFEST.md" \
  "ProjectBootstrapFramework/PLACEHOLDER_SPEC.md" \
  "ProjectBootstrapFramework/GENERATION_GUIDE.md" \
  "ProjectBootstrapFramework/VALIDATION.md" \
  "ProjectBootstrapFramework/README.md" \
  "ProjectBootstrapFramework/templates" \
  "ProjectBootstrapFramework/prompts" \
  "ProjectBootstrapFramework/Agents"
```

Expected: exit code `0`. If a prohibited term appears only in `ANALYSIS.md` or `FRAMEWORK_EXTRACTION.md`, narrow the leakage scan to normative Core, templates, prompts, and Agents rather than deleting source-analysis evidence.

- [ ] **Step 5: Perform final quality review**

Read every normative Core document, each template, each generator prompt, each review prompt, and all five Agent Layer documents. Confirm:

```markdown
- Every authoritative rule appears in one canonical document.
- Review prompts critique and repair; they do not generate facts.
- The repair loop revalidates mechanically.
- All future-scale features retained in V1 solve an identified multi-project problem.
- Deferred features are documented as deferred, not accidentally half-designed.
- A human can bootstrap a project by following README.md without prior chat context.
```

Expected: no blocker or major finding remains. If a finding needs a project-specific fact, leave it as a framework rule rather than manufacturing an example.

## Plan Self-Review

- **Spec coverage:** Task 1 implements the three-layer constitution, versioning, canonical ownership, assumptions, metadata, and the new review/repair contract. Task 2 implements intake, dependencies, regeneration, validation, and report output. Task 3 creates all eight generated-artifact templates. Task 4 supplies eight generators and eight reviewers. Task 5 implements the tool-neutral Agent Layer and onboarding. Task 6 freezes V1 and validates the full framework.
- **No-placeholder scan:** this plan contains no unfinished markers and every created file has an explicit responsibility and acceptance command.
- **Interface consistency:** `FRAMEWORK_SPEC.md` owns universal policy; `FRAMEWORK_MANIFEST.md` owns compatibility; `PLACEHOLDER_SPEC.md` owns syntax; `GENERATION_GUIDE.md` owns flow/dependencies/regeneration; `VALIDATION.md` owns checks; `VALIDATION_REPORT.md` records outcomes. Prompts and Agents reference these owners rather than restating their rules.
