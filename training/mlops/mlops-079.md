---
type: procedure
created: 2026-03-17
status: active
slug: mlops-079
tags: [procedure, nexus]
---

# MLOPS-079 — Set Up CI/CD Packaging with GitHub Actions for HF Models

## Problema
Manual model packaging and container builds block frequent model updates in production.

## Procedura
### Prerequisites
- GitHub repo
- Dockerfile for HF model
- container registry

### Steps
1. Create `.github/workflows/model-package.yml`.
2. Trigger on push to main or model file changes.
3. Build Docker image with model baked in.
4. Run automated tests: load model, run inference on test inputs, assert outputs.
5. Tag image with git SHA and model version.
6. Push to container registry (GHCR, ECR, or Docker Hub).
7. Add workflow status badge to README.

### Verification
- Workflow triggers on model file changes.
- Container image pushed to registry with correct tags.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- GitHub Actions
- Docker
- container registry

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
