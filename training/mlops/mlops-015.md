---
type: procedure
created: 2026-03-17
status: active
slug: mlops-015
tags: [procedure, nexus]
---

# MLOPS-015 — Set Up CI/CD for ML Projects with GitHub Actions

## Problema
ML code merged without automated testing and linting leads to broken pipelines in production.

## Procedura
### Prerequisites
- GitHub repository
- pytest tests written
- MLOPS-001 completed

### Steps
1. Create `.github/workflows/ci.yml` in your repository.
2. Define trigger: `on: [push, pull_request]` for main branch.
3. Set up Python: use `actions/setup-python@v5` with matrix for Python versions.
4. Install dependencies: `pip install -r requirements.txt`.
5. Add lint step: `pip install flake8 && flake8 src/`.
6. Add test step: `pytest -v --tb=short`.
7. Push and verify workflow runs green in GitHub Actions tab.

### Verification
- GitHub Actions workflow runs on push.
- All lint and test steps pass with green checkmarks.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- GitHub Actions
- pytest
- flake8

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
