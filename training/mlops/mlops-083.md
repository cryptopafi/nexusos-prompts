---
type: procedure
created: 2026-03-17
status: active
slug: mlops-083
tags: [procedure, nexus]
---

# MLOPS-083 — Deploy Hugging Face Models to Azure Container Apps

## Problema
Azure Container Apps offers simpler scaling than AKS for HF model serving at moderate scale.

## Procedura
### Prerequisites
- Azure account
- Containerized HF model (MLOPS-078)
- Azure CLI

### Steps
1. Push container to ACR (MLOPS-080).
2. Create Container Apps environment: `az containerapp env create`.
3. Deploy: `az containerapp create --name hf-model --image mlmodels.azurecr.io/hf-api:latest`.
4. Configure ingress: `--ingress external --target-port 8000`.
5. Set scaling rules: `--min-replicas 0 --max-replicas 10`.
6. Add health probe: `--health-probes '[{"path":"/health","type":"liveness"}]'`.
7. Test deployed endpoint and verify auto-scaling behavior.

### Verification
- Container App deploys and serves predictions.
- Scales to zero when idle, scales up under load.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Azure Container Apps
- Azure Container Registry
- MLOPS-078

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
