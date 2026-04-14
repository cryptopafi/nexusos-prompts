---
type: procedure
created: 2026-03-20
status: active
slug: mlops-006
tags: [procedure, nexus]
---

# MLOPS-006 — Perform NumPy Array Operations for ML Data Prep

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML pipelines require efficient numerical operations — matrix multiplications, broadcasting, random sampling, and shape transformations. Raw Python lists are 50-100x slower than NumPy for these operations. Incorrect array handling causes subtle shape mismatches, dtype precision loss, and memory blowups that silently corrupt model training.

## Procedura
### Prerequisites
- Python 3.10+, NumPy 1.24+
- Understanding of tensor shapes for target ML framework (PyTorch/TensorFlow)

### Steps
1. **Create arrays with explicit dtypes**: Always specify dtype — `np.zeros((batch_size, features), dtype=np.float32)` not `np.zeros(shape)`. Float32 uses 50% less memory than float64 with negligible precision loss for ML. Benchmark: 10M-element array in float32 = 40MB vs float64 = 80MB.
2. **Master broadcasting for feature engineering**: `normalized = (X - X.mean(axis=0)) / X.std(axis=0)` broadcasts column-wise stats across rows. Understand broadcasting rules: shapes `(1000, 50)` and `(50,)` broadcast; `(1000, 50)` and `(49,)` raise ValueError. Use `np.expand_dims()` when dimensions need alignment.
3. **Reshape for model input compatibility**: `X.reshape(-1, seq_len, features)` for RNNs, `X.reshape(-1, channels, height, width)` for CNNs. Use `-1` for automatic batch dimension. Verify: `assert X_reshaped.shape == (n_samples, seq_len, features)`. Common bug: `reshape` vs `resize` — reshape returns a view (fast, shares memory), resize modifies in-place.
4. **Implement efficient random sampling for ML**: Set seed for reproducibility: `rng = np.random.default_rng(42)` (modern API, thread-safe). Create train/val/test splits: `indices = rng.permutation(n); train_idx, val_idx = indices[:n_train], indices[n_train:]`. Use `rng.choice(n, size=batch_size, replace=False)` for mini-batch sampling.
5. **Vectorize distance computations**: `dists = np.linalg.norm(X[:, np.newaxis] - centroids[np.newaxis, :], axis=2)` computes all pairwise distances in one operation. For cosine similarity: `sim = X @ Y.T / (np.linalg.norm(X, axis=1)[:, np.newaxis] * np.linalg.norm(Y, axis=1))`. These replace Python loops that would take 100x longer.
6. **Handle sparse data efficiently**: For datasets with >80% zeros (NLP bag-of-words, recommendation systems): `from scipy.sparse import csr_matrix; X_sparse = csr_matrix(X)`. Memory: 1M x 50K dense = 400GB vs sparse with 1% fill = 8GB. Convert to dense only for operations that need it.
7. **Stack and concatenate for pipeline assembly**: `X_train = np.vstack([batch1, batch2, batch3])` for vertical stacking, `X_features = np.hstack([numeric_features, encoded_features])` for feature concatenation. Use `np.concatenate(arrays, axis=0)` when axis flexibility is needed. Always verify: `assert X_final.shape[1] == expected_features`.
8. **Convert between NumPy and ML frameworks**: `torch.from_numpy(X)` (shares memory, zero-copy), `X.numpy()` from torch (CPU only). For TensorFlow: `tf.constant(X)`. For pandas: `df.values` or `df.to_numpy(dtype=np.float32)`. DVC-tracked arrays: `np.save("features.npy", X)` with `.npy` in `.dvc` tracking.
9. **Profile and optimize memory**: `X.nbytes / 1e9` gives GB usage. Use memory-mapped arrays for datasets larger than RAM: `X = np.memmap("data.npy", dtype=np.float32, mode="r", shape=(n, d))`. Memory-mapped files enable training on 100GB+ datasets with 16GB RAM. Log array sizes to MLflow for resource planning.
10. **Validate numerical stability**: Check for NaN/Inf after operations: `assert not np.any(np.isnan(X)) and not np.any(np.isinf(X))`. Use `np.clip(X, -1e6, 1e6)` before divisions. For log computations: `np.log(np.maximum(X, 1e-10))`. Log stability metrics to W&B — numerical instability causes 15% of training failures.

### Verification
- All array operations execute in <100ms on 1M elements
- Float32 arrays used throughout (memory halved vs float64)
- No NaN/Inf in pipeline output arrays
- Shape assertions pass at every transformation step
- Memory-mapped access works for datasets >RAM

## Cortex Logging
- collection: procedures
- tags: mlops, training, numpy, numerical-computing, performance, memory

## Enforcement Loop
- WHERE: Every ML pipeline with numerical data processing
- WHEN: During feature engineering, data loading, batch preparation
- HOW: Use vectorized operations, explicit dtypes, shape assertions
- CONNECT: MLOPS-004 (pandas), MLOPS-005 (visualization), MLOPS-051 (feature engineering)

## Dependente
- numpy 1.24+, scipy.sparse
- PyTorch or TensorFlow (framework conversion)
- DVC (array versioning), MLflow/W&B (metric logging)

## Metrics
- Completion: all 10 steps executed, numerical stability validated
- Quality: zero NaN/Inf, <100ms per operation, 50% memory reduction via float32
- Level: INTERMEDIATE
- Benchmark: vectorized ops 50-100x faster than Python loops
