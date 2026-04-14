---
type: procedure
created: 2026-03-20
status: active
slug: smsads-p3-competitor-intel
tags: [procedure, nexus]
---

# SMSADS-P3 — Competitor Intelligence for SMS Carrier Marketing

**Domeniu**: affiliate | **Sub-domeniu**: smsads-competitor-intel
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Scope**: Intelligence competitiv sistematic per tara + verticala pentru SMSads — identificarea afiliatilor SMS activi in GEO, ofertele promovate, canalele de trafic folosite si keyword gaps exploatabile, folosind Ahrefs (primary) / SEMrush (alternativă), SimilarWeb, AdBeat/Taboola spy si semnalul de trafic direct (indiciu SMS/push ascuns).

---

## 1. Problema

### De ce exista aceasta procedura

SMSads opereaza cu zero vizibilitate competitiva structurata. Toate deciziile de lansare se iau in vid informational: nu stim cine altcineva trimite SMS in Bolivia sau Romania pe verticale similare, ce oferte promoveaza, ce copy angles folosesc si ce keyword gaps exista.

**Gap-urile de intelligence actuale (date concrete)**:

| Gap | Impact Cuantificabil |
|---|---|
| Zero monitoring competitori SMS Bolivia | Nu stim daca exista 0 sau 10 afiliati SMS pe MarathonBet Bolivia — CR estimat poate fi deja diminuat de saturatia |
| Zero analiza LP-uri competitori | Scriem copy SMS din imaginatie; competitorii cu 12+ luni de date A/B au unghiuri testate pe audienta locala |
| Zero keyword intelligence | Nu stim ce termeni in spaniola/romana convertesc pentru betting/trading — pierdem segment pregatit de cumparat |
| Zero spy pe canale native/display | AdBeat/Taboola pot arata daca un vertical e monetizat agresiv = validare ca merita si SMS |
| Zero STM Forum monitoring | Insight-uri despre CR real, probleme de tracking, oferte noi apar pe STM cu saptamani inainte sa ajunga pe emailul nostru |

**Cost al lipsei de intelligence**:
- Lansam pe MarathonBet Bolivia cu CTR/CR estimat; daca un competitor a saturat deja audienta, CTR-ul nostru real va fi 50-70% din estimare → EC/1K SMS real <<< estimat
- Nu exploatam keyword gaps: termeni in spaniola cu intent comercial ridicat + zero competitie SEO = oportunitate SMS direct-response cu CR ridicat
- Pierdem oferte nou listate pe 3SNET/MaxBounty descoperite de competitori cu 2-4 saptamani inainte

**WHY NOW**: Bolivia Test 2 (300K SMS) e planificat. Romania se deblocheaza April. Fara intelligence competitiv, rulam orb pe volume de 10-20x mai mari decat Test 1.

### Situatii acoperite
- Pre-lansare campanie noua intr-un GEO + vertical
- Evaluare saturatie competitor inainte de scale (crestere volume SMS)
- Discovery oferte noi prin reverse engineering competitori
- Identificare keyword gaps pentru landing pages si copy SMS

---

## 2. Procedura

### Pas 1: Seed Competitor Identification

**Tool**: mcp__brave-search + mcp__tavily__tavily_search + SimilarWeb + Ahrefs / SEMrush (alternativă) + STM Forum

**Actiune**:
Identifica 5-10 site-uri afiliate active per vertical + GEO ca punct de plecare:

**Cautari Google cu operatori**:
```
"casino afiliado" OR "casino affiliate" "Bolivia" review bonus
"betting affiliate" Bolivia "MarathonBet" OR "Sportaza" OR "1xBet"
"forex affiliate" Bolivia OR Romania 2025
"mejores apuestas Bolivia" bonus registro
"pariere sportiva Romania" bonus afiliat
```

**SimilarWeb Industry Search**:
- Industry Analysis > Gambling / Finance > GEO tinta (Bolivia / Romania)
- Filtreaza: Traffic Source "Search" > 40% (SEO affiliates) SAU "Direct" > 40% (potential SMS/push)
- Exporta top 20 domenii cu traffic estimat

