---
type: procedure
created: 2026-03-17
status: active
slug: mlops-037
tags: [procedure, nexus]
---

# MLOPS-037 — Run PyTorch Stable Diffusion in Rust with GPU

## Problema
Python-based image generation services have high latency that Rust bindings can reduce.

## Procedura
### Prerequisites
- Rust toolchain
- GPU with 8GB+ VRAM
- libtorch installed

### Steps
1. Set up `tch-rs` with CUDA-enabled libtorch.
2. Download Stable Diffusion model weights in ONNX or TorchScript format.
3. Write Rust code to load the UNet, VAE, and text encoder components.
4. Implement the diffusion sampling loop in Rust.
5. Add prompt parsing and CLIP text encoding.
6. Generate an image and save as PNG.
7. Benchmark generation time vs Python PyTorch baseline.

### Verification
- Generated image matches expected style for given prompt.
- Rust version shows measurable speedup over Python.

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
- tch-rs
- libtorch
- CUDA
- Stable Diffusion weights

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
