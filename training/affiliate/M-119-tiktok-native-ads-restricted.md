---
type: procedure
created: 2026-03-17
status: active
slug: m-119-tiktok-native-ads-restricted
tags: [procedure, nexus]
---

# TikTok + Native Ads for Restricted Verticals — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Strategie de advertising pe TikTok Ads si platforme native (Taboola, Outbrain, MGID) pentru verticals restrictionate (gambling, finance) in piete internationale (Peru, Kenya, Chile), cu channel selection matrix, compliance per canal, budget allocation cu benchmarks concrete, si optimization cycle structurat.

---

## 1. Problema

M-009 acopera doar Facebook Ads — care are restrictii severe pe gambling (ban risk crescut, approval 72h+, CPM inflat $3.5-5). TikTok a depasit Meta pentru Kenya + Chile in 2026 pe cost-efficiency. Native ads (Taboola, Outbrain, MGID) au CPCs de $0.02-0.05 in Kenya/Peru, fara restrictiile gambling ale Meta. Absenta completa din proceduri = dependenta pe un singur canal restrictiv, CPM-uri 2-3x mai mari, si zero diversificare risc.

**Situatii acoperite:**
- Setup campanie TikTok Ads gambling/finance pe GEO specific
- Setup campanie native ads (Taboola/Outbrain/MGID) cu whitelist gambling
- Creative compliance pentru canale cu restrictii per vertical
- Scaling cross-channel (TikTok + native simultan)
- Channel selection optimala per GEO pe baza CPM/CPC benchmarks
- Budget allocation si bid strategy per canal per GEO

**Ce se intampla fara procedura:**
- Dependenta exclusiva pe Meta Ads → ban risk crescut pe gambling, CPM-uri 2-3x mai mari
- TikTok Ads ignorat → pierdere acces la audienta 18-34 in emerging markets (70%+ reach Kenya/Chile)
- Native ads ignorate → pierdere CPC-uri $0.02-0.05 pe piete cu volum mare
- Creative non-compliant → cont suspendat, pierdere historical data
- Zero diversificare canale → single point of failure per GEO

## 2. Procedura

### Pas 1: CHANNEL SELECTION PER GEO

Completeaza matricea GEO x Canal OBLIGATORIE inainte de orice setup. Status per canal = DISPONIBIL / RESTRICTIONAT (cu whitelist) / BLOCAT.

**Matrice GEO x Canal x Status cu Benchmarks:**

| GEO | TikTok Ads | Taboola | Outbrain | MGID | Meta (referinta) |
|-----|-----------|---------|----------|------|------------------|
| **Peru (PE)** | RESTRICTIONAT — whitelist gambling necesar; CPM $1.5-2.5; CTR 3.2% | DISPONIBIL — gambling permis cu disclaimer; CPC $0.03-0.06 | DISPONIBIL — gambling permis; CPC $0.04-0.07 | DISPONIBIL — gambling permis; CPC $0.02-0.04 | RESTRICTIONAT — ban risk ridicat; CPM $3.5-5; CTR 1.0% |
| **Kenya (KE)** | DISPONIBIL — gambling permis cu disclaimer BCLB; CPM $1.0-1.8; CTR 4.1% | DISPONIBIL — gambling permis; CPC $0.02-0.04 | RESTRICTIONAT — coverage limitat Africa; CPC $0.05-0.08 | DISPONIBIL — strong Africa coverage; CPC $0.02-0.03 | RESTRICTIONAT — gambling restrictionat; CPM $2.5-4; CTR 1.2% |
| **Chile (CL)** | DISPONIBIL — gambling nou permis 2025; CPM $1.2-2.0; CTR 3.8% | DISPONIBIL — gambling permis; CPC $0.04-0.07 | DISPONIBIL — gambling permis; CPC $0.05-0.08 | DISPONIBIL — gambling permis; CPC $0.03-0.05 | RESTRICTIONAT — ban risk; CPM $3.0-4.5; CTR 1.1% |

**Regula selectie canal**: Minimum 2 canale non-Meta active per GEO. Prioritizare pe baza: (1) CPM/CPC cel mai mic, (2) CTR cel mai mare, (3) restrictii cele mai putine.

**Channel Priority Ranking (per GEO):**
- **Peru**: 1. MGID (CPC $0.02) → 2. TikTok (CPM $1.5, whitelist) → 3. Taboola (CPC $0.03)
- **Kenya**: 1. TikTok (CPM $1.0, CTR 4.1%) → 2. MGID (CPC $0.02) → 3. Taboola (CPC $0.02)
- **Chile**: 1. TikTok (CPM $1.2, CTR 3.8%) → 2. MGID (CPC $0.03) → 3. Taboola (CPC $0.04)

