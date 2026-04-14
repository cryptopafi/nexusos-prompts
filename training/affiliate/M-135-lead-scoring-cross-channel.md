---
type: procedure
created: 2026-03-17
status: active
slug: m-135-lead-scoring-cross-channel
tags: [procedure, nexus]
---

# M-135 — Cross-Channel Lead Scoring & Revenue Prioritization

**ID**: M-135
**Domeniu**: Affiliate / Analytics
**Sub-domeniu**: lead-scoring-cross-channel
**Versiune**: 2.0
**Creat**: 2026-03-05
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Conexiuni**: M-123, M-124, M-128, M-131
**Trigger**: La fiecare campanie nouă + review lunar per GEO

---

## Scope

Această procedură definește modelul de scoring cross-channel pentru calificarea lead-urilor generate prin SMS carrier (VOX), SEO organic și native ads, în GEO-urile active: PE, KE, CL, BO, RO. Acoperă:

- Definirea semnalelor de calitate per canal și atribuirea de punctaje
- Calibrarea factorilor per canal și multiplicatori GEO
- Formula Lead Quality Index (LQI) și pragurile HOT/WARM/COLD
- Matricea de prioritizare buget bazată pe scor
- Optimizarea RevShare vs CPA în funcție de LQI
- Detectarea drift-ului în model și recalibrarea periodică
- Integrarea cu Binom tracker și Google Sheets

Procedura NU acoperă: setup tehnic Binom (vezi M-123), negocierea termenilor CPA/RevShare cu rețelele (vezi M-124), analiza LTV cohort detaliată (vezi M-128).

---

## Section 1 — Problema

### De ce scoring-ul generic eșuează în affiliate SMS

**Problema centrală**: Canalele diferite produc lead-uri cu profiluri de calitate fundamental diferite, iar un model de scoring uniform tratează egal lead-uri cu valoare reală de 2-10x diferentă.

**Manifestări concrete**:

1. **Canal SMS carrier (VOX)** produce volume mari cu conversie la FTD (First Time Deposit) de 1.2-2.8% în GEO-uri low-income (BO, KE), dar RevShare-ul pe 50/50 cu VOX comprimă marja. Un lead SMS cu 3 click-uri și fără registration are valoare aproape zero față de un lead care a ajuns la registration.

2. **SEO organic** produce lead-uri cu intenție ridicată (au căutat activ "casino online Peru" sau "forex broker Kenya"), cu rate de conversie 4-8%, dar volum limitat. Tratate identic cu lead-urile SMS, SEO leads sunt sub-priorized la budget allocation.

3. **Native ads** generează lead-uri intermediare: intenție mai mare decât SMS push, volum mai mare decât SEO pur. Fără calibrare, native lead-urile din RO (piață reglementată, jucători sofisticați) sunt scorificate identic cu native leads din BO (piață low-competition, convertor mai simplu dar LTV mai mic).

4. **Desalinerea RevShare vs CPA**: Rețelele (3SNET, MaxBounty, ClickDealer) oferă CPA fix per FTD (~$15-50 PE, $8-25 KE). RevShare 50% din NGR poate fi superior pe un jucător high-value dar inferior pe un jucător care face un singur depozit mic. Fără scor predictiv, nu știm pe ce lead să împingem CPA vs RevShare.

5. **Atribuție complexă**: Un utilizator poate vedea un SMS, da click pe un SEO article a doua zi, și converti via native ad a treia zi. Fără scoring multi-touch, atribuim tot valoarea ultimului canal (last-click), distorsionând alocarea bugetului.

**Consecința businessului**: Budget alocat greșit, arbitraj RevShare/CPA lăsat pe masă, operatori cu termeni mai buni sub-utilizați pe lead-urile de calitate înaltă.

---

## Section 2 — Procedura

### Step 1: Lead Signal Definition per Channel

