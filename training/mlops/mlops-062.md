---
type: procedure
created: 2026-03-17
status: active
slug: mlops-062
tags: [procedure, nexus]
---

# MLOPS-062 — Deploy PyTorch Models with AWS App Runner

## Problema
SageMaker endpoints are overkill for simple PyTorch inference services that need auto-scaling.

## Procedura
### Prerequisites
- AWS account
- PyTorch model
- Docker
- MLOPS-025 completed

### Steps
1. Write FastAPI app that loads PyTorch model and serves predictions.
2. Create Dockerfile with PyTorch CPU image (lighter than GPU for inference).
3. Build and test locally: `docker run -p 8080:8080 pytorch-service`.
4. Push image to ECR: `aws ecr get-login-password | docker login && docker push`.
5. Create App Runner service: `aws apprunner create-service` with ECR source.
6. Configure auto-scaling: min 1, max 10, target concurrent requests per instance.
7. Test deployed service URL and verify cold start latency.

### Verification
- App Runner service deploys from ECR image.
- Service auto-scales under load.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS App Runner
- AWS ECR
- PyTorch
- FastAPI

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
