---
type: procedure
created: 2026-03-17
status: active
slug: m-136-creative-fatigue-detection
tags: [procedure, nexus]
---

# M-136 — Predictive Creative Fatigue Detection & Rotation Protocol

**ID**: M-136
**Domeniu**: Affiliate / Campaign Management
**Sub-domeniu**: creative-fatigue-prediction
**Versiune**: 2.0
**Creat**: 2026-03-05
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Conexiuni**: M-119, M-122, M-125, M-129
**Trigger**: Weekly per campanie activă + la lansarea oricărui creative nou

---

## Scope

Această procedură acoperă detectarea predictivă a oboselii creative (creative fatigue) în campaniile SMSads pe canalele SMS (VOX carrier), native ads (Taboola/Outbrain) și TikTok, pentru verticalele betting/casino și forex/CFD, în GEO-urile: Peru (PE), Kenya (KE), Chile (CL), Bolivia (BO), România (RO).

Include: stabilirea baseline-ului de performanță per canal, modelul predictiv de fatigue scoring, managementul bibliotecii de creative, sistemul automat de rotație, generarea brief-urilor pentru creative noi și monitorizarea post-rotație.

Nu acoperă: producția efectivă a creative-urilor (vezi M-119), buying media (vezi M-122) sau audit spy nativ (vezi M-129).

**Notă GDPR**: Această procedură procesează date comportamentale granulare per utilizator (frecvență de expunere, click-through time, audience overlap cross-platform Taboola/Outbrain). Pentru GEO România (jurisdicție UE), procesarea acestor date este supusă GDPR Art. 6. Asigurați-vă că baza legală de procesare este documentată și că retenția datelor respectă politica internă înainte de implementare în RO. Consultați departamentul de compliance pentru orice GEO UE.

---

## Section 1 — Problema

### De ce oboseala creativă distruge campaniile

**Oboseala creativă** apare când aceeași audiență vede același mesaj de prea multe ori. Efectul nu este gradual — este exponențial în rău.

#### Mecanismul de degradare

**CPM escaladare**: Algoritmii Taboola/Outbrain penalizează creative cu engagement rate în scădere. Un creative cu CTR decreasing primește inventory de calitate inferioară, CPM crește cu 15-40% fără ca bugetul să crească. La SMS, retrimiterile către segmente saturate generează rate mari de opt-out (>3%), ceea ce reduce reach-ul brut al listei pe termen lung.

**CTR Decay Curve — formă tipică per canal**:

- **SMS**: Decay curve în formă de „cliff" (cădere bruscă). SMS-ul are CTR ridicat la prima expunere (3-5%), urmată de o platformă de 5-7 zile, apoi o cădere bruscă la <1% după 2-3 re-trimiteri la același segment. Nu există „long tail" — oboseala e binară la SMS.

- **Native Ads (Taboola/Outbrain)**: Decay curve în formă de „S inversat lent". CTR-ul pornește de la baseline (0.2-0.8%), crește ușor în primele 2-3 zile (algoritmul optimizează), atinge peak, apoi scade progresiv pe parcursul a 7-14 zile. Declinul este mai lent dar consistent și greu reversibil fără rotație.

- **TikTok**: Decay curve în formă de „spike-and-crash". CTR poate fi 1-3% în primele 24-48h (push puternic algoritmic pentru content nou), după care cade accelerat la <0.5% dacă frecvența depășește 3.5x/săptămână per utilizator.

**Frequency saturation la SMS**: Contactarea aceluiași număr de mai mult de 2 ori pe lună în același GEO produce: rata opt-out >3%, scăderea CTR cu 60-80%, risc de blocare carrier (VOX penalizează senderii cu complaint rate ridicat).

**Audience overlap în rețelele native**: Taboola și Outbrain împart inventar din aceleași publisher-uri. Fără rotație cross-platformă, același utilizator poate vedea același unghi creativ de 6-8 ori/săptămână — depășind pragul psihologic de „banner blindness" ireversibilă.

