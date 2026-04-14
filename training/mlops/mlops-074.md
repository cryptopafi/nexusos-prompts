---
type: procedure
created: 2026-03-17
status: active
slug: mlops-074
tags: [procedure, nexus]
---

# MLOPS-074 — Navigate Hugging Face Hub

## Problema
New practitioners waste time searching for models instead of using Hub's search and filtering tools.

## Procedura
### Prerequisites
- Web browser
- Hugging Face account (free)

### Steps
1. Browse huggingface.co/models and filter by task (text-classification, image-classification).
2. Filter by library (PyTorch, TensorFlow), language, and license.
3. Read model card: check performance metrics, training data, and limitations.
4. Check the Files tab to understand model artifact structure.
5. Star useful models and create collections for your projects.
6. Browse huggingface.co/datasets for training data with similar filters.

### Verification
- Found relevant models filtered by task and library.
- Read model card and understand model capabilities/limitations.

## Cortex Logging
- collection: procedures
- tags: mlops, training, basic

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Hugging Face account

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: BASIC