**Tool**: Google Sheets (Lead Scoring Matrix) + Binom custom variables
**Output**: Tabel de semnale cu punctaje absolute
**Checkpoint**: Toate evenimentele sunt taguite în Binom și transmise spre Sheets via webhook

#### Tabel de semnale — punctaje absolute

| Signal | Descriere | Puncte |
|--------|-----------|--------|
| SMS_OPEN | Mesaj SMS deschis (delivery confirmed + open pixel) | 1 |
| SMS_CLICK | Click pe link în SMS | 5 |
| PUSH_OPEN | Notificare push deschisă | 2 |
| PUSH_CLICK | Click pe push notification | 4 |
| LP_VISIT | Vizită landing page (min 10 sec session) | 3 |
| LP_RETURN | Revenire pe landing page (sesiune repetată) | 6 |
| FORM_START | A început completarea formularului | 10 |
| REGISTRATION | Înregistrare completată pe platforma operatorului | 20 |
| KYC_PASS | Verificare identitate completată | 15 |
| FIRST_DEPOSIT | Primul depozit (FTD) — indiferent de sumă | 50 |
| FTD_HIGH | FTD ≥ $50 echivalent | 25 (bonus) |
| REPEAT_DEPOSIT | Al doilea depozit sau mai mult | 30 |
| REPEAT_3PLUS | 3+ depozite în primele 30 zile | 20 (bonus) |
| EMAIL_OPEN | Email marketing deschis (dacă colectat) | 1 |
| EMAIL_CLICK | Click în email marketing | 3 |
| CHURN_SIGNAL | Inactiv >14 zile post-registration | -10 |
| REFUND | Cerere de refund / chargeback | -30 |

**Regula de cumulare**: Punctele se sumează pe toată durata ferestrei de atribuire (default: 30 zile de la prima interacțiune). Un lead care face SMS_CLICK + LP_VISIT + REGISTRATION + FIRST_DEPOSIT = 5 + 3 + 20 + 50 = 78 puncte (înainte de calibrare).

**Formula Lead Quality Index (LQI)**:

```
LQI = (Σ signals × weights) × GEO_multiplier × Channel_calibration
```

Unde:
- `Σ signals × weights` = suma punctelor din tabelul de mai sus
- `GEO_multiplier` = factorul de ajustare per țară (definit în Step 3)
- `Channel_calibration` = factorul de calibrare per canal (definit în Step 2)

**Rezultat LQI → Clasificare**:
- **HOT**: LQI ≥ 80
- **WARM**: LQI 40–79
- **COLD**: LQI < 40

---

### Step 2: Channel-Specific Score Calibration

**Tool**: Google Sheets (tab "Channel Calibration") + date istorice Binom (90 zile rolling)
**Output**: Factori de calibrare per canal, actualizați lunar
**Checkpoint**: Calibration factors diferă cu <15% față de luna precedentă → alertă de investigat

#### Profiluri de calitate per canal

**SMS Carrier (VOX)**:
- Conversie medie FTD: 1.5-2.5% din clickers
- Profil utilizator: intenție mai scăzută (push non-solicitat), volum mare
- Risc fraud: mediu (carrier blasting poate include numere inactive)
- Calibration factor: **0.75** (reducere 25% față de scor brut)
- Rațional: un SMS_CLICK are valoare intrinsecă mai mică decât un SEO organic click

**SEO Organic**:
- Conversie medie FTD: 4-8% din vizitatori calificați
- Profil utilizator: intenție înaltă (query specific), cercetare prealabilă
- Risc fraud: scăzut
- Calibration factor: **1.25** (bonus 25% față de scor brut)
- Rațional: vizitorul SEO a arătat intenție activă înainte de primul contact

**Native Ads (Taboola/MGID/RevContent)**:
- Conversie medie FTD: 2.5-4.5% din clickers
- Profil utilizator: intenție medie (ad contextual, semi-solicitat)
- Risc fraud: mediu-scăzut (depinde de publisher)
- Calibration factor: **1.00** (neutru, referință)
- Rațional: native este canalul de referință în modelul nostru

