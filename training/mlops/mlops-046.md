---
type: procedure
created: 2026-03-17
status: active
slug: mlops-046
tags: [procedure, nexus]
---

# MLOPS-046 — Use AWS Glue for Batch Data Processing

## Problema
Manual ETL scripts for ML data preparation do not scale and lack monitoring.

## Procedura
### Prerequisites
- AWS account
- S3 data lake (MLOPS-045)
- basic PySpark knowledge

### Steps
1. Create a Glue Crawler to catalog data in S3: detect schema automatically.
2. View cataloged tables in Glue Data Catalog.
3. Write a Glue ETL job in PySpark: read from catalog, transform, write to S3.
4. Add data quality transforms: drop nulls, cast types, filter outliers.
5. Configure job bookmarks for incremental processing.
6. Schedule the Glue job with a trigger (cron or event-based).
7. Monitor job runs in Glue console and check CloudWatch logs.

### Verification
- Crawler catalogs data with correct schema.
- ETL job produces transformed output in target S3 path.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS Glue
- PySpark
- AWS S3

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
