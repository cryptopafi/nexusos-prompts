---
type: procedure
created: 2026-03-22
status: active
slug: f-094-venture-capital-evaluation
tags: [procedure, nexus]
---

# Venture Capital / Private Investment Evaluation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Procesul de due diligence complet pentru evaluarea unui startup sau companie privată ca oportunitate de investiție de tip venture capital sau angel investment.

---

## 1. Problema

Investițiile în startup-uri au rate de eșec de 90%+, dar câștigătorii pot returna 10-100x investiția. Fără un cadru riguros de evaluare, selecția devine intuitivă și susceptibilă la bias. Due diligence profesional la VC cuprinde: analiza pitch deck, TAM, unit economics, echipă, moat, projections sanity check, și termenii investiției. Aceasta procedură structurează procesul complet de la prima vedere la decizia de investiție.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Revizuirea pitch deck-ului și modelului de business
Prima lectură: evaluează dacă înțelegi în 5 minute CE face compania și PENTRU CINE. Slide-urile critice: problemă (este real și dureros?), soluție (este 10× mai bună decât alternativa?), produs (demo, screenshot, metrici de engagement), business model (cum generezi venituri: SaaS/marketplace/transactional/hardware), traction (ARR, growth rate, NPS, retention), team, market size, financiale, ce faci cu banii și use of proceeds. Red flags la pitch: "nu avem competitori" (piața nu există sau nu ai căutat suficient), projections hockey-stick fără mecanismul de creștere explicitat, fondatori care nu pot articula un client specific cu o problemă specifică.

### Pas 2: Validarea dimensiunii pieței (TAM/SAM/SOM)
Verifică rigoarea analizei de piață: **TAM** (Total Addressable Market) — piața totală dacă captezi 100% din clienți potențiali; fii sceptic față de TAM-uri de trilioane de dolari fără susținere; **SAM** (Serviceable Addressable Market) — segmentul pe care startup-ul îl poate servi realist cu soluția actuală; **SOM** (Serviceable Obtainable Market) — cota realistă de piață în 3-5 ani. Validează independent: rapoarte de industrie (Gartner/IDC/Forrester), dimensionare bottom-up (număr clienți potențiali × revenue per client), benchmark față de companii similare la stage similar.

### Pas 3: Analiza unit economics — CAC/LTV/Payback period
Calculele fundamentale de viabilitate: **CAC** (Customer Acquisition Cost) = total spending pe Sales & Marketing / număr clienți noi achiziționați în aceeași perioadă; **LTV** (Lifetime Value) = ARPU (Average Revenue Per User) / Churn Rate; sau mai precis: LTV = Gross Margin × (1/Churn Rate); **LTV:CAC ratio**: < 1 = business unsustainable, 1-3 = marginal, > 3 = healthy (B2B SaaS target > 3:1); **Payback Period** = CAC / (ARPU monthly × Gross Margin %); target < 18 luni pentru SaaS B2B. Red flag: CAC în creștere sau LTV în scădere (semnalizează saturare piață sau deteriorare produs).

### Pas 4: Evaluarea echipei — Founder-Market Fit
Cel mai important factor în early-stage investing. Evaluează: (1) **Domain expertise**: fondatorul a trăit problema pe care o rezolvă (ex-insider în industrie vs outsider)? (2) **Track record**: au mai construit ceva înainte (companie, produs, comunitate)? (3) **Technical depth**: pot executa produsul sau depind complet de un CTO de angajat? (4) **Commercial skills**: pot vinde? (5) **Team completeness**: CEO/CTO/CPO sau lipsesc roluri critice? (6) **Cohesiveness**: cofounder tension? vesting schedule cu cliff? (7) **References**: vorbește cu 3 foști colegi sau clienți despre fondatori.

### Pas 5: Evaluarea moat-ului competitiv
Identifică sursa de apărare pe termen lung: **Network Effects** (valoarea crește cu fiecare nou utilizator — marketplace, social): cel mai puternic moat; **Switching Costs** (costul schimbării furnizorului este mare — ERP, data integration): apără conturile existente; **Data/IP proprietary**: dacă datele acumulate devin avantaj de ML inimitabil; **Regulatory/licensing**: bariere de intrare create de regulatori; **Brand/distribution**: brand recunoscut sau canal de distribuție unic. Dacă nu există niciun moat clar, business-ul este vulnerabil la commoditization.

### Pas 6: Sanity check-ul proiecțiilor financiare
Evalueaza realismul: **Rata de creștere implicită**: 3× ARR în an 1, 2.5× în an 2, 2× în an 3 = agresiv dar posibil pentru top-quartile SaaS; 10× YoY = nerealist fără mecanisme explicite; **Bottom-up vs top-down**: proiecțiile construite bottom-up (X sales reps × Y deals/rep × Z ACV) sunt mai credibile decât "capturăm 1% din piata de $10B"; **Burn rate și runway**: verifică lunar burn față de fundraising; dacă runway < 12 luni post-investment, compania va intra în fundraising din nou înainte de a demonstra traction; **Revenue recognition**: ARR vs MRR vs Cash Collected — asigură că compari mere cu mere.

### Pas 7: Analiza term sheet — valorizare/dilution/liquidation preference
Termenii cheie de negociat: **Pre-money valuation**: ce valorizare implică runda? verifică față de comparable companies (ARR multiple: seed 5-15x, Series A 8-20x ARR); **Dilution**: câte % din companie cedezi? (seed: 15-25%, Series A: 20-30%); **Liquidation preference**: 1x non-participating = standard, >1x sau participating = antreprenor-defavorabil (VCs recuperează mai întâi); **Anti-dilution provisions**: broad-based weighted average = ok, full ratchet = periculos; **Board composition**: VCs cer locuri în board — câte? veto rights? **Pro-rata rights**: dreptul de a investi în runde viitoare pentru a menține procent. Consultă un avocat specializat în VC înainte de semnătură.

---

## 3. Cortex Logging

```json
{
  "text": "Venture Capital Evaluation F-094 executat: pitch deck analizat, TAM/SAM/SOM validat, unit economics CAC/LTV calculate, echipa evaluata, moat identificat, proiectii sanity-checked, term sheet analizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-094",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["venture-capital", "startup-evaluation", "due-diligence", "unit-economics", "LTV-CAC", "term-sheet", "angel-investing"],
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
- Evaluare VC realizată fără validarea traction metrics (ARR, churn, NRR) → violation
- Term sheet analizat fără compararea cu benchmarkuri de piață (Crunchbase/PitchBook) → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-094 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Venture Capital / Private Investment Evaluation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Pitch deck și data room | Materiale de analiză primită de la startup |
| Industry reports (Gartner/IDC) | Validarea independentă a dimensiunii pieței |
| VC lawyer / legal counsel | Revizuirea term sheet-ului |
| Reference calls (clienți/foști angajați) | Validarea echipei și produsului |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| LTV:CAC ratio minim acceptabil | > 3:1 |
| Payback period maxim | < 18 luni |
| Runway post-investment | > 18 luni |
