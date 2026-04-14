---
type: procedure
created: 2026-03-22
status: active
slug: f-056-var-expected-shortfall
tags: [procedure, nexus]
---

# VaR (Value at Risk) and Expected Shortfall Calculation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Calcularea VaR și Expected Shortfall (CVaR) pentru un portofoliu de investiții, cu stress testing față de crize istorice, folosind metoda parametrică, istorică, sau Monte Carlo.

---

## 1. Problema

VaR și Expected Shortfall sunt metricile standard de risc de piață reglementate (Basel III/IV) și utilizate de toate instituțiile financiare. Fără aceste calcule, un manager de portofoliu nu poate cuantifica pierderea maximă potențială și nu poate comunica riscul în termeni calibrați. Această procedură acoperă situațiile de risk reporting, compliance regulatoriu, sau comunicare cu board-ul și investitorii.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire parametri de calcul
Stabilește: Confidence Level (95% sau 99% — 99% este standard Basel), Time Horizon (1-day pentru trading books, 10-day pentru regulatory capital, 1-month pentru portfolio management), Metoda de calcul (parametrică / istorică / Monte Carlo — alege în funcție de distribuția returnurilor), Perioada de date istorice (minimum 250 trading days = 1 an pentru metoda istorică). Documentează aceste parametri în raportul de risc — VaR fără context este lipsit de sens.

### Pas 2: Calculare VaR parametrică (dacă distribuție normală)
Verifică mai întâi normalitatea returnurilor (Jarque-Bera test sau Q-Q plot). Dacă returnurile sunt aproximativ normale: `VaR (95%) = Portfolio Value × (μ_daily - 1.645 × σ_daily)` sau exprimat ca pierdere: `VaR_loss = Portfolio Value × (1.645 × σ_daily - μ_daily)`. Pentru VaR 99%: z = 2.326. Anualizare: `VaR_10day = VaR_1day × √10`. Exprimă VaR în $ și în % din portofoliu.

### Pas 3: Calculare VaR istorică (Historical Simulation)
Sortează randamentele zilnice istorice (250+ zile) de la cel mai negativ la cel mai pozitiv. VaR 95% = al 5-lea percentil (12-13 din 250 observații). VaR 99% = al 1-lea percentil (2-3 din 250 observații). Avantaj: captează fat tails și asimetria reală a distribuției. Dezavantaj: presupune că trecutul reprezintă viitorul. Verifică că perioada de date include cel puțin o perioadă de stres (ex: 2008, 2020 COVID).

### Pas 4: Calculare Expected Shortfall (CVaR)
Expected Shortfall = media pierderilor care depășesc VaR-ul. Formula: `ES = E[Loss | Loss > VaR]`. Practic (metoda istorică): calculezi media tuturor randamentelor mai negative decât percentila VaR. ES 95% = media celor mai slabi 5% din returneuri. ES este superior VaR deoarece măsoară magnitudinea pierderilor din coada distribuției (nu doar punctul de break). Sub Basel III, ES la 97.5% înlocuiește VaR la 99% pentru capital requirements.

### Pas 5: Stress Testing față de crize istorice
Rulează scenarii de stres cu date din crize majore: (a) Criza Financiară Globală 2008: S&P -57% peak-to-trough, VIX > 80. (b) COVID Crash martie 2020: S&P -34% în 33 zile, lichiditate colapsată în anumite piețe. (c) Dot-com bubble 2000-2002: tech -78%, 2.5 ani. (d) Rate shock 2022: bonds -20%, equities -20% simultan. Aplică randamentele istorice din aceste scenarii la portofoliu curent și calculează P&L impact.

### Pas 6: Raportare VaR în $ și % — format standard
Creează raportul de risc: (1) VaR 1-day la 99%: $X sau Y% din portofoliu. (2) VaR 10-day la 99%: $Z (regulatory). (3) Expected Shortfall 97.5%: $W — „dacă VaR este depășit, pierderea medie așteptată este $W". (4) Tabel stress test: pierderea estimată în fiecare scenariu de criză. (5) Backtesting: în ultimele 250 zile, de câte ori a fost VaR depășit? (≤ 5 depășiri = model valid la 99% confidence). Prezintă raportul board-ului cu interpretare clară în limbaj non-tehnic.

---

## 3. Cortex Logging

```json
{
  "text": "VaR și Expected Shortfall calculat: VaR parametric și istoric calculat la 95% și 99%, ES/CVaR calculat, stress test față de 2008/2020/2022 realizat, raport de risc complet generat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-056",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["VaR", "Expected-Shortfall", "CVaR", "risk-measurement", "Basel", "stress-testing", "advanced"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- VaR raportat fără Expected Shortfall → violation (ES este superior și necesar pentru context complet)
- Stress test omis → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-055 → Portfolio Risk Assessment (context MPT complet)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-056 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "VaR (Value at Risk) and Expected Shortfall Calculation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Prețuri zilnice (250+ zile) | Date pentru VaR istoric |
| Python (numpy, scipy.stats) / Excel | Calcul VaR parametric și ES |
| Risk management system | VaR real-time și backtesting |
| Date istorice crize (2008/2020/2022) | Stress testing |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Metode VaR calculate | Minimum 2 (parametric + istoric) |
| Scenarii de stres | Minimum 3 crize istorice |
| Backtesting VaR | Exceedances ≤ 5/250 zile la 99% confidence |
