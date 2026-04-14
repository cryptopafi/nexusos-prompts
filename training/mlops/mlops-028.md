---
type: procedure
created: 2026-03-17
status: active
slug: mlops-028
tags: [procedure, nexus]
---

# MLOPS-028 — Build Distroless Containers for ML

## Problema
Standard container images include unnecessary OS packages that increase attack surface and image size.

## Procedura
### Prerequisites
- Docker
- understanding of multi-stage builds
- MLOPS-025 completed

### Steps
1. Start with a multi-stage Dockerfile: builder uses full Python image.
2. Install all dependencies in builder stage.
3. Copy only runtime artifacts to `gcr.io/distroless/python3` base image.
4. Ensure no shell, package manager, or debug tools exist in final image.
5. Build and compare image sizes: standard vs distroless.
6. Test that ML inference works correctly in the minimal container.
7. Scan both images with `docker scout` or `trivy` and compare vulnerability counts.

### Verification
- Distroless image is significantly smaller than standard image.
- Vulnerability scan shows fewer CVEs.

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
- gcr.io/distroless images
- trivy or docker scout

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