**Pierderi concrete fără rotation protocol**:
- CTR cade cu 40-70% față de baseline în 10-21 zile
- CPA crește cu 50-120%
- Lista SMS se erodează cu 5-15% opt-out per campanie re-trimisă
- Spend risipat estimat: 30-45% din bugetul lunar pe creative epuizate

---

## Section 2 — Procedura

---

### Step 1: Creative Performance Baseline Setup

**Obiectiv**: Stabilirea valorilor de referință CTR per canal și per GEO înainte de a putea detecta fatigue.

**Tool**: Google Sheets / dashboard Taboola & Outbrain / SMS platform (VOX reporting) / TikTok Ads Manager

**Baseline CTR per canal (standarde SMSads)**:

| Canal | Baseline CTR minim | Baseline CTR optim | Baseline CTR excelent |
|-------|-------------------|-------------------|----------------------|
| SMS (VOX) | 2% | 3-5% | >5% |
| Native Ads (Taboola/Outbrain) | 0.15% | 0.2-0.8% | >0.8% |
| TikTok | 0.8% | 1-3% | >3% |

**Baseline per GEO** — ajustări față de standard:

| GEO | SMS adj. | Native adj. | TikTok adj. |
|-----|----------|-------------|-------------|
| PE (Peru) | -0.5% | -0.1% | standard |
| KE (Kenya) | +0.5% | -0.05% | +0.3% |
| CL (Chile) | standard | standard | standard |
| BO (Bolivia) | -0.3% | -0.1% | -0.2% |
| RO (România) | +0.2% | +0.1% | standard |

**Definirea pragului de fatigue**:

> **FATIGUE_SIGNAL**: CTR scade cu >30% față de baseline pe 3 zile consecutive.

Exemple concrete:
- SMS Peru, baseline 3% → FATIGUE_SIGNAL la CTR <2.1% timp de 3 zile
- Native Chile, baseline 0.4% → FATIGUE_SIGNAL la CTR <0.28% timp de 3 zile
- TikTok Kenya, baseline 1.5% → FATIGUE_SIGNAL la CTR <1.05% timp de 3 zile

**Frequency caps obligatorii per canal**:
- SMS: max 2x/lună per contact per GEO (hard cap VOX)
- Native Ads: max 3x/săptămână per utilizator unic
- TikTok: max 4x/săptămână per utilizator unic

**Output**: Tabel baseline completat în tracking sheet, cu valorile per canal + GEO + vertical.

**Checkpoint**: Baseline documenting confirmat în Cortex înainte de lansarea oricărei campanii noi. Fără baseline = nu se poate detecta fatigue.

---

### Step 2: Predictive Fatigue Model

**Obiectiv**: Detectarea fatigueului ÎNAINTE ca CTR să fi căzut deja (leading indicators, nu lagging).

**Tool**: Google Sheets cu formule custom / Python script opțional pentru automatizare

**Leading indicators — semnale de fatigue înainte de CTR drop**:

1. **Frequency delta crescător**: Frecvența medie per utilizator crește de la zi la zi (mai mulți utilizatori ating cap-ul de frecvență) → audiența activă se contractă.
2. **Click-through time crescător**: Utilizatorii care dau click o fac mai lent (hesitation crescut) → interes scăzut față de creative.
3. **CTR variance descrescătoare**: Ziua-de-zi variația CTR devine mai mică și mai mică → creative-ul „a atins plafonul" și intră în platou înainte de cădere.

**Formula Fatigue Score**:

```
Fatigue_Score = (Frequency_delta × 0.40) + (CTR_variance_delta × 0.35) + (CTR_trend × 0.25)
```

Unde:
- `Frequency_delta` = (frecvența medie azi - frecvența medie acum 3 zile) / frecvența medie acum 3 zile × 100 → normalizat 0-10
- `CTR_variance_delta` = (variance CTR săptămâna trecută - variance CTR săptămâna curentă) / variance CTR săptămâna trecută × 100 → normalizat 0-10 (variance în scădere = scor mai mare)
- `CTR_trend` = pantă regresie liniară CTR ultimele 7 zile → normalizat 0-10 (pantă negativă = scor mai mare)

