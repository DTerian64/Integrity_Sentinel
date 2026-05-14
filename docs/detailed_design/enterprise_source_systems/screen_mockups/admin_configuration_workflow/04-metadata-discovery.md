# Screen 4: Metadata Discovery

## Goal

Show the source metadata discovered from the connected system and identify what is available for governed ingestion.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Workday HCM                                                        │
│ Step 2 of 7: Metadata discovery                              [Run discovery] │
├──────────────────────────────────────────────┬───────────────────────────────┤
│ Discovery snapshot                            │ Discovery summary             │
│ Last run: May 14, 2026 12:45 PM               │ Objects: 5                     │
│ Schema version: Workday v43.0                 │ Required fields: 4/4 found     │
│                                               │ Relationships: 3               │
│ ┌──────────────┬──────────────┬─────────────┐ │ Warnings: 0                    │
│ │ Object       │ Records      │ Status      │ │                               │
│ ├──────────────┼──────────────┼─────────────┤ │ Hidden by policy               │
│ │ worker       │ 18,420       │ Available   │ │ Compensation                   │
│ │ sup_org      │ 840          │ Available   │ │ Personal information           │
│ │ job_profile  │ 312          │ Available   │ │                               │
│ │ location     │ 62           │ Available   │ │                               │
│ │ lifecycle    │ 96,102       │ Available   │ │                               │
│ └──────────────┴──────────────┴─────────────┘ │                               │
│                                               │                               │
│ Field sensitivity                             │                               │
│ worker_id Include · work_email Hash · notes Excluded                         │
│                                               │                               │
│ [Back]                                                    [Continue to scope] │
└──────────────────────────────────────────────┴───────────────────────────────┘
```

## Award Nominations Variant

Discovery shows:

| Object | Example Count | Default |
|---|---:|---|
| award_nomination | 12,480 | Include |
| nominee | 9,120 | Include |
| nominator | 8,760 | Include |
| nomination_approval | 10,430 | Include |
| award_category | 32 | Include |
| justification_text | 12,480 | SemanticEvidence |
| attachment | Unknown | Exclude |

## Main Controls

- Run discovery
- Object inventory table
- Field sensitivity summary
- Relationship summary
- Hidden-by-policy summary
- Discovery warning list

## Data Bindings

| UI Element | Source |
|---|---|
| Snapshot timestamp | `connector_discovery_snapshot.discovered_at` |
| Objects | `connector_discovery_snapshot.discovered_objects` |
| Fields | `connector_discovery_snapshot.discovered_fields` |
| Relationships | `connector_discovery_snapshot.discovered_relationships` |
| Estimated counts | `connector_discovery_snapshot.estimated_record_counts` |
| Warnings | `connector_discovery_snapshot.validation_findings` |
| Hidden source objects | Manifest policy and `connector_object_template` visibility rules |

## Primary Action

Continue to scope

## Secondary Actions

- Rerun discovery
- Download discovery report
- Back to connection setup