**SEMrush / Ahrefs Seed Keyword**:
- "casino affiliate Bolivia" → SERP Analysis → domenii care rankeza
- "betting Romania afiliat" → SERP → domenii
- Exporta primele 10 rezultate per keyword

**STM Forum**:
- Search: "SMS Bolivia", "SMS LATAM", "Bolivia betting", "Romania SMS affiliate"
- Filtre: thread-uri cu >5 raspunsuri (activitate reala)
- Noteaza domenii mentionate, networkuri recomandate, volume declarate

**Output template**:

| Domain | SimilarWeb Visits/mo | Primary Vertical | Primary GEO | Traffic Source (top%) | Affiliate Network | Key Offer | SMS Evidence |
|---|---|---|---|---|---|---|---|
| affiliate-bo.com | 85,000 | Betting | Bolivia | Organic 62% | 3SNET | MarathonBet BO | NU |
| bo-casino-reviews.net | 18,000 | Betting/Casino | Bolivia | Direct 58% | 3SNET est | 1xBet BO | DA (LP minimal, URL scurt) |
| trading-ro.com | 120,000 | Trading/Forex | Romania | Organic 78% | MaxBounty | XTB Romania | NU |

**Output (artifact)**: `smsads-competitor-seeds-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 5 competitori identificati per GEO + vertical. FAIL daca toti competitorii vin dintr-o singura sursa de cautare.

---

### Pas 2: Keyword Intelligence per Competitor

**Tool**: Ahrefs Site Explorer / SEMrush Organic Research (alternativă) + mcp__brave-search

**Actiune**:
Pentru top 3 competitori din Pas 1:

1. Acceseaza SEMrush: Domain Overview > Organic Research > Positions > Export CSV
   (sau Ahrefs: Site Explorer > Organic Keywords > Export CSV)
2. Filtreaza top 50 keywords dupa trafic estimat
3. Clasifica obligatoriu per keyword:

| Dimensiune | Categorii |
|---|---|
| Intent | Commercial / Informational / Transactional |
| Produs promovat | betting / casino / trading / health / apps / alt |
| GEO Specificity | Generic / GEO-specific (Bolivia/Romania) |
| Volume | Cautari lunare (nr.) |
| KD | Keyword Difficulty 0-100 |

**Signal de interpretat**:
- Competitor rankeza pe 10+ keywords "trading Bolivia/Romania" cu KD >25 → a investit serios → vertical probabil profitabil
- Keywords GEO-specific cu KD <20 si volume >200/luna = GAP exploatabil: daca competitorul e slab plasat acolo, SMS direct-response pe acel segment are CR potential ridicat

**Output (artifact)**: `smsads-competitor-keywords-{domain}-{YYYY-MM}.csv`
**Checkpoint PASS**: 50 keywords extrase per competitor, Intent 100% completat (zero celule goale). FAIL daca <3 competitori analizati sau Intent incomplet.

---

### Pas 3: Offer Reverse Engineering

**Tool**: Browser direct + WhereGoes.com (redirect logger) + mcp__brave-search + Google Cache

**Actiune**:
Viziteaza top 3 landing pages per competitor (cele mai accesate din SimilarWeb "Top Pages" sau Ahrefs "Top Pages by Traffic"):

Pentru fiecare LP, documenteaza:
1. **Oferta promovata**: brand + produs (ex: MarathonBet Bolivia, 1xBet BO, SpinBetter RO)
2. **Network link**: hover pe CTA sau click cu redirect logger → identifica:
   - 3SNET: `go.3snet.co/` sau `go.gmatrckcf.info/click?pid=`
   - MaxBounty: `tracking.maxbounty.com/`
   - HasOffers: `tracking.{network}.com/aff_c?offer_id=`
   - CAKE: `{brand}.go2jump.org/`
3. **Bonus promovat**: "100% bonus up to $100" → indicator ca comisionul suporta marja bonusului
4. **Structura LP**: direct-to-operator (1 click) vs pre-sell page (articol/review) vs minimal SMS LP (zero navigare)

**Cross-referinta comision**:
- Cauta oferta identificata pe 3SNET + MaxBounty
- Compara CPA/RevShare disponibil public cu ce promoveaza competitorul
- Daca competitorul promite bonus mare dar CPA oficial e mic → probabil are deal RevShare customizat; contacteaza AM pentru acelasi deal

**Output template**:

| Competitor | LP URL | Offer Name | Network Identificat | Comision Est. | Trafic LP (SimilarWeb) | Bonus Promovat | LP Type |
|---|---|---|---|---|---|---|---|
| affiliate-bo.com | /marathonbet-review | MarathonBet Bolivia | 3SNET | RevShare ~30% | 12,000/mo | Bonus 100% pana $100 | Pre-sell review |
| bo-casino-reviews.net | /1xbet | 1xBet Bolivia | 3SNET est | CPA $12 | 5,500/mo | $10 no-deposit | SMS LP minimal |

**Output (artifact)**: `smsads-competitor-offers-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 3 oferte identificate cu network source confirmat, minim 1 cu comision >$20 CPA sau RevShare >20%. FAIL daca nicio oferta nu are network identificat (doar branded URL).

