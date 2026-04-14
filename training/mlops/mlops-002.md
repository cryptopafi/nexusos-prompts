---
type: procedure
created: 2026-03-20
status: active
slug: mlops-002
tags: [procedure, nexus]
---

# MLOPS-002 — Write Parameterized Tests and Test Classes in pytest

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML preprocessing functions have combinatorial input spaces (dtypes, shapes, missing values, encodings). Writing individual test functions for each combination leads to 3-10x code duplication and missed edge cases. Parameterized tests compress hundreds of scenarios into maintainable test matrices.

## Procedura
### Prerequisites
- pytest installed (MLOPS-001 completed)
- ML utility module with functions accepting varied inputs (scalers, encoders, validators)

### Steps
1. **Parametrize data transforms**: Use `@pytest.mark.parametrize("input_data,expected", [(np.array([1,2,3]), np.array([0,0.5,1])), (np.array([5,5,5]), np.array([0,0,0]))])` to test MinMaxScaler on normal, constant, and edge-case arrays. Aim for 10-15 parameter sets per transform function.
2. **Build test matrices for feature engineering**: Create `@pytest.mark.parametrize("dtype", [np.float32, np.float64, np.int32])` combined with `@pytest.mark.parametrize("shape", [(100,5), (1,5), (10000,50)])` — pytest generates the cross-product (9 tests from 2 decorators). This catches dtype-specific bugs that cause silent precision loss.
3. **Group related tests in classes**: Create `class TestFeatureStore:` with `setup_method` loading a shared Feast/DVC feature DataFrame. Group `test_online_features()`, `test_offline_features()`, `test_point_in_time_join()` under one class. Shared setup reduces test runtime by 40-60% vs per-function fixtures.
4. **Write conftest.py with ML fixtures**: Define `@pytest.fixture(scope="session")` for expensive operations (loading model artifacts, connecting to MLflow). Use `@pytest.fixture(scope="function")` for mutable data. Add `@pytest.fixture(params=["lightgbm", "xgboost", "sklearn"])` to test across frameworks automatically.
5. **Add custom markers for ML test tiers**: Register markers in `pytest.ini`: `markers = unit: <100ms tests`, `integration: requires external services`, `gpu: requires CUDA`, `slow: >10s tests`. Run `pytest -m "not slow and not gpu"` in pre-commit, full suite in CI.
6. **Parametrize model evaluation thresholds**: `@pytest.mark.parametrize("metric,threshold", [("accuracy", 0.85), ("f1", 0.80), ("auc", 0.90)])` — test that loaded model meets each metric threshold on a fixed validation set. Store thresholds in a YAML config tied to model version in MLflow registry.
7. **Test W&B/MLflow logging correctness**: Parametrize across experiment trackers: `@pytest.mark.parametrize("tracker", ["mlflow", "wandb"])` and assert that `tracker.log_metric("loss", 0.5)` creates the expected entry. Use mock backends (`mlflow.set_tracking_uri("sqlite:///test.db")`, `wandb.init(mode="offline")`).
8. **Add hypothesis-based property tests**: Install `hypothesis` and write `@given(arrays(dtype=np.float64, shape=(st.integers(1,1000), st.integers(1,50))))` to generate random valid inputs. Assert invariants: output shape matches input, no NaN introduced by scaler, idempotent operations remain idempotent.
9. **Generate HTML coverage reports**: Run `pytest --cov=src --cov-report=html:coverage_html --cov-branch` — branch coverage catches untested conditional paths in preprocessing logic. Target 90%+ branch coverage on feature engineering code.
10. **Review parameterized test output in CI**: Configure `pytest --junitxml=test-results.xml -v` so CI dashboards show each parameterized case individually. Failed parameters pinpoint exact input combinations that break — essential for debugging data-dependent failures.

### Verification
- `pytest -v` shows parameterized tests expanding into 50+ individual test cases
- `pytest --cov=src --cov-branch --cov-fail-under=85` exits 0
- `pytest -m unit` completes in <10s
- Hypothesis finds zero property violations after 100+ examples

## Cortex Logging
- collection: procedures
- tags: mlops, training, testing, parameterized, hypothesis, coverage

## Enforcement Loop
- WHERE: ML projects with >3 preprocessing functions
- WHEN: When adding new feature transforms or model types
- HOW: Add parametrized cases before implementing function, TDD-style
- CONNECT: MLOPS-001 (basic pytest), MLOPS-003 (debugging), MLOPS-015 (CI/CD)

## Dependente
- pytest, pytest-cov, hypothesis
- numpy, pandas
- MLflow or W&B (for tracker tests)
- MLOPS-001

## Metrics
- Completion: 10 steps executed, conftest.py with ML fixtures
- Quality: 85%+ branch coverage, 50+ parameterized cases, zero hypothesis failures
- Level: INTERMEDIATE
- Benchmark: test matrix covers 3+ dtypes x 3+ shapes x edge cases
