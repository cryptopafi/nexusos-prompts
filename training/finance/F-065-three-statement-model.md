---
type: procedure
created: 2026-03-22
status: active
slug: f-065-three-statement-model
tags: [procedure, nexus]
---

# 3-Statement Financial Model Build — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui model financiar integrat (Income Statement + Balance Sheet + Cash Flow Statement) cu cele trei declarații legate și verificate intern.

---

## 1. Problema

Un model financiar cu o singură situație (ex: doar P&L) nu captează efectele complete ale deciziilor de business — impactul CapEx pe bilanț, al creditelor pe cash flow, al profitabilității pe capitaluri proprii. Modelul 3-Statement complet este fundația oricărei analize de valorizare, planificare sau due diligence. Această procedură acoperă construcția pas cu pas, linkarea corectă și verificarea modelului.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Construirea tabului de Assumptions
Crează un tab separat "Assumptions" cu toate ipotezele modelului: creșterea veniturilor (% YoY per segment sau total), marjele (gross margin, EBITDA margin, net margin), zilele de Working Capital (DSO, DIO, DPO), rata de impozitare efectivă, CapEx ca % din venituri sau valoare absolută, politica de amortizare (durata medie activelor), structura de finanțare (dobânda la datorii). Toate celulele din model trebuie să se refere la acest tab — nicio hardcodare în model.

### Pas 2: Construirea Income Statement
Structura: Revenues (per linie de business dacă necesar) → COGS → Gross Profit → EBIT (Operating Profit: Gross Profit - OpEx) → EBITDA (adaugă D&A) → Interest Expense / Income → EBT (Earnings Before Tax) → Tax → Net Income → EPS (Net Income / Shares Outstanding). Proiectează 3-5 ani în față pe coloane. Verifică: liniile se adună corect, marjele sunt consistente cu Assumptions.

### Pas 3: Construirea Balance Sheet
Active: Cash (rând de legătură cu Cash Flow Statement), Receivables (Revenue × DSO/365), Inventory (COGS × DIO/365), Fixed Assets net (PP&E: prior period + CapEx - Depreciation), alte active. Pasive: Payables (COGS × DPO/365), datorii financiare pe termen scurt și lung, alte pasive. Capitaluri proprii: Share Capital + Retained Earnings (prior RE + Net Income - Dividende). Verifică: TOTAL ACTIVE = TOTAL PASIVE + CAPITALURI PROPRII (balanță trebuie să se închidă!).

### Pas 4: Construirea Cash Flow Statement
Metoda indirectă (recomandată): **CFO** = Net Income + D&A ± variații Working Capital (ΔReceivables, ΔInventory, ΔPayables). **CFI** = -CapEx ± vânzări active. **CFF** = emisiuni/rambursări datorii + emisiuni/răscumpărări acțiuni - dividende plătite. **Net Change in Cash** = CFO + CFI + CFF. **Ending Cash** = Beginning Cash + Net Change. Linkează Ending Cash la linia Cash din Bilanț.

### Pas 5: Linkarea celor trei situații
Linkurile critice care trebuie verificate: (1) Net Income (P&L) → Retained Earnings (Bilanț) + CFO; (2) Depreciation (P&L) → adăugat în CFO, scăzut din Fixed Assets (Bilanț); (3) CapEx (Assumptions) → scăzut în CFI, adăugat la Fixed Assets (Bilanț); (4) Datorii noi (CFF) → adăugate la Long-term Debt (Bilanț); (5) Ending Cash (CFStatement) = Cash (Bilanț). Verifică că fiecare link este o formulă, nu o valoare hardcodată.

### Pas 6: Verificarea că Bilanțul se echilibrează
Adaugă un rând de control: `Check = Total Active - (Total Pasive + Capitaluri Proprii)`. Dacă Check ≠ 0, modelul are o eroare de linkare. Identifică sistematic eroarea: verifică Retained Earnings (legătură P&L), verifică Cash (legătură CF Statement), verifică D&A (P&L → CFO și Fixed Assets). Un model care nu balansează nu poate fi folosit pentru valorizare.

### Pas 7: Adăugarea Scenario Manager
Creează 3 scenarii: **Base Case** (ipoteze centrale), **Bull Case** (creștere cu 10-20% mai mare, marje mai bune), **Bear Case** (creștere cu 20-30% mai mică, compresie marje). Implementează dropdown/named ranges în Excel sau tab-uri separate per scenariu. Adaugă summary output: Revenue, EBITDA, Net Income, FCF, EPS pentru fiecare scenariu. Creează grafice care arată divergența scenariilor în timp.

---

## 3. Cortex Logging

```json
{
  "text": "3-Statement Financial Model F-065 construit: Assumptions tab, Income Statement, Balance Sheet, Cash Flow Statement, linkuri verificate, bilant echilibrat, scenario manager adaugat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-065",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["financial-modeling", "3-statement-model", "income-statement", "balance-sheet", "cash-flow", "Excel"],
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
1. `✅ [PROC] F-065 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "3-Statement Financial Model Build" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Microsoft Excel / Google Sheets | Construcția și linkarea modelului |
| Rapoarte anuale (10-K / Raport Anual) | Date istorice pentru calibrarea ipotezelor |
| Balance Check formula | Verificarea echilibrului bilanțier |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Balance Check | = 0 (bilanț echilibrat) |
| Celule hardcodate în model | 0 (toate referite din Assumptions tab) |
