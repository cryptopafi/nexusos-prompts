---
type: procedure
created: 2026-03-22
status: active
slug: f-069-sensitivity-analysis
tags: [procedure, nexus]
---

# Sensitivity Analysis (Data Tables and Scenario Manager) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea și interpretarea analizei de sensitivitate folosind Excel Data Tables și Scenario Manager pentru identificarea driver-ilor cheie de valoare.

---

## 1. Problema

Orice model financiar conține incertitudine în ipoteze. Fără analiza de sensitivitate nu poți ști care variabile au cel mai mare impact asupra rezultatelor și care variații de ipoteze sunt acceptabile. Aceasta procedură acoperă: identificarea driver-ilor cheie, construcția tabelelor de sensitivitate one-way și two-way, Scenario Manager, și interpretarea rezultatelor pentru decizii.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificarea driver-ilor cheie de valoare
Listează variabilele de input care au cel mai probabil impact major: (1) **Revenue growth rate** — variație ±2-5%/an schimbă radical valorizarea; (2) **EBITDA margin** — 1pp de marjă = mare impact la valorizare; (3) **Discount rate / WACC** — ±1% WACC poate schimba fair value cu 10-20%; (4) **Terminal growth rate** — variație mică = impact enorm pe terminal value; (5) **CapEx intensity** — direct impact pe FCF. Prioritizează 2-4 variabile pentru analysis.

### Pas 2: Construirea tabelului de sensitivitate one-way (1 variabilă vs output)
În Excel: (1) Definește output-ul de analizat (ex: Intrinsic Value per share); (2) Setează valorile de testat pentru input (ex: WACC: 6%, 7%, 8%, 9%, 10%) pe o coloană; (3) Adaugă formula output pe primul rând; (4) Selectează tabelul (coloana input + formulă output); (5) Data → What-If Analysis → Data Table → Column input cell (selectează celula WACC din model). Excel calculează automat output-ul pentru fiecare valoare de input. Formatează cu color scales pentru vizualizare rapidă.

### Pas 3: Construirea tabelului de sensitivitate two-way (2 variabile vs output)
Cel mai comun: WACC (pe rânduri) vs Revenue Growth (pe coloane), cu valoarea intrinsecă în mijloc. Procedura Excel: (1) Pune valorile WACC pe un rând (orizontal) și valorile Revenue Growth pe o coloană (vertical); (2) La intersecția rând/coloană, pune formula output; (3) Data → What-If Analysis → Data Table → Row input cell (Revenue Growth din model) + Column input cell (WACC din model). Formatează cu heat map: roșu = valoare sub prețul curent, verde = valoare peste prețul curent.

### Pas 4: Crearea Scenario Manager (Base/Bull/Bear)
Excel Data → What-If Analysis → Scenario Manager: Adaugă 3 scenarii: **Base Case** — toate ipotezele centrale; **Bull Case** — revenue growth +10%, marje +2pp, WACC -0.5%; **Bear Case** — revenue growth -15%, marje -3pp, WACC +1%. Pentru fiecare scenariu, definește valorile celulelor de input. Creează Scenario Summary care arată output-urile cheie (Revenue, EBITDA, Net Income, FCF, EV, Equity Value/share) pentru fiecare scenariu simultan.

### Pas 5: Identificarea variabilelor cu cel mai mare impact (Tornado Chart)
Construiește Tornado Chart: pentru fiecare variabilă key, calculează impactul unui shock de ±1 standard deviation pe output-ul final. Ordonează variabilele de la cel mai mare la cel mai mic impact. Graficul arată vizual care variabile "contează cel mai mult" pentru valorizare. Această analiză ghidează: pe ce să focusezi cercetarea suplimentară și unde să fii mai conservator în ipoteze.

### Pas 6: Documentarea rezultatelor și implicațiilor
Scrie interpretarea: (1) Ce range de valori intrinsece este rezonabil (ex: $150-$250/share)? (2) La ce combinație de ipoteze acțiunea este sub-/supravalorizată? (3) Care este scenariul cel mai probabil și de ce? (4) Ce trebuie să se întâmple (bear case) pentru ca investiția să fie o pierdere? Include tabelele și graficele în prezentarea finală a tezei de investiție.

---

## 3. Cortex Logging

```json
{
  "text": "Sensitivity Analysis F-069 executat: driveri cheie identificati, tabele one-way si two-way construite, Scenario Manager configurat, Tornado Chart creat, implicatii documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-069",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["sensitivity-analysis", "data-tables", "scenario-manager", "what-if-analysis", "Excel", "WACC", "financial-modeling"],
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
1. `✅ [PROC] F-069 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Sensitivity Analysis (Data Tables and Scenario Manager)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Microsoft Excel (Data → What-If Analysis) | Data Tables și Scenario Manager |
| Model financiar de bază (F-065 sau F-067) | Modelul pe care se aplică sensitivitatea |
| Conditional formatting / Heat maps | Vizualizarea tabelelor de sensitivitate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Număr variabile analizate | Minimum 4 (WACC, growth, marje, CapEx) |
| Număr scenarii | 3 (base/bull/bear) |
