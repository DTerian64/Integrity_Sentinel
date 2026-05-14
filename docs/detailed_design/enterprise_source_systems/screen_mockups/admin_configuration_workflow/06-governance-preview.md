# Screen 6: Governance Preview

## Goal

Let the Tenant Admin preview what will be ingested and resolve governance warnings before mapping and activation.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Award Nominations                                                  │
│ Step 4 of 7: Governance preview                                               │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Estimated ingestion                           │ Validation results            │
│ ┌──────────────────────┬──────────┬─────────┐│ ✓ Required IDs found           │
│ │ Type                 │ Count    │ Store   ││ ✓ Workday join fields found    │
│ ├──────────────────────┼──────────┼─────────┤│ ✓ Attachments excluded         │
│ │ RecognitionEvent     │ 12,480   │ Graph   ││ ✓ Tenant boundary confirmed    │
│ │ Approval             │ 10,430   │ Graph   ││ ! Semantic evidence review     │
│ │ Employee refs        │ 17,880   │ Graph   ││                               │
│ │ Justification text   │ 12,480   │ Vector  ││ Approval                       │
│ └──────────────────────┴──────────┴─────────┘│ Compliance review required     │
│                                               │                               │
│ Sample normalized record                      │                               │
│ nomination_id: NOM-109823                     │                               │
│ nominee_worker_id: W12345                     │                               │
│ nominator_worker_id: W48721                   │                               │
│ category: Innovation                          │                               │
│ justification_text: included as vector input  │                               │
│                                               │                               │
│ [Back]                                           [Submit for approval]        │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Preview Sections

- Estimated records by object type.
- Estimated relationships by relationship type.
- Target store summary:
  - Operational database
  - Graph database
  - Evidence store
  - Vector store
  - Immutable audit log
- Sample normalized records.
- Excluded objects and fields.
- Governance warnings.
- Required approvals.

## Data Bindings

| UI Element | Source |
|---|---|
| Estimated records | `connector_discovery_snapshot.estimated_record_counts` filtered by `tenant_connector_scope` |
| Validation findings | `connector_discovery_snapshot.validation_findings` and policy validation service |
| Excluded fields | `tenant_connector_scope.excluded_field_keys` |
| Sensitive field behavior | `tenant_connector_scope.sensitive_field_policy` |
| Approval state | `tenant_connector_scope.approval_status` |

## Primary Action

Submit for approval or approve scope, depending on tenant policy.

## Secondary Actions

- Return to scope
- Export preview summary

## Validation

- Semantic evidence use requires explicit preview and audit capture.
- Records with missing Workday worker IDs are quarantined or excluded based on failure handling.
- Hidden Workday objects cannot appear in the preview.

