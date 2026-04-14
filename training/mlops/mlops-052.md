---
type: procedure
created: 2026-03-17
status: active
slug: mlops-052
tags: [procedure, nexus]
---

# MLOPS-052 — Visualize and Cluster Data for ML Analysis

## Problema
Skipping exploratory analysis leads to poor feature selection and undetected data issues.

## Procedura
### Prerequisites
- Python with matplotlib, seaborn, scikit-learn
- processed dataset

### Steps
1. Create correlation heatmap: `sns.heatmap(df.corr())`.
2. Plot feature distributions with histograms and box plots.
3. Apply PCA for dimensionality reduction: `PCA(n_components=2).fit_transform(X)`.
4. Visualize PCA results with scatter plots colored by target.
5. Run K-Means clustering: `KMeans(n_clusters=k).fit(X_scaled)`.
6. Use elbow method to determine optimal k.
7. Plot cluster assignments overlaid on PCA visualization.

### Verification
- Visualizations saved as PNG files.
- Optimal cluster count determined from elbow plot.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- matplotlib
- seaborn
- scikit-learn

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
