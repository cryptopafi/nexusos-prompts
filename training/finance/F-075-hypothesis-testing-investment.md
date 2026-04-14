---
type: procedure
created: 2026-03-22
status: active
slug: f-075-hypothesis-testing-investment
tags: [procedure, nexus]
---

# Hypothesis Testing for Investment Strategies — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea testelor statistice de ipoteză pentru a evalua dacă o strategie de investiții are un avantaj real sau performanța sa se datorează întâmplării.

---

## 1. Problema

Orice strategie testată pe date istorice poate arăta performanță pozitivă din simplă întâmplare (overfitting, data mining). Fără teste statistice formale de ipoteză nu poți distinge o strategie cu edge real de una care a avut noroc pe sample-ul de test. Aceasta procedură acoperă: definirea ipotezelor, selectarea testului potrivit, calculul p-value, și interpretarea corectă a rezultatelor.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea ipotezei nule (H0)
Formulează precis: **H0 (Nulă)**: Strategia nu are niciun avantaj față de benchmark / întâmplare. Matematic: `H0: μ_strategie - μ_benchmark = 0` sau `H0: Sharpe_strategie ≤ 0`. Exemplu concret: "Strategia SMA Golden Cross nu generează returnuri mai mari decât buy-and-hold SPY". Dacă nu poți respinge H0 cu suficientă certitudine statistică, strategia nu are edge demonstrat.

### Pas 2: Definirea ipotezei alternative (H1)
**H1 (Alternativă)**: Strategia outperformează. Alege direcția: (1) **One-tailed**: `H1: μ_strategie > μ_benchmark` (știi direcția așteptată — mai bun); (2) **Two-tailed**: `H1: μ_strategie ≠ μ_benchmark` (testezi dacă există diferență în orice direcție). Recomandare pentru strategii de investiții: one-tailed (ești interesat dacă strategia e mai bună, nu mai proastă).

### Pas 3: Colectarea returnurilor strategiei vs benchmark
```python
import yfinance as yf
import numpy as np
from scipy import stats

# Strategia (ex: SMA din F-073) vs Benchmark (buy-and-hold)
# Asigura-te ca ambele serii au aceleasi date
strategy_returns = df['Strategy_Return'].dropna()
benchmark_returns = df['Daily_Return'].dropna()

# Aliniaza seriile la aceeasi perioada
common_idx = strategy_returns.index.intersection(benchmark_returns.index)
strat = strategy_returns[common_idx]
bench = benchmark_returns[common_idx]
diff = strat - bench  # Returnul activ zilnic

print(f"N observatii: {len(diff)}")
print(f"Return activ mediu: {diff.mean():.6f} ({diff.mean()*252:.2%} anual)")
print(f"Std return activ: {diff.std():.6f}")
```

### Pas 4: Alegerea testului statistic — t-test sau Wilcoxon
```python
# Verifica normalitatea returnurilor active
from scipy.stats import shapiro, ttest_1samp, wilcoxon

_, normality_p = shapiro(diff.sample(min(5000, len(diff)), random_state=42))
print(f"Test normalitate (p-value): {normality_p:.4f}")

if normality_p > 0.05:
    # Returnuri aproximativ normale -> t-test
    print("Distributie aproximativ normala: folosim t-test parametric")
    t_stat, p_value = ttest_1samp(diff, 0, alternative='greater')  # one-tailed
    test_name = "t-test one-tailed"
else:
    # Non-normal -> Wilcoxon signed-rank test (non-parametric)
    print("Distributie non-normala: folosim Wilcoxon signed-rank test")
    t_stat, p_value = wilcoxon(diff, alternative='greater')
    test_name = "Wilcoxon signed-rank"

print(f"\nTest: {test_name}")
print(f"Statistica test: {t_stat:.4f}")
print(f"P-value: {p_value:.6f}")
```

### Pas 5: Setarea nivelului de semnificație și calculul p-value
```python
alpha = 0.05  # Nivel de semnificatie standard (5%)

print(f"\n=== CONCLUZIE STATISTICA ===")
print(f"H0: Strategia nu outperformeaza benchmark-ul")
print(f"H1: Strategia outperformeaza benchmark-ul (one-tailed)")
print(f"Alpha: {alpha}")
print(f"P-value: {p_value:.6f}")
print()

if p_value < alpha:
    print(f"RESPINGEM H0 (p={p_value:.4f} < alpha={alpha})")
    print("Concluzie: Strategia are un avantaj statistic semnificativ la nivel de 5%")
else:
    print(f"NU RESPINGEM H0 (p={p_value:.4f} >= alpha={alpha})")
    print("Concluzie: Nu exista dovezi suficiente ca strategia outperformeaza")
    print("ATENTIE: Performanta observata poate fi intamplatoare!")
```

### Pas 6: Respingerea sau nerespingerea ipotezei nule
Interpretarea nuanțată a p-value: p < 0.01 = evidenta puternica contra H0, p 0.01-0.05 = evidenta moderata, p 0.05-0.10 = evidenta slabă (marginal), p > 0.10 = fara evidenta semnificativa. Important: **nerespingerea H0 ≠ H0 este adevărată** — poate că sample-ul e prea mic. Calculează **puterea testului** (1-β): dacă < 0.8, poate că aveai nevoie de mai multe date.

### Pas 7: Raportarea semnificanței statistice
```python
# Interval de incredere pentru returnul activ mediu
from scipy.stats import t as t_dist
n = len(diff)
se = diff.std() / np.sqrt(n)
t_critical = t_dist.ppf(0.95, df=n-1)  # one-tailed 95%
ci_lower = diff.mean() - t_critical * se
ci_upper = diff.mean() + t_critical * se

print(f"\n=== RAPORT FINAL ===")
print(f"Return activ mediu zilnic: {diff.mean():.6f}")
print(f"Return activ anualizat: {diff.mean()*252:.2%}")
print(f"IC 95% (one-tailed lower bound): > {ci_lower*252:.2%} anual")
print(f"Semnificanta statistica: {'DA' if p_value < alpha else 'NU'} (p={p_value:.4f})")
```
Documentează în rezumat: ce strategie, ce test, ce perioadă, N observații, p-value, concluzie, limitări (out-of-sample? transaction costs incluși?).

---

## 3. Cortex Logging

```json
{
  "text": "Hypothesis Testing Investment Strategies F-075 executat: H0/H1 definite, t-test/Wilcoxon aplicat, p-value calculat, H0 respins sau mentinut, raport complet generat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-075",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["hypothesis-testing", "statistical-significance", "t-test", "Wilcoxon", "p-value", "strategy-evaluation", "Python"],
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
1. `✅ [PROC] F-075 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Hypothesis Testing for Investment Strategies" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (scipy.stats) | Testele statistice (t-test, Wilcoxon, Shapiro) |
| Backtest returns din F-073 | Date pentru testul strategiei |
| scipy.stats.ttest_1samp / wilcoxon | Funcții statistice core |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Nivel semnificație standard | alpha = 0.05 |
| Puterea testului (1-β) | > 0.80 |
