---
type: procedure
created: 2026-03-20
status: active
slug: m-125-vox-taxonomy-segmentation
tags: [procedure, nexus]
---

# M-125 — VOX Taxonomy Segmentation Optimizer

**Domeniu**: affiliate | **Sub-domeniu**: taxonomy-segmentation
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Conexiuni**: M-122, M-123, M-124
**Trigger**: La fiecare campanie SMS nouă; review săptămânal per campanie activă

---

## Scope

Framework de segmentare a campaniilor SMS folosind taxonomia IAB pusă la dispoziție de VOX per operator GSM, pentru maximizarea relevanței mesajului față de audiență. M-125 produce pentru fiecare campanie: selecția de segmente IAB (primary / secondary / avoid), copy angle per segment, alocare buget SMS per segment, design A/B test, și sistem de tracking performance pe Binom per IAB segment. Target: minim +50% CTR față de blast nesequestat.

---

## Section 1 — Problema

SMSads trimite în prezent la FULL DATABASE: toți cei 2.4M contacte Bolivia, fără nicio segmentare după interesele declarate (categoriile IAB VOX). Această abordare de "blast total" generează pierderi masive de revenue.

**Date concrete care ilustrează problema**:

Mesaj de betting trimis la categorii nepotriv:
- Business&Industrial (100K) — utilizatori B2B, interes sport/gambling neglijabil
- Web Services (144K) — profil tehnic/developer, nu audienta tipica pentru casino/betting
- Business&Finance (116K) — profil investitor, nu consumator de entertainment

Estimare impact: un mesaj de betting care converteste la 1.5% CTR pe Arts&Entertainment (sports fans) converteste la 0.3% pe Business&Finance — diferenta de 5x pe același cost SMS.

**Calcul pierdere estimata (Bolivia, campanie MarathonBet)**:
- Blast total 2.4M: CTR mediu 0.6% (medie diluata de segmente irelevante)
- Targeted 800K segmente relevante (Arts&Ent + Tech + Software): CTR estimat 1.2%
- Clicks blast: 14,400 | Clicks targeted: 9,600
- Conversia la offer identica, dar: blast = 2.4M SMS cost | targeted = 800K SMS cost
- Revenue/1K SMS blast: $X | Revenue/1K SMS targeted: $X × 2-3x (cost redus, CR similar)
- **Multiplicator estimat al non-segmentarii: 3-5x pierdere eficienta**

**Ce rezolva M-125**:
- Elimina risipa de buget SMS pe segmente irelevante
- Permite copy personalizat per segment (mesaj diferit pentru sportivi vs. investitori)
- Creaza date granulare de performance per IAB segment → feedback loop pentru M-124
- Reduce costul per conversie prin cresterea CTR pe acelasi volum de SMS trimis

---

## Section 2 — Procedura

### Step 1 — VOX Category Audit per GEO

Documenteaza TOATE categoriile IAB disponibile per operator, cu dimensiune si coverage %. Solicita lista completa de la VOX inainte de orice campanie.

**Calcul Coverage %**:
```
Coverage % = Segment Size / Total GEO Volume × 100
```

**Category Audit Table — Bolivia (Viva) + Romania (Vodafone)**:

| IAB Category | Bolivia Size | Bolivia % | Romania Size | Romania % | Vertical Relevance (1-5)* |
|-------------|-------------|-----------|-------------|-----------|--------------------------|
| Technology | 693K | 28.9% | 981K | 4.9% | 4 (betting, apps) |
| Software | 523K | 21.8% | 1,000K | 5.0% | 4 (apps, tech offers) |
| Apps | 457K | 19.0% | TBD | TBD | 5 (app downloads direct) |
| Arts & Entertainment | 246K | 10.3% | TBD | TBD | 5 (betting, casino) |
| Web Services | 144K | 6.0% | TBD | TBD | 2 (nisa tehnica) |
| Travel | 130K | 5.4% | TBD | TBD | 3 (e-commerce adiacent) |
| Business & Finance | 116K | 4.8% | 2,100K | 10.5% | 5 (trading, forex) |
| Business & Industrial | 100K | 4.2% | 2,200K | 11.0% | 1 (irelevant majoritate oferte) |
| Finance & Insurance | N/A | N/A | 1,700K | 8.5% | 5 (trading, CFD) |
| Logistics | N/A | N/A | 1,700K | 8.5% | 1 (irelevant) |
| CPG | N/A | N/A | 1,400K | 7.0% | 3 (e-commerce, health) |
| Health | N/A | N/A | 1,200K | 6.0% | 5 (health offers direct) |
| (alte 25 categorii BO) | ~sum 2.4M | variabil | TBD | TBD | de evaluat per categorie |
| (alte 37 categorii RO) | N/A | N/A | ~sum 20M | variabil | de evaluat per campanie |

