---
type: procedure
created: 2026-03-20
status: active
slug: mlops-009
tags: [procedure, nexus]
---

# MLOPS-009 — Work with Hugging Face Datasets Library

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML teams manually download, split, and preprocess benchmark datasets — introducing inconsistent splits, data leakage between train/test, and non-reproducible preprocessing. The Hugging Face Datasets library provides 70,000+ datasets with standardized splits, streaming for large datasets, and Apache Arrow-backed processing that handles 100GB+ datasets without OOM.

## Procedura
### Prerequisites
- Python 3.10+, `pip install datasets transformers`
- DVC for dataset version control

### Steps
1. **Load datasets with version pinning**: `ds = load_dataset("imdb", revision="1.0.0")` — pin version for reproducibility. For private datasets: `load_dataset("company/internal-data", token=os.environ["HF_TOKEN"])`. Check dataset card: `ds["train"].info.description` for provenance and licensing information.
2. **Explore dataset structure systematically**: `print(ds)` shows splits and sizes. `ds["train"].features` reveals column types (ClassLabel, Value, Sequence). `ds["train"][0]` shows first example. Log dataset metadata to MLflow: `mlflow.log_params({"dataset": "imdb", "train_size": len(ds["train"]), "test_size": len(ds["test"])})`.
3. **Stream large datasets without downloading**: `ds = load_dataset("c4", streaming=True)` — processes 365GB dataset without local storage. Iterate with `for example in ds["train"].take(1000):`. Essential for datasets >10GB. Streaming supports `filter`, `map`, `shuffle` with reservoir sampling.
4. **Preprocess with batched map for 10x speedup**: `ds = ds.map(tokenize_fn, batched=True, batch_size=1000, num_proc=4)` — `batched=True` processes 1000 examples at once (10x faster than per-example). `num_proc=4` parallelizes across CPU cores. Remove original columns: `remove_columns=["text"]` to save memory.
5. **Apply train/validation/test splits properly**: If no validation split: `ds_split = ds["train"].train_test_split(test_size=0.1, seed=42, stratify_by_column="label")`. This creates stratified splits. For time-series: `ds["train"].select(range(n_train))` preserving temporal order. Log split sizes to W&B.
6. **Filter and combine datasets**: `filtered = ds["train"].filter(lambda x: len(x["text"]) > 100, num_proc=4)` — parallel filtering. Combine datasets: `from datasets import concatenate_datasets; combined = concatenate_datasets([ds1["train"], ds2["train"]])`. Interleave for balanced training: `interleave_datasets([ds1, ds2], probabilities=[0.7, 0.3])`.
7. **Create custom dataset from local data**: `ds = Dataset.from_pandas(df)` or `Dataset.from_dict({"text": texts, "label": labels})`. For large local files: `load_dataset("csv", data_files={"train": "train.csv", "test": "test.csv"})`. Push to HF Hub: `ds.push_to_hub("username/my-dataset", private=True)`.
8. **Version datasets with DVC integration**: `ds.save_to_disk("data/processed_v2/")` then `dvc add data/processed_v2/`. Commit `.dvc` file to Git. This enables: `dvc checkout` to restore any dataset version, `dvc diff` to compare versions, and `dvc push` to remote storage (S3/GCS/Azure).
9. **Set up dataset transforms for training**: `ds.set_format("torch", columns=["input_ids", "attention_mask", "labels"])` — zero-copy conversion to PyTorch tensors. Or `set_format("numpy")` for scikit-learn. Use `with_transform(transform_fn)` for on-the-fly augmentation without materializing transformed dataset.
10. **Create feature store entries from datasets**: Extract computed features and register in Feast feature store: define feature view, materialize features, serve online via Feast API. This bridges the gap between dataset exploration and production feature serving. Log feature definitions to MLflow as artifacts.

### Verification
- Dataset loads with expected splits, sizes, and features
- Batched map processing achieves >5x speedup over unbatched
- Streaming mode processes >1GB without OOM
- DVC-versioned dataset restores identically with `dvc checkout`
- PyTorch format conversion produces correct tensor dtypes/shapes

## Cortex Logging
- collection: procedures
- tags: mlops, training, huggingface, datasets, preprocessing, versioning

## Enforcement Loop
- WHERE: Every ML project requiring training/evaluation data
- WHEN: Data loading, preprocessing, version control
- HOW: Load with version pin, batched map processing, DVC version control
- CONNECT: MLOPS-008 (transformers), MLOPS-074 (HF Hub), MLOPS-077 (HF datasets advanced)

## Dependente
- datasets, transformers (tokenizer)
- DVC (versioning), Feast (feature store)
- MLflow/W&B (metadata logging)

## Metrics
- Completion: all 10 steps, dataset versioned in DVC
- Quality: reproducible splits, >5x batched speedup, streaming for large data
- Level: INTERMEDIATE
- Benchmark: 1M examples preprocessed in <60s with num_proc=4
