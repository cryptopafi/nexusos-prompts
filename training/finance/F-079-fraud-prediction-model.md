---
type: procedure
created: 2026-03-22
status: active
slug: f-079-fraud-prediction-model
tags: [procedure, nexus]
---

# Fraud Prediction Model Building — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea end-to-end a unui model de machine learning pentru detectarea fraudei financiare, de la feature engineering la deployment și monitorizare.

---

## 1. Problema

Frauda financiară costă industria globală sute de miliarde anual. Modelele de detectare a fraudei trebuie să gestioneze: dezechilibrul masiv al claselor (99.9% non-fraudă vs 0.1% fraudă), costul asimetric al erorilor (false negative = fraudă nedetectată costă mai mult decât false positive = tranzacție legitimă blocată), și data drift (pattern-urile de fraudă evoluează constant). Aceasta procedură acoperă procesul complet de la date la producție.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectarea și înțelegerea dataset-ului etichetat
```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, roc_auc_score, confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns

# Dataset public de referinta: creditcard.csv (Kaggle - ULB Machine Learning Group)
# 284,807 tranzactii, 492 frauduloase (0.172% - dezechilibru sever)
# df = pd.read_csv('creditcard.csv')

# Pentru exemplu, cream un dataset sintetic
np.random.seed(42)
n = 10000
df = pd.DataFrame({
    'amount': np.random.exponential(100, n),
    'hour_of_day': np.random.randint(0, 24, n),
    'merchant_category': np.random.randint(1, 50, n),
    'card_age_days': np.random.randint(1, 3650, n),
    'transactions_last_24h': np.random.poisson(3, n),
    'is_foreign': np.random.binomial(1, 0.1, n),
    'is_fraud': np.random.binomial(1, 0.002, n)  # 0.2% fraud rate
})

print(f"Total tranzactii: {len(df):,}")
print(f"Fraude: {df['is_fraud'].sum():,} ({df['is_fraud'].mean():.3%})")
print(f"\nClase dezechilibrate: raport {(1-df['is_fraud'].mean())/df['is_fraud'].mean():.0f}:1 (non-fraud:fraud)")
```

### Pas 2: Feature Engineering — variabile predictive
```python
# Creeaza features relevante pentru detectia fraudei
df['amount_log'] = np.log1p(df['amount'])  # Log transform pentru sumele skewed
df['is_night'] = df['hour_of_day'].apply(lambda x: 1 if (x < 6 or x > 22) else 0)  # Tranzactii noaptea = risc
df['amount_per_transaction_ratio'] = df['amount'] / (df['transactions_last_24h'] + 1)  # Suma neobisnuit de mare
df['velocity_risk'] = (df['transactions_last_24h'] > 10).astype(int)  # Velocity anormal

# Features finale
feature_cols = [
    'amount_log',
    'hour_of_day',
    'is_night',
    'merchant_category',
    'card_age_days',
    'transactions_last_24h',
    'amount_per_transaction_ratio',
    'velocity_risk',
    'is_foreign'
]

print(f"Numar features: {len(feature_cols)}")
print("\nCorrelatie features cu target:")
for feat in feature_cols:
    corr = df[feat].corr(df['is_fraud'])
    print(f"  {feat}: {corr:.4f}")
```

### Pas 3: Split train/test (80/20) cu stratificare
```python
X = df[feature_cols]
y = df['is_fraud']

# Stratified split - pastreaza proportia de fraude in ambele seturi
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.20,
    random_state=42,
    stratify=y  # CRITIC: stratificare pentru clase dezechilibrate
)

# Scalare features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print(f"Train: {len(X_train):,} ({y_train.sum():,} fraude - {y_train.mean():.3%})")
print(f"Test:  {len(X_test):,} ({y_test.sum():,} fraude - {y_test.mean():.3%})")
```

### Pas 4: Antrenarea modelului — Random Forest sau XGBoost
```python
from sklearn.utils.class_weight import compute_class_weight
from imblearn.over_sampling import SMOTE  # pip install imbalanced-learn

# Abordare 1: Class weights (simplu, eficient)
class_weights = compute_class_weight('balanced', classes=[0, 1], y=y_train)
cw_dict = {0: class_weights[0], 1: class_weights[1]}

# Random Forest cu class balancing
rf_model = RandomForestClassifier(
    n_estimators=200,
    max_depth=10,
    class_weight='balanced',  # Echilibreaza clasele automat
    random_state=42,
    n_jobs=-1
)
rf_model.fit(X_train_scaled, y_train)

# Predictii probabilistice (NU classify direct)
y_prob = rf_model.predict_proba(X_test_scaled)[:, 1]  # Probabilitatea de frauda

print("Model antrenat. Feature importances:")
importance_df = pd.DataFrame({
    'Feature': feature_cols,
    'Importance': rf_model.feature_importances_
}).sort_values('Importance', ascending=False)
print(importance_df.to_string(index=False))
```

