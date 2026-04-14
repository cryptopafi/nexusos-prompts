---
type: procedure
created: 2026-03-17
status: active
slug: mlops-070
tags: [procedure, nexus]
---

# MLOPS-070 — Run MLflow Projects from Remote Git Repositories

## Problema
Collaborators cannot reproduce experiments without cloning and configuring repositories manually.

## Procedura
### Prerequisites
- MLflow installed
- Git repository with MLproject file (MLOPS-069)

### Steps
1. Push MLflow project to GitHub with MLproject and environment files.
2. Run remotely: `mlflow run https://github.com/<user>/<repo> -P alpha=0.5`.
3. MLflow clones the repo, sets up environment, and executes the entry point.
4. Verify run tracked with git commit hash as source.
5. Run a specific branch: `mlflow run <url> --version branch-name`.
6. Run a specific entry point: `mlflow run <url> -e train`.

### Verification
- Remote project clones, installs deps, and executes.
- Run source shows git URL and commit hash.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- MLflow
- Git
- MLOPS-069

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
