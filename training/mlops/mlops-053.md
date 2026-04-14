---
type: procedure
created: 2026-03-17
status: active
slug: mlops-053
tags: [procedure, nexus]
---

# MLOPS-053 — Train Models with SageMaker Canvas (No-Code ML)

## Problema
Business analysts need ML predictions but lack coding skills for model training.

## Procedura
### Prerequisites
- AWS account with SageMaker access
- CSV dataset

### Steps
1. Open SageMaker Canvas from SageMaker console.
2. Create a new dataset by uploading CSV or connecting to S3.
3. Select target column for prediction.
4. Choose model type: Quick Build (2-15 min) or Standard Build (2-4 hours).
5. Review model metrics: accuracy, F1, RMSE depending on problem type.
6. Generate predictions on new data using the built model.
7. Export model to SageMaker Studio for further tuning if needed.

### Verification
- Canvas trains model without errors.
- Prediction results downloadable as CSV.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS SageMaker Canvas

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