*Vertical Relevance (1-5): 1 = evita, 3 = secundar, 5 = primar

**Instructiuni actualizare**:
- La fiecare campanie noua: verifica daca VOX a adaugat categorii noi
- Myanmar: solicita lista completa de la VOX (in prezent doar Finance 1.85M confirmat)
- Marcheaza cu TBD orice categorie neconfirmata oficial de VOX

### Step 2 — Offer-to-Segment Matching

Pentru fiecare oferta activa, defineste clar: segmente PRIMARY (trimite obligatoriu), SECONDARY (trimite daca buget permite), AVOID (nu trimite niciodata — risipa garantata).

**Logica de matching**:
- PRIMARY: overlap direct intre interesul IAB si produsul oferit (sports fan → betting)
- SECONDARY: overlap indirect (tech user → poate fi interesat de app/bonus)
- AVOID: profil divergent garantat (logistics B2B → casino offer = mismatch total)

**Offer-to-Segment Matching Table**:

| Oferta | GEO | Primary Segments | Secondary Segments | Avoid Segments | Expected CTR Uplift vs Untargeted |
|--------|-----|-----------------|-------------------|----------------|----------------------------------|
| MarathonBet | Bolivia | Arts&Entertainment (246K), Technology (693K) | Software (523K), Apps (457K) | Business&Industrial (100K), Web Services (144K), Business&Finance (116K), Logistics | +80-150% CTR estimat |
| AvaTrade ($250 CPA) | Bolivia | Business&Finance (116K) | Business&Industrial (100K), Software (523K) | Arts&Entertainment (246K), Travel (130K) | +100-200% CTR estimat |
| SpinBetter / Casino | Romania | Arts&Entertainment (TBD), Technology (981K) | Software (1M), Apps (TBD) | Business&Industrial (2.2M), Logistics (1.7M), Finance&Insurance (1.7M) | +60-120% CTR estimat |
| Trading offer (forex/CFD) | Romania | Business&Finance (2.1M), Finance&Insurance (1.7M) | Business&Industrial (2.2M), Software (1M) | Logistics (1.7M), CPG (1.4M), Arts&Entertainment | +120-200% CTR estimat |
| Health / Suplimente | Romania | Health (1.2M) | CPG (1.4M) | Business&Industrial (2.2M), Logistics (1.7M), Tech | +80-150% CTR estimat |
| App Install offer | Bolivia | Apps (457K), Software (523K), Technology (693K) | Web Services (144K) | Business&Industrial (100K), Travel (130K) | +50-100% CTR estimat |

**Regula de aur**: Suma segmentelor PRIMARY + SECONDARY NU trebuie sa depaseasca 70% din totalul volumului GEO. Daca depaseste, re-evalueaza PRIMARY cu criterii mai stricte.

### Step 3 — Copy Angle per Segment

Segmente IAB diferite necesita unghiuri de mesaj diferite pentru acelasi produs. Un singur SMS generic trimis la toate segmentele = CTR suboptim pe toate.

**Principii de redactare per profil IAB**:
- **Arts & Entertainment** (sports fans, consumatori entertainment): limbaj emotiv, urgenta, referinta la eveniment sportiv curent, ton informal
- **Technology / Software / Apps** (utilizatori tech): beneficiu functional, element de noutate/inovatie, ton direct
- **Business & Finance** (profil investitor): beneficiu financiar cuantificat, risc minim, ton professional
- **Health** (consumatori sanatate): beneficiu personal, dovada sociala, ton empatic
- **Travel** (calatori): oferta contextuala, libertate, ton aspirational

