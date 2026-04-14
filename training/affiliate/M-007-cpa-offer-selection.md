---
type: procedure
created: 2026-03-20
status: active
slug: m-007-cpa-offer-selection
tags: [procedure, nexus]
---

# M-007 — CPA Offer Selection and Competitor Analysis

**Domeniu**: affiliate | **Sub-domeniu**: cpa-offer-selection
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Selectia sistematica a ofertelor CPA profitabile din retelele aprobate (M-006), combinata cu analiza competitiva a afiliatilor existenti, validare trafic compatibil, si testare simultana cu kill/scale criteria. Procedura elimina selectia pe payout vizibil si o inlocuieste cu decizie data-driven bazata pe EPC real, CR verificat, si unit economics per sursa de trafic.

**SMSads Context**: Pe SMS, oferta gresita = buget ars in ore. SMS-ul ajunge instant, user-ul decide in 30 secunde. MarathonBet Bolivia cu CR 0.3% si AvaTrade cu CR 1.5% pe acelasi segment IAB → diferenta de 5x pe revenue. Selectia ofertei este cea mai importanta decizie in pipeline-ul SMS. Procedura SMSADS-P2 extinde M-007 cu focus specific SMS — M-007 ramane procedura generala aplicabila pe toate sursele de trafic.

**Ce se intampla fara procedura:**
- Selectie pe payout maxim fara EPC/CR real → CPA > payout pe 70% din ofertele testate
- Oferta promovata pe sursa de trafic interzisa → conversii anulate, cont suspendat
- Lipsa competitor analysis → entry intr-o piata saturata cu 10+ afiliati activi pe aceeasi oferta
- Zero split testing → rularea ofertei #2 in loc de #1 pe termen lung

---

## Pasi

### Pas 1: INVENTAR OFERTE DIN TOATE RETELELE APROBATE

Logheaza-te in dashboard-ul fiecarei retele CPA aprobate si construieste inventar:
- **MaxBounty**: Offers → Browse → filtreaza pe nisa + GEO + traffic type permis
- **ClickDealer**: Offers → filtreaza Country + Category + Traffic Source
- **Mobidea**: Dashboard → Top Offers → filtreaza GEO + Carrier + Payout Model
- **Advidi**: Offers → filtreaza Vertical + GEO + Min Payout
- **3SNET**: Dashboard → filtreaza per GEO + Operator

Coloane obligatorii in spreadsheet:
Offer ID | Network | Offer Name | Vertical | Payout | Model (CPA/CPL/CPI/RevShare) | EPC Network | CR Network | GEO | Traffic Types Allowed | Min Deposit (gambling) | Cap Volume | Approval Status

**Regula**: minim 15-20 oferte pe shortlist initiala. Sub 10 = insuficient pentru testare statistica.

**Eroare frecventa**: Limitarea la un singur network. Aceeasi oferta are payout diferit pe networks diferite — MarathonBet pe 3SNET = RevShare 30%, pe alt network = CPA fix $80. Compara MEREU.

### Pas 2: FILTRARE PE COMPATIBILITATE TRAFIC (ELIMINARE STRICTA)

Verifica explicit "Traffic Types Allowed" in offer terms pentru FIECARE oferta:

**Verificari per sursa de trafic:**
- **SMS traffic**: cauta explicit "SMS allowed" sau "carrier billing" sau "mobile traffic". Daca nu e mentionat explicit → contacteaza AM si intreaba
- **Push notifications**: majoritatea ofertelor accepta push, verifica creative restrictions
- **Native ads** (PropellerAds, RichAds, Evadav, MGID): verifica ca oferta accepta "native" sau "display"
- **Social/Facebook**: cele mai restrictive — gambling/nutra/dating sunt banned sau necesita whitehat approach
- **Email**: verifica "email allowed" si daca necesita opt-in proof

**Red flags in offer terms:**
- "Incentivized traffic only" → exclude SMS/push (nu e incentivized)
- "Search traffic only" → exclude orice non-search
- "No adult creative" → restrictioneaza dating angles
- "Pre-approved LPs only" → trebuie sa folosesti landing-urile lor (limiteaza A/B testing)
- "No carrier billing" → interzice SMS carrier traffic direct

**Regula**: orice oferta cu traffic type incompatibil → REMOVE din shortlist. Zero exceptii.

### Pas 3: EVALUARE EPC SI CONVERSION RATE REAL

EPC (Earnings Per Click) = metrica suprema pentru comparare oferte:

