---
type: procedure
created: 2026-03-17
status: active
slug: mlops-047
tags: [procedure, nexus]
---

# MLOPS-047 — Implement AWS Batch vs Streaming Job Architectures

## Problema
Choosing wrong processing pattern (batch vs stream) leads to latency issues or wasted compute.

## Procedura
### Prerequisites
- AWS account
- understanding of data arrival patterns

### Steps
1. Identify data characteristics: volume, velocity, processing latency requirements.
2. Implement batch pattern: S3 > Glue ETL > S3 (for daily model retraining).
3. Implement streaming pattern: Kinesis > Lambda > DynamoDB (for real-time features).
4. Compare: latency, cost, complexity, and fault tolerance of each approach.
5. Implement a hybrid: streaming for features, batch for model training.
6. Document decision criteria in an Architecture Decision Record (ADR).

### Verification
- Both batch and streaming pipelines process sample data correctly.
- ADR documents trade-offs and chosen pattern.

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
- AWS Kinesis
- AWS Lambda

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
