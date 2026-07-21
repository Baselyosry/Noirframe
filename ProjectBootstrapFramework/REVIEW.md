# Project Bootstrap Framework V1 — Independent Architecture Review

## Scope and Method

Reviewed all 40 Markdown artifacts under `ProjectBootstrapFramework/`, plus the implemented Work Queue dogfood slice. This review evaluates the implementation, not the intended design. “V1 freeze” and `PASS` claims were treated as claims requiring evidence.

## Perspective Scores

| Perspective | Score | Rationale |
|---|---:|---|
| Simplicity | 5/10 | The Core is compact, but templates, prompts, review prompts, reports, and IDs add process without an executable enforcement layer. |
| Extensibility | 5/10 | Namespaces and layers help, but no document contract or extension boundary is operational. |
| Maintainability | 4/10 | Rules are duplicated between specification, guide, prompts, and review prompts; drift has already occurred. |
| Reusability | 7/10 | Core, templates, and agent instructions are domain-neutral; source-analysis artifacts are appropriately separate. |
| Determinism | 3/10 | The specification permits prose variation and no implementation calculates, validates, or persists deterministic generation inputs. |
| Cognitive Load | 4/10 | A user must interpret five Core documents, templates, prompts, reviews, and reports before producing a second document. |
| Traceability | 4/10 | The intended model is sound but only the first project document was dogfooded and link rules are not enforceable. |
| Human Readability | 6/10 | Core files are concise; the generated-document skeletons are too sparse to guide a reader consistently. |
| Agent Readability | 5/10 | Short instructions are easy to parse but omit required context, ordering, and failure behavior. |
| Long-term Scalability | 5/10 | The architecture could scale, but evidence from one partial dogfood slice does not justify a frozen V1. |

## What Is Strong

- The three-layer separation is useful and addresses a real reuse problem: static method, generated project truth, and static agent behavior can evolve independently.
- Canonical ownership and explicit uncertainty are appropriate safeguards against invented project knowledge.
- The quality-review/repair distinction is valuable. It prevents a structural pass from being mistaken for an engineer-readable result.
- Core, templates, prompts, and Agents are free of reference-project domain leakage. `ANALYSIS.md` and `FRAMEWORK_EXTRACTION.md` retain source-analysis evidence without becoming normative policy.
- The Work Queue dogfood slice exposed and repaired a real duplication issue, demonstrating that review/repair is more than an abstract concept.

## Findings

### F-001 — V1 is declared frozen without end-to-end evidence

**Severity:** Critical  
**Location:** `FRAMEWORK_MANIFEST.md` §V1 Freeze; `validation/VALIDATION_REPORT.md`; `docs/dogfood/work-queue/`  
**Problem:** The framework claims V1 freeze and `PASS`, but dogfooding generated only `PROJECT.md` and a hand-authored traceability record. The seven remaining generated documents, their reviews, repair paths, dependency edges, and regeneration paths have not been exercised together.  
**Why it is a problem:** The stated V1 success criterion is bootstrapping a coherent knowledge base for an unrelated project without framework changes. Current evidence proves one document template, not the pipeline.  
**Suggested solution:** Remove the freeze claim and label the current state “vertical-slice validated.” Dogfood all eight project documents from one unrelated requirements set, run every generator/review prompt, introduce at least one upstream change, and verify the documented regeneration impact before freezing V1.  
**Tradeoffs:** Delays the freeze and exposes design defects sooner.  
**Expected benefit:** The V1 label becomes evidence-backed rather than aspirational.

### F-002 — “Mechanical validation” is specified but not implemented

**Severity:** Critical  
**Location:** `VALIDATION.md`; `validation/VALIDATION_REPORT.md`; all templates and prompts  
**Problem:** The framework defines ID syntax, reference resolution, ownership uniqueness, dependency checks, placeholder checks, and version checks, but provides no validator procedure capable of performing them. The repository-level verification only counts files and scans forbidden words.  
**Why it is a problem:** A validation policy is not validation. Users will produce reports manually, inconsistently, and may mark documents passed without checking the claims that matter.  
**Suggested solution:** For V1, add a precise, tool-neutral validation checklist with input/output examples and explicit manual algorithms for each rule; alternatively add a small Markdown-oriented validator only after the full dogfood run proves manual validation too error-prone. Do not retain a “PASS” report that asserts unperformed semantic checks.  
**Tradeoffs:** A manual algorithm increases documentation; an executable validator adds implementation and maintenance cost.  
**Expected benefit:** Validation claims become repeatable and auditable.

