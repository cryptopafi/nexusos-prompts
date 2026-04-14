---
type: procedure
created: 2026-03-17
status: active
slug: mlops-025
tags: [procedure, nexus]
---

# MLOPS-025 — Containerize ML Microservices for Deployment

## Problema
ML services that run on a developer laptop but fail in production need container isolation.

## Procedura
### Prerequisites
- Docker installed
- ML API code (MLOPS-012 or MLOPS-013)

### Steps
1. Write a `Dockerfile`: base image, copy code, install deps, expose port, set entrypoint.
2. Use multi-stage build: builder stage for deps, runtime stage for slim image.
3. Create `.dockerignore` to exclude `.git`, `__pycache__`, `.venv`, `data/`.
4. Build: `docker build -t ml-service:v1 .`.
5. Run: `docker run -p 8000:8000 ml-service:v1`.
6. Test the containerized service with curl requests.
7. Push to registry: `docker push <registry>/ml-service:v1`.

### Verification
- Container builds without errors.
- Service responds correctly from inside container.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Docker
- MLOPS-012 or MLOPS-013

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
