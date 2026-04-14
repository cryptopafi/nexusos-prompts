---
type: procedure
created: 2026-03-17
status: active
slug: mlops-044
tags: [procedure, nexus]
---

# MLOPS-044 — Use AWS CloudShell for ML Prototyping

## Problema
Quick AWS ML experiments are blocked by local CLI setup and credential management.

## Procedura
### Prerequisites
- AWS account with console access

### Steps
1. Open AWS CloudShell from the console toolbar.
2. Verify pre-installed tools: `aws --version`, `python3 --version`, `pip3 --version`.
3. Install ML libraries: `pip3 install --user scikit-learn pandas`.
4. Write a quick ML script: train a model on sample data.
5. Use `aws s3 cp` to upload/download data and models.
6. Save scripts to CloudShell's persistent 1GB home directory.

### Verification
- CloudShell opens with AWS credentials pre-configured.
- ML script runs and produces output.

## Cortex Logging
- collection: procedures
- tags: mlops, training, basic

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS CloudShell

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: BASIC
