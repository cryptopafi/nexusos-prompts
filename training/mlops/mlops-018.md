---
type: procedure
created: 2026-03-17
status: active
slug: mlops-018
tags: [procedure, nexus]
---

# MLOPS-018 — Fine-Tune Models with Hugging Face in Codespaces

## Problema
Fine-tuning requires GPU access and environment setup that blocks experimentation velocity.

## Procedura
### Prerequisites
- GitHub Codespaces with GPU
- Hugging Face account
- MLOPS-008 completed

### Steps
1. Launch a GPU-enabled Codespace (4-core + GPU machine type).
2. Install: `pip install transformers datasets accelerate`.
3. Load a pre-trained model and tokenizer for your task.
4. Prepare dataset with `datasets.load_dataset()` and tokenize with `.map()`.
5. Configure `TrainingArguments` with epochs, batch size, learning rate, output dir.
6. Create `Trainer` instance and call `trainer.train()`.
7. Evaluate with `trainer.evaluate()` and save model with `trainer.save_model()`.

### Verification
- Training loss decreases across epochs.
- Evaluation metrics improve over baseline.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- transformers
- datasets
- accelerate
- GPU Codespace

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
