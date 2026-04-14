---
type: procedure
created: 2026-03-20
status: active
slug: m-002-affiliate-program-evaluation
tags: [procedure, nexus]
---

# M-002 — Affiliate Program Evaluation and Selection

**Domeniu**: affiliate | **Sub-domeniu**: program-evaluation
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Evaluarea sistematica a programelor si ofertelor affiliate disponibile intr-o nisa validata (M-001), cu selectia celor mai profitabile pe baza EPC real, payout model, cookie/attribution window, backend revenue, si compatibilitate cu sursa de trafic. Acopera atat programele clasice (ClickBank, ShareASale, Impact) cat si CPA networks (MaxBounty, ClickDealer, Mobidea, Advidi) — cu focus pe ofertele SMS/push-compatibile pentru SMSads.

**SMSads Context**: Pe SMS, oferta gresita = buget ars in ore, nu zile. SMS-ul ajunge instant, user-ul decide in 30 secunde — CR-ul depinde 80% de oferta si 20% de creative. MarathonBet Bolivia = $80 CPA, CR 0.3%. O oferta cu CR 0.1% pe acelasi GEO = pierdere neta.

**Ce se intampla fara procedura:**
- Selectie pe baza payout-ului vizibil, ignorand EPC real si CR → CPA > payout
- Oferta promovata pe sursa de trafic interzisa → conversii anulate, cont suspendat
- Ignorarea backend commissions → pierdere 30-60% din revenue potential
- Lipsa split-testing pe oferte similare → rularea perpetua a ofertei #2 in loc de #1

---

## Pasi

### Pas 1: INVENTAR COMPLET OFERTE DIN TOATE SURSELE

Construieste un spreadsheet cu TOATE ofertele disponibile din:
- **CPA Networks**: MaxBounty, ClickDealer, Mobidea, Advidi, 3SNET, Perform[cb], CrakRevenue
- **Affiliate Platforms**: ClickBank (digital), ShareASale (retail), JVZoo (IM), Impact (SaaS)
- **Direct Programs**: programe affiliate directe de la operatori (bet365, 888, Betsson pt gambling)
- **SmartLink agregators**: Mobidea SmartLink, Clickaine (auto-optimizeaza oferta per user)

Coloane obligatorii: Offer ID, Network, Vertical, Payout, Payout Model (CPA/CPL/CPI/RevShare), EPC, CR, GEO, Traffic Types Allowed, Min Deposit (gambling), Cookie/Attribution Window, Approval Status.

**Eroare frecventa**: Limitarea la un singur network. Aceeasi oferta (acelasi operator) poate avea payout diferit pe networks diferite — MarathonBet pe 3SNET = $80 CPA, pe Advidi = $65 CPA, direct = $90 CPA dar cu threshold mai mare.

### Pas 2: FILTRARE PE COMPATIBILITATE TRAFIC

Elimina ofertele incompatibile cu sursa ta de trafic:
- **SMS traffic**: verifica explicit "SMS allowed" sau "carrier billing" in offer terms
- **Push notifications**: majoritatea ofertelor accepta push, dar verifica creative restrictions
- **Native ads**: PropellerAds, RichAds, Evadav — verifica ca oferta accepta "native" sau "display"
- **Social/Facebook**: cele mai restrictive — gambling/nutra/dating sunt banned sau require whitehat approach
- **Email**: verifica "email allowed" si daca necesita opt-in proof

**Red flags pe offer terms:**
- "Incentivized traffic only" = exclude SMS/push
- "Search traffic only" = exclude orice non-search
- "No adult creative" = restrictioneaza dating angles
- "Pre-approved LPs only" = trebuie sa folosesti landing-urile lor

### Pas 3: EVALUARE PAYOUT MODEL SI UNIT ECONOMICS

Compara payout models pe aceeasi oferta/vertical:

| Model | Best For | Risk | Example |
|-------|---------|------|---------|
| CPA (Cost Per Action) | High-volume SMS/push | Low (fixed payout) | $80 per FTD gambling |
| CPL (Cost Per Lead) | Finance, insurance | Low | $5-15 per lead |
| CPI (Cost Per Install) | Apps, utilities | Very low | $0.5-3 per install |
| RevShare | Long-term gambling | High (variable) | 25-45% of net revenue |
| Hybrid | Experienced affiliates | Medium | $30 CPA + 10% RevShare |

**Calcul comparativ:**
```
CPA: $80 x 100 FTDs = $8,000 (immediate, guaranteed)
RevShare 35%: 100 FTDs x avg $230 net/player = $8,050 (over 6-12 months, variable)
Verdict: CPA for cash flow, RevShare doar daca traffic quality proven
```

### Pas 4: ANALIZA EPC SI CONVERSION RATE REAL

EPC (Earnings Per Click) = metrica suprema:
- **EPC > $1.00**: oferta excelenta (top 10%)
- **EPC $0.30-1.00**: oferta buna, viabila
- **EPC < $0.30**: oferta slaba, necesita volum enorm

**Atentie la EPC inflated**: unele networks afiseaza EPC pe toate click-urile inclusiv bots. EPC real e 20-40% mai mic. Cere AM-ului EPC pe "quality traffic only".