### F-003 — Generator prompts do not implement the advertised contract

**Severity:** High  
**Location:** `prompts/02_generate_business_rules.md` through `08_generate_traceability.md`; compared with `FRAMEWORK_SPEC.md` §Universal Generation Contract  
**Problem:** Seven generator prompts are one sentence each. They omit inputs, outputs, upstream dependencies, generation algorithm, draft/review/repair handoff, validation, failure conditions, and regeneration instructions. Only `01_generate_project.md` contains a partial version of these requirements.  
**Why it is a problem:** Different agents will invent their own execution semantics. This directly conflicts with the universal deterministic contract and creates prompt drift from the first real use.  
**Suggested solution:** Keep one shared preamble, but require every generator delta to declare: owned IDs, required inputs, permitted evidence, output sections, required traceability edges, stop conditions, target-specific validation, and change-impact behavior.  
**Tradeoffs:** Prompts become longer, but they become executable instructions rather than labels.  
**Expected benefit:** Reliable, comparable generation across all document types.

### F-004 — Review-prompt contract is inconsistent and duplicated

**Severity:** High  
**Location:** `prompts/reviews/PROJECT.review.md`; `BUSINESS_RULES.review.md`; `DECISIONS.review.md`; remaining review prompts  
**Problem:** `PROJECT.review.md` defines a six-column finding table. `BUSINESS_RULES.review.md` and `DECISIONS.review.md` reference the PROJECT table rather than defining the required output, while the other five prompts use prose with no table at all.  
**Why it is a problem:** Review output cannot be aggregated into `VALIDATION_REPORT.md`, repairs cannot be reliably triaged, and a reviewer may omit the “repair allowed without new fact” decision that prevents invention.  
**Suggested solution:** Move the common finding schema to `VALIDATION.md` or a single review-prompt preamble. Each document-specific review prompt should contain only additional checks.  
**Tradeoffs:** One shared asset adds an indirection, but removes eight duplicated and inconsistent formats.  
**Expected benefit:** Uniform review evidence and safer repair behavior.

### F-005 — Static validation report is confused with a generation-run artifact

**Severity:** High  
**Location:** `VALIDATION.md` §Validation Report; `validation/VALIDATION_REPORT.template.md`; `validation/VALIDATION_REPORT.md`; Agent instructions  
**Problem:** The specification says every generation run emits a report, but the framework repository includes one frozen report that claims `PASS`. It is unclear whether a new project writes beside the framework, overwrites this report, or stores project-local reports. Agents are told to use “the validation report” without a location or freshness rule.  
**Why it is a problem:** A stale report can be mistaken for current evidence and the static framework becomes coupled to a particular validation run.  
**Suggested solution:** Keep only `VALIDATION_REPORT.template.md` in the framework. Define the generated report location as `<Project Knowledge Base>/validation/VALIDATION_REPORT.md`, require a source revision or run identifier, and make Agents reject stale or absent reports.  
**Tradeoffs:** Removes a convenient visible success artifact.  
**Expected benefit:** Clear report ownership and no false confidence from stale output.

### F-006 — Template schemas conflict with the “Not Applicable” policy

**Severity:** High  
**Location:** `templates/API.template.md`; `templates/DECISIONS.template.md`; `templates/REFERENCE.template.md`; `GENERATION_GUIDE.md` §Not Applicable  
**Problem:** API requires `API-001`, Decisions requires `ADR-001`, and Reference requires `REF-001` even when requirements establish no API, technical decision, or implementation pattern. The guide says such concerns must be marked Not Applicable instead of invented.  
**Why it is a problem:** The templates pressure agents to invent content solely to satisfy structure, violating the central non-invention principle.  
**Suggested solution:** Make the first item in each conditional document an explicit `## Applicability` section. Include “If Applicable” item skeletons only after the applicability decision. Define whether an empty document has no IDs or a document-level applicability ID.  
**Tradeoffs:** Slightly more template prose; less visually uniform output.  
**Expected benefit:** Templates reinforce rather than contradict evidence-driven generation.

### F-007 — Traceability is both bootstrap input and final derived output without lifecycle boundaries