**Tool**: Claude Opus — research disponibilitate canale + benchmarks CPM/CPC per GEO; M-114 (GEO scoring input).
**Output**: Document `channel-matrix-{GEO}.md` cu matrice completa + priority ranking + benchmarks.
**Checkpoint**: Matrice completata cu status per canal per GEO. Minimum 2 canale non-Meta cu status DISPONIBIL identificate. Benchmarks CPM/CPC documentate cu sursa.

### Pas 2: TIKTOK ADS SETUP

Setup complet TikTok Ads Manager per GEO:

**(a) Account Setup:**
- Business Center cu business verification completata
- Timezone si moneda setate per GEO (PEN/KES/CLP)
- Gambling whitelist application (daca RESTRICTIONAT) — include: LP referinta cu disclaimer, licenta operator, business documentation

**(b) Pixel & Tracking:**
- TikTok Pixel install pe LP (server-side preferat via Events API per M-121)
- Event mapping: ViewContent → Lead → CompleteRegistration → Purchase (FTD)
- Postback integration cu Voluum (M-121)
- Test mode: 3 test conversions confirmate

**(c) Audience Setup:**
- Interest targeting: Sports, Entertainment, Games (nu "Gambling" direct)
- Custom audiences: website visitors (pixel), customer lists (hashed)
- Lookalike audiences: bazate pe FTD converters (dupa 50+ events)
- Exclusion: sub 18, narrow interest "children/education"

**(d) Gambling-Specific Creative Rules:**
- Nu cuvinte directe ("casino", "bet", "gamble") in ad copy
- Foloseste derivate localizate: "apuestas deportivas" (PE/CL), "sports predictions" (KE)
- Video format: 15-30s, vertical 9:16, hook primele 3 secunde cu referinta culturala locala (M-116)
- Disclaimer overlay: ultimele 3 secunde, text conform regulator local (M-118)
- Zero cash imagery, zero promisiuni castig

**Tool**: TikTok Ads Manager, Voluum (tracking), M-116 (creative localizate), M-121 (server-side tracking).
**Output**: Screenshot `tiktok-setup-{GEO}.png` cu cont activ + pixel functional + 3 test conversions.
**Checkpoint**: Cont verificat + pixel live + test conversions confirmed. Gambling whitelist approved (daca aplicabil). Creative-uri respecta toate regulile gambling-specific.

### Pas 3: NATIVE ADS SETUP

Setup campanii native advertising (Taboola, Outbrain, MGID):

**(a) Account & Approval:**
- Cont creat pe platforma selectata per GEO (din matrice Pas 1)
- Gambling vertical whitelist: aplica cu LP referinta + disclaimer + licenta operator
- Approval timeline: Taboola 3-5 zile, Outbrain 5-7 zile, MGID 1-3 zile

**(b) Campaign Setup:**
- Objective: Clicks (faza testare) → Conversions (dupa 50+ events)
- Targeting: GEO + Device (mobile dominant) + OS + Browser language
- Publisher exclusions: block list news/children/education sites

**(c) Landing Page Intermediara (OBLIGATORIE):**
- Nu direct link la operator — native ads necesita pre-sell page format articol
- Structura: hook editorial → informatii sportive/entertainment → CTA natural catre operator
- Disclaimer regulator local vizibil above fold
- Tracking: LP intermediara → operator LP (double redirect via Voluum)

**(d) Native-Specific Creative Rules:**
- Thumbnail: lifestyle imagery (fans sportivi, ecrane telefon), nu cash/coins imagery
- Headline: curiosity gap controlat ("Cum fac fanii din Lima bani din cunostintele lor sportive" — nu misleading, dar engaging)
- Zero promisiuni castiguri garantate in headline sau thumbnail text
- Disclaimer vizibil pe LP intermediara

**Tool**: Taboola/Outbrain/MGID dashboard, Voluum (tracking), Claude Opus (LP intermediara copy).
**Output**: Document `native-setup-{platforma}-{GEO}.md` cu cont activ + campanie draft + LP intermediara URL.
**Checkpoint**: Cont aprobat pe gambling whitelist. LP intermediara live cu disclaimer vizibil. Tracking double redirect functional (testat cu test click).

### Pas 4: CREATIVE COMPLIANCE PER CANAL

Verifica compliance TOATE creative-urile per canal per GEO inainte de lansare:

**TikTok Compliance Checklist (per GEO):**

