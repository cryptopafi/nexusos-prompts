---
type: procedure
created: 2026-03-20
status: active
slug: tiered-link-architecture
tags: [procedure, nexus]
---

# TRAIN-SEO-008: TIERED-LINK-ARCHITECTURE — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate Tiered Link Theory + GSA SER + SAPE Integration -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / Architecture / T0-T3 Framework -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-008
**Scope**: Construirea și gestionarea unei arhitecturi de link building pe 4 niveluri (T0–T3) pentru domenii affiliate casino/gambling, cu filtru de protecție între spam și money site.

---

## 1. Problema

Google scrutinizează nișa gambling mai agresiv decât orice altă nișă (pharma, crypto, payday loans). Un singur link spam sau o singură footprint greșită direct pe money site poate declanșa o penalizare manuală sau algoritmică care distruge luni de muncă. Totodată, construirea manuală de linkuri high-quality pentru tot profilul este imposibil de scalat financiar — un guest post decent costă $150–500, un PBN T1 serios costă $500–2.000 în setup.

Soluția structural corectă este arhitectura în straturi (tiered link building): fiecare nivel filtrează "link juice" din nivelul inferior, amplificând equity-ul și eliminând riscul înainte ca acesta să atingă money site-ul. T3 spam → T2 web 2.0 → T1 authority → T0 money site.

**Problema fără procedură**:
- T3 spam trimis direct la T0 → penalizare imediată
- T1 construite și activate simultan cu T0 → velocity spike nenatural
- Anchor text over-optimizat pe T0 → penalizare exact match
- PBN T1 fără warmup T2/T3 → footprint clar pentru Google
- Parasite pages fără suficiente RDs → nu rankează, bani pierduți

**Business Application**: Gambling SEO Project — Peru / Kenya / Chile affiliate sites. Arhitectura se aplică atât pentru money sites noi, cât și pentru accelerarea parasite pages existente.

---

## 2. Procedura

### Vizualizare Arhitectură — Diagrama ASCII

```
┌─────────────────────────────────────────────────────────────────┐
│                    TIERED LINK ARCHITECTURE                     │
│                    Casino / Gambling SEO                        │
└─────────────────────────────────────────────────────────────────┘

                    ┌───────────────────┐
                    │   T0 — MONEY SITE │  ← Obiectivul final
                    │  Casino Affiliate │
                    │  Domain Principal │
                    └─────────┬─────────┘
                              │
                ┌─────────────▼──────────────┐
                │    ACCEPTĂ NUMAI T1 LINKS  │
                │  Anchor: 60% brand /       │
                │  20% partial / 10% exact   │
                │  Velocity: 3–5 links/lună  │
                └─────────────┬──────────────┘
                              │ HIGH QUALITY ONLY
                              │
         ┌────────────────────▼────────────────────┐
         │         T1 — AUTHORITY LAYER             │
         │                                          │
         │  [A] Parasite Pages                      │
         │      Medium, HuffPost, news outlets      │
         │      DR60+ host | TF — N/A               │
         │      Target: 10–20 RDs per parasite      │
         │                                          │
         │  [B] PBN Sites (Private Blog Network)    │
         │      DR20+ min | TF15+ min               │
         │      Expired domains cu history real     │
         │      Content: 800–1200 cuvinte/articol   │
         └───────────────┬─────────────────────────┘
                         │
             ┌───────────▼────────────┐
             │  ACCEPTĂ T2 LINKS +   │
             │  Niche Edits moderate  │
             │  Quality              │
             └───────────┬────────────┘
                         │ MEDIUM QUALITY
                         │
      ┌──────────────────▼───────────────────────┐
      │            T2 — POWER LAYER              │
      │                                          │
      │  Web 2.0: Tumblr, Blogspot,              │
      │           WordPress.com, Google Sites    │
      │  Social Bookmarks: Reddit, Pinterest,    │
      │                    Flipboard             │
      │  Blog Comments (low value, diversificare)│
      │  Volume: 50–200 per fiecare site T1      │
      │  Cost: ~$0 (timp + tool-uri)             │
      └───────────────┬──────────────────────────┘
                      │
          ┌───────────▼────────────┐
          │  ACCEPTĂ T3 LINKS     │
          │  Spam OK la T2        │
          └───────────┬────────────┘
                      │ LOW QUALITY / SPAM OK
                      │
   ┌──────────────────▼────────────────────────┐
   │             T3 — VOLUME LAYER             │
   │                                           │
   │  GSA SER automated submissions            │
   │  Indexing services (Speed-Links.net etc.) │
   │  Video sitemaps                           │
   │  Forum profiles (automated)               │
   │  Blog comments (automated)                │
   │  Volume: 100–1000+/zi la T2               │
   │  NICIODATĂ direct la T0 sau T1            │
   └───────────────────────────────────────────┘

    ═══════════════════════════════════════════════
    SAFETY FILTER PRINCIPLE:
    T3 spam → filtrează prin T2 → filtrează prin T1 → T0 primește CLEAN
    Dacă T1 e penalizat: șterge linkul T1→T0, înlocuiește T1, T0 rămâne intact
    ═══════════════════════════════════════════════
```

