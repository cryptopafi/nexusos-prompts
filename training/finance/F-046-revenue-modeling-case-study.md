---
type: procedure
created: 2026-03-22
status: active
slug: f-046-revenue-modeling-case-study
tags: [procedure, nexus]
---

# Revenue Modeling Case Study (Shark VR / IPO) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui model detaliat de revenue pentru o companie tech/gaming în pregătire de IPO, folosind unit economics și cohort analysis.

---

## 1. Problema

Modelarea revenue-ului pentru o companie tech cu model de abonament sau freemium necesită o abordare fundamentală diferită față de companiile tradiționale. Această procedură (case study Shark VR) acoperă situațiile de modelare pre-IPO, pitch deck pentru investitori, sau due diligence unde trebuie să construiești un model de revenue bazat pe user metrics, cohorts, și TAM penetration.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Înțelegere business și stream-uri de revenue
Citește materialele disponibile despre companie (S-1 draft, pitch deck, management presentations). Identifică toate stream-urile de revenue: subscriptions (lunar/anual), one-time purchases, licensing, advertising, în-app purchases, B2B partnerships. Înțelege modelul de pricing și pachetele existente. Documentează care stream este primary și care sunt secundare.

### Pas 2: Definire unit economics
Calculează metricile fundamentale per user: ARPU (Average Revenue Per User = Revenue / Total Active Users), ARPE (Average Revenue Per Employee — pentru scalabilitate), Customer Acquisition Cost (CAC), LTV (Lifetime Value = ARPU × Gross Margin % / Churn Rate). Calculează LTV/CAC ratio — target >3x pentru SaaS/subscription. Documentează payback period al CAC.

### Pas 3: Construire cohort model pentru subscriber growth
Modelează utilizatorii pe cohorts (luna sau trimestrul în care s-au înscris). Pentru fiecare cohort: număr de utilizatori noi, retention rate lunară (% care rămân după luna 1, 2, 3...), revenue per utilizator activ. Calculează Monthly Active Users (MAU) ca sum of all active cohorts in any given month. Vizualizează churn și retention în grafice cohort (logo retention și dollar retention distinct).

### Pas 4: Ajustări pentru sezonalitate
Identifică pattern-ul sezonier din datele istorice (dacă există): gaming tinde să aibă peak în Q4 (holiday season). Calculează indicii sezonali lunari (% față de medie anuală). Aplică indicii sezonali la forecast-ul de utilizatori noi și la revenue per utilizator. Ajustează modelul de cohort pentru sezonalitate — nu presupune creștere lineară.

### Pas 5: Calculare TAM și penetration curve
Definește TAM (Total Addressable Market): utilizatori globali de VR × % interesați de gaming competitiv × ARPU anual. Definește SAM (Serviceable Addressable Market): piețe geografice adresabile în orizontul de forecast. Construiește curba de penetrare (S-curve tipică pentru adoption): slow start → accelerare → platou. Compară penetration rate asumat cu benchmark-uri din industrie (comparabile).

### Pas 6: Modelare scenarii de creștere
Scenariu Conservative: retention rate scăzut (industry average), penetrare TAM lentă, fără noi revenue streams în primii 2 ani. Scenariu Base: retention rate mediu, penetrare moderată, un nou revenue stream (ex: enterprise/B2B) după 18 luni. Scenariu Aggressive: retention superioară industriei (product moat), penetrare accelerată, multiple revenue streams. Calculează revenue 5 ani pentru fiecare scenariu.

### Pas 7: Validare cu companii comparabile
Identifică 3-5 companii comparabile (Roblox, Unity, Meta Quest pentru gaming VR; sau SaaS benchmark pentru subscription metrics). Compară: creșterea YoY la stadii similare de maturitate, ARPU, retention/churn, LTV/CAC. Dacă ipotezele din model sunt semnificativ mai bune decât comparabilele — justifică sau ajustează. Prezintă bridgeul față de comparabile în pitch deck.

---

## 3. Cortex Logging

```json
{
  "text": "Revenue Modeling Case Study (Shark VR / IPO) executat: unit economics definite, cohort model construit, TAM și penetration curve calculate, 3 scenarii modelate, validat vs. comparabile",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-046",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["revenue-modeling", "cohort-analysis", "unit-economics", "IPO", "SaaS", "valuation", "advanced"],
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
- Model fără cohort analysis și retention curves → violation
- Ipoteze de creștere nevalidate față de comparabile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-047 → IPO prospectus analysis (complementar)

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-046 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Revenue Modeling Case Study (Shark VR / IPO)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| S-1 draft / pitch deck Shark VR | Sursă date pentru model |
| Comparable company data (Roblox/Unity/Meta Quest) | Benchmark validare |
| Excel / Google Sheets | Construire cohort model |
| Market research (Newzoo, IDC pentru VR market) | Date TAM |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| LTV/CAC ratio | > 3x în modelul Base |
| Scenarii construite | 3 (Conservative/Base/Aggressive) |
| Companii comparabile validate | Minimum 3 |
