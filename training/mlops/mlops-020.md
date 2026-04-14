---
type: procedure
created: 2026-03-17
status: active
slug: mlops-020
tags: [procedure, nexus]
---

# MLOPS-020 — Query Databricks Pipeline for DataOps

## Problema
Data teams cannot efficiently query and transform ML training data stored in lakehouse architectures.

## Procedura
### Prerequisites
- Databricks workspace access
- basic SQL knowledge

### Steps
1. Connect to Databricks workspace and open SQL editor.
2. Create a database and table: `CREATE TABLE ml_features USING DELTA`.
3. Load data into Delta table from cloud storage.
4. Query with SQL: joins, aggregations, window functions for feature computation.
5. Create a Databricks notebook pipeline with sequential SQL cells.
6. Schedule the notebook as a job with triggers and alerts.

### Verification
- Queries return expected results against Delta tables.
- Scheduled job runs on time without failures.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Databricks
- Delta Lake
- SQL

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
