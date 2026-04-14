---
type: procedure
created: 2026-03-20
status: active
slug: m-128-ltv-cohort-analysis
tags: [procedure, nexus]
---

# M-128 — Player LTV Cohort Analysis & Rev-Share Model Calibration

**Domeniu**: affiliate | **Sub-domeniu**: ltv-cohort-analysis
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Conectat la**: M-115 (Offer Discovery + EV), M-118 (Deal Negotiation), M-114 (GEO Scoring), M-121 (S2S Tracking)

---

## §1 — PROBLEMA

Modelul actual de evaluare a ofertelor (M-115) folosește asumpții statice preluate din medii de industrie:
- 2–4 depozite/lună per jucător activ
- Fereastră LTV de 6 luni standard
- Varianta reală: ±40–50% față de medie, în funcție de GEO și operator

**Riscul concret**: dacă rata de churn reală din Kenya este mai mare decât cea asumată, RevShare generează mai puțin decât CPA pe același interval — alegem modelul de plată greșit pentru 12–24 luni de campanie.

**Alte consecințe ale lipsei de validare**:
- Calitatea deciziei degradează în timp pe măsură ce piețele maturizează și comportamentul jucătorilor se schimbă
- Nu există mecanism de feedback din date reale spre M-115 și M-118
- Nu putem negocia RevShare îmbunătățit chiar dacă avem jucători de calitate dovedită

**Cronologie minimă de validare**:
- 30 zile: prime date disponibile per cohortă
- 90 zile: semnal LTV semnificativ (primul punct de calibrare)
- 180 zile: model complet (baza pentru decizia CPA vs RevShare)

---

## §2 — PROCEDURA

### Pas 1: DEFINIREA COHORTELOR & SETUP TRACKING

**Structura cohortei**:
O cohortă = toți jucătorii FTD (First Time Depositors) achiziționați într-o lună calendaristică specifică, per GEO, per operator.

**Date obligatorii per FTD**:
- `click_id` — din Voluum (S2S postback, M-121)
- `registration_date` — data înregistrării în platforma operatorului
- `first_deposit_date` — data primului depozit
- `first_deposit_amount` — suma primului depozit (valuta locală + USD echivalent)
- `source_channel` — TikTok / native / WhatsApp / SEO organic
- `GEO` — codul de țară ISO (PE, KE, CL)

**Surse de date**:
- Voluum: date click + conversie (postback S2S configurat în M-121)
- Dashboard operator: istoric depozite per jucător (export lunar)
- Reconciliere M-121: delta acceptabil <5% între Voluum și operator

**Convenție de denumire cohortă**:
`{GEO}-{operator}-{YYYY-MM}`
Exemplu: `KE-MarathonBet-2026-03`

**Setup Google Sheets per cohortă**:
Creează un fișier `cohort-tracking-{GEO}-{operator}.gsheet` cu structura:

| Snapshot | Active Players | Active % | Total Deposits | Avg Deposit | Dep/Player | Churn Rate | Cum. Revenue | Cum. RevShare |
|----------|----------------|----------|----------------|-------------|------------|------------|--------------|---------------|
| Month 1  |                |          |                |             |            |            |              |               |
| Month 2  |                |          |                |             |            |            |              |               |
| ...      |                |          |                |             |            |            |              |               |
| Month 12 |                |          |                |             |            |            |              |               |

**Output**: `cohort-tracking-{GEO}-{operator}.gsheet` — Google Sheets cu definiții cohortă + template snapshot lunar
**Checkpoint**: sheet de tracking creat pentru fiecare ofertă activă cu model RevShare; zero campanii RevShare fără tracking cohortă

---

### Pas 2: ACTUALIZARE LUNARĂ COHORTĂ

Prima zi a fiecărei luni: actualizează toate sheet-urile active cu datele din luna anterioară.

**Workflow actualizare**:
1. Loghează-te în dashboard-ul de afiliat al operatorului
2. Exportă raportul de performanță per jucător pentru luna precedentă
3. Filtrează per cohortă (luna de achiziție FTD)
4. Calculează și înregistrează per cohortă:
   - **Active%** = jucători cu cel puțin un depozit în luna curentă / total cohortă
   - **Avg Deposits** = total depozite / jucători activi
   - **Avg Deposit Amount** = suma totală / total depozite
   - **Churn Rate** = jucători cu zero activitate luna curentă față de luna precedentă

**Formula venit lunar**:
```
Monthly Revenue = Active% × Cohort Size × Avg Deposits × Avg Deposit Amount × RevShare%
```

**Cumulativ**: running total al tuturor lunilor de la startul cohortei.

