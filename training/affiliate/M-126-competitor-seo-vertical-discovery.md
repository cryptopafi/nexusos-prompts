---
type: procedure
created: 2026-03-17
status: active
slug: m-126-competitor-seo-vertical-discovery
tags: [procedure, nexus]
---

# M-126 — Competitor SEO Intelligence for New Vertical & Product Discovery

**Status**: ACTIVE
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Domeniu**: Affiliate / Competitive Intelligence / SEO
**Creat**: 2026-03-05
**Autor**: Genie Slim Core

---

## Scope

Procedura M-126 definește procesul complet de intelligence competitiv bazat pe SEO pentru descoperirea de verticale noi și produse profitabile pentru operațiunile SMSads.net. Analiza se bazează pe reverse-engineering-ul competitorilor afiliați (SEO și SMS) folosind SimilarWeb, SEMrush, Ahrefs și Google Trends pentru a extrage semnale de cerere, comisioane și GEO-uri neexploatate. Outputul procedural alimentează direct pipeline-ul de selecție verticală (M-122) și scoring-ul ofertelor (M-123), reducând ciclul de diversificare de la 6-12 luni la 2-3 săptămâni.

---

## Secțiunea 1 — Problema

### 1.1 Dependența de oferta curentă 3SNET

Fără research competitiv SEO, descoperirea de verticale noi depinde exclusiv de ce 3SNET listează activ azi. Această abordare reactivă ignoră trei semnale critice pe care competitorii care rankează organic le revelează deja:

**(a) Ce produse convertesc la scară** — Un competitor care investește în SEO pentru un keyword cu KD 40+ face asta numai dacă ROI-ul e dovedit. Prezența lor în top 10 organic e un proxy pentru faptul că oferta respectivă convertește.

**(b) Ce comisioane sunt suficient de mari** — Costul SEO (timp + link building) e justificat doar dacă comisionul suportă marja. Dacă 3 competitori rankează agresiv pentru "best trading app Kenya", comisionul pentru trading în Kenya e probabil >$50 CPA sau >30% RevShare.

**(c) Ce GEO-uri sunt neservite** — Un vertical cu volum de căutare >500/lună într-un GEO și zero competitori specializați = oportunitate directă. M-126 identifică aceste gap-uri înainte ca alți actori să le ocupe.

**Contrast direct**: Fără M-126, diversificarea verticală ia 6-12 luni de trial and error (lansezi, aștepți date, pivotezi). Cu M-126, intelligence-ul competitiv comprimă ciclul la 2-3 săptămâni: identifici ce funcționează deja, copiezi unghiul, aplici pe rețea, lansezi cu date validate.

### 1.2 Gap-urile de intelligence actuale ale SMSads

SMSads operează în prezent cu zero vizibilitate competitivă structurată:

| Gap | Situație curentă | Impact |
|-----|-----------------|--------|
| STM Forum Radar | Zero monitorizare activă a threadurilor SMS/LATAM/Africa | Ratăm insight-uri din comunitatea de performance marketing |
| SimilarWeb Monitoring | Zero tracking pe competitorii afiliați | Nu știm dacă competitorii cresc sau scad în verticala noastră |
| Competitor Keyword Tracking | Zero keyword tracking pe domenii terțe | Nu detectăm verticale noi pe care competitorii încep să le testeze |
| SMS LP Discovery | Zero metodologie pentru găsirea LP-urilor SMS ale competitorilor | Nu știm ce copy angles și oferte folosesc SMS affiliates paraleli |
| Offer Commission Benchmarking | Zero date comparative de comision per vertical per GEO | Nu știm dacă 3SNET ne oferă rate competitive |

Aceste gap-uri creează decizii luate în vid informațional — exact ce M-126 rezolvă.

---

## Secțiunea 2 — Procedura

### Step 1: Seed Competitor Identification

**Obiectiv**: Identificarea a 5-10 site-uri competitor afiliate per vertical/GEO ca punct de plecare pentru analiza SEO.

