---
type: procedure
created: 2026-03-17
status: active
slug: mlops-063
tags: [procedure, nexus]
---

# MLOPS-063 — Create Azure ML Workspace for MLOps

## Problema
Azure ML projects need a centralized workspace before any training, tracking, or deployment.

## Procedura
### Prerequisites
- Azure account
- Azure CLI with ml extension (MLOPS-007)

### Steps
1. Create resource group: `az group create -n mlops-rg -l eastus`.
2. Create workspace: `az ml workspace create -n mlops-ws -g mlops-rg`.
3. Verify: `az ml workspace show -n mlops-ws -g mlops-rg`.
4. Create compute instance for development: `az ml compute create --type ComputeInstance -n dev-vm`.
5. Create compute cluster for training: `az ml compute create --type AmlCompute -n train-cluster`.
6. Register a datastore pointing to Azure Blob Storage.

### Verification
- Workspace shows in Azure portal with compute resources.
- Datastore connection verified.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Azure CLI
- az ml extension
- Azure subscription

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
