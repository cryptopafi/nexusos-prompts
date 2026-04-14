---
type: procedure
created: 2026-03-22
status: active
slug: f-078-regression-strategy-evaluation
tags: [procedure, nexus]
---

# Regression-Based Strategy Evaluation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea riguroasă a unei strategii de investiții bazate pe factori prin regresie cross-secțională Fama-MacBeth, ajustată pentru costuri de tranzacționare și testată out-of-sample.

---

## 1. Problema

O strategie care arată alpha în backtest poate fi un artefact al data mining-ului sau overfitting-ului. Evaluarea profesională necesită: testul Fama-MacBeth cross-sectional, validare out-of-sample, ajustare pentru costuri reale de tranzacționare, și raportarea Information Coefficient (IC). Aceasta procedură separă strategiile cu edge real de cele cu rezultate întâmplătoare.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea ipotezei strategiei — factorul prezice returnurile
Formulează clar: "Factorul [X] prezice returnurile cross-secționale la orizontul [T]". Exemplu: "Companiile cu momentum ridicat (return 12-1 luni) outperformează în luna următoare". Specificifică: factorul de ranking (ex: momentum score = return ultimele 12 luni, exclusiv ultima lună), universul de acțiuni (ex: S&P 500), orizontul de predicție (1 lună), și mecanismul economic de ce ar funcționa (momentum = underreaction la știri).

### Pas 2: Colectarea datelor factor și returnuri forward
```python
import pandas as pd
import numpy as np
from scipy import stats
import statsmodels.api as sm

# Necesita date cross-sectionale: N actiuni × T perioade
# Factor signal: calculat la sfarsitul fiecarei luni
# Forward return: returnul din luna urmatoare

# Structura DataFrame:
# df['date'] = luna de calcul
# df['ticker'] = identificator actiune
# df['factor_score'] = valoarea factorului (ex: momentum)
# df['forward_return'] = returnul din luna urmàtoare (target)

# Exemplu cu momentum artificial pentru demonstratie:
np.random.seed(42)
n_stocks = 100
n_months = 60
dates = pd.date_range('2019-01-01', periods=n_months, freq='M')

df = pd.DataFrame({
    'date': np.repeat(dates, n_stocks),
    'ticker': np.tile([f'STK{i:03d}' for i in range(n_stocks)], n_months),
    'factor_score': np.random.randn(n_stocks * n_months),
    'forward_return': np.random.randn(n_stocks * n_months) * 0.05
})
# In productie: inlocuieste cu date reale din CRSP/Bloomberg/Compustat
```

### Pas 3: Regresia Fama-MacBeth cross-secțională
```python
# Fama-MacBeth: ruleaza regresia cross-sectionala IN FIECARE LUNA
# Coeficientul factorului per luna = gamma_t
# Alpha estrategia = media gamma_t in timp

fmb_results = []
for date in df['date'].unique():
    month_data = df[df['date'] == date].dropna()
    if len(month_data) < 20:  # Skip luni cu prea putine actiuni
        continue

    # Normalizeaza factorul cross-sectional (z-score) pentru comparabilitate
    month_data = month_data.copy()
    month_data['factor_norm'] = stats.zscore(month_data['factor_score'])

    # Regresia cross-sectionala
    X = sm.add_constant(month_data['factor_norm'])
    y = month_data['forward_return']
    result = sm.OLS(y, X).fit()

    fmb_results.append({
        'date': date,
        'gamma': result.params['factor_norm'],  # Coeficient factor
        'alpha': result.params['const'],
        't_stat': result.tvalues['factor_norm'],
        'r_squared': result.rsquared
    })

fmb_df = pd.DataFrame(fmb_results)
print(fmb_df.describe())
```

### Pas 4: Evaluarea factor loadings și t-stats
```python
# Fama-MacBeth t-stat = media gamma / std(gamma) * sqrt(T)
gamma_mean = fmb_df['gamma'].mean()
gamma_std = fmb_df['gamma'].std()
T = len(fmb_df)
fmb_t_stat = gamma_mean / (gamma_std / np.sqrt(T))
fmb_p_value = 2 * (1 - stats.t.cdf(abs(fmb_t_stat), df=T-1))

print(f"=== FAMA-MACBETH RESULTS ===")
print(f"Gamma mediu lunar: {gamma_mean:.6f}")
print(f"Gamma anualizat: {gamma_mean*12:.4f}")
print(f"Fama-MacBeth t-stat: {fmb_t_stat:.3f}")
print(f"P-value: {fmb_p_value:.4f}")
print(f"Semnificativ la 5%: {'DA' if fmb_p_value < 0.05 else 'NU'}")
print(f"\n[Regula practica: |t-stat| > 2.0 = factor valid]")
```

