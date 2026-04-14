---
id: B2B-007
title: Lead Scoring and Qualification Matrix
domain: b2b-sales
source: AI Cold Emails Lead Generation Sales Masterclass 2024
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Lead Scoring and Qualification Matrix — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Sistem de scoring și calificare a leads B2B care combină MEDDIC qualification cu scoring cantitativ Fit + Engagement, pentru prioritizarea timpului de sales pe oportunități cu probabilitate maximă de închidere. Include descalificarea rapidă și MEDDIC kill signals.

---

## 1. Problema

Fără un sistem de scoring, sales reps petrec timp egal pe leads cu potențial ridicat și scăzut. Rezultatul: pipeline blocat cu oportunități slabe care consumă resurse, în timp ce leads calde sunt urmărite cu întârziere. Benchmark: echipele cu lead scoring au 77% mai mult ROI pe lead generation vs. cele fără (MarketingSherpa).

Situații acoperite:
- Prioritizarea unui volum mare de leads inbound sau outbound
- Decizia de a investi timp în discovery vs. descalificare rapidă
- Alocarea leads între reps în funcție de calitate și urgency
- Agency: scoring leads care vin din cold email vs. LinkedIn vs. referral vs. inbound

---

## 2. Procedura

### Pas 1: Definirea dimensiunilor de scoring (Fit + Engagement)
Creează o matrice cu două dimensiuni independente:

**A) Fit Score (0-100)** — cât de bine se potrivește prospectul cu ICP:
| Criteriu | Pondere | Scoring |
|----------|---------|---------|
| Industrie | 25 pct | ICP perfect = 25, adiacent = 15, outside = 5 |
| Dimensiune companie (employees) | 20 pct | Sweet spot = 20, aproape = 12, too small/big = 5 |
| Titlu decident | 25 pct | Economic buyer = 25, influencer = 15, end user = 5 |
| Locație/Geografie | 10 pct | Piață primară = 10, secundară = 6, outside = 2 |
| Budget indicator | 20 pct | Budget confirmat = 20, probabil = 12, unknown = 5 |

**B) Engagement Score (0-100)** — cât de interesat pare:
| Acțiune | Puncte |
|---------|--------|
| Deschis email | +5 |
| Reply la email (pozitiv) | +25 |
| Click pe link din email | +10 |
| Vizitat website (pricing page) | +20 |
| Completat formular | +30 |
| Accepted LinkedIn connection | +10 |
| Responded pe LinkedIn | +20 |
| Requested demo/call | +40 |
| Attended webinar | +15 |
| Downloaded resource | +10 |

### Pas 2: Calibrarea scorurilor cu date istorice
NU stabili ponderile arbitrar — calibrează cu date reale:
1. Exportă din CRM ultimele 30-50 deals (Won + Lost + Stagnant)
2. Pentru fiecare deal: calculează retrospectiv Fit Score-ul la momentul intrării
3. Calculează media Fit Score pentru Closed Won vs. Closed Lost
4. Ajustează ponderile pentru a MAXIMIZA diferența între Won și Lost
5. Dacă nu ai date istorice (startup/new program): pornește cu ponderile de mai sus, revizuiește OBLIGATORIU după primele 30 deals
6. **Re-calibrare trimestrială**: Repetă analiza cu date noi. Dacă corelația Fit Score → Win Rate scade sub 50%: ponderile sunt stale

### Pas 3: Matricea de calificare 2x2 și acțiuni per cadran
Plasează fiecare lead pe matricea Fit (axa Y) × Engagement (axa X):

| | Engagement SCĂZUT (<40) | Engagement RIDICAT (≥40) |
|---|---|---|
| **Fit RIDICAT (≥60)** | **NURTURE** — Secvență automată de educare. Potențial mare dar încă nu interesat. Timeline: convert în 30-90 zile | **PRIORITY** — Contactează în <24h. Meeting ASAP. Cel mai bun prospect posibil |
| **Fit SCĂZUT (<60)** | **DEPRIORITIZE** — Nu aloca timp de sales. Auto-nurture sau discard | **INVESTIGATE** — Engagement mare dar fit incert. Califică rapid pe un 5-min call. Poate fi fit mai bun decât pare (titlu greșit, companie în tranziție) |