**Metodologie de identificare**:

**Google Search — advanced operators**:
```
"[product] affiliate" + "[GEO]" + "review"
"best [product] [GEO]" + "bonus" + "sign up"
"[vertical] affiliate site [GEO]" site:*.com OR site:*.net
"promo code [operator]" + "[GEO]"
```
Exemple practice:
- `"casino affiliate" Bolivia review`
- `"trading app affiliate" Kenya best 2025`
- `"loan affiliate" Romania comision`

**SimilarWeb — Industry Search**:
- Navighează la: Industry Analysis > Gambling / Finance / Shopping (per vertical)
- Filtrează după: GEO țintă (Bolivia, Kenya, Romania)
- Sortează după: Traffic Source "Search" > 40%
- Exportă top 20 domenii

**SEMrush / Ahrefs — Seed Keyword Approach**:
- Introduce seed keywords: `"casino affiliate Bolivia"`, `"SMS marketing affiliate"`, `"trading affiliate Bolivia"`
- Utilizează: Keyword Overview > SERP Analysis → identifică domeniile care rankează
- Exportă domeniile din SERP pentru primele 10 rezultate per keyword

**STM Forum Search**:
- Search: `"SMS Bolivia"`, `"SMS LATAM affiliate"`, `"SMS Africa"`
- Caută threaduri cu >10 răspunsuri (indiciu de activitate reală)
- Notează domeniile menționate, afiliații activi, networkurile recomandate

**Tool-uri**: SimilarWeb, SEMrush, Ahrefs, Google Search, STM Forum

**Output**: `competitor-seed-list-{vertical}-{GEO}.md`

**Template tabel output**:

| Domain | SimilarWeb Rank | Monthly Visits | Primary Vertical | Primary GEO | Revenue Est | Primary Traffic Source | Affiliate Network | Key Product | Notes |
|--------|----------------|---------------|-----------------|-------------|-------------|----------------------|------------------|-------------|-------|
| example-affiliate.com | 450,000 | 85,000 | Betting | Bolivia | $15K/mo est | Organic (62%) | 3SNET | Sportaza Bolivia | LP simplu, CTA direct |
| trading-reviews-ke.com | 890,000 | 32,000 | Trading CFD | Kenya | $8K/mo est | Organic (71%) | HasOffers | IQ Option | Blog + review |

**Checkpoint**: Minimum 5 competitori identificați per vertical per GEO. Dacă nu sunt găsiți 5, extinde căutarea Google cu variante de keyword (sinonime, limba locală).

---

### Step 2: Keyword Intelligence per Competitor

**Obiectiv**: Extragerea top 50 keywords organice per competitor pentru a înțelege ce verticale și produse monetizează.

**Metodologie**:

Pentru top 3 competitori din Step 1, accesează în SEMrush sau Ahrefs:
- **SEMrush**: Domain Overview > Organic Research > Positions > Export CSV
- **Ahrefs**: Site Explorer > Organic Keywords > Export CSV

Filtrează: primele 50 de keyword-uri după trafic estimat (nu după poziție).

**Clasificare obligatorie per keyword**:

| Dimensiune | Categorii | Exemplu |
|-----------|-----------|---------|
| Intent | Commercial / Informational / Transactional | "best casino Kenya" = Commercial |
| Product Category | betting / trading / loans / health / apps / other | betting |
| GEO Specificity | Generic / GEO-specific | GEO-specific |
| Volume | Monthly search volume (număr) | 1,200/lună |
| KD | Keyword Difficulty 0-100 | 34 |

**Insight de interpretat**: Dacă un competitor rankează pe 15+ keywords din categoria "trading" cu KD >30, înseamnă că au investit semnificativ în SEO pentru trading → trading e probabil profitabil în acel GEO.

**Tool-uri**: SEMrush Organic Research, Ahrefs Site Explorer

**Output**: `competitor-keywords-{domain}.csv`

**Template CSV (50 rânduri)**:

