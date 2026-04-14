---
type: procedure
created: 2026-03-17
status: active
slug: m-117-competitive-intelligence-automation
tags: [procedure, nexus]
---

# Competitive Intelligence Automation — Weekly Spy per GEO — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Monitorizare recurenta saptamanala a competitiei pe piete active de gambling afiliat (LATAM/Africa), cu brief actionabil si feed direct in campaniile active.

---

## 1. Problema

M-007 acopera competitor spy manual, o data per oferta. In gambling LATAM/Africa 2026, campaniile castigatoare apar si dispar in 48-72h. Fara monitorizare continua automatizata: ferestre de oportunitate pierdute, reactie la piata cu lag 2-4 saptamani, creative copy outdated, keyword gaps neexplorate, competitori noi nedetectati.

**Situatii acoperite:**
- Setup initial monitoring stack pe GEO-uri active (Peru, Kenya, Chile + extensii)
- Weekly intelligence scrape + synthesis cu brief actionabil
- Detectie pattern-uri castigatoare prin persistenta si volum
- Feed insights direct in campanii active cu task-uri prioritizate
- Alertare competitori noi agresivi in <72h

**Ce se intampla fara procedura:**
- Campanii competitor profitabile ruleaza 2-4 saptamani nedetectate
- Creative angles testate de altii nu sunt replicate/adaptate
- Keyword gaps descoperite prea tarziu (competitor deja ranked)
- Zero vizibilitate pe operatori noi care intra agresiv pe piata
- Decizii creative bazate pe intuitie, nu pe date competitive

## 2. Procedura

### Pas 1: SETUP MONITORING STACK
Configureaza instrumente de monitorizare continua per GEO activ:
- Meta Ad Library: search-uri salvate cu termeni verticali ("casino", "apuestas", "betting", "slots") per tara, filtru "active only"
- TikTok Creative Center: bookmarks per GEO cu filtre pe vertical gambling
- Google Alerts: fraze cheie per GEO ("casino affiliate Peru", "gambling affiliate Kenya", "apuestas Chile")
- SimilarWeb: alerts pe top 10 site-uri competitor per GEO
- SEO tracker (Ahrefs/SEMrush): proiecte per GEO cu keyword-uri target monitorizate

**Tool**: Meta Ad Library, TikTok Creative Center, Google Alerts, SimilarWeb, Ahrefs/SEMrush
**Output**: Document "Monitoring Stack Registry" — lista completa instrumente configurate per GEO cu date activare si status
**Checkpoint**: Stack valid = minim 4 din 5 instrumente active per GEO. Stack incomplet = BLOCKER pentru Pas 2

Setup unic per GEO, revalidat lunar. La adaugare GEO nou, re-executa Pas 1 inainte de includere in ciclul saptamanal.

### Pas 2: WEEKLY AD LIBRARY SCRAPE
Colecteaza date din Ad Library-uri (Meta + TikTok) si documenteaza top 5 reclame active per GEO per platforma.

Per reclama documenteaza: screenshot, copy text complet, hook principal, format (video/static/carousel), operator/brand promovat, landing page URL, data primei aparitii, numar zile activa.

**Tool**: Meta Ad Library, TikTok Creative Center, Google Sheets
**Output**: Spreadsheet "Weekly Ad Scrape" cu urmatoarele coloane obligatorii:

| GEO | Platform | Operator | Ad Format | Hook/Angle | Copy Text | LP URL | First Seen | Days Active | Volume Indicator | Notes |
|-----|----------|----------|-----------|------------|-----------|--------|------------|-------------|------------------|-------|

Minim 5 reclame per GEO per platforma. "Volume Indicator" = LOW / MEDIUM / HIGH bazat pe numarul de variante active ale aceluiasi operator.

**Checkpoint**: Spreadsheet completat cu minim 10 randuri per GEO (5 Meta + 5 TikTok). Randuri cu campuri goale = invalide.

### Pas 3: DETECT WINNING PATTERNS
Analizeaza datele din Pas 2 si identifica pattern-uri castigatoare prin doua criterii: persistenta (reclama activa 14+ zile = probabil profitabila) si volum (operator cu 10+ variante active = scala agresiv).

