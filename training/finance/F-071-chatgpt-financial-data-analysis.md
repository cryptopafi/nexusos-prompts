---
type: procedure
created: 2026-03-22
status: active
slug: f-071-chatgpt-financial-data-analysis
tags: [procedure, nexus]
---

# ChatGPT-Assisted Financial Data Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Utilizarea ChatGPT (Code Interpreter / Advanced Data Analysis) pentru analiza datelor financiare, identificarea pattern-urilor și generarea de vizualizări.

---

## 1. Problema

Analiza manuală a seturilor mari de date financiare (prețuri istorice, situații financiare, date macro) consumă timp și poate pierde pattern-uri neobvioase. ChatGPT cu Code Interpreter poate automatiza calculele, genera grafice și identifica anomalii — dar outputul AI trebuie verificat riguros. Această procedură acoperă: colectarea datelor, prompting structurat, cross-verificarea, și documentarea insight-urilor AI-assisted.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectarea datelor financiare
Descarcă datele din surse de încredere: **Yahoo Finance** (Finance.yahoo.com → Historical Data → CSV): prețuri zilnice, ajustate pentru dividende și splits; **SEC EDGAR** (edgar.sec.gov): 10-K/10-Q în format structured (XBRL data sau PDF); **Federal Reserve FRED** (fred.stlouisfed.org): date macro (PIB, inflație, dobânzi); **Bloomberg/Refinitiv** dacă ai acces. Pregătește datele: curăță erori evidente, asigură formatul consistent (date format uniform, header-e clare, fără rânduri goale).

### Pas 2: Pregătirea datelor în format structurat (CSV/JSON)
Structurează fișierul: fiecare coloană = o variabilă (Date, Open, High, Low, Close, Volume, Revenue, EPS etc.), fiecare rând = o observație (zi, trimestru, an). Elimină: duplicatele, zilele fără tranzacționare (weekends), valorile lipsă sau zero (tratează cu NaN sau interpolate justificat). Salvează în CSV cu delimiter virgulă, encoding UTF-8. Dimensiune maximă recomandată pentru ChatGPT: < 50MB per upload.

### Pas 3: Prompting ChatGPT pentru analiză (pattern-uri/anomalii/ratiouri)
Structurează prompt-urile specific: (1) "Analyze this financial data and calculate: [lista indicatorilor specifici]"; (2) "Identify any statistical anomalies or outliers in the [variabilă] column"; (3) "Plot [variabilă X] vs [variabilă Y] and describe the relationship"; (4) "Calculate the correlation matrix between all numeric variables". Evită prompt-uri vagi ("tell me about this data"). Cere explicit output-ul dorit (tabel, grafic, interpretare text).

### Pas 4: Cross-verificarea output-ului AI față de calculele manuale
Obligatoriu: verifică manual 20-30% din calculele cheie produse de AI. Compară: ratele financiare calculate de ChatGPT vs Excel manual; statisticile (mean, std, correlation) vs Python/R independent; orice afirmație factual care poate fi verificată. Dacă există discrepanțe: identifică sursa (date diferite? formulă diferită?), corectează ipotezele, re-rulează. Nu prezenta output AI nevenificat.

### Pas 5: Utilizarea Code Interpreter pentru generarea graficelor
Solicită ChatGPT să genereze: (1) Price chart cu volume (matplotlib); (2) Correlation heatmap; (3) Distribuție returns (histogram cu normal distribution overlay); (4) Scatter plot returns vs benchmark; (5) Rolling metrics (rolling Sharpe, rolling volatility). Descarcă graficele generate. Verifică că axele sunt etichetate corect, scalele sunt logice, și că graficele nu sunt înșelătoare (truncated axes etc).

### Pas 6: Identificarea insight-urilor neobvioase în date
Explorează cu ChatGPT: "What patterns are not obvious from a simple visual inspection?", "Are there seasonal patterns in this data?", "What is the statistical significance of [observație]?", "What are the top 3 predictors of [variabilă target]?". Documentează insight-urile descoperite, inclusiv limitările (correlation ≠ causation, sample size, data quality issues).

### Pas 7: Documentarea analizei AI-assisted
Scrie raportul: metodologia (ce date, ce perioadă, ce instrumente AI), insight-urile principale (cu graficele generate), limitările analizei (ce nu poate fi validat, ce date lipsesc), concluzia și implicațiile pentru decizia de investiție. Menționează explicit că analiza a fost AI-assisted și care componente au fost verificate manual.

---

## 3. Cortex Logging

```json
{
  "text": "ChatGPT Financial Data Analysis F-071 executat: date colectate/structurate, prompting specific, output verificat manual, grafice generate cu Code Interpreter, insights documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-071",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["AI-analysis", "ChatGPT", "Code-Interpreter", "financial-data", "data-analysis", "Python"],
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
1. `✅ [PROC] F-071 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "ChatGPT-Assisted Financial Data Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT Plus (GPT-4 cu Code Interpreter) | Analiza datelor și generarea graficelor |
| Yahoo Finance / SEC EDGAR / FRED | Surse date financiare verificate |
| Excel / Python | Cross-verificarea calculelor AI |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rata de verificare manuală | > 20% din calcule AI verificate |
| Grafice generate | Minimum 3 per analiză |