**Copy Angle Matrix**:

| IAB Segment | Angle Type | SMS Hook Exemplu (Bolivia ES / Romania RO) | CTA | Expected CTR |
|------------|-----------|-------------------------------------------|-----|-------------|
| Arts & Entertainment | Emotional / Event | [BO] "Real Madrid joacă azi! Pariază acum pe MarathonBet — bonus 100% primul depozit" / [RO] "EURO 2026 — Pariaza pe echipa ta favorita cu SpinBetter. Bonus 200 RON garantat" | "Inregistreaza-te acum" | 1.5-2.5% estimat |
| Technology | Functional / New | [BO] "App nou: MarathonBet — pariuri live direct de pe telefon. Descarca si primesti $10 gratis" | "Descarca acum" | 1.0-1.8% estimat |
| Software | Feature-driven | [BO] "Software inteligent pentru pariuri sportive. MarathonBet — analiza live, cote actualizate" | "Testeaza gratuit" | 0.9-1.5% estimat |
| Business & Finance | ROI / Income | [BO] "Tranzactioneaza forex din $10. AvaTrade — platforma reglementata, suport 24/7" / [RO] "Investeste inteligent: CFD-uri, actiuni, crypto. Cont demo gratuit azi" | "Deschide cont" | 1.2-2.0% estimat |
| Finance & Insurance | Professional / Trust | [RO] "Trading reglementat ASF. Platforma premiata. Capital minim $100. Incepe acum" | "Inregistreaza-te" | 1.5-2.2% estimat |
| Health | Empathic / Benefit | [RO] "Supliment natural pentru energie si imunitate. 10.000+ clienti multumiti. Comanda azi, livrare maine" | "Comanda acum" | 1.0-1.8% estimat |
| Apps | Direct / Gamified | [BO] "Joc nou gratis! Descarca [App] — joaca si castiga premii reale. Fara abonament" | "Descarca gratis" | 1.3-2.0% estimat |
| Travel | Aspirational | [BO] "Zbori spre destinatia visata? Gaseste cele mai bune oferte de calatorie. Rezerva acum" | "Vezi oferte" | 0.8-1.3% estimat |

**Instructiuni tehnice SMS**:
- Maxim 160 caractere per segment (un SMS standard)
- Include link tracker Binom cu parametru segment: `?seg=ARTS_BO`, `?seg=TECH_BO`, etc.
- Nu include cuvinte trigger spam: "GRATUIT", "CASTIG", "100%" in pozitia 1 a mesajului — testeaza variante

### Step 4 — Budget Allocation Model

Distribuie volumul de SMS disponibil catre segmente proportional cu Expected Revenue estimat, nu uniform.

**Formula Expected Revenue per Segment**:
```
Expected Revenue (Segment) = Segment Size × CTR_estimat × CR_offer × Payout_net
```

**Unde**:
- CTR_estimat = din Step 3 copy angle matrix (sau date istorice Binom per segment)
- CR_offer = rata conversie la landing page (din M-123 sau estimare 5-15% din clicks)
- Payout_net = payout dupa split VOX 50% (ex: $250 CPA AvaTrade → $125 net SMSads)

**Regula de alocare**:
1. Calculeaza Expected Revenue pentru fiecare segment PRIMARY + SECONDARY
2. Normalizeaza la 100% total volum de trimis (nu neaparat tot volumul GEO)
3. Minim 5,000 SMS per segment pentru semnificatie statistica (Step 5)
4. Daca un segment PRIMARY are < 5,000 contacte → combina cu cel mai apropiat SECONDARY

**Exemplu alocare Bolivia — Campanie MarathonBet (target 600K SMS)**:

