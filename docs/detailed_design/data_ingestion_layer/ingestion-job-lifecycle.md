# Ingestion Job Lifecycle

## Purpose

This document defines how Integrity Sentinel turns an activated tenant connector into a governed ingestion run.

The lifecycle begins after a Tenant Admin activates a connector instance in `enterprise_source_systems` and ends when normalized data has been written to core stores, downstream processing has been triggered, and the run is visible in ingestion history.

---

## Lifecycle Summary

```text
Activated Tenant Connector
  -> Run Requested
  -> Preflight Validation
  -> Source Extraction
  -> Policy Enforcement
  -> Schema & Mapping Validation
  -> Normalization
  -> Entity Resolution Preparation
  -> Store Writes
  -> Downstream Processing
  -> Run Finalization
  -> Ingestion History & Audit
```

---

## Mockup Alignment

This lifecycle follows the admin-led ingestion flow defined in `../enterprise_source_systems/admin-led-ingestion-mockups.md`.

The Enterprise Source Systems design owns connector selection, connection setup, metadata discovery, scope definition, governance preview, ontology mapping, and scheduling. The Data Ingestion Layer begins when the admin starts a run, schedules a run, or activates a connector schedule.

| Admin Mockup Screen | Data Ingestion Layer Responsibility |
|---|---|
| Screen 1: Connector Catalog | No ingestion run is created. The ingestion layer later receives the selected `tenant_connector_instance`. |
| Screen 2: Source Connection Setup | No extraction occurs. Connection status and credential metadata are later checked during preflight. |
| Screen 3: Metadata Discovery | Discovery snapshots are used as preflight and validation inputs for future ingestion runs. |
| Screen 4: Ingestion Scope & Metadata Filters | Approved scope becomes the ingestion policy used by policy enforcement. |
| Screen 5: Preview & Governance Validation | Preview estimates become expected counts and validation expectations for the run. |
| Screen 6: Ontology Mapping | Accepted mappings drive schema validation and normalization. |
| Screen 7: Schedule & Run Ingestion | Creates manual, scheduled, backfill, or event-stream ingestion runs. |
| Screen 8: Ingestion Monitor | Reads ingestion run, stage, count, error, quarantine, retry, and downstream processing status. |
| Screen 9: Investigation Handoff | Receives alerts, graph updates, evidence context, AI summaries, and links from downstream processing. |

The same alignment applies to the clickable prototype under `../enterprise_source_systems/screen_mockups/admin_configuration_workflow/`.

---

## Primary Actors

### Tenant Admin

- Starts a run manually or configures a schedule.
- Reviews ingestion history, stage counts, failures, and quarantine records.
- Retries failed runs or failed stages when permitted.

### Data Owner

- Reviews policy or evidence-retention warnings.
- Approves or rejects ingestion scopes that require business approval.

### Ingestion Worker

- Executes extraction, validation, normalization, store writes, retries, and run finalization.

### Integrity Intelligence Core

- Consumes normalized records and relationships for graph updates, evidence correlation, feature extraction, rules, anomaly detection, and risk scoring.

---

## Run Triggers

| Trigger | Description | Example |
|---|---|---|
| Manual run | Tenant Admin starts a connector run from the connector instance detail screen. | Run Workday now after updating scope. |
| Scheduled batch | Platform scheduler creates a run from `tenant_connector_schedule`. | Daily Workday sync at 2:00 AM PT. |
| Backfill | A bounded historical run created during initial setup or scope change. | Backfill Award Nominations for FY26. |
| Event stream | Connector receives events continuously and groups them into processing windows. | Future identity or access-event connector. |
| Migration validation run | Run created to validate a connector manifest change before full activation. | New Workday lifecycle field in manifest v1.1.0. |

---

## Run States

| State | Meaning |
|---|---|
| Queued | Run has been created and is waiting for worker capacity. |
| Preflight | Credentials, connector status, scope approval, mapping, and schedule constraints are being checked. |
| Extracting | Records or events are being read from the source system. |
| Validating | Extracted records are being checked against policy, schema, mappings, and tenant boundary rules. |
| Normalizing | Source payloads are being converted into canonical Integrity records, relationships, and evidence references. |
| Writing | Normalized data is being written to operational, graph, evidence, vector, and audit stores. |
| Processing | Downstream graph, evidence, risk, alert, and AI-summary work has been triggered. |
| Completed | Run completed within configured failure thresholds. |
| CompletedWithWarnings | Run completed but warnings, skipped records, or quarantined records need review. |
| Failed | Run did not complete and requires retry, configuration change, or operator intervention. |
| Canceled | Run was canceled by an admin or platform control. |

