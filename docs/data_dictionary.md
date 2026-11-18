# Data Dictionary

This document describes the pipeline tables, their purpose, and their analytical grain. Exact column-level schemas should be verified against the exported notebook and the live tables before treating this as a complete schema reference.


## Source datasets
| Dataset | Description | Rows | Source columns |
|---|---|---:|---:|
| Patients | Patient demographic and record attributes | 1,163 | 25 |
| Encounters | Healthcare encounter events | 61,459 | 15 |
| Conditions | Patient condition records | 38,094 | 6 |
| Procedures | Procedures recorded for patients/encounters | 83,823 | 9 |


## Bronze layer
Bronze tables preserve source-oriented ingested data and include pipeline metadata such as ingestion timestamp, source file, and source system.
| Table | Purpose |
|---|---|
| `healthcare_bronze_patients` | Ingested patient source records |
| `healthcare_bronze_encounters` | Ingested encounter source records |
| `healthcare_bronze_conditions` | Ingested condition source records |
| `healthcare_bronze_procedures` | Ingested procedure source records |

Metadata columns noted in the implementation: `_ingested_at`, `_source_file`, `_source_system`.

## Silver layer
Silver tables are the cleaned and standardized datasets used to build analytical Gold tables.
| Table | Purpose |
|---|---|
| `healthcare_silver_patients` | Cleaned patient records |
| `healthcare_silver_encounters` | Cleaned encounter records |
| `healthcare_silver_conditions` | Cleaned condition records |
| `healthcare_silver_procedures` | Cleaned procedure records |

## Gold layer
| Table | Intended grain / key | Purpose |
|---|---|---|
| `gold_fact_encounters` | One row per encounter; `encounter_id` | Encounter-level analytical fact table |
| `gold_patient_summary` | One row per patient; `patient_id` | Patient-level summary metrics |
| `gold_condition_summary` | Composite grain: condition code + condition description | Condition prevalence and analysis |
| `gold_procedure_summary` | Composite grain: procedure code + procedure description | Procedure utilization and cost analysis |
| `gold_organization_summary` | One row per organization; `organization_id` | Organization utilization/performance summaries |
| `gold_provider_summary` | One row per provider; `provider_id` | Provider workload/performance summaries |
| `gold_payer_summary` | One row per payer; `payer_id` | Payer coverage and financial summaries |

Catalog/schema used in the project: `workspace.default`.

## Persistent data-quality audit table
`workspace.default.healthcare_data_quality_audit`

The audit table stores pipeline/run identifiers and audit metadata, including run timestamp/date, pipeline name, table name, actual row count, minimum expected rows, null-key count, duplicate-key count, and status.

## Gold key audit baseline
| Gold table | Expected row count from observed validation | Key |
|---|---:|---|
| `gold_fact_encounters` | 61,459 | `encounter_id` |
| `gold_patient_summary` | 1,163 | `patient_id` |
| `gold_condition_summary` | 204 | `condition_code` + `condition_description` |
| `gold_procedure_summary` | 178 | `procedure_code` + `procedure_description` |
| `gold_organization_summary` | 1,122 | `organization_id` |
| `gold_provider_summary` | 1,123 | `provider_id` |
| `gold_payer_summary` | 10 | `payer_id` |

These row counts represent the recorded validation baseline and should be rechecked if source data or transformations change.
