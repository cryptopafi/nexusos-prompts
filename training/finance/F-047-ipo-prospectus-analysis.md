---
type: procedure
created: 2026-03-22
status: active
slug: f-047-ipo-prospectus-analysis
tags: [procedure, nexus]
---

# S-1 / IPO Prospectus Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiza sistematică a unui S-1 sau prospectus de IPO pentru formarea unei teze de investiție documentate, de la extragerea metricilor financiare la evaluare vs. comparabile.

---

## 1. Problema

Un S-1 are sute de pagini cu informații dense și uneori obfuscate. Fără un framework sistematic, un analist poate rata riscurile materiale sau poate fi orbit de marketing-ul narrativ al companiei. Această procedură acoperă situațiile de evaluare pre-IPO pentru investitori instituționali, retail investors sofisticați, sau analiști de research care trebuie să producă o opinie de investiție.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Obținere și organizare S-1 de pe SEC EDGAR
Accesează SEC EDGAR (sec.gov/cgi-bin/browse-edgar). Caută după companie sau ticker (dacă există deja). Descarcă S-1 principal și toate amendamentele (S-1/A). Organizează: identifică secțiunile principale — Prospectus Summary, Risk Factors, Use of Proceeds, MD&A, Financial Statements, Business Description, Management, Principal Shareholders.

### Pas 2: Citire MD&A și extragere metrici financiare cheie
Citește Management Discussion & Analysis cu atenție — este secțiunea unde managementul explică rezultatele. Extrage: Revenue (și YoY growth), Gross Profit și Gross Margin %, EBITDA/Operating Loss, Net Income/Loss, Cash Burn Rate (dacă pre-profitabil), Cash și Cash Equivalents, Debt total. Calculează metrici derivate: Revenue CAGR (3 ani), gross margin trend, operating leverage (crește sau scade pierderea ca % din revenue).

### Pas 3: Identificare top 5 riscuri materiale din Risk Factors
Secțiunea Risk Factors poate fi deliberat lungă și plină de boilerplate. Filtrează: identifica riscurile care sunt specifice companiei (nu generice), cele cu impact financiar cuantificabil, cele cu probabilitate rezonabilă. Categorizează: Business risks (competiție, dependență client/furnizor), Financial risks (burn rate, covenant breach, dilution), Regulatory risks (sector-specific), Technology/execution risks. Rezumă top 5 ca tabelă cu risc + severitate + mitigant.

### Pas 4: Analiză use of proceeds
Citește secțiunea „Use of Proceeds" (cum va folosi compania banii strânși din IPO). Clasifică intenția: R&D / capex → growth investment (pozitiv), Working capital → operational nevoie (neutru), Repayment of debt → curățenie bilanț (neutru-pozitiv), General corporate purposes → vag (semnal de atenție), Selling shareholders (secondary offering) → insiderii se lichidează (red flag major dacă proporție mare). Calculează % din proceeds pentru fiecare categorie.

### Pas 5: Evaluare insider ownership și lockup-uri
Identifică structura acționariatului post-IPO: % deținut de fondatori, management, VC/PE investors, și publicul nou. Găsește lockup period (tipic 180 zile) și volumul de acțiuni care se eliberează. Calculează „overhang" — # acțiuni care pot fi vândute post-lockup ca % din float. Overhang ridicat (>50% din float) = risc de presiune vânzare la expirarea lockup-ului. Notează dacă există dual-class shares (clasă B cu vot superior pentru fondatori).

### Pas 6: Evaluare valuation vs. companii comparabile
Calculează implied valuation din IPO: Enterprise Value (EV) = Market Cap + Net Debt, sau din prețul de ofertă × acțiuni total. Calculează multiplii: EV/Revenue, EV/EBITDA (dacă profitabil), P/E (dacă profit). Identifică 3-5 peer companies publice similare ca profil (industrie, model de business, stadiu de maturitate). Compară multiplii IPO vs. peers. IPO la premium semnificativ față de peers → risc de supraapreciere.

### Pas 7: Formare teză de investiție
Sintetizează: Bull case (de ce compania merită premium), Bear case (de ce riscurile sunt subevaluate), Base case (valuation fair range). Evaluează: management track record, moat competitiv (network effects / switching costs / IP), calitatea creșterii (organic vs. achiziții / paid marketing). Concluzie: Participi la IPO / Aștepți post-lockup / Evitați. Documentează teza și prețul-țintă de intrare.

---

## 3. Cortex Logging

```json
{
  "text": "S-1 / IPO Prospectus Analysis executat: metrici financiare extrase, top 5 riscuri identificate, use of proceeds analizat, ownership și lockup evaluate, valuation vs. comparabile realizată, teză de investiție formată",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-047",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["IPO-analysis", "S-1", "prospectus", "valuation", "investment-thesis", "advanced"],
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
- Analiză fără evaluare lockup overhang → violation
- Teză de investiție fără comparare valuation vs. peers → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-046 → revenue modeling (complementar pentru model financiar detaliat)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-047 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "S-1 / IPO Prospectus Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SEC EDGAR (sec.gov) | Sursă S-1 și amendamente |
| Bloomberg / CapIQ | Peer comparables și multiples |
| Pitchbook / Crunchbase | Date funding rounds și valuation istorică |
| Excel | Calcul multiples și comparare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Riscuri materiale identificate | Minimum 5 specifice companiei |
| Peer companies comparate | Minimum 3 |
| Teză de investiție | Concluzie clară cu raționament |