| # | Criteriu | Peru (ASJ) | Kenya (BCLB) | Chile (SJC) |
|---|----------|------------|--------------|-------------|
| 1 | Zero cash/money imagery | Obligatoriu | Obligatoriu | Obligatoriu |
| 2 | Zero promisiuni castig garantat | Obligatoriu | Obligatoriu | Obligatoriu |
| 3 | Disclaimer 3s overlay final | "Juega responsablemente. Solo +18. ASJ." | "Gamble responsibly. 18+. Licensed by BCLB." | "Juega con responsabilidad. +18. SJC." |
| 4 | Zero underage imagery | Obligatoriu | Obligatoriu | Obligatoriu |
| 5 | Nu cuvinte interzise in copy | "casino", "bet" → "apuestas deportivas" | "casino", "bet" → "sports predictions" | "casino", "bet" → "apuestas en linea" |
| 6 | Responsible gambling mention | "juego responsable" | "responsible gambling" | "juego responsable" |

**Native Ads Compliance Checklist:**

| # | Criteriu | Detaliu |
|---|----------|---------|
| 1 | Pre-sell page obligatorie | Articol-format, nu direct link operator |
| 2 | Disclaimer vizibil above fold | Text conform regulator GEO |
| 3 | Zero misleading headlines | Curiosity gap DA, promisiuni false NU |
| 4 | Thumbnail fara cash imagery | Lifestyle/sport imagery doar |
| 5 | Tracking transparent | Double redirect via Voluum, no cloaking |
| 6 | Responsible gambling on LP | Link sau text "juego responsable" / "gamble responsibly" |

**Tool**: Claude Opus — compliance audit creative-uri; M-118 (compliance framework 10-punct).
**Output**: Document `creative-compliance-{canal}-{GEO}.md` cu checklist completat per creative.
**Checkpoint**: 6/6 per TikTok + 6/6 per Native = GO. Sub 6/6 pe orice canal = BLOCKER, revenire la creative production (M-116 Pas 3).

### Pas 5: BUDGET ALLOCATION + BID STRATEGY

Aloca budget pe baza benchmarks din matrice Pas 1:

**TikTok Budget & Bid:**

| GEO | Budget start/zi | Bid strategy | CPA target | Adsets paralele |
|-----|----------------|-------------|------------|-----------------|
| Peru | $30-50/adset | Manual CPA bid (primele 7 zile), apoi auto-optimize | $8-15 per FTD | 3-5 cu creative diferite |
| Kenya | $20-40/adset | Manual CPA bid (primele 7 zile) | $5-10 per FTD | 3-5 cu creative diferite |
| Chile | $30-50/adset | Manual CPA bid (primele 7 zile) | $8-12 per FTD | 3-5 cu creative diferite |

**Native Budget & Bid:**

| GEO | Budget start/zi | CPC bid | CPA target estimat | Headlines test |
|-----|----------------|---------|-------------------|----------------|
| Peru | $20-30/campanie | $0.03-0.06 (Taboola), $0.02-0.04 (MGID) | $10-20 per FTD | 5 variante |
| Kenya | $15-25/campanie | $0.02-0.04 (Taboola), $0.02-0.03 (MGID) | $5-12 per FTD | 5 variante |
| Chile | $20-30/campanie | $0.04-0.07 (Taboola), $0.03-0.05 (MGID) | $8-15 per FTD | 5 variante |

**Split Budget Rule:**
- Daca ambele canale disponibile per GEO: 60% pe canal cu CPM/CPC mai mic, 40% pe celalalt
- Dupa 7 zile date: re-alocare pe baza CPA actual (shift budget catre canal cu CPA mai mic)
- Escalare: +50% budget pe creative castigatoare dupa 3 zile consecutive cu ROI > 1.5x

**Tool**: TikTok Ads Manager, Taboola/Outbrain/MGID dashboard, Voluum (tracking ROI).
**Output**: Document `budget-allocation-{GEO}.md` cu split per canal, bid strategy, si CPA targets.
**Checkpoint**: Budget alocat per canal conform benchmarks. Manual CPA bid setat pe TikTok (nu auto-bid in primele 7 zile). Minimum 5 headline variante per campanie nativa.

### Pas 6: OPTIMIZATION CYCLE

Ciclu de optimizare structurat pe 3 niveluri temporale:

**Zilnic (primele 14 zile):**
- Verifica CR, CPA, ROI per creative per canal
- Kill creative cu CPA > 2x target dupa 200 clicks — nu inainte (insufficient data)
- Kill adset TikTok cu CTR < 1% dupa 1000 impressions
- Kill headline nativ cu CTR < 0.5% dupa 2000 impressions
- Identifica top 3 creative-uri per canal

