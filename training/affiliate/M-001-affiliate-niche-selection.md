---
type: procedure
created: 2026-03-20
status: active
slug: m-001-affiliate-niche-selection
tags: [procedure, nexus]
---

# M-001 — Affiliate Niche Selection and Validation

**Domeniu**: affiliate | **Sub-domeniu**: niche-selection
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5

---

## Obiectiv

Selectarea si validarea sistematica a unei nise profitabile pentru affiliate marketing, cu scoring multi-criterial calibrat pe verticale reale (gambling $50-150 CPA, nutra $30-80, sweepstakes $1-5, app installs $0.5-3). Procedura elimina selectia pe "feeling" si inlocuieste cu date verificabile din spy tools, CPA network dashboards, si analiza competitor spend.

**SMSads Context**: Verticalele SMS-compatibile (betting, casino, nutra, dating, finance) au CPA-uri si conversion rates radical diferite pe Bolivia/Peru/Kenya/Chile. Nisa gresita pe SMS = EC/1K negativ dupa 3 zile. Aceasta procedura previne burn de buget prin validare inainte de prima campanie.

**Ce se intampla fara procedura:**
- Nisa aleasa pe hype temporar → piata saturata in 30 zile, margins 0
- Vertical gambling in tara fara carrier billing → CR 0% pe SMS
- Nisa cu comisioane mari dar zero oferte pe CPA networks accesibile → blocaj operational
- Ignorarea pietelor LATAM/Africa cu CPA-uri mici dar volum urias → pierdere 10x ROI vs US-only focus
- Competitie analizata superficial → entry intr-o nisa dominata de super-affiliates cu $50K+/zi spend

---

## Pasi

### Pas 1: BRAINSTORM VERTICALE CU MATRICE SMS/WEB

Genereaza 12-15 verticale potentiale folosind doua axe: canal primar (SMS/push/native/search/social) si model payout (CPA/CPL/CPI/RevShare). Nu brainstorma generic — fiecare vertical trebuie sa aiba cel putin o oferta identificabila pe MaxBounty, ClickDealer, Mobidea, sau Advidi.

**Verticale de referinta cu CPA benchmarks:**
| Vertical | CPA Range | Best Traffic | Difficulty |
|----------|-----------|-------------|------------|
| Gambling/Casino | $50-150 | SMS, push, native | High |
| Sports Betting | $30-100 | SMS, push, social | High |
| Nutra/Health | $30-80 | Native, social, search | Medium |
| Sweepstakes | $1-5 | Push, pop, SMS | Low |
| App Installs | $0.5-3 | Push, in-app, SMS | Low |
| Dating | $2-8 (SOI), $15-40 (DOI) | Push, native, social | Medium |
| Finance/Loans | $20-60 | Search, email, SMS | High |
| VPN/Antivirus | $10-40 | Search, native | Medium |
| Crypto/Trading | $200-500 | Search, native, social | Very High |
| Insurance | $15-45 | Search, email | High |

**Eroare frecventa**: Alegerea gambling doar pentru CPA mare fara verificare ca GEO-ul target accepta gambling ads pe canalul ales. Peru accepta SMS gambling, dar Kenya restrictioneaza advertising gambling fara licenta BCLB.

### Pas 2: VALIDARE DISPONIBILITATE OFERTE PE CPA NETWORKS

Pentru fiecare vertical din shortlist, verifica disponibilitatea reala de oferte pe:
- **MaxBounty** — cel mai mare mix, bun pentru gambling US/CA/EU
- **ClickDealer** — puternic pe dating, sweeps, mobile content
- **Mobidea** — specialist mobile, CPI, carrier billing (ideal SMS)
- **Advidi** — gambling/dating EU, payouts premium
- **3SNET** — gambling LATAM/Africa (partenerul SMSads actual)
- **Clickbooth/Perform[cb]** — finance, insurance US

Minim 10 oferte active per vertical = viabil. Sub 5 oferte = red flag (piata prea mica sau prea restrictiva). Noteaza: payout mediu, EPC mediu, GEO-uri acceptate, traffic types permise.