---

## Lifecycle Stages

### 1. Run Requested

The ingestion layer creates an ingestion run from an active `tenant_connector_instance`.

Inputs:

- Tenant connector instance.
- Connector manifest version.
- Approved tenant connector scope.
- Tenant connector mapping.
- Tenant connector schedule.
- Credential metadata.

Outputs:

- New ingestion run record.
- Initial audit event.
- Stage records for the expected run plan.

### 2. Preflight Validation

Preflight prevents unsafe or impossible runs before any source data is extracted.

Validation checks:

- Connector instance is Active.
- Credential status is Valid.
- Connector manifest version is supported.
- No blocking connector migration task is pending.
- Ingestion scope is approved.
- Required mappings are accepted.
- Tenant boundary is present.
- Schedule is allowed by tenant policy.
- Downstream processing toggles are compatible with scope.

Failure handling:

- Missing secrets: fail before extraction.
- Expired credentials: fail before extraction.
- Pending approval: block run and surface status to admin.
- Blocking migration task: block run until migration review is complete.

### 3. Source Extraction

The connector reads only data that is allowed by the approved ingestion scope.

Extraction modes:

| Mode | Behavior |
|---|---|
| Manual import | Reads a tenant-provided file or upload package. |
| Scheduled batch | Reads from source APIs, reports, exports, SFTP paths, or provider batch interfaces. |
| Event stream | Reads provider events or webhooks into bounded processing windows. |

Extraction outputs:

- Raw source records staged for validation.
- Source cursor, watermark, or file manifest.
- Extraction counts by source object.
- Source-level errors.

### 4. Policy Enforcement

Policy enforcement happens before normalization writes any durable business records.

Rules:

- Excluded objects are ignored.
- Excluded fields are dropped.
- Redacted fields are replaced before persistence.
- Hashed fields are hashed before persistence.
- Tokenized fields are tokenized before persistence.
- Metadata-only fields are persisted without evidence payload.
- Semantic evidence fields are routed to governed evidence and vector processing.
- Hidden Workday objects cannot be extracted, previewed, normalized, or written.

Award Nominations rule:

- `justification_text` is allowed as governed semantic evidence when scope approval is complete.

Workday rule:

- Compensation and personal-information objects are hidden and must not be extracted.

### 5. Schema & Mapping Validation

The ingestion layer validates that each scoped record can be mapped to the Integrity ontology.

Validation checks:

- Required source object exists.
- Required source fields are present.
- Required identifiers are not empty.
- Timestamp fields are parseable.
- Tenant connector mapping exists for required fields.
- Relationship source and target identifiers can be resolved or queued for resolution.
- Semantic evidence fields have approved evidence concepts.

Invalid records are handled according to `tenant_connector_schedule.failure_handling`.

### 6. Normalization

Normalization converts source records into canonical Integrity records.

Normalized outputs:

- Entity upserts.
- Event upserts.
- Relationship upserts.
- Evidence metadata.
- Semantic evidence chunks.
- Audit facts.
- Rejected or quarantined records.

Examples:

| Source | Normalized Output |
|---|---|
| Workday worker | Employee entity. |
| Workday supervisory organization | OrganizationUnit entity. |
| Workday manager field | Employee `ReportsTo` relationship. |
| Award nomination | RecognitionEvent entity/event. |
| Award nominee ID | RecognitionEvent `NominatedFor` Employee relationship. |
| Award justification text | EvidenceText and vector input. |

### 7. Entity Resolution Preparation

The ingestion layer prepares identity and relationship references for the Integrity Intelligence Core.

Responsibilities:

- Attach stable source identifiers.
- Preserve source-system lineage.
- Create cross-system join hints.
- Mark unresolved references.
- Queue relationship resolution tasks.

Cross-system rule:

- Custom application employee references should resolve through Workday worker identifiers whenever possible.

