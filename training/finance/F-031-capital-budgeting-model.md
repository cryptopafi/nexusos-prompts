---
type: procedure
created: 2026-03-22
status: active
slug: f-031-capital-budgeting-model
tags: [procedure, nexus]
---

# Capital Budgeting Full Model Build (Excel) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui model complet de capital budgeting în Excel pentru evaluarea unui proiect de investiții, incluzând FCF, NPV/IRR, sensitivity și scenario analysis.

---

## 1. Problema

Luarea deciziilor de investiții fără un model structurat și documentat expune compania la erori de evaluare și investiții distructoare de valoare. Această procedură acoperă situațiile în care un analist financiar trebuie să construiască de la zero un model de capital budgeting pentru un proiect de CapEx sau investiție strategică.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire scope proiect și timeline
Clarifică cu stakeholderii: scopul proiectului (expansion / replacement / new product), durata de viață economică (ani), data de start, și output-urile așteptate ale modelului (NPV / IRR / Payback / MOIC). Creează structura fișierului Excel cu taburi: Assumptions, Revenue, Costs, FCF, Valuation, Sensitivity, Scenarios. Aplică formatare consistentă (albastru = input, negru = formula, verde = output).

### Pas 2: Construire proiecții de venituri
În tabul Revenue: modelează volumele unitare și prețul per unitate pe fiecare an al proiectului. Dacă proiectul are mai multe segmente sau produse, modelează separat. Aplică ipoteze de creștere documentate în tabul Assumptions. Include ipoteze de ramp-up în primii ani. Calculează Revenue total anual ca sumă a segmentelor.

### Pas 3: Construire structura de costuri (CapEx + OpEx)
CapEx: construiește schedule de investiții inițiale și recurente (capex maintenance). OpEx: descompune în costuri fixe (overhead, amortizare ROU, staff) și variabile (direct materials, comisioane). Calculează EBITDA (Revenue - OpEx fără D&A). Calculează D&A pe baza programului de amortizare CapEx. Calculează EBIT = EBITDA - D&A.

### Pas 4: Calculare Free Cash Flows
Formula: `FCF = EBIT × (1 - Tax Rate) + D&A - ΔWorking Capital - CapEx`. NOPAT = EBIT × (1 - t). Add-back D&A (non-cash). Scade ΔWC (creșterea NWC = cash out). Scade CapEx maintenance și additional. Prezintă FCF pe fiecare an al proiectului. Calculează Terminal Value dacă proiectul continuă: `TV = FCF_n × (1+g) / (WACC - g)`.

### Pas 5: Construire analiză NPV / IRR
Determină rata de actualizare (WACC al companiei sau hurdle rate ajustat pentru risc). Calculează NPV: `NPV = Σ [FCF_t / (1 + WACC)^t] - Initial Investment`. Calculează IRR (rata care face NPV = 0, folosind funcția Excel IRR sau XIRR). Calculează Payback Period și Discounted Payback Period. Calculează MOIC dacă relevant. Decizie: NPV > 0 și IRR > Hurdle Rate → accept.

### Pas 6: Sensitivity analysis (2-way table)
Construiește tabelul de sensitivitate cu: variabila 1 pe axă (ex: Revenue growth rate: ±5%, ±10%, ±15%), variabila 2 pe cealaltă axă (ex: WACC: ±1%, ±2%, ±3%). Valoarea din celule: NPV sau IRR. Evidențiază zona în care proiectul devine NPV negativ. Identifică care variabilă are impactul cel mai mare (tornado chart recomandat).

### Pas 7: Scenario analysis și prezentare stakeholders
Definește scenarii: Base (ipoteze centrale), Upside (ipoteze optimiste — percentila 75), Downside (ipoteze conservatoare — percentila 25). Calculează NPV/IRR pentru fiecare scenariu. Prezintă concluzia: recomandare clară (Accept / Reject / Condiționat), cu justificarea bazată pe model. Include executive summary cu un slide de sinteză.

---

## 3. Cortex Logging

```json
{
  "text": "Capital Budgeting Full Model Build (Excel) executat: model complet construit cu FCF, NPV/IRR, sensitivity 2-way și scenario analysis, prezentat stakeholderilor",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-031",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["capital-budgeting", "NPV", "IRR", "DCF", "financial-modeling", "corporate-finance", "advanced"],
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
- Model fără sensitivity analysis → violation
- Decizie fără justificare explicită bazată pe NPV/IRR → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-031 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Capital Budgeting Full Model Build (Excel)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Business case / project brief | Scope și ipoteze de bază |
| WACC al companiei (sau hurdle rate) | Rata de actualizare |
| CapEx schedule din engineering/operations | Date investiție |
| Excel (funcții NPV, IRR, XIRR, data tables) | Construire model |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Taburi model | Minimum 6 (Assumptions, Revenue, Costs, FCF, Valuation, Sensitivity) |
| Scenarii analizate | Minimum 3 (Base/Upside/Downside) |
| Sensitivitate testată | Minimum 2 variabile cheie |