```
Keyword | Volume | KD | Intent | Product | GEO | Competitor Position | Our Position
best casino Bolivia | 880 | 28 | Commercial | betting | Bolivia | 3 | N/A
casino bonus Bolivia | 590 | 31 | Commercial | betting | Bolivia | 7 | N/A
IQ Option Kenya review | 1200 | 22 | Commercial | trading | Kenya | 2 | N/A
```

**Checkpoint**: Minimum 50 keywords extrase per competitor. Intent classification 100% completă (nicio celulă goală). Dacă un competitor are <50 keywords organice, notează în tabel și continuă cu alt competitor din seed list.

---

### Step 3: Affiliate Offer Reverse Engineering

**Obiectiv**: Identificarea ofertelor specifice promovate de competitori pentru a descoperi operatori, comisioane și structuri de LP.

**Metodologie**:

Vizitează top 3 landing pages per competitor (paginile cu cel mai mult trafic din Step 2 — verifică în SimilarWeb "Top Pages" sau Ahrefs "Top Pages by Traffic").

**Pentru fiecare LP, documentează**:

1. **Operatorul promovat**: Numele brandului (Sportaza, 1xBet, IQ Option etc.)
2. **Network link structure**: Urmărește link-ul afiliat (hover sau click cu redirect logger)
   - HasOffers: `tracking.{network}.com/aff_c?offer_id=`
   - 3SNET: `go.3snet.co/`
   - MaxBounty: `tracking.maxbounty.com/`
   - CAKE: `{brand}.go2jump.org/`
3. **CTA și bonus evidențiat**: "Get 100% bonus up to $200" → indică că comisionul suportă marja pentru un bonus atât de mare
4. **Structura LP**: Direct-to-operator (1 click) vs pre-sell page (articol/review înainte de redirect)

**Cross-referință comision**:
- Caută oferta identificată pe 3SNET, MaxBounty, HasOffers
- Compară CPA/RevShare disponibil public cu ce promovează competitorul

**Tool-uri**: Browser direct, redirect logger (e.g., WhereGoes.com), Google Cache pentru pagini șterse

**Output**: `competitor-offers-{vertical}-{GEO}.md`

**Template tabel output**:

| Competitor | LP URL | Offer Name | Network | Est. Commission | Traffic to LP (SimilarWeb) | Bonus Promoted |
|-----------|--------|-----------|---------|----------------|---------------------------|----------------|
| affiliate-bo.com | /sportaza-review | Sportaza Bolivia | 3SNET | $45 CPA est | 12,000 visits/mo | 100% up to $100 |
| trading-ke.com | /iq-option-kenya | IQ Option Kenya | HasOffers | 35% RevShare | 8,500 visits/mo | $10 no-deposit bonus |

**Checkpoint**: Minimum 3 oferte identificate cu network source confirmat. Cel puțin 1 ofertă cu comision >$30 CPA sau RevShare >25%. Dacă nu sunt găsite, extinde la top 5 LP-uri per competitor.

---

### Step 4: Vertical Trend Analysis

**Obiectiv**: Validarea cererii actuale și a direcției de trend pentru fiecare vertical candidat identificat în Steps 2-3.

**Metodologie per vertical candidat**:

**Google Trends (gratuit)**:
- Setează: 12 luni, GEO specific (Bolivia, Kenya, Romania)
- Compară 3-5 verticale simultan (feature "Compare")
- Exportă graficul ca date CSV
- Interpretare: >20% creștere în 12 luni = GROWING; ±10% = STABLE; >20% scădere = DECLINING

**SEMrush Keyword Overview**:
- Caută seed keyword per vertical în GEO
- Verifică "Trend" (graficul de volum lunar): crescător sau descrescător?
- Notează variația YoY (an față de an)

**SimilarWeb Industry Analysis**:
- Industry > [Vertical specific] > [GEO]
- Verifică "Total Industry Traffic" — crește sau scade industria?

