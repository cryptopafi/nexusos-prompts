---
type: procedure
created: 2026-03-20
status: active
slug: mlops-001
tags: [procedure, nexus]
---

# MLOPS-001 — Write Python Unit Tests with pytest

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML teams ship untested code causing silent regressions in data pipelines and model inference. A 2024 Google study found 43% of ML production incidents trace to untested utility functions. Without pytest as a gate, broken preprocessing or feature logic reaches production undetected.

## Procedura
### Prerequisites
- Python 3.10+, pip, a sample ML utility module (e.g., feature transformers, data loaders)
- MLflow or W&B installed for optional experiment-test integration

### Steps
1. **Install pytest ecosystem**: `pip install pytest pytest-cov pytest-xdist pytest-timeout` — xdist enables parallel test execution (2-5x speedup on large suites), timeout prevents hung tests from blocking CI.
2. **Create test structure**: `mkdir -p tests/unit tests/integration` — separate unit tests (pure functions, <100ms each) from integration tests (DB/API calls). Add `conftest.py` at root with shared fixtures for sample DataFrames, model artifacts, and temp directories.
3. **Write data validation tests**: Test every preprocessing function — `test_normalize_features()` should assert output shape matches input shape, values fall in [0,1] range for MinMaxScaler, and NaN handling produces zero nulls. Use `np.testing.assert_array_almost_equal` for float comparisons with `decimal=6`.
4. **Write model inference tests**: Create `test_predict()` that loads a saved model artifact, runs inference on a known input, and asserts output shape and dtype. Benchmark: single prediction should complete in <50ms for tabular models, <200ms for transformer-based.
5. **Add edge-case tests**: Test with empty DataFrames, single-row inputs, all-NaN columns, dtype mismatches (int vs float), and string-in-numeric columns. These catch 60%+ of production data quality issues per Google's ML Test Score paper.
6. **Configure pytest.ini**: Set `testpaths = tests`, `addopts = -v --tb=short --strict-markers -x`, `markers = slow: marks tests as slow`. The `-x` flag fails fast on first error during development.
7. **Add coverage gate**: Run `pytest --cov=src --cov-report=term-missing --cov-fail-under=80` — enforce minimum 80% line coverage. For ML code, focus coverage on data transforms and feature engineering (target 95%), not on model architecture definitions.
8. **Integrate with pre-commit**: Add pytest to `.pre-commit-config.yaml` so tests run before every commit. Configure `pytest --timeout=30` to prevent slow tests from blocking commits. Run full suite in CI only.
9. **Wire to CI pipeline**: In GitHub Actions, add `pytest --junitxml=results.xml` and upload results as artifacts. Configure branch protection to require passing tests before merge. Typical ML project achieves 85%+ coverage with 200-500 unit tests.
10. **Track test metrics in MLflow**: Log test pass rate and coverage percentage as MLflow metrics alongside model metrics. This creates an audit trail linking code quality to model performance over time.

### Verification
- `pytest -v` shows all tests PASS with zero warnings
- `pytest --cov=src --cov-fail-under=80` exits 0
- Test suite completes in <30s for unit tests
- Pre-commit hook blocks commits with failing tests

## Cortex Logging
- collection: procedures
- tags: mlops, training, testing, pytest, ci-cd, quality-gate

## Enforcement Loop
- WHERE: Every ML project with Python code
- WHEN: Before any code merge, during feature development
- HOW: Run pytest in pre-commit + CI, enforce coverage gate
- CONNECT: MLOPS-002 (parameterized tests), MLOPS-015 (GitHub Actions CI/CD)

## Dependente
- pytest, pytest-cov, pytest-xdist, pytest-timeout
- numpy (for array assertions)
- MLflow (optional, for test metric tracking)

## Metrics
- Completion: all 10 steps executed, pre-commit configured
- Quality: 80%+ coverage, <30s suite runtime, zero flaky tests
- Level: BASIC → INTERMEDIATE
- Benchmark: Google ML Test Score Level 1 compliance
