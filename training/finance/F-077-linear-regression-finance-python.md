---
type: procedure
created: 2026-03-22
status: active
slug: f-077-linear-regression-finance-python
tags: [procedure, nexus]
---

# Linear Regression for Financial Analysis (Python) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea regresiei liniare OLS pentru estimarea beta-ului acțiunilor, analiza relației return-factor, și interpretarea riguroasă a rezultatelor statistice.

---

## 1. Problema

Regresia liniară este instrumentul fundamental al finanțelor cantitative: estimarea beta-ului (CAPM), analiza factorilor Fama-French, modelarea relației dintre prețuri și fundamental. Fără a interpreta corect R², p-values, și verificarea asumpțiilor regresiei (reziduri normale, homoscedasticity), concluziile pot fi eronate. Aceasta procedură acoperă: setup complet OLS, interpretare statistici, diagnosticare reziduri.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea variabilelor dependente și independente
Clarifică structura regresiei: **Dependent variable (Y)**: ce vrei să explici sau prezici (ex: returnul zilnic al unei acțiuni). **Independent variable(s) (X)**: factorii explicativi (ex: returnul pieței pentru beta CAPM, sau factori Fama-French). Exemplu standard: `Y = Return_AAPL`, `X = Return_SPY (Market Return)`. Formula CAPM: `R_stock = alpha + beta × R_market + ε`. Documentează ipoteza economică din spatele relației.

### Pas 2: Colectarea și pregătirea datelor
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import statsmodels.api as sm
from scipy import stats

# Descarca date
tickers = ['AAPL', 'SPY']  # Stock + Market proxy
data = yf.download(tickers, start="2019-01-01", end="2024-12-31")['Adj Close']
returns = data.pct_change().dropna()
returns.columns = ['AAPL', 'Market']

print(f"N observatii: {len(returns)}")
print(f"Corelatie AAPL-Market: {returns.corr().iloc[0,1]:.4f}")

# Variabile pentru regresie
Y = returns['AAPL']
X = returns['Market']
X_with_const = sm.add_constant(X)  # Adauga interceptul (alpha)
```

### Pas 3: Rularea regresiei OLS cu statsmodels
```python
# OLS Regression
model = sm.OLS(Y, X_with_const)
results = model.fit()

# Afisare summary complet
print(results.summary())

# Extragere valori cheie
alpha = results.params['const']
beta = results.params['Market']
r_squared = results.rsquared
p_alpha = results.pvalues['const']
p_beta = results.pvalues['Market']

print(f"\n=== REZULTATE CHEIE ===")
print(f"Alpha (Jensen's alpha): {alpha:.6f} ({alpha*252:.2%} anual)")
print(f"Beta: {beta:.4f}")
print(f"R-squared: {r_squared:.4f} ({r_squared:.1%})")
print(f"P-value alpha: {p_alpha:.4f} ({'semnificativ' if p_alpha < 0.05 else 'nesemnificativ'})")
print(f"P-value beta: {p_beta:.6f} ({'semnificativ' if p_beta < 0.05 else 'nesemnificativ'})")
```

### Pas 4: Interpretarea coeficienților — beta/alpha, R², p-values, t-stats
Interpretare riguroasă: **Beta > 1**: acțiunea este mai volatilă decât piața (ex: beta=1.2 → +1% piață = +1.2% acțiune); **Beta < 1**: mai puțin volatilă (defensive stock); **Alpha**: return în exces față de ce prezice CAPM; dacă p_alpha > 0.05, alpha nu e statistic diferit de zero (no edge). **R²**: % din variația returnului acțiunii explicat de piață (ex: R²=0.60 = 60% din mișcări sunt legate de piață). **t-stat > 2** și **p-value < 0.05** = coeficient statistic semnificativ.

### Pas 5: Construirea graficului regresiei cu scatter și linia de regresie
```python
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Scatter plot cu linia de regresie
axes[0].scatter(X, Y, alpha=0.4, s=15, color='steelblue', label='Observatii zilnice')
x_line = np.linspace(X.min(), X.max(), 100)
y_line = alpha + beta * x_line
axes[0].plot(x_line, y_line, 'r-', linewidth=2,
             label=f'OLS: α={alpha:.4f}, β={beta:.4f}\nR²={r_squared:.3f}')
