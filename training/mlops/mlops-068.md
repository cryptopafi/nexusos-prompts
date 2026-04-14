---
type: procedure
created: 2026-03-17
status: active
slug: mlops-068
tags: [procedure, nexus]
---

# MLOPS-068 — Use MLflow Tracking UI for Parameters and Metrics

## Problema
Logging experiments without visualization prevents systematic model comparison and selection.

## Procedura
### Prerequisites
- MLflow installed (MLOPS-067)
- ML training code

### Steps
1. Start an MLflow run within your training script.
2. Log hyperparameters: `mlflow.log_params({'lr': 0.01, 'epochs': 10, 'batch_size': 32})`.
3. Log metrics per epoch: `mlflow.log_metric('val_loss', loss, step=epoch)`.
4. Log artifacts: `mlflow.log_artifact('confusion_matrix.png')`.
5. Open MLflow UI and compare multiple runs side-by-side.
6. Use the chart view to visualize metric trends across runs.
7. Filter and sort runs to identify best configuration.

### Verification
- Multiple runs with different parameters visible in UI.
- Metric comparison charts show clear winner.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- MLflow
- MLOPS-067

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
