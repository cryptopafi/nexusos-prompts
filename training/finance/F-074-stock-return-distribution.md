---
type: procedure
created: 2026-03-22
status: active
slug: f-074-stock-return-distribution
tags: [procedure, nexus]
---

# Stock Return Distribution Modeling — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiza statistică a distribuției returnurilor unui instrument financiar, testarea normalității, și identificarea "fat tails" pentru implicații de risk management.

---

## 1. Problema

Modelele financiare clasice (Markowitz, Black-Scholes) presupun că returnurile sunt normal distribuite. În realitate, returnurile financiare prezintă "fat tails" (leptokurtosis) și skewness negativ, ceea ce înseamnă că evenimentele extreme sunt mult mai frecvente decât prezice distribuția normală. Fără înțelegerea distribuției reale, risk management-ul este incorect. Aceasta procedură acoperă: descărcare date, vizualizare distribuție, teste de normalitate, și fitarea distribuției optime.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Descărcarea returnurilor — minimum 5 ani de date
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats
from scipy.stats import norm, t as t_dist, skewnorm

ticker = "SPY"
data = yf.download(ticker, start="2019-01-01", end="2024-12-31")
returns = data['Adj Close'].pct_change().dropna()
print(f"Observatii: {len(returns)} zile de trading")
print(f"Perioada: {returns.index[0].date()} - {returns.index[-1].date()}")
```
5 ani minim = ~1260 observații, suficient pentru analiză statistică. Mai mult e mai bun — 10+ ani include cel puțin un ciclu complet de piață.

### Pas 2: Construirea histogramei returnurilor zilnice
```python
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Histograma cu kernel density estimate
axes[0].hist(returns, bins=100, density=True, alpha=0.7, color='steelblue', label='Returnuri reale')

# Overlay cu distributia normala teoretica
mu, sigma = returns.mean(), returns.std()
x = np.linspace(returns.min(), returns.max(), 300)
axes[0].plot(x, norm.pdf(x, mu, sigma), 'r-', linewidth=2, label=f'Normal (μ={mu:.4f}, σ={sigma:.4f})')
axes[0].set_title('Distributia Returnurilor Zilnice')
axes[0].legend()

# Q-Q Plot pentru vizualizarea deviatiei de la normalitate
stats.probplot(returns, dist="norm", plot=axes[1])
axes[1].set_title('Q-Q Plot (Normal)')
plt.tight_layout()
plt.savefig('return_distribution.png', dpi=150)
```

### Pas 3: Calculul statisticilor descriptive — mean/std/skewness/kurtosis
```python
print("=== STATISTICI DESCRIPTIVE ===")
print(f"Media zilnica: {returns.mean():.6f} ({returns.mean()*252:.2%} anual)")
print(f"Std zilnica:   {returns.std():.6f} ({returns.std()*np.sqrt(252):.2%} anual)")
print(f"Skewness:      {returns.skew():.4f}")
print(f"Kurtosis (excess): {returns.kurtosis():.4f}")
print(f"Min return:    {returns.min():.4f} ({returns.min():.2%})")
print(f"Max return:    {returns.max():.4f} ({returns.max():.2%})")
print(f"1% percentil:  {returns.quantile(0.01):.4f} ({returns.quantile(0.01):.2%})")
```
Interpretare: Skewness < 0 = distribuție cu coadă stângă mai lungă (pierderi extreme mai frecvente). Kurtosis > 0 = fat tails (excess kurtosis față de normală). Pentru piețe financiare tipic: skewness -0.5 to -1.0, kurtosis 5-10.

### Pas 4: Testul de normalitate — Shapiro-Wilk sau Jarque-Bera
```python
from scipy.stats import shapiro, jarque_bera, kstest

# Jarque-Bera test (potrivit pentru sample-uri mari)
jb_stat, jb_pvalue = jarque_bera(returns)
print(f"\n=== TESTE DE NORMALITATE ===")
print(f"Jarque-Bera: statistica={jb_stat:.2f}, p-value={jb_pvalue:.6f}")