axes[0].axhline(y=0, color='gray', linestyle='--', alpha=0.5)
axes[0].axvline(x=0, color='gray', linestyle='--', alpha=0.5)
axes[0].set_xlabel('Return Piata (SPY)')
axes[0].set_ylabel('Return AAPL')
axes[0].set_title('AAPL vs Market Return — Regresia CAPM Beta')
axes[0].legend()
axes[0].grid(alpha=0.3)

# Reziduri in timp
residuals = results.resid
axes[1].plot(returns.index, residuals, alpha=0.7, linewidth=0.8, color='purple')
axes[1].axhline(y=0, color='red', linestyle='--')
axes[1].set_title('Reziduri Regresia OLS in Timp')
axes[1].set_ylabel('Reziduu')
axes[1].grid(alpha=0.3)

plt.tight_layout()
plt.savefig('ols_regression.png', dpi=150)
plt.show()
```

### Pas 6: Verificarea rezidurilor — normalitate și heteroscedasticity
```python
from scipy.stats import shapiro, jarque_bera
import statsmodels.stats.diagnostic as diag

# Test normalitate reziduri
jb_stat, jb_p = jarque_bera(residuals)
print(f"\n=== DIAGNOSTICE REGRESIE ===")
print(f"Normalitate reziduri (Jarque-Bera): p={jb_p:.4f} -> {'Normal OK' if jb_p > 0.05 else 'Non-normal!'}")

# Test heteroscedasticity (White test)
white_test = diag.het_white(residuals, X_with_const)
white_p = white_test[1]
print(f"Homoscedasticity (White test): p={white_p:.4f} -> {'OK' if white_p > 0.05 else 'Heteroscedasticity!'}")

# Autocorrelatie (Durbin-Watson)
dw = sm.stats.stattools.durbin_watson(residuals)
print(f"Durbin-Watson: {dw:.4f} -> {'OK (DW~2)' if 1.5 < dw < 2.5 else 'Autocorrelatie!'}")

# Daca DW < 1.5 sau > 2.5, rezidurile sunt autocorrelate -> erori standard incorecte
# Solutie: Newey-West standard errors
if not (1.5 < dw < 2.5):
    results_nw = model.fit(cov_type='HAC', cov_kwds={'maxlags': 5})
    print(f"  -> Aplicam Newey-West SE. Beta NW: {results_nw.params['Market']:.4f}")
```

### Pas 7: Aplicarea insight-urilor din regresie
Documentează concluzia practică: (1) **Beta estimat**: cum modifică sizing-ul poziției (beta=1.5 → reducere position size cu 33% față de piață pentru aceeași expunere efectivă); (2) **Alpha**: dacă semnificativ pozitiv → stock picker value; dacă zero → nu există edge față de ETF; (3) **Limitări**: linearitatea relației (verifică dacă există relație non-liniară), instabilitatea beta în timp (beta rolling), perioadele de criză când beta crește brusc.

---

## 3. Cortex Logging

```json
{
  "text": "Linear Regression Finance Python F-077 executat: OLS model construit (CAPM beta), alpha/beta interpretate, R²/p-values analizate, grafic scatter+reziduri generat, diagnostice normalitate/heteroscedasticity verificate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-077",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["linear-regression", "OLS", "CAPM", "beta", "alpha", "statsmodels", "Python", "residual-analysis"],
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
1. `✅ [PROC] F-077 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Linear Regression for Financial Analysis (Python)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python statsmodels | OLS regression și diagnostice |
| Python scipy.stats | Teste normalitate reziduri |
| yfinance | Date returnuri pentru regresie |
| matplotlib | Vizualizare scatter și reziduri |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Diagnostice reziduri verificate | 3 (normalitate, homo, autocorrelație) |
| P-value beta | < 0.001 (beta semnificativ) |