### 8. Store Writes

Normalized data is written to the core stores according to type and policy.

| Store | Written Data |
|---|---|
| Operational database | Runs, stages, alerts state, connector state, normalized operational records, ingestion status. |
| Graph database | Entities and relationships. |
| Evidence store / data lake | Governed evidence payloads and retained source artifacts. |
| Vector store | Semantic evidence embeddings and retrieval metadata. |
| Immutable audit log | Run events, policy decisions, field handling, approvals, store write summaries. |

Write behavior:

- Store writes are idempotent by tenant, source system, object type, and source identifier.
- Partial writes must be stage-tracked.
- Audit writes are append-only.
- Vector writes occur only for approved semantic evidence fields.

### 9. Downstream Processing

Downstream processing follows the toggles configured on `tenant_connector_schedule`.

| Toggle | Downstream Work |
|---|---|
| update_graph | Trigger graph update and relationship resolution. |
| run_risk_scoring | Trigger feature extraction, rules, anomaly detection, graph analysis, semantic similarity, and risk scoring. |
| generate_alerts | Create risk alerts when score thresholds or policy rules are met. |
| enable_ai_summaries | Generate AI investigation summaries for eligible alerts or high-risk clusters. |

The ingestion layer records downstream job IDs and stage status, but the Intelligence Core owns scoring and alert semantics.

### 10. Run Finalization

Finalization summarizes the ingestion run.

Finalization outputs:

- Final run status.
- Records extracted, accepted, normalized, skipped, and quarantined.
- Store write counts.
- Downstream processing status.
- Warning and error summaries.
- Next cursor or watermark.
- Admin notification.
- Immutable audit summary.

---

## Failure Handling

| Failure Type | Default Handling | Admin Action |
|---|---|---|
| Credential expired | Fail before extraction. | Rotate or reauthorize credentials. |
| Missing required mapping | Fail preflight. | Update ontology mapping. |
| Scope approval missing | Block run. | Submit or approve scope. |
| Source API unavailable | Retry with backoff, then fail. | Retry run. |
| Invalid source record | Quarantine or skip based on policy. | Review quarantine. |
| Missing Workday employee join | Quarantine custom app record. | Fix source data or mapping. |
| Store write failure | Retry idempotently. | Retry stage or run. |
| Vector processing failure | Complete with warnings if graph writes succeed. | Retry semantic evidence processing. |
| Downstream scoring failure | Complete ingestion with downstream warning. | Retry downstream processing. |

---

## Quarantine Rules

Records are quarantined when they cannot safely proceed but should remain available for admin review.

Quarantine examples:

- Award nomination references a nominee worker ID not found in Workday.
- Required timestamp cannot be parsed.
- Required identifier is missing.
- Semantic evidence field exceeds policy limits.
- Source record violates tenant boundary.

Quarantined records must preserve:

- Tenant ID.
- Connector instance ID.
- Ingestion run ID.
- Source object key.
- Source record identifier when available.
- Failure reason.
- Policy decision.
- Redacted preview payload.

---

## Audit Requirements

Every ingestion run writes immutable audit events for:

- Run creation.
- Preflight validation result.
- Source extraction start and completion.
- Policy decisions.
- Field redaction, hashing, tokenization, exclusion, metadata-only, and semantic evidence handling.
- Scope approval used by the run.
- Mapping version used by the run.
- Store write summaries.
- Quarantine summaries.
- Downstream processing triggers.
- Run completion, warning, failure, or cancellation.

---

## Monitoring Requirements

Tenant Admins need ingestion history and run-detail views that show:

- Run status.
- Trigger type.
- Connector instance.
- Manifest version.
- Scope name and approval status.
- Start and end time.
- Records extracted, accepted, normalized, skipped, quarantined, and failed.
- Stage status.
- Error list.
- Retry controls.
- Downstream processing status.
- Links to generated alerts or investigation workspace.

---

## Open Design Questions

- Should vector processing failure block run completion when semantic evidence was explicitly approved?
- Should retry be allowed at the whole-run level only, or should admins retry individual failed stages?
- How long should quarantined payload previews be retained?
- Should ingestion runs use one unified status or separate extraction, write, and downstream statuses?
- Should scheduled runs auto-pause after repeated failures?