**Interpretare scor**:
| Fatigue_Score | Status | Acțiune |
|--------------|--------|---------|
| 0 - 3.0 | HEALTHY | Monitorizare standard |
| 3.1 - 5.5 | WARNING | Review manual, pregătire creative nou |
| 5.6 - 7.5 | ALERT | Brief nou generat, rotație în 48h |
| 7.6 - 10.0 | CRITICAL | Rotație imediată, creative oprit |

**Exemplu calcul concret**:

Campanie: SMS betting, GEO Kenya, creative `KE-betting-fomo-text-v2`

- Frequency_delta: frecvența medie a crescut de la 1.2 la 1.8 → delta 50% → normalizat = **6.0**
- CTR_variance_delta: variance a scăzut de la 0.8% la 0.3% → delta 62% → normalizat = **7.5**
- CTR_trend: regresie liniară ultimele 7 zile arată pantă -0.15%/zi → normalizat = **5.5**

```
Fatigue_Score = (6.0 × 0.40) + (7.5 × 0.35) + (5.5 × 0.25)
             = 2.40 + 2.625 + 1.375
             = 6.40 → STATUS: ALERT
```

Acțiune: Brief nou generat, rotație în 48h.

**Output**: Fatigue Score calculat zilnic per creative activ, consemnat în tracking sheet.

**Checkpoint**: Dacă Fatigue_Score > 5.5 și nu există brief nou creat în sistem → VIOLATION (vezi Section 4).

---

### Step 3: Creative Library Management

**Obiectiv**: Menținerea unei biblioteci organizate de creative pentru rotație rapidă.

**Tool**: Google Drive (folder structurat) + Notion database

**Taxonomia creativelor**:

| Dimensiune | Valori acceptate |
|------------|-----------------|
| Angle (unghi) | Curiosity / Fear / Social_Proof / FOMO / Benefit |
| Format | image / video / text / carousel |
| GEO | PE / KE / CL / BO / RO |
| Vertical | betting / casino / forex / cfd |
| Language | es / en / sw / ro |

**Naming convention obligatorie**:

```
{GEO}-{Vertical}-{Angle}-{Format}-{Version}
```

Exemple concrete:
- `BO-betting-curiosity-image-v3` — Bolivia, betting, unghi curiosity, format imagine, versiunea 3
- `KE-casino-fomo-video-v1` — Kenya, casino, unghi FOMO, format video, versiunea 1
- `PE-forex-social_proof-text-v2` — Peru, forex, unghi social proof, format text, versiunea 2
- `RO-casino-benefit-image-v4` — România, casino, unghi benefit, format imagine, versiunea 4
- `CL-cfd-fear-carousel-v1` — Chile, CFD, unghi fear, format carousel, versiunea 1

**Regula bibliotecii**: Fiecare GEO × Vertical trebuie să aibă minimum 3 creative active în bibliotecă (diferite Angles) pentru rotație. Sub 3 = gap de acoperire (flagat în metrici).

**Versioning**: Versiunile se incrementează numeric. Niciodată nu se suprascrie o versiune existentă — se creează versiune nouă. Versiunile arhivate se marchează cu `-archived` în Notion.

**Output**: Biblioteca completată în Notion cu toate metadatele, fișierele în Google Drive organizate pe GEO/Vertical.

**Checkpoint**: Coverage check săptămânal — fiecare combinație GEO × Vertical are cel puțin 3 creative active?

---

### Step 4: Rotation Trigger System

**Obiectiv**: Declanșarea automată a rotației creative pe baza semnalelor definite.

**Tool**: Zapier / Make (automatizare trigger) + SMS platform (VOX) + Taboola/Outbrain dashboard + TikTok Ads Manager

**Triggere automate per canal**:

