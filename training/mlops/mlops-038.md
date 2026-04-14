---
type: procedure
created: 2026-03-17
status: active
slug: mlops-038
tags: [procedure, nexus]
---

# MLOPS-038 — Deploy Rust ONNX Model on AWS Lambda with EFS

## Problema
Large ML models exceed Lambda's 250MB package limit, requiring EFS for model storage.

## Procedura
### Prerequisites
- AWS account
- Rust toolchain
- ONNX model file

### Steps
1. Create an EFS file system in the same VPC as your Lambda.
2. Upload ONNX model to EFS mount point.
3. Write Rust Lambda handler using `lambda_runtime` crate.
4. Add `ort` (ONNX Runtime) crate for model inference.
5. Configure Lambda to mount EFS and load model from `/mnt/models/`.
6. Cross-compile for Lambda: `cargo lambda build --release`.
7. Deploy with `cargo lambda deploy` and test invocation.

### Verification
- Lambda cold start loads model from EFS successfully.
- Inference returns correct predictions within timeout.

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
- AWS Lambda
- AWS EFS
- ONNX Runtime
- cargo-lambda

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
