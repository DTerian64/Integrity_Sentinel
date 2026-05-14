# Screen 8: Schedule & Downstream Processing

## Goal

Configure when ingestion runs and what downstream platform processing should happen after records are normalized.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Workday HCM                                                        │
│ Step 6 of 7: Schedule & processing                                            │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Run mode                                      │ Downstream processing         │
│ ( ) Run once                                  │ [x] Update graph              │
│ (●) Scheduled batch                           │ [x] Run risk scoring          │
│ ( ) Event stream                              │ [x] Generate alerts           │
│                                               │ [x] Enable AI summaries       │
│ Schedule                                      │                               │
│ Time zone                                     │ Failure handling              │
│ [America/Los_Angeles                    v]   │ [Retry, then quarantine   v]  │
│ Frequency                                     │                               │
│ [Daily                                  v]    │ Notifications                 │
│ Time                                          │ [hris-admin@contoso.com  ]    │
│ [02:00 AM                              ]     │ [integrity-ops@contoso.com]   │
│                                               │                               │
│ Backfill                                      │                               │
│ Start date                                    │                               │
│ [Jan 1, 2026                           ]     │                               │
│ End date                                      │                               │
│ [Today                                  ]     │                               │
│                                               │                               │
│ [Back]                                               [Review configuration]  │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Award Nominations Defaults

- Run mode: Scheduled batch or manual import.
- Backfill: Current awards cycle.
- Failure handling: Quarantine records with missing Workday employee IDs.
- Downstream processing:
  - Update graph: On
  - Run risk scoring: On
  - Generate alerts: On
  - Enable AI summaries: On
  - Vector store semantic evidence: On when justification text is included

## Data Bindings

| UI Element | Source |
|---|---|
| Run mode | `tenant_connector_schedule.run_mode` |
| Time zone | `tenant_connector_schedule.schedule_timezone` |
| Schedule | `tenant_connector_schedule.schedule_expression` |
| Backfill | `tenant_connector_schedule.backfill_start_at`, `backfill_end_at` |
| Failure handling | `tenant_connector_schedule.failure_handling` |
| Notifications | `tenant_connector_schedule.notification_recipients` |
| Downstream toggles | `tenant_connector_schedule.update_graph`, `run_risk_scoring`, `generate_alerts`, `enable_ai_summaries` |

## Primary Action

Review configuration

## Secondary Actions

- Save draft
- Return to mapping

## Validation

- Event stream is available only when the connector capability supports it.
- Schedule must respect tenant-level quiet hours if configured.
- Semantic evidence processing must remain enabled when included semantic evidence fields are in scope.

