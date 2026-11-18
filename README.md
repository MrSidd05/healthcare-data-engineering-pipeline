# Healthcare Data Engineering Pipeline

An end-to-end batch data engineering project built using Apache Spark, PySpark, SQL, and Databricks. This project transforms synthetic healthcare data into structured, analytics-ready datasets using the Medallion Architecture (Bronze, Silver, Gold), with data quality validation, audit logging, scheduled orchestration, and reliability checks.

## Project Overview

The objective is to build a scalable healthcare data pipeline that ingests raw healthcare datasets, cleans and transforms them, and produces analytics-ready data for healthcare operational and financial analysis.

The pipeline uses a Medallion Architecture to organize data processing into three layers:

- Bronze: Raw source ingestion
- Silver: Cleaned and standardized datasets
- Gold: Aggregated analytical datasets

The project also includes scheduled batch execution, data quality checks, audit history, and pipeline reliability validation.

## Architecture

```text
Synthetic Healthcare CSV Data
            |
            v
     BRONZE LAYER
  Raw Data Ingestion
  Source Metadata
            |
            v
      SILVER LAYER
 Cleaning & Standardization
  Data Quality Processing
            |
            v
       GOLD LAYER
 Analytical Data Models
 Aggregations & KPIs
            |
            v
  Healthcare Analytics
  Business KPI Analysis
```

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Data processing and pipeline logic |
| PySpark | Distributed data transformation |
| Apache Spark | Data processing engine |
| Spark SQL | Analytical querying |
| Delta Lake | Table storage and data management |
| Databricks | Pipeline development and execution |
| SQL | Data analysis and validation |
| Git & GitHub | Version control and project portfolio |

## Dataset

The project uses synthetic healthcare data generated from the Synthea dataset.

| Dataset | Records | Columns |
|---|---:|---:|
| Patients | 1,163 | 25 |
| Encounters | 61,459 | 15 |
| Conditions | 38,094 | 6 |
| Procedures | 83,823 | 9 |
| **Total** | **184,539** | **55 across source schemas** |

The datasets cover patient demographics, healthcare encounters, conditions, procedures, providers, organizations, and payer-related information.

No real patient information is used.

## Medallion Architecture

### Bronze Layer — Raw Ingestion

Purpose:
- Ingest source CSV files into Databricks tables.
- Preserve source-level data.
- Add ingestion metadata for traceability.

Metadata columns include:

- `_ingested_at`
- `_source_file`
- `_source_system`

Bronze tables:

| Table |
|---|
| healthcare_bronze_patients |
| healthcare_bronze_encounters |
| healthcare_bronze_conditions |
| healthcare_bronze_procedures |

### Silver Layer — Cleaned & Standardized

Purpose:
- Prepare data for analytical processing.
- Apply transformations and standardization.
- Create consistent datasets for downstream modeling.

Silver tables:

| Table |
|---|
| healthcare_silver_patients |
| healthcare_silver_encounters |
| healthcare_silver_conditions |
| healthcare_silver_procedures |

### Gold Layer — Analytics & Business Models

Gold contains seven analytical tables.

| Table | Purpose | Key |
|---|---|---|
| gold_fact_encounters | Encounter-level analytical fact | encounter_id |
| gold_patient_summary | Patient-level metrics | patient_id |
| gold_condition_summary | Condition analytics | condition_code + description |
| gold_procedure_summary | Procedure analytics | procedure_code + description |
| gold_organization_summary | Organization-level analytics | organization_id |
| gold_provider_summary | Provider-level analytics | provider_id |
| gold_payer_summary | Payer-level analytics | payer_id |

Composite keys are used where the actual analytical grain requires multiple columns. Condition and procedure records are not incorrectly treated as unique by code alone.

## Data Quality & Validation

Data quality checks are implemented throughout the pipeline and during reliability monitoring.

Validation includes:

