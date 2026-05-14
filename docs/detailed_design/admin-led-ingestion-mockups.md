# Admin-Led Ingestion Workflow Mockups

## Purpose

This document describes the first-pass product mockup for configuring a governed ingestion from an enterprise source system into Integrity Sentinel.

The workflow expands the high-level architecture path:

```text
Enterprise Source Systems
  -> Data Ingestion Layer
  -> Core Data Stores
  -> Integrity Intelligence Core
  -> AI Investigation Layer
  -> Core Platform API Layer
  -> Investigation Workspace
```

The central product concept is the **Ingestion Policy / Metadata Scope**: a governed contract that defines what data may be ingested, how it is filtered, how it maps to the Integrity Ontology, and how it can be used in downstream investigations.

---

## Primary User

### Tenant Admin

The tenant admin configures the source connection, selects metadata scope, validates governance settings, and schedules ingestion.

### Secondary Users

- Compliance officers review ingestion scope and governance warnings.
- Investigators consume downstream alerts, graph context, and evidence.
- Platform administrators manage connector availability and tenant-level settings.

---

## Screen 1: Connector Catalog

### Goal

Let the tenant admin select an enterprise source system to ingest from.

### Suggested UI

- Connector category tabs:
  - HR / Employee Systems
  - ERP / Finance
  - Procurement
  - Identity & Access
  - Collaboration Metadata
  - Custom Applications
- Connector cards showing:
  - Connector name
  - Data domains
  - Supported sync modes
  - Required permissions
  - Last configured status

### Primary Action

Select connector

### Example Connector

Procurement System

Supported domains:

- Vendors
- Purchase orders
- Invoices
- Approvals
- Payment events
- Request metadata

---

## Screen 2: Source Connection Setup

### Goal

Configure the connection without yet ingesting data.

### Suggested UI Fields

- Source display name
- Connector type
- Environment
- Tenant or business unit
- Authentication method
- Sync mode:
  - Manual import
  - Scheduled batch
  - Event stream
- Connection test status

### Primary Action

Test connection

### Secondary Action

Save as draft

---

## Screen 3: Metadata Discovery

### Goal

Show what entity and event metadata is available from the connected source.

### Suggested UI

- Metadata inventory table
- Entity type summary
- Relationship type summary
- Field sensitivity indicators
- Estimated record counts

### Example Metadata Types

- Vendor
- Invoice
- Purchase order
- Approval
- Approver
- Requestor
- Payment event
- Department
- Cost center

### Primary Action

Continue to scope

---

## Screen 4: Ingestion Scope & Metadata Filters

### Goal

Define what data is allowed into Integrity Sentinel.

### Suggested UI

- Date range selector
- Department filter
- Region filter
- Vendor category filter
- Approval type filter
- Transaction threshold filter
- Include or exclude relationship fields
- Metadata-only toggle
- Sensitive field exclusion list

### Example Filters

```text
Date range: Last 12 months
Departments: Procurement, Finance
Regions: North America
Vendor categories: Professional Services, IT Services
Minimum invoice amount: 5,000
Include approval chains: Yes
Include free-text notes: No
Include attachments: No
```

### Primary Action

Preview scoped data

---

## Screen 5: Preview & Governance Validation

### Goal

Let the admin see what will be ingested before the job runs.

### Suggested UI

- Sample normalized records
- Estimated records by entity type
- Estimated relationships by relationship type
- Excluded fields
- Governance warnings
- Required approval status
- Validation results

### Example Validation Results

- Schema compatible
- Required identifiers present
- Sensitive free-text fields excluded
- Tenant boundary confirmed
- Audit logging enabled
- Evidence retention policy selected

### Primary Action

Approve scope

### Secondary Action

Return to filters

---

## Screen 6: Ontology Mapping

### Goal

Map source fields to Integrity Sentinel concepts.

### Suggested UI

- Source field column
- Detected data type
- Suggested ontology concept
- Required or optional indicator
- Mapping confidence
- Manual override control

### Example Mapping

| Source Field | Integrity Concept | Required |
|---|---|---|
| vendor_id | Vendor.identifier | Yes |
| invoice_id | Transaction.identifier | Yes |
| requester_id | Employee.identifier | Yes |
| approver_id | Approval.actor | Yes |
| invoice_amount | Transaction.amount | Yes |
| approval_timestamp | Event.timestamp | Yes |
| cost_center | OrganizationUnit.identifier | No |

### Primary Action

Validate mapping

---

## Screen 7: Schedule & Run Ingestion

### Goal

Run the ingestion once or configure recurring sync.

### Suggested UI

- Run mode:
  - Run once
  - Scheduled batch
  - Event stream
- Schedule selector
- Backfill range
- Failure handling
- Notification recipients
- Downstream processing toggles:
  - Update graph
  - Run risk scoring
  - Generate alerts
  - Enable AI investigation summaries

### Primary Action

Start ingestion

---

## Screen 8: Ingestion Monitor

### Goal

Show data flowing through the platform.

### Suggested UI

- Stepper or pipeline visualization:
  - Source connected
  - Metadata scoped
  - Records extracted
  - Data normalized
  - Operational database updated
  - Graph database updated
  - Evidence store updated
  - Vector store updated
  - Audit log written
  - Risk scoring completed
  - Alerts created
- Counts by stage
- Error list
- Retry controls
- Link to generated alerts

### Primary Action

Open investigation workspace

---

## Screen 9: Investigation Handoff

### Goal

Move from ingestion setup into the user-facing investigation experience.

### Suggested UI

- New alerts generated
- Highest-risk entities
- New relationship clusters
- Policy exceptions
- AI-generated summaries
- Link to entity relationship explorer
- Link to investigation cases

### Primary Action

Review alerts

---

## Data Flow Notes

### Source to Ingestion

The connector extracts only data allowed by the ingestion policy and metadata filters.

### Ingestion to Core Data Stores

Normalized records are written to appropriate stores:

- Operational database for cases, alerts, tenants, policies, and ingestion state.
- Graph database for entities and relationships.
- Evidence store or data lake for retained operational evidence.
- Vector store for semantic similarity and retrieval.
- Immutable audit log for traceability.

### Core Data Stores to Intelligence Core

The Integrity Intelligence Core uses normalized data for ontology modeling, graph correlation, anomaly detection, policy rules, evidence correlation, and risk scoring.

### Intelligence Core to Investigation

Risk output feeds the Sentinel AI Investigator, Core Platform API, and Investigation Workspace.

---

## Open Product Questions

- Which connector should be mocked first: procurement, HR, identity, or custom CSV import?
- Should ingestion approval require a compliance officer before the first run?
- Should free-text fields be excluded by default?
- Should attachments be stored in the evidence lake during the prototype phase?
- Should ontology mapping be required before preview, or should preview happen first?
- Should risk scoring run automatically after ingestion or require explicit admin approval?

