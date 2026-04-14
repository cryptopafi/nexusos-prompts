---
type: procedure
created: 2026-03-22
status: active
slug: f-076-correlation-analysis-python
tags: [procedure, nexus]
---

# Correlation Analysis for Portfolio Construction (Python) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Calculul și interpretarea matricei de corelație între active financiare pentru construcția unui portofoliu diversificat care minimizează riscul prin selectarea activelor cu corelație scăzută.

---

## 1. Problema

Diversificarea este singurul "free lunch" în investiții (Markowitz), dar funcționează doar dacă activele au corelații scăzute sau negative. Fără analiza cantitativă a corelațiilor, diversificarea poate fi iluzorie — multe active care par diferite au corelații > 0.8 în perioade de stres. Această procedură acoperă: colectarea returnurilor pentru 5-10 active, calculul matricei, vizualizarea heat map, și construcția portofoliului diversificat.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectarea returnurilor pentru 5-10 active diversificate
```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Un mix de clase de active diferite
tickers = {
    'SPY': 'US Equities',
    'EFA': 'International Equities',
    'BND': 'US Bonds',
    'GLD': 'Gold',
    'VNQ': 'Real Estate (REIT)',
    'DBC': 'Commodities',
    'TIP': 'TIPS (Inflation Protected)',
    'EMB': 'Emerging Market Bonds'
}

data = yf.download(list(tickers.keys()), start="2015-01-01", end="2024-12-31")['Adj Close']
returns = data.pct_change().dropna()
returns.columns = [tickers[t] for t in returns.columns]

print(f"Active analizate: {len(tickers)}")
print(f"Observatii: {len(returns)} zile")
print(f"\nStatistici de baza:")
print(returns.describe().round(4))
```

### Pas 2: Calculul matricei de corelație cu pd.DataFrame.corr()
```python
# Correlatie Pearson (standard)
corr_matrix = returns.corr(method='pearson')
print("\n=== MATRICE DE CORELATIE (Pearson) ===")
print(corr_matrix.round(3))

# Correlatie Spearman (mai robusta la outlieri)
corr_spearman = returns.corr(method='spearman')

# Comparatie - diferente mari intre Pearson si Spearman sugereaza relatii non-liniare
diff = (corr_matrix - corr_spearman).abs()
print(f"\nDiferenta medie Pearson vs Spearman: {diff.values[np.triu_indices_from(diff.values, k=1)].mean():.4f}")
```

### Pas 3: Vizualizarea heat map cu seaborn
```python
fig, axes = plt.subplots(1, 2, figsize=(18, 7))

# Heatmap corelatie
mask = np.triu(np.ones_like(corr_matrix, dtype=bool))  # Ascunde triunghiul superior (duplicat)
sns.heatmap(
    corr_matrix,
    annot=True, fmt='.2f',
    cmap='RdYlGn',  # Rosu = corelatie negativa, Verde = pozitiva
    center=0,
    vmin=-1, vmax=1,
    mask=mask,
    ax=axes[0],
    square=True,
    linewidths=0.5
)
axes[0].set_title('Matrice Corelatie (Pearson)\n2015-2024', fontsize=12)

# Rolling correlation intre 2 active cheie (ex: equities vs bonds)
window = 252  # 1 an
rolling_corr = returns['US Equities'].rolling(window).corr(returns['US Bonds'])
axes[1].plot(rolling_corr.index, rolling_corr, color='steelblue', linewidth=1.5)
axes[1].axhline(y=0, color='red', linestyle='--', alpha=0.7)
axes[1].fill_between(rolling_corr.index, rolling_corr, 0, alpha=0.2)
axes[1].set_title('Corelatie Rolling (252 zile)\nUS Equities vs US Bonds', fontsize=12)
axes[1].set_ylabel('Corelatie')
axes[1].grid(alpha=0.3)

plt.tight_layout()
plt.savefig('correlation_analysis.png', dpi=150)
plt.show()
```

