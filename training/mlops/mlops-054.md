---
type: procedure
created: 2026-03-17
status: active
slug: mlops-054
tags: [procedure, nexus]
---

# MLOPS-054 — Select GPU vs CPU for ML Training

## Problema
Using wrong compute type wastes money (GPU for small data) or time (CPU for deep learning).

## Procedura
### Prerequisites
- AWS account
- ML training code
- understanding of model architecture

### Steps
1. Profile your training job on CPU: measure time per epoch and memory usage.
2. Identify if model uses operations that benefit from GPU (matrix multiplication, convolutions).
3. Run same job on GPU instance: compare time per epoch.
4. Calculate cost efficiency: (training time x instance cost) for CPU vs GPU.
5. Use Spot instances for cost reduction: `--instance-type ml.p3.2xlarge --use-spot`.
6. Document decision matrix: model type, data size, budget, and recommended instance.

### Verification
- Benchmark results documented for CPU and GPU.
- Cost-per-training-run calculated for both options.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS EC2/SageMaker instances
- nvidia-smi (for GPU)

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
