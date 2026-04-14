---
type: procedure
created: 2026-03-17
status: active
slug: mlops-077
tags: [procedure, nexus]
---

# MLOPS-077 — Add and Use Hugging Face Datasets

## Problema
Custom datasets need Hub integration for versioning, sharing, and use in training pipelines.

## Procedura
### Prerequisites
- Hugging Face account
- pip install datasets

### Steps
1. Create dataset loading script or use CSV/JSON/Parquet files.
2. Upload dataset: `huggingface-cli upload my-dataset ./data-dir --repo-type dataset`.
3. Load your dataset: `ds = load_dataset('<user>/my-dataset')`.
4. Add dataset card README.md with description, features, and usage.
5. Version dataset with git tags on the Hub repository.
6. Stream large datasets: `load_dataset('<user>/my-dataset', streaming=True)`.

### Verification
- Dataset uploads and loads from Hub without errors.
- Dataset card visible on Hub page.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- datasets library
- huggingface_hub

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
