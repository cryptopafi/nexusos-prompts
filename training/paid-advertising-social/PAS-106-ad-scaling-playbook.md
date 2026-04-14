---
id: PAS-106
title: Ad Scaling Playbook (Horizontal + Vertical)
domain: paid-advertising-social
source: TikTok Ads Mastery — Scaling Strategy
version: 2.0
forge_score: "estimated M/3.5"
---

# Ad Scaling Playbook (Horizontal + Vertical)

## Obiectiv
Playbook structurat pentru scalarea de la €50/zi la €500+/zi fără a distruge ROAS, cu scaling vertical (budget increases max 20-30%/3 zile), horizontal (duplicare structuri cu variații), TikTok-specific tactics, și diagnostic/recovery când scalarea merge prost.

## Pași

### 1. Pre-requisite pentru scaling — green lights
NU scala fără toate condițiile bifate:
- [ ] CPA sub target minim 3 zile consecutive
- [ ] ROAS > 2x (ideal 3x+) minim 3 zile
- [ ] Pixel/event tracking funcționează (Events Manager verified)
- [ ] Landing page CVR > 2% (altfel scalezi waste)
- [ ] Creative pool: minim 5 creativeuri funcționale (nu 1 singur ad)
- [ ] Audience size: minim 10x bugetul zilnic (500K pentru €50/zi)
- [ ] Learning phase completă (50+ conversii/săptămână per AdSet)

### 2. Scaling vertical — budget increase rules
**Regula de aur**: max +20-30% la 3-4 zile. NEVER >30% per day.
```
Ziua 1-3:  €50/zi (baseline stabil)
Ziua 4-6:  €65/zi (+30%)
Ziua 7-9:  €85/zi (+30%)
Ziua 10-12: €110/zi (+30%)
Ziua 13-15: €145/zi (+30%)
Ziua 16-18: €190/zi (+30%)
...
→ €500/zi în ~6 săptămâni cu 0 reset learning phase
```

De ce nu poți dubla: Meta/TikTok intră în re-learning la >50% increase → CPA crește → panic stop prematur.

**CBO Automated Rule** (exception): configurează regula automată +20%/3 zile dacă CPA < target → hands-off scaling.

### 3. Scaling horizontal — multiplicare structuri
**Metodă A — Duplicate cu audiențe noi**:
- AdSet câștigător → Duplicate → schimbă audiența (alt interes, alt LAL %)
- Aceeași reclamă câștigătoare pe audiență nouă
- Budget: egal cu originalul la lansare

**Metodă B — Duplicate cu creative noi**:
- Audiența câștigătoare → Duplicate AdSet → creative noi
- Testează noi creativeuri pe audiență dovedită

**Metodă C — Expansion geografică**:
- Funcționează în RO → duplică pentru MD, BG, HU (piețe similare)
- Sau diaspora RO (UK, IT, DE, ES) cu reclama în română

**Metodă D — Campanie nouă separată**:
- Reset algoritm — uneori aduce performanță mai bună pe audiența identică

### 4. Scaling TikTok — specificități vs Meta
- Creative fatigue mai rapid pe TikTok (audiența sensibilă la repetiție)
- Soluție: "Creative Rotation" — 5-7 creativeuri active simultan
- Evită "Accelerated Delivery" (consumă budget rapid, instabil)
- "Smart Creative Optimization": TikTok combină elemente automat — bun la scale
- Bidding: la scale mare, treci de la Lowest Cost la Cost Cap pentru CPA stabil
- CPM TikTok ($5-10) < Meta ($8-15) — TikTok mai ieftin dar fatigue mai rapid

### 5. Budget allocation formulas la scale
```
Scale €500/zi:
├── 70% Proven winners (campanii cu ROAS > 2x demonstrat)
├── 20% Testing (noi creative, noi audiențe, noi geos)
└── 10% Experimental (noi platforme, noi formate)

Per vertical:
├── Retargeting: 20-30% (highest ROAS, smallest audience)
├── Lookalike: 30-40% (scalable, decent ROAS)
└── Broad/Interest: 30-40% (highest scale potential)
```

### 6. Diagnostic — semne că scalarea a mers prost
**CPA explodat 2-3x**: STOP scaling → verifică overlap, pixel, fatigue → revert la buget anterior, pauzează 24h
**ROAS scade progresiv**: audiența saturată → expand audiență, LAL din surse noi
**CTR scade >40% vs peak**: creative fatigue → înlocuiește creative, NU crește bugetul
**Costuri cresc, volum stagnează**: CPM crescut (competiție) → expand audiențe noi, schimbă bid strategy
**Learning phase resetată repetat**: bugetul crește prea rapid → revert la +20%/3 zile

### 7. Automated rules pentru scaling hands-off
**Meta Ads Manager → Automated Rules**:
- IF CPA < target AND ROAS > 2x FOR 3 days THEN increase budget +20%
- IF CPA > 2x target FOR 3 days THEN pause AdSet
- IF Frequency > 4 THEN reduce budget -20%
- Notification email la fiecare trigger

**TikTok**: Smart Creative Optimization + Cost Cap bidding = semi-automated scaling

### 8. CPM benchmarks la scale (2026)
| Budget Level | Meta CPM | TikTok CPM | Notes |
|-------------|----------|------------|-------|
| €50-100/zi | €8-12 | €5-8 | Normal |
| €200-500/zi | €10-18 | €6-12 | Competition increase |
| €500+/zi | €12-25 | €8-15 | Premium placement needed |
| Q4 (Nov-Dec) | +30-50% | +20-30% | Seasonal premium |

## Verificare
- [ ] Pre-requisite checklist complet bifat
- [ ] Plan scaling documentat (buget pe săptămâni, grafic)
- [ ] Creative pool: min 5-7 ad-uri funcționale pentru rotație
- [ ] Automated rules configurate (stop la CPA > 2x, scale la CPA < target)
- [ ] Horizontal expansion planned (audiențe noi, geos noi)
- [ ] Monitoring zilnic primele 30 zile de scaling
- [ ] Recovery plan documentat (ce faci la CPA explodat)
- [ ] Creative refresh calendarizat (la fiecare 2-3 săptămâni)

## Instrumente
- Meta Ads Manager → Automated Rules — scaling/stop automat
- TikTok Ads Manager → Smart Creative + Cost Cap
- Revealbot / Madgicx — automatizare avansată reguli
- Google Sheets — tracking zilnic buget/CPA/ROAS per campanie

## Note
- Scaling vertical fără creative refresh = ad fatigue garantat în 2-3 săptămâni
- CBO la €500+/zi Meta e esențial — redistribuire automată eficientă
- "Unicorn creative" = reclama care funcționează luni → mai valoroasă decât 10 care funcționează 2 săpt
- Never scale >30%/zi — regula cea mai importantă din tot playbook-ul
