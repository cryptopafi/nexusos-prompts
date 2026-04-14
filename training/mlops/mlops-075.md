---
type: procedure
created: 2026-03-17
status: active
slug: mlops-075
tags: [procedure, nexus]
---

# MLOPS-075 — Download and Use Hugging Face Models Programmatically

## Problema
Manual model downloads break reproducibility; programmatic access enables automated pipelines.

## Procedura
### Prerequisites
- Python 3.9+
- pip install transformers torch

### Steps
1. Download model: `model = AutoModel.from_pretrained('bert-base-uncased')`.
2. Download tokenizer: `tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased')`.
3. Cache management: models stored in `~/.cache/huggingface/` by default.
4. Set custom cache: `TRANSFORMERS_CACHE=/data/models` environment variable.
5. Save model locally: `model.save_pretrained('./my-model/')`.
6. Load from local path: `model = AutoModel.from_pretrained('./my-model/')`.

### Verification
- Model downloads and loads without errors.
- Local save/load produces identical outputs.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- transformers
- torch

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