### Pas 3: ANALIZA SATURARE COMPETITIVA CU SPY TOOLS

Foloseste spy tools pentru a evalua densitatea competitiva:
- **Adplexity** (push/native/mobile) — vezi cate ad-uri ruleaza pe verticalul tau, de cat timp, pe ce GEO-uri
- **SpyFu/SEMrush** — competitie SEO pe buyer-intent keywords
- **Anstrex** — native ads spy, vezi creative-uri si landing pages
- **SimilarWeb** — trafic estimat pe site-urile affiliate competitor

**Scorare saturare:**
- 0-50 ads active pe vertical/GEO = Low competition (ideal entry)
- 50-200 ads = Medium (viabil cu diferentiere)
- 200+ ads = High (necesita buget mare, creatives superioare, sau angle unic)

**Eroare frecventa**: Confundarea competitiei scazute cu lipsa de cerere. Zero ads pe un vertical/GEO poate insemna ca nimeni nu a reusit sa faca profit acolo, nu ca e oportunitate neexplorata. Verifica cu Pas 4.

### Pas 4: SCORING MULTI-CRITERIAL (7 DIMENSIUNI, 0-100)

Scoreaza fiecare vertical pe 7 dimensiuni cu ponderi:

| Criteriu | Pondere | Scor 1-10 | Sursa |
|---------|---------|-----------|-------|
| CPA/Payout potential | 25% | 1=sub $5, 10=$100+ | Network dashboard |
| Disponibilitate oferte | 15% | 1=sub 5, 10=50+ oferte | MaxBounty/Mobidea |
| Compatibilitate SMS/push | 20% | 1=interzis, 10=canal primar | Offer terms |
| Saturare competitiva | 15% | 1=200+ ads, 10=sub 20 | Adplexity/Anstrex |
| Compliance accessibility | 10% | 1=licenta obligatorie, 10=fara restrictii | Legislatie GEO |
| Scalabilitate multi-GEO | 10% | 1=1 GEO, 10=10+ GEO-uri | Network coverage |
| Evergreen potential | 5% | 1=sezonier, 10=constant | Trends data |

**Formula**: Scor compozit = Sum(criteriu x pondere). Range: 0-100. Threshold: minim 55/100 pentru a avansa.

### Pas 5: VALIDARE UNIT ECONOMICS PER GEO

Pentru top 3 verticale (scor > 55), calculeaza unit economics pe GEO-urile target:

```
Revenue per 1K SMS = (CR% x payout) x 1000
Cost per 1K SMS = cost_sms + cost_tracking + cost_creative
Profit per 1K SMS = Revenue - Cost

Exemplu gambling Bolivia:
CR = 0.3%, Payout = $80, Cost SMS = $15/1K
Revenue/1K = 0.003 x $80 x 1000 = $240
Cost/1K = $15 + $2 (Voluum) + $1 (creative) = $18
Profit/1K = $222 (ROI 1233%)

Exemplu sweepstakes Peru:
CR = 2%, Payout = $2, Cost SMS = $12/1K
Revenue/1K = 0.02 x $2 x 1000 = $40
Cost/1K = $12 + $2 + $1 = $15
Profit/1K = $25 (ROI 167%)
```

**Eroare frecventa**: Ignorarea costului real SMS per GEO. Bolivia costa $15/1K dar Kenya costa $8/1K — diferenta de 47% schimba complet unit economics. Verifica preturile VOX/carrier actuale.

### Pas 6: TEST DE PIATA RAPID (72 ORE)

Ruleaza un test minim pe verticalul #1:
- **Buget**: $100-200 (echivalent 5K-15K SMS in functie de GEO)
- **Tracker**: Voluum sau BeMob cu postback configurat
- **Oferta**: cea cu EPC cel mai mare din network
- **Landing**: simpla, mobile-first, sub 3 secunde load time
- **Metrici de urmarit**: CR, EPC real vs estimat, cost per conversie, bounce rate landing

**Kill criteria**: Dupa $100 spend fara nicio conversie pe un vertical cu CR estimat > 1% = kill. Dupa $200 spend cu CPA > 2x payout = kill.

