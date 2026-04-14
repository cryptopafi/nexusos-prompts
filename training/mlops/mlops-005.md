---
type: procedure
created: 2026-03-20
status: active
slug: mlops-005
tags: [procedure, nexus]
---

# MLOPS-005 — Manipulate DataFrames and Visualize Data with Pandas

## Version: 2.0 — Opus v2 Rewrite

## Problema
Raw DataFrames need transformation before ML consumption — joins across sources, aggregations for feature windows, pivots for time-series, and visualizations for stakeholder communication. Inefficient pandas operations on >1M rows cause 10-100x slowdowns and OOM errors without vectorized patterns.

## Procedura
### Prerequisites
- MLOPS-004 completed, pandas 2.0+ with pyarrow backend
- matplotlib, seaborn, plotly installed

### Steps
1. **Enable pyarrow backend**: `pd.options.mode.dtype_backend = "pyarrow"` then load with `dtype_backend="pyarrow"`. Arrow-backed DataFrames use 30-50% less memory. Benchmark: 1M-row groupby drops from 2.1s to 0.4s.
2. **Build feature aggregations**: `features = df.groupby("user_id").agg({"amount": ["mean", "std", "count"], "timestamp": ["min", "max"]})`. Flatten MultiIndex: `features.columns = ["_".join(col) for col in features.columns]`. For time windows: `df.groupby("user_id").rolling("7D", on="timestamp").mean()`.
3. **Merge multi-source data safely**: `pd.merge(df_a, df_b, on="key", how="left", validate="many_to_one")` — `validate` catches unexpected duplicates. Always check `len(merged)` vs `len(original)`. A >5% increase indicates join key issues.
4. **Apply vectorized operations**: Replace `for idx, row in df.iterrows()` with `df["feature"] = np.where(df["amount"] > threshold, df["amount"] * factor, 0)`. Benchmark: vectorized on 1M rows = 10ms vs iterrows = 45s (4500x faster). Use `.apply(engine="numba")` only as last resort.
5. **Handle time-series features**: `df["day_of_week"] = df["timestamp"].dt.dayofweek`, `df["lag_1"] = df.groupby("id")["value"].shift(1)`, `df["rolling_mean_7"] = df.groupby("id")["value"].transform(lambda x: x.rolling(7).mean())`. Store definitions in Feast feature store.
6. **Create publication-quality visualizations**: `sns.set_theme(style="whitegrid", palette="husl")`. Essential plots: `sns.pairplot(df, hue="target")`, `sns.heatmap(df.corr(), annot=True, cmap="RdBu_r", center=0)`, `sns.violinplot(data=df, x="category", y="value")`. Save as MLflow artifacts.
7. **Visualize model-relevant patterns**: Feature importance bar chart, learning curves with confidence intervals, confusion matrix heatmap, residuals vs predicted scatter. These go into the model card and stakeholder reports.
8. **Detect and visualize outliers**: `z_scores = np.abs(stats.zscore(df[numeric_cols]))`, then `df_clean = df[(z_scores < 3).all(axis=1)]`. Visualize with `sns.boxenplot()` (letter-value plot, better than boxplot for large data). Log outlier removal count as MLflow metric. Typical removal: 1-5%.
9. **Optimize memory before pipeline handoff**: Downcast numerics: `pd.to_numeric(df[cols], downcast="integer")`. Convert low-cardinality strings to `category`. Typical reduction: 50-70%. Save to Parquet with DVC tracking.
10. **Export interactive dashboard**: `px.scatter_matrix(df, dimensions=feature_cols, color="target")`. Export standalone HTML. Upload to W&B as report or MLflow as artifact. Share URL with stakeholders before modeling begins.

### Verification
- All vectorized operations complete in <1s on 1M rows
- Memory optimization achieves >50% reduction
- No unexpected >0.95 feature correlation pairs
- All visualizations saved as MLflow/W&B artifacts

## Cortex Logging
- collection: procedures
- tags: mlops, training, pandas, visualization, feature-engineering, eda

## Enforcement Loop
- WHERE: Data preparation phase of every ML project
- WHEN: After initial EDA, before model training
- HOW: Vectorized transforms, standard plots, save artifacts
- CONNECT: MLOPS-004 (loading), MLOPS-006 (NumPy), MLOPS-051 (feature engineering)

## Dependente
- pandas 2.0+, pyarrow, numpy, seaborn, matplotlib, plotly
- MLflow/W&B (artifacts), DVC (versioning), Feast (feature store)

## Metrics
- Completion: all 10 steps, dashboard exported
- Quality: <1s ops on 1M rows, 50%+ memory reduction, zero join anomalies
- Level: INTERMEDIATE
- Benchmark: feature pipeline processes 1M rows in <30s end-to-end
