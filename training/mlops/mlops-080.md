---
type: procedure
created: 2026-03-17
status: active
slug: mlops-080
tags: [procedure, nexus]
---

# MLOPS-080 — Automate Container Packaging with Azure Container Registry

## Problema
Manual ACR pushes and builds do not scale for frequent model updates across environments.

## Procedura
### Prerequisites
- Azure account
- Azure CLI
- Dockerfile for ML service

### Steps
1. Create ACR: `az acr create -n mlmodels -g mlops-rg --sku Basic`.
2. Login: `az acr login -n mlmodels`.
3. Build in ACR (no local Docker needed): `az acr build -r mlmodels -t ml-service:v1 .`.
4. List images: `az acr repository list -n mlmodels`.
5. Set up ACR Tasks for automatic builds on git push.
6. Configure geo-replication for multi-region deployment.
7. Clean up old images: `az acr run --cmd 'acr purge' -r mlmodels`.

### Verification
- Image builds and stores in ACR.
- ACR Task triggers automatic build on git push.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Azure CLI
- Azure Container Registry

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
