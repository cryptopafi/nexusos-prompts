---
type: procedure
created: 2026-03-25
status: active
slug: m-137-sms-product-discovery
tags: [procedure, nexus]
---

# M-137 — SMS Product Discovery: Multi-Channel Demand Mining

**Domeniu**: affiliate | **Sub-domeniu**: sms-product-discovery
**Versiune**: 1.0 | **Data**: 2026-03-23
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Replaces**: SMSADS-P0 (Market Discovery)
**Compatible with**: SMSADS-P1, P2, P3, P4 (until deprecated)
**Scope**: Descoperirea sistematica a celor mai profitabile produse/oferte de promovat prin SMS carrier marketing, prin mineritul a 7+ canale de date gratuite (Google Ads, Meta Ads, keyword volume, Google Trends, App Store, X, retele afiliate), cross-referinta demand vs supply, si ranking final cu EC/1K SMS. Sistem viu — scorurile se actualizeaza dupa fiecare campanie.

---

## 1. Problema

### De ce exista aceasta procedura

Pipeline-ul SMSADS actual (P0-P4) descopera produse intreband AI-ul "ce ar putea functiona?" in loc sa mineze date reale pentru a gasi "ce FUNCTIONEAZA deja?". Nu exista niciun proces care sa analizeze ce se promoveaza activ prin Google Ads, Meta, sau alte canale intr-o tara si sa cross-referinteze cu ofertele disponibile pe retelele afiliate.

**Consecinte concrete ale lipsei acestei proceduri**:
- Selectia verticalelor se face reactiv (ce propune 3SNET AM) nu proactiv (ce converteste in piata)
- Produse cu demand masiv in Bolivia (ex: apps cu milioane de descarcari, trading cu Google Ads activ) raman nepromovate prin SMS
- Competitia SMS e aproape zero in Bolivia — dar fara demand mining, nu stii CE sa promovezi, doar CA poti promova
- Oportunitate pierduta: produse care genereaza profit prin Google Ads/Meta in Bolivia vor genera profit si prin SMS (acelasi market, canal diferit)

> **Caveat**: Semnalul cross-channel (Google Ads → SMS) e un proxy, nu o garantie. Google Ads targeteaza intent activ (cautare), SMS targeteaza audienta pasiva. De aceea Pas 14 (Validation Send) e obligatoriu — confirma cu date reale inainte de scale.

- Dependenta de un singur vertical (gambling/betting) si o singura retea (3SNET)

**Situatii acoperite de aceasta procedura**:
- Intrare pe piata noua VOX (Bolivia, Romania, Myanmar)
- Diversificarea portofoliului de oferte pe GEO activ
- Refresh trimestrial al oportunitatilor de produse
- Evaluarea unui vertical/produs nou propus de echipa sau AM
- Pre-lansare pe GEO nou: scanare completa inainte de prima campanie

**WHY NOW**: Bolivia are audienta masiva subutilizata (693K Technology, 523K Software, 457K Apps). Romania se deblocheaza. Fiecare saptamana fara demand mining = pierdere de oportunitate cuantificabila in verticale profitabile dincolo de gambling.

---

## 2. Arhitectura si Procedura

### Channel Tiers

**Tier 1 — MANDATORY (semnal direct puternic)**:
1. Google Ads Transparency Center — cine PLATESTE pentru ads in GEO? (weight: 0.20)
2. Meta Ad Library — cine PLATESTE pentru social ads in GEO? (weight: 0.15)
3. Affiliate Network Dashboards — ce oferte exista, sortate dupa EPC? (weight: 0.20)

**Tier 2 — RECOMMENDED (semnal suport)**:
4. Google Keyword Planner — volum cautari per vertical (weight: 0.15)
5. Google Trends — directie trend + sezonalitate (weight: 0.10)
6. Google Play Store top charts — cerere apps per categorie (weight: 0.08)
7. X Ads Transparency + Trending — activitate advertiseri + demand spikes (weight: 0.12)

**Tier 3 — v2 (se adauga dupa validarea v1)**:
8. TikTok Creative Center
9. MercadoLibre / marketplace-uri e-commerce

**Enhancement layers platite (v1.1)**:
- SEMrush ($130/mo): imbunatateste canalele 1, 4 (PPC spy + keyword depth)
- SimilarWeb ($149/mo): adauga canal nou (traffic source analysis, SMS hidden indicator)
- Ahrefs ($99/mo): imbunatateste canalul 4 (keyword research + content gaps)

### Flow executie

```
FAZA A: MINING PARALEL (~3-4 ore)
├── TRACK DEMAND (canale 1,2,4,5,6,7)
│   ├── Pas 1: Google Ads Transparency
│   ├── Pas 2: Meta Ad Library
│   ├── Pas 3: Google Keyword Planner
│   ├── Pas 4: Google Trends
│   ├── Pas 5: Google Play Store
│   └── Pas 6: X Ads + Trending
│
└── TRACK SUPPLY (canal 3)
    └── Pas 7: Network Mining (3SNET/MaxBounty/ClickDealer/Mobidea/PerformCB)

VERIFICARE 1 (optional, recomandat la GEO nou)
└── Pas 7b: Verificare Manuala Date (Leo/Kody) — accuracy check pe 18+ data points

FAZA B: CROSS-REFERENCE + SCORING (~1-2 ore)
├── Pas 8: Matrice Unificata Demand Signals
├── Pas 9: Match Demand ↔ Supply
├── Pas 10: Risk Marking (regulatory + payment)
└── Pas 11: Calcul EC/1K SMS

FAZA C: RANKING FINAL + PLAN LANSARE (~1 ora)
├── Pas 12: Scor Final de Prioritizare
├── Pas 13: Product Cards Top 5
└── Pas 14: Plan Validation Send

VERIFICARE 2 (optional, recomandat la GEO nou)
└── Pas 14b: Review Manual Rezultate (Leo/Kody) — 5-dim rating + improvement feedback

FAZA D: FEEDBACK LOOP (dupa fiecare campanie)
└── Pas 15: Inregistrare Rezultate + Actualizare Master File
```

---

## 3. Procedura

### FAZA 0: MARKET PROFILE (ruleaza O SINGURA DATA per GEO — rezultatele se cacheaza permanent)

---

### Pas 0: Market Profile — Cine sunt, ce vorbesc, cum cumpara?

**Tool**: Delphi D4 Deep Research (all scouts) — ruleaza INAINTE de orice alt pas
**Trigger**: Prima data cand se ruleaza M-137 pentru un GEO nou
**Cache**: Rezultatele se salveaza in `~/.nexus/projects/smsads/market-profiles/{GEO}.json` si nu se re-ruleaza decat la refresh anual

**De ce exista**: Fara acest pas, nu stii ce limba sa cauti, ce marketplaces exista, ce metode de plata functioneaza, sau ce nume de brand sunt locale. Toate pasii urmatori depind de aceste informatii.

**Actiune — Ruleaza un Delphi D4 research cu urmatorul prompt (adapteaza {COUNTRY}):**

```
Research the market fundamentals of {COUNTRY} for affiliate marketing. I need PERMANENT reference data:

1. LANGUAGE & SEARCH BEHAVIOR
   - Official language(s) and which one is used for online searches
   - Common slang/colloquial terms for commerce (e.g., "plata" instead of "dinero" in Bolivia)
   - Do users search in English or local language? What percentage?
   - Regional vocabulary differences (e.g., "celular" vs "smartphone")

2. E-COMMERCE & MARKETPLACES (ranked by traffic)
   - Top 5 e-commerce sites by traffic (e.g., MercadoLibre, SHEIN, Temu)
   - Local marketplaces specific to this country
   - Facebook Marketplace activity level (high/medium/low)
   - Top classifieds sites (OLX equivalent)
   - Main delivery/logistics companies

3. PAYMENT INFRASTRUCTURE
   - Credit card penetration (% of population)
   - Mobile wallets (name the dominant ones)
   - QR payment adoption
   - Carrier billing availability (which telcos)
   - COD (Cash on Delivery) feasibility
   - Crypto adoption level
   - International payment services available (PayPal, Payoneer, etc.)

4. MOBILE & TELECOM
   - Top 3 mobile carriers (by subscriber count)
   - Smartphone penetration %
   - Android vs iOS market share
   - Average data plan cost
   - SMS readership behavior (do people read SMS ads?)

5. DIGITAL ADVERTISING LANDSCAPE
   - Top advertising platforms used (Google, Meta, TikTok, local?)
   - Average CPC in this market (Google Ads)
   - Social media platforms ranked by usage (Facebook, Instagram, TikTok, X, WhatsApp)
   - Influencer marketing maturity level

6. REGULATORY & COMPLIANCE
   - Online gambling legality (legal/grey/illegal)
   - Health/nutra advertising restrictions
   - Financial product advertising rules
   - Data privacy laws (GDPR equivalent?)
   - SMS marketing regulations (opt-in requirements)

7. KEY LOCAL BRANDS & ADVERTISERS
   - Top 10 companies by revenue/market cap
   - Top advertisers on Google/Meta in this country
   - Local app ecosystem (banking apps, ride-hailing, food delivery)
   - Known affiliate programs from local brands

8. ECONOMIC INDICATORS
   - GDP per capita
   - Average monthly salary
   - Currency and exchange rate vs USD
   - Inflation rate (impacts pricing sensitivity)
   - Population and internet penetration

For each section, provide SPECIFIC names, numbers, and URLs — not generic statements.
```

**Output documentat**:

| Camp | Ce documentezi |
|---|---|
| Limba principala + cautari | Limba locala, slang-uri, % cautari in engleza |
| Top 5 e-commerce | Nume, URL, trafic estimat |
| Top 3 mobile carriers | Nume, nr. abonati, SMS capabilities |
| Payment methods | Lista ranked cu penetrare % |
| Credit card penetration | % exact |
| Top social media | Platforma, nr. useri, rang |
| Regulatory restrictions | Per vertical (gambling, nutra, finance, dating) |
| Currency + GDP/capita | Valori exacte |
| Top 10 companii locale | Nume, sector, potentialul de affiliate |