**Output**: sheet-uri actualizate + `cohort-monthly-summary-{YYYY-MM}.md`

Format tabel rezumat:

| Cohortă | Vârstă (luni) | Cohortă Size | Active % | Rev Luna | Rev Cumul | M-115 Predicție | Delta % |
|---------|---------------|--------------|----------|----------|-----------|-----------------|---------|
|         |               |              |          |          |           |                 |         |

**Checkpoint**: toate sheet-urile actualizate până pe data de 5 a fiecărei luni; zero cohorte cu gaps în datele lunare

---

### Pas 3: FITTING CURBA LTV

La **90 de zile** (primul punct de calibrare semnificativ) și la **180 de zile** (model complet):

**Pas 3.1: Plotează curba de retenție**
Calculează % din cohortă încă activă la lunile: 1, 2, 3, 6, 9, 12.

**Pas 3.2: Fit exponential decay**
Aplică modelul de declin exponențial:
```
Retention(t) = R0 × e^(-λt)
```
Unde:
- `R0` = retenție inițială (% activi luna 1)
- `λ` = rata de churn (se estimează prin regresie pe datele disponibile)
- `t` = luna (1, 2, 3, ...)

**Pas 3.3: Calculează LTV 6 luni**
```
LTV_6mo = Σ [Monthly Revenue(t) × Retention(t)] pentru t=1..6
```

**Pas 3.4: Compară cu M-115**
Calculează delta față de predicția M-115:
```
Delta% = (Actual LTV - M-115 Predicted LTV) / M-115 Predicted LTV × 100
```

Dacă `|Delta%| > 20%` → **declanșează recalibrare M-115** (Pas 4).

**Template curba LTV**:

| Luna | Cohortă Size | Active % | Avg Deposits | Avg Amount | Rev Lunar | Rev Cumulativ | M-115 Predicție | Delta % |
|------|-------------|----------|--------------|------------|-----------|---------------|-----------------|---------|
| 1    |             |          |              |            |           |               |                 |         |
| 2    |             |          |              |            |           |               |                 |         |
| 3    |             |          |              |            |           |               |                 |         |
| 6    |             |          |              |            |           |               |                 |         |
| 12   |             |          |              |            |           |               |                 |         |

**Output**: `ltv-model-{GEO}-{operator}-{cohort_month}.md` — curba fittată + parametri calibrați + comparație M-115
**Checkpoint**: model LTV fitted la 90 zile; delta vs M-115 calculat; dacă delta >20%, recalibrare M-115 declanșată

---

### Pas 4: RECALIBRARE MODEL DE PLATĂ

Dacă LTV actual deviază >20% față de proiecția M-115:

**Pas 4.1: Recalculează pragul CPA vs RevShare**
Regula actuală din M-115: LTV > 4× CPA = RevShare; 2–4× = Hybrid; <2× = CPA pura.

Cu date reale, recalculează pragurile:
- Dacă LTV real este mai mic → pragul CPA urcă (CPA devine mai atrăgătoare relativ)
- Dacă LTV real este mai mare → RevShare devine mai atrăgătoare

**Pas 4.2: Actualizează inputurile din M-115 Pas 5**
Înlocuiește în formula EV din M-115:
- `λ_assumed` → `λ_actual` (rata de churn calibrată)
- `avg_deposit_assumed` → `avg_deposit_actual`
- `deposit_frequency_assumed` → `deposit_frequency_actual`

**Pas 4.3: Documentează recalibrarea în Cortex**
Salvează cu: cohort ID, asumpție originală, date reale, parametru nou.

**Matricea deciziilor actualizată**:

| GEO | Operator | LTV Asumpție M-115 | LTV Actual (6mo) | Delta | Model Recomandat | Actualizat La |
|-----|----------|-------------------|------------------|-------|------------------|---------------|
|     |          |                   |                  |       |                  |               |

**Output**: `payout-model-update-{GEO}-{YYYY-MM-DD}.md` — parametri actualizați + rațional
**Checkpoint**: asumpțiile M-115 actualizate în max 30 zile de la atingerea marcajului de 180 zile al cohortei; zero oferte RevShare rulând >6 luni fără validare LTV

---

### Pas 5: BENCHMARKING CROSS-GEO

După 3+ cohorte per GEO (9+ luni de operare):

**Pas 5.1: Compară curbele de retenție între GEO-uri**
- Este LTV Kenya semnificativ diferit de Peru?
- Care GEO are jucătorii de cea mai înaltă calitate? (LTV/FTD cel mai mare)

