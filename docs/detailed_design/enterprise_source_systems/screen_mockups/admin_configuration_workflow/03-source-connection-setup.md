# Screen 3: Source Connection Setup

## Goal

Create the tenant connector instance and configure non-secret connection details before storing credentials.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Workday HCM                                                        │
│ Step 1 of 7: Source connection                                                │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Source details                                │ Setup progress                │
│ Display name                                  │ ● Source connection           │
│ [Workday Production                      ]    │ ○ Metadata discovery          │
│ Environment                                   │ ○ Scope                        │
│ (●) Production  ( ) Sandbox  ( ) Test         │ ○ Preview                      │
│ Owning business unit                          │ ○ Mapping                      │
│ [People Operations                       ]    │ ○ Schedule                     │
│ Data owner                                    │ ○ Activate                     │
│ [hr-data-owner@contoso.com               ]    │                               │
│ Technical owner                               │ Connection status              │
│ [hris-admin@contoso.com                  ]    │ Not tested                     │
│                                               │                               │
│ Authentication                                │                               │
│ Method                                        │                               │
│ [Client credentials                 v]        │                               │
│ Workday tenant URL                            │                               │
│ [https://wd2-impl-services.workday.com...]    │                               │
│ API version                                   │                               │
│ [v43.0                                  ]     │                               │
│ Integration user                              │                               │
│ [ISU_IntegritySentinel                  ]     │                               │
│                                               │                               │
│ [Save draft]                     [Test] [Next]│                               │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Award Nominations Variant

Fields change based on selected auth option:

- Manual upload:
  - Display name
  - Environment
  - Owning business unit
  - Data owner
  - File schema template
- SFTP:
  - Host
  - Folder path
  - File naming pattern
  - Key reference
- API key:
  - Base URL
  - API version
  - Header name
  - Secret reference

## Main Controls

- Display name
- Environment
- Owning business unit
- Data owner
- Technical owner
- Authentication method
- Required non-secret connector settings
- Secret entry component backed by tenant-isolated secret store
- Test connection
- Save draft

## Data Bindings

| UI Element | Source |
|---|---|
| Display name | `tenant_connector_instance.display_name` |
| Environment | `tenant_connector_instance.environment` |
| Business unit | `tenant_connector_instance.owning_business_unit` |
| Owners | `tenant_connector_instance.data_owner_contact`, `technical_owner_contact` |
| Auth method | `tenant_connector_credential.auth_type` |
| Secret pointer | `tenant_connector_credential.secret_store_reference` |
| Non-secret settings | `tenant_connector_setting` |
| Test status | `tenant_connector_instance.connection_status` |

## Primary Action

Test connection

## Secondary Actions

- Save draft
- Continue after successful test

## Validation

- Required connector settings must be complete.
- Secret values must be stored only in the tenant-isolated secret store.
- Connection test must pass before metadata discovery can run.

