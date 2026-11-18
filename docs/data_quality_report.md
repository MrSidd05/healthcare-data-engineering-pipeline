# Data Quality Report

## Purpose
This report summarizes the validation checks implemented for the healthcare batch pipeline and the results reported during project execution.

## Checks
- Source and layer row-count validation / reconciliation
- Gold-table key null checks
- Gold-table duplicate-key checks
- Persistent audit logging
- KPI and financial reconciliation

## Gold key-integrity results reported
All seven Gold tables passed the key-integrity audit at their defined grain, with zero null keys and zero duplicate keys.
| Gold table | Expected rows (observed baseline) | Key definition |
|---|---:|---|
| `gold_fact_encounters` | 61,459 | `encounter_id` |
| `gold_patient_summary` | 1,163 | `patient_id` |
| `gold_condition_summary` | 204 | `condition_code` + `condition_description` |
| `gold_procedure_summary` | 178 | `procedure_code` + `procedure_description` |
| `gold_organization_summary` | 1,122 | `organization_id` |
| `gold_provider_summary` | 1,123 | `provider_id` |
| `gold_payer_summary` | 10 | `payer_id` |

## Persistent audit logging
The project created `workspace.default.healthcare_data_quality_audit` to persist validation results. The recorded schema includes run ID, run timestamp, audit date, pipeline name, table name, actual row count, minimum expected rows, null-key rows, duplicate-key count, and status.

## Open reconciliation issue
A mismatch was identified between the overall claim-cost total in the project overview and the expenditure aggregation output (reported as roughly a tenfold difference). The discrepancy has not been documented as resolved. Recheck the source fields, filters, aggregation grain, currency/unit handling, and KPI definitions before publishing a resolved status.

## Scope of this report
These results are based on validation outcomes reported during development. This file is a project summary, not a new test execution. Rerun the notebook's validations to confirm the current state before relying on the values after code or data changes.