**Severity:** High  
**Location:** `GENERATION_GUIDE.md` §§Requirement Intake and Default Pipeline; `templates/TRACEABILITY.template.md`; `FRAMEWORK_SPEC.md` §Canonical Ownership  
**Problem:** `TRACEABILITY.md` must exist first to own `RQ-*`, then is “enriched and finalized” after all documents. The framework does not define draft/final sections, allowed mutations, stable relationship types, or what validation may rely on before finalization.  
**Why it is a problem:** This hidden two-phase state makes dependencies ambiguous and can create circular operational behavior: generators require a traceability artifact whose downstream links do not yet exist.  
**Suggested solution:** Explicitly split the file into immutable Requirement Register and append-only Derived Links/Impact Index sections. Define which sections are valid before and after each pipeline step, and validate only available links at each stage.  
**Tradeoffs:** Adds lifecycle detail to one document.  
**Expected benefit:** Removes ambiguity without adding another artifact.

### F-008 — Dependency and regeneration rules are incomplete and not ID-aware

**Severity:** High  
**Location:** `GENERATION_GUIDE.md` §§Dependency Edge List and Regeneration Impact  
**Problem:** Rules operate at document type level (“behavior → rules, API/testing/checklist”) but traceability is intended to support selective regeneration. There is no algorithm for mapping a changed `RQ-*`, `FEAT-*`, or `BR-*` to affected IDs, no decision for deleted IDs, and no handling of newly added requirements.  
**Why it is a problem:** The framework promises minimum affected scope but can only recommend broad document-level regeneration. This is a gap between traceability and regeneration.  
**Suggested solution:** State V1 honestly as document-level impact analysis, using traceability to identify sections for human review. Postpone ID-level automatic regeneration until a real executor exists. Define add/change/deprecate ID semantics now.  
**Tradeoffs:** Reduces the apparent precision of V1.  
**Expected benefit:** Achievable and reliable regeneration guidance.

### F-009 — Metadata contract is underspecified and internally inconsistent

**Severity:** Medium  
**Location:** `FRAMEWORK_SPEC.md` §Metadata; all templates  
**Problem:** The spec requires upstream documents and source requirement IDs; templates vary in list syntax, contain placeholder values that are not described as valid YAML states, and omit an input-source revision despite README and generation policy relying on requirements provenance. `TRACEABILITY.template.md` lists no upstream source despite owning normalized input provenance.  
**Why it is a problem:** Metadata cannot be compared or validated consistently, and a changed source file may be indistinguishable from an unchanged one.  
**Suggested solution:** Define one exact metadata schema, including `requirements_source` and a stable source revision/fingerprint supplied by the operator. Define empty list syntax and when `source_requirements` is allowed to be empty.  
**Tradeoffs:** Adds two stable fields and a small schema table.  
**Expected benefit:** Better provenance without noisy timestamps.

### F-010 — “Deterministic policy” is weakened by allowing unconstrained prose variance

**Severity:** Medium  
**Location:** `FRAMEWORK_SPEC.md` §Universal Generation Contract  
**Problem:** Equivalent inputs are said to produce equivalent claims and outcomes, but prose may vary. This makes diffs, review, and regeneration non-deterministic in practice because wording can alter perceived scope and quality-review findings.  
**Why it is a problem:** The framework uses Markdown as its primary output; semantic-equivalence claims are not mechanically verifiable.  
**Suggested solution:** Define deterministic invariants for V1: stable metadata, IDs, headings, source links, relationship types, and canonical claims. Allow prose variation only inside explanatory paragraphs that cannot introduce claims.  
**Tradeoffs:** Constrains generator style.  
**Expected benefit:** More stable reviews and predictable regeneration.

### F-011 — Agent instructions omit necessary sources and operational rules

**Severity:** Medium  
**Location:** `Agents/IMPLEMENT.md`, `Agents/REVIEW.md`, `Agents/CHANGE.md`  
**Problem:** IMPLEMENT omits `DECISIONS.md`, `TESTING.md`, and `TRACEABILITY.md`; REVIEW prescribes `REFERENCE.md` even when not applicable; CHANGE relies on a guide that lacks ID-level impact rules. None specifies how to discover required documents, verify manifest compatibility, or handle a missing validation report.  
**Why it is a problem:** Agents can make implementation choices contrary to accepted decisions or miss traceability/verification obligations. Tool neutrality does not require underspecification.  
**Suggested solution:** Add a common Agent preamble: discover manifest, inspect document applicability, load only applicable canonical owners, verify report freshness, stop on incompatibility, and record a handoff. Keep role-specific files as deltas.  
**Tradeoffs:** Slightly longer agent instructions.  
**Expected benefit:** Better agent reliability without IDE coupling.

### F-012 — Identifier system is valuable but over-applied in sparse documents

