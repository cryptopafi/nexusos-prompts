---
type: procedure
created: 2026-03-17
status: active
slug: mlops-022
tags: [procedure, nexus]
---

# MLOPS-022 — Build Serverless Data Engineering Pipelines on AWS

## Problema
Server-based ETL requires provisioning and maintenance overhead that slows ML data pipeline iteration.

## Procedura
### Prerequisites
- AWS account
- AWS CLI
- MLOPS-021 completed

### Steps
1. Design pipeline: S3 trigger > Lambda > SQS > Lambda > DynamoDB/S3.
2. Create SQS queue for decoupling ingestion from processing.
3. Write Lambda producer that reads S3 events and enqueues messages.
4. Write Lambda consumer that processes messages and writes results.
5. Configure dead-letter queue for failed messages.
6. Deploy with SAM or CloudFormation template.
7. Load test with sample data and verify end-to-end throughput.

### Verification
- Messages flow through SQS without loss.
- Dead-letter queue captures failed messages for inspection.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS Lambda
- AWS SQS
- AWS SAM
- AWS CloudFormation

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