**Push Notifications (non-VOX)**:
- Conversie medie FTD: 1.0-2.0%
- Calibration factor: **0.70**

**Email Marketing (list proprie)**:
- Conversie medie FTD: 3.0-6.0%
- Calibration factor: **1.15**

#### Tabel calibrare rezumat

| Canal | Calibration Factor | Note |
|-------|-------------------|------|
| SMS Carrier (VOX) | 0.75 | Revizuit lunar |
| SEO Organic | 1.25 | Revizuit trimestrial |
| Native Ads | 1.00 | Referință |
| Push Non-VOX | 0.70 | Revizuit lunar |
| Email Propriu | 1.15 | Revizuit trimestrial |

**Calculul calibrării din date reale**:
```
Channel_calibration = (FTD_rate_canal / FTD_rate_native_reference) × 0.8 + 0.2
```
Factorul 0.8/0.2 amortizează variațiile extreme. Dacă SEO produce 6% vs native 3%, calibrarea = (6/3) × 0.8 + 0.2 = 1.80 (cap la 1.50 pentru stabilitate).

**Cap / Floor**: Minimum 0.50, maximum 1.50. Valorile în afara acestui range necesită investigare manuală.

---

### Step 3: GEO-Specific Scoring Adjustments

**Tool**: Google Sheets (tab "GEO Multipliers") + date operatori per GEO
**Output**: Multiplicatori GEO actualizați lunar
**Checkpoint**: Multiplicatorii reflectă date reale din ultimele 60 zile, nu estimări

#### Profiluri GEO și raționale

**Peru (PE)**:
- Piață: semi-maturizată, concurență medie, regulatori atenți
- SMS leads: conversie medie, LTV mediu ($80-120 pe 90 zile)
- SEO leads: conversie bună, LTV ridicat ($120-200)
- Operatori preferați: brand-uri locale + 3SNET CPA
- GEO_multiplier: **1.15**
- Rațional: piață cu volum bun și CPA atractiv ($30-50), lead-urile merită bonus

**Kenya (KE)**:
- Piață: în creștere rapidă, mobile-first (M-Pesa), reglementare relaxată
- SMS leads: conversie scăzută-medie dar volum masiv via VOX carrier
- SEO leads: puține dar extrem de valoroase
- LTV mediu: $40-70 (depozite mici, frecvență mare)
- GEO_multiplier: **0.85**
- Rațional: LTV mai mic comprimă valoarea per lead; CPA mai mic ($8-20) reflectat în scor

**Chile (CL)**:
- Piață: premium, jucători cu venituri mai mari, conversie excelentă
- SMS leads: mai puțin eficient (penetrare SMS business limitată)
- SEO leads: excelent, LTV ridicat ($150-250)
- GEO_multiplier: **1.30**
- Rațional: piață premium cu cei mai buni LTV din portofoliu; orice lead din CL valorează mai mult

**Bolivia (BO)**:
- Piață: emergentă, low-competition, dar purchasing power scăzut
- SMS leads: singurul canal scalabil acum
- LTV mediu: $25-50
- GEO_multiplier: **0.70**
- Rațional: piață de volum, nu de valoare; scoruri reduse reflectă LTV scăzut

**Romania (RO)**:
- Piață: reglementată strict (ONJN), jucători sofisticați, LTV foarte ridicat
- SMS leads: practic interzis (GDPR + ONJN)
- SEO leads: dominant, conversie 5-9%, LTV $200-400
- Native ads: funcționează dar costul e ridicat
- GEO_multiplier: **1.40**
- Rațional: cel mai ridicat LTV din portofoliu; orice lead calificat din RO are prioritate maximă

#### Tabel GEO multipliers

