---
type: procedure
created: 2026-03-17
status: active
slug: mlops-036
tags: [procedure, nexus]
---

# MLOPS-036 — Build Rust Hugging Face Translator with GPU

## Problema
High-performance translation services need Rust's speed with Hugging Face model access.

## Procedura
### Prerequisites
- Rust toolchain
- GPU-enabled machine
- MLOPS-034 completed

### Steps
1. Add `rust-bert` crate to Cargo.toml for Hugging Face model support.
2. Download a translation model (e.g., Helsinki-NLP/opus-mt-en-fr).
3. Write Rust code to load model and tokenizer from local cache.
4. Implement translation function with GPU acceleration via `tch-rs` (libtorch bindings).
5. Build REST API with `actix-web` wrapping the translation function.
6. Benchmark: compare latency and throughput vs Python equivalent.
7. Add batch translation support for multiple sentences.

### Verification
- Translation API returns correct French text for English input.
- GPU utilization visible during inference.

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
- rust-bert
- tch-rs
- libtorch
- GPU

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
