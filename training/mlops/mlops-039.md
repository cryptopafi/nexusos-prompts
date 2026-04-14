---
type: procedure
created: 2026-03-17
status: active
slug: mlops-039
tags: [procedure, nexus]
---

# MLOPS-039 — Deploy Rust Actix Microservice on Google Cloud Run

## Problema
Rust microservices need containerized deployment on managed platforms for auto-scaling ML inference.

## Procedura
### Prerequisites
- Rust toolchain
- Docker
- Google Cloud account
- gcloud CLI

### Steps
1. Write an Actix-web microservice with `/predict` and `/health` endpoints.
2. Create a multi-stage Dockerfile: build with `rust:latest`, run with `debian:slim`.
3. Build and test locally: `docker build -t ml-rust . && docker run -p 8080:8080 ml-rust`.
4. Push to Google Container Registry: `gcloud builds submit --tag gcr.io/<project>/ml-rust`.
5. Deploy to Cloud Run: `gcloud run deploy ml-rust --image gcr.io/<project>/ml-rust --port 8080`.
6. Test the deployed URL with curl.
7. Configure auto-scaling: min 0, max 10 instances.

### Verification
- Cloud Run service responds to HTTP requests.
- Service scales to zero when idle.

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
- Actix-web
- Docker
- Google Cloud Run

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