| Segment IAB | Size | CTR Est. | Alloc % | SMS Alocate | Expected Clicks |
|-------------|------|----------|---------|------------|----------------|
| Arts&Entertainment | 246K | 2.0% | 41% | ~246K | ~4,920 |
| Technology | 693K | 1.2% | 50% | ~300K | ~3,600 |
| Software | 523K | 0.8% | 9% | ~54K | ~432 |
| **TOTAL** | | | **100%** | **~600K** | **~8,952** |

**Comparatie blast vs. targeted**:
- Blast 2.4M (toate categoriile): CTR mediu 0.5% → ~12,000 clicks, cost: 2.4M SMS
- Targeted 600K (segmente relevante): CTR mediu 1.4% → ~8,400 clicks, cost: 600K SMS
- **Eficienta: targeted = 70% din clicks la 25% din costul SMS → revenue/1K SMS de 2.8x mai mare**

**Regula minima per segment**: 5,000 SMS. Sub aceasta valoare nu exista semnificatie statistica.

### Step 5 — A/B Test Design per Segment

Testeaza 2 variante de copy per segment inainte de scale la volumul complet.

**Protocol A/B Test**:

**Setup**:
- Varianta A: copy angle tip 1 (ex: emotional/event)
- Varianta B: copy angle tip 2 (ex: functional/feature)
- Volumul de test: 10,000 SMS per varianta per segment (daca segmentul permite)
- Daca segment < 20,000: test pe 50% din segment, scale cu winner pe restul

**Stop Rule (Winner Declaration)**:
```
Winner = Varianta cu CTR mai mare DACA:
  - Minim 10,000 SMS trimise per varianta
  - Diferenta CTR >= 15% relativ (ex: 1.0% vs 1.15% = 15% diferenta relativa)
  - Ambele conditii indeplinite simultan
```

**Scale**:
- Winner declarat → trimite restul volumului segmentului cu varianta castigatoare
- Daca dupa 10,000 SMS diferenta < 15%: oricare varianta e valida, alege varianta B (mai scurta/directa, costuri potentiale mai mici)

**Tracking in Binom**:
- Campaign separation: campanie Binom separata per segment + per varianta
- UTM / parametri: `?seg=ARTS_BO&var=A` si `?seg=ARTS_BO&var=B`
- Kody configureaza tracking la setup campanie (obligatoriu inainte de prima trimitere)

**Salvare rezultate A/B**:
- Dupa fiecare test: salveaza winner + CTR per segment in fisierul de tracking campanie
- Aceste date alimenteaza Step 3 cu CTR_real (inlocuieste estimarile cu date reale)

### Step 6 — Performance Attribution per Segment

Tracking granular in Binom per categorie IAB. Weekly review. Kill decisiv pe segmente underperformante.

**Setup Binom per Segment**:
- Fiecare segment IAB = campanie sau sub-campanie separata in Binom
- Parametru de segment transmis: `{seg}` → valori: ARTS_BO, TECH_BO, SOFTWARE_BO, BIZ_FIN_BO, etc.
- Postback configurata per oferta pentru CR tracking

**Kill Rule**:
```
Daca dupa 20,000 SMS trimise pe un segment:
  CTR_segment < 50% × CTR_best_segment → KILL segmentul (opreste trimiterea)
```

Exemplu: Best segment CTR = 2.0% (Arts&Ent). Daca Software la 20K SMS are CTR 0.8% (< 1.0% = 50% din 2.0%) → opreste Software din campanie.

**Segment Performance Scorecard** (actualizat saptamanal):

| IAB Segment | SMS Sent | CTR% | Clicks | CR% | Conversii | Revenue | EC/1K SMS | Status |
|-------------|----------|------|--------|-----|-----------|---------|-----------|--------|
| Arts&Entertainment | 246,000 | 2.1% | 5,166 | 8% | 413 | $X | $Y | SCALE |
| Technology | 300,000 | 1.1% | 3,300 | 6% | 198 | $X | $Y | CONTINUE |
| Software | 54,000 | 0.7% | 378 | 5% | 19 | $X | $Y | MONITOR |
| Business&Finance | 0 | N/A | N/A | N/A | N/A | N/A | N/A | AVOID (betting) |
| **TOTAL** | **600,000** | **1.4%** | **8,844** | **7%** | **630** | **$X** | **$Y** | |

