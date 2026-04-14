---
type: procedure
created: 2026-03-22
status: active
slug: f-007-discretionary-accruals-model
tags: [procedure, nexus]
---

# Discretionary Accruals Model (Modified Jones) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Detectarea potențialului earnings management prin calcularea accrualelor discreționare folosind modelul Modified Jones, cu extensii Kothari (performance-matched) și analiză Beneish M-Score.

---

## 1. Problema

Analiza calității câștigurilor necesită separarea accrualelor normale (non-discreționare) de cele manipulate (discreționare). Companiile care practică earnings management pot supraestima profitul cu 2-5% din active, ducând la decizii de investiție eronate. Această procedură acoperă situațiile de forensic accounting, due diligence pre-achiziție, screening investițional, sau auditare externă, unde analistul trebuie să cuantifice gradul de manipulare potențială a profitului net. Detectarea timpurie a earnings management poate preveni pierderi semnificative — Enron, WorldCom, și Wirecard au arătat consecințele ignorării semnalelor.

> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectare situații financiare și pregătire date (5 ani)
Extrage din rapoartele anuale (10-K, Raport Anual BVB): Net Income (NI), Cash Flow from Operations (CFO), Total Assets (TA), variația conturilor de creanțe (ΔAR), variația veniturilor (ΔRevenue), Gross PP&E, ROA. Asigură-te că datele sunt comparabile (aceleași politici contabile). Notează orice schimbări de politici contabile (IFRS 15 adoption, schimbare metoda de amortizare). Sursele: SEC EDGAR XBRL, Bloomberg Terminal, sau Refinitiv Eikon. Pentru companii românești: BVB e-raportare, MFP portal.

**Formula Excel — Total Accruals (metoda bilanțieră alternativă):**
```
=ΔCurrentAssets - ΔCash - ΔCurrentLiabilities + ΔShortTermDebt - Depreciation
```

### Pas 2: Calculare total accruals și normalizare
Formula directă: `Total Accruals (TA) = Net Income - Cash Flow from Operations`. Normalizează prin total active de la începutul perioadei: `TA_scaled = TA / Total_Assets_{t-1}`. Verifică semnul: accruale pozitive persistente sugerează recunoaștere agresivă a veniturilor. Compară cu industria: accruale în exces față de mediana sectorului = semnal de alertă.

**Formula Google Sheets:**
```
=( NetIncome - CFO ) / LaggedTotalAssets
```

**Eroare frecventă de evitat:** Nu confunda Total Accruals cu Accrued Expenses din bilanț. Total Accruals = diferența completă între profitul contabil și cash flow-ul operațional.

### Pas 3: Estimare accruale non-discreționare — Modified Jones Model
Modelul Modified Jones estimează componenta „normală" a accrualelor prin regresia cross-secțională pe date de industrie:

`TA_i/A_{i,t-1} = α₁(1/A_{i,t-1}) + α₂(ΔRevenue_i - ΔAR_i)/A_{i,t-1} + α₃(PPE_i/A_{i,t-1}) + ε_i`

