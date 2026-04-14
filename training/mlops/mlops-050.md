---
type: procedure
created: 2026-03-17
status: active
slug: mlops-050
tags: [procedure, nexus]
---

# MLOPS-050 — Clean and Scale Data for ML on AWS

## Problema
Raw data with missing values, outliers, and inconsistent scales produces poor ML models.

## Procedura
### Prerequisites
- Python with pandas/scikit-learn
- raw dataset on S3

### Steps
1. Load raw data from S3 into a pandas DataFrame.
2. Handle missing values: impute with mean/median or drop based on threshold.
3. Remove duplicates and fix inconsistent categorical values.
4. Detect and handle outliers using IQR or z-score methods.
5. Scale numerical features: StandardScaler or MinMaxScaler from scikit-learn.
6. Encode categorical variables: one-hot encoding or label encoding.
7. Save cleaned dataset back to S3 in Parquet format.

### Verification
- Cleaned dataset has zero nulls and consistent dtypes.
- Scaled features have expected distributions.

## Cortex Logging
- collection: procedures
- tags: mlops, training, intermediate

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing mlops skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- pandas
- scikit-learn
- AWS S3

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
- Level: INTERMEDIATE
