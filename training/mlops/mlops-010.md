---
type: procedure
created: 2026-03-20
status: active
slug: mlops-010
tags: [procedure, nexus]
---

# MLOPS-010 — Build CLI Tools with argparse and Click

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML scripts with hardcoded parameters (learning rate, batch size, data paths, model names) cannot be reused across experiments, integrated into CI/CD pipelines, or orchestrated by workflow engines like Kubeflow/Airflow. Every experiment requires editing source code instead of passing arguments — destroying reproducibility and blocking automation.

## Procedura
### Prerequisites
- Python 3.10+, Click library
- ML training script to parameterize

### Steps
1. **Design CLI interface for ML workflow**: Map ML parameters to CLI args: `--model-name`, `--learning-rate`, `--batch-size`, `--epochs`, `--data-path`, `--output-dir`, `--experiment-name`. Group into subcommands: `train`, `evaluate`, `predict`, `export`. This structure maps directly to Kubeflow pipeline steps.
2. **Build with Click for production CLIs**: `@click.group()` for multi-command interface. `@click.command()` with `@click.option("--learning-rate", type=float, default=1e-3, help="Adam optimizer LR")`. Click advantages over argparse: automatic help generation, type validation, composable commands, and testing support via `CliRunner`.
3. **Add configuration file support**: `@click.option("--config", type=click.Path(exists=True), default="config.yaml")` — load YAML config with `OmegaConf.load(config)`. CLI args override config values: `final_config = OmegaConf.merge(file_config, cli_config)`. This enables: default configs in Git, experiment-specific overrides via CLI.
4. **Integrate with MLflow experiment tracking**: Inside CLI handler: `mlflow.set_experiment(experiment_name)` and `mlflow.log_params({"lr": lr, "batch_size": bs})`. Log the CLI invocation command itself: `mlflow.set_tag("mlflow.source.command", " ".join(sys.argv))`. This creates full reproducibility — any run can be re-executed from its logged command.
5. **Add W&B sweep integration**: `@click.option("--sweep-id", default=None)` — when provided, pulls hyperparameters from W&B sweep agent instead of CLI args. Enables: `wandb agent sweep_id` to run hyperparameter search using the same CLI tool. Sweep config defines search space over CLI parameters.
6. **Implement validation and error handling**: Use Click's built-in validators: `type=click.Choice(["resnet", "vit", "bert"])`, `type=click.FloatRange(1e-6, 1.0)`, `type=click.Path(exists=True, dir_okay=False)`. Add custom validation: `if epochs < 1: raise click.BadParameter("epochs must be >= 1")`. Exit codes: 0=success, 1=user error, 2=system error.
7. **Add progress bars and logging**: `with click.progressbar(data_loader, label="Training") as bar:` for epoch progress. Configure Python logging: `logging.basicConfig(level=logging.DEBUG if verbose else logging.INFO, format="%(asctime)s %(name)s %(levelname)s %(message)s")`. Log to file: `--log-file training.log`.
8. **Write CLI tests with CliRunner**: `from click.testing import CliRunner; runner = CliRunner(); result = runner.invoke(train, ["--epochs", "1", "--data-path", "test_data/"])`. Assert `result.exit_code == 0`. Test invalid inputs: `result = runner.invoke(train, ["--lr", "-1"]); assert result.exit_code != 0`. Integrate with pytest suite.
9. **Package as installable command**: In `pyproject.toml`: `[project.scripts] mlops-train = "mypackage.cli:main"`. After `pip install .`, the command is available system-wide: `mlops-train --help`. This enables: Docker entrypoints, Kubeflow component commands, and ArgoCD job specs to reference the CLI directly.
10. **Wire CLI to Kubeflow/Argo pipeline steps**: Each CLI subcommand becomes a pipeline step: `train_op = dsl.ContainerOp(name="train", command=["mlops-train", "train", "--lr", "1e-3"])`. Parameters flow through the DAG: `train_op = train(lr=hyperparams.output)`. This bridges local development to orchestrated production pipelines.

### Verification
- `mlops-train --help` shows all parameters with descriptions and defaults
- `CliRunner` tests pass for valid and invalid inputs
- MLflow logs the exact CLI command for every training run
- Packaged command installs and runs from any environment
- CLI parameters map to Kubeflow pipeline step arguments

## Cortex Logging
- collection: procedures
- tags: mlops, training, cli, click, automation, kubeflow, reproducibility

## Enforcement Loop
- WHERE: Every ML project with training/evaluation scripts
- WHEN: Before first experiment, during pipeline automation
- HOW: Click CLI wrapping all scripts, tested with CliRunner, integrated with MLflow
- CONNECT: MLOPS-011 (packaging), MLOPS-015 (CI/CD), MLOPS-031 (Makefile)

## Dependente
- click, omegaconf (config), mlflow, wandb
- Kubeflow/Argo (pipeline orchestration)
- pytest (CLI testing)

## Metrics
- Completion: all 10 steps, CLI packaged and testable
- Quality: 100% parameter coverage, all tests pass, MLflow integration working
- Level: INTERMEDIATE
- Benchmark: any training run reproducible from logged MLflow command
