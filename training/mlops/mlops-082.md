---
type: procedure
created: 2026-03-17
status: active
slug: mlops-082
tags: [procedure, nexus]
---

# MLOPS-082 — Register HF Models and Datasets in Azure ML Studio

## Problema
Hugging Face assets need Azure ML registration for governance, lineage, and managed deployment.

## Procedura
### Prerequisites
- Azure ML workspace (MLOPS-063)
- Hugging Face model

### Steps
1. Download HF model locally: `model.save_pretrained('./hf-model/')`.
2. Upload to Azure ML datastore: `az ml data create --name hf-bert --path ./hf-model/ --type uri_folder`.
3. Register as Azure ML model: `az ml model create --name hf-bert --path azureml://datastores/.../hf-model`.
4. Add model metadata: task type, framework, metrics from model card.
5. Similarly register datasets: `az ml data create --name imdb --path ./imdb-data/`.
6. Link model to dataset for lineage tracking.

### Verification
- Model and dataset visible in Azure ML Studio.
- Metadata and lineage correctly associated.

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
- Hugging Face
- az ml extension

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