Modificarea Jones vs Original Jones: scade ΔAR din ΔRevenue (presupune că creșterea creanțelor din creșterea vânzărilor nu este „normală" — poate fi manipulare). Rulează regresia OLS folosind minimum 15-20 companii din același SIC code (2-digit). Coeficienții α₁, α₂, α₃ sunt estimați pe industrie, apoi aplicați la compania analizată.

**Python (statsmodels):**
```python
import statsmodels.api as sm
X = sm.add_constant(industry_df[['inv_assets', 'adj_rev_change', 'ppe_scaled']])
model = sm.OLS(industry_df['ta_scaled'], X).fit()
nda = model.predict(company_data)
```

### Pas 4: Extensie Kothari — Performance-Matched Discretionary Accruals
Modelul Kothari adaugă ROA ca variabilă de control pentru a elimina correlația dintre performanță și accruale:

`TA_i/A = α₁(1/A) + α₂(ΔRev - ΔAR)/A + α₃(PPE/A) + α₄(ROA_{t-1}) + ε`

ROA controlează faptul că firmele profitabile au în mod natural accruale diferite. Alternativ: match fiecare firmă cu o „control firm" cu ROA similar din aceeași industrie și calculează DA = TA_firma - TA_control. Kothari reduce semnificativ false positives în detectarea earnings management.

### Pas 5: Calculare accruale discreționare și interpretare
`Discretionary Accruals (DA) = Total Accruals_scaled - NDA (estimat din model)`. DA pozitiv mare → posibilă supraestimare a profitului (income-increasing manipulation). DA negativ mare → posibilă subestimare (earnings bath / big bath accounting — managementul „curăță" un an pentru a arăta recovery în anul următor). Interpretează residualul din regresie ca proxy pentru DA.

**Praguri de alertă:**
- |DA| > 2% din total active → semnal de atenție
- |DA| > 5% din total active → red flag major
- DA persistent pozitiv 3+ ani consecutivi → pattern sistematic de manipulare

### Pas 6: Calculare Beneish M-Score pentru confirmare
Beneish M-Score = model cu 8 variabile care estimează probabilitatea de manipulare:

`M = -4.84 + 0.920×DSRI + 0.528×GMI + 0.404×AQI + 0.892×SGI + 0.115×DEPI - 0.172×SGAI + 4.679×TATA - 0.327×LVGI`

Unde: DSRI = Days Sales Receivable Index, GMI = Gross Margin Index, AQI = Asset Quality Index, SGI = Sales Growth Index, DEPI = Depreciation Index, SGAI = SGA Expense Index, TATA = Total Accruals to Total Assets, LVGI = Leverage Index.

**Interpretare:** M > -1.78 → probabilitate ridicată de manipulare (Enron avea M = -1.89 în 2000, Beneish l-a identificat ca manipulator înainte de colaps). M < -2.22 → probabilitate redusă.

**Excel:** Calculează fiecare componentă separat, apoi aplică formula compozită.

### Pas 7: Benchmark față de industrie și perechi
Compară DA calculat cu mediana industriei (folosind peer companies din același SIC code 2-digit sau GICS sub-industry). Calculează percentila companiei analizate în distribuția industriei. DA în percentila >75 → semnal de alertă pentru manipulare agresivă. Construiește grafic boxplot al DA pe industrie cu compania evidențiată. Surse peer data: COMPUSTAT, Bloomberg, sau Refinitiv Eikon. Pentru România: BVB sector reports.

### Pas 8: Corroborare, raportare și comunicare
Coroborează DA cu alte semnale calitative: schimbări recente de management/CFO, presiune pe targets (EPS guidance tight), bonusuri legate de EPS/EBITDA, comisioane de audit neobișnuit de mari, schimbare auditor (de la Big 4 la non-Big 4 = red flag), existența whistleblower complaints, SEC comment letters. Calculează scorul agregat de risc de manipulare (Low/Medium/High).

Documentează concluzia în raport: (1) DA calculat și percentila, (2) Beneish M-Score, (3) Semnale calitative, (4) Concluzia și nivelul de încredere, (5) Recomandarea (investiție sigură / due diligence suplimentar necesar / evită).

**Eroare frecventă de evitat:** DA ridicat ≠ fraudă dovedită. Este un screening tool, nu o dovadă. Mulți factori legitimi pot produce DA ridicat (creștere rapidă, M&A, schimbare model de business). Folosește ca input într-o analiză mai largă, nu ca verdict final.

---

## 3. Cortex Logging

```json
{
  "text": "Discretionary Accruals Model (Modified Jones + Kothari + Beneish) v2.0 executat: total accruals calculate, Modified Jones OLS rulat, Kothari performance-matched, Beneish M-Score calculat, benchmarkat față de industrie, semnale calitative coroborate",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-007",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["discretionary-accruals", "modified-jones", "kothari", "beneish-m-score", "earnings-management", "forensic-accounting", "advanced"],
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
- DA calculat fără benchmark față de industrie → violation
- Flag emis fără corroborare cu alte semnale → violation
- Model rulat cu <15 companii peer → violation (sample insuficient)
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance


### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-007 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Discretionary Accruals Model (Modified Jones)" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Situații financiare auditate (5+ ani) | Date pentru calcul accruale |
| Python (statsmodels) / R / Excel (regresie OLS) | Estimare model Modified Jones și Kothari |
| Bază de date peer companies (COMPUSTAT/Bloomberg/BVB) | Benchmark industrie și regresie cross-secțională |
| SEC EDGAR / BVB e-raportare | Sursă filing-uri 10-K și rapoarte anuale |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași (8/8) |
| VK emisie | 2/2 |
| R² model regresie | Minimum 0.25 pentru validitate (cross-secțional) |
| Peer set industrie | Minimum 15 companii comparabile |
| Beneish M-Score calculat | Da, cu interpretare explicită |
| Concluzie documentată | Cu nivel de încredere explicit (Low/Medium/High) |
