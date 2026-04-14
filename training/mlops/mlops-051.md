---
type: procedure
created: 2026-03-17
status: active
slug: mlops-051
tags: [procedure, nexus]
---

# MLOPS-051 — Perform Feature Engineering with Scikit-Learn on AWS

## Problema
Raw features rarely have the predictive power needed; engineered features drive model performance.

## Procedura
### Prerequisites
- Python with scikit-learn
- cleaned dataset (MLOPS-050)

### Steps
1. Build a scikit-learn Pipeline with preprocessing steps.
2. Create polynomial features: `PolynomialFeatures(degree=2)`.
3. Apply feature selection: `SelectKBest(f_classif, k=10)`.
4. Implement custom transformers by subclassing `BaseEstimator, TransformerMixin`.
5. Use `ColumnTransformer` to apply different transforms to numeric vs categorical columns.
6. Fit pipeline on training data, transform both train and test sets.
7. Save the fitted pipeline with joblib for deployment.

### Verification
- Pipeline fits and transforms without errors.
- Feature importance or selection scores documented.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- scikit-learn
- joblib
- MLOPS-050

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
