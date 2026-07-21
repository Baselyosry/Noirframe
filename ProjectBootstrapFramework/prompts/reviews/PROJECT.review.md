# Review PROJECT.md

Review `PROJECT.md` against the requirements, `TRACEABILITY.md`, and Framework Core.

Check scope clarity, duplicate authority, ambiguous features, unsupported claims, implementation leakage, missing non-goals, uncertainty labels, readability, and whether the document is useful to both engineers and agents.

Do not generate project facts. For each finding, return:

| Severity | Finding | Evidence | Affected canonical ID | Repair allowed without new fact? | Required action |
|---|---|---|---|---|---|
| major | <finding> | <section or ID> | <ID> | yes/no | <repair or blocking question> |

Repairs may simplify or relocate supported content. A missing fact requires a blocking question or a visible uncertainty record, followed by mechanical revalidation.
