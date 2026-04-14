---
id: B2B-009
title: Deal Pipeline Management and Forecasting
domain: b2b-sales
source: AI Cold Emails Lead Generation Sales Masterclass 2024
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Deal Pipeline Management and Forecasting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Sistem de management al pipeline-ului de deals și forecasting de revenue cu acuratețe ≥80%, bazat pe MEDDIC stage validation, weighted probability calculations, și pipeline hygiene disciplinată.

---

## 1. Problema

Pipeline reviews subiective, cu deals stagnante și valori estimate fără bază metodologică duc la forecasting inexact (<50% acuratețe) și decizii de business eronate. Un pipeline sănătos necesită hygiene săptămânală, probabilități bazate pe date reale (nu gut feel), și MEDDIC validation pe fiecare deal.

Situații acoperite:
- Pipeline review săptămânal al echipei de sales
- Forecasting lunar/trimestrial de revenue pentru management
- Identificarea deals la risc și intervenție proactivă
- Agency forecasting MRR (monthly recurring revenue) growth

---

## 2. Procedura

### Pas 1: Calcularea stage conversion rates pe date istorice
Nu folosi probabilitățile default din CRM — sunt generice. Calculează-le pe datele tale:
1. Exportă din CRM date pe ultimele 6-12 luni: câte deals au intrat în fiecare stagiu
2. Calculează conversion rate per stagiu:

| Tranziție | Formula | Exemplu tipic agency |
|-----------|---------|---------------------|
| Lead → Engaged | Engaged / Total Leads | 15-25% |
| Engaged → Discovery | Discovery / Engaged | 40-60% |
| Discovery → Qualified | Qualified / Discovery | 50-70% |
| Qualified → Proposal | Proposal / Qualified | 60-80% |
| Proposal → Negotiation | Negotiation / Proposal | 40-60% |
| Negotiation → Won | Won / Negotiation | 60-80% |
| **Overall Lead → Won** | Won / Total Leads | **2-5%** |

3. Actualizează aceste rate TRIMESTRIAL — piața și echipa se schimbă
4. Dacă ai sub 30 deals istorice: folosește benchmark-urile din tabel, dar marchează ca "estimated"

### Pas 2: Formula de forecast weighted pipeline
Forecast lunar = suma probabilităților ponderate per deal:

```
Forecast = Σ (Deal Value × Stage Probability × MEDDIC Confidence Modifier)
```

- **Deal Value**: Valoarea deal-ului (MRR × durata contract sau one-time)
- **Stage Probability**: Bazată pe conversion rates istorice (Pas 1), NU pe estimarea rep-ului
- **MEDDIC Confidence Modifier**: Factor de ajustare bazat pe cât de complet e MEDDIC-ul:
  - 6/6 MEDDIC elements confirmed = 1.0× (full probability)
  - 4-5/6 = 0.8×
  - 2-3/6 = 0.5×
  - 0-1/6 = 0.2×

Exemplu: Deal $5,000/mo la Proposal stage (40% probability), MEDDIC 4/6 (0.8×):
Forecast contribution = $5,000 × 0.40 × 0.80 = $1,600

### Pas 3: Pipeline hygiene săptămânală (LUNI, 30 minute)
Audit obligatoriu al TUTUROR deals active, fiecare luni dimineață:

| Check | Acțiune |
|-------|---------|
| Deals fără activitate >14 zile | Marchează "AT RISK" → task urgent follow-up |
| Deals cu close date în trecut | Actualizează data realistă SAU mută la Closed Lost |
| Deals fără next action date | Adaugă IMEDIAT sau disqualifică |
| Deals cu sumă $0 sau nedefinită | Completează estimare sau elimină din pipeline |
| Deals cu MEDDIC <3/6 peste 30 zile | Re-califică sau disqualifică |
| Deals cu close date >90 zile în viitor | Review: realist sau "parking lot"? |

**Regula 3x pipeline**: Pipeline total trebuie să fie ≥3× target-ul de revenue lunar. Dacă target = $50K/mo, pipeline activ ≥ $150K. Sub 3×: activează imediat prospecting suplimentar.

### Pas 4: Deal review săptămânal (1:1 manager-rep, 20 minute)
Format structurat, nu freestyle:
- Rep prezintă **top 5 deals** active cu:
  - Status MEDDIC (câte elemente confirmate din 6)
  - Next action și dată
  - Probabilitate subiectivă de închidere luna aceasta
  - Blocaje/obiecții nerezolvate