---

### Pas 1: CLASIFICARE ȚINTĂ — Ce construim?

Înainte de orice acțiune, stabilește tipul de campanie:

**Decizie Framework: Money Site vs. Parasite First**

| Condiție | Recomandare |
|---|---|
| Site nou, domeniu 0–3 luni | Parasite first — construiește T1 parasit înainte de a construi T0 backlinks |
| Site existent cu DR10+ | Activare directă T1→T0 (cu precauție velocity) |
| Piață low-competition (Peru/Kenya rural) | Parasite page poate ranka solo fără T0 |
| Piață high-competition (Chile urban, RO) | T0 obligatoriu, parasite ca suport |
| Budget sub $500 | Web 2.0 T2 + GSA T3 pentru T1 parasit mai întâi |
| Budget $1.500+ | PBN T1 + parasite simultane |

**Output Pas 1**: Document decizie — tip campanie, target URL T0, lista URL-uri T1 (parasite + PBN), timeline.

---

### Pas 2: BUILD T1 — Authority Layer (Luna 1)

**T1A — Parasite Pages**

Parasite page = conținut publicat pe un domeniu cu autoritate mare (Medium, HuffPost, site de știri cu DR60+). Rankează pe autoritatea domeniului gazdă, nu pe a ta.

Acțiuni:
1. Identifică platforme potrivite pentru piața țintă:
   - Peru/Chile (Spanish): Medium.com, LinkedIn Articles, site-uri știri hispanice cu guest post acceptat
   - Kenya (English): Medium.com, Pulse.com.gh variante locale, LinkedIn
   - RO: Ziare.com advertorial, platforme guest post românești
2. Scrie articol 1.000–2.000 cuvinte optimizat pentru keyword T1 (nu același cu T0 exact match)
3. Include 1–2 linkuri spre T0 cu anchor text conform regulii: branded sau partial match
4. Publică și notează URL-ul T1A

**Buget parasite**: 10 PBN links ($1.000) + 10 Niche Edits ($500) = **$1.500/pagină parasite**
**Target referring domains per parasite**: 10–20 RDs înainte de activare spre T0

**Velocity pentru parasite**:
- Subdomain parasite (medium.com/@user): 1–2 linkuri/săptămână (lent)
- Root domain parasite (site cu guest post): 2+ linkuri/zi (rapid, autoritate mai mare)

**T1B — PBN Sites (Private Blog Network)**

PBN = domenii expirate cu profil de linkuri real, reactivate sub controlul tău.

Criterii selecție domeniu expirat:
- DR minim: 20 (pentru piețe moderate), 30+ (pentru Chile/RO)
- TF (Trust Flow) minim: 15
- Nicio penalizare istorică (verifică în Ahrefs → History)
- Nicio footprint clară (nu fost part dintr-un alt PBN)
- Niche relevantă sau cel puțin neutră (nu gaming → gambling direct, footprint)

Setup PBN site:
1. Cumpără domeniu expirat (GoDaddy Auctions, Expireddomains.net)
2. Hosting: IP unic, provider diferit per site (nu aceeași subrețea — footprint!)
3. Instalează WordPress, temă diferită per site
4. Publică 3–5 articole de calitate (800–1.200 cuvinte) — conținut relevant la nișa domeniului
5. NU construi links din PBN spre T0 imediat — warmup 30 de zile

**Regulă critică PBN**: Construiește și validează T2 → T3 spre PBN ÎNAINTE de a activa linkul PBN → T0.

---

### Pas 3: BUILD T2 — Power Layer (Luna 2)

