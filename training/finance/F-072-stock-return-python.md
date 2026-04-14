---
type: procedure
created: 2026-03-22
status: active
slug: f-072-stock-return-python
tags: [procedure, nexus]
---

# Stock Return Calculation and Visualization (Python) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Descărcarea datelor de prețuri, calculul returnurilor zilnice și cumulative, și vizualizarea grafică a performanței folosind Python (yfinance + matplotlib).

---

## 1. Problema

Calculul manual al returnurilor pentru o perioadă lungă este laborios și predispus la erori. Python cu yfinance și matplotlib permite automatizarea completă: download date, calcul returnuri, statistici și vizualizare în câteva zeci de linii de cod. Această procedură este fundația oricărei analize cantitative a acțiunilor. Acoperă: setup, descărcare date, calcul returnuri, statistici de bază, și generare grafice.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Instalarea bibliotecilor necesare
Instalează în terminal: `pip install yfinance pandas matplotlib numpy`. Verifică instalarea: `import yfinance as yf; import pandas as pd; import matplotlib.pyplot as plt; import numpy as np`. Dacă folosești Jupyter Notebook: `!pip install yfinance` în prima celulă. Versiuni recomandate: yfinance >= 0.2.0, pandas >= 1.5, matplotlib >= 3.5.

### Pas 2: Descărcarea datelor istorice de prețuri cu yfinance
```python
import yfinance as yf
ticker = "AAPL"  # sau orice ticker valid
data = yf.download(ticker, start="2020-01-01", end="2024-12-31")
print(data.head())
print(data.shape)
```
Folosește `Adj Close` (adjusted for dividends and splits) nu `Close` pentru returnuri corecte. Verifică: nu sunt date lipsă (`.isnull().sum()`), datele acoperă perioada dorită, volumul este > 0 pentru toate zilele.

### Pas 3: Calculul returnurilor zilnice cu .pct_change()
```python
returns = data['Adj Close'].pct_change().dropna()
print(f"Medie returnuri zilnice: {returns.mean():.4f}")
print(f"Std returnuri zilnice: {returns.std():.4f}")
print(f"Min return: {returns.min():.4f}")
print(f"Max return: {returns.max():.4f}")
```
Interpretare: un return mediu zilnic de 0.05% = ~12.5% anual (aproximare). Returnul zilnic negativ cel mai mare indică evenimentele de criză.

### Pas 4: Calculul returnurilor cumulative
```python
cumulative_returns = (1 + returns).cumprod() - 1
total_return = cumulative_returns.iloc[-1]
print(f"Return total pe perioadă: {total_return:.2%}")

# Anualizat (CAGR):
n_years = len(returns) / 252
cagr = (1 + total_return) ** (1/n_years) - 1
print(f"CAGR: {cagr:.2%}")
```
Verifică CAGR față de returnul publicat de sursele externe pentru cross-validare.

### Pas 5: Construirea graficelor cu matplotlib
```python
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8))

# Grafic 1: Prețul ajustat
ax1.plot(data.index, data['Adj Close'], color='steelblue', linewidth=1.5)
ax1.set_title(f'{ticker} — Preț Ajustat', fontsize=14)
ax1.set_ylabel('Preț (USD)')
ax1.grid(alpha=0.3)

# Grafic 2: Return cumulativ
ax2.plot(cumulative_returns.index, cumulative_returns * 100, color='green', linewidth=1.5)
ax2.axhline(y=0, color='red', linestyle='--', alpha=0.5)
ax2.set_title(f'{ticker} — Return Cumulativ (%)', fontsize=14)
ax2.set_ylabel('Return (%)')
ax2.grid(alpha=0.3)

plt.tight_layout()
plt.savefig(f'{ticker}_performance.png', dpi=150)
plt.show()
```

### Pas 6: Calculul statisticilor cheie de performanță
```python
# Sharpe Ratio (anualizat, risk-free rate = 0.05)
rf_daily = 0.05 / 252
sharpe = (returns.mean() - rf_daily) / returns.std() * np.sqrt(252)

# Maximum Drawdown
rolling_max = (1 + returns).cumprod().cummax()
drawdown = ((1 + returns).cumprod() - rolling_max) / rolling_max
max_drawdown = drawdown.min()

print(f"Sharpe Ratio: {sharpe:.2f}")
print(f"Max Drawdown: {max_drawdown:.2%}")
print(f"Volatilitate anualizată: {returns.std() * np.sqrt(252):.2%}")
```

### Pas 7: Exportul raportului
Salvează: graficele ca PNG (`plt.savefig`), datele procesate ca CSV (`data.to_csv`), statisticile ca DataFrame în Excel sau print la consolă. Documentează metodologia: perioadă analizată, ajustări aplicate (dividende, splits), interpretarea statisticilor. Raportul final include: prețul grafic, return cumulativ, tabel statistici (CAGR, Sharpe, Max Drawdown, Volatilitate).

---

## 3. Cortex Logging

```json
{
  "text": "Stock Return Calculation Python F-072 executat: yfinance setup, date descarcate, returnuri zilnice/cumulative calculate, grafice generate, Sharpe/MaxDD calculate, raport exportat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-072",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["Python", "yfinance", "stock-returns", "matplotlib", "Sharpe-ratio", "quantitative-finance"],
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
1. `✅ [PROC] F-072 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Stock Return Calculation and Visualization (Python)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python 3.8+ | Runtime |
| yfinance | Descărcare date Yahoo Finance |
| pandas | Manipulare DataFrame |
| matplotlib / numpy | Vizualizare și calcule statistice |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Grafice generate | Minimum 2 (preț + return cumulativ) |
| Statistici calculate | Minimum 4 (CAGR, Sharpe, MaxDD, Volatilitate) |
