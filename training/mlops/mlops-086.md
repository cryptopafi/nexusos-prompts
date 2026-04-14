---
type: procedure
created: 2026-03-17
status: active
slug: mlops-086
tags: [procedure, nexus]
---

# MLOPS-086 — Export Hugging Face Models to ONNX Format

## Problema
Native HF models have slow inference; ONNX enables hardware-optimized execution across platforms.

## Procedura
### Prerequisites
- Hugging Face model
- pip install optimum onnx onnxruntime

### Steps
1. Install Optimum: `pip install optimum[onnxruntime]`.
2. Export model: `optimum-cli export onnx --model bert-base-uncased ./onnx-model/`.
3. Verify ONNX model: `python -c "import onnx; onnx.checker.check_model('model.onnx')"`.
4. Run inference with ONNX Runtime: `ort_session = ort.InferenceSession('model.onnx')`.
5. Benchmark: compare ONNX vs PyTorch inference latency.
6. Quantize for further speedup: `optimum-cli export onnx --optimize O2`.
7. Validate that ONNX output matches PyTorch output within tolerance.

### Verification
- ONNX model exports without errors.
- ONNX inference matches PyTorch output within 1e-4 tolerance.

## Cortex Logging
- collection: procedures
- tags: mlops, training, advanced

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- optimum
- onnxruntime
- onnx

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: ADVANCED
