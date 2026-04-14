---
type: procedure
created: 2026-03-17
status: active
slug: mlops-055
tags: [procedure, nexus]
---

# MLOPS-055 — Track ML Experiments and Compare Models

## Problema
Without experiment tracking, teams cannot reproduce results or systematically improve models.

## Procedura
### Prerequisites
- MLflow or SageMaker Experiments
- ML training code

### Steps
1. Set up experiment tracking: `mlflow.set_experiment('my-experiment')`.
2. Log parameters: `mlflow.log_param('learning_rate', 0.001)`.
3. Log metrics: `mlflow.log_metric('accuracy', 0.95)`.
4. Log artifacts: `mlflow.log_artifact('model.pkl')`.
5. Train multiple model variants with different hyperparameters.
6. Compare runs in MLflow UI: sort by metric, visualize parameter impact.
7. Select best model and register it for deployment.

### Verification
- Multiple runs visible in experiment tracker.
- Best model identifiable by sorting on target metric.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- MLflow or SageMaker Experiments

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