### Pas 5: Evaluarea modelului — precision/recall/AUC-ROC
```python
from sklearn.metrics import precision_recall_curve, average_precision_score

# AUC-ROC (masura principala pentru fraud detection)
auc_roc = roc_auc_score(y_test, y_prob)
auc_pr = average_precision_score(y_test, y_prob)

print(f"\n=== METRICI DE EVALUARE ===")
print(f"AUC-ROC: {auc_roc:.4f}  [target: > 0.95 pentru fraud detection]")
print(f"AUC-PR:  {auc_pr:.4f}   [mai relevant pentru clase dezechilibrate]")

# Classification report la threshold 0.5
y_pred = (y_prob > 0.5).astype(int)
print(f"\nClassification Report (threshold=0.5):")
print(classification_report(y_test, y_pred, target_names=['Non-Fraud', 'Fraud']))

# Confusion Matrix
cm = confusion_matrix(y_test, y_pred)
print(f"\nConfusion Matrix:")
print(f"TN={cm[0,0]:,} | FP={cm[0,1]:,}")
print(f"FN={cm[1,0]:,} | TP={cm[1,1]:,}")
print(f"\nFraude detectate: {cm[1,1]}/{y_test.sum()} ({cm[1,1]/y_test.sum():.1%} recall)")
print(f"False alarms: {cm[0,1]:,} ({cm[0,1]/(cm[0,1]+cm[1,1]):.1%} din alertele generate)")
```

### Pas 6: Setarea threshold-ului pe baza toleranței businessului
```python
# Business question: ce cost e mai mare?
# Cost FN (frauda nedetectata) = pierderea medie per frauda
# Cost FP (alarma falsa) = cost investigatie + experienta negativa client

cost_fn = 200  # EUR pierdere medie per frauda nedetectata
cost_fp = 5    # EUR cost per investigatie alarma falsa

# Calculeaza costul total per threshold
thresholds = np.arange(0.01, 0.99, 0.01)
total_costs = []
for thresh in thresholds:
    pred = (y_prob > thresh).astype(int)
    cm_t = confusion_matrix(y_test, pred)
    fn = cm_t[1, 0]  # Fraude nedetectate
    fp = cm_t[0, 1]  # Alarme false
    total_cost = fn * cost_fn + fp * cost_fp
    total_costs.append(total_cost)

optimal_threshold = thresholds[np.argmin(total_costs)]
optimal_cost = min(total_costs)

print(f"\n=== THRESHOLD OPTIMIZATION ===")
print(f"Threshold optim (minimizeaza cost business): {optimal_threshold:.2f}")
print(f"Cost total minim: {optimal_cost:,.0f} EUR")
print(f"\nLa threshold {optimal_threshold:.2f}:")
pred_optimal = (y_prob > optimal_threshold).astype(int)
print(classification_report(y_test, pred_optimal, target_names=['Non-Fraud', 'Fraud']))
```

### Pas 7: Implementarea în pipeline de producție și monitorizarea drift-ului lunar
Definește procesul de producție: (1) **Serving**: model serialized cu `joblib.dump(model)`, API endpoint care primește tranzacție în timp real și returnează fraud score 0-1; (2) **Decision engine**: dacă score > threshold_optim → hold pentru review manual, dacă score > 0.95 → block automat; (3) **Monitoring lunar**: calculează AUC-ROC pe tranzacțiile din ultima lună cu labels finalizate; dacă AUC scade > 5% față de baseline → trigger retraining; (4) **Data drift**: monitorizează distribuția feature-urilor de input (PSI - Population Stability Index) lunar; (5) **Retraining schedule**: retrain complet trimestrial cu date recente, fine-tune lunar.

---

## 3. Cortex Logging

```json
{
  "text": "Fraud Prediction Model F-079 construit: dataset colectat, feature engineering, train/test split stratificat, Random Forest antrenat, AUC-ROC evaluat, threshold optimizat pe cost business, pipeline productie definit.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-079",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["fraud-detection", "machine-learning", "Random-Forest", "XGBoost", "AUC-ROC", "imbalanced-classes", "Python", "fintech"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-079 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Fraud Prediction Model Building" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (sklearn, imbalanced-learn, pandas) | Training și evaluare model |
| Dataset fraud etichetat | Date de antrenament |
| Cost matrix business | Optimizarea threshold-ului |
| Monitoring pipeline | Detectare drift lunar |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| AUC-ROC | > 0.95 |
| Recall fraude la threshold optim | > 80% |
| Frecvență monitorizare drift | Lunar |
