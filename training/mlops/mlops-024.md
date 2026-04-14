---
type: procedure
created: 2026-03-17
status: active
slug: mlops-024
tags: [procedure, nexus]
---

# MLOPS-024 — Deploy Rust Azure Function with GitHub Actions

## Problema
Rust-based ML inference functions need automated build and deploy pipelines for Azure.

## Procedura
### Prerequisites
- Rust toolchain installed
- Azure account
- GitHub repository

### Steps
1. Create Azure Function project with `func init --worker-runtime custom`.
2. Write Rust handler using `azure_functions` crate or custom HTTP handler.
3. Add `Cargo.toml` with dependencies and build configuration.
4. Create GitHub Actions workflow with Rust build, test, and deploy steps.
5. Configure Azure credentials as GitHub secrets.
6. Deploy using `azure/functions-action@v1` in the workflow.
7. Verify function responds correctly in Azure portal.

### Verification
- GitHub Actions workflow builds and deploys on push.
- Azure Function responds to HTTP requests.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Rust
- Azure Functions
- GitHub Actions

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
