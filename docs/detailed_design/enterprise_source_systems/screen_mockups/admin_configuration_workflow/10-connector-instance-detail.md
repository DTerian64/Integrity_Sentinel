# Screen 10: Connector Instance Detail

## Goal

Show the operational state of a configured connector after activation, including health, sync status, migration tasks, and configuration links.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Data Sources / Workday Production                             [Run now]      │
├──────────────────────────────────────────────────────────────────────────────┤
│ Workday Production                                                            │
│ Active · Connected · Manifest v1.0.0 · Last sync: May 14, 2026 2:04 AM PT     │
│                                                                              │
│ ┌──────────────────────┐ ┌──────────────────────┐ ┌──────────────────────┐  │
│ │ Health               │ │ Last run             │ │ Next run             │  │
│ │ Connected            │ │ 18,420 workers       │ │ May 15, 2026 2:00 AM │  │
│ │ Secrets valid        │ │ 840 org units        │ │ Scheduled batch      │  │
│ └──────────────────────┘ └──────────────────────┘ └──────────────────────┘  │
│                                                                              │
│ Tabs                                                                         │
│ [Overview] [Scope] [Mappings] [Runs] [Migration tasks] [Audit]               │
│                                                                              │
│ Migration tasks                                                              │
│ ┌──────────────┬──────────────┬──────────────────────┬────────────────────┐ │
│ │ Status       │ Target       │ Required action      │ Impact             │ │
│ ├──────────────┼──────────────┼──────────────────────┼────────────────────┤ │
│ │ Pending      │ v1.1.0       │ Review               │ New lifecycle field│ │
│ │ Not needed   │ v1.0.1       │ None                 │ Documentation only │ │
│ └──────────────┴──────────────┴──────────────────────┴────────────────────┘ │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Main Controls

- Run now.
- Pause connector.
- Edit connection.
- Edit scope.
- Edit mappings.
- View run history.
- View migration tasks.
- View audit log.

## Migration Task Behavior

Capability, mapping, authentication, or governance changes create migration tasks for affected tenant connector instances.

Examples:

| Change | Migration Task |
|---|---|
| New optional Workday lifecycle field | Review |
| Required field renamed in custom app template | RemapFields |
| Authentication method deprecated | ReauthorizeConnector |
| Semantic evidence policy changed | ReapproveScope |
| Capability removed | Review and possibly pause schedule |

## Data Bindings

| UI Element | Source |
|---|---|
| Instance header | `tenant_connector_instance` |
| Health | `tenant_connector_instance.connection_status`, `tenant_connector_credential.credential_status` |
| Schedule | `tenant_connector_schedule` |
| Scope tab | `tenant_connector_scope` |
| Mappings tab | `tenant_connector_mapping` |
| Migration tasks | `connector_migration_task` |
| Manifest version | `tenant_connector_instance.connector_manifest_version` |
| Audit | Immutable audit log events |

## Primary Action

Run now

## Secondary Actions

- Pause connector
- Review migration task
- Edit configuration
- Open ingestion monitor

