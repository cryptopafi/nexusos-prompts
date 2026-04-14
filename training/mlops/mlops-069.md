---
type: procedure
created: 2026-03-17
status: active
slug: mlops-069
tags: [procedure, nexus]
---

# MLOPS-069 — Create and Run MLflow Projects

## Problema
ML code without standardized project format cannot be shared or executed reproducibly.

## Procedura
### Prerequisites
- MLflow installed
- ML training code

### Steps
1. Create `MLproject` file defining entry points, parameters, and environment.
2. Define conda or pip environment in `conda.yaml` or `python_env.yaml`.
3. Set up entry point: `main` with parameters like `alpha`, `l1_ratio`.
4. Run locally: `mlflow run . -P alpha=0.5`.
5. Override parameters: `mlflow run . -P alpha=0.1 -P l1_ratio=0.1`.
6. Verify run appears in MLflow tracking UI with logged parameters.

### Verification
- MLflow project runs with specified parameters.
- Run logged to tracking server with correct metadata.

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