| GEO | Multiplier | LTV mediu 90d | Canal dominant | Notă |
|-----|-----------|---------------|----------------|------|
| PE | 1.15 | $80-120 | SMS + Native | Echilibrat |
| KE | 0.85 | $40-70 | SMS VOX | Volume play |
| CL | 1.30 | $150-250 | SEO + Native | Premium |
| BO | 0.70 | $25-50 | SMS VOX | Emergent |
| RO | 1.40 | $200-400 | SEO | Top priority |

**Integrare cu M-128 (LTV Cohort Analysis)**:
Dacă cohortele din M-128 indică că un GEO specific a produs LTV mediu cu >20% mai mare decât baseline în ultimele 90 de zile, GEO_multiplier se ajustează automat:
```
GEO_multiplier_ajustat = GEO_multiplier_baza × (1 + (LTV_actual / LTV_baseline - 1) × 0.5)
```
Factorul 0.5 limitează amplitudinea ajustării. Ajustarea maximă: ±25% față de multiplierul de bază. Revizuire manuală dacă ajustarea depășește ±20%.

---

### Step 4: Scoring Model Implementation

**Tool**: Binom Tracker (custom variables CV1-CV5) + Google Sheets (scoring engine) + webhook automation
**Output**: Dashboard LQI în timp real per lead, clasificare automată HOT/WARM/COLD
**Checkpoint**: 95%+ din lead-uri primesc scor în <5 minute de la eveniment; fără scor = alerta

#### Implementare Binom

**Custom Variables în Binom**:
- `CV1`: Canal (sms_vox / seo_organic / native / push / email)
- `CV2`: GEO (PE / KE / CL / BO / RO)
- `CV3`: Raw score (suma semnalelor înainte de calibrare)
- `CV4`: LQI final (după calibrare × GEO multiplier)
- `CV5`: Clasificare (HOT / WARM / COLD)

**Webhook flow**:
```
Eveniment (click/registration/deposit)
  → Binom înregistrează + trimite webhook
  → Google Sheets Script primește payload
  → Calculează LQI: raw_score × channel_calibration × geo_multiplier
  → Actualizează CV4, CV5 în Binom via API
  → Alertă Slack/Telegram dacă LQI ≥ 80 (HOT lead)
```

#### Google Sheets — Structura scoring engine

**Tab "Raw Events"**: Toate evenimentele per lead_id, timestamp, canal, GEO, tip eveniment, puncte
**Tab "Lead Scores"**: Agregare per lead_id — suma raw score, calibrare aplicată, LQI final, clasificare
**Tab "Channel Calibration"**: Factorii actuali + istoricul lunilor anterioare
**Tab "GEO Multipliers"**: Multiplicatorii actuali + LTV data din M-128
**Tab "Dashboard"**: Pivot per GEO × Canal × Clasificare cu revenue asociat

#### Formula implementare în Google Sheets

```
=SUMIF(RawEvents[lead_id], A2, RawEvents[points])
  * VLOOKUP(B2, ChannelCalibration, 2, FALSE)
  * VLOOKUP(C2, GEOMultipliers, 2, FALSE)
```

**Clasificare automată**:
```
=IF(LQI >= 80, "HOT", IF(LQI >= 40, "WARM", "COLD"))
```

#### Praguri acționale

| Clasificare | LQI | Acțiune imediată |
|-------------|-----|-----------------|
| HOT | ≥ 80 | Trimite la RevShare operator; alertă account manager; fast-track KYC |
| WARM | 40-79 | CPA standard sau RevShare conservator; nurturing sequence activată |
| COLD | < 40 | Cost control strict; fără buget incremental; retargeting minimal |

---

### Step 5: Revenue Prioritization Matrix

**Tool**: Google Sheets (tab "Prioritization Matrix") + rapoarte lunare
**Output**: Ranking GEO × Canal × Offer pentru alocarea bugetului următor
**Checkpoint**: Matrix revizuită la fiecare 2 săptămâni; decizii de buget documentate