**SMS (VOX)**:
- Trigger principal: după 50.000 trimiteri per segment per creative
- Trigger secundar: dacă CTR < baseline × 0.70 în ultimele 3 zile (FATIGUE_SIGNAL confirmat)
- Trigger hard: dacă opt-out rate > 2.5% pe o trimitere
- Acțiune: switch la creative următor din bibliotecă (același GEO + Vertical, unghi diferit)

**Native Ads (Taboola/Outbrain)**:
- Trigger principal: Fatigue_Score > 5.5 (STATUS ALERT) pe 3 zile consecutive
- Trigger secundar: FATIGUE_SIGNAL confirmat (CTR drop >30% × 3 zile)
- Trigger hard: CPM crește cu >25% față de baseline cu CTR în scădere simultană
- Acțiune: pauză creative curent, activare creative următor din bibliotecă; notificare generare brief dacă librăria < 3 creative active

**TikTok**:
- Trigger principal: frecvența per utilizator depășește 3.5x/săptămână
- Trigger secundar: CTR drop >40% față de baseline în 48h (decay rapid specific TikTok)
- Trigger hard: completion rate video scade sub 15%
- Acțiune: rotație imediată, brief pentru unghi nou

**Logica de selecție creative la rotație**:
1. Prioritizează creative cu Angle diferit față de cel fatiguat
2. Prioritizează creative nevăzute de segmentul curent în ultimele 30 de zile
3. Dacă librăria este goală → generează brief imediat (Step 5) + escaladare manuală

**Output**: Log de rotație cu timestamp, creative-ul înlocuit, creative-ul activat, trigger-ul care a declanșat rotația.

**Checkpoint**: Rotația a fost executată în <24h de la declanșarea triggerului? Dacă nu → VIOLATION.

---

### Step 5: New Creative Brief Generation

**Obiectiv**: Generarea automată a brief-ului pentru creative nou când fatigue este detectat sau biblioteca este sub minimul de 3.

**Tool**: Claude Opus (via Genie) pentru analiză și generare brief / template standard SMSads

**Template brief automat (generat la detecție fatigue)**:

```
CREATIVE BRIEF — ROTAȚIE FATIGUE
================================
ID Brief: {GEO}-{Vertical}-brief-{YYYY-MM-DD}
Generat: {timestamp}
Trigger: {trigger_type} — Fatigue_Score {score} / FATIGUE_SIGNAL {confirmed}

CREATIVE PRECEDENT (fatiguat):
- ID: {creative_id}
- Angle câștigător: {winning_angle}
- CTR peak: {peak_ctr}%
- Longevitate: {days} zile
- Ce a funcționat: {top_performing_element}

CE TREBUIE SCHIMBAT:
- Headline formula: [{schimba_din}] → [{schimba_în}]
  Exemplu: „Câștigă X" → „X oameni din {GEO} au câștigat deja"
- Thumbnail/vizual: {descriere_schimbare_vizual}
- Copy lead (primul rând): {nou_unghi_deschidere}
- Angle nou recomandat: {recommended_angle} (diferit de {fatigued_angle})

REFERINTE INSPIRATIE:
- M-129 Native Ads Spy: verifică top performers din GEO {GEO} în ultimele 14 zile
- Angle-uri testate anterior în {GEO}: {list_tested_angles}
- Angle-uri neîncercate: {list_untested_angles}

SPECIFICATII TEHNICE:
- Canal: {channel}
- GEO: {GEO} | Vertical: {vertical}
- Format: {format}
- Naming convention: {GEO}-{vertical}-{new_angle}-{format}-v{next_version}
- Deadline producție: {deadline — 48h de la brief}

COMPLIANCE (obligatoriu per GEO):
[ ] Disclaimer gambling inclus conform reglementare {GEO}:
    PE: "Las apuestas pueden crear adicción. MINCETUR. +18."
    KE: "Bahati mbaya. BCLB imeruhusu. +18."
    CL: "El juego puede crear adicción. SCJ. +18."
    BO: "Juega responsablemente. Solo mayores de 18."
    RO: "Jocurile de noroc pot crea dependență. Jucați responsabil. ONJN. +18."
[ ] Affiliate disclosure inclus (dacă reglementat în GEO)
[ ] Nicio garanție de câștig sau limbaj înșelător

CHECKLIST BRIEF:
[ ] Angle diferit față de cel fatiguat
[ ] Headline testată în M-129 spy sau bazată pe competitor winner
[ ] Vizual actualizat (nu reuse din versiunea fatiguată)
[ ] Copy lead schimbat complet (primul rând)
[ ] Naming convention respectat
```