**AdPlexity Mobile (dacă disponibil)**:
- Search: vertical keyword + GEO
- Verifică dacă există reclame mobile/SMS active pentru vertical în GEO
- Prezența de reclame = validation că vertical e monetizabil via mobile

**Decision Rule**:

```
GROWING trend + Volume >200/lună în GEO + competitor prezent = OPPORTUNITY → testează
GROWING trend + Volume >200/lună în GEO + ZERO competitor = PRIORITY OPPORTUNITY → testează urgent
STABLE + Volume >500/lună + competitor prezent = CONSIDER → evaluează comision
DECLINING sau Volume <200/lună = AVOID → nu aloca resurse
```

**Tool-uri**: Google Trends, SEMrush Trends, SimilarWeb Industry, AdPlexity Mobile

**Output**: `vertical-trend-{GEO}.md`

**Template tabel output**:

| Vertical | 12mo Trend | Volume YoY | Competitor Prezent | SMS Ads Prezent | Decision |
|---------|-----------|-----------|-------------------|----------------|---------|
| Betting | GROWING +35% | +28% | DA (8 site-uri) | DA | OPPORTUNITY |
| Trading CFD | GROWING +18% | +15% | DA (3 site-uri) | PARȚIAL | CONSIDER |
| Crypto Exchange | STABLE | -5% | DA (12 site-uri) | NU | AVOID (saturat) |
| App Downloads | GROWING +52% | +47% | NU | NU | PRIORITY OPPORTUNITY |
| Loans Online | DECLINING -20% | -25% | DA (2 site-uri) | NU | AVOID |

**Checkpoint**: Minimum 5 verticale analizate per GEO. Coloana "Decision" 100% completă. Nicio decizie fără date de trend + volum + competitor status.

---

### Step 5: Traffic Source Analysis

**Obiectiv**: Înțelegerea de unde vine traficul competitorilor pentru a distinge competitorii SEO (vizibili) de cei SMS/push (ascunși).

**Metodologie**:

Pentru top 5 competitori din seed list, accesează în SimilarWeb:
- Domain Overview > Traffic Sources (pie chart)
- Exportă: % Organic, % Direct, % Social, % Referral, % Paid Search, % Mail

**Interpretare pattern-uri**:

| Pattern trafic | Interpretare | Acțiune |
|---------------|--------------|---------|
| Organic >60% | SEO afiliat pur — studiază keywords și LP-uri | Aplică Steps 2-3 complet |
| Direct >40% | Brand puternic SAU trafic SMS/push (nu apare ca sursă) | Investighează LP-urile lor (Step 6) |
| Referral >30% | Trafic din rețele afiliate sau backlinks | Urmărește link-urile referrer |
| Paid Search >30% | Campanii PPC active — vertical profitabil | Notează ofertele promovate via ads |
| Social >25% | Social ads sau organic social | Verifică paginile de Facebook/TikTok |

**Insight critic SMS**: Un competitor cu 60%+ "Direct" + "Referral" și trafic >20K/lună e probabil un SMS/push afiliat deghizat ca site de review. LP-urile lor sunt template-uri pentru SMS copy. Identificarea acestor site-uri e mai valoroasă decât SEO affiliates clasici.

**Tool-uri**: SimilarWeb Traffic Sources report

**Output**: `traffic-source-analysis-{competitor}.md`

**Template output**:

```markdown
## Traffic Source Analysis: {domain}

**Monthly Visits**: X,XXX
**Breakdown**:
- Organic Search: XX%
- Direct: XX%
- Referral: XX%
- Social: XX%
- Paid Search: XX%
- Mail: XX%

**Interpretare**: [pattern identificat] → [acțiune recomandată]

**Hidden SMS indicator**: [DA/NU] — Motivare: [...]
```

**Checkpoint**: Traffic source breakdown complet pentru toți 5 competitori seed. Cel puțin 1 competitor identificat cu "Hidden SMS indicator: DA" (sau motivare clară de absență).

---

### Step 6: SMS-Specific Competitor Discovery