**Pas 5.2: Alimentează M-114 GEO Scoring**
- Actualizează componenta "Market Sizing" din M-114 cu date reale de calitate jucător
- Înlocuiește scoring-ul bazat pe estimări cu date observate

**Pas 5.3: Analiză pattern sezonier**
- Sunt cohortele din ianuarie mai bune decât cele din iulie?
- Calitatea bugetului publicitar afectează calitatea jucătorilor → feedback spre planificarea campaniilor

**Output**: `ltv-benchmark-{YYYY-MM-DD}.md` — tabel comparativ LTV cross-GEO + analiză pattern sezonier
**Checkpoint**: comparație cross-GEO trimestrială generată; M-114 GEO scoring actualizat cu date LTV reale anual

---

### Pas 6: TRIGGER NEGOCIERE OPERATOR

Dacă o cohortă demonstrează LTV >6× pragul CPA (calitate excepțională):

**Pas 6.1: Declanșează M-118 Pas 6 (Escaladare Deal Direct)**
Pregătește pachetul de evidențe pentru renegocierea condițiilor:
- Curba de retenție a cohortei vs medie industrie
- Grafic venituri cumulative cu proiecție
- Comparație λ (rata churn) vs benchmark operator

**Pas 6.2: Argumentul de negociere**
```
"Jucătorii noștri din {GEO} arată X% retenție la Luna 6 față de media industrie Y%.
LTV per FTD este Z× mai mare decât CPA curent.
Solicităm RevShare de la {current}% la {target}% sau structură hibridă CPA+RevShare."
```

**Pas 6.3: Documentează și urmărește**
Înregistrează: data declanșării, cerința, răspunsul operatorului, deal rezultant.

**Output**: `negotiation-trigger-{GEO}-{operator}-{YYYY-MM-DD}.md` — pachet dovezi pentru escaladare deal
**Checkpoint**: negocierea declanșată în max 60 zile de la confirmarea cohortei excepționale; zero cohorte excepționale ratate

---

## §3 — PARAMETRI MODEL LTV (Stare Curentă → De Calibrat)

| GEO | Operator | λ Asumată (churn/lună) | Avg Deposit Asumat | Frecvență Asumată | LTV 6mo Asumat | Status |
|-----|----------|----------------------|--------------------|--------------------|----------------|--------|
| PE (Peru) | BetSafe | 0.25 (est.) | $45 USD | 2.5 dep/lună | $270 | Nevalidat |
| KE (Kenya) | MarathonBet | 0.30 (est.) | $18 USD | 3.0 dep/lună | $130 | Nevalidat |
| CL (Chile) | Codere | 0.20 (est.) | $60 USD | 2.0 dep/lună | $360 | Nevalidat |

**Notă**: Toate valorile sunt estimări de industrie. Statusul se schimbă în "Validat" după atingerea marcajului de 180 zile cu delta <20%.

---

## §4 — CORTEX KNOWLEDGE BASE

```json
{
  "procedure_id": "M-128",
  "title": "Player LTV Cohort Analysis & Rev-Share Model Calibration",
  "domain": "affiliate",
  "sub_domain": "ltv-cohort-analysis",
  "version": "1.0",
  "status": "active",
  "connects_to": ["M-115", "M-118", "M-114", "M-121"],
  "output_files": [
    "cohort-tracking-{GEO}-{operator}.gsheet",
    "cohort-monthly-summary-{YYYY-MM}.md",
    "ltv-model-{GEO}-{operator}-{cohort_month}.md",
    "payout-model-update-{GEO}-{YYYY-MM-DD}.md",
    "ltv-benchmark-{YYYY-MM-DD}.md",
    "negotiation-trigger-{GEO}-{operator}-{YYYY-MM-DD}.md"
  ],
  "key_parameters": {
    "ltv_accuracy_target": "±20% la 6 luni față de date reale",
    "recalibration_trigger": "Delta >20% față de M-115 predicție",
    "negotiation_trigger": "LTV >6x CPA threshold",
    "update_frequency": "lunar (Pas 1-2), trimestrial (Pas 5)"
  },
  "tags": ["ltv", "cohort", "revshare", "cpa", "gambling", "kenya", "peru", "chile", "calibration"]
}
```

---

## §5 — CONEXIUNI INTER-PROCEDURI

| Procedură | Tip Conexiune | Detaliu |
|-----------|---------------|---------|
| **M-115** | Output → Input | LTV calibrat actualizează formula EV din M-115 Pas 5 (Payout Model Score) |
| **M-118** | Trigger | LTV excepțional (>6x CPA) declanșează M-118 Pas 6 — renegociere deal |
| **M-114** | Feedback anual | Date calitate jucător per GEO actualizează componenta Market Sizing din scoring |
| **M-121** | Date Sursă | Postback S2S M-121 furnizează click_id și date conversie pentru definirea cohortei |