**Referință M-129**: La fiecare generare de brief, consultă M-129 (Native Ads Spy Protocol) pentru a identifica creative-urile câștigătoare ale competitorilor în GEO-ul respectiv. Angle-urile de succes din spy = input prioritar pentru brief.

**Output**: Brief completat salvat în Notion (linked la creative-ul fatiguat), assignat echipei de producție.

**Checkpoint**: Brief generat în <4h de la detecția fatigue ALERT? Creative nou livrat în <48h de la brief?

---

### Step 6: Post-Rotation Performance Monitoring

**Obiectiv**: Măsurarea lift-ului post-rotație și calibrarea modelului predictiv pentru cicluri viitoare.

**Tool**: Google Sheets / Taboola dashboard / TikTok Ads Manager / SMS platform reporting

**Metrici urmărite post-rotație**:

**Lift vs creative fatiguat**:
- CTR lift = (CTR_creative_nou - CTR_creative_fatiguat_ultimele_3_zile) / CTR_creative_fatiguat × 100
- Target: lift >50% față de CTR-ul fatiguat în primele 3 zile
- Warning: dacă lift <20% → brief-ul sau producția au greșit angle; analiză manuală

**Timp până la fatigue următor (longevitate)**:
- Tracking de la data activării creative-ului nou până la primul FATIGUE_SIGNAL
- Benchmarks longevitate per GEO:

| GEO | SMS longevitate | Native longevitate | TikTok longevitate |
|-----|----------------|-------------------|-------------------|
| PE | 18-25 zile | 14-21 zile | 7-12 zile |
| KE | 20-28 zile | 15-22 zile | 8-14 zile |
| CL | 15-22 zile | 12-18 zile | 6-10 zile |
| BO | 22-30 zile | 16-24 zile | 9-15 zile |
| RO | 12-18 zile | 10-16 zile | 5-9 zile |

- Dacă longevitatea creativului nou este cu >30% mai mică decât benchmark → angle greșit sau audiența este suprasaturată; acțiune: GEO audience refresh sau unghi complet nou.

**Calibrare model predictiv**:
- La fiecare ciclu de rotație: compară Fatigue_Score la momentul rotației cu CTR actual
- Dacă rotația a fost prea devreme (CTR era încă ok) → ajustează ponderea în formulă
- Dacă rotația a fost prea târzie (CTR deja colapsase) → scade pragurile trigger

**Output**: Report post-rotație completat în Notion (linked la brief și creative-ul nou), cu lift%, longevitate estimată, calibrare model.

**Checkpoint**: Report post-rotație completat la 7 zile de la fiecare rotație?

---

## Section 3 — Cortex Logging

```json
{
  "procedure": "M-136",
  "version": "2.0",
  "event_type": "creative_lifecycle",
  "timestamp": "{ISO8601}",
  "campaign": {
    "geo": "{PE|KE|CL|BO|RO}",
    "vertical": "{betting|casino|forex|cfd}",
    "channel": "{sms|native|tiktok}"
  },
  "creative": {
    "id": "{GEO}-{vertical}-{angle}-{format}-v{n}",
    "angle": "{Curiosity|Fear|Social_Proof|FOMO|Benefit}",
    "format": "{image|video|text|carousel}",
    "activated_date": "{YYYY-MM-DD}",
    "deactivated_date": "{YYYY-MM-DD|null}",
    "longevity_days": "{int|null}"
  },
  "performance": {
    "baseline_ctr": "{float}",
    "peak_ctr": "{float}",
    "ctr_at_rotation": "{float}",
    "fatigue_score": "{float}",
    "fatigue_score_status": "{HEALTHY|WARNING|ALERT|CRITICAL}",
    "fatigue_signal_confirmed": "{true|false}",
    "rotation_trigger": "{volume_50k|fatigue_signal|frequency_cap|manual}"
  },
  "rotation": {
    "brief_generated": "{true|false}",
    "brief_id": "{brief_id|null}",
    "new_creative_id": "{creative_id|null}",
    "rotation_response_time_hours": "{float|null}",
    "lift_percent": "{float|null}"
  },
  "library_status": {
    "active_creatives_count": "{int}",
    "coverage_gap": "{true|false}",
    "angles_available": ["{angle1}", "{angle2}"]
  }
}
```