### Pas 5: Verificarea anti-data-mining — testul out-of-sample
```python
# Imparte perioada in IN-SAMPLE (calibrare) si OUT-OF-SAMPLE (test)
split_date = fmb_df['date'].median()
in_sample = fmb_df[fmb_df['date'] <= split_date]
out_sample = fmb_df[fmb_df['date'] > split_date]

in_gamma = in_sample['gamma'].mean()
out_gamma = out_sample['gamma'].mean()

print(f"\n=== VALIDARE OUT-OF-SAMPLE ===")
print(f"In-sample gamma: {in_gamma:.6f}")
print(f"Out-of-sample gamma: {out_gamma:.6f}")
print(f"Decay ratio (out/in): {out_gamma/in_gamma:.2f}")
print(f"[Target: decay ratio > 0.5 = factor robust]")
print(f"[Decay ratio < 0.3 = factor probabil data-mined]")
```

### Pas 6: Ajustarea pentru costuri de tranzacționare
```python
# Estimare turnover lunar (% din portofoliu tranzactionat)
# Pentru strategii cu ranking lunar: turnover tipic 30-60% lunar

turnover_monthly = 0.40  # 40% turnover lunar (exemplu)
transaction_cost_bps = 10  # 10 bps per tranzactie (institutional)
cost_per_month = turnover_monthly * transaction_cost_bps / 10000

gross_return = gamma_mean  # Gross alpha per luna
net_return = gross_return - cost_per_month

print(f"\n=== AJUSTARE COSTURI TRANZACTIONARE ===")
print(f"Alpha gross lunar: {gross_return:.6f} ({gross_return*12:.2%} anual)")
print(f"Turnover lunar: {turnover_monthly:.0%}")
print(f"Cost lunar estimat: {cost_per_month:.6f} ({cost_per_month*12:.2%} anual)")
print(f"Alpha net lunar: {net_return:.6f} ({net_return*12:.2%} anual)")
print(f"Strategia este profitabila net: {'DA' if net_return > 0 else 'NU'}")
```

### Pas 7: Raportarea IC (Information Coefficient) și alpha ajustat la risc
```python
# Information Coefficient = Spearman rank correlation intre factor si forward return
ic_series = []
for date in df['date'].unique():
    month_data = df[df['date'] == date].dropna()
    if len(month_data) < 20:
        continue
    ic, _ = stats.spearmanr(month_data['factor_score'], month_data['forward_return'])
    ic_series.append(ic)

ic_mean = np.mean(ic_series)
ic_std = np.std(ic_series)
icir = ic_mean / ic_std  # IC Information Ratio

print(f"\n=== INFORMATION COEFFICIENT REPORT ===")
print(f"IC Mediu: {ic_mean:.4f}")
print(f"IC Std: {ic_std:.4f}")
print(f"IC IR (ICIR): {icir:.4f}")
print(f"\nBenchmark-uri ICIR:")
print(f"  ICIR > 0.5  = factor BUN")
print(f"  ICIR > 0.3  = factor acceptabil")
print(f"  ICIR < 0.2  = factor slab/nerelevant")
print(f"\nConcluzie: Factorul este {'VALID' if abs(icir) > 0.3 else 'SLAB'} (ICIR={icir:.3f})")
```

---

## 3. Cortex Logging

```json
{
  "text": "Regression Strategy Evaluation F-078 executat: ipoteza definita, date colectate, Fama-MacBeth rulat, t-stat calculat, OOS validat, costuri ajustate, IC/ICIR raportat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-078",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["Fama-MacBeth", "factor-investing", "IC", "ICIR", "cross-sectional-regression", "out-of-sample", "quantitative-finance"],
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
1. `✅ [PROC] F-078 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Regression-Based Strategy Evaluation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (statsmodels, scipy, pandas) | Regresia Fama-MacBeth și statistici |
| Date cross-secționale (CRSP/Bloomberg) | Factor scores și returnuri forward |
| Transaction cost model | Ajustarea alpha net |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Fama-MacBeth t-stat | > 2.0 pentru factor valid |
| Out-of-sample decay ratio | > 0.5 |
| ICIR | > 0.3 |
