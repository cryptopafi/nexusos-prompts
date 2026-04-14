---
type: procedure
created: 2026-03-17
status: active
slug: mlops-067
tags: [procedure, nexus]
---

# MLOPS-067 — Install and Configure MLflow for Experiment Tracking

## Problema
Without a tracking server, ML experiments are lost and cannot be compared or reproduced.

## Procedura
### Prerequisites
- Python 3.9+

### Steps
1. Install MLflow: `pip install mlflow`.
2. Start tracking server: `mlflow server --host 0.0.0.0 --port 5000`.
3. Set tracking URI: `mlflow.set_tracking_uri('http://localhost:5000')`.
4. Create an experiment: `mlflow.create_experiment('my-ml-project')`.
5. Log a test run: `with mlflow.start_run(): mlflow.log_param('test', True)`.
6. Verify in MLflow UI at http://localhost:5000.

### Verification
- MLflow UI displays the test experiment and run.
- Parameters and metrics visible in run detail.

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

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