**Scor canal (1-5)**:
- 5 = Profil complet cu date specifice per tara, minim 50 data points
- 3 = Profil partial, lipsesc sectiuni
- 1 = Doar informatii generice care ar putea fi pentru orice tara

**Output (artifact)**: `~/.nexus/projects/smsads/market-profiles/{GEO}.json`
**Checkpoint PASS**: Toate 8 sectiunile completate cu date specifice. Limba locala identificata. Minim 3 marketplaces cu URL. Minim 3 payment methods. FAIL daca lipseste limba sau payment infrastructure.

**Cum se foloseste downstream**:
- Pas 1-7: Toate cautarile se fac in limba identificata aici
- Pas 3 (Keywords): Seed keywords includ termeni locali (nu traduceri generice)
- Pas 7 (Networks): Payment methods dicteaza ce tipuri de oferte sunt fezabile (COD, CC, mobile wallet)
- Pas 10 (Risk): Regulatory data de aici alimenteaza risk marking-ul
- Pas 11 (EC/1K): Currency si GDP/capita ajuta la calibrarea payout expectations

---

### FAZA A: MINING PARALEL

> Pasii 1-7 pot rula in paralel. Atribuie la sub-agenti sau executa secvential daca opereaza o singura persoana.
> Toate cautarile se fac in LIMBA LOCALA a GEO-ului tinta (identificata la Pas 0).
> **Exclusiuni universale**: NU mina keywords/ads pentru: adult, arme, tutun, pharma fara licenta. Aceste verticale sunt interzise universal pe toti carrierii SMS.
> **Privacy (EU GEOs)**: Pentru GEO-uri EU (Romania): documenteaza doar date publice din ad libraries. Nu colecta date personale din ad creatives.

---

### Prerequisite — Conturi si Acces Necesar