# Shapiro-Wilk (mai precis pentru n < 5000)
if len(returns) <= 5000:
    sw_stat, sw_pvalue = shapiro(returns.sample(min(5000, len(returns)), random_state=42))
    print(f"Shapiro-Wilk: statistica={sw_stat:.4f}, p-value={sw_pvalue:.6f}")

# Interpretare
alpha = 0.05
if jb_pvalue < alpha:
    print(f"\n[CONCLUZIE] Returnurile NU sunt normal distribuite (p < {alpha})")
    print("Modelele care presupun normalitate subestimează riscul de tail events!")
```

### Pas 5: Compararea cu distribuția normală — vizualizarea "fat tails"
```python
# Calcul frecventa observata vs asteptata pentru tail events
threshold_sigma = [-3, -2, -1, 1, 2, 3]
print("\n=== FRECVENTA TAIL EVENTS ===")
print(f"{'Prag (σ)':>10} | {'Freq. Observata':>18} | {'Freq. Normala':>15}")
print("-" * 50)
for s in threshold_sigma:
    observed = (returns < s*sigma).mean() if s < 0 else (returns > s*sigma).mean()
    expected = norm.cdf(s) if s < 0 else 1 - norm.cdf(s)
    print(f"{s:>10}σ | {observed:>17.4%} | {expected:>14.4%}")

# Concluzia: returnurile < -3σ sunt de X ori mai frecvente decat prezice normalitatea
```

### Pas 6: Fitarea celei mai bune distribuții (normal/t-Student/skewed)
```python
from scipy.stats import t as t_dist, skewnorm, nct

# Fit t-Student (capteaza fat tails)
df_t, loc_t, scale_t = t_dist.fit(returns)
print(f"\n=== FIT DISTRIBUTII ===")
print(f"t-Student: df={df_t:.2f}, loc={loc_t:.6f}, scale={scale_t:.6f}")
print(f"  [Interpretare: df < 30 indica fat tails semnificative]")

# Compara log-likelihood
ll_normal = norm.logpdf(returns, mu, sigma).sum()
ll_t = t_dist.logpdf(returns, df_t, loc_t, scale_t).sum()
print(f"\nLog-likelihood Normal: {ll_normal:.2f}")
print(f"Log-likelihood t-Student: {ll_t:.2f}")
print(f"t-Student fit {'mai bun' if ll_t > ll_normal else 'mai slab'} (ΔLL={ll_t-ll_normal:.2f})")
```

### Pas 7: Interpretarea fat tails pentru risk management
Documentează implicațiile practice: (1) **VaR mai conservativ**: folosește t-Student VaR în loc de Normal VaR pentru estimări mai realiste ale pierderilor maxime; (2) **Sizing pozițiilor**: reduci size-ul dacă distribuția arată tail events frecvente; (3) **Black Swan preparedness**: marchezi că un eveniment -5σ nu e imposibil, ci cu probabilitate de 2-5× mai mare decât prezice normalitatea; (4) **Options pricing**: fat tails justifică prețuri mai mari pentru out-of-the-money puts (volatility smile). Scrie concluzia finală cu recomandări concrete.

---

## 3. Cortex Logging

```json
{
  "text": "Stock Return Distribution Modeling F-074 executat: histograma construita, skewness/kurtosis calculate, Jarque-Bera test rulat, fat tails quantificate, t-Student fittat, implicatii risk management documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-074",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["return-distribution", "fat-tails", "kurtosis", "normality-test", "t-Student", "VaR", "risk-management", "Python"],
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
1. `✅ [PROC] F-074 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Stock Return Distribution Modeling" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (scipy.stats, yfinance, matplotlib) | Runtime pentru analiză statistică |
| Minimum 5 ani date zilnice | Volum suficient pentru teste statistice |
| scipy.stats.jarque_bera, shapiro | Teste de normalitate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Observații analizate | Minimum 1260 (5 ani) |
| Teste de normalitate | Minimum 2 (Jarque-Bera + Shapiro-Wilk) |