---

### Pas 4: Traffic Source Analysis si SMS Signal Detection

**Tool**: SimilarWeb Traffic Sources report + mcp__brave-search

**Actiune**:
Pentru toti 5 competitori din seed list, extrage din SimilarWeb:
- Organic Search % / Direct % / Referral % / Social % / Paid Search % / Mail %

**Interpretare pattern-uri**:

| Pattern | Interpretare | Actiune |
|---|---|---|
| Organic >60% | SEO afiliat pur | Analizeaza keywords si LP-uri complet (Pas 2-3) |
| Direct >40% + trafic >10K/luna | Brand puternic SAU trafic SMS/push ascuns | Investigheza LP-urile (Pas 5 SMS Discovery) |
| Referral >30% | Trafic din retele afiliate sau backlinks | Urmareste link-urile referrer cu SimilarWeb Referrals |
| Paid Search >30% | Campanii PPC active | Vertical confirmat profitabil; noteaza ofertele din ads |
| Social >25% | Social ads sau organic social | Verifica Facebook Ads Library pentru ofertele promovate |
| Mail >10% | Email list activa | Competitorul are lista proprie; indicator de long-term player |

**SMS Hidden Indicator** — semnal principal de afiliat SMS activ:
- Direct > 40% + Referral > 20% + trafic >10K/luna = PROBABIL SMS/push afiliat
- LP-urile lor sunt template pentru copy SMS direct aplicabil

**Output template per competitor**:
```
Domain: {domain}
Monthly Visits: {X,XXX}
Traffic Breakdown: Organic {X}%, Direct {X}%, Referral {X}%, Social {X}%, Paid {X}%
SMS Hidden Indicator: DA/NU
Motivare: [pattern identificat]
```

**Output (artifact)**: `smsads-traffic-analysis-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Traffic source breakdown complet pentru toti 5 competitori. Minim 1 competitor evaluat pentru SMS Hidden Indicator (cu justificare). FAIL daca doua sau mai multe coloane sunt goale per competitor.

---

### Pas 5: SMS-Specific LP Discovery

**Tool**: mcp__brave-search + mcp__tavily__tavily_search + STM Forum + Google advanced operators

**Actiune**:
Identifica landing page-uri SMS ale competitorilor — distinct de site-urile SEO:

**Google Search operatori avansati**:
```
"MarathonBet" "promo" site:bit.ly OR site:t.co OR short.link
"casino" "bonus" "SMS" "Bolivia" 2025 OR 2026
"1xBet" "registro" Bolivia SMS
"SpinBetter" OR "casino" "bonus" "Romania" SMS
"apuestas" "Bolivia" "click aqui" -site:marathonbet.com
"pariere" "Romania" "inregistreaza" SMS
```

**Caracteristici LP SMS identificabil**:
- URL scurt sau aleator (nu keyword-friendly, ex: `go.affiliateX.net/bo123`)
- Continut minimal: logo + tagline + un singur buton CTA
- Zero navigare, zero footer, zero blog sau meniu
- Single conversion action (click la offer sau colectare numar de telefon)
- Load time <2s (optimizat pentru 3G mobil)
- Parametri de tracking vizibili in sursa: `utm_source=sms`, `?seg=`, `?clickid=`

**STM Forum**:
- Search: "SMS Bolivia", "SMS LATAM betting", "Bolivia push traffic"
- Filtre: thread-uri cu atasamente sau link-uri externe (LP-uri partajate)
- Noteaza: copy angles, volume declarate, probleme de tracking mentionate

**Documenteaza per LP gasit**:
- URL + redirect final
- Copy principal (headline + CTA text)
- Oferta promovata si network
- Structura vizuala (screenshot daca posibil)
- Estimare volum (daca tracker e vizibil in sursa)

**Output (artifact)**: `smsads-competitor-lps-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 3 SMS-specific LP-uri identificate per GEO. FAIL acceptat cu motivare: "Piata SMS imatura in GEO {X} — oportunitate first-mover" (in caz ca piata e cu adevarat virgin).