Clasificare unghiuri creative: FOMO (bonus limitat, countdown), Social Proof (castigatori locali, testimoniale), Aspirational (lifestyle, lux), Educational (ghid, tutorial, tips), Other (neincadrat).

**Tool**: Google Sheets, Claude Opus (pentru clasificare pattern-uri ambigue)
**Output**: Matrice GEO x Unghi in Google Sheets cu urmatorul format:

| GEO / Angle | FOMO | Social Proof | Aspirational | Educational | Other | TOTAL |
|-------------|------|--------------|--------------|-------------|-------|-------|
| Peru        | 12   | 8            | 3            | 5           | 2     | 30    |
| Kenya       | 6    | 15           | 1            | 9           | 3     | 34    |
| Chile       | 9    | 7            | 4            | 6           | 1     | 27    |
| TOTAL       | 27   | 30           | 8            | 20          | 6     | 91    |

Valoare celula = numar reclame active care folosesc acel unghi. Sub matrice: sectiune "Top Operator per GEO" — top 3 operatori per GEO rankati dupa numar total reclame active.

**Checkpoint**: Matrice completata cu valori numerice in toate celulele. Nicio celula goala. Total per GEO >= 10.

### Pas 4: COMPETITOR LP AUDIT
Auditeaza top 3 landing pages ale celor mai activi afiliati per GEO (identificati in Pas 3).

Per landing page evalueaza: headline principal, bonus offer (suma + conditii), tip social proof (reviews/testimoniale/numar utilizatori), payment methods afisate, disclaimer prezent (da/nu + continut), CTA (text + plasare), mobile speed score (PageSpeed Insights), numar scroll-uri pana la CTA.

**Tool**: Browser, PageSpeed Insights, Google Sheets
**Output**: Tabel audit comparativ per GEO cu scoring 1-5 per element si coloana "De copiat/imbunatatit" cu elemente concrete actionale.
**Checkpoint**: Minim 3 LP auditate per GEO activ. Fiecare LP cu toate 8 elementele evaluate.

### Pas 5: KEYWORD + SEO COMPETITIVE SNAPSHOT
Verifica pozitii saptamanale pentru top 5 keywords gambling per GEO:
- Peru: "casino online Peru", "mejores casinos online", "tragamonedas online", "bonos casino Peru", "apuestas deportivas Peru"
- Kenya: "best betting sites Kenya", "casino online Kenya", "sports betting Kenya", "Mpesa casino", "free spins Kenya"
- Chile: "casino online Chile", "mejores casinos Chile", "bonos casino", "apuestas deportivas Chile", "tragamonedas gratis"

Per keyword documenteaza: pozitii top 10, tip continut (review/listicle/guide/LP), DR/DA competitor, featured snippets existente, schimbari vs saptamana anterioara.

**Tool**: Ahrefs/SEMrush, Google Search
**Output**: Spreadsheet "SEO Competitive Snapshot" cu coloane: GEO, Keyword, Pozitie 1-10 (site + URL), Tip Continut, DR/DA, Featured Snippet (da/nu), Change vs Last Week (+/-/new/lost). Sectiune separata: "Keyword Gap Opportunities" — keywords unde niciun afiliat nu rankează sau unde exista pozitii slabe (pos 4+) atacabile.
**Checkpoint**: Minim 5 keywords per GEO documentate. Change vs Last Week completat (prima saptamana = "baseline").

### Pas 6: SYNTHESIZE INTELLIGENCE BRIEF
Sintetizeaza toate datele din Pasii 2-5 intr-un brief executabil de maximum 1 pagina.

**Tool**: Claude Opus
**Output**: Document "Weekly Competitive Intelligence Brief" cu urmatorul template obligatoriu:

**HEADER**: Saptamana [data], GEO-uri acoperite: [lista]

**Sectiunea 1 — Top 3 Oportunitati Imediate**
Per oportunitate: descriere in 1-2 propozitii, GEO, canal recomandat, potential estimat (LOW/MEDIUM/HIGH), sursa datelor (pas + reclama/keyword specific).