T2 = infrastructura care "alimentează" T1 cu link juice și îl face să pară natural în ochii Google.

**Web 2.0 Properties** — creează profiluri/bloguri pe:
- Tumblr.com
- Blogspot.com (Google-owned — signal pozitiv)
- WordPress.com (nu .org)
- Google Sites (sites.google.com — trust maxim)
- Weebly.com

Fiecare web 2.0:
- 1–2 articole de conținut relevant (poate fi spun/low quality — nu contează pentru T2)
- 2–5 linkuri spre URL-uri T1 (parasite pages sau PBN sites)
- Anchor text variat — nu optimizat, generic sau branded

**Social Bookmarks**:
- Reddit (post în subreddit relevant, link spre T1)
- Pinterest (pin cu link spre T1 URL)
- Flipboard (magazine cu articole de la T1)
- Scoop.it, Mix.com

**Target volum T2**: 50–200 proprietăți T2 per site T1
**Cost**: Aproape zero (timp sau servicii de outsourcing ieftine — $50–100 pentru 100 web 2.0s)

**Regulă T2**: Niciun link T2 direct spre T0. T2 linkează NUMAI la T1.

---

### Pas 4: BUILD T3 — Volume Layer (Luna 3)

T3 = automatizare masivă care bombadează T2 cu link juice brut. Spam ok la acest nivel.

**GSA Search Engine Ranker (GSA SER)**:
- Software pentru automated link submissions la mii de platforme
- Setup drip: 20–50 verified links/zi per campanie (nu 1.000/zi — prea agresiv)
- Target exclusiv: URL-uri T2 (web 2.0s, social bookmarks)
- Niciodată T0 sau T1 în lista de targets GSA

**Indexing Services**:
- Speed-Links.net sau echivalent
- Trimite URL-urile T3 spre indexare (Google trebuie să le vadă)
- Fără indexare, T3 = zero impact

**Video Sitemaps** (opțional):
- Crează video-uri scurte (YouTube, Dailymotion) cu link în descriere spre T2
- Trimite sitemap video pentru indexare rapidă

**Regulă T3**: T3 NICIODATĂ direct spre T0 sau T1. Funcționează NUMAI ca accelerator T2.

---

### Pas 5: ACTIVARE T1 → T0 (Luna 3+)

Activarea = momentul în care T1 (parasite sau PBN) publică linkul spre T0.

**Pre-condiții obligatorii înainte de activare**:
- [ ] T1 site/page are minim 30 de zile de existență
- [ ] T1 are minim 10 RDs (din T2 + niche edits)
- [ ] T2 are minim 50 proprietăți care linkează la T1
- [ ] T3 drip activ de minim 2 săptămâni spre T2
- [ ] T0 anchor text plan confirmat (60/20/10/10 ratio)

**Anchor Text Protocol pentru T0**:
```
60% Branded:     "CasinoXYZ" / "BestCasinosPeru" (numele brandului/domeniu)
20% Partial:     "cazino online Peru" / "casino bonos Chile"
10% Exact:       "mejores casinos online Peru 2026"
10% Generic:     "click here" / "visit site" / "read more" / URL nud
```

**Link Velocity pentru T0 nou** (0–6 luni):
- 3–5 linkuri T1 noi per lună (NU mai mult)
- Spacing: minimum 5–7 zile între linkuri noi
- Spike de velocity = red flag pentru Google în nișa gambling

**Link Velocity pentru T0 existent** (6+ luni, DR15+):
- 10–15 linkuri T1 per lună (mai agresiv)
- Mixează tipuri: parasite + PBN + guest posts reale

---

### Pas 6: MONITORIZARE ȘI PROTECȚIE

**Monitorizare lunară** (Ahrefs + Google Search Console):

| Metric | Semnal Sănătos | Semnal de Alertă |
|---|---|---|
| T0 new RDs/lună | 3–15 | 50+ (spike) sau 0 (stagnare) |
| T0 anchor text ratio | 60/20/10/10 | Exact match >20% |
| T1 penalizare Google | 0 manual actions | Manual action detectată |
| T0 DR trend | Creștere lentă | Drop brusc |
| GSC — Coverage | Index stabil | Crawl errors noi |

