---
document_id: TRACEABILITY
document_schema_version: 1.0
framework_version: 1.0.0
upstream_documents: []
source_requirements: []
---

# Traceability

## Requirement Register

| ID | Source locator | Status | Summary |
|---|---|---|---|
| RQ-001 | Requirements.md §R1 | confirmed | Record and assign incoming support work. |
| RQ-002 | Requirements.md §R2 | confirmed | Requests have required fields and three statuses. |
| RQ-003 | Requirements.md §R3 | confirmed | Team members create, view, and update requests. |
| RQ-004 | Requirements.md §R4 | confirmed | Supervisors view counts grouped by status. |
| RQ-005 | Requirements.md §R5 | confirmed | Named capabilities are excluded from the first release. |
| RQ-006 | Requirements.md §R6 | confirmed | Supervisors can export the status summary as CSV. |

## Relationships

| Source ID | Relationship | Target ID |
|---|---|---|
| RQ-001 | derived | FEAT-001 |
| RQ-002 | derived | FEAT-001 |
| RQ-003 | derived | FEAT-001 |
| RQ-004 | derived | FEAT-002 |
| RQ-005 | derived | FEAT-003 |
| RQ-001 | derived | REF-001 |
| RQ-002 | derived | REF-001 |
| RQ-003 | derived | REF-001 |
| RQ-004 | derived | REF-002 |
| RQ-005 | derived | REF-003 |
| RQ-001 | verified_by | TEST-001 |
| RQ-002 | verified_by | TEST-001 |
| RQ-003 | verified_by | TEST-001 |
| RQ-004 | verified_by | TEST-002 |
| RQ-005 | verified_by | TEST-003 |
| RQ-001 | completion_checked_by | FC-001 |
| RQ-002 | completion_checked_by | FC-001 |
| RQ-003 | completion_checked_by | FC-001 |
| RQ-004 | completion_checked_by | FC-002 |
| RQ-005 | completion_checked_by | FC-003 |
| RQ-006 | derived | FEAT-004 |
| RQ-006 | derived | BR-005 |
| RQ-006 | derived | REF-004 |
| RQ-006 | verified_by | TEST-004 |
| RQ-006 | completion_checked_by | FC-004 |
| RQ-002 | derived | BR-001 |
| RQ-003 | derived | BR-002 |
| RQ-004 | derived | BR-003 |
| RQ-005 | derived | BR-004 |
