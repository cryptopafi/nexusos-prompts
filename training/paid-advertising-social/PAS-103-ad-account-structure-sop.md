---
id: PAS-103
title: Ad Account Structure SOP (Campaign / AdSet / Ad)
domain: paid-advertising-social
source: TikTok Ads Mastery — Account Architecture
version: 2.0
forge_score: "estimated M/3.5"
---

# Ad Account Structure SOP (Campaign / AdSet / Ad)

## Obiectiv
Definirea structurii standard de organizare a unui cont de reclame social (Meta/TikTok) pe 3 niveluri, cu naming conventions, CBO vs ABO decision framework, budget allocation formulas, scaling rules și weekly audit checklist.

## Pași

### 1. Nivelul Campaign — obiective și naming convention
**Regula**: 1 campanie = 1 obiectiv de business. Nu amesteca obiective.

Obiective și când le folosești:
- **Awareness/Reach**: brand nou, audiență rece, CPM scăzut dorit
- **Traffic**: direcționare landing page, CTR optimization
- **Conversions/Sales**: standard e-commerce și lead gen — cel mai folosit
- **Lead Generation**: leads direct în platformă (Meta Lead Ads, TikTok Lead Gen)

**Naming convention**: `[Brand]_[Obiectiv]_[Piață]_[CBO/ABO]_[Data]`
Exemplu: `Albastru_Conv_RO_ABO_2026-03-20`

### 2. CBO vs ABO — decision framework
**ABO (Ad Set Budget Optimization)** — recomandat pentru:
- Testing phase (primele 30 zile pe un cont nou)
- Control granular per audiență
- Bugete mici (sub €100/zi)
- Când vrei să vezi exact ce audiență funcționează
- Budget: setat la AdSet level manual

**CBO (Campaign Budget Optimization)** — recomandat pentru:
- Scaling phase (€200+/zi)
- Conturi cu pixel matur (50+ conversii/săptămână)
- Lasă Meta/TikTok să redistribuie automat bugetul
- Mai eficient la scale — algoritmul optimizează în timp real

**Regula simplă**: start cu ABO → testing → identifici câștigătorii → treci la CBO pentru scaling.

### 3. Nivelul AdSet/Ad Group — structura de testing
**Regula**: 1 AdSet = 1 audiență distinctă.
```
Campaign: Albastru_Conv_RO_ABO
├── AdSet 01: Retargeting — Website Visitors 30d [€15/zi]
├── AdSet 02: LAL 1% — Purchasers [€20/zi]
├── AdSet 03: Interest — Cooking/Food Broad [€15/zi]
├── AdSet 04: Interest — Agritourism/Rural Tourism [€15/zi]
└── AdSet 05: Custom — Email List Upload [€10/zi]
```

**Naming**: `[Tip]_[Detaliu]_[Budget/Day]` → `LAL1%_Purchasers_€20d`

**Setări critice AdSet**:
- Bid Strategy: Lowest Cost (testing) → Cost Cap (scaling)
- Scheduling: "Run on schedule" pentru orele cu ROI testat
- Learning phase: min 50 conversii/săptămână per AdSet pentru stabilitate

### 4. Nivelul Ad — organizarea creativelor
**Regula**: minim 3 ad-uri per AdSet activ (algoritmul optimizează între ele).
```
AdSet 01: Retargeting
├── Ad A: Video testimonial 30sec
├── Ad B: Carousel produse + prețuri
└── Ad C: Static image ofertă clară
```

**Naming**: `[Format]_[Hook/Concept]_[Versiune]` → `Vid_CookingHook_v2`

**Creative rotation**: Frequency > 3 pe cold audiences → ad fatigue → refresh creative

### 5. Budget allocation framework (70/20/10)
```
Budget total lunar: €1.000 (exemplu)

Split recomandat:
├── 70% Proven (Conversions/Sales campaigns — generates revenue)
├── 20% Testing (noi audiențe, noi creative concepts)
└── 10% Experimental (noi platforme, noi formate, cold expansion)

Per campanie Conversions:
├── Retargeting AdSets: €8-15/zi (highest ROAS, smallest audience)
├── Lookalike AdSets: €10-20/zi per AdSet
└── Broad Interest AdSets: €10-15/zi per AdSet
```

### 6. Scaling rules (CRITICAL — nu ignora)
**Nu schimba nimic în primele 72h** — algoritmul e în learning phase.

**Scale signals (crește buget)**:
- CPA sub target 3+ zile consecutive → scale
- ROAS > 2x target 3+ zile → scale
- Frequency sub 2.5 → audience not exhausted

**Scale method**: crește bugetul cu max 20-30% la 3-4 zile. NEVER >30%/zi.
- €50 → €65 (+30%, ziua 4) → €85 (ziua 7) → €110 (ziua 10)

**Stop signals (pauzează)**:
- CPA > 2x target 3+ zile → pause
- Frequency > 4 fără refresh creative → fatigue
- CTR drop >50% vs peak → creative dying

### 7. CPM benchmarks per platformă (2026)
| Platformă | CPM (€) | Best for |
|-----------|---------|----------|
| Meta (FB/IG) | €8-15 | Conversions, retargeting |
| TikTok | €5-10 | Awareness, discovery |
| Snapchat | €3-7 | Gen Z, AR filters |
| Pinterest | €5-12 | Shopping, inspiration |

### 8. Weekly audit checklist
- [ ] Frequency per AdSet verificat (cold <3.0, warm <5.0)
- [ ] CPA trend: ultima săptămână vs anterioară (growing? dropping?)
- [ ] Budget pace: se consumă uniform? (accelerated = potențial waste)
- [ ] Learning phase: AdSets sub 50 conv/săpt → consolidate sau increase budget
- [ ] Creative fatigue: CTR drop >30% vs peak → refresh urgent
- [ ] Overlap check: AdSeturi cu >30% overlap → consolidează sau exclude
- [ ] Top performer: care AdSet are cel mai mic CPA? Scale it.
- [ ] Bottom performer: care AdSet are cel mai mare CPA? Kill it.

## Verificare
- [ ] Naming conventions implementate consistent în tot contul
- [ ] Max 1 obiectiv per campanie (nu mix)
- [ ] CBO vs ABO decision documentată per campanie
- [ ] Minim 3 ad-uri per AdSet activ
- [ ] Budget alocat 70/20/10 framework
- [ ] Learning phase respectată (no edits primele 72h)
- [ ] Scaling rules documentate (max +20-30% la 3 zile)
- [ ] Weekly audit calendarizat (ziua fixă)

## Instrumente
- Meta Ads Manager — structurare și management
- TikTok Ads Manager — structurare echivalentă
- Google Sheets — tracking buget și performanță
- Madgicx / Revealbot — automatizare reguli scalare/stop
- Meta Audience Overlap Tool — verificare suprapuneri

## Note
- CBO la €500+/zi pe Meta devine esențial — algoritmul redistribuie mai eficient
- TikTok Ad Groups = Meta AdSets — logica identică, interfața diferă
- Conturi noi: structura minimă (1 campanie, 2-3 AdSets, 3 ads) → complică treptat
- Budget minim per AdSet pentru date semnificative: €10-15/zi