**Severity:** Medium  
**Location:** `FRAMEWORK_SPEC.md` §Canonical Ownership; `templates/API.template.md`, `REFERENCE.template.md`, `TESTING.template.md`, `FEATURE_CHECKLIST.template.md`  
**Problem:** Every type of statement receives a first-class namespace even when a project may have a single document-level convention. For example, a testing philosophy paragraph does not need `TEST-*`, and an implementation reference may have no discrete patterns.  
**Why it is a problem:** It increases authoring burden and encourages artificial atomization, reducing human readability for small projects.  
**Suggested solution:** Retain IDs for traceable, changeable obligations and decisions. Allow document-level sections without IDs for stable explanatory conventions; require IDs only for items that participate in a traceability edge or regeneration decision.  
**Tradeoffs:** Validation must distinguish addressable obligations from explanatory prose.  
**Expected benefit:** Progressive complexity for small projects.

### F-013 — Quality gate is subjective but has no termination or escalation policy

**Severity:** Medium  
**Location:** `FRAMEWORK_SPEC.md` §§Document Review and Repair and Quality Gates; `VALIDATION.md` §Quality Review  
**Problem:** Major findings require repair, but the framework does not define when review loops stop, how a disputed quality finding is resolved, or whether a quality finding that needs a stakeholder decision blocks finalization.  
**Why it is a problem:** Agents can loop indefinitely rewriting prose or produce a “PASS” after subjective disagreement.  
**Suggested solution:** Permit one automatic repair/review iteration; then convert unresolved major findings into a blocking question or explicit exception requiring human approval. Treat missing project facts as blocking immediately.  
**Tradeoffs:** May leave a document with an approved exception.  
**Expected benefit:** Bounded execution and clear human escalation.

### F-014 — Validation report’s PASS claim overstates performed review

**Severity:** Medium  
**Location:** `validation/VALIDATION_REPORT.md` §§Mechanical Validation and Final Status  
**Problem:** The report states semantic and framework passes based on file presence and high-level inspection, while the implemented validator does not inspect ownership, resolved references, or compatibility.  
**Why it is a problem:** It undermines trust in the framework’s own audit trail.  
**Suggested solution:** Replace the report with a transparent “partial verification” result until F-002 is addressed, or attach evidence for every claimed check.  
**Tradeoffs:** Less polished release documentation.  
**Expected benefit:** Honest validation provenance.

### F-015 — Template detail is insufficient to preserve the golden documentation’s quality

**Severity:** Medium  
**Location:** all templates, especially `FEATURE_CHECKLIST.template.md` and `REFERENCE.template.md`  
**Problem:** Templates provide headings but little guidance on content granularity, cross-reference expectations, optionality, examples of good versus prohibited entries, or how to distinguish facts from explanations.  
**Why it is a problem:** The framework claims to preserve documentation quality but yields highly variable outputs depending on generator interpretation.  
**Suggested solution:** Add concise, domain-neutral author notes inside templates or generator prompts for required sections, evidence expectations, and common failure modes. Do not add domain examples.  
**Tradeoffs:** More template text.  
**Expected benefit:** More consistent generated knowledge bases.

## Documents or Concepts to Merge

1. Merge the common review finding schema into `VALIDATION.md` or a `REVIEW_PREAMBLE.md`; remove duplicated/inconsistent schemas from individual review prompts.
2. Merge static validation-report “PASS” evidence out of the framework package; retain the report template only.
3. Keep `ANALYSIS.md` and `FRAMEWORK_EXTRACTION.md` separate from normative Core, as currently implemented. They serve different audiences: evidence versus extracted method.

## Documents or Concepts to Split

1. Split `TRACEABILITY.md` internally into Requirement Register and Derived Links/Impact Index lifecycle sections; do not create a new top-level artifact.
2. Split generator prompts conceptually into shared contract, shared required-output schema, and small document-specific delta. Current single-sentence deltas are too thin.

## Missing Capabilities

- An explicit project-local output layout for generated documents and validation reports.
- A requirement-source revision or fingerprint policy.
- A manual validation procedure or evidence-based executable validator.
- Applicability handling that prevents forced `ADR-001`, `API-001`, and `REF-001`.
- ID deprecation/deletion semantics.
- A bounded quality-review escalation rule.
- A complete dogfood run covering all eight documents and at least one regeneration scenario.

## Final Verdict

The implementation has a sound conceptual nucleus but is not ready for an open-source V1 freeze. The most important action is not to add more architecture: complete one end-to-end dogfood pipeline, make validation claims executable or explicitly manual, and align templates/prompts/reviews with the specification. After those corrections, the framework can remain lightweight while becoming credible and maintainable.