**Obiectiv**: Identificarea landing page-urilor SMS ale competitorilor — distinct față de site-urile SEO afiliate — pentru a extrage copy angles și structuri de LP direct aplicabile în campaniile SMSads.

**Metodologie**:

**Google Search cu operatori avansați**:
```
"[offer name]" "promo code" site:bit.ly OR site:t.co OR short.link
"[operator]" "bonus" "SMS" "[GEO]"
"[product]" "înregistrare" OR "registro" "SMS" "[GEO]"
"[offer]" "click aici" OR "haz clic" filetype:html -site:operator.com
```

Exemple practice (Bolivia):
- `"Sportaza" "promo code" site:bit.ly`
- `"casino" "bonus" "SMS" "Bolivia" 2025`
- `"1xBet" "registro" "SMS" Bolivia`

**Pattern Binom/Voluum LP**:
SMS landing pages au caracteristici distincte:
- URL scurt sau random (nu keyword-friendly)
- Conținut minimal: logo + tagline + CTA button
- Zero navigare, zero footer, zero blog
- Single conversion action (click to offer sau colectare număr telefon)
- Fast load time (<2s — optimizat pentru mobil 3G)

**STM Forum Searches**:
- Caută: `"SMS Bolivia"`, `"SMS Peru"`, `"SMS Kenya"`, `"SMS LATAM"`
- Filtrează threaduri cu atașamente sau link-uri externe (LP-uri partajate)
- Notează trackeri menționați (Binom, Voluum, RedTrack) — indiciu de budget serios

**Pentru fiecare SMS LP găsit**:
- Documentează: structura vizuală, copy-ul principal (headline + CTA), oferta promovată, estimare volum de trimitere (dacă tracker-ul e vizibil în sursă)
- Screenshot obligatoriu

**Tool-uri**: Google Search (advanced operators), STM Forum search, Google Cache, Browser DevTools

**Output**: `sms-competitor-lps-{GEO}.md`

**Template tabel output**:

| LP URL | Offer | Copy Angle | CTA Text | Est. Volume | Network | SMS Indicator |
|--------|-------|-----------|---------|-------------|---------|--------------|
| bit.ly/xxx → redirect | Sportaza Bolivia | "Joacă fără risc azi" | "Ia bonusul acum" | ~50K SMS/săpt est | 3SNET | URL scurt + LP minimal |
| short.link/yyy | IQ Option Kenya | "Dublu-ți primul depozit" | "Start Trading" | necunoscut | HasOffers | Single CTA, 0 navigare |

**Checkpoint**: Minimum 3 SMS-specific LP-uri identificate per GEO. Dacă nu sunt găsite, notează absența ca semnal (piața SMS e imatura în GEO → oportunitate de first-mover) și continuă cu Step 7.

---

### Step 7: Opportunity Brief

**Obiectiv**: Sintetizarea Steps 1-6 într-un brief acționabil, gata de decizie, care alimentează M-122 și M-123.

**Metodologie de sinteză** (rulat cu Claude Opus):

Aggreghează toate outputurile din Steps 1-6 și generează brief-ul folosind prompt-ul:

```
Ești un expert în affiliate marketing și competitive intelligence.
Ai primit datele din analiza SEO competitivă pentru [GEO].
Sintetizează în format brief: top 3 verticale noi + top 5 oferte + top 3 copy angles.
Format output: secțiuni clare, bullet points, tabel action items cu Owner + Deadline.
Date input: [paste outputs Steps 1-6]
```

**Structura obligatorie a brief-ului**:

```markdown
# Vertical Discovery Brief — {GEO} — {date}

## Top 3 Verticale Recomandate
| Rank | Vertical | GEO | Demand Signal | Commission Est | SMS Compatibility | Competition Gap |
|------|---------|-----|--------------|----------------|------------------|----------------|
| 1 | ... | ... | GROWING +X% | $XX CPA | HIGH | LOW |

## Top 5 Oferte de Aplicat (3SNET / Alternative)
| Rank | Offer | Operator | Network | CPA/RevShare | GEO | Status |
|------|-------|---------|---------|-------------|-----|--------|
| 1 | ... | ... | 3SNET | $XX | ... | APPLY NOW |

## Top 3 Copy Angles (din competitor research)
1. Angle: "..." — Sursă: {competitor} — Aplicare: {vertical/offer}
2. Angle: "..." — Sursă: {competitor} — Aplicare: {vertical/offer}
3. Angle: "..." — Sursă: {competitor} — Aplicare: {vertical/offer}

## Keywords de Adăugat în M-117 Monitoring
- keyword 1 (volum X, GEO)
- keyword 2 (volum X, GEO)

## Competitori Noi pentru M-117 Stack
- domain.com — vertical — GEO — prioritate HIGH/MEDIUM/LOW

## Action Items
| # | Acțiune | Owner | Deadline | Prioritate |
|---|--------|-------|---------|-----------|
| 1 | Aplică pentru oferta X pe 3SNET | Pafi | +3 zile | HIGH |
| 2 | Testează LP angle Y pentru vertical Z | Pafi | +7 zile | MEDIUM |
```

**Tool**: Claude Opus (sinteză), Claude Sonnet (colectare date Steps 1-6)

**Output**: `vertical-discovery-brief-{GEO}-{date}.md`

**Checkpoint**: Brief conține minimum: 3 verticale + 5 oferte + 3 copy angles + tabel action items cu Owner + Deadline completate. Brief e ≤2 pagini A4 (dense, acționabile — fără fluff).

---

## Competitor Analysis Master Table

Template centralizat pentru tracking cross-run. Actualizat după fiecare rulare M-126.

**Locație fișier**: `~/.nexus/smsads/competitor-master-table.md`

| Domain | Vertical | GEO | Monthly Visits | Primary Traffic | Top Keyword | Top Offer | Est Commission | SMS Evidence | Opportunity Score |
|--------|---------|-----|---------------|----------------|------------|----------|----------------|--------------|-----------------|
| sportaza-affiliate.com | Betting | Bolivia | 85,000 | Organic 62% | "casino Bolivia bonus" | Sportaza BO | $45 CPA | NU | 8/10 |
| trading-reviews-ke.com | Trading CFD | Kenya | 32,000 | Organic 71% | "IQ Option Kenya review" | IQ Option | 35% RevShare | NU | 7/10 |
| bo-apps-download.net | App Downloads | Bolivia | 18,000 | Direct 58% | N/A (Direct-heavy) | AppFlood CPL | $2.50 CPL | DA (LP minimal) | 9/10 |
| romania-trading.ro | Trading | Romania | 120,000 | Organic 78% | "broker forex Romania" | XTB Romania | €150 CPA | NU | 6/10 |
| kenya-loan-fast.com | Loans | Kenya | 45,000 | Social 44% | "quick loan Kenya" | Tala Kenya | $8 CPA | PARȚIAL | 5/10 |

**Opportunity Score calcul**:
- Demand GROWING: +3
- Commission >$30 CPA sau >25% RevShare: +2
- SMS Evidence: +2
- Competition gap (≤3 competitori în GEO): +2
- Volume >500/lună în GEO: +1

---

## Secțiunea 3 — Cortex Logging

La finalul fiecărei rulări M-126, salvează în Cortex:

```json
{
  "domain": "smsads",
  "sub_domain": "competitor-seo-intelligence",
  "procedure": "M-126",
  "version": "2.0",
  "run_date": "{YYYY-MM-DD}",
  "geo_analyzed": "{GEO}",
  "vertical_analyzed": "{vertical}",
  "competitors_found": {number},
  "verticals_discovered": [{vertical1}, {vertical2}, {vertical3}],
  "offers_identified": [{offer1}, {offer2}, {offer3}, {offer4}, {offer5}],
  "copy_angles_captured": {number},
  "sms_lps_found": {number},
  "brief_path": "~/.nexus/smsads/briefs/vertical-discovery-brief-{GEO}-{date}.md",
  "master_table_updated": true,
  "feeds_into": ["M-117", "M-122", "M-123", "M-115"],
  "next_run_scheduled": "{YYYY-MM-DD}",
  "tags": ["smsads", "competitor-intelligence", "SEO", "vertical-discovery", "Bolivia", "Romania"],
  "notes": "{observații relevante din run}"
}
```