**Protocol de urgență — T1 penalizat**:
1. Identifică T1-ul penalizat în Ahrefs (DR drop brusc sau manual action în GSC)
2. IMEDIAT: șterge/noindex articolul T1 sau setează linkul T1→T0 la nofollow
3. Remove linkul din profilul T0 (Disavow Tool dacă nu poți controla T1)
4. T0 rămâne curat — nu a primit spam direct
5. Înlocuiește T1 cu un site/pagina nouă după 30 de zile
6. Investighează cauza: footprint PBN? Conținut thin? Spam vizibil T2?

**Disavow Strategy**:
- Nu folosești Disavow la T2/T3 (nu le controlezi direct pe T0)
- Dacă T1 e un PBN compromis care arată spre T0: Disavow imediat
- Dacă T1 e un parasite cu link nofollow: relaxat, risc minim

---

### Pas 7: TIMELINE COMPLET — New Site

```
Luna 1: BUILD T1
  - Cumpără 3–5 domenii expirate PBN (DR20+, TF15+)
  - Setup hosting unic per domeniu
  - Publică conținut pe fiecare PBN (3–5 articole)
  - Crează 2–3 parasite pages (Medium etc.)
  - NU activezi linkuri spre T0 încă

Luna 2: BUILD T2
  - Crează 100–200 web 2.0 properties
  - Social bookmarks spre fiecare T1 URL
  - Target: 50+ T2 per site T1
  - PBN sites: acum au 30 zile vechime

Luna 3: ACTIVARE T3 + Primele T1→T0 linkuri
  - Pornești GSA SER drip: 20–50 verified/zi spre T2
  - Trimite T2/T3 URLs la indexare (Speed-Links)
  - Activezi PRIMUL link T1→T0 (cel mai puternic T1)
  - Anchor: branded

Luna 4–5: SCALE T1→T0
  - Activezi 2–3 linkuri T1→T0 suplimentare
  - Continuă GSA drip și web 2.0s spre T1
  - Monitoring lunar (Ahrefs + GSC)

Luna 6: RANKINGS CHECK
  - Ar trebui să vezi mișcare în SERP pentru keywords target
  - Dacă nu: audit T1 quality, anchor text, competitor gap
  - Dacă da: scale — construiește mai multe T1s
```

---

### Pas 8: SPECIFICAȚII GAMBLING NIȘĂ

Gambling este nișa cu cea mai mare scrutinizare Google. Regulile diferă față de pharma/crypto:

**Praguri mai stricte pentru T1**:
- Parasite host: DR60+ (față de DR40+ în alte nișe)
- PBN: DR20+ TF15+ (față de DR10+ în alte nișe)
- Niche edits: site real cu traffic organic verificabil

**Piețe țintă — DR thresholds ajustate**:
| Piață | T1 PBN DR minim | Parasite DR minim | Competiție |
|---|---|---|---|
| Peru | DR15+ | DR40+ | SCĂZUT |
| Kenya | DR15+ | DR40+ | SCĂZUT |
| Chile | DR20+ | DR50+ | MEDIU |
| Romania | DR25+ | DR60+ | RIDICAT |
| UK/Canada | DR35+ | DR70+ | FOARTE RIDICAT |

**Peru + Kenya avantaj**: Competiție scăzută permite T1 mai slabe să rankeze. Un parasite page pe Medium cu 10 RDs poate ranka #1–3 pentru "best casino Kenya" fără T0.

**Abordare recomandată pentru piețe noi (Peru/Kenya)**:
1. Start cu 2–3 parasite pages (cost: $1.500/pagină)
2. Testează care keyword/pagină rankează mai rapid
3. Construiește money site și T1 PBN abia după ce ai validat market cu parasites
4. Nu investi în PBN full înainte de validare piață

---

## 3. Cortex Logging

```json
{
  "text": "Tiered Link Architecture executat pentru piața [COUNTRY]. Tip campanie: [parasite-first/money-site/hibrid]. T0: [URL]. T1 assets: [N] PBN + [N] parasite. T2 properties: [N]. T3 drip: [X] verified/zi. T1→T0 activat: [DA/NU]. Anchor split T0: [60/20/10/10 confirmat]. DR T0 delta: [+/-X]. Rankings delta: [+/-X]. Cost total: $[X]. Alertă: [NONE/YELLOW/RED].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "TIERED-LINK-ARCHITECTURE",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-008",
    "tags": ["tiered-link-building", "t0-t1-t2-t3", "pbn", "parasite-seo", "gambling", "casino", "architecture", "peru", "kenya", "chile", "anchor-text", "link-equity"],
    "has_enforcement_loop": true,
    "version": "2.0",
    "forge_version": "2.0"
  }
}
```