**Sectiunea 2 — Creative Angles de Testat**
Top 3 unghiuri identificate din matricea Pas 3 cu exemple concrete de hook-uri si copy observate, adaptate per GEO.

**Sectiunea 3 — New Competitor Alerts**
Operatori sau afiliati noi detectati (primele aparitii in Ad Library), volum estimat, GEO-uri vizate, nivel amenintare (LOW/MEDIUM/HIGH).

**Sectiunea 4 — Keyword Gaps**
Top 5 keyword gaps actionale din Pas 5, cu pozitia curenta, dificultatea estimata, si tipul de continut recomandat pentru attack.

**Sectiunea 5 — Action Items**
Tabel cu actiuni concrete:

| Action | Owner | Deadline | Priority | Source Pas |
|--------|-------|----------|----------|------------|

Minim 3 action items. Fiecare cu owner explicit si deadline concret (data calendaristica).

**Checkpoint**: Brief complet = toate 5 sectiunile prezente, minim 3 action items, livrat pana miercuri 12:00 aceleiasi saptamani.

### Pas 7: FEED INTO ACTIVE CAMPAIGNS
Translateaza action items din brief (Pas 6) in actiuni concrete in procedurile existente:
- Unghi castigator detectat → creare varianta creativa noua (M-116 Pas 3)
- Competitor nou pe keyword → update content plan (M-113)
- Oferta noua cu volum mare → evaluare prin M-115 → M-114
- LP superioara detectata → initiere A/B test pe elementul superior (M-118)
- Keyword gap identificat → prioritizare in M-119 content calendar

**Tool**: Google Sheets (task tracker), proceduri M-113/M-114/M-115/M-116/M-118/M-119
**Output**: Task-uri concrete adaugate in task tracker cu: actiune, procedura referinta, prioritate (HIGH/MEDIUM/LOW), owner, deadline, status (OPEN/IN PROGRESS/DONE).
**Checkpoint**: 100% action items din Pas 6 translatese in task-uri. Zero action items fara owner sau deadline.

## 3. Cortex Logging