---

## §6 — METRICI DE SUCCES

| Metric | Target | Metodă Măsurare |
|--------|--------|-----------------|
| Acuratețe LTV la 6 luni | ±20% față de date reale | Delta% în Pas 3 |
| Cohorte cu tracking complet | 100% din ofertele RevShare active | Audit sheet-uri lunar |
| Time-to-recalibration | <30 zile de la 180-day mark | Data actualizare M-115 |
| Negocieri declanșate corect | 100% din cohorte excepționale | Audit trimestrial |
| M-115 actualizat | Cel puțin o dată per GEO per an | Timestamp versiune |

---

## §7 — VIOLĂRI HOW (Interzis)

- **VIOLAȚIE**: Rularea unei campanii RevShare >6 luni fără cohortă de tracking → risc de pierdere financiară nedetectată
- **VIOLAȚIE**: Actualizarea M-115 fără documentarea în Cortex a recalibrării → pierderea trasabilității deciziei
- **VIOLAȚIE**: Folosirea datelor de la o singură cohortă pentru concluzii cross-GEO → eșantionare insuficientă
- **VIOLAȚIE**: Negocierea fără pachet de dovezi cohortă → argument slab, risc respingere

---

---

## §8 — ENFORCEMENT LOOP (META-H-002)

**WHERE**: La activarea oricărei campanii RevShare noi (M-118 Pas 5) + la prima zi a fiecărei luni (actualizare cohortă)

**WHEN**:
- Imediat la lansarea primei campanii RevShare per GEO (cohortă setup obligatorie)
- Luna 1 a fiecărei luni: actualizare snapshot cohortă (Pas 2)
- La 90 zile post-lansare cohortă: fitting curba LTV (Pas 3) — prima dată obligatorie
- La 180 zile post-lansare cohortă: model complet + decizie recalibrare (Pas 4)
- Trimestrial: benchmarking cross-GEO (Pas 5)

**HOW — Violations critice**:
- **VIOLAȚIE CRITICĂ**: Campanie RevShare activă >30 zile fără cohortă de tracking → risc financiar necontrolat
- **VIOLAȚIE CRITICĂ**: Delta LTV >20% față de M-115 fără recalibrare în 30 zile → decizii de buget pe date incorecte
- **VIOLAȚIE**: Cohortă excepțională (LTV >6× CPA) fără trigger negociere M-118 în 60 zile → pierdere de leverage comercial
- **VIOLAȚIE**: Concluzii cross-GEO din date <3 cohorte per GEO → eșantionare insuficientă, concluzie invalidă

**CONNECT**:
- **M-115** (Offer Discovery + EV): LTV calibrat actualizează formula EV din M-115 Pas 5 (Payout Model Score)
- **M-118** (Deal Negotiation): LTV excepțional declanșează M-118 Pas 6 (renegociere)
- **M-114** (GEO Scoring): date calitate jucător per GEO alimentează componenta Market Sizing anual
- **M-121** (S2S Tracking): postback S2S furnizează click_id și date conversie pentru definirea cohortei

**VERIFY**:
- [ ] Fiecare ofertă RevShare activă are sheet de tracking `cohort-tracking-{GEO}-{operator}.gsheet`?
- [ ] Toate sheet-urile actualizate până pe data de 5 a lunii curente?
- [ ] Model LTV fitted la marcajul de 90 zile pentru toate cohortele active?
- [ ] Delta >20% față de M-115 → recalibrare documentată în `payout-model-update-{GEO}-{YYYY-MM-DD}.md`?
- [ ] Cohorte excepționale (LTV >6× CPA) → trigger M-118 Pas 6 declanșat și documentat?
- [ ] Zero campanii RevShare >6 luni fără validare LTV completă (180-day mark atins)?

**VK-uri obligatorii**:
- `✅ [PROC] M-128 | cohort: {GEO}-{operator}-{YYYY-MM} | snapshot: Lună {N} | delta vs M-115: {X}% | status: {OK/RECALIBRATE}`
- `✅ [CORTEX] "M-128 LTV cohort {GEO}-{operator}" | rule: TRAIN-S-001 | v1.0`

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-6): fitting curba LTV (Pas 3), analiza pattern sezonier (Pas 5), interpretare delta semnificativ
- **Claude Sonnet**: actualizare lunară sheet-uri (Pas 2), calcule formula, generare rapoarte de rutină

---

*Procedură creată conform standardului FORGE v1.2 — Genie Slim Core*
