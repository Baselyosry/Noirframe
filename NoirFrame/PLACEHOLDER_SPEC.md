# Placeholder Specification

## Placeholder Syntax

Templates use angle-bracket placeholders in Title Case:

```text
<Project Name>
<Primary Entity>
<Requirement: RQ-001>
<Business Rule: BR-001>
<API Resource>
```

The unqualified form names a value to be supplied. The qualified form links a value to an existing canonical ID. IDs themselves are never placeholders.

## Source Rules

Every placeholder value must come from exactly one source:

1. the requirements document;
2. a named upstream canonical document; or
3. a visible user-approved assumption.

Placeholders do not imply defaults. An unresolved placeholder is valid only in a template or draft and is a structural validation failure in final project documentation.

## Formatting Rules

- Do not nest placeholders.
- Do not place a placeholder inside an identifier.
- Use one placeholder for one semantic value.
- Use a qualified placeholder when a cross-document reference must be preserved.
- Replace or remove optional template sections when requirements provide no evidence for them.