---

## Secțiunea 4 — Enforcement Loop

### WHERE

- **Trigger periodic**: Rulat lunar (1 în fiecare lună), indiferent de alte activități
- **Trigger on-demand**: Activat imediat când apare orice discuție despre "testăm o verticală nouă", "intrăm într-un GEO nou", sau "3SNET a adăugat oferte noi"
- **Trigger automat**: Dacă M-117 raportează un competitor nou cu trafic >10K/lună în noul GEO

### WHEN

- **Rulare lunară**: Prima zi a lunii (sau primul business day)
- **Rulare ad-hoc**: Maximum 48h de la trigger on-demand
- **Review master table**: La fiecare 30 de zile — competitori inactivi (scădere trafic >30%) sunt marcați INACTIVE și înlocuiți

### HOW — Violations

| Tip | Condiție | Acțiune |
|-----|---------|--------|
| VIOLATION | Verticală nouă lansată fără M-126 completat | STOP lansare. Rulează M-126 urgent. Notifică în Cortex. |
| VIOLATION | Ofertă aplicată pe 3SNET fără Step 3 completat | Suspendă aplicarea. Completează reverse engineering. |
| WARNING | Competitor list neactualizată în >30 zile | Rulează Step 1 pentru top GEO-uri. |
| WARNING | Brief negenerat după run complet | Rulează Step 7 în 24h. |
| INFO | SMS LP-uri <3 identificate per GEO | Notează ca "SMS imatur în GEO" — oportunitate first-mover. |

### CONNECT

| Procedură | Relație | Direcție |
|----------|--------|---------|
| **M-117** | Competitorii identificați în M-126 se adaugă în monitoring stack M-117 | M-126 → M-117 |
| **M-122** | Verticalele recomandate în brief alimentează direct selecția verticală | M-126 → M-122 |
| **M-123** | Ofertele identificate în Step 3 intră în scoring pipeline M-123 | M-126 → M-123 |
| **M-115** | Oferte noi descoperite prin competitor research se adaugă în M-115 offer discovery | M-126 → M-115 |
| **M-122** | Outputul M-122 (verticale selectate) poate declanșa M-126 pentru GEO-ul respectiv | M-122 → M-126 |

### VERIFY — 6 Checks

| # | Check | Metodă de verificare | Pass Criteria |
|---|-------|---------------------|--------------|
| 1 | Competitor seed list completă | Numără rândurile din `competitor-seed-list-{vertical}-{GEO}.md` | ≥5 competitori per GEO |
| 2 | Keywords extrase și clasificate | Deschide CSV, verifică coloana "Intent" — zero celule goale | 50 keywords, Intent 100% completat |
| 3 | Oferte cu network confirmat | Verifică coloana "Network" din `competitor-offers-*.md` | ≥3 oferte, ≥1 cu commission >$30 CPA |
| 4 | Trend analysis Decision completat | Verifică coloana "Decision" din `vertical-trend-{GEO}.md` | ≥5 verticale, Decision 100% completat |
| 5 | Brief generat și acționabil | Deschide brief, verifică secțiunile: 3 verticale + 5 oferte + 3 angles + action items | Toate secțiunile prezente, Owner + Deadline completate |
| 6 | Cortex logging efectuat | Confirmă save Cortex cu run_date curent | JSON valid, toate câmpurile completate |

### MODEL ROUTING