```json
{
  "collection": "procedures",
  "document": {
    "title": "M-117 Competitive Intelligence Automation",
    "content": "{weekly brief + matrices output}",
    "metadata": {
      "type": "procedure",
      "procedure": "M-117",
      "rule_id": "TRAIN-S-001",
      "domain": "affiliate",
      "sub_domain": "competitive-intelligence",
      "pipeline": "recurring-weekly",
      "version": "2.0",
      "status": "ACTIVE",
      "tags": ["affiliate", "competitive-intelligence", "gambling", "monitoring", "LATAM", "Africa", "training"]
    }
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- Post-setup: dupa configurare monitoring stack per GEO (Pas 1)
- Weekly cycle: luni dimineata (Pas 2-5), brief livrat pana miercuri (Pas 6), task-uri distribuite pana joi (Pas 7)

### WHEN
- **Cadenta**: in fiecare luni, incepand la 09:00, finalizare scrape + analiza pana la 12:00
- **Brief delivery**: miercuri 12:00 (hard deadline)
- **Task distribution**: joi 12:00
- **Re-setup**: la adaugare GEO nou in portofoliu (ad-hoc, Pas 1 doar)
- **Revalidare stack**: prima luni din luna (audit monitoring stack completeness)

### HOW (violation detection)
- Brief absent sau livrat dupa miercuri 18:00 → **VIOLATION**
- Scrape incomplet (<10 reclame per GEO) → **VIOLATION**
- Matrice GEO x Unghi cu celule goale sau lipsa → **VIOLATION**
- Insights fara action items translatese in task-uri (>48h) → **WARNING**
- Monitoring stack incomplet (<4 instrumente per GEO) → **VIOLATION**
- Action items fara owner sau deadline → **VIOLATION**
- SEO snapshot fara "Change vs Last Week" → **WARNING**

### CONNECT
- M-007 (Competitor Analysis manual) — M-117 automatizeaza si extinde monitorizarea din M-007
- M-009 (Creative Brief) — consume insights creative angles din Pas 3
- M-113 (Content SEO Strategy) — primeste keyword gaps din Pas 5
- M-114 (Geo Scoring Framework) — re-scoring trigger la detectie piata noua
- M-115 (Offer Discovery per GEO) — trigger la detectie oferta/operator nou
- M-116 (Creative Production) — primeste unghiuri castigatoare din Pas 3 si Pas 6
- M-118 (Gambling Compliance & Platform Selection) — primeste insights LP din Pas 4, compliance check pe operatori noi detectati
- M-119 (TikTok + Native Ads for Restricted Verticals) — primeste creative angles din Pas 3, keyword priorities din Pas 5
- M-120 (WhatsApp/SMS Funnel Automation) — consuma trend data din brief saptamanal pentru optimizare secvente
- M-121 (Server-Side Tracking & First-Party Data) — compara predictii M-117 cu rezultate reale din tracking S2S

### VERIFY
1. **Completeness check**: Spreadsheet "Weekly Ad Scrape" contine minim 10 randuri per GEO cu toate 11 coloanele completate — verificat luni 12:00
2. **Matrix validity**: Matricea GEO x Unghi are valori numerice in toate celulele, totaluri corecte, si minim 10 reclame clasificate per GEO — verificat marti 12:00
3. **Brief quality**: Brief contine toate 5 sectiunile obligatorii, minim 3 action items cu owner + deadline, livrat pana miercuri 12:00 — verificat miercuri 12:00
4. **Task conversion rate**: 100% action items din brief translatese in task-uri cu owner, deadline, si procedura referinta — verificat joi 12:00
5. **Timeliness**: Zero brief-uri livrate dupa miercuri 18:00 in ultimele 4 saptamani — verificat la audit lunar

### MODEL ROUTING
- **Claude Opus**: sinteza intelligence brief (Pas 6), pattern detection complex (Pas 3), clasificare unghiuri ambigue
- **Claude Sonnet**: formatare date, populare spreadsheet, quick lookups
- **Perplexity Pro**: real-time verification date competitive, keyword positions

## 5. Dependente

- **Tools**: Meta Ad Library, TikTok Creative Center, Google Alerts, SimilarWeb, Ahrefs/SEMrush, PageSpeed Insights, Claude Opus, Google Sheets
- **Proceduri input**: M-007 (baseline competitor data), M-114 (GEO-uri active si scoring)
- **Proceduri output**: M-009, M-113, M-115, M-116, M-118, M-119, M-120, M-121
- **Data sources**: Ad libraries (Meta, TikTok), SEO tools (Ahrefs/SEMrush), traffic intelligence (SimilarWeb)
- **Infrastructure**: Google Sheets template pre-configurat cu coloanele definite in Pas 2, Pas 3, si Pas 5

## 6. Metrics

| Metric | Target | Frecventa |
|--------|--------|-----------|
| Brief delivery rate | 100% weekly, livrat pana miercuri 12:00 | Saptamanal |
| Actionable insights per brief | Minim 3 action items | Saptamanal |
| Competitor detection lag | <72h de la prima aparitie in Ad Library | Lunar (audit) |
| Oportunitati majore ratate | 0 (validat contra actiuni competitor detectate retroactiv) | Lunar |
| Effort per ciclu complet (Pas 2-7) | <3h total | Saptamanal |
| Task conversion rate | 100% action items → task-uri cu owner + deadline | Saptamanal |
| Pattern prediction accuracy | >60% unghiuri recomandate testate cu ROI pozitiv | Trimestrial |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential cu output clar si checkpoint per pas
- [x] Tools specificate per pas
- [x] KPIs masurabile cu target numeric
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging template JSON valid cu metadata block complet
- [x] Dependente listate (tools + proceduri input/output)
- [x] Metrics cu target si frecventa
- [x] Cadenta saptamanala explicita (luni 09:00, brief miercuri 12:00)
- [x] Conexiuni cu proceduri existente validate (M-007, M-009, M-113-M-121)
- [x] Output format concret per pas (spreadsheet coloane, matrice, brief template)
- [x] Descrie CE nu CUM — fara cod inline sau detalii de implementare
