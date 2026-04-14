---
type: procedure
created: 2026-03-22
status: active
slug: f-091-options-hedging-strategy
tags: [procedure, nexus]
---

# Options-Based Hedging Strategy Design — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proiectarea și implementarea unei strategii de hedging cu opțiuni pentru protejarea unui portofoliu de investiții împotriva riscurilor de scădere, cu managementul complet al Greeks.

---

## 1. Problema

Deținerile de portofoliu sunt expuse la riscuri semnificative de downside pe care deținerea simplă a activelor nu le mitigă. Opțiunile permit protecție precisă la un cost controlat, dar necesită înțelegerea Greeks (delta/gamma/theta/vega) și monitorizare activă. Fără o strategie coerentă, hedging-ul poate fi prea scump sau inadecvat. Aceasta procedură acoperă: identificarea expunerii, selectarea instrumentelor, sizing, P&L la expirare, execuție și roll.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificarea expunerii de hedged
Definește precis: (1) **Natura expunerii**: portofoliu equity (S&P 500 correlation > 0.8), expunere valutară (EUR/USD pentru companie exportatoare), commodity (petrol pentru companie de transport), rate dobândă (portofoliu obligatari); (2) **Mărimea expunerii**: valoarea totală în USD/EUR a poziției de hedged; (3) **Orizontul de timp**: când este expunerea relevantă (1M, 3M, 6M, 1Y); (4) **Nivelul de protecție dorit**: protecție totală vs parțială (ex: hedge 75% din portofoliu).

### Pas 2: Selectarea tipului de opțiune/strategie
Alege strategia în funcție de obiectiv și cost acceptat: (1) **Protective Put**: cumpără put pe index/ETF (SPY put dacă ai portofoliu equity); cea mai simplă, protecție totală sub strike, cost = premiu put; (2) **Collar**: cumperi put + vinzi call finanțând parțial costul putului; limitezi upside dar reduci costul net; (3) **Put Spread**: cumperi put la strike mai sus + vinzi put la strike mai jos; cost mai mic, dar protecție limitată la banda spread-ului; (4) **VIX Calls**: hedging indirect prin cumpărare calls pe VIX (VIX crește în crize); cost mai mic, corelație imperfectă.

### Pas 3: Calculul dimensiunii poziției și costul hedge-ului
```
# Exemplu: Hedge portofoliu equity de $500,000

Valoare portofoliu: $500,000
Beta portofoliu vs SPY: 1.15
Valoare ajustată beta: $500,000 × 1.15 = $575,000
Preț SPY: $450
Contract SPY Put (100 shares): 100 × $450 = $45,000
Contracte necesare: $575,000 / $45,000 = 12.8 → 13 contracte

Premiu put (ATM 3M): $12.50 per share
Cost per contract: 100 × $12.50 = $1,250
Cost total hedge: 13 × $1,250 = $16,250
Cost ca % portofoliu: $16,250 / $500,000 = 3.25% pe 3 luni
Cost anualizat: 3.25% × 4 = 13% anual (scump!)
→ Consideră put spread pentru reducerea costului
```

### Pas 4: Analiza Break-Even și P&L la expirare
Construiește graficul P&L la expirare pentru fiecare strategie considerată:

**Protective Put (SPY 450 Put, premiu $12.50)**:
- SPY > 450 la expirare: pierdere = premiu plătit ($12.50/share)
- SPY = 437.50: break-even (450 strike - 12.50 premiu)
- SPY < 437.50: put intră în bani, câștigul offsette pierderea din portofoliu

**Collar (450 Put cumpărat, 480 Call vândut)**:
- SPY > 480: upside limitat (compania câștigă max la 480)
- SPY 437.50-480: zona protejată fără câștig net din options
- SPY < 437.50: protecție activată

Calculează costul net: dacă premiu put > premiu call primit → zero-cost collar nu există, costul net = diferența.

### Pas 5: Compararea cost vs reducere potențiala a pierderii
Cuantifică beneficiul hedge-ului: Scenario bear market -20% (SPY de la 450 la 360): fără hedge pierdere = -20% × $500,000 = -$100,000; cu protective put pierdere = -3.25% premiu + -0% pe pierderea sub 450 = -$16,250 (în loc de $100,000). Break-even hedge: costul premiu ($16,250) / (probabilitate crash × pierdere medie crash). Dacă estimated expected value (EV) a hedge-ului este negativ dar dormi mai bine noaptea → "insurance premium" justificat.

### Pas 6: Executarea hedge-ului
Înainte de execuție: verifică bid-ask spread (evită opțiuni cu spread > 5% din premiu), verifică open interest (> 1,000 contracte = lichiditate bună), selectează expirarea (standard lunară sau trimestrială pentru lichiditate maximă). La execuție: folosește ordine limit la mid-price, nu market orders (spread-urile la opțiuni sunt mari). Confirmă execuția, înregistrează prețul mediu de execuție și Greeks la momentul cumpărării.

### Pas 7: Monitorizarea Greeks și roll/închidere la expirare
Monitorizează săptămânal: **Delta**: cât se modifică valoarea hedge-ului per $1 mișcare a activului; dacă delta s-a schimbat semnificativ față de target → rebalansare; **Gamma**: accelerarea deltei (maximă near expiry — optiunile devin mai sensibile); **Theta**: time decay zilnic (costul "asigurării" care se erodează zilnic); **Vega**: sensibilitate la volatilitate implicită (opțiunile devin mai scumpe dacă VIX crește). La 3-4 săptămâni înainte de expirare: **Roll forward** (închide poziția actuală și deschide una cu expirare mai îndepărtată) pentru a menține protecția continuă. Documentează costul total al rolling-ului.

---

## 3. Cortex Logging

```json
{
  "text": "Options Hedging Strategy F-091 proiectata: expunere identificata, strategie protective put/collar selectata, sizing calculat, P&L expirare analizat, executie documentata, Greeks monitorizate, roll schedule definit.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-091",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["options", "hedging", "protective-put", "collar", "Greeks", "delta", "theta", "risk-management"],
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
1. `✅ [PROC] F-091 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Options-Based Hedging Strategy Design" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Options broker platform (IBKR/TD Ameritrade) | Execuția tranzacțiilor cu opțiuni |
| Options chain data | Prețuri, Greeks, open interest |
| Options P&L calculator / Excel | Analiza P&L la expirare per strategie |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Cost hedge ca % din portofoliu | < 5% anual (target: 2-3%) |
| Frecvență monitorizare Greeks | Săptămânal |
| Roll forward lead time | 3-4 săptămâni înainte de expirare |