**Benchmarks EPC per vertical:**
| EPC Range | Calificare | Actiune |
|-----------|-----------|---------|
| >$1.50 | Excelent (top 5%) | Prioritate maxima de testare |
| $0.50-1.50 | Bun | Testare standard |
| $0.20-0.50 | Mediocru | Testare doar daca zero alternative |
| <$0.20 | Slab | Skip — necesita volum enorm |

**ATENTIE la EPC inflated**: network-urile afiseaza EPC pe TOATE click-urile inclusiv bots si accidentale. EPC real e 20-40% mai mic. Cere AM-ului: "What's the real EPC for quality traffic on this offer?"

**CR benchmarks per vertical (SMS traffic):**
| Vertical | CR Range SMS | Good CR |
|----------|-------------|---------|
| Gambling FTD | 0.1-0.5% | >0.3% |
| Gambling Registration | 0.5-2.0% | >1.0% |
| Sweepstakes SOI | 5-15% | >8% |
| App Install | 3-10% | >5% |
| Nutra (COD) | 0.5-2% | >1% |
| Dating SOI | 3-8% | >5% |
| Trading Lead | 0.8-2.5% | >1.5% |
| Finance CPL | 1-4% | >2% |

### Pas 4: COMPETITOR SPY RESEARCH

Foloseste spy tools pentru a evalua cine ruleaza deja oferta:

**Tool stack per canal:**
| Tool | Canal | Cost | Ce vezi |
|------|-------|------|---------|
| Adplexity Push | Push ads | $149/luna | Push creatives, landing pages, GEO-uri, zile active |
| Adplexity Native | Native ads | $149/luna | Native ads, LP-uri, spend estimates |
| Anstrex | Native + push | $69/luna | Budget-friendly alternativa la Adplexity |
| SpyFu / SEMrush | SEO/PPC | $33-99/luna | Keywords, ad copy, spend PPC |
| Facebook Ad Library | Social ads | Gratuit | Ads active pe Meta platforms |
| SimilarWeb | All channels | Freemium | Traffic estimates, referral sources |

**Ce cauti:**
- Cate ads/campanii ruleaza pe oferta ta + GEO-ul tau? (saturare)
- De cat timp ruleaza? (>30 zile active = profitabil confirmat)
- Ce creative-uri/angles folosesc? (inspiratie, nu copiere)
- Ce landing pages au? (structura, flow, copy angles)

**Scorare saturare:**
- 0-5 ads active pe vertical/GEO = LOW (ideal entry)
- 5-20 ads = MEDIUM (viabil cu diferentiere)
- 20+ ads = HIGH (necesita buget mare sau angle unic)

### Pas 5: ANALIZA CREATIVE-URI SI LANDING PAGES COMPETITORI

Pentru top 5 competitori identificati in Pas 4:
1. **Screenshot toate creative-urile** (ads, headlines, thumbnails)
2. **Viziteaza landing pages-urile** — documenteaza: headline, structure, social proof, CTA, load time
3. **Identifica pattern-uri comune** = ce functioneaza in nisa (nu reinventa roata)
4. **Identifica puncte slabe** = oportunitati de imbunatatire (testimoniale lipsa, load time lent, copy generic)
5. **Documenteaza angles folosite**: emotional, logical, curiosity, fear, social proof

**Eroare frecventa**: Copierea verbatim a creative-urilor competitorilor. Riscuri: copyright issues, ad fatigue (user-ul a vazut deja aceleasi ads), penalizare de platform. Reverse-engineer strategia, nu copia executia.

### Pas 6: CREAREA VARIATIEI PROPRII IMBUNATATITE

Pe baza analizei competitoare, creaza creative-uri proprii:
- **Headline**: angle diferit de competitori — daca toti folosesc "curiosity gap", tu foloseste "social proof story"
- **Landing page**: structura similara cu winner-ul competitiv dar cu imbunatatiri pe punctele slabe identificate
- **Copy**: adaptat la GEO + limba nativa (spaniola peruviana ≠ spaniola europeana)
- **Trust elements**: adauga ce competitorii omit (testimoniale, disclaimere, badges)
- **Speed**: LP-ul tau trebuie sa incarce mai rapid decat al competitorului (advantage pe mobil)

### Pas 7: SCORING FINAL SI SHORTLIST TOP 5

Scoreaza fiecare oferta pe 6 criterii cu ponderi:

| Criteriu | Pondere | 1-10 |
|---------|---------|------|
| EPC real (sau estimat) | 30% | 1=sub $0.10, 10=$2+ |
| Payout absolut | 20% | 1=sub $1, 10=$100+ |
| Traffic compatibility | 20% | 1=incompatibil, 10=perfect match |
| Saturare competitiva | 10% | 1=200+ ads, 10=sub 5 ads |
| Network reliability | 10% | 1=unknown, 10=top-tier (MaxBounty) |
| Conversion flow simplicity | 10% | 1=10+ steps, 10=1-click |

**Selecteaza top 5 cu scor >60/100** pentru testare.

### Pas 8: SETUP TRACKING PER OFERTA

In Voluum/BeMob/RedTrack/Binom:
- Un campaign per oferta cu postback URL configurat
- SubID-uri: s1=traffic_source, s2=creative_id, s3=geo, s4=device, s5=carrier (SMS specific)
- Cost tracking: manual (SMS cost per GEO) sau auto (push/native)
- Landing page rotation daca testezi multiple LP-uri per oferta
- **Validare obligatorie**: click test → verifica in tracker → AM fire manual postback → confirma revenue in tracker

### Pas 9: TESTARE SIMULTANA 48-72 ORE CU BUGET EGAL

Lanseaza top 5 oferte simultan:
- **Buget egal**: $50-100/oferta (sau 5K-10K SMS per oferta pentru SMS traffic)
- **Acelasi trafic, aceleasi creatives** (izoleaza variabila "oferta")
- **Metrici**: CR real, EPC real, cost per conversie, ROI
- **Duration**: minim 48 ore, ideal 72 ore de date

**Kill criteria:**
- Zero conversii dupa spend = 2x payout → KILL imediat
- CPA > 1.5x payout dupa 1000+ clicks → KILL
- CR < 50% din media celorlalte oferte testate → KILL

**Scale criteria:**
- ROI > 30% sustinut pe 48h+ → SCALE candidat
- CR consistent (nu spike one-time) → SCALE

### Pas 10: DOCUMENTARE WINNER SI NEGOCIERE PAYOUT

Dupa identificarea winner-ului:
1. Documenteaza: oferta castigatoare, CR real, EPC real, ROI, GEO, traffic source
2. Scale gradual: +20% buget la fiecare 48 ore
3. Contacteaza AM: "I'm sending [X] conversions/day on [offer]. Can we discuss a payout bump?" (detalii complete in M-112)
4. Salveaza in `~/.nexus/projects/smsads/offer-winner-{vertical}-{geo}-{date}.md`
5. Continue testing: oferta #2 din shortlist devine next test candidate

---

## Verificare

- [ ] Inventar din minim 3 CPA networks cu 15+ oferte?
- [ ] Filtrare trafic aplicata — zero oferte incompatibile ramase?
- [ ] EPC real verificat (nu doar network average)?
- [ ] Competitor spy completat (minim 5 competitori, ads si LP-uri documentate)?
- [ ] Scoring pe 6 criterii calculat pentru toate ofertele?
- [ ] Top 5 cu tracking configurat in Voluum/BeMob?
- [ ] Test 48-72h rulat cu kill/scale criteria definite?
- [ ] Winner identificat si documentat?
- [ ] Negociere payout initiata (M-112)?

---

## Instrumente

| Tool | Rol |
|------|-----|
| MaxBounty / ClickDealer / Mobidea / Advidi / 3SNET | Sursa oferte CPA |
| Adplexity / Anstrex | Spy push + native ads |
| SpyFu / SEMrush | Spy PPC + SEO competitors |
| Facebook Ad Library | Spy social ads (gratuit) |
| SimilarWeb | Traffic estimates competitori |
| Voluum / BeMob / Binom | Tracking si comparare oferte |
| Google Sheets | Spreadsheet scoring + tracking results |

---

## Note

- Depinde de M-006 (acces la retele CPA aprobate).
- Este prerequisite pentru M-008 (tracking setup), M-009 (Facebook Ads), M-112 (negotiation).
- Re-evaluare: la fiecare 30 zile — payout-uri se schimba, oferte expira, competitia evolueaza.
- SmartLinks (Mobidea) utile ca baseline — ruleaza in paralel si compara EPC cu oferte directe.
- Gambling CPA benchmarks Tier: T1 $100-200, T2 $50-100, T3 $30-80. SMS markets (Bolivia/Peru/Kenya) sunt T2-T3.
