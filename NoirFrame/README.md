# Project Bootstrap Framework

## Purpose

Generate a project engineering knowledge base from requirements while preserving evidence, canonical ownership, explicit uncertainty, human readability, and tool neutrality.

## Use This Framework

You need one source document that describes the project. It can be a PDF, DOCX,
Markdown file, text file, or another format an AI agent can read. Its filename
does not matter: `client-brief.pdf`, `product-spec.md`, and
`discovery-notes.docx` are all valid inputs.

The current framework is AI-guided: an agent reads the source document and
uses the templates and prompts to generate the knowledge base. It is not a
one-command PDF parser.

### 1. Add the source document

Put the source document in your new repository, for example:

```text
my-project/
├── client-brief.pdf
└── ProjectBootstrapFramework/
```

### 2. Create an output folder

Create `docs/knowledge-base/`. The generated documentation belongs there, not
inside the framework folder.

```text
my-project/
├── client-brief.pdf
├── ProjectBootstrapFramework/
└── docs/
    └── knowledge-base/
```

### 3. Give an AI agent the bootstrap instruction

Replace `<source-document>` with the path to your actual file:

```text
Read ProjectBootstrapFramework/README.md, FRAMEWORK_SPEC.md,
FRAMEWORK_MANIFEST.md, PLACEHOLDER_SPEC.md, GENERATION_GUIDE.md, VALIDATION.md,
all templates, and all prompts.

Use <source-document> as the only project-specific source.

Initialize docs/knowledge-base/TRACEABILITY.md from the traceability template.
Assign RQ IDs and source locators. Stop and ask questions for blocking gaps.

Then generate and review, in order:
1. PROJECT.md
2. BUSINESS_RULES.md
3. DECISIONS.md
4. API.md
5. REFERENCE.md
6. TESTING.md
7. FEATURE_CHECKLIST.md
8. TRACEABILITY.md finalization

Use the matching template, generator prompt, and review prompt for every
document. Validate, review, repair only supported issues, then revalidate.

Never invent project facts. Mark unsupported concerns as Not Applicable or
raise a blocking question. Write the output to docs/knowledge-base/.
```

### 4. Review the generated output

The output should contain:

```text
docs/knowledge-base/
├── PROJECT.md
├── BUSINESS_RULES.md
├── DECISIONS.md
├── API.md
├── REFERENCE.md
├── TESTING.md
├── FEATURE_CHECKLIST.md
├── TRACEABILITY.md
└── validation/
    └── VALIDATION_REPORT.md
```

Do not treat a generated file as automatically correct. Review the validation
report, resolve blocking questions with the project stakeholder, then rerun
affected generators.

### 5. Use the knowledge base during development

Give coding agents `Agents/SYSTEM.md` plus the role instruction that matches
their task:

- `IMPLEMENT.md` for feature work;
- `REVIEW.md` for implementation review;
- `TEST.md` for test work;
- `CHANGE.md` for requirement or documentation changes.

Those agents should use your generated `docs/knowledge-base/` files as the
project's source of truth.

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
