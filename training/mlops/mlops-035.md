---
type: procedure
created: 2026-03-17
status: active
slug: mlops-035
tags: [procedure, nexus]
---

# MLOPS-035 — Build CI/CD for Rust with GitHub Actions

## Problema
Rust ML projects need automated build, test, and release pipelines like Python projects.

## Procedura
### Prerequisites
- Rust project with tests
- GitHub repository

### Steps
1. Create `.github/workflows/rust-ci.yml`.
2. Define matrix: `os: [ubuntu-latest, macos-latest]`, stable Rust toolchain.
3. Cache cargo registry and build artifacts with `actions/cache@v4`.
4. Add steps: `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test`.
5. Add release workflow that builds binaries on tag push.
6. Upload release artifacts with `actions/upload-artifact@v4`.

### Verification
- CI passes on push with fmt, clippy, and test steps.
- Release workflow produces downloadable binaries.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Rust
- GitHub Actions
- cargo

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
