---
document_id: PROJECT
document_schema_version: 1.0
framework_version: 1.0.0
upstream_documents:
  - TRACEABILITY
source_requirements:
  - RQ-001
  - RQ-002
  - RQ-003
  - RQ-004
  - RQ-005
---

# Work Queue

## Overview

Work Queue is an internal web tool for a support team to record incoming work requests and assign them to team members.

## Vision

Give the support team one shared view of incoming work and its current status.

## Goals

- Record and assign incoming work requests.
- Make each request's status visible.
- Give supervisors a count of requests by status.

## Actors

### Team Member

- Purpose: manage incoming support work.
- Capabilities: create requests, view requests, update assignees, and update request status.
- Limitations: no additional capabilities are specified.

### Supervisor

- Purpose: understand the current workload.
- Capabilities: view request counts by status.
- Limitations: no additional capabilities are specified.

## Features

### FEAT-001: Request Management

Team members can create, view, assign, and update requests. A request has a title, description, requester name, assignee, and one of the defined statuses. Source: RQ-001, RQ-002, RQ-003.

### FEAT-002: Status Summary

Supervisors can view counts of requests grouped by status. Source: RQ-004.

### FEAT-003: First-Release Scope Exclusions

The first release excludes email notifications, customer accounts, file attachments, and automated assignment. Source: RQ-005.

## Domain Concepts

- Request: an incoming support work item.
- Assignee: the team member responsible for a request.
- Status: New, In Progress, or Complete.

## Non-Goals

- Email notifications.
- Customer accounts.
- File attachments.
- Automated assignment.