### Pas 4: Identificarea perechilor cu corelație ridicată (>0.8)
```python
print("=== PERECHI CU CORELATIE RIDICATA (> 0.80) ===")
print("Acestea nu ofera diversificare reala:\n")

high_corr_pairs = []
for i in range(len(corr_matrix.columns)):
    for j in range(i+1, len(corr_matrix.columns)):
        corr_val = corr_matrix.iloc[i, j]
        if abs(corr_val) > 0.80:
            col1, col2 = corr_matrix.columns[i], corr_matrix.columns[j]
            high_corr_pairs.append((col1, col2, corr_val))
            print(f"  {col1} <-> {col2}: {corr_val:.3f}")

if not high_corr_pairs:
    print("  Nicio pereche cu corelatie > 0.80 identificata")
```

### Pas 5: Identificarea perechilor cu corelație scăzută/negativă (<0.3) pentru diversificare
```python
print("\n=== PERECHI CU CORELATIE MICA / NEGATIVA (< 0.30) ===")
print("Acestea ofera diversificare reala:\n")

low_corr_pairs = []
for i in range(len(corr_matrix.columns)):
    for j in range(i+1, len(corr_matrix.columns)):
        corr_val = corr_matrix.iloc[i, j]
        if corr_val < 0.30:
            col1, col2 = corr_matrix.columns[i], corr_matrix.columns[j]
            low_corr_pairs.append((col1, col2, corr_val))
            print(f"  {col1} <-> {col2}: {corr_val:.3f}")
```

### Pas 6: Calculul impactului corelației portofoliului
```python
# Portofoliu equal-weight ca baza
weights = np.array([1/len(returns.columns)] * len(returns.columns))
port_return = (returns * weights).sum(axis=1)

# Volatilitate portofoliu vs media ponderata a volatilitatilor individuale
individual_vols = returns.std() * np.sqrt(252)
avg_weighted_vol = (individual_vols * weights).sum()
port_vol = port_return.std() * np.sqrt(252)
diversification_ratio = avg_weighted_vol / port_vol

print(f"\n=== BENEFICIU DIVERSIFICARE ===")
print(f"Volatilitate medie ponderata active individuale: {avg_weighted_vol:.2%}")
print(f"Volatilitate portofoliu equal-weight: {port_vol:.2%}")
print(f"Diversification Ratio: {diversification_ratio:.2f}x")
print(f"Reducere risc prin diversificare: {(1 - port_vol/avg_weighted_vol):.1%}")
```

### Pas 7: Construirea portofoliului diversificat minimizând corelația
Recomandare pe baza analizei: (1) Exclud sau limitez activele cu corelație > 0.8 față de altele din portofoliu (aleg unul din pereche); (2) Includ obligatoriu active cu corelație < 0.3 (ex: Gold și Bonds față de Equities în perioade normale); (3) Monitorizez corelațiile rolling — în crize (2008, 2020) corelațiile cresc brusc, reducând diversificarea; (4) Construiesc portofoliu final cu greutăți care maximizează Diversification Ratio sau minimizează volatilitatea portofoliului.

---

## 3. Cortex Logging

```json
{
  "text": "Correlation Analysis Portfolio Construction F-076 executat: returnuri 8 active colectate, matrice corelatie calculata, heatmap generat, perechi high/low-corr identificate, beneficiu diversificare calculat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-076",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["correlation-analysis", "portfolio-construction", "diversification", "seaborn", "Python", "Markowitz", "asset-allocation"],
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
1. `✅ [PROC] F-076 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Correlation Analysis for Portfolio Construction (Python)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Python (yfinance, pandas, seaborn, numpy) | Runtime și vizualizare |
| seaborn.heatmap | Vizualizare matrice corelație |
| Minimum 5 active diversificate | Analiză relevantă de portofoliu |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Active analizate | 5-10 clase diferite |
| Diversification Ratio | > 1.2 (20% reducere risc prin diversificare) |