---

### Pas 6: Keyword Gaps Analysis

**Tool**: Ahrefs Content Gap / SEMrush Keyword Gap (alternativă) + mcp__brave-search

**Actiune**:
Identifica keyword-urile comerciale cu intent ridicat in GEO + vertical pe care:
- Competitorii rankeza = cerere validata
- SMSads NU are prezenta = gap de exploatat

**Metodologie**:
1. In Ahrefs: Content Gap → compara domeniul SMSads (daca exista ranking) vs. top 3 competitori
2. In Ahrefs: Content Gap → acelasi setup
3. Daca SMSads nu are prezenta organica: analizeaza direct competitorii si extrage keywords cu:
   - Volume >100/luna in GEO
   - KD <30 (accesibil)
   - Intent: Commercial / Transactional

**Interpretare pentru SMS**:
- Keyword cu volume ridicat + intent transactional + zero competitie SMSads = audienta pregatita de actiune → CTR SMS potential ridicat pe aceasta audienta
- Daca audienta in acel segment IAB VOX exista (ex: "Business&Finance" Romania = 2.1M) → keyword gap confirma ca audienta e activa si cautand produsul

**Output template**:

| Keyword | Volume/luna | KD | Intent | Competitor Rank | Smsads Rank | IAB Segment VOX | Action |
|---|---|---|---|---|---|---|---|
| "casino bonus Bolivia" | 590 | 28 | Commercial | affiliate-bo.com #3 | NU | Arts&Entertainment | SMS copy + LP pe acest termen |
| "trading forex Romania" | 1,200 | 35 | Commercial | trading-ro.com #2 | NU | Business&Finance | SMS audienta B&F RO + LP pe acest termen |
| "casino sin deposito Bolivia" | 320 | 18 | Transactional | (nimeni) | NU | Arts&Entertainment | GAP activ — intrare imediata |

**Output (artifact)**: `smsads-keyword-gaps-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 5 keyword gaps identificate cu Volume + KD + IAB Segment VOX completate. FAIL daca Action column e goala pentru orice keyword.

---

### Pas 7: Competitor Intelligence Brief

**Tool**: Claude Opus (sinteza) + mcp__cortex__cortex_store

**Actiune**:
Sintetizeaza outputs Pasii 1-6 intr-un brief acationabil:

**Structura obligatorie brief**:
```markdown
# Competitor Intel Brief — {vertical} {GEO} — {YYYY-MM-DD}

## Afiliati SMS Activi Identificati
| Competitor | SMS Evidence | Volume Est | Offer Promovat | Network |
|...|

## Oferte Promovate de Competitori (reverse engineered)
| Offer | Network | Comision Est | LP Type | Angle Copy |

## Canale de Trafic Dominante
[SEO % / SMS/Push % / Native % / Social %]

## Top 5 Keyword Gaps Actionabile
| Keyword | Volume | IAB Segment VOX | Priority |

## Copy Angles Capturate de la Competitori
1. Angle: "..." | Sursa: {competitor} | Aplicare: {offer/vertical}
2. ...