- Manager pune **MEDDIC challenge questions**:
  - "Ai vorbit direct cu economic buyer-ul sau doar cu champion-ul?"
  - "Ce ar putea face ca acest deal să NU se închidă?"
  - "Care e cea mai mare obiecție nerezolvată?"
  - "Ce a zis competitorul care este menționat?"
  - "Dacă pui mâna pe piept — se închide luna asta realist?"
- Documentează concluziile în CRM pe fiecare deal

### Pas 5: Categorii de forecast (Commit / Best Case / Pipeline)
Clasifică fiecare deal în una din 3 categorii:

| Categorie | Criterii | Weight în forecast |
|-----------|---------|-------------------|
| **Commit** | MEDDIC 6/6, verbal agreement, next steps concrete, close date luna aceasta | 100% |
| **Best Case** | MEDDIC 4-5/6, urmărire activă, probabilitate 50-84% | 50% |
| **Pipeline** | MEDDIC <4/6, incert, probabilitate <50% | 15% |

**Forecast lunar** = Commit 100% + Best Case 50% + Pipeline 15%

Model conservator — dacă business-ul tău outperform-ează consistent cu +20%, ajustează. Dar NICIODATĂ nu forecasta optimist pe deals individuale — optimismul agregat peste 20 deals produce forecast dezastruos.

### Pas 6: Identificarea și salvarea deals la risc
Deal la risc = orice deal cu 2+ red flags:
- Același stagiu >21 zile fără activitate
- Champion nu mai răspunde
- Deal value redus nejustificat (scope creep downward)
- Competitor menționat recent fără contra-argumentare documentată
- Close date amânată de 2+ ori
- Economic buyer nu a fost contactat direct

**Protocol de salvare**:
1. Rep escaladează la manager cu context
2. Analiză împreună: schimbare unghi, ofertă alternativă, executive-to-executive call, acceptare pierdere
3. Acțiune concretă în 48h — nu lăsa deals la risc să treneze
4. Dacă 3 intervening actions fără progres → Closed Lost (nu ține în pipeline artificial)

### Pas 7: Raportarea și velocity metrics
**Raport săptămânal** (1 pagină, generat luni):
- Total pipeline value (vs. săptămâna precedentă)
- Forecast luna curentă (Commit + Best Case + Pipeline)
- Deals nou intrate în pipeline (număr + value)
- Deals pierdute (număr + motive top 3)
- Deals câștigate (număr + value + average deal size)
- Deals la risc (listă cu acțiuni planificate)
- Pipeline health: 3× coverage? ✅/❌

**Velocity metrics** (trimestrial):
- **Average deal size**: Target monitorizat — creștere trimestrială
- **Win rate**: Deals Won / (Won + Lost). Target agency: 25-40%
- **Sales cycle length**: Zile medii de la Lead la Closed Won. Target agency: 21-45 zile
- **Pipeline velocity** = (# Deals × Win Rate × Avg Deal Size) / Sales Cycle Length = Revenue/period

Dacă forecast-ul este sub target cu >20%: activează IMEDIAT protocol de urgență — nu aștepta end-of-month.

---

## 3. Cortex Logging

```json
{
  "text": "Pipeline management MEDDIC-weighted activ: stage conversion rates calculate pe date reale, forecast ponderat cu MEDDIC modifier, hygiene săptămânală executată, deals la risc identificate și adresate, velocity metrics tracked trimestrial.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-009",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["pipeline", "forecasting", "deals", "b2b-sales", "revenue", "crm", "meddic", "velocity"],
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

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Forecast bazat pe probabilități generice CRM (nu istorice) → violation
- Pipeline hygiene omisă >2 săptămâni → violation
- Deals fără close date sau next action acceptate în pipeline → violation
- Deals la risc neescaladate >7 zile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum b2b-sales

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Stage conversion rates calculate pe date reale?
- [ ] Pipeline hygiene executată în ultima săptămână?
- [ ] Pipeline coverage ≥3× target?
- [ ] Velocity metrics tracked trimestrial?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-009 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Deal Pipeline Management and Forecasting" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HubSpot / Salesforce / Pipedrive | Pipeline management și rapoarte |
| Google Sheets | Model de forecast ponderat MEDDIC |
| Slack / Email | Raportare săptămânală management |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Acuratețe forecast vs. actuals | ≥80% |
| Deals fără next action date | 0 |
| Pipeline hygiene executată | 100% săptămâni |
| Deals stagnate >21 zile | <15% din pipeline |
| Pipeline coverage (vs. target) | ≥3× |
| Win rate | 25-40% |
| Sales cycle length | 21-45 zile |
| Stage conversion rates actualizate | trimestrial |