#### Logica de prioritizare

**Principiu**: Bugetul incremental se alocă combinației GEO × Canal × Offer cu cel mai ridicat LQI mediu ponderat × volumul potențial.

**Formula de prioritizare**:
```
Priority_Score = LQI_mediu_band × Volum_leads_luna × (Revenue_per_LQI / Cost_per_lead)
```

#### Matricea RevShare vs CPA — Decizie per LQI

**Regula principală**: Dacă LQI > 80 (HOT), preferă RevShare operator.

| LQI | Model preferat | Rațional |
|-----|---------------|----------|
| ≥ 80 (HOT) | RevShare 50% NGR | Jucătorul are potențial LTV înalt; RevShare capturează valoarea pe termen lung |
| 60-79 (WARM+) | RevShare sau CPA ≥ $30 | Risc echilibrat; prefer RevShare dacă operatorul oferă ≥ $30 CPA echivalent |
| 40-59 (WARM-) | CPA fix | Certitudine imediată; RevShare prea riscant pe LTV necunoscut |
| < 40 (COLD) | CPA mic sau drop | Cost-control; nu investi în nurturing |

**Exemplu concret**:
- Lead din RO (GEO=1.40) via SEO (Channel=1.25), cu REGISTRATION + LP_RETURN = 20+6 = 26 raw
- LQI = 26 × 1.25 × 1.40 = **45.5** → WARM-
- Decizie: CPA fix cu 3SNET (dacă disponibil ~$35 RO), altfel RevShare conservator

- Lead din CL (GEO=1.30) via Native (Channel=1.00), cu FTD_HIGH = 50+25 = 75 raw
- LQI = 75 × 1.00 × 1.30 = **97.5** → HOT
- Decizie: Direcționat imediat la RevShare operator, alertă account manager

#### Tabel prioritizare buget (exemplu template lunar)

| Rank | GEO | Canal | Offer/Operator | LQI Mediu | Volum Potențial | Priority Score | Buget Alocat |
|------|-----|-------|----------------|-----------|-----------------|----------------|--------------|
| 1 | RO | SEO | Operator A RevShare | 85+ | Limitat | Înalt | 35% |
| 2 | CL | Native | Operator B RevShare | 78 | Mediu | Înalt | 25% |
| 3 | PE | SMS + Native | 3SNET CPA $40 | 55 | Mare | Mediu | 20% |
| 4 | KE | SMS VOX | ClickDealer CPA $15 | 38 | Foarte mare | Mediu-mic | 15% |
| 5 | BO | SMS VOX | MaxBounty CPA $10 | 28 | Mare | Mic | 5% |

**Nota**: Alocarea se face pe baza datelor reale din luna precedentă. Template-ul de mai sus este ilustrativ.

---

### Step 6: Score Drift Detection & Recalibration

**Tool**: Google Sheets (tab "Drift Monitor") + script automat de alertă
**Output**: Raport drift lunar + decizie de recalibrare
**Checkpoint**: Drift monitorizat săptămânal; recalibrare obligatorie dacă orice trigger este atins

#### Triggere de recalibrare

| Trigger | Prag | Acțiune |
|---------|------|---------|
| Conversion rate change | >20% față de baseline 90 zile | Recalibrare imediată channel factor |
| LTV shift per GEO | >25% față de M-128 cohort | Ajustare GEO multiplier |
| Nou operator lansat | Oricând | Recalibrare scoring pe ofertă specifică |
| Sezonier (Crăciun, Champions League, etc.) | 2 săptămâni înainte | Ajustare temporară +15% HOT threshold |
| Fraud spike detectat | FTD chargeback >5% | Recalibrare canal factor DOWN -20% |
| Campanie SMS nouă masivă (>100k SMS) | La lansare | Recalibrare după primele 72h de date |

#### Procesul de recalibrare