**Status definitions**:
- **SCALE**: CTR > 1.5x medie → aloca mai mult volum
- **CONTINUE**: CTR in medie → mentine alocarea
- **MONITOR**: CTR 50-70% din best → inca 10K SMS, re-evalueaza
- **KILL**: CTR < 50% din best dupa 20K SMS → opreste imediat
- **AVOID**: segment definit ca neeligibil in Step 2

**Weekly Review Checklist**:
- [ ] Export date Binom per segment (Kody)
- [ ] Actualizeaza Segment Performance Scorecard
- [ ] Aplica Kill Rule pe segmente underperformante
- [ ] Ajusteaza alocare buget per winner segments
- [ ] Noteaza CTR real → actualizeaza Copy Angle Matrix Step 3

---

## Section 3 — Cortex Logging

La finalizarea setup-ului de campanie segmentata si la fiecare weekly review, salveaza in Cortex:

```json
{
  "domain": "smsads",
  "sub_domain": "taxonomy-segmentation",
  "procedure": "M-125",
  "version": "1.0.0",
  "campaign": {
    "offer": "MarathonBet / AvaTrade / etc.",
    "geo": "Bolivia / Romania / Myanmar",
    "date_start": "YYYY-MM-DD",
    "total_volume_planned": 600000
  },
  "segments": [
    {
      "iab_category": "Arts&Entertainment",
      "size": 246000,
      "status": "PRIMARY",
      "sms_allocated": 246000,
      "ctr_estimated": 2.0,
      "ctr_actual": null,
      "status_performance": "PENDING"
    },
    {
      "iab_category": "Technology",
      "size": 693000,
      "status": "PRIMARY",
      "sms_allocated": 300000,
      "ctr_estimated": 1.2,
      "ctr_actual": null,
      "status_performance": "PENDING"
    }
  ],
  "ab_test": {
    "variant_a": "Emotional/Event angle",
    "variant_b": "Functional/Feature angle",
    "winner": null,
    "test_volume": 10000
  },
  "weekly_review_date": "YYYY-MM-DD",
  "uplift_vs_untargeted": null,
  "notes": ""
}
```

---

## Section 4 — Enforcement Loop

**WHERE**: Se execută înainte de fiecare campanie SMS nouă, la definirea setului de segmente VOX. Rulat de affiliate manager după ce oferta este confirmată activ pe 3SNET și GEO-ul este ACTIVE în M-124.

**WHEN**: La fiecare campanie nouă (obligatoriu pre-launch) + review săptămânal per campanie activă. La schimbarea ofertei sau a GEO-ului, segmentarea se reia de la Step 2.

**CONNECT**:
- M-122 (SMS Vertical Discovery) — defineste verticalul si oferta → M-125 primeste oferta si aplica segmentarea
- M-123 (Offer Commission Scoring) — furnizeaza EC/1K SMS benchmarks pentru Step 4 allocation model
- M-124 (GEO-Product Affinity Scoring) — furnizeaza top combinatii GEO × Produs; M-125 le optimizeaza prin segmentare

**HOW**:
Campanie SMS lansata fara segmentare IAB = **VIOLATION M-125**.
Estimare pierdere revenue per campanie nesegmentata: 3-5x multiplicator pe EC/1K SMS.
Nicio campanie SMSads nu porneste fara Step 2 (Offer-to-Segment Matching) completat.

**VERIFY — 6 Checks obligatorii pre-launch**:
1. [ ] VOX Category Audit (Step 1) actualizat pentru GEO-ul campaniei
2. [ ] Offer-to-Segment Matching (Step 2) completat: PRIMARY + SECONDARY + AVOID definite
3. [ ] Copy angle creat per segment PRIMARY (minim) — Step 3
4. [ ] Budget allocation calculata cu formula Expected Revenue — Step 4
5. [ ] Binom configurat cu parametri per segment + per varianta A/B — Step 5
6. [ ] Segment Performance Scorecard creat (gol) si datat pentru weekly review — Step 6

