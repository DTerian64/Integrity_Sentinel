# Screen 9: Review & Activate

## Goal

Give the Tenant Admin a final review of connector setup, policy approval, mapping validation, and schedule before activation.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Award Nominations                                                  │
│ Step 7 of 7: Review & activate                                                │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Configuration summary                         │ Activation checklist          │
│ Connector: Award Nominations                  │ ✓ Connection configured       │
│ Template: Platform approved                   │ ✓ Metadata discovered         │
│ Manifest: v0.9.0                              │ ✓ Scope approved              │
│ Environment: Production                       │ ✓ Mappings validated          │
│ Owner: Corporate Awards                       │ ✓ Schedule configured         │
│                                               │ ✓ Audit logging enabled       │
│ Scope                                         │ ! Semantic evidence retention │
│ Objects: 6 included, 1 excluded               │   policy requires approval    │
│ Filters: FY26 awards cycle                    │                               │
│ Justification text: Semantic evidence         │                               │
│ Attachments: Excluded                         │                               │
│                                               │                               │
│ Schedule                                      │                               │
│ Scheduled batch · Daily at 2:00 AM PT         │                               │
│                                               │                               │
│ [Back]                         [Save draft] [Activate when approved]          │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Main Controls

- Configuration summary.
- Activation checklist.
- Approval status.
- Manifest version.
- Scope summary.
- Mapping validation summary.
- Schedule summary.
- Final activation action.

## Data Bindings

| UI Element | Source |
|---|---|
| Configuration summary | `tenant_connector_instance` |
| Manifest version | `tenant_connector_instance.connector_manifest_version` |
| Scope summary | `tenant_connector_scope` |
| Mapping status | `tenant_connector_mapping` |
| Schedule summary | `tenant_connector_schedule` |
| Approval state | `tenant_connector_scope.approval_status` |
| Checklist | Connection, discovery, scope, mapping, schedule, and audit validation results |

## Primary Action

Activate connector, or activate when approval is complete.

## Secondary Actions

- Save draft
- Return to schedule
- Export configuration summary

## Validation

- Activation is blocked until required approval is complete.
- Activation is blocked if required mappings are missing.
- Activation is blocked if connector credentials are expired or revoked.
- Activation writes an immutable audit event.

