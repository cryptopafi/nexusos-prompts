---
type: procedure
created: 2026-03-17
status: active
slug: mlops-081
tags: [procedure, nexus]
---

# MLOPS-081 — Automate Container Packaging with Docker Hub

## Problema
Teams without cloud-native registries need Docker Hub automation for ML container distribution.

## Procedura
### Prerequisites
- Docker Hub account
- Dockerfile
- GitHub repository

### Steps
1. Create Docker Hub repository for your ML service.
2. Set up GitHub Actions with `docker/login-action@v3` and Docker Hub credentials.
3. Use `docker/build-push-action@v5` with tags and push enabled.
4. Implement multi-platform builds: `platforms: linux/amd64,linux/arm64`.
5. Add Docker Hub webhook for deployment notifications.
6. Tag with semver and latest: `ml-service:1.0.0` and `ml-service:latest`.
7. Set up automated vulnerability scanning with Docker Scout.

### Verification
- Images push to Docker Hub on git push.
- Multi-platform images available for amd64 and arm64.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Docker Hub
- GitHub Actions

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