1. **Detectare**: Script Google Sheets compară rolling 14 zile vs baseline 90 zile per canal × GEO
2. **Alertă**: Dacă delta >20%, alert automat Telegram (@Claudeautomationbot)
3. **Investigare**: Verificare manuală — este drift real sau anomalie temporară (weekend, sărbătoare)?
4. **Recalibrare**: Dacă confirmat, ajustare Channel_calibration sau GEO_multiplier cu formula:
   ```
   New_factor = Old_factor × (1 + drift_delta × 0.6)
   ```
   Factorul 0.6 amortizează reacția excesivă.
5. **Documentare**: Entry în tab "Calibration History" cu data, motivul, factorii vechi și noi
6. **Validare**: Monitorizare intensivă 7 zile post-recalibrare

#### Frecvența standard de revizuire

| Componentă | Frecvență | Responsabil |
|------------|-----------|-------------|
| Raw signal weights | Trimestrial | Senior Analyst |
| Channel calibration factors | Lunar | Analytics |
| GEO multipliers | Lunar (sau la trigger) | Analytics + M-128 sync |
| LQI thresholds (HOT/WARM/COLD) | Trimestrial | Strategy |
| RevShare vs CPA decision matrix | La nou operator sau trimestrial | Affiliate Manager |

---

## Section 3 — Cortex Logging

La fiecare actualizare a modelului de scoring, se salvează în Cortex:

```json
{
  "procedure": "M-135",
  "version": "2.0",
  "timestamp": "{{ISO_DATE}}",
  "scoring_model": {
    "signals": {
      "SMS_OPEN": 1,
      "SMS_CLICK": 5,
      "PUSH_OPEN": 2,
      "PUSH_CLICK": 4,
      "EMAIL_OPEN": 1,
      "EMAIL_CLICK": 3,
      "LP_VISIT": 3,
      "LP_RETURN": 6,
      "FORM_START": 10,
      "REGISTRATION": 20,
      "KYC_PASS": 15,
      "FIRST_DEPOSIT": 50,
      "FTD_HIGH_BONUS": 25,
      "REPEAT_DEPOSIT": 30,
      "REPEAT_3PLUS_BONUS": 20,
      "CHURN_SIGNAL": -10,
      "REFUND": -30
    },
    "channel_calibration": {
      "sms_vox": 0.75,
      "seo_organic": 1.25,
      "native_ads": 1.00,
      "push_non_vox": 0.70,
      "email_own": 1.15
    },
    "geo_multipliers": {
      "PE": 1.15,
      "KE": 0.85,
      "CL": 1.30,
      "BO": 0.70,
      "RO": 1.40
    },
    "thresholds": {
      "HOT": 80,
      "WARM_lower": 40,
      "WARM_upper": 79,
      "COLD_max": 39
    },
    "lqi_formula": "raw_score * channel_calibration * geo_multiplier",
    "revenue_decision": {
      "HOT_lqi_gte_80": "prefer_revshare",
      "WARM_plus_lqi_60_79": "revshare_or_cpa_gte_30",
      "WARM_minus_lqi_40_59": "cpa_fixed",
      "COLD_lqi_lt_40": "cpa_minimal_or_drop"
    },
    "ltv_integration": "M-128",
    "drift_recalibration_threshold_pct": 20,
    "calibration_history_tab": "Google Sheets / Calibration History"
  },
  "active_geos": ["PE", "KE", "CL", "BO", "RO"],
  "active_verticals": ["betting", "casino", "forex_cfd", "health_supplements"],
  "networks": ["3SNET", "MaxBounty", "ClickDealer"],
  "carrier_partner": "VOX",
  "last_calibration": "{{ISO_DATE}}",
  "next_review": "{{ISO_DATE_PLUS_30}}"
}
```

**Trigger Cortex save**: La fiecare modificare a oricărui factor de calibrare, GEO multiplier sau prag LQI. Minimum: o dată pe lună.

---

## Section 4 — Enforcement Loop