**Saptamanal:**
- Shift budget intre canale pe baza ROI comparat (canalul cu CPA mai mic primeste +20% din celalalt)
- Adauga minimum 2 creative-uri noi per canal (combate creative fatigue)
- Analiza device split (mobile vs desktop) — realoca budget pe device dominant
- Test noi audiente TikTok (expand interests, noi lookalike segments)

**Lunar:**
- Refresh complet creative set — creative fatigue la 30-45 zile pe TikTok, 45-60 zile pe native
- Re-evaluare channel matrix (CPM-uri fluctueaza sezonier)
- Compliance re-check pe toate materialele active (M-118 Pas 5 re-audit)
- Report: CPA per canal, ROI per canal, spend total, conversii totale

**Tool**: Voluum (performance data), TikTok Ads Manager, Taboola/Outbrain/MGID, Claude Opus (analysis + new creative briefs).
**Output**: Document `optimization-report-{GEO}-{saptamana/luna}.md` cu actiuni luate + rezultate.
**Checkpoint**: Zero creative active cu CPA > 2x target dupa 200 clicks. Minimum 2 creative noi/saptamana. Refresh complet la 30 zile TikTok / 45 zile native.

### Pas 7: SCALE + CROSS-CHANNEL

Dupa identificare creative castigatoare (CPA < 1.5x target + minimum 500 clicks):

**(a) Scalare Verticala:**
- Budget +50% la 48h intervals (nu +100% dintr-o data — algoritmul destabilizat)
- Monitor CPA dupa fiecare increment — daca CPA creste > 20%, pause increment 48h

**(b) Scalare Orizontala:**
- Creative castigatoare TikTok → adaptare pentru native (video → thumbnail + headline)
- Creative castigatoare native → adaptare pentru TikTok (articol → video script 15s)
- Cross-reference M-116 pentru adaptare culturala per canal

**(c) Audience Expansion:**
- TikTok: Lookalike audiences bazate pe FTD converters (1%, 3%, 5%)
- Native: Broader targeting — remove publisher restrictions pe top performers
- Retargeting cross-channel: TikTok visitors → native retarget (si invers)

**(d) GEO Expansion:**
- Creative castigatoare → adaptare pentru noi GEO-uri via M-116 (localization)
- Compliance check pe noul GEO via M-118 (OBLIGATORIU inainte de lansare)
- Channel matrix actualizata per noul GEO (Pas 1 re-run)

**Tool**: TikTok Ads Manager, Taboola/Outbrain/MGID, Voluum, Claude Opus (creative adaptation), M-116 (localization), M-118 (compliance noul GEO).
**Output**: Document `scale-plan-{GEO}.md` cu scaling steps + timeline + budget proiectii.
**Checkpoint**: Scalare verticala fara CPA increase > 20%. Minimum 1 creative adaptat cross-channel per saptamana. Zero GEO-uri noi fara M-118 compliance check complet.

## 3. Cortex Logging

