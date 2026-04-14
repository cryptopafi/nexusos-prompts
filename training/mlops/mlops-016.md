---
type: procedure
created: 2026-03-17
status: active
slug: mlops-016
tags: [procedure, nexus]
---

# MLOPS-016 — Use GitHub Codespaces for Cloud ML Development

## Problema
Local environment setup varies across machines and wastes onboarding time for ML projects.

## Procedura
### Prerequisites
- GitHub account
- repository with ML code

### Steps
1. Open repository on GitHub and click 'Code' > 'Codespaces' > 'Create codespace'.
2. Wait for container to build (uses `.devcontainer/devcontainer.json` if present).
3. Create `.devcontainer/devcontainer.json` with Python image and extensions.
4. Add `postCreateCommand` to install ML dependencies automatically.
5. Verify GPU availability if using ML-optimized machine type.
6. Commit `.devcontainer/` config so team members get identical environments.

### Verification
- Codespace launches with all dependencies pre-installed.
- Python and ML libraries importable without manual setup.

## Cortex Logging
- collection: procedures
- tags: mlops, training, basic

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- GitHub Codespaces
- devcontainer spec

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: BASIC
