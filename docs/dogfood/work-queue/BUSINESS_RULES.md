---
document_id: BUSINESS_RULES
document_schema_version: 1.0
framework_version: 1.0.0
upstream_documents: [PROJECT, TRACEABILITY]
source_requirements: [RQ-001, RQ-002, RQ-003, RQ-004, RQ-005]
---

# Business Rules

## Work Request Management
### Purpose
Define the supported information and operations for incoming support work requests.

### Rules
- BR-001: Each work request must include a title, short description, requester name, assignee, and exactly one status. The allowed statuses are New, In Progress, and Complete. Source: RQ-002, FEAT-001.
- BR-002: Team members can create work requests, view all work requests, update a work request's assignee, and update a work request's status. Source: RQ-003, FEAT-001.
- BR-003: Supervisors can view a count of work requests grouped by status. Source: RQ-004, FEAT-002.
- BR-004: The first release excludes email notifications, customer accounts, file attachments, and automated assignment. Source: RQ-005, FEAT-003.
- BR-005: Supervisors can export the status summary as a CSV file. Source: RQ-006, FEAT-004.

### Permissions
Team members have the capabilities defined in BR-002. Supervisors have the capability defined in BR-003. No additional permission rules are specified. Source: RQ-003, RQ-004.

### Lifecycle and State Changes
Work requests use the statuses defined in BR-001. Permitted transitions between statuses are not specified. Source: RQ-002.

### Edge Cases and Uncertainty
The requirements do not specify validation details beyond the required fields, how assignees are selected, or any behavior for unavailable assignees.
