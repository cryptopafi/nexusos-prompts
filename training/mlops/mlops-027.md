---
type: procedure
created: 2026-03-17
status: active
slug: mlops-027
tags: [procedure, nexus]
---

# MLOPS-027 — Build End-to-End Containerized ML Solutions

## Problema
Fragmented ML workflows across notebooks, scripts, and manual deploys cannot be reproduced or scaled.

## Procedura
### Prerequisites
- Docker
- Docker Compose
- MLOPS-025 and MLOPS-026 completed

### Steps
1. Define `docker-compose.yml` with services: api, model-training, data-pipeline, monitoring.
2. Create shared volume for model artifacts between training and serving containers.
3. Add health checks for each service in compose configuration.
4. Implement model versioning: tag containers with model version.
5. Add a model registry service (MLflow or custom) to the compose stack.
6. Write end-to-end test: train > register > deploy > predict.
7. Document the full pipeline in a Makefile with targets for each stage.

### Verification
- docker-compose up starts all services.
- End-to-end test passes: training through prediction.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Docker Compose
- MLOPS-025
- MLOPS-026

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
