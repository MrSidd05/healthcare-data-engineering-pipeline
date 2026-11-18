# Healthcare Data Engineering Pipeline

## Project Overview

This project implements an end-to-end batch data engineering pipeline for synthetic healthcare data using Databricks, Apache Spark, PySpark, Spark SQL, and Delta tables. It demonstrates ingestion, transformation, analytical modeling, data quality validation, KPI generation, and scheduled orchestration.

> **Data privacy:** The project uses synthetic Synthea-generated data. It does not use real patient records or PHI.

## Objectives
- Ingest healthcare source CSV datasets into a lakehouse-style architecture.
- Organize data into Bronze, Silver, and Gold layers.
- Apply cleaning, standardization, and transformation logic.
- Create analytical tables for healthcare operations and utilization analysis.
- Validate data quality and key integrity.
- Run the batch pipeline through Databricks Jobs and maintain audit results.

## Dataset
| Dataset | Rows | Source columns |
|---|---:|---:|
| Patients | 1,163 | 25 |
| Encounters | 61,459 | 15 |
| Conditions | 38,094 | 6 |
| Procedures | 83,823 | 9 |
| **Total** | **184,539** | **55 across source schemas** |

## Architecture
1. **Source:** Synthetic CSV files for patients, encounters, conditions, and procedures.
2. **Bronze:** Source-oriented ingestion tables with ingestion metadata (`_ingested_at`, `_source_file`, `_source_system`).
3. **Silver:** Cleaned and standardized tables for downstream analysis.
4. **Gold:** Analytical fact and summary tables designed around business dimensions and analysis grains.
5. **Quality and audit:** Row-count reconciliation, null-key checks, duplicate-key checks, and a persistent audit table.
6. **Orchestration:** Databricks batch job named `healthcare_batch_pipeline`, configured on a daily schedule.

## Technologies
- Databricks Free Edition Serverless
- Apache Spark 4.2
- PySpark
- Spark SQL
- Delta tables
- Databricks Jobs
- Synthetic Synthea CSV datasets

## Bronze tables
- `workspace.default.healthcare_bronze_patients`
- `workspace.default.healthcare_bronze_encounters`
- `workspace.default.healthcare_bronze_conditions`
- `workspace.default.healthcare_bronze_procedures`

## Silver tables
- `workspace.default.healthcare_silver_patients`
- `workspace.default.healthcare_silver_encounters`
- `workspace.default.healthcare_silver_conditions`
- `workspace.default.healthcare_silver_procedures`

## Gold tables
- `workspace.default.gold_fact_encounters`
- `workspace.default.gold_patient_summary`
- `workspace.default.gold_condition_summary`
- `workspace.default.gold_procedure_summary`
- `workspace.default.gold_organization_summary`
- `workspace.default.gold_provider_summary`
- `workspace.default.gold_payer_summary`

## KPI domains
1. Patient & Encounter Overview
2. Monthly Encounter Trends
3. Healthcare Expenditure & Claims
4. Payer Coverage & Patient Financial Responsibility
5. Provider Workload & Performance
6. Organization Utilization & Performance
7. Condition Prevalence & Analysis
8. Procedure Utilization & Cost Analysis

## Data quality and reconciliation
Gold-layer key integrity audits were reported as passing for all seven audited Gold tables, with zero null keys and zero duplicate keys at the configured grain. Condition and procedure summaries use composite keys rather than code-only keys. A persistent audit table is maintained:
`workspace.default.healthcare_data_quality_audit`.

An expenditure reconciliation discrepancy was identified between KPI totals and expenditure aggregation. It remains an open investigation and is not represented as resolved in this documentation.

## Batch orchestration
The Databricks job is named `healthcare_batch_pipeline`. It is configured for a daily schedule at 6:00 AM IST and has success/failure notifications configured. A successful manual execution and one successful observed run were reported; repeated scheduled-run reliability has not yet been established.

## Scope and limitations
- Batch processing only; streaming is outside the current implementation.
- Tableau dashboard development is not included in this repository version.
- Dataset is synthetic and intended for learning, portfolio demonstration, and technical discussion.
- Databricks Free Edition is the development and demonstration environment, not a production deployment.

## Repository contents
The repository includes the notebook, sample datasets, architecture assets, and documentation. Execution depends on the Databricks environment and the source data being available at the configured location.
