---
type: procedure
created: 2026-03-17
status: active
slug: mlops-059
tags: [procedure, nexus]
---

# MLOPS-059 — Deploy Hugging Face Pre-Trained Models on AWS

## Problema
Deploying HF models to production requires proper infrastructure beyond local inference scripts.

## Procedura
### Prerequisites
- AWS account
- Hugging Face model
- SageMaker access

### Steps
1. Install SageMaker SDK: `pip install sagemaker`.
2. Import HuggingFaceModel from sagemaker.huggingface.
3. Define model: specify `transformers_version`, `pytorch_version`, `model_data` (S3 path or Hub ID).
4. Create inference script `inference.py` with `model_fn` and `predict_fn`.
5. Deploy to SageMaker endpoint: `model.deploy(instance_type='ml.g4dn.xlarge', initial_instance_count=1)`.
6. Test endpoint with `predictor.predict({'inputs': 'test text'})`.
7. Configure auto-scaling policy for the endpoint.

### Verification
- Endpoint deploys and responds to inference requests.
- Auto-scaling policy attached to endpoint.

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
- Hugging Face
- sagemaker SDK

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
