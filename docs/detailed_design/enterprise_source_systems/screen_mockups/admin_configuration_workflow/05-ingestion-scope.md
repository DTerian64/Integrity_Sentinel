# Screen 5: Ingestion Scope

## Goal

Let the Tenant Admin define which discovered objects, fields, relationships, and records are allowed into Integrity Sentinel.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Award Nominations                                                  │
│ Step 3 of 7: Ingestion scope                                                  │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Scope name                                    │ Policy posture                │
│ [Corporate Awards - FY26                 ]   │ Template: Platform approved   │
│                                               │ Free text: Semantic evidence  │
│ Objects                                       │ Attachments: Excluded         │
│ [x] Award nominations                         │ Employee join: Workday ID     │
│ [x] Nominees                                  │                               │
│ [x] Nominators                                │ Required approval             │
│ [x] Approvals                                 │ Review required               │
│ [x] Award categories                          │                               │
│ [x] Justification text                        │                               │
│ [ ] Attachments                               │                               │
│                                               │                               │
│ Filters                                       │                               │
│ Date range                                    │                               │
│ [Jan 1, 2026] to [Dec 31, 2026]               │                               │
│ Award categories                              │                               │
│ [Spot Bonus] [Innovation] [Customer Trust]    │                               │
│ Business units                                │                               │
│ [All eligible units                       v]  │                               │
│                                               │                               │
│ [Back]                                             [Preview scoped data]     │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Workday Variant

Suggested scope filters:

- Worker status
- Supervisory organization
- Region
- Location
- Job family
- Worker lifecycle event type
- Date range

Compensation and personal information are not shown as selectable objects.

## Award Nominations Variant

Suggested scope filters:

- Award category
- Submission date
- Approval status
- Business unit
- Nominee region
- Nominator region
- Minimum award value, when applicable
- Include justification text as semantic evidence
- Exclude attachments

## Data Bindings

| UI Element | Source |
|---|---|
| Scope name | `tenant_connector_scope.scope_name` |
| Included objects | `tenant_connector_scope.included_object_keys` |
| Excluded objects | `tenant_connector_scope.excluded_object_keys` |
| Included fields | `tenant_connector_scope.included_field_keys` |
| Excluded fields | `tenant_connector_scope.excluded_field_keys` |
| Filters | `tenant_connector_scope.filter_expression` |
| Sensitive field policy | `tenant_connector_scope.sensitive_field_policy` |
| Approval status | `tenant_connector_scope.approval_status` |

## Primary Action

Preview scoped data

## Secondary Actions

- Save draft
- Return to discovery

## Validation

- Required objects from the platform-approved template cannot be removed.
- Justification text requires semantic evidence retention controls.
- Attachments remain excluded for the first release.

