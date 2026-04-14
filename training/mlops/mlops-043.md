---
type: procedure
created: 2026-03-17
status: active
slug: mlops-043
tags: [procedure, nexus]
---

# MLOPS-043 — Set Up AWS SageMaker Studio Lab for ML

## Problema
New practitioners need a free GPU-enabled Jupyter environment without AWS billing complexity.

## Procedura
### Prerequisites
- AWS account or SageMaker Studio Lab account

### Steps
1. Sign up at studiolab.sagemaker.aws (free, no AWS account required).
2. Launch a new project and select CPU or GPU runtime.
3. Open JupyterLab and create a new notebook.
4. Install ML libraries: `%pip install scikit-learn pandas matplotlib`.
5. Run a quick ML experiment: load data, train model, plot results.
6. Save notebook and verify persistence across sessions.

### Verification
- JupyterLab opens with selected runtime.
- Installed packages persist after restart.

## Cortex Logging
- collection: procedures
- tags: mlops, training, basic

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS SageMaker Studio Lab account

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: BASIC
