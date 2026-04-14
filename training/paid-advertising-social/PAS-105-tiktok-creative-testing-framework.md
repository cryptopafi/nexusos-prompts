---
id: PAS-105
title: TikTok Creative Testing Framework
domain: paid-advertising-social
source: TikTok Ads Mastery — Testing & Optimization
version: 2.0
forge_score: "estimated M/3.5"
---

# TikTok Creative Testing Framework

## Obiectiv
Sistem structurat de testare creative TikTok cu DCT (Dynamic Creative Testing) 3x3 matrix, KPIs clare per fază, decision tree scalare/oprire, Creative Winners Library și cadență de producție pentru identificarea și scalarea câștigătorilor în 7-14 zile.

## Pași

### 1. Principii de testare — regula variabilei unice
**Regula fundamentală**: testează O singură variabilă odată.
- Nu schimba hook-ul ȘI CTA-ul simultan — nu știi ce a cauzat diferența
- Impact ranking variabile TikTok:
  1. **Hook** (primele 3 sec) — cel mai mare impact pe performance
  2. **Oferta/mesaj** principal — al doilea
  3. **Format** (UGC vs branded vs text-on-screen) — al treilea
  4. **CTA** ("Cumpără" vs "Află" vs "Link bio") — al patrulea
  5. **Lungime** (15s vs 30s vs 60s) — al cincilea

### 2. DCT (Dynamic Creative Testing) — 3x3 Matrix
**Framework**: 3 hooks x 3 bodies = 9 variante testate simultan
```
         Body A (demo)    Body B (testimonial)   Body C (educational)
Hook 1   V1: Question     V2: Question+Test      V3: Question+Edu
Hook 2   V4: Demo+Demo    V5: Demo+Test          V6: Demo+Edu
Hook 3   V7: Stats+Demo   V8: Stats+Test         V9: Stats+Edu
```

**Producție**: filmează 3 hook-uri (3 sec fiecare) + 3 bodies (12-27 sec) → combine în CapCut
**Buget DCT**: €10/zi per variantă x 9 = €90/zi (sau €50/zi cu Smart Creative Optimization)
**Timeline**: 72h evaluare hooks → 7 zile evaluare completă → scale winners

### 3. Setup campanie testing în TikTok Ads Manager
```
Campaign: [Brand]_TESTING_[Data]
  Obiectiv: Conversions (dacă pixel matur) sau Traffic (cont nou)
  Budget: €50-90/zi total

Ad Group 01: All Variants (Broad audience)
  Audiență: Broad (algoritmul distribuie)
  Budget: €50-90/zi cu Smart Creative Optimization

Ads (9 variante din 3x3 matrix):
  V1-V9 (generate din DCT matrix)
```

**TikTok A/B Test nativ** (alternativă mai rapidă):
- Campaign → A/B Test → variabilă de testat (Creative / Audience / Bid)
- TikTok distribuie 50/50 automat → declară câștigătorul

### 4. KPIs de evaluare — benchmarks TikTok 2026
**Hook Rate (primele 3 sec)** — cel mai important:
- Formula: (3s viewers / Impressions) x 100
- <25% = slab → kill | 25-40% = OK | 40%+ = excelent → scale
- Locație: Ads Manager → Columns → "Video Views 3s" / Impressions

**Watch Completion Rate (75%+ viewed)**:
- <15% = slab | 15-30% = OK | 30%+ = excelent
- Indică dacă body-ul (după hook) menține interesul

**CTR (Click-Through Rate)**:
- <0.5% = slab | 0.5-1.5% = mediu | 1.5%+ = bun | 3%+ = excelent

**CPR (Cost Per Result)**:
- Evaluabil DOAR după 50+ events — nu judeca după 10 clicuri

**CPM benchmarks**: TikTok $5-10 | Meta $8-15 | Snapchat $3-7

### 5. Decision tree — scalare sau oprire
```
După 72h + min €15/variantă:
├── Hook Rate > 35% AND CTR > 1%?
│   ├── DA → potențial winner → lasă 7 zile
│   └── NU → Hook Rate < 25%? → KILL, alt hook
│
După 7 zile + 50+ conversii:
├── CPA sub target?
│   ├── DA + ROAS > 2x → SCALE (+30% budget la 3 zile)
│   ├── DA + ROAS borderline → lasă încă 7 zile
│   └── NU, CPA > 2x target → KILL
│
Scale rules:
├── Duplicate winner la +20-30% budget (nu +50%!)
├── Max scaling: +30%/3 zile (never more)
└── Creative fatigue: CTR drop >30% → create variant pe baza winner-ului
```

### 6. Creative Winners Library (Notion / Google Sheets)
Creează și menține un repository cu winners:
```
Coloane:
- Ad ID | Hook type | Body type | Data lansare
- Peak Hook Rate | Peak CTR | Peak ROAS | CPA mediu
- Durata viață (zile active) | Data retired
- "De ce a funcționat" (note 1-2 propoziții)
- Status: Active / Paused / Retired / Evergreen
```
- **Evergreen winners**: creative-uri care rulează 6-12 luni fără fatigue — identifică-le și protejează-le
- Când un winner obosește → creează variantă pe baza sa (schimbă hook, păstrează body)

### 7. Cadență producție creative (per buget)
```
Buget mic (€20-100/zi):
  - 3-5 variante noi testate/săptămână
  - Refresh minor: la 3 săptămâni

Buget mediu (€100-500/zi):
  - 5-10 variante noi/săptămână
  - Refresh minor: săptămânal
  - UGC pipeline activ (3-5 creators)

Buget scaling (€500+/zi):
  - 10-20 variante noi/săptămână
  - "Evergreen library": 20+ active în rotație
  - Dedicated production team
```

### 8. UGC Testing Pipeline (complement DCT)
- Recrutează 5-10 utilizatori reali (Billo €30-100/video)
- Brief standard: "Filmează-te folosind produsul, 15-30s, vertical, natural"
- Testează ca Spark Ads → validează winner → scale
- UGC outperformează studio 2:1 pe TikTok
- Cost-effective: €150-500 pentru 5 UGC videos vs €2.000+ studio

## Verificare
- [ ] DCT 3x3 matrix definită (3 hooks x 3 bodies = 9 variante)
- [ ] Campanie testing setup corect (1 variabilă per test)
- [ ] Budget minim alocat: €10-15/zi per variantă (sub = date insuficiente)
- [ ] KPI tracking configurat: Hook Rate, Completion, CTR, CPA
- [ ] Decision tree scalare/oprire definit cu criterii clare
- [ ] Creative Winners Library creată (Notion sau Sheets)
- [ ] UGC pipeline: minim 3 creators recrutați
- [ ] Cadență producție definită per buget level

## Instrumente
- TikTok Ads Manager — campaign setup + A/B testing nativ
- TikTok Creative Center (ads.tiktok.com/creative) — research winners industrie
- CapCut — editare rapidă multiple variante (swap hooks)
- Google Sheets / Notion — Creative Winners Library + KPI tracking
- Foreplay.co — inspirație creative ads
- Billo / Insense — UGC creator marketplace

## Note
- Un hook puternic salvează un body mediocru; body excelent + hook slab = 0 views
- Budget minim testing credibil: €500/lună; sub €200/lună = date insuficiente
- TikTok CPM $5-10 vs Meta $8-15 — TikTok mai cost-effective pentru testing
- DCT 3x3 = 9 variante din 6 assets filmați — efficiency maximă