**Frecvență logging**: La fiecare eveniment (activare creative, FATIGUE_SIGNAL, rotație, brief generat, post-rotație report).

**Destinație**: Cortex KB via `cortex_store` cu tag `creative-fatigue` + `{GEO}` + `{vertical}`.

---

## Section 4 — Enforcement Loop

### WHERE
- Toate campaniile active SMSads pe SMS (VOX), Taboola, Outbrain, TikTok
- Toate GEO-urile: PE, KE, CL, BO, RO
- Toate verticalele: betting, casino, forex, CFD

### WHEN
- **Weekly**: Review Fatigue_Score pentru toate creative-urile active (luni dimineața)
- **La lansare**: Baseline setup completat înainte de prima zi activă a oricărui creative nou
- **Trigger-based**: Acțiune în <24h de la orice trigger automat (ALERT sau CRITICAL)
- **Post-rotație**: Report la 7 zile de la fiecare rotație

### HOW — VIOLATION Declarations

**VIOLATION-M136-001**: Creative activ cu Fatigue_Score > 7.5 (CRITICAL) fără rotație executată în 24h.
→ Acțiune: Oprire imediată creative, escaladare manager, audit cauze întârziere.

**VIOLATION-M136-002**: Fatigue_Score calculat dar nu consemnat în Cortex sau tracking sheet.
→ Acțiune: Completare retroactivă obligatorie, investigare gap de logging.

**VIOLATION-M136-003**: Librărie creativă sub 3 creative active pentru orice GEO × Vertical activ, fără brief în lucru.
→ Acțiune: Brief generat imediat (aceeiași zi), escaladare producție cu deadline 48h.

**VIOLATION-M136-004**: Creative nou lansat fără baseline documentat.
→ Acțiune: Baseline calculat retroactiv din primele 3 zile de date, notat ca retroactiv.

**VIOLATION-M136-005**: Frequency cap depășit (SMS >2x/lună, native >3x/săptămână, TikTok >4x/săptămână).
→ Acțiune: Oprire imediată campanie pe segmentul respectiv, audit liste/audience.

### CONNECT
- **M-119**: Producție creative noi (primește brief-urile din Step 5)
- **M-122**: Media buying — primește instrucțiunile de rotație și activare
- **M-125**: Reporting campanii — furnizează datele CTR pentru calculul Fatigue_Score
- **M-129**: Native Ads Spy — sursă de inspirație pentru brief-uri noi la detecție fatigue

### VERIFY — 6 Checks Obligatorii

1. **Baseline completat**: Există baseline CTR documentat pentru fiecare creative activ? (DA/NU)
2. **Fatigue_Score calculat**: Scorul a fost calculat și consemnat în ultimele 7 zile pentru fiecare creative? (DA/NU)
3. **Librărie acoperire**: Fiecare GEO × Vertical activ are ≥ 3 creative în bibliotecă? (DA/NU — listează gap-urile)
4. **Rotații executate la timp**: Toate triggerele ALERT/CRITICAL au dus la rotație în <24h? (DA/NU)
5. **Brief-uri generate**: Există brief activ pentru fiecare creative în ALERT sau CRITICAL? (DA/NU)
6. **Post-rotație report**: Toate rotațiile din ultimele 14 zile au report de lift completat? (DA/NU)

### MODEL ROUTING

