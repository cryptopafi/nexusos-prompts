---
type: procedure
created: 2026-03-17
status: active
slug: mlops-034
tags: [procedure, nexus]
---

# MLOPS-034 — Set Up Rust for MLOps (AWS Cloud9)

## Problema
Rust development for ML requires specific toolchain setup that varies by cloud environment.

## Procedura
### Prerequisites
- AWS account
- Cloud9 environment

### Steps
1. Launch AWS Cloud9 environment with at least t3.medium instance.
2. Install Rust: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`.
3. Source environment: `source $HOME/.cargo/env`.
4. Verify: `rustc --version && cargo --version`.
5. Create a new project: `cargo new ml-ops-rust && cd ml-ops-rust`.
6. Add ML-relevant crates to Cargo.toml: `ndarray`, `serde`, `reqwest`.
7. Build and run: `cargo build && cargo run`.

### Verification
- Rust toolchain installed and verified.
- Sample project builds and runs without errors.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- AWS Cloud9
- Rust toolchain

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