### WHERE
- Google Sheets Scoring Engine (activ 24/7, date refresh la fiecare eveniment via webhook)
- Binom Tracker (CV1-CV5 populate în timp real)
- Telegram alertă (@Claudeautomationbot) pentru HOT leads și drift alerts
- Review lunar în cadrul ședinței de analytics (calendat fix, prima zi lucrătoare a lunii)

### WHEN
- **Continuu**: Scoring per eveniment (webhook automat)
- **Săptămânal**: Drift monitor check (automat + review manual scurt)
- **La campanie nouă**: Verificare că noua sursă are channel_calibration factor definit
- **Lunar**: Full review calibrare + GEO multiplieri + prioritization matrix
- **La trigger**: Imediat la orice trigger de recalibrare din Step 6

### HOW — VIOLATION Declarations

**VIOLATION-M135-001**: Lead-uri nescorificulate în Binom (CV4 = NULL) la mai mult de 24h de la eveniment.
- Consecință: Budget freeze pe canalul afectat până la remediere.

**VIOLATION-M135-002**: Channel calibration sau GEO multiplier neactualizat mai mult de 45 de zile fără documentare explicită a motivului.
- Consecință: Escaladare la Senior Analyst; model revert la valorile precedente validate.

**VIOLATION-M135-003**: Decizie RevShare/CPA contrară matricei (ex: lead HOT trimis la CPA sub $30) fără aprobare documentată.
- Consecință: Audit retrospectiv al campaniei; documentare obligatorie a motivului excepției.

**VIOLATION-M135-004**: Recalibrare executată fără entry în "Calibration History" tab.
- Consecință: Recalibrarea este invalidă până la documentare retroactivă.

### CONNECT
- **M-128** (LTV Cohort): GEO multiplieri sunt sincronizați cu datele LTV lunare
- **M-123** (Binom Setup): Custom variables CV1-CV5 configurate conform M-135
- **M-124** (Network Agreements): CPA rates din M-124 alimentează matricea RevShare/CPA din Step 5
- **M-131** (Campaign Attribution): Atribuția multi-touch din M-131 furnizează raw signals pentru sumarea corectă în cazul journey-urilor cross-canal

### VERIFY — 6 Checks

| # | Check | Criterii de PASS | Frecvență |
|---|-------|-----------------|-----------|
| 1 | LQI Coverage | >95% din lead-uri au LQI calculat în <5min de la eveniment | Zilnic (automat) |
| 2 | Calibration Currency | Channel factors și GEO multipliers actualizați în ultimele 45 zile | Săptămânal |
| 3 | HOT Lead Routing | >90% din lead-urile HOT direcționate corect (RevShare sau CPA ≥$30) | Lunar |
| 4 | Drift Alert Functionality | Test alert trimis și recepționat pe Telegram | Lunar |
| 5 | Cortex Sync | Entry Cortex prezent cu timestamp în ultimele 30 zile | Lunar |
| 6 | Revenue per Band | Revenue/lead pentru HOT > 3× față de COLD; dacă nu, investigare model | Lunar |

### MODEL ROUTING

| Sarcină | Model | Justificare |
|---------|-------|-------------|
| Calibrare inițială + setup model | Claude Opus 4.6 | Complexitate ridicată, raționament strategic |
| Review lunar standard | Claude Sonnet 4.6 | Sarcini analitice standard |
| Alertă drift + investigare rapidă | Claude Sonnet 4.6 | Viteză + cost |
| Raport executiv lunar | Claude Opus 4.6 | Sinteză strategică, insight-uri acționabile |
| Automatizare webhook/script debug | Claude Sonnet 4.6 | Cod + debugging standard |

---

## Section 5 — Dependente

