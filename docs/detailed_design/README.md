# Detailed Design

This section contains workflow-level design artifacts for Integrity Sentinel.

Detailed design documents sit between high-level architecture and implementation planning. They describe product flows, screen concepts, data movement, governance checkpoints, platform behavior, and user-facing workflows.

The folder structure follows the high-level workflow in `../technical_architecture/high_level_workflow.mmd`.

## Workflow Areas

| Workflow Stage | Directory | Purpose |
|---|---|---|
| Enterprise Source Systems | `enterprise_source_systems/` | Source-system catalog, connector configuration, tenant-admin setup screens, and source metadata governance. |
| Data Ingestion Layer | `data_ingestion_layer/` | Extraction, ingestion jobs, normalization, validation, ingestion policy execution, and ingestion monitoring. |
| Integrity Intelligence Core | `integrity_intelligence_core/` | Ontology modeling, graph construction, rules, anomaly detection, evidence correlation, and risk scoring. |
| AI Investigation Layer | `ai_investigation_layer/` | Sentinel AI Investigator behavior, summaries, reasoning support, investigation assistance, and evidence retrieval. |
| Core Platform API Layer | `core_platform_api_layer/` | APIs, tenancy boundaries, service contracts, authorization, and platform integration surfaces. |
| User Experience & Investigation Workspace | `ux_investigation_workspace/` | Investigator-facing workspace, cases, alerts, entity graph exploration, and workflow handoff screens. |

## Current Detailed Designs

The current detailed design work is under `enterprise_source_systems/` and focuses on the Connector Catalog and Tenant Admin configuration workflow.
