---
type: procedure
created: 2026-03-17
status: active
slug: mlops-071
tags: [procedure, nexus]
---

# MLOPS-071 — Connect MLflow to Databricks

## Problema
Local MLflow tracking does not scale for team collaboration; Databricks provides managed tracking.

## Procedura
### Prerequisites
- Databricks workspace
- MLflow installed
- databricks-cli

### Steps
1. Install Databricks CLI: `pip install databricks-cli`.
2. Configure: `databricks configure --token` with workspace URL and token.
3. Set MLflow tracking URI: `mlflow.set_tracking_uri('databricks')`.
4. Create experiment in Databricks workspace.
5. Log runs from local code to Databricks-hosted MLflow.
6. Verify runs visible in Databricks ML > Experiments.
7. Access Databricks Model Registry from MLflow: `mlflow.register_model()`.

### Verification
- Runs logged from local machine appear in Databricks UI.
- Model registered in Databricks Model Registry.

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
- Databricks
- databricks-cli

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
