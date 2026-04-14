---
type: procedure
created: 2026-03-17
status: active
slug: mlops-048
tags: [procedure, nexus]
---

# MLOPS-048 — Build Map-Reduce Pipelines for ML on AWS

## Problema
Large-scale feature engineering requires distributed computation beyond single-machine capability.

## Procedura
### Prerequisites
- AWS account
- EMR cluster or Glue
- PySpark knowledge

### Steps
1. Design map phase: partition data, apply feature extraction per partition.
2. Design reduce phase: aggregate features across partitions.
3. Implement in PySpark: `rdd.map(extract_features).reduceByKey(aggregate)`.
4. Alternatively use DataFrame API: `df.groupBy().agg()`.
5. Deploy on EMR cluster with appropriate instance types.
6. Monitor job progress in Spark UI.
7. Optimize: tune partitions, use broadcast joins, cache intermediate results.

### Verification
- Map-reduce job completes on full dataset.
- Output features match expected aggregations on sample data.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS EMR
- PySpark
- Spark UI

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
