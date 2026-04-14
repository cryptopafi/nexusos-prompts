---
type: procedure
created: 2026-03-17
status: active
slug: mlops-064
tags: [procedure, nexus]
---

# MLOPS-064 — Create Azure AutoML Job for Model Training

## Problema
Manual model selection and hyperparameter tuning is slow; AutoML automates the search process.

## Procedura
### Prerequisites
- Azure ML workspace (MLOPS-063)
- training dataset

### Steps
1. Upload training data to Azure ML datastore.
2. Create AutoML job YAML: specify task type (classification/regression), target column, dataset.
3. Configure compute target: reference the training cluster.
4. Set constraints: max trials, timeout, primary metric (e.g., AUC_weighted).
5. Submit job: `az ml job create --file automl-job.yml -g mlops-rg -w mlops-ws`.
6. Monitor in Azure ML Studio: view trial runs, metrics, and model explanations.
7. Register the best model: `az ml model create --name best-model --path <output>`.

### Verification
- AutoML job completes with multiple trial runs.
- Best model registered with metric above baseline.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Azure ML
- AutoML
- MLOPS-063

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
