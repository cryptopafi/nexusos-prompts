---
type: procedure
created: 2026-03-17
status: active
slug: mlops-031
tags: [procedure, nexus]
---

# MLOPS-031 — Build Makefile-Based Python ML Projects

## Problema
ML projects lack standardized commands for install, test, lint, and deploy across team members.

## Procedura
### Prerequisites
- GNU Make
- Python project with tests

### Steps
1. Create a `Makefile` in project root.
2. Add `install` target: `pip install -r requirements.txt`.
3. Add `test` target: `pytest -v`.
4. Add `lint` target: `flake8 src/ && black --check src/`.
5. Add `format` target: `black src/`.
6. Add `all` target that chains: install > lint > test.
7. Add `clean` target to remove `__pycache__`, `.pytest_cache`, build artifacts.
8. Document all targets with `help` target using `@grep -E '^[a-zA-Z_-]+:.*?## '`.

### Verification
- `make all` runs full pipeline without errors.
- `make help` lists all available targets.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- GNU Make
- Python project

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
