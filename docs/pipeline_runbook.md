# Pipeline Runbook

## Environment
- Databricks Free Edition Serverless
- Spark 4.2
- Catalog/schema: `workspace.default`
- Job: `healthcare_batch_pipeline`
- Source volume (development): `/Volumes/workspace/default/healthcare_source_data/`
- Pipeline type: batch

## Source files
Expected input datasets:
- `patients.csv`
- `encounters.csv`
- `conditions.csv`
- `procedures.csv`

## Normal execution
1. Open the Healthcare Data Engineering notebook in the Databricks workspace.
2. Confirm that the expected source files are available at the configured source volume path.
3. Run the notebook cells in sequence, or start the configured Databricks Job.
4. Review ingestion and transformation outputs for the Bronze, Silver, and Gold layers.
5. Review KPI outputs across the eight KPI domains.
6. Run the data-quality and reconciliation checks.
7. Confirm that the audit records were written to `workspace.default.healthcare_data_quality_audit`.
8. Review the Databricks Job run status and task logs.

## Layer outputs
### Bronze
- `healthcare_bronze_patients`
- `healthcare_bronze_encounters`
- `healthcare_bronze_conditions`
- `healthcare_bronze_procedures`

### Silver
- `healthcare_silver_patients`
- `healthcare_silver_encounters`
- `healthcare_silver_conditions`
- `healthcare_silver_procedures`

### Gold
- `gold_fact_encounters`
- `gold_patient_summary`
- `gold_condition_summary`
- `gold_procedure_summary`
- `gold_organization_summary`
- `gold_provider_summary`
- `gold_payer_summary`

## Monitoring checklist
- [ ] Job run completed successfully.
- [ ] All expected source files were ingested.
- [ ] Bronze, Silver, and Gold tables were created/refreshed.
- [ ] Gold row counts match the current expected baselines (where applicable).
- [ ] Null-key checks passed.
- [ ] Duplicate-key checks passed using each table's defined grain.
- [ ] Condition and procedure audits use composite keys.
- [ ] KPI outputs were generated.
- [ ] Audit results were persisted.
- [ ] No unexplained reconciliation discrepancies remain (or open issues are documented).

## Troubleshooting
### Source file not found
Check that the source file exists in the configured Databricks Volume path, that its filename and case match the notebook, and that the path is accessible from the compute environment.

### Job failed
Open the failed Job run, inspect the failed task and notebook cell output, review the exception and input path, and correct the root cause before rerunning. Do not blindly rerun when a failure may have partially written outputs; inspect the affected tables first.

### Unexpected row counts
Compare source counts, Bronze counts, Silver counts, and Gold grains. Check filtering, deduplication, joins, and aggregation logic. Confirm that changes are intentional and documented before updating baselines.

### Null or duplicate keys
Use the table's correct key definition. Conditions and procedures require composite keys (code plus description), not code alone. Investigate the affected rows and transformation logic before accepting the run.

### KPI reconciliation mismatch
Compare the metric definitions, filters, aggregation grain, source columns, and units. The expenditure mismatch previously observed in this project is an open validation issue; do not mark it resolved without re-running the reconciliation and recording evidence.

## Recovery and rerun guidance
- Identify which notebook steps completed and which tables may have been written.
- Check the latest run's audit records and table state.
- Rerun using the notebook's intended execution sequence after understanding partial writes.
- Verify output counts, key audits, reconciliation, and audit persistence after rerun.
- Record material failures and corrective actions in the project notes.

## Scheduling
The job is configured for a daily 6:00 AM IST schedule with success/failure notifications. A successful manual run and an observed successful execution have been reported. Confirm future scheduled runs in the Databricks Job run history before claiming recurring schedule reliability.
