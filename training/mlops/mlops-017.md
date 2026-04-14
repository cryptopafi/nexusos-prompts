---
type: procedure
created: 2026-03-17
status: active
slug: mlops-017
tags: [procedure, nexus]
---

# MLOPS-017 — Use GitHub Templates for ML Project Scaffolding

## Problema
Starting ML projects from scratch leads to inconsistent structure and missing best practices.

## Procedura
### Prerequisites
- GitHub account

### Steps
1. Create a template repository with standard ML project structure.
2. Include: `src/`, `tests/`, `data/`, `models/`, `notebooks/`, `Makefile`, `Dockerfile`.
3. Add `pyproject.toml` with common ML dependencies.
4. Add `.github/workflows/ci.yml` for automated testing.
5. Mark repository as 'Template repository' in Settings.
6. Create new project from template: 'Use this template' > 'Create a new repository'.
7. Customize the generated project for your specific ML use case.

### Verification
- New repository created from template with full structure.
- CI workflow runs on first push.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- GitHub
- MLOPS-015

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
