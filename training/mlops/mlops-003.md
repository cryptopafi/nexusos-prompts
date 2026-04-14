---
type: procedure
created: 2026-03-20
status: active
slug: mlops-003
tags: [procedure, nexus]
---

# MLOPS-003 — Debug Python with pdb and pytest Fixtures

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML pipeline failures are notoriously difficult to debug — shape mismatches appear deep in training loops, NaN gradients emerge after hours of training, and data corruption manifests only on specific batches. Standard print-debugging wastes 2-4 hours per incident. Systematic debugging with pdb + pytest fixtures reduces resolution time to 15-30 minutes.

## Procedura
### Prerequisites
- Python 3.10+, pytest with MLOPS-001/002 completed
- Familiarity with ML training loops and data pipelines

### Steps
1. **Set up ipdb for ML debugging**: `pip install ipdb` — superior to pdb with syntax highlighting and tab completion. Insert `import ipdb; ipdb.set_trace()` before the suspected failure point. In training loops, use conditional breakpoints: `if batch_idx == 42: ipdb.set_trace()` to inspect specific failing batches.
2. **Debug with pytest --pdb flag**: Run `pytest --pdb test_preprocessing.py` — drops into debugger on first failure. Use `pytest --pdb --pdb-trace` to break at start of every test. In debugger: inspect tensor shapes (`p tensor.shape`), check for NaN (`p torch.isnan(tensor).any()`), examine DataFrame dtypes (`p df.dtypes`).
3. **Create diagnostic fixtures**: Write `@pytest.fixture` that logs full DataFrame statistics before each test — shape, dtypes, null counts, min/max per column. On failure, these logs instantly reveal whether input data was corrupted. Store diagnostics in `/tmp/test_diagnostics/` with timestamp for post-mortem analysis.
4. **Build model checkpoint fixtures**: Create `@pytest.fixture(scope="session")` that loads a known-good model checkpoint from MLflow registry (production stage). Compare outputs layer by layer: `torch.allclose(output_good, output_bad, atol=1e-5)` to isolate where divergence begins.
5. **Debug data pipeline stages individually**: Write fixtures that cache intermediate pipeline outputs: `@pytest.fixture` returning `(raw_data, cleaned_data, featured_data, scaled_data)`. When a test fails, inspect each stage separately. Use DVC to version these intermediate artifacts for reproducibility.
6. **Use pytest-sugar and pytest-clarity**: Install both for enhanced failure output — pytest-clarity shows diffs between expected/actual values with color highlighting. For tensor comparisons, `pytest-torch` formats shape mismatches clearly: `Expected shape (32, 768) but got (32, 512)`.
7. **Set up remote debugging for GPU training**: Configure `debugpy` for attaching to running jobs: `import debugpy; debugpy.listen(5678)`. In VS Code, attach to running process. Essential for debugging CUDA OOM, gradient explosions, and multi-GPU sync issues without restarting training.
8. **Create failure reproduction fixtures**: Write fixtures that capture exact random state (`np.random.get_state()`, `torch.random.get_rng_state()`) and data batch that caused failure. Save to pickle file. Next debug session loads this state for deterministic reproduction — saves hours on intermittent failures.
9. **Implement W&B debug logging**: During debugging, log suspicious tensors: `wandb.log({"debug/gradient_norm": grad.norm(), "debug/activation_stats": act.mean()})`. Create dedicated "debug" W&B project. After fixing, compare debug run metrics against production baseline to confirm fix.
10. **Build post-mortem debugging playbook**: Document common ML failure patterns and pdb investigation steps: shape mismatch → `p x.shape` at each transform; NaN propagation → `p torch.isnan(x).sum()` backward through computation; OOM → `p torch.cuda.memory_summary()`. Reference from test failure messages.

### Verification
- `pytest --pdb` drops into debugger at failure point with full context
- Diagnostic fixtures log shape/dtype/null stats for every test
- Failure reproduction fixture deterministically recreates bugs
- Debug session resolves issue in <30 minutes (vs 2-4h baseline)

## Cortex Logging
- collection: procedures
- tags: mlops, training, debugging, pdb, fixtures, reproducibility

## Enforcement Loop
- WHERE: Any ML project experiencing test failures or pipeline bugs
- WHEN: When tests fail, during incident investigation
- HOW: Start with `pytest --pdb`, use diagnostic fixtures, log to W&B
- CONNECT: MLOPS-001 (basic tests), MLOPS-002 (parameterized tests), MLOPS-055 (experiment tracking)

## Dependente
- pytest, ipdb, debugpy, pytest-sugar, pytest-clarity
- MLflow (checkpoint loading), W&B (debug logging)
- DVC (intermediate artifact versioning)

## Metrics
- Completion: all 10 steps executed, playbook documented
- Quality: debug resolution time <30min, reproduction fixtures capture 90%+ failures
- Level: INTERMEDIATE → ADVANCED
- Benchmark: MTTR reduced by 70%