### Pas 4: Implementarea în CRM (tool-specific)
- **HubSpot Pro+ ($500/mo)**: Lead Scoring nativ — Settings → Properties → Lead Score. Setează positive attributes (Fit) + activity-based scoring (Engagement). Creează Smart Lists per cadran
- **HubSpot Free/Starter**: Câmpuri custom numerice "Fit Score" + "Engagement Score" calculate manual sau via Zapier. Create views sorted by score
- **Pipedrive**: Câmpuri custom + Automations pentru alert la score threshold. Nu are scoring nativ — implementează cu câmpuri formula
- **Close.com**: Smart Views filtrate pe custom fields. Lead status mapping pe cadranele 2x2
- **Actualizare scor**: Engagement Score se actualizează automat la fiecare interacțiune (email open, click, website visit). Fit Score se setează la calificare inițială și se ajustează doar la informații noi

### Pas 5: Definirea pragurilor de acțiune (SLA intern)
Praguri clare, agreate cu echipa:
| Scor compozit (Fit × 0.6 + Engagement × 0.4) | Acțiune | SLA |
|---|----|---|
| ≥80 | Rep alocă timp IMEDIAT, highest priority | Contact în aceeași zi |
| 60-79 | Urmărire activă | Contact în 48h |
| 40-59 | Sequență automată de nurture | Automated, check manual monthly |
| <40 | Arhivare sau newsletter only | No sales time |

### Pas 6: MEDDIC Kill Signals — descalificarea rapidă
Criterii care descalifică IMEDIAT indiferent de Fit + Engagement Score:
- **No Budget + No Timeline**: Budget confirmat inexistent ȘI fără urgency în 12 luni → DISQUALIFY
- **No Authority Access**: Nu există acces la economic buyer și nici champion intern → DISQUALIFY (vei vinde ceva ce nu poate fi aprobat)
- **Excluded Industry/Type**: Business type pe lista de excludere (ex: agency nu lucrează cu gambling, crypto scams, etc.)
- **Competitor Lock-in**: Contract activ cu competitor pe 12+ luni fără exit clause → NURTURE long-term, nu active pursuit
- **Below Minimum Deal Size**: Deal value sub minimul profitabil ($X definit per business) → DISQUALIFY sau redirect la self-serve
- **Ghost After Discovery**: Prospect fully qualified dar dispărut complet post-discovery (3+ follow-ups fără răspuns) → mută în NURTURE, nu forța

Regula: Un prospect descalificat rapid eliberează 2-5 ore/săptămână per rep pentru prospects calificați. Descalificarea NU e un eșec — e o decizie inteligentă.

### Pas 7: Revizuirea lunară a matricei de scoring
Lunar (30 minute, prima zi a lunii):
1. **Corelație analysis**: % Closed Won pe fiecare bandă de scor. Target: ≥60% din Won vin din banda ≥80
2. **False positives**: Leads cu scor ≥80 care au fost Closed Lost — de ce? Ajustează criteriile
3. **False negatives**: Deals Won care aveau scor <60 la intrare — ce am ratat? Adaugă trigger events noi
4. **Engagement triggers noi**: Adaugă surse de engagement noi (webinar attendance, community participation, social media engagement cu brand)
5. **Documentare**: Versionează fiecare modificare de scoring matrix cu data și raționalul

---

## 3. Cortex Logging

```json
{
  "text": "Lead scoring matrix MEDDIC-aligned implementată: Fit (5 criterii ponderate) + Engagement (10 acțiuni), matrice 2x2 cu acțiuni per cadran, praguri SLA definite, kill signals MEDDIC active, calibrare cu date istorice.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-007",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["lead-scoring", "qualification", "icp", "crm", "b2b-sales", "meddic", "prioritization"],
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
- Scoruri stabilite fără calibrare cu date istorice (sau plan de calibrare) → violation
- Praguri de acțiune nedefinite → violation
- Revizuire lunară omisă → violation
- Leads cu kill signals neadresate (rămân active în pipeline) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum b2b-sales

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Scoring calibrat cu date istorice (sau plan post-30 deals)?
- [ ] Praguri SLA documentate și agreate cu echipa?
- [ ] Kill signals MEDDIC definite și active?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-007 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Lead Scoring and Qualification Matrix" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HubSpot Pro ($500/mo) | Lead scoring nativ |
| Pipedrive + câmpuri custom | Scoring manual sau semi-automat |
| Close.com + Smart Views | Scoring via custom fields |
| Clay / Apollo | Enrichment date pentru Fit Score |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Corelație scor ≥80 → Closed Won | ≥60% |
| Timp mediu de urmărire lead scor ≥80 | <24h (SLA) |
| % leads descalificate rapid (kill signals) | monitorizat |
| Revizuire lunară scoring executată | 100% |
| % reps utilizează scoring în prioritizare | 100% |
| False positive rate (scor ≥80 → Lost) | <30% |
| Re-calibrare trimestrială | executată |