**Eroare frecventa**: Rularea testului pe un GEO diferit de cel din unit economics (ex: calculi pe Bolivia dar testezi pe Peru). GEO-ul testului TREBUIE sa fie identic cu cel din Pas 5.

### Pas 7: DECIZIE FINALA SI DOCUMENTARE COMPLETA

Documenteaza decizia finala cu:
- Verticalul selectat + scor compozit
- Top 3 GEO-uri cu unit economics calculate
- Rezultatele testului de piata (Pas 6)
- Ofertele identificate pe fiecare network
- Competitor landscape (saturare, top players, budgete estimate)
- Riscuri identificate + plan de mitigare
- Entry strategy: buget zilnic initial, scaling plan, kill thresholds

Salveaza in `~/.nexus/projects/smsads/niche-validation-{vertical}-{date}.md`.

### Pas 8: COMPLIANCE PRE-CHECK PER GEO

Inainte de commit la vertical, verifica compliance pe fiecare GEO target:

| GEO | Gambling | Nutra | Dating | Finance |
|-----|---------|-------|--------|---------|
| Bolivia | Grey (no specific law) | OK | OK | OK |
| Peru | Legal (MINCETUR) | OK cu disclaimers | OK 18+ | Regulated SBS |
| Kenya | BCLB license required | OK | OK 18+ | Regulated CBK |
| Chile | Legal (SCJ) | OK cu disclaimers | OK | Regulated CMF |
| Romania | ONJN license required | Restricted health claims | OK 18+ | Regulated BNR |

**Eroare frecventa**: Assumarea ca "grey market" = fara restrictii. Bolivia nu are lege specifica gambling, dar carrier-ul poate refuza SMS cu continut gambling explicit. Verifica MEREU cu VOX/carrier approval inainte de lansare.

### Pas 9: PIVOT PLAN SI ALTERNATIVE

Defineste pivot plan daca verticalul #1 esueaza in test:
- Vertical #2 din scoring (deja validat, ready to test)
- Vertical #3 ca backup final
- Buget maxim de alocat pe testare inainte de pivot: 3x payout-ul ofertei
- Timeline: 7 zile test per vertical, max 21 zile pentru decizia finala

**Eroare frecventa**: Abandonarea prea devreme (1 zi de test) sau prea tarziu (30 zile fara profit). Regula: minim 72 ore de date, maxim 7 zile fara ROI pozitiv = pivot.

---

## Verificare

- [ ] Minim 12 verticale evaluate cu CPA benchmarks reale?
- [ ] Disponibilitate oferte verificata pe cel putin 3 CPA networks?
- [ ] Scoring compozit calculat pe 7 dimensiuni pentru top 5?
- [ ] Unit economics calculate per GEO pentru top 3?
- [ ] Test de piata rulat 72 ore cu tracker configurat?
- [ ] Compliance pre-check completat pentru toate GEO-urile target?
- [ ] Pivot plan documentat cu 2 alternative?
- [ ] Decizia finala salvata cu toate datele suport?

---

## Instrumente

| Tool | Rol |
|------|-----|
| MaxBounty / ClickDealer / Mobidea / Advidi / 3SNET | Verificare disponibilitate oferte si EPC |
| Adplexity / Anstrex | Analiza competitiva ads (push/native) |
| SpyFu / SEMrush | Competitie SEO pe buyer-intent keywords |
| Voluum / BeMob | Tracking test de piata |
| Google Trends | Validare sezonalitate si trend |
| SimilarWeb | Estimare trafic competitori |
| VOX dashboard | Costuri SMS per GEO si carrier approval |

---

## Note

- Procedura este prerequisite pentru M-002, M-006, M-007. Nu avansa fara nisa validata.
- Re-executie obligatorie la: adaugare GEO nou, schimbare legislatie, sau dupa 90 zile fara re-validare.
- Scorurile CPA din tabel sunt benchmarks 2025-2026. Actualizeaza trimestrial din network dashboards.
- Gambling ramane verticalul cu cel mai mare CPA dar si cel mai restrictiv. Pentru entry, sweepstakes ($1-5 CPA, low compliance) sunt ideale ca training ground inainte de gambling.
