---
type: procedure
created: 2026-03-17
status: active
slug: mlops-065
tags: [procedure, nexus]
---

# MLOPS-065 — Trigger Azure ML Pipelines with GitHub Actions

## Problema
ML pipelines triggered manually cannot keep models fresh or respond to data changes automatically.

## Procedura
### Prerequisites
- Azure ML workspace
- GitHub repository
- Azure service principal

### Steps
1. Create Azure service principal: `az ad sp create-for-rbac --name ml-cicd`.
2. Add service principal credentials as GitHub secrets: AZURE_CREDENTIALS.
3. Write Azure ML pipeline YAML defining training steps.
4. Create GitHub Actions workflow triggered on push or schedule.
5. Use `azure/login@v2` action with service principal credentials.
6. Submit ML pipeline: `az ml job create --file pipeline.yml`.
7. Add job monitoring step: poll for completion and check metrics.

### Verification
- GitHub Actions triggers Azure ML pipeline on push.
- Pipeline job runs to completion and logs metrics.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Azure ML
- GitHub Actions
- Azure Service Principal

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
