---
type: procedure
created: 2026-03-17
status: active
slug: mlops-073
tags: [procedure, nexus]
---

# MLOPS-073 — Serve ML Models with MLflow Model Serving

## Problema
Registered models need a serving endpoint for real-time predictions without custom API code.

## Procedura
### Prerequisites
- MLflow with registered model (MLOPS-072)

### Steps
1. Serve locally: `mlflow models serve -m models:/MyModel/Production -p 5001`.
2. Test with curl: `curl -X POST -H 'Content-Type: application/json' -d '{"inputs": [...]}' http://localhost:5001/invocations`.
3. Build Docker container: `mlflow models build-docker -m models:/MyModel/1 -n my-model`.
4. Run containerized: `docker run -p 5001:8080 my-model`.
5. Test containerized endpoint with same curl command.
6. Deploy container to cloud platform (ECS, Cloud Run, or ACI).

### Verification
- Local serving endpoint returns predictions.
- Docker container serves model identically.

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
- Docker
- MLOPS-072

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
