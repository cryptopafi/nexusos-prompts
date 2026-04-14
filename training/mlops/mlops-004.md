---
type: procedure
created: 2026-03-20
status: active
slug: mlops-004
tags: [procedure, nexus]
---

# MLOPS-004 — Load and Explore Data with Pandas

## Version: 2.0 — Opus v2 Rewrite

## Problema
ML engineers skip systematic data exploration, jumping straight to modeling. This causes 40-60% of model failures in production (per Andrew Ng's data-centric AI research). Proper EDA with pandas, profiling, and validation catches data quality issues before they propagate through the pipeline.

## Procedura
### Prerequisites
- Python 3.10+, pandas 2.0+, pyarrow (for Parquet)
- Access to raw data source (S3, GCS, local, or feature store)

### Steps
1. **Load data with type optimization**: Use `pd.read_parquet()` over CSV whenever possible (3-10x faster, preserves dtypes). For CSV: `pd.read_csv(path, dtype={"id": "int32", "category": "category"}, parse_dates=["timestamp"])` — explicit dtypes prevent silent type inference errors and reduce memory 40-70%.
2. **Generate automated data profile**: Install `ydata-profiling`: `ProfileReport(df, title="Dataset EDA", explorative=True)`. Export HTML report — generates 50+ statistics per column including distributions, correlations, missing patterns, and duplicates in one command.
3. **Validate data schema with pandera**: Define expected schema: `schema = pa.DataFrameSchema({"feature_1": pa.Column(float, pa.Check.in_range(0, 1)), "target": pa.Column(int, pa.Check.isin([0, 1]))})`. Run `schema.validate(df)` to catch schema drift. Log violations to MLflow.
4. **Check class balance and target distribution**: `df["target"].value_counts(normalize=True)` — if majority >80%, flag for stratified splitting. For regression: check skewness (`df["target"].skew()`) — values >2 need log transform. Store stats in W&B tables.
5. **Profile memory usage**: `df.info(memory_usage="deep")` — dataset should fit in <50% of available RAM. If >8GB, switch to chunked processing or Polars/Dask. Log memory footprint as MLflow metric.
6. **Detect data leakage signals**: Check temporal ordering: `assert df["timestamp"].is_monotonic_increasing`. Compute correlation with target: `df.corrwith(df["target"]).abs().sort_values(ascending=False)` — features >0.95 are suspicious. Version validated dataset with DVC.
7. **Analyze missing value patterns**: `missingno.matrix(df)` — visualize whether missingness is MCAR, MAR, or MNAR. This determines imputation strategy. Log missing % per column to experiment tracker.
8. **Profile feature distributions for drift baseline**: Compute mean, std, min, max, quantiles (5th, 25th, 50th, 75th, 95th) per numeric feature. Store as JSON baseline artifact in MLflow — becomes reference for production drift detection (MLOPS-061).
9. **Export exploration artifacts**: Save to Parquet with pyarrow engine. Save EDA HTML report. Save schema YAML. Commit data profile to DVC, code to Git. Create W&B artifact linking dataset version to EDA findings.
10. **Create data card documentation**: Standardized data card: source, collection date, size, feature descriptions, known biases, class distribution, recommended preprocessing. Store alongside dataset in DVC. Data cards reduce debugging time by 35% (Google Model Cards paper).

### Verification
- ydata-profiling report generated with zero high-severity warnings
- pandera schema validation passes
- Memory usage <50% of available RAM
- No leakage signals (max target correlation <0.95)
- Drift baseline saved as MLflow artifact

## Cortex Logging
- collection: procedures
- tags: mlops, training, eda, pandas, data-quality, validation

## Enforcement Loop
- WHERE: First step of every ML project, every new dataset onboarding
- WHEN: Before any feature engineering or model training
- HOW: Run full EDA pipeline, validate schema, save artifacts
- CONNECT: MLOPS-005 (DataFrame manipulation), MLOPS-050 (cleaning), MLOPS-051 (feature engineering)

## Dependente
- pandas 2.0+, pyarrow, ydata-profiling, pandera, missingno
- MLflow (artifacts), W&B (tables), DVC (data versioning)

## Metrics
- Completion: all 10 steps executed, data card written
- Quality: zero schema violations, <5% missing values, no leakage
- Level: BASIC → INTERMEDIATE
- Benchmark: EDA catches 80%+ data quality issues before modeling
