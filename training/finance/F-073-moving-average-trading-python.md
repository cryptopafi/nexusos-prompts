---
type: procedure
created: 2026-03-22
status: active
slug: f-073-moving-average-trading-python
tags: [procedure, nexus]
---

# Simple Moving Average Trading Strategy (Python) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea, backtestarea și evaluarea unei strategii de trading bazate pe crossover-ul SMA-50/SMA-200 folosind Python.

---

## 1. Problema

Strategiile bazate pe moving averages sunt printre cele mai populare sisteme mecanice de trading, dar eficiența lor reală trebuie testată cantitativ. Fără backtest riguros și comparație cu buy-and-hold, nu poți ști dacă SMA crossover adaugă valoare reală sau doar iluzia controlului. Aceasta procedură acoperă: implementare completă în Python cu semnale, backtest, metrici de performanță, și vizualizare.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Descărcarea datelor cu yfinance
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

ticker = "SPY"  # ETF S&P 500 ca proxy de piata
data = yf.download(ticker, start="2010-01-01", end="2024-12-31")
df = data[['Adj Close']].copy()
df.columns = ['Price']
print(f"Perioada: {df.index[0].date()} - {df.index[-1].date()}")
print(f"Observatii: {len(df)}")
```
Folosește minimum 10 ani de date pentru un backtest reprezentativ care include atât bull cât și bear markets.

### Pas 2: Calculul SMA-50 și SMA-200
```python
df['SMA50'] = df['Price'].rolling(window=50).mean()
df['SMA200'] = df['Price'].rolling(window=200).mean()

# Elimina primele 200 de zile (insufficient history)
df = df.dropna()
print(f"Date disponibile dupa warming period: {len(df)} zile")
```
SMA-50 = media prețului din ultimele 50 de zile (trend pe termen mediu). SMA-200 = media ultimelor 200 de zile (trend pe termen lung). Crossover SMA50 > SMA200 = "Golden Cross" (semnal bullish). Crossover SMA50 < SMA200 = "Death Cross" (semnal bearish).

### Pas 3: Generarea semnalelor de trading
```python
df['Signal'] = 0
df.loc[df['SMA50'] > df['SMA200'], 'Signal'] = 1   # Long / Buy
df.loc[df['SMA50'] < df['SMA200'], 'Signal'] = 0   # Cash / Sell

# Identificarea tranzactiilor (schimbarea semnalului)
df['Position'] = df['Signal'].diff()
buy_signals = df[df['Position'] == 1]
sell_signals = df[df['Position'] == -1]

print(f"Numar cumparari: {len(buy_signals)}")
print(f"Numar vanzari: {len(sell_signals)}")
```

### Pas 4: Backtestarea strategiei — calculul returnurilor
```python
df['Daily_Return'] = df['Price'].pct_change()

# Strategia: returnul zilnic × semnalul (1 = investit, 0 = cash)
df['Strategy_Return'] = df['Daily_Return'] * df['Signal'].shift(1)

# Returnuri cumulative
df['BuyHold_Cumulative'] = (1 + df['Daily_Return']).cumprod()
df['Strategy_Cumulative'] = (1 + df['Strategy_Return']).cumprod()

print(f"Buy & Hold total return: {df['BuyHold_Cumulative'].iloc[-1] - 1:.2%}")
print(f"SMA Strategy total return: {df['Strategy_Cumulative'].iloc[-1] - 1:.2%}")
```

### Pas 5: Compararea strategiei vs buy-and-hold cu metrici cheie
```python
n_years = len(df) / 252

# CAGR
bh_cagr = df['BuyHold_Cumulative'].iloc[-1] ** (1/n_years) - 1
strat_cagr = df['Strategy_Cumulative'].iloc[-1] ** (1/n_years) - 1

# Sharpe Ratio
rf = 0.03 / 252
bh_sharpe = (df['Daily_Return'].mean() - rf) / df['Daily_Return'].std() * np.sqrt(252)
strat_sharpe = (df['Strategy_Return'].mean() - rf) / df['Strategy_Return'].std() * np.sqrt(252)

# Maximum Drawdown
strat_dd = (df['Strategy_Cumulative'] / df['Strategy_Cumulative'].cummax() - 1).min()
bh_dd = (df['BuyHold_Cumulative'] / df['BuyHold_Cumulative'].cummax() - 1).min()

print(f"\nComparatie Strategie vs Buy & Hold:")
print(f"CAGR: {strat_cagr:.2%} vs {bh_cagr:.2%}")
print(f"Sharpe: {strat_sharpe:.2f} vs {bh_sharpe:.2f}")
print(f"Max Drawdown: {strat_dd:.2%} vs {bh_dd:.2%}")
```

### Pas 6: Calculul Sharpe Ratio și max drawdown
Analiza completă: dacă Sharpe strategiei > Sharpe buy-and-hold → strategie justificată. Dacă Max Drawdown strategie < Max Drawdown buy-and-hold → strategie reduce riscul. Evaluează: rata de câștig (% tranzacții profitabile), costul de tranzacționare estimat (fiecare semnal = 2 tranzacții). Adaugă estimare cost: `n_trades × 0.001` (0.1% per tranzacție = realistic pentru retail).

### Pas 7: Vizualizarea semnalelor pe graficul de prețuri
```python
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(14, 10))

# Grafic 1: Pretul cu SMA si semnale
ax1.plot(df.index, df['Price'], alpha=0.7, label='Price', color='black', linewidth=1)
ax1.plot(df.index, df['SMA50'], label='SMA 50', color='blue', linewidth=1.5)
ax1.plot(df.index, df['SMA200'], label='SMA 200', color='red', linewidth=1.5)
ax1.scatter(buy_signals.index, buy_signals['Price'], marker='^', color='green', s=50, label='Buy', zorder=5)
ax1.scatter(sell_signals.index, sell_signals['Price'], marker='v', color='red', s=50, label='Sell', zorder=5)
ax1.legend(); ax1.set_title(f'{ticker} SMA Strategy Signals'); ax1.grid(alpha=0.3)

# Grafic 2: Performanta cumulativa comparata
ax2.plot(df.index, df['Strategy_Cumulative'], label='SMA Strategy', color='blue')
ax2.plot(df.index, df['BuyHold_Cumulative'], label='Buy & Hold', color='green')
ax2.legend(); ax2.set_title('Strategy vs Buy & Hold'); ax2.grid(alpha=0.3)

plt.tight_layout()
plt.savefig(f'{ticker}_sma_strategy.png', dpi=150)
plt.show()
```

---

## 3. Cortex Logging

```json
{
  "text": "Moving Average Trading Strategy Python F-073 implementat: SMA50/SMA200 calculate, semnale generate, backtest complet, Sharpe/MaxDD comparate vs buy-and-hold, grafice vizualizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-073",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["Python", "trading-strategy", "moving-average", "SMA", "backtest", "golden-cross", "quantitative-finance"],
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
1. `✅ [PROC] F-073 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Simple Moving Average Trading Strategy (Python)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (yfinance, pandas, numpy, matplotlib) | Runtime complet pentru strategie |
| Date istorice minimum 10 ani | Backtest reprezentativ |
| Calculatoare costuri tranzacționare | Evaluare realistă a performanței nete |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Perioada backtest | Minimum 10 ani |
| Metrici calculate | CAGR, Sharpe, MaxDD, Win Rate |