**MODEL ROUTING**:

| Step | Model | Justificare |
|------|-------|-------------|
| Step 1 — Category Audit | Sonnet | Structurare date VOX existente, fara sinteza complexa |
| Step 2 — Offer-to-Segment Matching | Sonnet | Mapare tabelara determinista |
| Step 3 — Copy per Segment | Opus | Redactare copy personalizat per profil IAB + GEO |
| Step 4 — Budget Allocation | Opus | Modelare economica expected revenue per segment |
| Step 5 — Binom Config | Sonnet | Configurare tehnica standard |
| Step 6 — Performance Scorecard | Sonnet | Agregare date si tracking continuu |

**PROHIBIT**:
- Trimitere la full database GEO fara segmentare
- Copy identical pe segmente cu profiluri diferite (Arts&Ent vs. Business&Finance)
- Launch campanie fara tracking Binom per segment configurat
- Ignorare Kill Rule dupa 20K SMS per segment

---

## Section 5 — Dependente

| Dependenta | Tip | Cum se obtine |
|-----------|-----|---------------|
| VOX Platform IAB Data | Input principal | Request formal per campanie: lista categorii + volume actualizate |
| Binom Tracker | Executie + raportare | Kody configureaza campanii per segment. Access: Kody primary |
| M-123 EC/1K SMS benchmarks | Input economic | Ruleaza M-123 inainte de Step 4 allocation |
| M-124 Priority Matrix | Input strategic | Top GEO × Produs din M-124 → input pentru alegerea segmentelor |
| 3SNET / Oferta activa | Conditie prealabila | Oferta activa si trackable inainte de orice segmentare |
| Date istorice campanii | Optimizare continua | Export Binom post-campanie → alimenteaza CTR_real in Step 3 |

---

## Section 6 — Metrics

| Metric | Target | Sursa |
|--------|--------|-------|
| Segmentation uplift CTR vs. untargeted | +50% minim (target: +100-150%) | Binom comparatie campanie segmentata vs. blast anterior |
| Revenue per 1K SMS (segmentat) | > 2x Revenue/1K SMS blast | Binom EC/1K per campanie |
| Segment coverage % (PRIMARY utilizat) | Minim 60% din volumul campaniei in segmente PRIMARY | Step 4 allocation table |
| Kill rate segmente underperformante | 100% Kill Rule aplicata dupa 20K SMS | Weekly review scorecard |
| A/B test winner declared | 100% campanii cu test completat inainte de scale | Binom A/B report |
| Timp setup segmentare per campanie | < 4 ore (Steps 1-5 completate) | Timestamp task vs. launch |

---

## Checklist Pre-Publicare

- [x] Scope definit clar (segmentare IAB per campanie SMS)
- [x] Problema documentata cu calcul pierdere estimata (3-5x multiplicator)
- [x] Step 1: Category Audit cu toate datele disponibile BO + RO (TBD pentru categorii incomplete)
- [x] Step 2: Offer-to-Segment matching table cu 6 oferte concrete (MarathonBet, AvaTrade, SpinBetter, Trading RO, Health RO, App Install BO)
- [x] Step 3: Copy Angle Matrix cu 8 segmente, hookuri exemplu BO/RO, CTA si CTR estimat
- [x] Step 4: Budget Allocation Model cu formula Expected Revenue + exemplu numeric Bolivia MarathonBet 600K SMS
- [x] Step 5: A/B Test Design cu Stop Rule clara (10K SMS + 15% delta) + tracking Binom
- [x] Step 6: Performance Attribution cu Kill Rule (50% din best dupa 20K SMS) + Scorecard template
- [x] Section 3: Cortex JSON complet cu segmente si A/B test tracking
- [x] Section 4: Enforcement Loop cu VIOLATION declaration + 6 Checks obligatorii pre-launch
- [x] Section 5: Dependente documentate
- [x] Section 6: Metrics cu targets masurabile
- [x] Procedura in romana cu termeni tehnici in engleza
