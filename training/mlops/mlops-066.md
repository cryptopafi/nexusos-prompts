---
type: procedure
created: 2026-03-17
status: active
slug: mlops-066
tags: [procedure, nexus]
---

# MLOPS-066 — Use Hyperparameters and Train Models with Azure Python SDK

## Problema
Command-line model training does not integrate well with experiment tracking and hyperparameter sweeps.

## Procedura
### Prerequisites
- Azure ML workspace
- Python SDK v2 installed

### Steps
1. Install SDK: `pip install azure-ai-ml azure-identity`.
2. Connect to workspace: `MLClient.from_config()` or with explicit credentials.
3. Create training script with argparse for hyperparameters.
4. Define a Command job with environment, compute, and hyperparameter inputs.
5. Submit job: `ml_client.jobs.create_or_update(command_job)`.
6. Set up Sweep job for hyperparameter search: define search space, sampling method.
7. Submit sweep and retrieve best trial: `ml_client.jobs.get(sweep_job.name)`.

### Verification
- Training job logs metrics to Azure ML.
- Sweep job identifies best hyperparameter combination.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- azure-ai-ml SDK
- Azure ML workspace

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