**CR benchmarks per vertical (SMS traffic):**
| Vertical | CR Range | Good CR |
|----------|---------|---------|
| Gambling FTD | 0.1-0.5% | > 0.3% |
| Sweepstakes SOI | 5-15% | > 8% |
| App Install | 3-10% | > 5% |
| Nutra (COD) | 0.5-2% | > 1% |
| Dating SOI | 3-8% | > 5% |

### Pas 5: VERIFICARE ATTRIBUTION WINDOW

| Traffic Type | Optimal Window | Why |
|-------------|---------------|-----|
| SMS | 24h-7 days | User decide instant |
| Push | 24h-3 days | Impulse traffic |
| SEO/Content | 30-90 days | Research cycle |
| Email | 7-14 days | Consider period |

**Gambling specific**: Majority FTDs within 1 hour of click for SMS. Cookie 30 zile irrelevant — dar cookie 24h riscant daca KYC dureaza 2-3 zile.

### Pas 6: INVESTIGARE BACKEND REVENUE

Backend revenue poate dubla earnings fara cost suplimentar:
- **Gambling**: RevShare pe deposits ulterioare (6x revenue potential)
- **Nutra**: Auto-ship subscriptions (CPA + recurring)
- **SaaS**: Monthly recurring (ConvertKit 30%, Shopify 20%)
- **Finance**: Tiered commissions (loan amount tiers)

**Intrebari obligatorii pentru AM:**
1. "Do you offer backend/rebill commissions?"
2. "What's the average LTV of a converted user?"
3. "Can I get RevShare on top of CPA?"
4. "Are there upsell funnels I get credited for?"

### Pas 7: SCORING FINAL SI SHORTLIST TOP 5

| Criteriu | Pondere | 1-10 |
|---------|---------|------|
| EPC real | 30% | 1=sub $0.10, 10=$2+ |
| Payout absolute | 20% | 1=sub $1, 10=$100+ |
| Traffic compatibility | 20% | 1=incompatibil, 10=perfect |
| Backend potential | 10% | 1=zero, 10=2x+ front-end |
| Network reliability | 10% | 1=unknown, 10=top-tier |
| Conversion flow simplicity | 10% | 1=10+ steps, 10=1-click |

Selecteaza top 5 cu scor > 60/100 pentru testare.

### Pas 8: SETUP TRACKING PER OFERTA

In Voluum/BeMob/RedTrack/Binom:
- Un campaign per oferta cu postback URL configurat
- SubID-uri: s1=traffic_source, s2=creative_id, s3=geo, s4=device, s5=carrier
- Cost tracking: manual sau auto
- Landing page rotation daca testezi multiple LPs

**Validare**: click test → verifica in tracker → simulare conversie test (AM manual postback) → confirma revenue corect.

### Pas 9: TESTARE SIMULTANA 48-72 ORE

Lanseaza 5 oferte simultan cu buget egal ($50-100/oferta):
- Acelasi trafic, aceleasi creatives (izoleaza variabila "oferta")
- Masoara: CR real, EPC real, cost per conversie, ROI

**Kill criteria:** Zero conversii dupa spend = 2x payout → kill. CPA > 1.5x payout dupa 1000+ clicks → kill.
**Scale criteria:** ROI > 30% sustinut 48h+ → scale candidat.

### Pas 10: NEGOCIERE PAYOUT PE WINNER

Dupa identificarea winner-ului, contacteaza AM:
- **Ask**: +10-30% peste standard rate
- **Leverage**: volum consistent (30+ conv/zi), low chargeback, quality traffic
- **Timing**: dupa minim 50 conversii, ideal 100+
- Detalii complete in M-112.

---

## Verificare

- [ ] Inventar din minim 3 CPA networks + 2 affiliate platforms?
- [ ] Filtrare trafic aplicata — zero oferte incompatibile?
- [ ] EPC real verificat (nu doar network average)?
- [ ] Attribution window validat per traffic type?
- [ ] Backend revenue investigat?
- [ ] Scoring pe 6 criterii calculat?
- [ ] Top 5 cu tracking configurat in Voluum/BeMob?
- [ ] Test 48-72h rulat cu kill/scale criteria?
- [ ] Winner identificat si negociere initiata?

---

## Instrumente

| Tool | Rol |
|------|-----|
| MaxBounty / ClickDealer / Mobidea / Advidi / 3SNET | Sursa oferte CPA |
| ClickBank / ShareASale / JVZoo / Impact | Sursa oferte affiliate |
| Voluum / BeMob / RedTrack / Binom | Tracking si comparare |
| Google Sheets | Spreadsheet evaluare |
| Affiliate Manager (direct) | EPC real, backend info, bumps |

---

## Note

- Depinde de M-001. Nu evalua oferte fara nisa confirmata.
- Re-evaluare la: schimbare payout, oferta suspendata, sau la fiecare 30 zile.
- SmartLinks (Mobidea) utile ca baseline — ruleaza in paralel si compara EPC.
- Gambling CPA benchmarks: Tier 1 $100-200, Tier 2 $50-100, Tier 3 $30-80.