**Skill saves suplimentare** (la milestone-uri):
- După primul T1→T0 activat: velocity, anchor folosit, semnal în GSC
- La 3 luni: DR change T0, ranking change, cost per link
- La penalizare T1: root cause identificat, recovery time
- La parasite care rankează: DR host, RDs construite, timp până la ranking

**Metrics de tracking**:
- Cost per RD câștigat (target: sub $50/RD în piețe LATAM/Africa)
- Timp mediu de ranking (target: 3-6 luni pentru keyword mediu)
- Survival rate PBN (câte PBN sites supraviețuiesc 12 luni fără penalizare)
- ROI per piață (trafic organic generat x EPC x CR afiliat)

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + La orice planificare sau execuție de campanie tiered link building pentru gambling SEO.

### WHEN
La fiecare campanie nouă (piață nouă sau money site nou). La fiecare activare T1→T0. La fiecare verificare lunară post-activare. La orice alertă galbenă sau roșie.

### HOW (violation detection)
- Link T2 sau T3 trimis direct la T0 → **violation critică** (bypass filtru de protecție)
- T1→T0 activat fără cele 5 pre-condiții îndeplinite (Pas 5) → **violation critică**
- Velocity T0 depășește 5 links/lună pentru site nou (<6 luni) → violation
- Anchor text T0 cu exact-match >20% → violation
- PBN cumpărat cu DR sub 20 sau TF sub 15 → violation
- T1 fără warmup 30 zile activat spre T0 → violation
- Alertă ROȘIE ignorată >48h → **violation critică**
- T2 build absent pentru T1 (T1 fără minim 50 T2 properties) → violation

**Verificări Pre-Build (înainte de a construi orice T1)**:
- [ ] Domeniile PBN cumpărate au DR20+ și TF15+ confirmate în Ahrefs?
- [ ] Hosting T1 PBN: IP unic, provider diferit per domeniu?
- [ ] Conținut T1 (parasite sau PBN): 800+ cuvinte, relevant, nu spun?
- [ ] T0 anchor text plan documentat (60/20/10/10)?
- [ ] Buget T2 alocat (100-200 web 2.0s per T1)?

**Verificări Pre-Activare T1→T0**:
- [ ] T1 are 30+ zile vechime?
- [ ] T1 are 10+ RDs din T2/niche edits?
- [ ] T2 are 50+ proprietăți active?
- [ ] T3 drip activ 14+ zile?
- [ ] Ultimul link T0 primit: 5+ zile în urmă (velocity check)?

**Verificări Lunare**:
- [ ] Ahrefs T0: DR trend, new RDs, anchor text distribution
- [ ] Google Search Console: manual actions, crawl errors, indexation
- [ ] T1 status: toate T1-urile sunt indexate și active?
- [ ] GSA SER: drip-ul funcționează? Verified links log?
- [ ] Piață target: competitors au schimbat ceva? Noi parasite pages rankate?

**Triggers de Escaladare**:

**Alertă GALBENA** (monitorizare mai frecventă):
- T0 DR scade cu 5+ puncte într-o lună
- T0 pierde 20%+ din keywords ranked
- T1 PBN pierde 30%+ din DR

**Alertă ROSIE** (acțiune imediată):
- Manual action în GSC pentru T0
- T1 penalizat + link activ spre T0
- Google update major (Core Update) cu drop >50% trafic T0

**Acțiune la Alertă ROSIE**:
1. STOP toate activitățile de build (înghețare)
2. Audit complet profil backlinks T0 (Ahrefs + GSC)
3. Identifică T1-urile expuse
4. Disavow selectiv sau nofollow T1→T0
5. Așteaptă 30 de zile, reevaluează
6. NU submite reconsideration request fără audit complet

### CONNECT
- TRAIN-SEO-008 → enforcement tiered link architecture gambling SEO
- TRAIN-SEO-006 (PARASITE-LINK-BUILDING) → T1A parasite pages, link building procedură
- TRAIN-SEO-007 (GSA-SER-TIER2-SETUP) → T2/T3 automation layer
- TRAIN-SEO-001 (CASINO-MARKET-ANALYSIS) → prerequisit: piața selectată + DR thresholds
- `procedure-health.json` → adaugă entry
- Training Mode → curriculum SEO avansat, arhitectură link building gambling

