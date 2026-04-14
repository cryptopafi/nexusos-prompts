---
type: procedure
created: 2026-03-17
status: active
slug: mlops-021
tags: [procedure, nexus]
---

# MLOPS-021 — Build Data Ingestion Pipelines on AWS

## Problema
Manual data uploads do not scale and create bottlenecks for ML training data freshness.

## Procedura
### Prerequisites
- AWS account
- S3 bucket
- data source (API, database, or files)

### Steps
1. Set up an S3 landing bucket with lifecycle policies.
2. Create a Lambda function triggered by S3 PUT events for ingestion validation.
3. Use AWS Glue Crawler to catalog incoming data schema automatically.
4. Build a Glue ETL job to transform raw data into ML-ready Parquet format.
5. Add data quality checks: schema validation, null checks, range validation.
6. Set up EventBridge rule to trigger pipeline on schedule.
7. Monitor with CloudWatch dashboards for ingestion latency and error rates.

### Verification
- Data lands in S3 and triggers automated processing.
- Glue catalog shows correct schema for ingested data.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS S3
- AWS Lambda
- AWS Glue
- AWS EventBridge

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