- Record count reconciliation
- Null key validation
- Duplicate key validation
- Primary key integrity
- Composite key integrity
- Gold table availability
- Audit run validation
- Safe rerun checks against established baselines

### Gold Data Quality Results

| Gold Table | Records | Null Keys | Duplicate Keys |
|---|---:|---:|---:|
| Fact Encounters | 61,459 | 0 | 0 |
| Patient Summary | 1,163 | 0 | 0 |
| Condition Summary | 204 | 0 | 0 |
| Procedure Summary | 178 | 0 | 0 |
| Organization Summary | 1,122 | 0 | 0 |
| Provider Summary | 1,123 | 0 | 0 |
| Payer Summary | 10 | 0 | 0 |

Result: All 7 Gold tables passed the implemented key-integrity and non-empty table checks.

## KPI & Analytical Domains

Eight analytical domains were developed using the Silver and Gold datasets.

1. Patient & Encounter Overview
2. Monthly Encounter Trends
3. Healthcare Expenditure & Claims
4. Payer Coverage & Patient Financial Responsibility
5. Provider Workload & Performance
6. Organization Utilization & Performance
7. Condition Prevalence & Analysis
8. Procedure Utilization & Cost Analysis

These analytical domains support healthcare utilization, patient, provider, organization, condition, procedure, and financial analysis.

## Batch Orchestration

The pipeline is configured as a scheduled Databricks Job.

- Job name: `healthcare_batch_pipeline`
- Execution: Scheduled batch processing
- Schedule: Daily at 6:00 AM IST
- Execution monitoring: Databricks Jobs
- Notifications: Success and failure notifications configured

The pipeline was manually executed successfully during project validation. The recurring schedule is configured.

## Monitoring & Reliability

The reliability phase included:

- Job execution monitoring
- Gold table data quality monitoring
- Persistent validation audit history
- Gold table availability checks
- Failure recovery readiness
- Safe rerun validation
- Operational runbook
- Final reliability review

### Audit Logging

Audit table:

`workspace.default.healthcare_data_quality_audit`

Audit records capture:

- Run ID
- Run timestamp
- Pipeline name
- Table name
- Actual row count
- Minimum expected row count
- Null key count
- Duplicate key count
- Validation status

Audit history is append-based to preserve records from previous validation executions.

## Project Scope

Included:

- Batch data ingestion
- Bronze, Silver, Gold architecture
- PySpark transformations
- Spark SQL analysis
- Delta tables
- Analytical data modeling
- KPI analysis
- Data quality monitoring
- Audit logging
- Job orchestration
- Reliability validation

Not included in the current implementation:

- Streaming ingestion
- Tableau dashboard development
- Production cloud deployment

## Repository Structure

```text
healthcare-data-engineering-pipeline/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── notebooks/
│
├── architecture/
│
├── sql/
│
├── docs/
│
├── data/
│   └── sample/
│
├── screenshots/
│   ├── ingestion/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   ├── kpis/
│   ├── job_orchestration/
│   └── data_quality/
│
└── reports/
    ├── data_quality_results/
    └── kpi_results/
```

## Project Outcomes

- Processed 184,539 synthetic healthcare source records across four datasets.
- Implemented a three-layer Medallion data architecture.
- Built and validated seven Gold analytical tables.
- Developed eight healthcare KPI analysis domains.
- Configured scheduled batch orchestration in Databricks.
- Implemented persistent data quality audit logging.
- Completed key-integrity and reliability validation across Gold datasets.

## Future Enhancements

- Advanced data quality thresholds and anomaly detection
- Dashboard integration
- More granular incremental processing
- Enhanced operational monitoring
- Deployment to a production-grade cloud environment

## Disclaimer

This project uses synthetic healthcare data and is intended for educational, portfolio, and demonstration purposes only. It is not a production healthcare system and should not be used for clinical decision-making.

---

**Author:** Siddharth  
**Domain:** Data Engineering | Healthcare Analytics