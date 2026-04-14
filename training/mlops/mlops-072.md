---
type: procedure
created: 2026-03-17
status: active
slug: mlops-072
tags: [procedure, nexus]
---

# MLOPS-072 — Package and Register Models with MLflow

## Problema
Trained models without standardized packaging cannot be versioned, shared, or deployed consistently.

## Procedura
### Prerequisites
- MLflow with tracking server
- trained model

### Steps
1. Log model during training: `mlflow.sklearn.log_model(model, 'model')` (or pytorch/tensorflow).
2. Register model: `mlflow.register_model('runs:/<run-id>/model', 'MyModel')`.
3. View registered model in MLflow UI Model Registry.
4. Transition model version to Staging: `client.transition_model_version_stage('MyModel', 1, 'Staging')`.
5. Load model by version: `mlflow.pyfunc.load_model('models:/MyModel/Staging')`.
6. Promote to Production after validation.

### Verification
- Model registered with version number in registry.
- Model loadable by stage name (Staging/Production).

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- MLflow Model Registry

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
