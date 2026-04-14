---
type: procedure
created: 2026-03-17
status: active
slug: mlops-040
tags: [procedure, nexus]
---

# MLOPS-040 — Deploy Rust Microservice via Google App Engine

## Problema
Some ML workloads need App Engine's managed infrastructure instead of Cloud Run's container model.

## Procedura
### Prerequisites
- Rust toolchain
- Google Cloud account
- gcloud CLI

### Steps
1. Write Rust HTTP service using Actix-web listening on PORT env variable.
2. Create `app.yaml` for App Engine flex environment with custom runtime.
3. Write Dockerfile for the custom runtime.
4. Deploy: `gcloud app deploy`.
5. View logs: `gcloud app logs tail -s ml-rust`.
6. Test with the App Engine URL: `https://<project>.appspot.com/predict`.

### Verification
- App Engine deployment succeeds.
- Service responds to requests at appspot.com URL.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Rust
- Google App Engine
- Docker

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