| Task | Model | Justificare |
|------|-------|------------|
| Steps 1-6: data collection, clasificare keywords, documentare offers | Claude Sonnet | Task repetitiv, structurat — eficiență cost |
| Step 7: sinteză Opportunity Brief | Claude Opus | Raționament complex, recomandări strategice, prioritizare |
| Interpretare pattern-uri neobișnuite de trafic | Claude Opus | Judecată calitativă necesară |
| Update master table | Claude Sonnet | Task mecanic, structurat |

---

## Secțiunea 5 — Dependente

| Dependență | Tip | Obligatorie | Alternativă |
|-----------|-----|------------|-------------|
| SEMrush subscription (plan Business sau Agency) | Tool plătit | DA — pentru Organic Research | Ahrefs (echivalent) |
| Ahrefs subscription | Tool plătit | RECOMANDAT | SEMrush (dacă există deja) |
| SimilarWeb Pro | Tool plătit | DA — pentru Traffic Source Analysis | SimilarWeb Free (limitat la 5 site-uri/lună) |
| STM Forum — membership activ | Comunitate | RECOMANDAT | BlackHatWorld (alternativă parțială) |
| AdPlexity Mobile | Tool plătit | OPȚIONAL | Appraisal: skip dacă budget limitat |
| M-122 output (verticale candidate) | Procedură internă | DA — seed pentru GEO targeting | Manual GEO selection |
| Google Trends | Gratuit | DA | N/A |
| Browser + redirect logger | Gratuit | DA — pentru Step 3 | WhereGoes.com (gratuit) |

---

## Secțiunea 6 — Metrics

| Metric | Target | Metodă de măsurare | Frecvență |
|--------|--------|-------------------|-----------|
| Verticale noi descoperite per run | ≥2 cu status OPPORTUNITY | Count din `vertical-trend-{GEO}.md` Decision=OPPORTUNITY | Per run (lunar) |
| Oferte identificate cu network confirmat | ≥5 per run | Count din `competitor-offers-*.md` cu Network completat | Per run |
| Copy angles capturate per run | ≥3 per run | Count din brief, secțiunea Copy Angles | Per run |
| SMS-specific LP-uri descoperite | ≥3 per GEO | Count din `sms-competitor-lps-{GEO}.md` | Per run |
| Timp total per run complet (Steps 1-7) | <4h | Timer start/stop per run, logat în Cortex | Per run |
| Rata de conversie verticală (discovery → lansare) | ≥1 verticală lansată per trimestru | Track în projects-tracker | Trimestrial |
| Competitor master table coverage | ≥10 competitori activi per GEO principal | Count rânduri active în master table | Lunar |

---

## Checklist Pre-Publicare

- [x] Header complet (Status, Versiune, Regula, Scope 3 propoziții)
- [x] Secțiunea 1 — Problema articulată clar cu date specifice SMSads (gap-uri curente)
- [x] Contrast trial-and-error vs intelligence (6-12 luni vs 2-3 săptămâni)
- [x] Step 1 — Seed Identification: metodologie completă (Google, SimilarWeb, SEMrush, STM)
- [x] Step 2 — Keyword Intelligence: clasificare 5 dimensiuni, template CSV
- [x] Step 3 — Offer Reverse Engineering: network identification, commission cross-reference
- [x] Step 4 — Trend Analysis: Decision Rule explicit (GROWING/STABLE/DECLINING)
- [x] Step 5 — Traffic Source Analysis: interpretare pattern SMS ascuns
- [x] Step 6 — SMS LP Discovery: advanced operators + Binom/Voluum patterns
- [x] Step 7 — Opportunity Brief: template complet cu toate secțiunile obligatorii
- [x] Competitor Analysis Master Table: template + 5 rânduri exemplu (Bolivia, Kenya, Romania)
- [x] Cortex Logging JSON: toate câmpurile, tags corecte
- [x] Enforcement Loop: WHERE/WHEN/HOW violations/CONNECT/VERIFY/MODEL ROUTING
- [x] Dependente: 8 intrări cu alternativă pentru fiecare
- [x] Metrics: 7 KPI-uri cu target și metodă de măsurare
- [x] Connections la M-115, M-117, M-122, M-123 documentate explicit
