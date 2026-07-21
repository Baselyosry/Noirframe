---
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
---

# Framework Manifest

## Compatibility Policy

Patch releases correct wording or validation defects and remain compatible. Minor releases add optional behavior and remain compatible with the current major version. Major releases change required contracts and require a documented project-knowledge-base migration.

This Markdown manifest is canonical. A machine-readable manifest is deferred until a real executor needs one.

## V1 Evidence Rule

New Core concepts require evidence from generating or maintaining real projects: the observed problem, why an existing asset cannot solve it, and the compatibility impact. Hypothetical future use alone is insufficient.

## V1 Freeze

Version 1.0.0 is frozen after repository validation. Future architecture changes require evidence from real project generation, a compatibility assessment, and an updated validation report.
