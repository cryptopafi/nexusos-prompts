---
type: procedure
created: 2026-03-20
status: active
slug: mlops-008
tags: [procedure, nexus]
---

# MLOPS-008 — Use Hugging Face Transformers Pipeline

## Version: 2.0 — Opus v2 Rewrite

## Problema
Teams reinvent NLP/CV inference code from scratch — writing tokenization, model loading, post-processing, and batching logic that Hugging Face Transformers already provides battle-tested. This wastes 2-5 days per model integration and introduces bugs in text handling, padding, and output parsing that pre-built pipelines have already solved.

## Procedura
### Prerequisites
- Python 3.10+, `pip install transformers torch accelerate`
- GPU recommended for models >1B parameters

### Steps
1. **Select task-appropriate pipeline**: `from transformers import pipeline`. Supported tasks: `sentiment-analysis`, `text-generation`, `summarization`, `translation`, `question-answering`, `zero-shot-classification`, `text2text-generation`, `image-classification`, `object-detection`. Choose task before model — pipeline handles all pre/post-processing.
2. **Initialize with production-grade model**: `classifier = pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english", device=0)` — `device=0` uses first GPU, `device=-1` forces CPU. For production: pin model version with commit hash: `model="distilbert-base-uncased-finetuned-sst-2-english", revision="af0f99b"`.
3. **Run batch inference efficiently**: `results = classifier(["text1", "text2", "text3"], batch_size=32)` — batching provides 3-8x throughput improvement over sequential calls. Benchmark: DistilBERT processes 1000 sentences in 2.1s batched vs 12.4s sequential on V100. Log latency to MLflow.
4. **Configure generation parameters**: For text-generation: `generator = pipeline("text-generation", model="gpt2"); output = generator("prompt", max_new_tokens=100, temperature=0.7, top_p=0.9, do_sample=True, num_return_sequences=3)`. Document parameter choices in MLflow run metadata.
5. **Handle model caching and offline mode**: Models download to `~/.cache/huggingface/`. Set `TRANSFORMERS_CACHE=/path/to/shared/cache` for team sharing. For air-gapped environments: `TRANSFORMERS_OFFLINE=1` after pre-downloading. Cache size monitoring: typical model = 250MB-2GB. Track cache usage in infrastructure metrics.
6. **Register pipeline model in MLflow**: `mlflow.transformers.log_model(classifier, "sentiment-model", task="text-classification")`. This creates a reproducible model artifact with all tokenizer configs, model weights, and pipeline settings. Load back: `mlflow.transformers.load_model("runs:/run_id/sentiment-model")`.
7. **Benchmark inference performance**: Measure latency percentiles (p50, p95, p99) and throughput (samples/sec). Use `torch.cuda.Event` for GPU timing. Typical benchmarks: DistilBERT = 5ms/sample (GPU), BERT-base = 8ms, BERT-large = 15ms, GPT-2 = 25ms. Log to W&B for comparison across models.
8. **Optimize with ONNX export**: `from optimum.onnxruntime import ORTModelForSequenceClassification; model = ORTModelForSequenceClassification.from_pretrained(model_id, export=True)`. ONNX inference is 2-4x faster than PyTorch on CPU. Combine with `pipeline("sentiment-analysis", model=model)` — same API, faster inference.
9. **Add input validation and error handling**: Wrap pipeline calls with input length checking (`len(tokenizer(text)["input_ids"]) <= max_length`), encoding validation (UTF-8), and empty input guards. Log rejected inputs to monitoring system. Set `truncation=True` to handle over-length inputs gracefully.
10. **Deploy pipeline with BentoML**: `import bentoml; bentoml.transformers.save_model("sentiment", classifier)`. Create BentoService with `/predict` endpoint. BentoML handles batching, serialization, and containerization. Alternative: Seldon Core wrapper for Kubernetes-native serving with canary deployments.

### Verification
- Pipeline returns correct label and score for test inputs
- Batch inference achieves >3x throughput over sequential
- MLflow model artifact loads and reproduces identical predictions
- ONNX-optimized inference shows measurable speedup
- BentoML endpoint responds with <100ms p95 latency

## Cortex Logging
- collection: procedures
- tags: mlops, training, huggingface, transformers, inference, deployment

## Enforcement Loop
- WHERE: Any NLP/CV project using pre-trained models
- WHEN: Model selection, inference optimization, deployment preparation
- HOW: Pipeline first, benchmark, optimize with ONNX, deploy with BentoML
- CONNECT: MLOPS-009 (datasets), MLOPS-074 (HF Hub), MLOPS-078 (containerization)

## Dependente
- transformers, torch, accelerate, optimum[onnxruntime]
- MLflow (model registry), BentoML (serving), W&B (benchmarking)

## Metrics
- Completion: all 10 steps, model registered in MLflow
- Quality: <100ms p95 latency, >3x batch speedup, ONNX optimized
- Level: INTERMEDIATE
- Benchmark: DistilBERT at 5ms/sample GPU, 2x faster with ONNX on CPU
