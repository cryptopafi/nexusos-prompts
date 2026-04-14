---
type: procedure
created: 2026-03-17
status: active
slug: mlops-030
tags: [procedure, nexus]
---

# MLOPS-030 — Implement Transfer Learning for Custom Models

## Problema
Training models from scratch requires massive data and compute that most teams do not have.

## Procedura
### Prerequisites
- Python 3.9+
- PyTorch or TensorFlow
- pre-trained model
- custom dataset

### Steps
1. Load a pre-trained model (e.g., ResNet, BERT) with pretrained weights.
2. Freeze base layers: `for param in model.parameters(): param.requires_grad = False`.
3. Replace the final classification layer to match your number of classes.
4. Prepare your custom dataset with appropriate transforms/tokenization.
5. Train only the new layers for a few epochs with a small learning rate.
6. Unfreeze some base layers and fine-tune with an even smaller learning rate.
7. Evaluate on held-out test set and compare against baseline.

### Verification
- Fine-tuned model outperforms random initialization baseline.
- Training loss converges within expected epochs.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- PyTorch or TensorFlow
- pre-trained model weights
- custom labeled dataset

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
