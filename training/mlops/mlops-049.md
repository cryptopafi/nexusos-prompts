---
type: procedure
created: 2026-03-17
status: active
slug: mlops-049
tags: [procedure, nexus]
---

# MLOPS-049 — Use EMR Serverless for Data Processing

## Problema
EMR cluster management overhead distracts from ML data engineering work.

## Procedura
### Prerequisites
- AWS account
- PySpark job code

### Steps
1. Create EMR Serverless application: `aws emr-serverless create-application --type SPARK`.
2. Package PySpark job with dependencies into a zip/egg file.
3. Upload job script and deps to S3.
4. Submit job: `aws emr-serverless start-job-run` with script location and config.
5. Monitor job run: `aws emr-serverless get-job-run`.
6. View Spark logs in S3 output location.
7. Compare cost and performance vs provisioned EMR cluster.

### Verification
- Job runs to completion on EMR Serverless.
- Output data written to expected S3 location.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS EMR Serverless
- PySpark
- AWS S3

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