Inainte de a incepe, verifica accesul la:
- [ ] Cont Google Ads (pentru Keyword Planner) — ads.google.com
- [ ] Cont Facebook (pentru Meta Ad Library) — facebook.com/ads/library
- [ ] Dashboard 3SNET (#30063) — go.3snet.co
- [ ] Dashboard MaxBounty — maxbounty.com
- [ ] Dashboard ClickDealer — clickdealer.com (optional)
- [ ] Dashboard Mobidea — mobidea.com (optional)
- [ ] Dashboard Perform[cb] — performcb.com (optional)
- [ ] Brave Search MCP activ — mcp__brave-search
- [ ] Perplexity Pro (via OpenRouter API) — primary research engine for Steps 1-7
- [ ] Exa API — financial reports and category-filtered searches
- [ ] Tavily API — deep crawling and URL extraction (backup)

Nu e necesar cont pentru: Google Ads Transparency, Google Trends, Google Play Store, X Ads Transparency.

**Timp estimat per pas**:
| Pas | Timp est. | Nota |
|-----|-----------|------|
| 1-2 (Google+Meta Ads) | 45-60 min | Cautare manuala per vertical |
| 3 (Keywords) | 30-45 min | Necesita cont Google Ads |
| 4 (Trends) | 15-20 min | Rapid, interfata simpla |
| 5 (Play Store) | 20-30 min | Browsing manual |
| 6 (X) | 15-20 min | Skip daca GEO <5M useri X |
| 7 (Networks) | 45-60 min | Depinde de nr. retele cu cont |
| 8-11 (Scoring) | 30-45 min | Calcule tabelar |
| 12-14 (Ranking) | 30-45 min | Sinteza + product cards |
| **Total** | **4-6 ore** | Tier 1 only: 2-3 ore |

**Prioritate Motoare de Cautare (in ordine)**:
1. **Perplexity Pro** (sonar-pro via OpenRouter) — PRIMARY: raspunsuri sintetizate cu citatii, cel mai bun raport calitate/cost
2. **Exa Advanced** — SPECIALIST: rapoarte financiare, lucrari academice, filtrare pe categorie
3. **Tavily Research** — DEEP: cercetare autonoma pe topicuri largi, crawling multi-pagina
4. **Brave Search** — FALLBACK: URL-uri raw, descoperire domenii, verificare rapida
5. **DuckDuckGo** — LAST RESORT: backup cand celelalte au cota epuizata

**Nota**: Brave Search free plan = 2,000 requests/luna. Recomandat upgrade la paid ($5/luna = 15,000 requests).

---

### Pas 1: Google Ads Transparency — Cine plateste pentru ads in GEO?

**Tool**: Google Ads Transparency Center (adstransparency.google.com) — gratuit, fara cont
**Tool optional**: SpyFu ($39/mo) sau SEMrush Advertising Research ($130/mo)

**Actiune**:
1. Acceseaza adstransparency.google.com
2. Filtreaza pe: Country = GEO tinta (ex: Bolivia)
3. Cauta pe rand fiecare vertical in limba locala:
   - Gambling/Betting: "apuestas deportivas", "casino online", "apostar"
   - Trading/Finance: "forex trading", "invertir dinero", "prestamos rapidos", "credito online"
   - Apps/Tech: "descargar aplicaciones", "VPN gratis", "antivirus"
   - Health: "suplementos", "bajar de peso", "farmacia online"
   - E-commerce: "comprar online", "ofertas", "descuentos"
   - Insurance: "seguro de vida", "seguro auto"
   - Education: "cursos online", "aprender ingles"
   - Dating: "citas online", "conocer gente"
4. Per vertical, documenteaza:

| Camp | Ce documentezi |
|---|---|
| Advertiser Name/Domain | Cine plateste (ex: avatrade.com, marathonbet.com) |
| Vertical | Categoria produsului |
| Nr. Ads Active | Cate reclame ruleaza simultan |
| Duration Active | De cat timp ruleaza (>30 zile = profitabil) |
| Ad Copy Themes | Ce beneficii/oferte promoveaza (bonus, free, discount) |
| Landing Page Type | Direct sale, lead form, app install, registration |

5. **Semnal SMS**: Daca un advertiser plateste >$1K/luna pe Google Ads in GEO pentru >3 luni = STRONG demand signal. Produsul converste, altfel ar opri.

**Scor canal (1-5)**:
- 5 = 5+ advertiseri activi in vertical, durate >30 zile
- 4 = 3-4 advertiseri activi, durate >14 zile
- 3 = 1-2 advertiseri activi
- 1 = Zero ads gasite in vertical
- 0 = Canal nescanat (exclus din normalizare)

**Output (artifact)**: `m137-google-ads-intel-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 5 verticale cautate cu rezultate documentate. Minim 10 advertiseri totali gasiti. FAIL daca cautarile s-au facut doar in engleza (obligatoriu limba locala).

---

### Pas 1b: Top Sites Ad Mining — Ce se promoveaza pe cele mai vizitate site-uri din GEO?

**Tool**: SimilarWeb (gratuit 5 lookups/zi) SAU Delphi D3 (scout-web + scout-domain)
**Tool alternativ**: Alexa/Semrush Traffic Analytics, Exa (category: company)
**Logica**: In loc sa cautam keywords si sa vedem cine face reclama, facem INVERS — gasim site-urile cu cel mai mare trafic din tara, le vizitam, si vedem CE se promoveaza pe ele (bannere, pop-ups, sidebar ads, affiliate links).

**De ce exista**: Multe oferte afiliate NU apar in Google Ads sau Meta Ad Library. Ele apar ca bannere pe site-uri locale, ca native ads in articole, sau ca pop-unders pe site-uri de streaming/download. Acest pas captureaza ceea ce toate celelalte canale pierd.

**Actiune**:

1. **Gaseste Top 50 site-uri per GEO**:
   - Delphi D3: "Top 50 most visited websites in {COUNTRY} 2025-2026 by traffic, split by category"
   - SAU SimilarWeb → Top Websites → Country = {GEO} → All Categories
   - SAU Exa: `category: company, userLocation: {GEO_CODE}, query: "most popular website {COUNTRY}"`

2. **Clasifica site-urile pe verticale**:

   | Categorie | Exemple Bolivia |
   |-----------|----------------|
   | News/Media | lostiempos.com, paginasiete.bo, eldeber.com.bo |
   | E-commerce | mercadolibre.com.bo, shein.com, temu.com |
   | Finance/Banking | bcp.com.bo, bmsc.com.bo, bancosol.com.bo |
   | Entertainment | youtube.com, netflix.com, local streaming |
   | Sports | marca.com, espn.com, sites locale de futbol |
   | Health | farmacorp.com, local health portals |
   | Education | universidades locales |
   | Government | gob.bo, impuestos.gob.bo |
   | Forums/Community | Facebook groups, Reddit Bolivia |

3. **Per site din Top 30, verifica**:

   | Camp | Ce cauti |
   |---|---|
   | Banner ads | Ce produse/servicii sunt promovate? Screenshot-uri. |
   | Sidebar ads | Bannere laterale — adesea affiliate (gambling, nutra, dating) |
   | Native ads | Articole sponsorizate sau "recomandate" (Taboola, Outbrain, MGID) |
   | Pop-ups/Pop-unders | Comune pe site-uri de streaming/download — arata oferte afiliate |
   | Affiliate links in content | Link-uri cu tracking params in articole (reviews, comparatii) |
   | Ad network identification | Verifica URL-urile bannerelor: tracking.xyz, ad.doubleclick, propellerads, etc. |
   | Advertiser names | Cine plateste pentru bannere? Brand direct sau afiliat? |

4. **Per banner/ad gasit, documenteaza**:
   - Advertiser (brand sau afiliat)
   - Product/Offer name
   - Vertical (gambling, nutra, trading, etc.)
   - Landing page URL (click pe banner → copiaza URL-ul)
   - Tracking parameters (aff_id=, click_id=, sub=, etc.)
   - Ad network (din URL: propellerads.com, adsterra.com, mgid.com, etc.)
   - Affiliate network (din tracking URL: daca contine mobidea, clickdealer, 3snet, etc.)

5. **Analiza speciala pentru site-uri de sports**:
   - Site-urile de sporturi din LATAM sunt PLINE de bannere de betting/gambling
   - Verifica: marca.com, espn.com versiuni locale, site-uri de futbol bolivian
   - Identifica CARE brand de betting apare cel mai des → acela are cel mai mare buget

6. **Analiza speciala pentru site-uri de download/streaming**:
   - Site-uri de torrente, streaming gratuit, APK download sunt heavy in:
     - VPN ads
     - Antivirus ads
     - Dating pop-unders
     - Gambling pop-unders
   - Aceste ads sunt adesea affiliate — verifica tracking URLs

**Scor canal (1-5)**:
- 5 = 30+ site-uri analizate, 10+ advertiseri unici identificati, 5+ affiliate networks detectate din tracking URLs
- 4 = 20+ site-uri, 5+ advertiseri, 3+ networks
- 3 = 10+ site-uri, 3+ advertiseri
- 1 = Sub 10 site-uri analizate
- 0 = Canal nescanat

**Output (artifact)**: `m137-top-sites-ads-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 20 site-uri verificate. Minim 5 bannere/ads cu URL-uri documentate. Minim 2 affiliate networks identificate din tracking URLs. FAIL daca doar homepage-uri verificate fara a intra pe pagini interioare (articole, categorii).

---

### Pas 2: Meta Ad Library — Cine plateste pentru social ads in GEO?

**Tool**: Meta Ad Library (facebook.com/ads/library) — gratuit, cont Facebook necesar
**Tool optional**: agent-browser (automatizare browsing)

**Actiune**:
1. Acceseaza facebook.com/ads/library
2. Filtreaza: Country = GEO tinta, Category = All, Status = Active
3. Cauta per vertical (aceleasi keywords ca la Pas 1, in limba locala)
4. Per vertical, documenteaza:

| Camp | Ce documentezi |
|---|---|
| Nr. Active Ads | Total reclame active in vertical |
| Nr. Unique Advertisers | Cati advertiseri distincti |
| Top 5 Advertisers | Numele/paginile cu cele mai multe ads |
| Ad Duration | Data start cea mai veche (longevitate = profitabilitate) |
| Creative Themes | Format (image/video/carousel), mesaj principal, CTA |
| SMS Copy Angles | Ce angles din ads pot fi adaptate pentru SMS copy |

5. **Semnal SMS**: >20 active ads de la >5 advertiseri = VALIDATED demand (scor 5). Ads active >30 zile = advertiser profitabil = produsul merge.

**Scor canal (1-5)**:
- 5 = >20 ads active, >5 advertiseri unici, durate >30 zile
- 4 = 10-20 ads, 3-5 advertiseri
- 3 = 5-10 ads, 2-3 advertiseri
- 1 = <5 ads sau 1 singur advertiser
- 0 = Canal nescanat

**Output (artifact)**: `m137-meta-ads-intel-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate verticalele din Pas 1 cautate si in Meta. Minim 3 verticale cu date. FAIL daca doar o categorie cautata.

---

### Pas 3: Google Keyword Planner — Ce cauta oamenii in GEO?

**Tool**: Google Keyword Planner (ads.google.com) — gratuit cu cont Google Ads
**Tool alternativ**: Brave Search (estimari volum) sau mcp__brave-search
**Tool optional**: SEMrush Keyword Magic ($130/mo) sau Ahrefs Keywords Explorer ($99/mo)

**Actiune**:
1. Logheaza-te in Google Keyword Planner
2. Seteaza: GEO = tinta, Language = limba locala
3. Per vertical whitelisted, defineste 5-10 seed keywords:
   - Bolivia (spaniola): "prestamos rapidos Bolivia", "casino online Bolivia", "descargar aplicaciones gratis", "forex trading Bolivia", "suplementos salud Bolivia", "vpn gratis", "comprar online Bolivia"
4. Per seed keyword, extrage top 20 related keywords dupa volum
5. Clasifica fiecare keyword dupa intent:
   - **I** = Informational ("que es forex") — IGNORA
   - **C** = Commercial ("mejor casino online Bolivia") — TINTA
   - **T** = Transactional ("descargar VPN gratis") — TINTA PRINCIPALA
   - **N** = Navigational ("MarathonBet Bolivia") — NOTA (brand awareness existent)
6. Concentreaza-te EXCLUSIV pe keywords C si T — acestia reprezinta oameni gata sa actioneze

**Scor canal (1-5)** — bazat pe volum total C+T keywords per vertical in GEO:
- 5 = >10,000 cautari/luna (STRONG demand)
- 4 = 5,000-10,000
- 3 = 2,000-5,000 (MODERATE demand)
- 2 = 500-2,000
- 1 = <500 cautari/luna (WEAK demand)
- 0 = Canal nescanat

**Output (artifact)**: `m137-keyword-demand-{GEO}-{YYYY-MM}.csv`
Coloane: Keyword | Volume/luna | Intent (I/C/T/N) | Vertical | KD (daca disponibil) | CPC est. | SMS Copy Potential (DA/NU)

**Checkpoint PASS**: Minim 50 keywords C+T documentate din minim 4 verticale. Volume din Google Keyword Planner (nu estimate). FAIL daca keywords sunt doar in engleza.

> **Nota piete mici**: Google Keyword Planner are acuratete redusa pentru piete sub 20M populatie (Bolivia ~12M). Volumele pot fi subestimate. Complementeaza cu Google Trends (Pas 4) si Brave Search pentru validare calitativa.

---

### Pas 4: Google Trends — Ce creste si ce scade in GEO?

**Tool**: Google Trends (trends.google.com) — gratuit, fara cont

**Actiune**:
1. Seteaza GEO = tinta, perioada = ultimele 12 luni
2. Per vertical, cauta 2-3 keywords reprezentative:
   - Betting: "apuestas deportivas", "casino online"
   - Trading: "forex", "invertir dinero"
   - Apps: "descargar aplicaciones", "mejores apps"
   - Health: "suplementos", "bajar de peso"
   - Loans: "prestamos rapidos", "credito online"
   - VPN: "vpn gratis", "vpn Bolivia"
3. Documenteaza per vertical:

| Camp | Ce documentezi |
|---|---|
| Trend Direction | RISING / STABLE / DECLINING |
| Peak Months | Lunile cu volum maxim + magnitudine |
| Seasonal Pattern | Ciclicitate anuala (da/nu, ce luni) |
| Breakout Queries | Queries cu crestere >5000% (oportunitati emergente) |
| Regional Interest | Ce departamente/orase au interes maxim |

4. **Timing lansare SMS per vertical**:
   - Betting: peak la evenimente sportive majore (Copa America, Liga local, FIFA)
   - Finance: peak la inceput de an (rezolutii), sezon taxe
   - E-commerce: Black Friday, Craciun, zile de salariu locale
   - Health: Ianuarie (rezolutii), pre-vara
   - Apps: constant, usor crescator in vacante scolare

**Scor canal (1-5)**:
- 5 = RISING trend + breakout queries + sezonalitate clara
- 4 = RISING trend, fara breakout
- 3 = STABLE trend
- 2 = STABLE dar descrescator pe termen scurt
- 1 = DECLINING trend
- 0 = Canal nescanat

**Output (artifact)**: `m137-trends-intel-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate verticalele analizate. Trend direction + peak months documentate. FAIL daca <3 verticale analizate sau fara date sezonalitate.

---

### Pas 5: Google Play Store — Ce descarca oamenii in GEO?

**Tool**: Google Play Store (play.google.com) — gratuit, browsing direct
**Tool optional**: data.ai (App Annie) free tier, Sensor Tower free tier

**Actiune**:
1. Acceseaza Google Play Store, seteaza tara = GEO tinta
2. Browse Top Charts per categorie relevanta:
   - **Top Free Apps** — overall si per categorie
   - **Top Grossing Apps** — indica unde curg banii (CRITICAL)
   - **Trending Apps** — cerere in crestere
3. Documenteaza top 10 apps per categorie relevanta:
   - Finance: banking, trading, crypto, loans
   - Entertainment: streaming, games, gambling
   - Health & Fitness: supplements, workout, wellness
   - Shopping: e-commerce, marketplace
   - Tools: VPN, antivirus, productivity
4. Per categorie, noteaza:
   - Install count ranges (100K+, 1M+, 10M+)
   - Model monetizare (ads, in-app purchase, subscription, free)
   - Rating mediu
5. Cross-referinta: exista oferte CPI (Cost Per Install) pe retele afiliate pentru aceste apps sau similare?

**Semnal SMS**:
- Categorie cu 5+ apps cu 1M+ installs in GEO = HIGH demand vertical
- Top Grossing dominat de finance/gaming = useri dispusi sa cheltuiasca bani = bun pentru CPA offers
- Trending apps intr-o categorie noua = emerging opportunity

**Scor canal (1-5)**:
- 5 = Categorie cu 5+ apps cu 1M+ installs, oferte CPI disponibile
- 4 = 3-4 apps populare, potential CPI
- 3 = Activitate moderata in categorie
- 1 = Categorie slaba sau apps irelevante pentru SMS
- 0 = Canal nescanat

**Output (artifact)**: `m137-appstore-intel-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Top charts browsate din minim 4 categorii. Top Grossing verificat (crucial pentru spending behavior). FAIL daca Top Grossing nescanat.

---

### Pas 6: X (Twitter) Ads + Trending — Ce e activ pe X in GEO?

**Tool**: X Ads Transparency Center (ads.x.com) — gratuit + X Trending tab + Brave Search
**Tool alternativ**: mcp__brave-search pentru cautari X posts

**Efficiency gate**: Daca GEO are <5M useri X (Bolivia ~2M), limiteaza Pas 6 la 15 min max. Concentreaza-te pe X Ads Transparency si brand presence check. Skip affiliate chatter search.

**Actiune**:
1. **X Ads Transparency Center** (ads.x.com):
   - Filtreaza pe GEO tinta
   - Cauta per vertical: cine ruleaza ads platite targetand GEO?
   - Documenteaza: Advertiser | Vertical | Ad Type | Duration
2. **X Trending**:
   - Verifica trending topics pentru GEO / regiune (LATAM pentru Bolivia)
   - Identifica spike-uri de demand (evenimente sportive → betting, crize financiare → loans/crypto)
3. **Cautare affiliate chatter**:
   - Brave Search pe X: "affiliate Bolivia", "CPA LATAM", "SMS marketing LATAM"
   - Identifica alti afiliati care discuta ce functioneaza in regiune
4. **Brand presence check**:
   - Cauta brand names din ofertele candidate + GEO
   - Prezenta activa a brand-ului pe X = au buget marketing = vor plati afiliati

**Scor canal (1-5)**:
- 5 = Ads active + brand presence puternica + trending relevant
- 4 = Ads active SAU brand presence puternica
- 3 = Prezenta moderata, unele branduri active
- 1 = Zero activitate relevanta pe X pentru GEO
- 0 = Canal nescanat

**Output (artifact)**: `m137-x-intel-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: X Ads Transparency si trending verificate. Brand check facut pentru minim 3 verticale. FAIL daca doar trending verificat fara ads transparency.

---

### Pas 6b: Affiliate Link Detection — Ce produse au deja programe de afiliere?

**Tool**: Automat — analizeaza URL-urile gasite in Pasii 1-6
**Trigger**: Se ruleaza automat dupa completarea Pasilor 1-6. Analizeaza TOATE URL-urile colectate.

**Actiune**:
1. Colecteaza toate URL-urile din rezultatele Pasilor 1-6 (landing pages, ad URLs, redirect chains)
2. Scaneaza fiecare URL pentru indicatori de afiliere:

**Tracking Parameters (in URL query string)**:
- `?ref=`, `?aff=`, `?affiliate=`, `?partner=`
- `?click_id=`, `?clickid=`, `?sub_id=`, `?subid=`
- `?pid=`, `?offer_id=`, `?campaign_id=`
- `utm_source=affiliate`, `utm_medium=cpa`, `utm_source=partner`
- `?btag=`, `?stag=`, `?tracker=`, `?source=`

**Redirect Domains (indica retea de afiliere)**:
- `trk.` / `track.` / `go.` / `click.` / `rdr.` → generic tracking
- `3snet.com` → 3SNET
- `maxbounty.com` / `mb102.com` → MaxBounty
- `mobidea.com` / `mdealink.com` → Mobidea
- `clickdealer.com` / `clik.cc` → ClickDealer
- `performcb.com` / `cbtrk.com` → Perform[cb]
- `adcombo.com` / `adc.am` → AdCombo
- `dr.cash` / `drca.sh` → Dr.Cash
- `everad.com` / `everad.link` → Everad
- `leadbit.com` / `lbt.link` → Leadbit
- `alfaleads.net` / `al-link.net` → AlfaLeads
- `affise.com` → Affise platform (multiple networks)
- `hasoffers.com` / `go2cloud.org` → HasOffers/TUNE platform
- `everflow.io` → Everflow platform
- `impact.com` / `sjv.io` → Impact Radius
- `partnerize.com` → Partnerize
- `admitad.com` / `ad.admitad.com` → Admitad
- `cj.com` / `emjcd.com` → CJ Affiliate
- `shareasale.com` → ShareASale
- `awin.com` / `zenaps.com` → Awin

3. Per URL detectat cu indicatori de afiliere, documenteaza:

| Camp | Ce documentezi |
|---|---|
| Product/Brand | Numele produsului/brandului promovat |
| Original URL | URL-ul gasit in Pasii 1-6 |
| Affiliate Indicator | Ce parametru/domeniu a fost detectat |
| Detected Network | Reteaua de afiliere identificata (sau "UNKNOWN" daca doar parametri generici) |
| Already On Network? | DA (avem cont) / NU (de aplicat) / UNKNOWN |
| Vertical | Categoria produsului |

4. Clasificare:
   - **CONFIRMED AFFILIATE**: Retea identificata + produs clar → candidat direct pentru promovare
   - **PROBABLE AFFILIATE**: Parametri de tracking generici dar retea necunoscuta → investiga manual
   - **DIRECT ADVERTISER**: URL fara indicatori de afiliere → candidat pentru contact direct (vezi lista Top 10 fara afiliere)

**Scor canal (1-5)**:
- 5 = 10+ produse cu affiliate links detectate, 3+ retele identificate
- 4 = 5-9 produse, 2+ retele
- 3 = 3-4 produse cu indicatori
- 2 = 1-2 produse cu indicatori generici
- 1 = Zero affiliate links detectate
- 0 = Pas nescanat

**Output (artifact)**: `m137-affiliate-detection-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate URL-urile din Pasii 1-6 scanate. Minim 50 URL-uri analizate. FAIL daca sub 20 URL-uri scanate.

> **Nota importanta**: Pas 7 (Network Mining) devine acum un PAS DE CONFIRMARE, nu de descoperire. Rezultatele din Pas 6b se cross-referinteaza cu ofertele gasite pe dashboards la Pas 7.

---

### Pas 7: Affiliate Network Mining — Ce oferte exista pe retele?

**Tool**: Dashboards retele afiliate active + OfferVault.com + mcp__brave-search

**Retele ACTIVE (scanare obligatorie)**:

| Retea | Vertical | Dashboard |
|-------|----------|-----------|
| 3SNET (#30063) | Gambling | go.3snet.co |
| 1win Partners | Gambling | 1win-partners.com |
| Mostbet Partners | Gambling | mostbet-partners.com |
| ACE Partners | Gambling | ace.pa |
| CPAgetti | Gambling | cpagetti.com |
| Alpha-Affiliates | Gambling | alpha-affiliates.com |
| Rewards Affiliates | Gambling | rewardsaffiliates.com |
| Dr.Cash | Nutra/Health | dr.cash |
| AdCombo | Nutra/COD | adcombo.com |
| Everad | Nutra/Health | everad.com |
| Leadbit | Nutra/Health | leadbit.com |
| AlfaLeads | Nutra/Health | alfaleads.net |
| Traffic Light | Nutra/Health | cpa.tl |

**Retele PENDING (de inregistrat — task Leo/Kody)**:

| Retea | Vertical | URL Signup | Prioritate |
|-------|----------|------------|------------|
| MaxBounty | CPA general/finance | maxbounty.com/apply.cfm | HIGH — cel mai mare CPA network global |
| Mobidea | Mobile/carrier billing | mobidea.com/affiliate/ | MEDIUM — specialist mobile, bun pentru CPI |
| Perform[cb] | Enterprise/finance | performcb.com | MEDIUM — finance vertical strong |
| ClickDealer | Mobile/CPI | clickdealer.com/affiliates/ | HIGH — deja inregistrat dar fara confirmation email, re-apply |

> **Task pentru Leo/Kody**: Inregistreaza-te pe MaxBounty, Mobidea, Perform[cb]. Re-aplica pe ClickDealer. Deadline: +7 zile de la prima executie M-137. Procedura M-006 (CPA Network Application) contine procesul complet.

**Actiune**:
1. Logheaza-te in fiecare retea si filtreaza:
   - GEO = tinta (Bolivia, Romania, etc.)
   - Sorteaza dupa EPC (Earnings Per Click) descrescator — top EPC = convertoare dovedite
2. Per oferta gasita, documenteaza:

| Camp | Ce documentezi |
|---|---|
| Offer Name | Numele complet al ofertei |
| Network | Pe ce retea e disponibila |
| Vertical | Categoria (betting, trading, apps, health, etc.) |
| Payout Model | CPA / RevShare / CPI / CPL / COD |
| Payout Value | Suma bruta per conversie |
| EPC | Earnings Per Click (daca disponibil) |
| Conversion Event | Ce trebuie sa faca userul (install, register, deposit, lead, purchase) |
| GEO | Tarile acceptate |
| LP Available | Landing page dedicata disponibila? (DA/NU) |
| Special Conditions | Cap volum, min threshold, restrictii trafic |
| Status | Activa/De aplicat/De negociat |

3. Pentru verticale cu demand signals din Pasii 1-6 dar FARA oferta gasita pe retele:
   - Contacteaza AM la fiecare retea: "Aveti oferte pentru {vertical} in {GEO}?"
   - Cauta pe Brave: "{vertical} affiliate program {GEO}"
   - Verifica OfferVault.com: filter by GEO + vertical keyword
4. Cauta si pe retele in care NU ai cont inca — daca au oferte interesante, noteaza ca "DE APLICAT"

**Scor canal (1-5)**:
- 5 = 5+ oferte active cu EPC verificat, pe 2+ retele
- 4 = 3-4 oferte, pe 1-2 retele
- 3 = 1-2 oferte disponibile
- 2 = Oferte gasite dar neaprobate / de negociat
- 1 = Zero oferte gasite pe nicio retea
- 0 = Canal nescanat

**Output (artifact)**: `m137-network-supply-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Minim 3 retele scanate. Toate verticalele cu demand score >=3 (din Pasii 1-6) au fost cautate pe retele. FAIL daca verticale cu demand puternic nu au fost cautate pe retele.

---

### Pas 7b: Verificare Manuala Date Colectate (OPTIONAL)

**Tool**: Spreadsheet / browser manual
**Executor**: Leo sau Kody (NU persoana care a executat Pasii 1-7)
**Trigger**: Recomandat la prima executie M-137 pe un GEO nou. Optional la refresh trimestrial.

**Actiune**:
1. Verificatorul primeste output-urile din Pasii 1-7 (cele 7 artifacts)
2. Per canal, verifica MINIM 3 date aleatorii:
   - Pas 1 (Google Ads): deschide adstransparency.google.com, verifica 3 advertiseri raportati — exista? ads active?
   - Pas 2 (Meta Ads): deschide facebook.com/ads/library, verifica 3 advertiseri — nr. ads corect?
   - Pas 3 (Keywords): verifica 3 keywords in Google Keyword Planner — volumul raportat e corect?
   - Pas 4 (Trends): verifica 2 verticale in Google Trends — trend direction corect?
   - Pas 5 (Play Store): verifica top 3 apps raportate — exista? installs corecte?
   - Pas 6 (X): verifica 2 brand presence claims — exista pe X?
   - Pas 7 (Networks): verifica 2 oferte pe dashboard-ul retelei — payout corect? GEO corect?

3. Completeaza formularul de verificare:

```markdown
## Verification Report — M-137 Faza A | {GEO} | {YYYY-MM-DD}

**Verificator**: {Leo / Kody}
**Timp verificare**: {N} minute

| Canal | Date verificate | Corecte | Incorecte | Observatii |
|-------|----------------|---------|-----------|------------|
| Google Ads | 3 | {N} | {N} | {detalii} |
| Meta Ads | 3 | {N} | {N} | {detalii} |
| Keywords | 3 | {N} | {N} | {detalii} |
| Trends | 2 | {N} | {N} | {detalii} |
| Play Store | 3 | {N} | {N} | {detalii} |
| X | 2 | {N} | {N} | {detalii} |
| Networks | 2 | {N} | {N} | {detalii} |

**Accuracy Rate**: {total_corecte / total_verificate × 100}%
**Linkuri moarte gasite**: {lista URLs care nu mai functioneaza}
**Verdict**: PASS (>80% accuracy) / RECHECK (60-80%) / REDO (<60%)

**Feedback pentru imbunatatire procedura**:
- Ce a fost greu de verificat?
- Ce lipseste din date?
- Ce pas a produs cele mai slabe rezultate?
- Sugestii de imbunatatire (tool, metoda, template):
```

4. Daca accuracy rate <80%: re-executa canalele cu erori inainte de Faza B
5. Salveaza raportul: `m137-verification-faza-a-{GEO}-{YYYY-MM}.md`
6. Feedback-ul se adauga in master file (`verification_history[]`) pentru tracking improvement

**Output (artifact)**: `m137-verification-faza-a-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 18 date verificate (3+3+3+2+3+2+2). Accuracy rate documentat. FAIL daca verificatorul e aceeasi persoana care a colectat datele.

---

### Pas 7c: Product-Level Deep Dive (Propunere Kody) — OPTIONAL

**Tool**: Perplexity Pro + Google Keyword Planner + Meta Ad Library + SEMrush PPC Spy
**Trigger**: Dupa ce Pas 8 identifica top 3-5 verticale din demand matrix. Se ruleaza DOAR pe verticalele cu Composite Demand Score >= 3.0.

**Actiune**:
1. Pentru fiecare vertical selectat (max 7), gaseste:
   a. **Top 10 keywords** (in spaniola) sortate dupa CPC (cost-per-click) descrescator — keywords scumpe = produse care convertesc
   b. **Top 3 produse/branduri** care apar in Google Ads pentru aceste keywords
   c. **Top 3 produse/branduri** care apar in Facebook/Meta Ads
   d. **Top 3 produse/branduri** in Display Networks (daca date disponibile)

2. Pentru fiecare produs gasit, verifica:
   - Are program de afiliere? Pe ce retea? (3SNET, MaxBounty, Mobidea, etc.)
   - Daca NU are program de afiliere → adauga la lista "CONTACT DIRECT" (vezi Pas 7c output)

3. Verticale de scanat (default 7):
   - Health/Nutra (weight loss, supplements, beauty)
   - Apps/VPN (downloads, tools, security)
   - Trading/Forex (brokers, crypto, investment)
   - Gambling/Betting (sports, casino, slots)
   - Finance/Loans (credit, microfinance, fintech)
   - E-commerce (shopping, marketplace, deals)
   - Education (courses, language, certificates)

**Output (artifact)**: `m137-deep-dive-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 3 verticale analizate. Minim 5 produse specifice identificate cu brand name. FAIL daca output contine doar verticale generice fara produse concrete.

---

### FAZA B: CROSS-REFERENCE + SCORING

---

### Pas 8: Matrice Unificata Demand Signals

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
1. Construieste o matrice cu VERTICALELE ca randuri si CANALELE ca coloane
2. Completeaza cu scorurile (1-5) din fiecare canal (Pasii 1-7)
3. Calculeaza Composite Demand Score per vertical:

```
Composite Demand Score =
  (Google Ads Signal × 0.20) +
  (Meta Ads Signal × 0.15) +
  (Network EPC Signal × 0.20) +
  (Keyword Volume Signal × 0.15) +
  (Google Trends Signal × 0.10) +
  (Google Play Signal × 0.08) +
  (X Signal × 0.12)
= 1.00
```

4. Daca un canal nu a fost scanat (scor 0), redistribuie weightul proportional intre canalele scanate
5. Sorteaza descrescator dupa Composite Demand Score

**Template matrice**:

| Vertical | Google Ads (0.20) | Meta Ads (0.15) | Networks (0.20) | Keywords (0.15) | Trends (0.10) | Play Store (0.08) | X (0.12) | Composite Demand |
|---|---|---|---|---|---|---|---|---|
| trading_forex | 4 | 3 | 5 | 4 | 3 | 2 | 3 | 3.67 |
| gambling_betting | 3 | 4 | 5 | 3 | 4 | 1 | 3 | 3.43 |
| app_downloads | 2 | 2 | 3 | 5 | 4 | 5 | 2 | 3.06 |
| ... | ... | ... | ... | ... | ... | ... | ... | ... |

**Output (artifact)**: `m137-demand-matrix-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate verticalele cu date din minim 3 canale au scor calculat. FAIL daca vreun canal coloana e complet gol (minim 1 scor per canal per vertical).

---

### Pas 9: Match Demand ↔ Supply

**Tool**: Tabel comparativ

**Actiune**:
1. Pentru fiecare vertical cu Composite Demand Score >= 2.0:
   - Verifica daca exista oferte gasite la Pas 7
   - Documenteaza match-ul:

| Vertical | Demand Score | Offers Found | Best Offer | Network | Payout | Match Status |
|---|---|---|---|---|---|---|
| trading_forex | 3.66 | 2 | AvaTrade $250 CPA | MaxBounty | $250 | MATCHED |
| gambling_betting | 3.43 | 1 | MarathonBet RevShare | 3SNET | ~$30/FTD | MATCHED |
| app_downloads | 3.06 | 0 | — | — | — | NO OFFER — CONTACT AM |
| health_nutra | 2.50 | 0 | — | — | — | NO OFFER — SEARCH |

2. Clasificare match:
   - **MATCHED**: Demand + supply exista → candidat pentru ranking final
   - **NO OFFER — CONTACT AM**: Demand exista dar nicio oferta gasita → contacteaza AM-uri la retele
   - **NO OFFER — SEARCH**: Demand slab + nicio oferta → cauta pe OfferVault, Brave, direct advertiser programs
   - **SUPPLY ONLY**: Oferta exista pe retea dar demand nesemnificativ → risc, dar testeaza daca EC/1K e atractiv

**Output (artifact)**: `m137-demand-supply-match-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Fiecare vertical cu demand >=2.0 are match status documentat. Verticale MATCHED au offer details complete. FAIL daca verticale cu demand >=3.0 au match status gol.

---

### Pas 10: Risk Marking

**Tool**: Brave Search + cunostinte regulatory

> IMPORTANT: Riscurile se MARCHEAZA pe produse — nu se exclud niciodata. Decizia finala apartine echipei.

**Actiune**:
1. Per vertical/produs din Pas 9, evalueaza 4 categorii de risc:

**`[REG-RISK]` — Risc Regulatory**
- Intrebare: E legal sa promovezi acest produs prin SMS in GEO?
- Surse: regulatory body al GEO (ATT Bolivia, ASFI Bolivia), VOX compliance docs, Brave Search
- Aplica daca: reguli neclare, necesita licenta, disclaimeri obligatorii, zona gri
- NU aplica daca: complet interzis (adult, arme, tutun — aceste verticale nici nu ar trebui sa fie in lista)

**`[PAY-RISK]` — Risc Payment**
- Intrebare: Pot userii din GEO sa plateasca/converteasca pe acest produs?
- Surse: payment method landscape al GEO-ului
- Aplica daca: oferta necesita credit card iar CC penetration <10%, oferta necesita crypto wallet, subscription model in piata fara recurring billing
- Exemplu Bolivia: CC penetration ~5% → orice oferta bazata pe subscription primeste [PAY-RISK]

**`[SAT-RISK]` — Risc Saturare**
- Intrebare: Exista deja competitie in acest vertical pe SMS in GEO?
- Surse: STM Forum, AM contacts, SimilarWeb (Direct traffic >40% indicator)
- Aplica daca: 1+ competitori SMS identificati, piata mica cu multi jucatori pe alte canale
- Nota: In Bolivia, probabilitatea de SMS competition e foarte mica

**`[CAP-RISK]` — Risc Volume Cap**
- Intrebare: Oferta are limita de conversii pe luna?
- Surse: network dashboard, AM direct
- Aplica daca: cap explicit (ex: AvaTrade max 15 conversii/luna = max $1,875 net)
- Impact: limiteaza revenue ceiling chiar daca EC/1K e atractiv

2. Marcheaza fiecare produs cu tag-urile aplicabile. Un produs poate avea 0, 1 sau mai multe tag-uri.

**Output (artifact)**: `m137-risk-markers-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate produsele MATCHED din Pas 9 au fost evaluate pe cele 4 categorii. FAIL daca vreun produs MATCHED nu are evaluare risc.

---

### Pas 11: Calcul EC/1K SMS

**Tool**: Calculator / spreadsheet

**Actiune**:
1. Per produs MATCHED (din Pas 9), calculeaza EC/1K SMS:

```
EC/1K SMS = CTR% × CR% × Payout_net × 1000
Payout_net = Payout_brut × carrier_split
```

**Parametri configurabili**:
- `carrier_split`: default 0.50 (VOX 50/50 revshare). Ajusteaza per contract carrier.

**Benchmark-uri conservative (default — se inlocuiesc cu date reale dupa validation send)**:
- CTR default: 3.0% (floor conservator — datele reale SMS+ arata 7-9%)
- CR default: 1.0% (floor conservator)

> REGULA: Foloseste INTOTDEAUNA benchmark-urile conservative la prima evaluare. NU supraestima. Validation send-ul (Pas 14) va inlocui aceste estimari cu date reale.

2. Calcul exemplu — Trading/Forex Bolivia (AvaTrade $250 CPA):
   - Payout_net = $250 × 0.50 = $125
   - EC/1K SMS = 3.0% × 1.0% × $125 × 1000 = 0.03 × 0.01 × $125 × 1000 = **$37.50/1K SMS**
   - [CAP-RISK]: max 15 conversii/luna = ceiling $1,875 net/luna

3. Calcul exemplu — App Downloads Bolivia (CPI $1.50):
   - Payout_net = $1.50 × 0.50 = $0.75
   - EC/1K SMS = 3.0% × 1.0% × $0.75 × 1000 = **$0.225/1K SMS** (conservative)
   - Nota: cu CTR real 8% si CR 5%, EC/1K ar fi $3.00 — validation send va clarifica

4. Normalizare EC/1K Score (1-5):
   - EC/1K > $50 = 5
   - EC/1K $20-50 = 4
   - EC/1K $10-20 = 3
   - EC/1K $5-10 = 2
   - EC/1K < $5 = 1

**Output (artifact)**: `m137-ec1k-calculations-{GEO}-{YYYY-MM}.csv`
Coloane: Product | Network | Payout_brut | carrier_split | Payout_net | CTR% | CR% | EC/1K SMS | EC/1K Score | Risk Flags

**Checkpoint PASS**: EC/1K calculat pentru toate produsele MATCHED. Formula foloseste benchmark-uri conservative (3%/1%), nu optimiste. FAIL daca CTR >5% folosit fara date reale.

---

### FAZA C: RANKING FINAL + PLAN LANSARE

---

### Pas 12: Scor Final de Prioritizare

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
Calculeaza scorul final combinand 4 dimensiuni:

```
Final Score =
  (Composite Demand Score × 0.35) +
  (EC/1K SMS Score × 0.30) +
  (SMS Compatibility Score × 0.20) +
  (Offer Readiness Score × 0.15)
```

**Normalizari**:
- **Composite Demand Score**: din Pas 8 (deja 1-5)
- **EC/1K SMS Score**: din Pas 11 (deja 1-5)
- **SMS Compatibility Score**: din SMSADS-P1 Pas 2 formula (5 dimensiuni: Risc Regulatory, Path Conversie, Consent Carrier, Payout, Audience). Daca P1 nu a fost executat, estimeaza cu formula simplificata:
  - 5 = actiune simpla (click→install), fara restrictii, payout bun
  - 3 = path mediu (3 pasi), unele restrictii
  - 1 = path complex (5+ pasi), restrictii multiple
- **Offer Readiness Score**:
  - 5 = Oferta aprobata pe cont, tracking setat
  - 3 = Oferta disponibila pe retea, de aplicat
  - 1 = Oferta de cautat/negociat cu AM

**Clasificare actiune**:
- Score >= 4.0: **LAUNCH** — procedeaza la SMSADS-P2 scoring detaliat + Binom tracking setup
- Score 3.0-3.9: **PILOT** — test cu 5K-10K SMS, valideaza CR
- Score 2.0-2.9: **BACKLOG** — monitorizeaza 30 zile, re-score
- Score < 2.0: **PASS** — insuficient evidence

**Template ranking final**:

| Rank | Vertical | Product/Offer | Network | EC/1K Est. | Demand | SMS Compat | Offer Ready | Final Score | Action | Risk Flags |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | trading_forex | AvaTrade $250 | MaxBounty | $37.50 | 3.67 | 3.80 | 5 | 3.99 | LAUNCH | [CAP-RISK] |
| 2 | gambling_betting | MarathonBet | 3SNET | ~$9.00 | 3.43 | 4.20 | 5 | 3.72 | PILOT | [REG-RISK] |
| 3 | vpn_antivirus | NordVPN CPA | MaxBounty | $3.75 | 2.80 | 4.40 | 3 | 3.12 | PILOT | — |
| 4 | app_downloads | Android CPI | ClickDealer | $0.23 | 3.06 | 4.60 | 1 | 2.68 | BACKLOG | — |
| 5 | health_nutra | Dr.Cash COD | Dr.Cash | $1.50 | 2.50 | 3.50 | 3 | 2.63 | BACKLOG | [PAY-RISK] |

**Output (artifact)**: `m137-final-ranking-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Top 5+ produse rankate cu toate cele 4 dimensiuni completate. FAIL daca vreun produs din top 5 nu are scor complet.

---

### Pas 13: Product Cards Top 5

**Tool**: Markdown

**Actiune**:
Per fiecare produs din top 5, creeaza un **Product Card** cu:

```markdown
## Product Card #{rank}: {Product Name}

### Identitate
- **Vertical**: {vertical}
- **Offer**: {offer_name} | **Network**: {network}
- **Payout**: {payout_brut} {model} | **Net (post-split)**: {payout_net}
- **Conversion Event**: {ce trebuie sa faca userul}

### Demand Evidence
- Google Ads: {scor} — {detalii: cati advertiseri, ce keywords}
- Meta Ads: {scor} — {detalii: cate ads, ce creative themes}
- Keywords: {scor} — {detalii: volum total C+T, top 3 keywords}
- Trends: {scor} — {detalii: directie, peak months}
- Play Store: {scor} — {detalii: apps relevante, installs}
- X: {scor} — {detalii: brand presence, ads}
- Network EPC: {scor} — {detalii: EPC raportat, nr oferte}

### Economics
- **EC/1K SMS (conservative)**: ${ec1k_conservative} (CTR 3%, CR 1%)
- **EC/1K SMS (optimistic)**: ${ec1k_optimistic} (CTR {benchmark}, CR {benchmark})
- **Monthly Revenue Ceiling**: ${ceiling} (based on {limitation})
- **Break-even**: {N} SMS trimise per conversie

### Risk Flags
{lista risk flags cu explicatie per flag}

### SMS Strategy
- **Copy Angle Recomandat**: {bazat pe ad copy gasit la Pas 1-2}
- **IAB Segment Tinta**: {din VOX taxonomy}
- **Launch Timing**: {din Google Trends sezonalitate}
- **Validation Send**: {N} SMS → measure CTR/CR → decision in {N} zile

### Status
- [ ] Validation send planificat
- [ ] Tracking setup (Binom)
- [ ] SMS creative draftat
- [ ] IAB segment selectat in VOX
```

**Output (artifact)**: inclus in `m137-final-ranking-{GEO}-{YYYY-MM}.md` (sectiune Product Cards)
**Checkpoint PASS**: Top 5 produse au Product Card complet. Fiecare card are demand evidence, economics, risk flags, si SMS strategy. FAIL daca vreun card are sectiune goala.

---

### Pas 14: Plan Validation Send

**Tool**: Planificare campanie

**Actiune**:
Per top 3 produse (din Pas 12), defineste planul de validare:

```markdown
## Validation Send Plan — {Product Name}

- **Oferta**: {offer_name} pe {network}
- **SMS de trimis**: 5,000-10,000 (minim 5K pentru semnificanta statistica)
- **Cost estimat**: ${cost} (la rata VOX per SMS)
- **IAB Segment**: {segmentul tinta din VOX taxonomy}
- **SMS Copy**: {draft bazat pe ad copy din Pas 1-2}
- **Tracking**: Binom click ID → network postback S2S
- **Durata test**: 3-5 zile (distribuie SMS pe mai multe zile)
**Break-even formula**:
break_even = Cost_per_SMS × 1000
Exemplu: daca VOX costa $0.005/SMS → break_even = $5.00/1K SMS

- **Success Criteria**:
  - CTR > {threshold}% → vertical viabil pentru SMS
  - CR > {threshold}% → oferta converteste
  - EC/1K > ${break_even} → profitabil dupa VOX split
- **Decision**:
  - PASS all criteria → SCALE (treci la full campanie, 50K+ SMS)
  - PASS CTR, FAIL CR → test alt landing page sau alta oferta in acelasi vertical
  - FAIL CTR → vertical neviabil pentru SMS in acest GEO, marcheaza KILL
```

**Output (artifact)**: `m137-validation-plan-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Top 3 produse au validation plan cu success criteria. FAIL daca validation send <5K SMS (insuficient pentru semnificanta).

---

### Pas 14b: Review Manual Rezultate Finale (OPTIONAL)

**Tool**: Ranking final (Pas 12) + Product Cards (Pas 13) + Validation Plan (Pas 14)
**Executor**: Leo sau Kody (NU persoana care a executat Faza B-C)
**Trigger**: Recomandat la prima executie M-137 pe un GEO nou. Optional la refresh trimestrial.

**Actiune**:
1. Verificatorul primeste: ranking final, product cards top 5, validation plan
2. Evalueaza pe 5 dimensiuni (1-5 fiecare):

```markdown
## Review Final — M-137 Faza C | {GEO} | {YYYY-MM-DD}

**Reviewer**: {Leo / Kody}

### Rating (1-5 per dimensiune)

| Dimensiune | Scor | Comentariu |
|-----------|------|------------|
| **Relevanta produse** — Top 5 au sens pentru GEO si SMS? | {1-5} | {de ce} |
| **Acuratete scoruri** — Demand scores reflecta realitatea pietei? | {1-5} | {de ce} |
| **Fezabilitate oferte** — Ofertele MATCHED sunt realiste si accesibile? | {1-5} | {de ce} |
| **Calitate product cards** — Ai suficienta informatie sa lansezi? | {1-5} | {de ce} |
| **Validation plan** — Planul de test e realist si executabil? | {1-5} | {de ce} |

**Scor Review Total**: {suma / 25 × 100}%

### Produse lipsa
- Ce produse/verticale crezi ca lipsesc din ranking? De ce?

### Produse contestate
- Ce produse din top 5 NU ar trebui sa fie acolo? De ce?

### Sugestii de imbunatatire
- Ce ar face procedura mai buna pentru urmatoarea executie?
- Ce canal de date a produs cele mai utile insights?
- Ce canal a produs cele mai slabe/inutile insights?

### Verdict
- [ ] APPROVE — ranking e bun, treci la validation send
- [ ] ADJUST — modifica {ce anume} inainte de lansare
- [ ] REDO — probleme majore, re-executa {care faza}
```

3. Daca scor review <60%: discuta cu executorul, ajusteaza ranking-ul inainte de validation send
4. Salveaza review-ul: `m137-review-final-{GEO}-{YYYY-MM}.md`
5. Adauga review score + feedback in master file (`review_history[]`) pentru tracking improvement

**Cum alimenteaza self-improvement**:
- Dupa 3+ executii M-137, compara review scores per canal → canalele cu cel mai slab scor de relevanta pot fi demotate sau inlocuite
- Compara accuracy rate (Pas 7b) cu review score (Pas 14b) → daca datele sunt corecte dar ranking-ul e slab, problema e in formulele de scoring (Faza B)
- Produsele lipsa raportate de reviewer → adauga ca seed keywords / verticale noi la urmatoarea executie
- Canalele cele mai utile (din feedback) → promoveaza la Tier 1 / creste weight-ul in formula

**Output (artifact)**: `m137-review-final-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Toate 5 dimensiunile scoruite. Verdict emis. FAIL daca reviewer-ul e aceeasi persoana care a executat procedura.

---

### FAZA D: FEEDBACK LOOP

---

### Pas 15: Inregistrare Rezultate + Actualizare Master File

**Tool**: Master file JSON + mcp__cortex__cortex_store
**Trigger**: Dupa FIECARE campanie (validation send sau campanie completa)

**Actiune**:
1. Completeaza template-ul de rezultate:

```
Campaign: {offer_name} | GEO: {geo} | Date: {YYYY-MM-DD}
SMS Sent: {N}
Clicks: {N}
Conversions: {N}
Revenue: ${N}
---
CTR: (Clicks / SMS Sent × 100) = {auto}%
CR: (Conversions / Clicks × 100) = {auto}%
EC/1K: (Revenue / SMS Sent × 1000) = ${auto}
Status: VALIDATED / KILL / RETEST
```

2. Actualizare master file (`product-intelligence-{GEO}.json`):
   - Inlocuieste `ec1k_estimated` cu `ec1k_actual`
   - Adauga entry in `campaign_results[]`
   - Updateaza `status`: CANDIDATE → VALIDATED / KILL
   - Recalculeaza CTR/CR medii daca exista multiple campanii

3. Push la Cortex (collection: `smsads-product-intel`)

4. Updateaza `procedure-health.json` (la prima executie):
   - Path: `~/.openclaw/procedure-health.json`
   - Adauga entry: `"M-137-SMS-PRODUCT-DISCOVERY": {"status": "ACTIVE", "last_run": "{YYYY-MM-DD}", "version": "1.0"}`

5. **Re-ranking**: Dupa fiecare actualizare, recalculeaza Final Score cu date reale:
   - EC/1K Score se recalculeaza cu CTR/CR actuali, nu defaults
   - Produse KILL se muta la sfarsitul rankingului
   - Produse VALIDATED cu EC/1K peste asteptari se promoveaza

**Status transitions**:
- CANDIDATE → VALIDATED (EC/1K real > break-even dupa validation send)
- CANDIDATE → KILL (CTR < 1% SAU EC/1K < break-even dupa validation send)
- CANDIDATE → RETEST (rezultate inconcluzive, <5K SMS trimise, probleme tehnice)
- VALIDATED → SCALE (3+ campanii consecutive profitabile)
- VALIDATED → KILL (performance degradeaza sub break-even pe 2+ campanii)

**Output (artifact)**: Master file actualizat + Cortex entry
**Checkpoint PASS**: Cele 4 numere introduse (sent, clicks, conversions, revenue), CTR/CR/EC1K calculat. Master file actualizat. FAIL daca campania a fost executata dar rezultatele nu au fost inregistrate in max 48h.

---

### Pas 15b: KSL Self-Improvement Loop (Karpathy Skill Loop)

**Trigger**: Dupa fiecare executie COMPLETA a M-137 pe un GEO (Pas 1-15)
**Framework**: KSL — audit → fix → re-audit → keep/discard
**Profile**: EPR (experiment mode, max 20 iterations, cost cap $3/run)

**Actiune**:

1. **MEASURE** — Scor executie pe 5 dimensiuni (1-5):

| Dimensiune | Ce masori |
|-----------|----------|
| Data Completeness | Cate verticale au date din 3+ canale? (target: 6/8) |
| Data Accuracy | Accuracy rate din Pas 7b (target: >80%) |
| Tool Effectiveness | Cate tool-uri au returnat date utile vs failed? (target: >70%) |
| Time Efficiency | Timp total vs target 4-6 ore (target: <6h) |
| Score Delta | Scorul final s-a imbunatatit fata de run-ul anterior? |

2. **AUDIT** — Identifica ce a mers prost:
   - Care canale au esuat (rate limits, zero date)?
   - Care verticale au fost sub-cercetate?
   - Care tool-uri au fost lente sau inutile?
   - Care prompturi au returnat rezultate slabe?

3. **FIX** — Aplica mutatii:
   - Swap tool-uri (ex: Brave → Exa daca rate limited)
   - Ajusteaza prompturi (adauga/elimina keywords)
   - Reordoneaza pasii (ruleaza tool-urile cu cel mai mare value primele)
   - Adauga/elimina canale din Tier 1/2/3
   - Ajusteaza weights in formula Composite Demand Score

4. **RE-RUN** — Executa din nou cu fix-urile aplicate (optional: doar pasii afectati)

5. **COMPARE** — Scorul e mai mare? Da → keep changes. Nu → rollback.

6. **REPEAT** — Pana la convergenta (scor platou 2 iteratii consecutive) sau max 5 iteratii per GEO.

**Learned Patterns (se actualizeaza dupa fiecare run)**:

Exemplu din Bolivia Run 1 (2026-03-24):
- P1: Google Ads Transparency = inutila pt piete mici (<20M pop). Downgrade la Tier 2.
- P2: Exa Advanced Search (neural) = cel mai bun tool. Promote la Tier 1.
- P3: SEMrush PPC Spy = zero date pt Bolivia. Skip, foloseste doar Keyword Magic Tool.
- P4: Meta Ad Library URL params functioneaza perfect. Standardizeaza template.
- P5: Health/Nutra era subevaluata in Phase 1. Ruleaza SEMrush Keyword Magic PRIMELE.
- P6: Brave Search free tier (2000 queries/luna) = insuficient. Budget pt paid sau Exa primary.
- P7: Parallel API requests trigger rate limits. Stagger cu 2s delay intre queries.
- P8: Ordinea optima: SEMrush KW → Exa → Meta Ad Library → Affiliate Dashboards

**Output**: `m137-ksl-loop-{GEO}-{run_number}.md` cu scoruri, fix-uri aplicate, si delta vs run anterior.
**Checkpoint PASS**: Minim 3 din 5 dimensiuni au scor >=3. FAIL daca scorul total a scazut fata de run-ul anterior fara rollback.

---

## 3.5 KSL — Karpathy Skill Loop (Optimizare Continua)

### Principiu
Dupa fiecare executie M-137, ruleaza un loop de optimizare pe 3 dimensiuni:
1. **Keyword Optimization**: Ce keywords au produs cele mai bune rezultate? Adauga la seed list. Ce keywords au returnat zero? Elimina.
2. **Tool Optimization**: Ce engine de cautare a dat cele mai relevante rezultate per pas? Ajusteaza prioritatea.
3. **Scoring Optimization**: Scorurile estimate (CTR 3%, CR 1%) se inlocuiesc cu date reale. Formula se recalibreaza.

### KSL Log Format
Dupa fiecare executie, adauga in master file:
```json
"ksl_log": [
  {
    "run_date": "2026-03-24",
    "geo": "bolivia",
    "patterns_found": [
      "Health/Nutra surpassed Gambling as #1 (31K vs 8K searches)",
      "Perplexity Pro returned 3x more specific product names than Brave",
      "CPC-sorted keywords revealed weight loss pills = highest commercial intent"
    ],
    "adjustments_made": [
      "Added Perplexity Pro as primary search engine",
      "Added Pas 7c (Kody's Product Deep Dive) as sub-procedure",
      "Updated Health/Nutra keyword seeds with 14 weight loss terms from Kody"
    ]
  }
]
```

### Frequenta
- **Luna 1**: KSL dupa fiecare executie (invatare rapida)
- **Luna 2+**: KSL trimestrial (procedura stabilizata)

### Ideas Processing (Pas 15c)

**Trigger**: La inceputul fiecarei sesiuni de lucru.
**Sursa**: Dashboard tab "Ideas" — mesaje de la Pafi, Leo, Kody.

**Actiune**:
1. Citeste toate ideile noi din dashboard
2. Clasifica fiecare idee:
   - **Vertical/produs nou** → adauga la seed keywords pentru urmatorul M-137 run
   - **Tool suggestion** → evalueaza si adauga la tool stack daca relevant
   - **Imbunatatire procedura** → actualizeaza M-137 direct, log in KSL
   - **Bug/eroare date** → corecteaza in master file + Cortex
   - **GEO nou** → planifica executie M-137 pentru noul GEO
   - **Intel general** → adauga in Braindump ca FINDING
3. Marcheaza ideile procesate (NU le sterge — pastrate pentru audit trail)

---

## 4. State Management

### Master File Schema

**Path**: `~/.nexus/projects/smsads/product-intelligence-{GEO}.json`

```json
{
  "geo": "bolivia",
  "last_discovery_run": "2026-03-23",
  "last_updated": "2026-03-23",
  "carrier_split": 0.50,
  "procedure_version": "1.0",
  "channels_mined": ["google_ads", "meta_ads", "keyword_planner", "google_trends", "play_store", "x", "networks"],
  "products": [
    {
      "id": "trading_forex_avatrade_maxbounty",
      "vertical": "trading_forex",
      "offer_name": "AvaTrade",
      "network": "maxbounty",
      "payout_model": "CPA",
      "payout_brut": 250.00,
      "payout_net": 125.00,
      "demand_scores": {
        "google_ads": 4,
        "meta_ads": 3,
        "networks": 5,
        "keywords": 4,
        "trends": 3,
        "play_store": 2,
        "x": 3
      },
      "composite_demand": 3.66,
      "sms_compatibility": 3.80,
      "offer_readiness": 5,
      "ec1k_estimated": 37.50,
      "ec1k_actual": null,
      "ctr_default": 3.0,
      "cr_default": 1.0,
      "ctr_actual": null,
      "cr_actual": null,
      "final_score": 3.98,
      "action": "LAUNCH",
      "risk_flags": ["CAP-RISK"],
      "status": "CANDIDATE",
      "campaign_results": [],
      "notes": ""
    }
  ],
  "verification_history": [
    {
      "date": "2026-03-23",
      "type": "faza_a",
      "reviewer": "Leo",
      "accuracy_rate": 89,
      "dead_links": 0,
      "verdict": "PASS",
      "feedback": "Keywords volume slightly off for VPN vertical"
    }
  ],
  "review_history": [
    {
      "date": "2026-03-23",
      "type": "faza_c",
      "reviewer": "Kody",
      "scores": { "relevanta": 4, "acuratete": 4, "fezabilitate": 3, "calitate_cards": 4, "validation_plan": 5 },
      "total_pct": 80,
      "missing_products": ["crypto_exchange"],
      "contested_products": [],
      "best_channel": "google_ads",
      "weakest_channel": "x",
      "verdict": "APPROVE"
    }
  ]
}
```

### Cortex Mirror

**Collection**: `smsads-product-intel`
**Push pattern**: Refoloseste pattern-ul existent din `cortex-smsads-push.py` + LaunchAgent `com.nexus.cortex-smsads-push`
**Frecventa**: Dupa fiecare actualizare master file (manual trigger sau LaunchAgent la 5 min)

---

## 5. Cortex Logging

```json
{
  "text": "M-137 executat: multi-channel product discovery {GEO} {YYYY-MM}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-137-SMS-PRODUCT-DISCOVERY",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "sms-product-discovery",
    "version": "1.0",
    "tags": ["sms", "product-discovery", "demand-mining", "multi-channel", "VOX"],
    "geo_analyzed": "",
    "channels_mined": 7,
    "verticals_discovered": 0,
    "products_matched": 0,
    "top_product": "",
    "top_ec1k": "",
    "execution_mode": "FULL",
    "execution_date": "",
    "executed_by": ""
  }
}
```

---

## 6. Enforcement Loop (META-H-002)

### WHERE
- La activarea oricarui GEO VOX nou (obligatoriu inainte de prima campanie pe vertical non-gambling)
- La review trimestrial al oportunitatilor de produse per GEO
- La propunerea unui vertical/produs nou de catre echipa sau AM
- Cand SMSADS-P0 ar fi fost executat anterior (M-137 il inlocuieste)

### WHEN
- Trigger GEO nou: executie in max 7 zile de la confirmarea accesului VOX
- Trigger trimestrial: prima saptamana Q1, Q2, Q3, Q4
- Trigger propunere vertical: executie in max 72h de la propunere

### HOW (violation detection)
- VIOLATION: Vertical non-gambling lansat fara M-137 completat = BLOCARE
- WARNING: M-137 neactualizat >90 zile pentru GEO activ = WARNING automat
- WARNING: Validation send executat dar rezultate neinregistrate in 48h = WARNING

### CONNECT
- **SMSADS-P1** (Vertical Discovery): primeste top verticale din M-137 pentru scoring SMS compatibility detaliat
- **SMSADS-P2** (Product Finder): primeste produsele LAUNCH/PILOT din M-137 pentru scoring oferte detaliat
- **SMSADS-P3** (Competitor Intel): primeste verticalele selectate pentru analiza competitiva
- **SMSADS-P4** (Network Discovery): primeste GEO-ul analizat pentru discovery retele
- **M-125** (VOX Taxonomy Segmentation): primeste verticalele aprobate si le segmenteaza IAB
- **M-117** (Competitive Intelligence Automation): alimenteaza Pas 2 si 6 cu date monitorizare
- `procedure-health.json` → adauga entry: `M-137-SMS-PRODUCT-DISCOVERY`

### VERIFY (checkpoint procedural)
La finalul executiei procedurii, verifica:
- [ ] Faza A completa? (minim Tier 1 obligatoriu: Pasii 1, 2, 7)
- [ ] Faza B completa? (Pasii 8, 9, 10, 11)
- [ ] Faza C completa? (Pasii 12, 13, 14)
- [ ] Master file creat/actualizat?
- [ ] Cortex save confirmat?
- [ ] Daca oricare = NU → procedura NU e completa, nu marca DONE

**Doua VK-uri obligatorii (VK-H-001)**:
1. `[PROC] M-137 | §1-§6 | {GEO} | {nr_produse} produse rankate | {nr_matched} matched | complete`
2. `[CORTEX] "M-137: product discovery {GEO} {YYYY-MM}" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Pasii 1-7: Mining canale | Sonnet | Task-uri de colectare date, repetitive, structurate |
| Pas 8: Matrice demand | Sonnet | Calcul tabelar deterministic |
| Pas 9-10: Match + Risk | Sonnet | Comparatie si clasificare structurata |
| Pas 11: EC/1K calcul | Opus | Modelare economica + interpretare benchmarks |
| Pas 12: Ranking final | Opus | Sinteza multi-dimensionala + rationament prioritizare strategic |
| Pas 13: Product Cards | Opus | Sinteza demand evidence + strategy recommendations |
| Pas 14: Validation plan | Sonnet | Planificare structurata cu template |
| Pas 15: Feedback loop | Sonnet | Inregistrare date + update mecanic |

---

## 7. Dependente

| Componenta | Rol | Path/Endpoint |
|---|---|---|
| Google Ads Transparency Center | Sursa date Pas 1 | adstransparency.google.com (gratuit) |
| Meta Ad Library | Sursa date Pas 2 | facebook.com/ads/library (cont FB) |
| Google Keyword Planner | Sursa date Pas 3 | ads.google.com (cont Google Ads) |
| Google Trends | Sursa date Pas 4 | trends.google.com (gratuit) |
| Google Play Store | Sursa date Pas 5 | play.google.com (gratuit) |
| X Ads + Trending | Sursa date Pas 6 | ads.x.com + x.com (gratuit) |
| 3SNET Dashboard | Sursa oferte Pas 7 | go.3snet.co / cont #30063 |
| MaxBounty Dashboard | Sursa oferte Pas 7 | maxbounty.com |
| ClickDealer Dashboard | Sursa oferte Pas 7 | clickdealer.com |
| Mobidea Dashboard | Sursa oferte Pas 7 | mobidea.com |
| Perform[cb] Dashboard | Sursa oferte Pas 7 | performcb.com |
| Brave Search MCP | Cautari suplimentare | mcp__brave-search |
| Cortex | Persistenta date | mcp__cortex__cortex_store |
| SMSADS-P1 | Downstream: SMS compatibility scoring | `SMSADS-P1-vertical-discovery.md` |
| SMSADS-P2 | Downstream: detailed offer scoring | `SMSADS-P2-product-finder.md` |

---

## 8. Metrics

| Metrica | Ce masoara | Target |
|---|---|---|
| Produse noi descoperite per GEO per trimestru | Diversificare pipeline | Minim 5 cu demand score >2.5 |
| Produse MATCHED (demand + supply) | Actionability rate | >50% din produse descoperite |
| EC/1K estimat vs EC/1K real | Acuratete estimare | <40% deviatie dupa validation send |
| Timp executie completa M-137 | Eficienta procedurii | <6 ore (full) / <3 ore (Tier 1 only) |
| Produse LAUNCH/PILOT lansate in 30 zile | Viteza go-to-market | 100% din top 3 |
| Revenue din verticale descoperite prin M-137 | Impact procedura | >$500/luna in 90 zile |

---

## 9. Versioning Plan

| Versiune | Continut | Timeline |
|---|---|---|
| **v1.0** (current) | 7 canale gratuite (Tier 1+2), manual execution, team-readable | Now |
| **v1.1** | + SEMrush/SimilarWeb/Ahrefs enhancement layers | Cand revenue SMS >$500/mo |
| **v2.0** | + TikTok Creative Center, MercadoLibre (Tier 3) | Dupa validarea v1 pe 2+ GEO-uri |
| **v3.0** | AI-agent executable (Genie/subagent autonom) | Dupa v2 stabilizat + team tested |
| **v4.0** | Channel-agnostic (nu doar SMS — Google Ads, native, push, email) | Dupa deprecarea completa SMSads |

---

## Checklist Pre-Publicare

- [x] Regula asociata: TRAIN-S-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu checks obligatorii
- [x] Doi VK-uri obligatorii per VK-H-001 specificati
- [x] MODEL ROUTING prezent cu justificare per pas
- [x] Artifacts cu naming convention per fiecare pas
- [x] Formule de scoring concrete (demand composite + EC/1K + final score)
- [x] Checkpoints PASS/FAIL per pas
- [x] Conexiuni la SMSADS-P1, P2, P3, P4, M-117, M-125 documentate
- [x] Risk markers definite (REG/PAY/SAT/CAP) cu regula "never exclude"
- [x] State management (master file JSON + Cortex mirror) specificat
- [x] Feedback loop (Pas 15) cu template rezultate si status transitions
- [x] Versioning plan (v1.0 → v4.0) documentat
- [x] Limbaj romana cu termeni tehnici in engleza
