# Screen 2: Connector Detail

## Goal

Show enough connector detail for the Tenant Admin to decide whether to configure the connector.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Connector Catalog / Workday HCM                              [Configure]    │
├──────────────────────────────────────────────────────────────────────────────┤
│ Workday HCM                                                                  │
│ Employee and organization system of record                                    │
│ GA · Manifest v1.0.0 · Provider: Workday                                      │
│                                                                              │
│ ┌──────────────────────┐ ┌──────────────────────┐ ┌──────────────────────┐  │
│ │ Data domains         │ │ Sync modes           │ │ Authentication       │  │
│ │ Employee             │ │ Scheduled batch      │ │ OAuth2               │  │
│ │ Organization         │ │ Incremental support  │ │ Client credentials   │  │
│ │ Location             │ │                      │ │                      │  │
│ └──────────────────────┘ └──────────────────────┘ └──────────────────────┘  │
│                                                                              │
│ Included source objects                                                       │
│ ┌──────────────────────┬──────────────────────┬───────────────────────────┐ │
│ │ Source object        │ Integrity concept    │ Default behavior          │ │
│ ├──────────────────────┼──────────────────────┼───────────────────────────┤ │
│ │ worker               │ Employee             │ Include                   │ │
│ │ supervisory_org      │ OrganizationUnit     │ Include                   │ │
│ │ job_profile          │ Role                 │ Include                   │ │
│ │ location             │ Location             │ Include                   │ │
│ │ lifecycle_event      │ Event                │ Include                   │ │
│ └──────────────────────┴──────────────────────┴───────────────────────────┘ │
│                                                                              │
│ Governance defaults                                                           │
│ Free text: Excluded · Attachments: Excluded · Sensitive personal: Excluded    │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Workday Variant

The detail screen must not show Workday compensation or personal-information objects in the Tenant Admin-visible source object list.

## Award Nominations Variant

For Award Nominations, the detail screen shows:

- Platform-approved custom app template.
- Recognition, approval, employee event, and semantic evidence domains.
- Justification text as governed semantic evidence.
- Attachments excluded for first release.
- Manual import, SFTP, API key, and scheduled batch setup options.

## Data Bindings

| UI Element | Source |
|---|---|
| Header | `connector_definition` |
| Manifest version | `connector_definition.current_manifest_version` |
| Domains | `connector_object_template.domain` |
| Source object list | `connector_object_template` |
| Authentication methods | `connector_auth_option` |
| Capabilities | `connector_capability` |
| Governance defaults | `connector_field_template.default_ingestion_behavior` |

## Primary Action

Configure connector

## Secondary Actions

- Back to catalog
- View setup documentation
- View required permissions

