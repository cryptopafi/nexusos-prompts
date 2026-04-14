---
type: procedure
created: 2026-03-17
status: active
slug: mlops-026
tags: [procedure, nexus]
---

# MLOPS-026 — Build Containerized Continuous Delivery Pipelines

## Problema
Manual container builds and deploys create inconsistency and slow down ML model release cycles.

## Procedura
### Prerequisites
- Docker
- GitHub Actions
- container registry
- MLOPS-025 completed

### Steps
1. Create GitHub Actions workflow triggered on main branch push.
2. Add Docker build step with caching: `docker/build-push-action@v5`.
3. Tag images with git SHA and latest: `registry/ml-service:${{ github.sha }}`.
4. Push to container registry (Docker Hub, ECR, or GHCR).
5. Add deployment step: update Kubernetes manifest or ECS task definition.
6. Run integration tests against deployed container.
7. Add rollback step on test failure.

### Verification
- New commits trigger automatic build, push, and deploy.
- Integration tests pass against deployed container.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Docker
- GitHub Actions
- container registry
- MLOPS-025

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