## Actiuni Recomandate (cu Owner + Deadline)
| # | Actiune | Owner | Deadline | Prioritate |
| 1 | Aplica pentru oferta {X} pe {network} | Pafi | +3 zile | HIGH |
| 2 | Testeaza copy angle {Y} pe segment IAB {Z} | Kody | +7 zile | HIGH |
```

**Prompt Opus pentru sinteza** (ruleaza cu datele din Pasii 1-6):
```
Esti expert in affiliate marketing si competitive intelligence pentru SMS carrier marketing.
Ai primit intelligence competitiv pentru {GEO} + vertical {vertical}.
Sintetizeaza: afiliati SMS activi, oferte promovate, keyword gaps, copy angles.
Format: brief acationabil cu action items, Owner + Deadline.
Date input: [paste outputs Pasii 1-6]
```

**Output (artifact)**: `smsads-competitor-brief-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Brief contine: afiliati SMS identificati + oferte reverse-engineered + 5 keyword gaps + 3 copy angles + action items cu Owner + Deadline. FAIL daca lipseste oricare sectiune.

---

## 3. Cortex Logging

```json
{
  "text": "SMSADS-P3 executat: competitor intel {vertical} {GEO} {YYYY-MM}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SMSADS-P3-COMPETITOR-INTEL",
    "rule_id": "TRAIN-S-001",
    "domain": "smsads",
    "sub_domain": "competitor-intelligence",
    "version": "1.0",
    "tags": ["smsads", "competitor-intel", "SEO", "SMS", "SimilarWeb", "Ahrefs", "AdBeat", "Bolivia", "Romania"],
    "geo_analyzed": "",
    "vertical_analyzed": "",
    "competitors_found": 0,
    "sms_affiliates_identified": 0,
    "offers_reverse_engineered": 0,
    "keyword_gaps_found": 0,
    "copy_angles_captured": 0,
    "brief_path": "",
    "execution_date": "",
    "executed_by": "",
    "artifacts": {
      "competitor_seeds": "smsads-competitor-seeds-{vertical}-{GEO}-{YYYY-MM}.md",
      "competitor_keywords": "smsads-competitor-keywords-{domain}-{YYYY-MM}.csv",
      "competitor_offers": "smsads-competitor-offers-{vertical}-{GEO}-{YYYY-MM}.md",
      "traffic_analysis": "smsads-traffic-analysis-{GEO}-{YYYY-MM}.md",
      "competitor_lps": "smsads-competitor-lps-{GEO}-{YYYY-MM}.md",
      "keyword_gaps": "smsads-keyword-gaps-{vertical}-{GEO}-{YYYY-MM}.md",
      "competitor_brief": "smsads-competitor-brief-{vertical}-{GEO}-{YYYY-MM}.md"
    }
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Pre-lansare orice campanie noua cu volume >100K SMS pe un vertical + GEO
- Trigger periodic: lunar (prima zi a lunii) per GEO activ
- Trigger on-demand: la primirea unui brief de la 3SNET AM despre "oferta populara in GEO"
- Pre-scale: inainte de cresterea volumului cu >3x fata de ultima campanie

### WHEN
- Trigger pre-lansare: executie in max 72h inainte de prima trimitere
- Trigger lunar: executie in prima saptamana a lunii
- Trigger on-demand: max 48h de la trigger

### HOW (violation detection)
- VIOLATION: Scale la >300K SMS pe un vertical fara SMSADS-P3 completat = BLOCARE
- VIOLATION: Campanie noua in GEO nou fara competitor brief = BLOCARE
- WARNING: Competitor brief neactualizat >30 zile per GEO activ = WARNING
- WARNING: Brief fara copy angles capturate (minim 3) = WARNING
- Runner: verificare la session start Genie + notificare la 30 zile

### CONNECT
- **SMSADS-P1** (Vertical Discovery) — upstream: verticalele selectate in P1 trigger P3 pentru GEO respectiv
- **SMSADS-P2** (Product Finder) — paralel: ofertele reverse-engineered din P3 intra in master offer list P2
- **SMSADS-P4** (Network Discovery) — complementar: networkurile identificate la competitori in P3 → evaluate in P4
- **M-122** (SMS Vertical Discovery) — similar: P3 extinde M-122 cu intelligence competitiv specific SMSads
- **M-126** (Competitor SEO Intel) — procedura similara generala; SMSADS-P3 e specializarea per pipeline SMSads
- **M-125** (VOX Taxonomy Segmentation) — copy angles capturate in P3 alimenteaza Step 3 din M-125
- `procedure-health.json` → adauga entry: `SMSADS-P3-COMPETITOR-INTEL`

### VERIFY
- [ ] Procedura executata complet? (toti pasii 1-7 cu artifacts documentate)
- [ ] Output satisface criteriile? (minim 5 competitori + oferte reverse-engineered + 5 keyword gaps + brief complet)
- [ ] VK emis in sesiune? (ambele linii vizibile pentru Pafi)
- [ ] Daca oricare = NU → procedura NU e completa

**Doua VK-uri obligatorii (VK-H-001)**:
1. `[PROC] SMSADS-P3 | §1 §2 §3 §4 | {GEO}/{vertical} | {N} competitori | {M} keyword gaps | complete`
2. `[CORTEX] "SMSADS-P3: competitor intel {vertical} {GEO}" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Pas 1: Seed competitor identification | Sonnet | Cautari structurate, agregare date |
| Pas 2: Keyword intelligence | Sonnet | Export CSV + clasificare tabelara |
| Pas 3: Offer reverse engineering | Sonnet | Documentare LP-uri + network identification |
| Pas 4: Traffic source analysis + SMS signal | Opus | Interpretare pattern-uri complexe de trafic + judecata calitativa SMS indicator |
| Pas 5: SMS LP discovery | Sonnet | Cautari structurate, documentare LP-uri |
| Pas 6: Keyword gaps analysis | Sonnet | Comparatie tabelara, filtrare mecanica |
| Pas 7: Brief synthesis | Opus | Sinteza multi-sursa + rationament strategic + action items |

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|---|---|---|
| Ahrefs (plan Standard+) | Site Explorer + Content Gap + Keyword research | ahrefs.com |
| SEMrush (alternativă Ahrefs) | Organic keyword research + keyword gap | semrush.com |
| SimilarWeb Pro | Traffic source analysis per competitor | similarweb.com |
| AdBeat / Taboola Spy | Native/display ads per vertical GEO | adbeat.com |
| AdPlexity Mobile (optional) | SMS/mobile ad spy | adplexity.com/mobile |
| STM Forum (membership activ) | SMS affiliate insights, LP sharing | stmforum.com |
| SMSADS-P1 | Upstream: furnizeaza GEO + vertical | `SMSADS-P1-vertical-discovery.md` |
| SMSADS-P2 | Paralel: primeste oferte reverse-engineered | `SMSADS-P2-product-finder.md` |
| M-125 | Downstream: primeste copy angles | `M-125-vox-taxonomy-segmentation.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---|---|---|
| Competitori SMS activi identificati per GEO | Gradul de saturatie real | Actualizat lunar; trend documentat |
| Keyword gaps actionabile identificate | Oportunitate de targeting | Minim 5 per run |
| Copy angles capturate per run | Calitate competitive intel | Minim 3 per run |
| SMS LP-uri competitori descoperite | Maturitate piata SMS | Minim 3 per GEO (0 = piata imatura = first-mover) |
| Timp executie SMSADS-P3 | Eficienta procedurii | <4 ore per GEO + vertical |
| Oferte noi descoperite prin reverse engineering | Pipeline expansion | Minim 1 oferta noua per run intrata in SMSADS-P2 |

---

## Checklist Pre-Publicare

- [x] Regula asociata: TRAIN-S-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu 4 checks obligatorii
- [x] Doi VK-uri obligatorii per VK-H-001 specificati
- [x] MODEL ROUTING prezent cu justificare per pas
- [x] Logica SMS Hidden Indicator (Direct >40% = potential SMS afiliat ascuns) documentata
- [x] Checkpoints PASS/FAIL per pas
- [x] Brief template obligatoriu cu structura completa (Pas 7)
- [x] Conexiuni la SMSADS-P1, P2, P4, M-125, M-126 documentate
- [x] Limbaj romana cu termeni tehnici in engleza
