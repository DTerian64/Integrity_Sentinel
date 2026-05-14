# Screen 1: Connector Catalog

## Goal

Let the Tenant Admin find and select an enterprise source-system connector.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Integrity Sentinel                                      Tenant: Contoso       │
├───────────────┬──────────────────────────────────────────────────────────────┤
│ Admin         │ Connector Catalog                              [New Custom]  │
│ Data Sources  │ Search connectors...                                         │
│ Policies      │                                                              │
│ Users         │ [All] [HR / Employee] [Custom Apps] [Identity] [Finance]     │
│ Audit         │                                                              │
│               │ ┌──────────────────────┐ ┌──────────────────────┐           │
│               │ │ Workday HCM          │ │ Award Nominations     │           │
│               │ │ HR / Employee        │ │ Custom App Template   │           │
│               │ │ Domains: Employee,   │ │ Domains: Recognition, │           │
│               │ │ Org, Location        │ │ Approval, Evidence    │           │
│               │ │ Sync: Scheduled      │ │ Sync: Manual, Batch    │           │
│               │ │ Status: Not set up   │ │ Status: Draft exists   │           │
│               │ │ [View details]       │ │ [Resume setup]        │           │
│               │ └──────────────────────┘ └──────────────────────┘           │
│               │                                                              │
│               │ ┌──────────────────────┐ ┌──────────────────────┐           │
│               │ │ Entra ID             │ │ Coupa Procurement     │           │
│               │ │ Identity & Access    │ │ Procurement           │           │
│               │ │ Status: Not set up   │ │ Status: Preview       │           │
│               │ │ [View details]       │ │ [View details]        │           │
│               │ └──────────────────────┘ └──────────────────────┘           │
└───────────────┴──────────────────────────────────────────────────────────────┘
```

## Main Controls

- Search input for connector name, provider, data domain, and category.
- Category tabs:
  - All
  - HR / Employee
  - Custom Applications
  - Identity & Access
  - ERP / Finance
  - Procurement
  - Collaboration Metadata
- Connector cards with:
  - Connector name
  - Category
  - Release stage
  - Data domains
  - Supported sync modes
  - Required permission summary
  - Tenant setup status
  - Primary action

## Example Cards

### Workday HCM

- Category: HR / Employee
- Provider: Workday
- Release stage: GA
- Data domains: Employee, Organization, Location, Worker Lifecycle
- Sync modes: Scheduled batch
- Status: Not configured
- Primary action: View details

### Award Nominations

- Category: Custom Applications
- Provider: Tenant Custom App
- Release stage: Preview
- Data domains: Recognition, Approval, Employee Event, Semantic Evidence
- Sync modes: Manual import, scheduled batch
- Status: Draft exists
- Primary action: Resume setup

## Data Bindings

| UI Element | Source |
|---|---|
| Connector cards | `connector_definition` |
| Data domains | `connector_object_template.domain` |
| Sync modes | `connector_definition.default_sync_modes` |
| Permission summary | `connector_auth_option.permission_scopes` |
| Tenant status | Latest `tenant_connector_instance` for tenant and connector |
| Release stage | `connector_definition.release_stage` |

## Primary Action

View details

## Secondary Actions

- Resume setup
- Filter by category
- Create from approved custom app template