```json
{
  "text": "M-119 TikTok + Native Ads for Restricted Verticals — Channel selection matrix, TikTok + native setup, creative compliance, budget allocation cu CPM/CPC benchmarks, optimization cycle structurat per GEO",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-119",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "paid-advertising-alternative",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "tiktok", "native-ads", "taboola", "outbrain", "mgid", "gambling", "restricted-verticals", "training", "Peru", "Kenya", "Chile"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- La lansare orice campanie non-Meta pe vertical restrictionat (gambling, finance)
- La optimization cycle (zilnic/saptamanal/lunar)
- La scaling cross-channel sau GEO expansion
- La refresh creative set

### WHEN
- La fiecare campanie TikTok/native noua — Pas 1-5 complet
- La fiecare optimization cycle — Pas 6 conform cadenta (zilnic/saptamanal/lunar)
- La fiecare scaling decision — Pas 7 complet
- Lunar: re-validare channel matrix (Pas 1) si compliance (Pas 4)
- La schimbare politici TikTok Ads sau native platforms — re-audit imediat

### HOW (violation detection)
- Campanie doar pe Meta fara check canale alternative (Pas 1) → **WARNING** (matrice canal obligatorie)
- Creative fara compliance check Pas 4 (6/6) → **VIOLATION CRITICA** (retragere creative, risk cont suspendat)
- Budget alocat fara benchmarks CPM/CPC per GEO → **VIOLATION** (re-alocare obligatorie cu date)
- Native ads cu link direct la operator (fara pre-sell page) → **VIOLATION** (cont suspendat guaranteed)
- TikTok cu cuvinte interzise in copy ("casino", "bet") → **VIOLATION CRITICA** (cont suspendat)
- Creative active peste 45 zile fara refresh → **WARNING** (creative fatigue, performance degradation)
- Scalare > +100% budget intr-o zi → **VIOLATION** (algoritm destabilizat, CPA spike)
- GEO expansion fara M-118 compliance check → **VIOLATION CRITICA** (blocker absolut)

### CONNECT
- M-009 (Facebook Ads) — complementar, nu inlocuitor; M-119 diversifica canalele
- M-116 (Localization Engine) — creative localizate cultural per GEO la Pas 2d, 3d, 7b
- M-117 (Competitive Intelligence) — benchmark competitori pe TikTok/native per GEO
- M-118 (Gambling Compliance) — compliance framework 10-punct, obligatoriu pre-lansare
- M-008 (A/B Testing Framework) — test matrix aplicabil cross-channel
- M-114 (GEO Scoring) — prioritizare piete, input channel matrix
- M-120 (WhatsApp/SMS Funnel) — funnel post-click pentru traffic TikTok/native
- M-121 (Server-Side Tracking) — tracking setup TikTok Pixel + native postbacks

### VERIFY
1. Channel matrix GEO x Canal completata cu status si benchmarks CPM/CPC — verificare document `channel-matrix-{GEO}.md` existent si datat < 30 zile
2. Compliance checklist 6/6 per canal per GEO documentat — verificare document `creative-compliance-{canal}-{GEO}.md`
3. Budget allocation conform benchmarks cu split rule documentat — verificare document `budget-allocation-{GEO}.md`
4. Optimization report existent conform cadenta (zilnic/saptamanal/lunar) — verificare documente `optimization-report-{GEO}-*.md`
5. Zero creative active cu CPA > 2x target dupa 200 clicks — cross-check Voluum reports
6. Refresh creative set la max 45 zile TikTok / 60 zile native — verificare creation dates creative-uri active

### MODEL ROUTING
- **Claude Opus**: Creative generation gambling-compliant, compliance audit, channel analysis, optimization strategy, scaling decisions (complex reasoning + compliance nuance)
- **Claude Sonnet**: Budget tracking, performance reporting, headline variations, quick compliance checks (cost optimization)

## 5. Dependente

- **Tools**: TikTok Ads Manager, Taboola, Outbrain, MGID, Voluum, Claude Opus, Meta Ad Library (competitor research)
- **Proceduri input**: M-114 (GEO scoring — piete prioritare), M-118 (compliance framework — obligatoriu pre-lansare)
- **Proceduri framework**: M-008 (A/B testing), M-009 (Facebook Ads — complementar), M-121 (server-side tracking)
- **Proceduri adjacent**: M-116 (localization engine — creative localizate), M-117 (competitive intelligence), M-120 (WhatsApp/SMS funnel)
- **Data sources**: TikTok Ads benchmarks per GEO, Taboola/MGID industry reports, Voluum performance data
- **Output consumed by**: M-116 (creative TikTok/native localizate), M-117 (benchmark canale alternative), M-121 (tracking config per canal)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| CPA per canal per GEO | < 2x target (PE: $15, KE: $10, CL: $12) | Zilnic (primele 14 zile), apoi saptamanal | Voluum |
| ROI per campanie | Pozitiv (>1.0x) in 14 zile | Saptamanal | Voluum + platform dashboards |
| Canale non-Meta active per GEO | Min 2 | Lunar | Platform dashboards |
| CTR TikTok | >3% (PE), >4% (KE), >3.5% (CL) | Saptamanal | TikTok Ads Manager |
| CPC Native | <$0.06 (PE), <$0.04 (KE), <$0.07 (CL) | Saptamanal | Taboola/Outbrain/MGID |
| Creative refresh cycle | TikTok: 30 zile, Native: 45 zile | Lunar | Creative creation dates |
| Compliance violations | 0 per campanie | Per campanie | Compliance audit docs |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential (1-7) cu Tool, Output (artifact name), si Checkpoint (criteriu masurabil) per pas
- [x] Tools specificate per pas (nu doar lista generica)
- [x] KPIs masurabile cu target numeric, frecventa, si sursa (7 metrici)
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Dependente listate (tools + proceduri input/framework/adjacent + data sources + output consumed by)
- [x] Metrics cu target, frecventa si sursa masurare
- [x] Channel selection MATRIX GEO x Canal cu benchmarks CPM/CPC concrete per GEO (Pas 1)
- [x] Compliance checklists per canal per GEO cu criteriu specific (Pas 4)
- [x] Budget allocation cu benchmarks concrete per GEO per canal (Pas 5)
- [x] Cross-references validate (M-008, M-009, M-114, M-116, M-117, M-118, M-120, M-121)
