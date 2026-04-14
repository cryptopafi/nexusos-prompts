---
type: procedure
created: 2026-03-17
status: active
slug: mlops-076
tags: [procedure, nexus]
---

# MLOPS-076 — Use Hugging Face CLI for Model/Dataset Management

## Problema
Web-based model management does not integrate with automated MLOps pipelines.

## Procedura
### Prerequisites
- Python 3.9+
- pip install huggingface_hub

### Steps
1. Install CLI: `pip install huggingface_hub[cli]`.
2. Login: `huggingface-cli login` with access token from Settings.
3. Download model: `huggingface-cli download bert-base-uncased`.
4. Upload model: `huggingface-cli upload my-model ./model-dir`.
5. List repo files: `huggingface-cli repo-info bert-base-uncased`.
6. Create new repo: `huggingface-cli repo create my-new-model --type model`.

### Verification
- CLI login succeeds with token.
- Model upload creates accessible repository on Hub.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- huggingface_hub CLI

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
