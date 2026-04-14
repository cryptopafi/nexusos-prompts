---
type: procedure
created: 2026-03-22
status: active
slug: f-054-investment-style-selection
tags: [procedure, nexus]
---

# Investment Style Selection (Value / Growth / Momentum) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea și selectarea stilului de investiție (value, growth, momentum, quality, low volatility) potrivit pentru obiectivele investitorului și regimul de piață curent.

---

## 1. Problema

Stilurile de investiție au performanțe ciclice — value outperforms în unele perioade, growth în altele. Alegerea incorectă a stilului sau menținerea unui stil nepotrivit regimului de piață duce la underperformance prelungit și frustrazione pentru investitor. Această procedură acoperă situațiile de selecție inițială de stil sau reevaluare periodică a stilului pentru un portofoliu.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire horizon temporal și obiective de return
Clarifică: investitorul are un horizon lung (>10 ani) sau scurt (<5 ani)? Un horizon lung permite toleranță la perioadele de underperformance ale unui stil (ex: value a subperformat 2010-2020, dar a revenit 2021-2023). Obiective: capital appreciation (growth/momentum) vs. income + capital preservation (value + quality). Documentează că stilul ales trebuie să fie menținut pe minimum un ciclu complet de piață (3-7 ani).

### Pas 2: Înțelegerea caracteristicilor fiecărui stil principal
**Value**: cumperi companii ieftine față de fundamentale (P/B, P/E, EV/EBITDA scăzut). Premium historic: ~3%/an vs. market (Fama-French). Risc: value traps (companii ieftine care merită să fie ieftine). **Growth**: cumperi companii cu creștere rapidă, chiar la premium de valuation. Sensibil la rata dobânzii (duration lungă). **Momentum**: cumperi winners recenți (3-12 luni), evitezi losers. Premium historic: ~5-7%/an, dar crash-uri bruște (momentum crashes). **Quality**: profitabilitate ridicată, bilanț solid, durabilitate competitive advantage. Defensiv. **Low Volatility**: minimum variance stocks, performează bine în bear markets, subperformează în bull markets puternice.

### Pas 3: Backtesting fiecărui stil în medii de piață relevante
Analizează performanța fiecărui stil în diferite medii: (a) Creștere economică accelerată: growth + momentum outperform. (b) Recesiune/bear market: quality + low volatility + value outperform. (c) Inflație ridicată: value (commodities-heavy) + real assets outperform. (d) Dobânzi în creștere: value > growth (growth are duration lungă). Surse date: AQR factor library, French Data Library, MSCI factor indices. Documentează performanța relativă per mediu macroeconomic.

### Pas 4: Evaluare premium de stil ajustat la risc
Calculează pentru fiecare stil: Return anual vs. market (alpha față de MSCI World / S&P 500), Volatilitate relativă, Sharpe Ratio al stilului, Max Drawdown specific stilului (momentum crashes pot fi -30% în câteva săptămâni). Evaluează persistența premium-ului: este stilul recunoscut academic (peer-reviewed) sau doar backtest-fit? Crowd-edness: dacă toată lumea urmează același stil, premium-ul poate fi erodat.

### Pas 5: Selectare stil(uri) pentru regimul de piață curent
Identifică regimul macroeconomic actual: faza ciclului economic (expansiune / vârf / contracție / recuperare), nivelul ratei dobânzii și direcția, nivelul de valuation al piețelor (Shiller CAPE). Mapează la stilul cel mai potrivit per analiză din Pas 3. Decide: single style (concentrat, mai mult active risk) sau multi-style blended (value 40% + quality 40% + momentum 20% = diversificare factor). Documentează raționamentul explicit.

### Pas 6: Selectare fund / ETF care implementează stilul ales
Evaluează vehiculele de implementare: ETF-uri factor (iShares MSCI Value / MSCI Momentum / MSCI Quality), mutual funds cu mandată de stil clar, sau construct-ți portofoliu propriu de titluri cu screening de factori. Criterii de selecție ETF: expense ratio (cel mai mic), AUM (lichiditate), factor loading pur (high R² față de factorul dorit), turnover (momentum ETF-urile au turnover ridicat → cost mai mare). Compară minimum 3 vehicule alternative per stil selectat.

### Pas 7: Monitorizare trimestrială a factor exposure
Monitorizează trimestrial: factor loading al portofoliului (folosind Barra sau Bloomberg factor model), performanța relativă vs. benchmark pe 3-6-12 luni, semne de style drift (fondul sau titlurile migrează spre alt stil fără intenție). Reevaluează stilul anual sau când regimul macro se schimbă semnificativ. Nu schimba stilul după o perioadă scurtă de underperformance — verifică mai întâi dacă underperformance este sistematic sau aleator.

---

## 3. Cortex Logging

```json
{
  "text": "Investment Style Selection executat: stiluri principale analizate (value/growth/momentum/quality), backtest per mediu macro realizat, stil selectat și vehicul de implementare ales, monitorizare factor exposure planificată",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-054",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["investment-style", "factor-investing", "value", "growth", "momentum", "quality", "portfolio-management", "intermediate"],
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
- Stil selectat fără backtesting în medii macro diferite → violation
- Selectare fund/ETF fără comparare minimum 3 vehicule → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-053 → Portfolio Construction (implementare după selecție stil)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-054 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Investment Style Selection (Value / Growth / Momentum)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| AQR Factor Library / French Data Library | Date istorice de factor performance |
| Bloomberg / Morningstar | ETF screening și factor loading |
| MSCI Factor Indices | Benchmark per stil |
| Barra Factor Model | Monitorizare factor exposure portofoliu |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Stiluri analizate | Minimum 4 (value, growth, momentum, quality) |
| Medii macro analizate în backtest | Minimum 3 (expansiune, recesiune, inflație) |
| ETF-uri / fonduri comparate | Minimum 3 per stil selectat |
