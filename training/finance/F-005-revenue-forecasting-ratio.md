---
type: procedure
created: 2026-03-22
status: active
slug: f-005-revenue-forecasting-ratio
tags: [procedure, nexus]
---

# Revenue Financial Forecasting (Ratio-Based) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui forecast de venituri bazat pe ratio-uri istorice și drivere macroeconomice, cu scenarii multiple, formule de regresie, și validare cross-funcțională.

---

## 1. Problema

Forecasting-ul de venituri fără o metodologie structurată produce estimări inconsistente și nedocumentate. Această procedură acoperă situațiile în care un analist trebuie să proiecteze veniturile pe 1-5 ani folosind date istorice și ratio-uri economice, inclusiv pentru bugete anuale, prezentări investitori, sau modele de evaluare. Un forecast slab propagă erori în DCF, capital budgeting și toate deciziile de alocare a capitalului.

> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectare și auditare date istorice (minimum 5 ani)
Adună minimum 5 ani de date de venituri din situațiile financiare auditate (10-K sau Raport Anual). Organizează datele pe segmente (produs/geografie/canal) dacă sunt disponibile. Verifică consistența perioadelor contabile (FY vs CY). Identifică schimbări de politici contabile (IFRS 15 / ASC 606 adoption), M&A care distorsionează seria, și one-time items (vânzări de active, contracte nerecurente). Ajustează pentru comparabilitate prin crearea unui „clean revenue" pro-forma. Surse: SEC EDGAR, BVB (pentru România), Bloomberg Terminal, sau Yahoo Finance.

**Formula Excel — Clean Revenue:**
```
=Revenue_Reported - OneTimeItems - DiscontinuedOperations
```

### Pas 2: Calculare rate de creștere și CAGR
Calculează rata de creștere an-la-an: `Growth Rate = (Revenue_t / Revenue_{t-1}) - 1`. Calculează media simplă, media ponderată (ponderi mai mari pe anii recenți: 1-2-3-4-5), și CAGR pe întreaga perioadă: `CAGR = (Revenue_final / Revenue_initial)^(1/n) - 1`. Identifică outlier-i (ani cu evenimente excepționale — COVID 2020, boom post-pandemic 2021). Calculează și mediana (mai robustă la outlier-i decât media).

**Formula Google Sheets — CAGR:**
```
=POWER(Revenue_Final/Revenue_Initial, 1/Years) - 1
```

**Formula Excel — Media ponderată (ponderi crescătoare):**
```
=SUMPRODUCT(GrowthRates, {1,2,3,4,5}) / SUM({1,2,3,4,5})
```

### Pas 3: Descompunere revenue în drivere fundamentale
Nu extrapola top-line direct — descompune revenue-ul în componentele fundamentale: `Revenue = Volume × Preț Mediu` sau `Revenue = Clienți × ARPU` sau `Revenue = Capacitate × Utilizare × Tarif`. Pentru fiecare componentă, analizează trendul separat. Exemplu retail: `Revenue = Same-Store Sales Growth + New Stores Contribution`. Exemplu SaaS: `Revenue = Existing ARR × (1 - Churn) + New ARR`. Descompunerea permite forecast mai granular și mai ușor de validat.

### Pas 4: Identificare trend, sezonalitate și ciclicitate
Determină tipul de trend (linear, exponențial, logistic/S-curve). Rulează o regresie OLS pe date istorice: `Revenue_t = α + β × t + ε`. Dacă R² > 0.85, trend-ul linear este suficient. Pentru sezonalitate, calculează indicii sezonali trimestriali: `Indice_Q = Revenue_Q / (Revenue_Anual / 4)`. Ajustează datele brute pentru one-time items. Folosește decompoziția clasică (trend + sezonalitate + rezidual) sau STL decomposition în Python. Verifică ciclicitatea față de ciclul economic (corelație cu GDP growth).

**Eroare frecventă de evitat:** Extrapolarea unui trend exponențial fără a verifica dacă piața suportă creșterea implicită (saturation check). Calculează: `Revenue_an5_proiectat / TAM < 30%` ca sanity check.

### Pas 5: Aplicare elasticitate față de drivere macroeconomice
Identifică driverele relevante: revenue/GDP, revenue/retail sales, revenue/industry output. Calculează elasticitatea istorică prin regresie: `ln(Revenue) = α + β × ln(GDP) + ε`. Coeficientul β = elasticitatea (β=1.2 înseamnă +1% GDP → +1.2% Revenue). Surse macro: FMI World Economic Outlook, BNR (România), FRED (SUA), Eurostat, Bloomberg Economics. Pentru context românesc: corelează cu creșterea PIB-ului României, rata inflației (IPC), cursul EUR/RON, și indicele producției industriale.

**Formula Excel — Elasticitate (prin regresie log-log):**
```
=SLOPE(LN(Revenue_Range), LN(GDP_Range))
```

