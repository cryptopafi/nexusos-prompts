---
type: procedure
created: 2026-03-20
status: active
slug: mlops-007
tags: [procedure, nexus]
---

# MLOPS-007 — Install and Use Azure CLI for ML Workspaces

## Version: 2.0 — Opus v2 Rewrite

## Problema
Manual Azure portal navigation is slow, non-reproducible, and blocks ML infrastructure automation. Teams waste 30-60 minutes per workspace setup clicking through UI. Azure CLI with ML extension enables Infrastructure-as-Code for ML workspaces, compute clusters, and data stores — reproducible across environments in <5 minutes.

## Procedura
### Prerequisites
- Azure account with active subscription (Free tier works for learning)
- macOS/Linux terminal access

### Steps
1. **Install Azure CLI with ML extension**: `brew install azure-cli && az extension add -n ml -y`. Verify: `az version` shows 2.50+ and `az ml -h` lists ML commands. Pin version in team docs to prevent breaking changes across environments.
2. **Authenticate and configure defaults**: `az login --use-device-code` (works in SSH/remote environments). Set defaults: `az configure --defaults group=mlops-rg location=eastus workspace=mlops-ws`. Store subscription ID in env var: `export AZURE_SUBSCRIPTION_ID=$(az account show --query id -o tsv)`. Use service principal for CI/CD: `az ad sp create-for-rbac --name mlops-cicd --role contributor`.
3. **Create ML workspace with IaC YAML**: Write `workspace.yml`: `$schema`, `name: mlops-ws`, `resource_group: mlops-rg`, `storage_account`, `key_vault`, `application_insights` references. Deploy: `az ml workspace create --file workspace.yml`. This YAML is version-controlled in Git — enables identical workspace recreation across dev/staging/prod.
4. **Provision compute cluster for training**: `az ml compute create --name gpu-cluster --type AmlCompute --size Standard_NC6s_v3 --min-instances 0 --max-instances 4 --idle-time-before-scale-down 1800`. Zero min-instances = zero cost when idle. NC6s_v3 provides 1x V100 GPU at ~$3.06/hr. Log compute config to MLflow for cost tracking.
5. **Register data stores and datasets**: `az ml data create --name training-data --type uri_folder --path azureml://datastores/blob_store/paths/datasets/v1/`. Version data: `--version 2` creates immutable snapshot. Wire to DVC for local-remote data sync: `dvc remote add azure az://container/path`.
6. **Configure workspace networking and security**: Enable private endpoints: `az ml workspace update --name mlops-ws --public-network-access disabled`. Set up managed identity: `az ml workspace update --managed-identity system`. Configure Key Vault for secrets: `az keyvault secret set --vault-name mlops-kv --name wandb-api-key --value $WANDB_API_KEY`.
7. **Deploy compute instances for development**: `az ml compute create --name dev-instance --type ComputeInstance --size Standard_DS3_v2 --idle-time-before-shutdown-minutes 60`. Auto-shutdown saves 40-60% on development compute costs. Enable JupyterLab and VS Code tunneling.
8. **Set up MLflow tracking with Azure ML**: Azure ML natively integrates MLflow — `export MLFLOW_TRACKING_URI=$(az ml workspace show --query mlflow_tracking_uri -o tsv)`. All MLflow experiments, runs, and model registry operations sync to Azure ML Studio automatically. No additional MLflow server needed.
9. **Create environment definitions**: `az ml environment create --file env.yml` where env.yml specifies Docker base image, conda dependencies, and pip packages. Version environments: `--version 2`. Use curated environments for common frameworks: `az ml environment list --query "[?contains(name, 'pytorch')]"`.
10. **Automate with ArgoCD/GitHub Actions**: Create `.github/workflows/infra.yml` that runs `az ml workspace create --file workspace.yml` on push to `infra/` directory. Use `azure/login@v1` action with service principal. Add `az ml compute create` for auto-provisioning compute. Tag deployments with Git SHA for traceability.

### Verification
- `az ml workspace show` returns workspace details with all linked resources
- Compute cluster scales to 0 when idle (verify in Azure portal)
- MLflow tracking URI connects successfully: `mlflow.set_tracking_uri()` + `mlflow.list_experiments()`
- Service principal authenticates in CI/CD pipeline
- Data store accessible from compute: `az ml data show --name training-data`

## Cortex Logging
- collection: procedures
- tags: mlops, training, azure, infrastructure, iac, workspace

## Enforcement Loop
- WHERE: Azure-based ML projects, team workspace provisioning
- WHEN: Project kickoff, environment replication, CI/CD setup
- HOW: YAML-first infrastructure, version control all configs, automate via Actions
- CONNECT: MLOPS-063 (Azure ML workspace), MLOPS-065 (Azure ML + GitHub Actions), MLOPS-066 (Azure training)

## Dependente
- Azure CLI 2.50+, az ml extension
- Azure subscription with Contributor role
- GitHub Actions (CI/CD), MLflow (tracking), DVC (data sync)

## Metrics
- Completion: all 10 steps, workspace + compute + data store provisioned
- Quality: zero manual portal steps, <5min workspace creation, auto-scaling active
- Level: INTERMEDIATE
- Benchmark: workspace provisioning automated end-to-end via CI/CD
