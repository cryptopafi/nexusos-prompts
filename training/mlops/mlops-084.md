---
type: procedure
created: 2026-03-17
status: active
slug: mlops-084
tags: [procedure, nexus]
---

# MLOPS-084 — Troubleshoot Container Deployments for ML Models

## Problema
ML container deployments fail with opaque errors that block production releases.

## Procedura
### Prerequisites
- Docker
- deployed container (any platform)

### Steps
1. Check container logs: `docker logs <container>` or cloud platform equivalent.
2. Debug startup failures: run container interactively `docker run -it --entrypoint bash <image>`.
3. Check memory limits: ML models may exceed container memory allocation.
4. Verify port mapping: container's EXPOSE matches platform's expected port.
5. Test dependency resolution: `docker run <image> pip list` to verify packages.
6. Check model file paths: ensure model artifacts copied correctly in Dockerfile.
7. Monitor resource usage: `docker stats` for CPU, memory, and network.

### Verification
- Root cause identified for container failure.
- Container runs successfully after fix.

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
- cloud platform CLI

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
