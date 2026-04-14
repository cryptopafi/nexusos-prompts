---
type: procedure
created: 2026-03-20
status: active
slug: mlops-011
tags: [procedure, nexus]
---

# MLOPS-011 — Package Python Projects with Dependencies

## Version: 2.0 — Opus v2 Rewrite

## Problema
Unpackaged ML code cannot be versioned semantically, installed as a dependency, distributed across teams, or referenced in Docker/Kubeflow/ArgoCD pipelines. Copy-pasting utility functions between projects creates drift — a bug fixed in one project persists in others. Proper packaging with pyproject.toml enables reproducible ML environments.

## Procedura
### Prerequisites
- Python 3.10+, pip, build tools
- ML project with reusable modules (feature engineering, data loading, model utilities)

### Steps
1. **Create production project structure**: `src/ml_package/` layout (src-layout prevents import conflicts): `src/ml_package/__init__.py`, `src/ml_package/features.py`, `src/ml_package/models.py`, `src/ml_package/data.py`, `tests/`, `pyproject.toml`. The src-layout ensures tests import the installed package, not local source — catching missing files.
2. **Write comprehensive pyproject.toml**: `[build-system] requires = ["setuptools>=68.0"]`, `[project] name = "ml-package"`, `version = "1.0.0"`, `requires-python = ">=3.10"`, `dependencies = ["numpy>=1.24", "pandas>=2.0", "scikit-learn>=1.3", "mlflow>=2.8"]`. Pin minimum versions, not exact — allows compatible upgrades.
3. **Define optional dependency groups**: `[project.optional-dependencies] training = ["torch>=2.0", "wandb"]`, `gpu = ["nvidia-cuda-runtime-cu12"]`, `dev = ["pytest", "ruff", "mypy"]`, `serving = ["bentoml", "fastapi"]`. Install with: `pip install "ml-package[training,dev]"`. This keeps the base package lightweight.
4. **Add entry points for CLI tools**: `[project.scripts] ml-train = "ml_package.cli:train"`, `ml-predict = "ml_package.cli:predict"`. These become system commands after install. Add `[project.gui-scripts]` for dashboard tools. Entry points integrate with Kubeflow ContainerOps and ArgoCD job definitions.
5. **Configure code quality tools in pyproject.toml**: `[tool.ruff] line-length = 100`, `select = ["E", "F", "I", "UP"]`. `[tool.mypy] python_version = "3.10"`, `warn_return_any = true`. `[tool.pytest.ini_options] testpaths = ["tests"]`, `addopts = "--cov=src --cov-fail-under=80"`. One config file for all tools.
6. **Build and verify package**: `python -m build` creates `dist/ml_package-1.0.0.tar.gz` and `.whl`. Verify: `pip install dist/ml_package-1.0.0-py3-none-any.whl && python -c "import ml_package; print(ml_package.__version__)"`. Check no missing files with `tar -tzf dist/*.tar.gz | head -20`.
7. **Set up semantic versioning with git tags**: `git tag -a v1.0.0 -m "Initial release"`. Automate version from git tags: use `setuptools-scm` — `[tool.setuptools_scm]` in pyproject.toml auto-generates version from tags. This ensures package version always matches git history. Log version to MLflow with every training run.
8. **Publish to private PyPI**: Set up private index with `pip install devpi` or use AWS CodeArtifact/GCP Artifact Registry. Publish: `twine upload --repository private dist/*`. Teams install: `pip install --index-url https://private.pypi.com/simple/ ml-package`. This enables consistent package versions across training and serving environments.
9. **Create Docker base image with package**: `FROM python:3.10-slim` → `COPY dist/ml_package-*.whl /tmp/` → `RUN pip install /tmp/ml_package-*.whl[serving]`. Tag image with package version: `docker build -t ml-package:1.0.0 .`. Push to container registry for Kubeflow/Seldon/BentoML serving.
10. **Integrate with CI/CD pipeline**: GitHub Actions workflow: on push → lint (`ruff check`) → type check (`mypy src/`) → test (`pytest --cov`) → build (`python -m build`) → publish to private PyPI → build Docker image → push to registry. ArgoCD watches the registry for new versions and triggers deployment.

### Verification
- `pip install -e ".[dev]"` succeeds with all dependencies
- `python -c "import ml_package"` works from any directory (not just project root)
- `python -m build` creates valid .whl and .tar.gz
- Semantic version matches git tag
- Docker image builds and runs successfully
- CI/CD pipeline passes all stages

## Cortex Logging
- collection: procedures
- tags: mlops, training, packaging, pyproject, versioning, cicd

## Enforcement Loop
- WHERE: Every ML project intended for production or team sharing
- WHEN: Before first deployment, when code needs reuse across projects
- HOW: src-layout, pyproject.toml, semantic versioning, private PyPI, CI/CD
- CONNECT: MLOPS-010 (CLI), MLOPS-014 (virtual environments), MLOPS-025 (containers)

## Dependente
- build, twine, setuptools-scm, ruff, mypy
- Docker (containerization), private PyPI (distribution)
- GitHub Actions/ArgoCD (CI/CD)

## Metrics
- Completion: all 10 steps, package published and containerized
- Quality: 80%+ test coverage, zero ruff/mypy errors, semantic versioning active
- Level: INTERMEDIATE
- Benchmark: package install + import in <10s, CI/CD pipeline <5min