### Pas 6: Construire scenarii Base/Bull/Bear cu probabilități
**Base Case (60%):** trend continuu + elasticitate medie față de macro; growth rate = media ponderată 5 ani. **Bull Case (20%):** creștere accelerată (percentila 75 istorică) + macro favorabil + un catalizator specific (lansare produs nou, expansiune geografică, M&A). **Bear Case (20%):** creștere decelerată (percentila 25 istorică) + macro nefavorabil + risc specific (pierdere client major, intrare competitor). Calculează Expected Value: `EV = 0.6 × Base + 0.2 × Bull + 0.2 × Bear`.

**Eroare frecventă de evitat:** Scenarii simetrice în jurul Base. Bear Case trebuie să fie mai sever decât Bull este optimist (distribuția pierderilor are fat tails). Aplică raport 1:1.5 (Bear deviere = 1.5× Bull deviere).

### Pas 7: Sensitivity analysis bidimensională
Construiește o tabelă 2-way în Excel (Data → What-If → Data Table): growth rate (axă X, ±5pp în incremente de 1pp) vs macro driver sau preț mediu (axă Y, ±10% în incremente de 2%). Output = Revenue Year 5 sau CAGR implicat. Identifică combinațiile critice care duc la break-even sau covenant breach. Aplică conditional formatting (heat map) pentru vizualizare rapidă. Documentează care variabile sunt cele mai sensibile (Tornado Chart recomandat).

### Pas 8: Validare cross-funcțională și documentare completă
Validează forecast-ul cu: (1) Departamentul comercial — sunt targetele de vânzări realiste? (2) Industry analysts — consensul de piață este aliniat? (3) Capacity check — compania are capacitatea fizică/umană de a livra revenue-ul proiectat? (4) Working capital check — poate finanța creșterea de NWC? Creează un tab „Assumptions" cu toate ipotezele explicit etichetate, sursa datelor, data ultimei actualizări, și owner-ul fiecărei ipoteze. Include o secțiune de limitări și riscuri ale modelului. Versioning: salvează modelul cu data în filename (Model_Revenue_v2.0_2026-03-20.xlsx).

### Pas 9: Reconciliere cu consensus și comunicare
Compară forecast-ul intern cu consensul analiștilor (Bloomberg BEST estimates, FactSet, Visible Alpha). Dacă diferența > 10%, documentează de ce și discută cu managementul. Pregătește un executive summary de 1 pagină: Revenue CAGR 5 ani (per scenariu), ipotezele cheie, riscurile principale, și range-ul de valori. Prezintă board-ului sau investment committee-ului.

**Context fiscal românesc:** Pentru companii românești, incorporează: impactul TVA (19% standard, 9% alimentar) asupra prețurilor, efectul cursului EUR/RON asupra exporturilor, sezonalitatea cheltuielilor guvernamentale (Q4 supradimensionat), și ciclul electoral (cheltuieli publice cresc pre-electoral).

---

## 3. Cortex Logging

```json
{
  "text": "Revenue Financial Forecasting (Ratio-Based) v2.0 executat: 5+ ani date istorice analizate, descompunere în drivere, elasticitate macro calculată, 3 scenarii construite, sensitivity 2-way completat, validare cross-funcțională realizată",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-005",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["revenue-forecasting", "ratio-analysis", "scenario-analysis", "CAGR", "elasticity", "sensitivity", "intermediate"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
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
- Forecast livrat fără scenarii multiple (min. Base + Bear) → violation
- Ipoteze nedocumentate sau fără sursă → violation
- Revenue descompus doar la nivel top-line fără drivere → violation
- Sensitivity analysis absentă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-069 → Sensitivity Analysis (procedura detaliată de sensitivity)
- F-005 → input critic pentru F-031 (Capital Budgeting) și F-065 (3-Statement Model)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?
- [ ] Revenue descompus în drivere fundamentale?
- [ ] Elasticitate macro calculată?
- [ ] Scenarii cu probabilități și Expected Value?

**VK-uri:**
1. `✅ [PROC] F-005 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Revenue Financial Forecasting (Ratio-Based)" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Situații financiare auditate (5+ ani) | Sursă date istorice |
| Date macro (FMI/BNR/FRED/Bloomberg) | Drivere externe și elasticitate |
| Excel / Google Sheets | Construire model, Data Tables, regression |
| SEC EDGAR / BVB / Rapoarte anuale | Surse financiare publice |
| Bloomberg Terminal / Yahoo Finance / Finviz | Date comparabile și consensus |
| TradingView | Vizualizare trends și corelații macro |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași (9/9) |
| VK emisie | 2/2 |
| Scenarii construite | Minimum 3 (Base/Bull/Bear) cu probabilități |
| Ipoteze documentate | 100% cu sursă și owner |
| Sensitivity variables | Minimum 2 variabile cheie analizate (2-way table) |
| Forecast accuracy vs actual (ex-post) | MAPE < 10% la 1 an |
| Validare cross-funcțională | Minimum 2 surse de validare |
