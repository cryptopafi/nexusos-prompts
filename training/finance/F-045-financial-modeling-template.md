---
type: procedure
created: 2026-03-22
status: active
slug: f-045-financial-modeling-template
tags: [procedure, nexus]
---

# 7-Step Financial Modeling Template — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui model financiar complet și prezentabil în 7 pași standardizați, de la definirea scopului la output-ul final pentru decizie.

---

## 1. Problema

Modelele financiare construite fără o structură standardizată sunt greu de auditat, actualizat, și comunicat stakeholderilor. Această procedură definește template-ul de referință pentru orice model financiar complex (DCF, LBO, merger model, project finance), aplicabil în evaluare, investment banking, sau FP&A.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definire scop și output-uri
Clarifică înainte de a scrie prima formulă: ce decizie va informa modelul? Ce output-uri sunt necesare (NPV, share price, IRR, credit metrics)? Cine sunt utilizatorii modelului (board, investitori, management)? Cât de detaliat trebuie să fie (high-level strategic vs. detaliat operațional)? Documentează scopul pe pagina de Cover a modelului. Un model fără scop clar = efort pierdut.

### Pas 2: Tab Assumptions — toate input-urile clar etichetate
Toate ipotezele se introduc EXCLUSIV în tabul Assumptions, niciodată hardcoded în formule. Structurează pe secțiuni: Macro (inflație, FX, PIB), Operational (creștere venituri, marje, CapEx/Revenue), Financial (WACC, tax rate, cost of debt), și Project-specific. Colorează celulele input în albastru. Adaugă coloana Source/Rationale pentru fiecare ipoteză. Versioning: data ultimei actualizări pe fiecare tab.

### Pas 3: Tab Historical Financials (3-5 ani)
Adaugă datele istorice din rapoartele anuale: Income Statement, Balance Sheet, Cash Flow Statement. Calculează KPI-urile istorice: marje (gross/EBITDA/net), ratiourile de creștere, ratiourile de rentabilitate (ROIC/ROE/ROA), ratiourile de leverage. Istoricul servește ca bază de calibrare pentru ipotezele de forecast și ca check de rezonabilitate.

### Pas 4: Tab Revenue Model (pe segment/produs)
Decompune revenue-ul în componentele sale fundamentale: unități × preț, sau clienți × ARPU, sau capacitate × utilizare. Modelează fiecare segment separat cu ipoteze proprii de creștere. Adaugă Market Share Analysis dacă relevanță. Include ramp-up pentru produse/piețe noi. Sum-up la revenue total consolidat care alimentează Income Statement.

### Pas 5: Tab Cost Model (fix vs. variabil)
Clasifică fiecare linie de cost: fix (nu variază cu volumul — overhead, D&A, rent) vs. variabil (variază cu volumul — COGS direct, comisioane). Modelează costurile fixe ca valori absolute și costurile variabile ca % din revenue (sau pe unitate). Calculează EBITDA, EBIT, EBT, Net Income. Construiește waterfall vizual de la Revenue la Net Income.

### Pas 6: Three-Statement Model integrat (IS/BS/CF)
Integrează cele 3 situații financiare: Income Statement alimentează Retained Earnings în Balance Sheet. Cash Flow Statement reconciliază BS opening cu BS closing. Verifici integrarea: BS trebuie să balanseze (Assets = Liabilities + Equity) în fiecare an. Cash din BS = Cash din CF. Acest tab este inima modelului — erori de integrare invalidează toate output-urile.

### Pas 7: Tab Output (DCF / Comps / Sensitivity) și formatare finală
Output-uri specifice scopului: DCF → Enterprise Value, Equity Value, implied share price. Comps → trading multiples vs. peer set. Sensitivity → 2-way table cu variabilele cheie. Formatează pentru prezentare: font consistent (Calibri 10), header-uri clare, zero hardcoded numbers vizibile. Adaugă Executive Summary tab: 1 pagină cu concluzia și metricile cheie. Fișierul final: versioned, shared read-only cu stakeholderii.

---

## 3. Cortex Logging

```json
{
  "text": "7-Step Financial Modeling Template executat: model complet construit cu Assumptions, Historical, Revenue, Costs, 3-statement integrat, Output cu DCF/sensitivity, formatat pentru prezentare",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-045",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["financial-modeling", "DCF", "three-statement-model", "valuation", "advanced"],
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
- Model cu inputuri hardcoded în formule (nu în Assumptions tab) → violation
- Three-statement model neintegrat (BS nu balansează) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-031 → capital budgeting model (aplicare practică a template-ului)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-045 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "7-Step Financial Modeling Template" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Situații financiare auditate | Historical data |
| Bloomberg / CapIQ | Peer comps și macro data |
| Excel (funcții avansate: INDEX/MATCH, XIRR, Data Tables) | Construire model |
| PowerPoint | Executive Summary pentru stakeholders |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Taburi model | Minimum 7 (Cover, Assumptions, Historical, Revenue, Costs, 3-Statement, Output) |
| BS balance check | 100% (Assets = L + E în fiecare an) |
| Inputuri hardcoded în formule | 0 (zero) |
