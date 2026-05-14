# Data Ingestion Layer

## Purpose

This folder will contain detailed design artifacts for extraction, ingestion jobs, normalization, validation, ingestion policy execution, and ingestion monitoring.

## Planned Artifacts

- Batch and event-stream processing behavior.
- Normalization and mapping execution.
- Ingestion monitor screen designs.
- Failure handling, retry, quarantine, and audit behavior.

## Current Artifacts

| Artifact | Description |
|---|---|
| `ingestion-job-lifecycle.md` | Detailed lifecycle for ingestion run creation, preflight, extraction, policy enforcement, validation, normalization, store writes, downstream processing, finalization, failure handling, quarantine, audit, and monitoring. |
| `ingestion-job-lifecycle.mmd` | Mermaid workflow diagram for the governed ingestion job lifecycle. |
