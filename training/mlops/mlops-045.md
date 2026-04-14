---
type: procedure
created: 2026-03-17
status: active
slug: mlops-045
tags: [procedure, nexus]
---

# MLOPS-045 — Create Data Lakes with Amazon S3

## Problema
Unstructured ML data scattered across systems cannot be queried or versioned for reproducible training.

## Procedura
### Prerequisites
- AWS account
- AWS CLI configured

### Steps
1. Create S3 bucket with versioning: `aws s3api create-bucket --bucket ml-data-lake`.
2. Define folder structure: `raw/`, `processed/`, `features/`, `models/`.
3. Set lifecycle policies: transition old data to Glacier after 90 days.
4. Enable server-side encryption with AWS KMS.
5. Upload data with partitioning: `s3://bucket/raw/year=2026/month=03/data.parquet`.
6. Configure S3 event notifications for automated processing triggers.
7. Set up AWS Lake Formation for access control and governance.

### Verification
- Data uploads to correct partitioned paths.
- Lifecycle policies and encryption configured correctly.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS S3
- AWS KMS
- AWS Lake Formation

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