| Task | Model | Justificare |
|------|-------|-------------|
| Calculare Fatigue_Score + interpretare | Sonnet | Task mecanic, formulă fixă |
| Analiză pattern fatigue multi-GEO | Opus | Raționament complex cross-campaign |
| Generare brief creativ (Step 5) | Opus | Creativitate + context strategic |
| Calibrare model predictiv | Opus | Ajustare ponderi + raționament statistic |
| Logging Cortex + tracking sheet | Sonnet | Task mecanic repetitiv |
| Verificare coverage bibliotecă | Sonnet | Task mecanic de inventar |
| Spy research M-129 pentru brief | Opus | Analiză competitivă complexă |

---

## Section 5 — Dependente

| Procedură | Tip relație | Detaliu |
|-----------|-------------|---------|
| M-119 | Output → Input | M-136 trimite brief-uri; M-119 produce creative-urile noi |
| M-122 | Coordonare | M-136 emite instrucțiuni rotație; M-122 execută în platforme |
| M-125 | Input | M-125 furnizează datele de performance (CTR, frequency, CPM) |
| M-129 | Referință | M-136 consultă M-129 pentru inspirație unghiuri noi la fiecare brief |
| TRAIN-S-001 | Regulă parentală | Toate procedurile de training urmează standardul TRAIN-S-001 |

---

## Section 6 — Metrics

| Metric | Definiție | Target | Warning | Critical |
|--------|-----------|--------|---------|----------|
| Creative Longevity (zile) | Zile de la activare la primul FATIGUE_SIGNAL | ≥ benchmark GEO | <75% benchmark | <50% benchmark |
| CTR Lift Post-Rotație (%) | (CTR_nou - CTR_fatiguat) / CTR_fatiguat × 100 | >50% | 20-50% | <20% |
| Creative Library Coverage (%) | % combinații GEO×Vertical cu ≥3 creative active | 100% | <85% | <70% |
| Rotation Response Time (ore) | Ore de la trigger ALERT la rotație executată | <24h | 24-48h | >48h |
| Fatigue_Score Accuracy (%) | % cazuri în care Fatigue_Score a prezis corect FATIGUE_SIGNAL | >75% | 60-75% | <60% |
| Brief Generation Time (ore) | Ore de la detecție ALERT la brief generat | <4h | 4-12h | >12h |

---

## Checklist Pre-Publicare

- [x] Scope definit și acoperă toate canalele + GEO-urile SMSads
- [x] Baseline CTR per canal documentat cu ajustări GEO
- [x] FATIGUE_SIGNAL definit (>30% drop × 3 zile consecutive)
- [x] Frequency caps documentate per canal (SMS 2x/lună, native 3x/săpt, TikTok 4x/săpt)
- [x] Fatigue_Score formula completă cu ponderi (0.40 / 0.35 / 0.25)
- [x] Exemplu calcul Fatigue_Score concret (KE-betting-fomo-text-v2, scor 6.40)
- [x] CTR decay curve descrisă per canal (cliff SMS, S-inversat native, spike-crash TikTok)
- [x] Taxonomie creativă completă (Angle × Format × GEO × Vertical × Language)
- [x] Naming convention documentată cu exemple (`BO-betting-curiosity-image-v3` etc.)
- [x] Triggere automate per canal documentate
- [x] Template brief automat complet
- [x] Referință M-129 inclusă în Step 5 (inspirație brief)
- [x] Benchmarks longevitate per GEO documentate în Step 6
- [x] Cortex JSON schema completă
- [x] 5 VIOLATION declarations clare
- [x] 6 VERIFY checks obligatorii
- [x] MODEL ROUTING completat (Opus vs Sonnet per task)
- [x] Tabel dependențe complet (M-119, M-122, M-125, M-129)
- [x] Tabel metrici complet cu target/warning/critical
- [x] Toate secțiunile META-H-002 prezente (Scope + S1 + S2 + S3 + S4 + S5 + S6 + Checklist)
- [x] Limbă: română (termeni tehnici în engleză OK)
- [x] Status: ACTIVE
