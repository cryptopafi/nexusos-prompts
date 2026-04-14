---
type: procedure
created: 2026-03-17
status: active
slug: mlops-057
tags: [procedure, nexus]
---

# MLOPS-057 — Deploy ML Models with Reproducible Workflows on AWS

## Problema
Ad-hoc model deployments cannot be audited, rolled back, or reproduced for compliance.

## Procedura
### Prerequisites
- AWS account
- trained model
- CI/CD pipeline (MLOPS-015)

### Steps
1. Version model artifacts in S3 with metadata (training data hash, hyperparams, metrics).
2. Create SageMaker Model from S3 artifact and container image.
3. Deploy to SageMaker endpoint with specific instance configuration.
4. Implement blue/green deployment: create new endpoint, shift traffic gradually.
5. Add automated smoke tests that run post-deployment.
6. Store deployment manifest (model version, endpoint config, test results) in git.
7. Document rollback procedure: revert to previous model version.

### Verification
- Model deploys to endpoint and passes smoke tests.
- Rollback to previous version works within 5 minutes.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS SageMaker
- AWS S3
- CI/CD pipeline

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
