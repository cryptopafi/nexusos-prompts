---
type: procedure
created: 2026-03-17
status: active
slug: mlops-023
tags: [procedure, nexus]
---

# MLOPS-023 — Deploy Google Cloud Functions from Zero

## Problema
Setting up serverless endpoints for ML inference on GCP is blocked by unfamiliar tooling.

## Procedura
### Prerequisites
- Google Cloud account
- gcloud CLI installed

### Steps
1. Authenticate: `gcloud auth login` and set project: `gcloud config set project <id>`.
2. Write a Cloud Function in `main.py` with `def predict(request):` entry point.
3. Create `requirements.txt` with function dependencies.
4. Deploy: `gcloud functions deploy predict --runtime python311 --trigger-http --allow-unauthenticated`.
5. Test with `curl` using the deployed URL.
6. View logs: `gcloud functions logs read predict`.

### Verification
- Function deploys without errors.
- HTTP request returns expected prediction response.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- gcloud CLI
- Google Cloud Functions

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
