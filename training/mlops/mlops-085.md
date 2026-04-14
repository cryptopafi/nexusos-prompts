---
type: procedure
created: 2026-03-17
status: active
slug: mlops-085
tags: [procedure, nexus]
---

# MLOPS-085 — Fine-Tune Hugging Face Models

## Problema
Pre-trained models need domain adaptation to achieve production-quality results on specific tasks.

## Procedura
### Prerequisites
- GPU machine
- Hugging Face model
- labeled dataset
- MLOPS-008 completed

### Steps
1. Select base model appropriate for your task (e.g., bert-base for classification).
2. Prepare dataset: tokenize with model's tokenizer, create train/eval splits.
3. Configure TrainingArguments: learning_rate, num_epochs, batch_size, warmup_steps.
4. Create Trainer with model, args, datasets, and compute_metrics function.
5. Train: `trainer.train()` and monitor loss curves.
6. Evaluate: `trainer.evaluate()` on held-out test set.
7. Save fine-tuned model: `trainer.save_model('./fine-tuned-model/')`.

### Verification
- Training loss decreases consistently.
- Evaluation metrics exceed pre-trained baseline.

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
- GPU
- accelerate

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
