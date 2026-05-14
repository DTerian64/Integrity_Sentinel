# Enterprise Source Systems

## Purpose

This folder contains detailed design artifacts for the Enterprise Source Systems stage of Integrity Sentinel.

This area covers source-system selection, connector catalog metadata, tenant-specific connector configuration, metadata discovery, source scope governance, and admin-facing setup workflows.

## Current Artifacts

| Artifact | Description |
|---|---|
| `connector-catalog-data-store.md` | Data-store design for connector definitions, manifest versions, tenant connector instances, credential metadata, scope, mapping overrides, schedules, discovery snapshots, and migration tasks. |
| `admin-led-ingestion-workflow.mmd` | Mermaid diagram for tenant-admin source connection, metadata filtering, ingestion, storage, intelligence, and investigation handoff. |
| `admin-led-ingestion-mockups.md` | Screen-by-screen product mockup notes for the admin-led connector configuration and ingestion setup experience. |
| `screen_mockups/` | Markdown screen specs and clickable static app prototype for the admin configuration workflow. |

## Primary Source Systems

- Workday HCM is the employee and organization system of record.
- Award Nominations is modeled as a platform-approved custom application connector template.

## Design Decisions

- Platform-managed connector definitions are versioned as deployable catalog manifests and published into the operational database.
- Custom application connectors begin from platform-approved templates.
- Workday compensation and personal-information objects are hidden from Tenant Admin setup flows for the first release.
- Award Nominations justification text is supported as governed semantic evidence.
- Connector capability changes create migration tasks for affected tenant connector instances.

## Related Workflow Areas

- `../data_ingestion_layer/`
- `../integrity_intelligence_core/`
- `../ai_investigation_layer/`
- `../core_platform_api_layer/`
- `../ux_investigation_workspace/`
