---
type: procedure
created: 2026-03-17
status: active
slug: mlops-078
tags: [procedure, nexus]
---

# MLOPS-078 — Containerize Hugging Face Models with FastAPI

## Problema
HF models need production-ready containerized APIs for scalable deployment beyond Spaces.

## Procedura
### Prerequisites
- Docker
- FastAPI
- Hugging Face model
- MLOPS-013 and MLOPS-025 completed

### Steps
1. Create FastAPI app that loads HF model at startup.
2. Define Pydantic schemas for input (text) and output (prediction, confidence).
3. Implement `/predict` endpoint using transformers pipeline.
4. Write Dockerfile: use Python slim base, install deps, copy app.
5. Pre-download model during build: `RUN python -c "from transformers import pipeline; pipeline('sentiment-analysis')"`.
6. Build and test: `docker build -t hf-api . && docker run -p 8000:8000 hf-api`.
7. Optimize: use multi-stage build, set `TRANSFORMERS_CACHE` in container.

### Verification
- Container builds with model baked in (no download at runtime).
- API returns predictions within expected latency.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Docker
- FastAPI
- transformers

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