### VERIFY
La finalul fiecărei execuții, verifică:
- [ ] Tip campanie clasificat corect: parasite-first / money-site / hibrid (Pas 1)?
- [ ] T1 assets construite conform specificațiilor DR/TF (Pas 2)?
- [ ] T2 properties create: minim 50 per T1 (Pas 3)?
- [ ] T3 drip activ cu GSA SER, target exclusiv T2 (Pas 4)?
- [ ] T1→T0 activat doar cu toate 5 pre-condiții îndeplinite (Pas 5)?
- [ ] Monitoring configurat: Ahrefs + GSC alerts active (Pas 6)?
- [ ] Timeline respectat: Luna 1 build, Luna 2 T2, Luna 3 activare (Pas 7)?
- [ ] Specificații gambling nișă aplicate: DR thresholds per piață (Pas 8)?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `[PROC] TIERED-LINK-ARCHITECTURE | S1 S2 S3 S4 VER | complete`
2. `[CORTEX] "Tiered Link Building Architecture — Gambling SEO" | FORGE | rule: TRAIN-SEO-008 | v1.0`

---

## 5. Dependențe

### Proceduri Prerequisite

- **TRAIN-SEO-001** (`CASINO-MARKET-ANALYSIS.md`) — Analiza pieței trebuie completată înainte de a decide tipul de T1 (parasite vs PBN) și DR thresholds necesare pentru piața specifică.

### Tool-uri Necesare

| Tool | Utilizare | Nivel |
|---|---|---|
| Ahrefs | Verificare DR/TF domenii T1, monitoring T0, SERP analysis | OBLIGATORIU |
| GSA Search Engine Ranker | T3 automated submissions | OBLIGATORIU pentru T3 |
| Speed-Links.net (sau similar) | Indexare T2/T3 URLs | RECOMANDAT |
| Google Search Console | Monitoring T0 manual actions, indexation | OBLIGATORIU |
| Expireddomains.net / GoDaddy Auctions | Sourcing domenii expirate pentru PBN | Pentru T1B |
| WordPress + hosting | Setup PBN sites | Pentru T1B |
| Medium / LinkedIn / platforme parasite | Publicare T1A parasite pages | Pentru T1A |

### Resurse de Referință

- `memory/research/seo-2026-full-spectrum.md` — Context complet white/grey/black hat + PBN construction + tiered building deep-dive
- `memory/research/gambling-markets-global-2026.md` — Profil piețe Peru/Kenya/Chile, competitor landscape, DR mediu în top 10
- `TRAIN-SEO-001` (`CASINO-MARKET-ANALYSIS.md`) — Framework analiză piață pre-investiție

### Buget Estimat Campanie Completă (Single T0 + 3 T1s)

| Componentă | Cost Estimat |
|---|---|
| 3 PBN domains (expirate, DR20+) | $300–900 |
| Hosting PBN (3 servere IP unic) | $30–60/lună |
| 2 Parasite pages cu 10 RDs fiecare | $3.000 ($1.500 × 2) |
| T2 web 2.0s (outsourcing 200 proprietăți) | $100–200 |
| GSA SER license | $99 (one-time) |
| Indexing service (6 luni) | $60–120 |
| **Total setup (Luna 1–3)** | **$3.600–4.400** |
| **Lunar operațional (din Luna 4)** | **$100–200** |

**Break-even estimate (Peru/Kenya)**: 500–1.000 vizitatori organici/lună × CR 2–5% × CPA $30–80 = $300–4.000/lună potențial după Luna 6.

---

**DISCLAIMER**: Această procedură descrie tehnici de Grey/Black Hat SEO (tiered link building, PBN networks, automated submissions) utilizate în industria afiliată de gambling. Construirea de PBN-uri și link building automatizat încalcă Google Webmaster Guidelines. Riscurile includ penalizări manuale sau algoritmice pe T0 money site, pierderea indexării T1 assets, și ban-ul conturilor de pe platforme parasite. Procedura este documentată exclusiv în scop educațional și de cercetare SEO avansată. Utilizatorul este singurul responsabil pentru deciziile de implementare și consecințele lor.

---

*Procedură FORGE — TRAIN-SEO-008 | v1.0 | 2026-03-04*