| Dependență | ID | Tip | Descriere | Impact dacă lipsește |
|------------|-----|-----|-----------|---------------------|
| LTV Cohort Analysis | M-128 | Input critic | Furnizează LTV mediu per GEO pentru calibrarea GEO_multiplier | GEO multiplieri bazați pe estimări, nu date reale |
| Binom Tracker Setup | M-123 | Infrastructură | Custom variables CV1-CV5 trebuie configurate | Scoring nu poate fi stocat și urmărit în Binom |
| Network CPA Agreements | M-124 | Input | CPA rates curente per rețea și GEO | Matricea RevShare/CPA folosește valori incorecte |
| Campaign Attribution | M-131 | Input | Atribuție multi-touch pentru journey-uri cross-canal | Sumarea semnalelor este incompletă pentru leaduri cross-canal |
| Google Sheets Engine | — | Infrastructură | Scoring engine live | Fără implementare, scoring rămâne manual |
| Telegram Alert Bot | — | Output | Alertă HOT leads și drift | Alertele nu ajung la echipă în timp real |
| VOX Carrier API | — | Input | Date delivery/open SMS | Semnalele SMS_OPEN nu sunt colectate |

---

## Section 6 — Metrics

| Metric | Definiție | Target | Frecvență Raportare |
|--------|-----------|--------|---------------------|
| Model Accuracy | % lead-uri HOT care convertesc la FTD vs total HOT | ≥ 40% HOT → FTD | Lunar |
| Predicted vs Actual Conversion | Rata FTD predicționată de model vs rata reală per band | Delta < 15% per band | Lunar |
| Score Drift Rate | % change în LQI mediu per canal × GEO față de luna precedentă | < 20% fără trigger identificat | Săptămânal |
| Revenue per Lead — HOT | Venit mediu generat per lead clasificat HOT | > 3× COLD | Lunar |
| Revenue per Lead — WARM | Venit mediu generat per lead clasificat WARM | > 1.5× COLD | Lunar |
| Revenue per Lead — COLD | Venit mediu generat per lead clasificat COLD | Benchmark (referință) | Lunar |
| RevShare Capture Rate | % din lead-urile HOT efectiv direcționate la RevShare | ≥ 85% | Lunar |
| Recalibration Frequency | Număr de recalibrări pe lună | 1-3 normale; >5 = model instabil | Lunar |
| LQI Coverage | % lead-uri cu LQI calculat vs total lead-uri generate | ≥ 95% | Zilnic |
| HOT Lead Alert Latency | Timp mediu de la eveniment FTD la alertă Telegram HOT | < 10 minute | Săptămânal |

---

## Checklist Pre-Publicare

- [x] Toate 7 secțiunile prezente și complete
- [x] Formula LQI definită explicit cu toate componentele
- [x] Praguri HOT/WARM/COLD definite (80 / 40-79 / <40)
- [x] Tabel semnale complet (17 semnale cu punctaje)
- [x] Channel calibration factors definiți pentru toate cele 5 canale
- [x] GEO multipliers definiți pentru toate cele 5 GEO-uri (PE, KE, CL, BO, RO)
- [x] Integrare M-128 (LTV Cohort) documentată explicit
- [x] Regula RevShare vs CPA per LQI band definită clar
- [x] Binom implementation cu CV1-CV5 specificat
- [x] Triggere de recalibrare complete (6 triggere documentate)
- [x] Enforcement Loop complet (WHERE / WHEN / HOW / CONNECT / VERIFY / MODEL ROUTING)
- [x] VIOLATION declarations prezente (4 violations)
- [x] VERIFY checklist cu 6 checks și criterii de PASS
- [x] Cortex JSON logging structurat și complet
- [x] Tabel dependente complet (7 dependențe)
- [x] Tabel metrics complet (10 metrici cu targets)
- [x] Model routing table prezent
- [x] Toate secțiunile în limba română (termeni tehnici în engleză OK)
- [x] Conexiuni la M-123, M-124, M-128, M-131 documentate
- [x] Trigger definit: "La fiecare campanie nouă + review lunar per GEO"
