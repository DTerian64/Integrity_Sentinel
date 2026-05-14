# Screen 7: Ontology Mapping

## Goal

Confirm or override source-to-Integrity ontology mappings before ingestion can run.

## Wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ Configure Award Nominations                                                  │
│ Step 5 of 7: Ontology mapping                              [Validate mapping]│
├──────────────────────────────────────────────────────────────────────────────┤
│ Object: award_nomination                                                      │
│                                                                              │
│ ┌──────────────────────┬──────────────────────┬──────────┬────────────────┐ │
│ │ Source field         │ Integrity concept    │ Required │ Status         │ │
│ ├──────────────────────┼──────────────────────┼──────────┼────────────────┤ │
│ │ nomination_id        │ RecognitionEvent.id  │ Yes      │ Accepted       │ │
│ │ nominee_worker_id    │ Employee.identifier  │ Yes      │ Accepted       │ │
│ │ nominator_worker_id  │ Employee.identifier  │ Yes      │ Accepted       │ │
│ │ submitted_at         │ Event.timestamp      │ Yes      │ Accepted       │ │
│ │ award_category_id    │ RecognitionCategory  │ Yes      │ Accepted       │ │
│ │ approval_status      │ Approval.status      │ No       │ Suggested      │ │
│ │ approver_worker_id   │ Approval.actor       │ No       │ Suggested      │ │
│ │ justification_text   │ Evidence.text        │ No       │ Accepted       │ │
│ └──────────────────────┴──────────────────────┴──────────┴────────────────┘ │
│                                                                              │
│ Relationships                                                                │
│ NominatedBy · NominatedFor · ApprovedBy · BelongsToCategory                  │
│                                                                              │
│ [Back]                                                      [Continue]        │
└──────────────────────────────────────────────────────────────────────────────┘
```

## Main Controls

- Object selector.
- Mapping table.
- Required/optional indicator.
- Mapping confidence.
- Manual override control.
- Transform expression editor for normalization when needed.
- Relationship mapping list.
- Validation result panel.

## Workday Required Mappings

| Source Field | Integrity Concept | Required |
|---|---|---:|
| worker_id | Employee.identifier | Yes |
| employee_status | Employee.status | Yes |
| manager_worker_id | Employee.managerIdentifier | Yes |
| supervisory_org_id | OrganizationUnit.identifier | Yes |
| work_email | Employee.workEmail | No |
| location_id | Location.identifier | No |

## Award Nominations Required Mappings

| Source Field | Integrity Concept | Required |
|---|---|---:|
| nomination_id | RecognitionEvent.identifier | Yes |
| nominee_worker_id | Employee.identifier | Yes |
| nominator_worker_id | Employee.identifier | Yes |
| submitted_at | Event.timestamp | Yes |
| award_category_id | RecognitionCategory.identifier | Yes |
| justification_text | Evidence.text | No |

## Data Bindings

| UI Element | Source |
|---|---|
| Suggested mapping | `connector_field_template.integrity_concept` |
| Tenant overrides | `tenant_connector_mapping` |
| Relationship mappings | `connector_relationship_template` and `tenant_connector_mapping` |
| Mapping status | `tenant_connector_mapping.mapping_status` |
| Transform logic | `tenant_connector_mapping.transform_expression` |

## Primary Action

Validate mapping

## Secondary Actions

- Accept all suggested mappings
- Override selected mapping
- Return to preview

## Validation

- Required mappings must be accepted before activation.
- Employee references from custom applications must map to Workday employee identifiers.
- Semantic evidence fields must map to approved evidence concepts.

