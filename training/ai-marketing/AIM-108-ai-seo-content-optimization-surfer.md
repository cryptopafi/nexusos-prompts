---
type: procedure
created: 2026-03-20
status: active
slug: aim-108-ai-seo-content-optimization-surfer
tags: [procedure, nexus]
---

# AI SEO Content Optimization Workflow (Surfer SEO + AI) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Optimizarea conținutului pentru motoarele de căutare folosind AI + Surfer SEO (sau alternative), de la keyword research și content planning la scrierea optimizată NLP, auditarea paginilor existente, și monitorizarea ranking-ului post-publicare. Include prompt templates per etapă SEO, technical SEO checklist, content cluster strategy, și ROI framework pentru trafic organic.

---

## 1. Problema

Conținut de calitate fără SEO = trafic organic zero. SEO fără conținut de calitate = ranking temporar apoi decline. Combinația AI + Surfer SEO face procesul data-driven: AI scrie conținut natural, Surfer optimizează cu date din SERP real (NLP terms, word count, heading structure, keyword density). Un content marketer cu acest stack produce 3-5 articole SEO-optimizate pe săptămână la calitate comparabilă cu un specialist SEO + copywriter care produce 1-2. Fără proces, conținutul fie este optimizat dar robotic, fie este natural dar invizibil în search.

Situații acoperite:
- Scrierea unui articol nou targetând un keyword specific
- Optimizarea unui articol existent pentru îmbunătățirea ranking-ului (pozițiile 5-20)
- Construirea unui content cluster (pillar page + cluster articles)
- Audit de conținut site-wide pentru identificarea oportunităților SEO
- SEO content as a service (agenție/freelancer)

---

## 2. Procedura

### Pas 1: Research — Keyword research structurat cu strategie de prioritizare

**Pasul 1A — Seed Keywords și Expansiune:**

Prompt pentru **Gemini Pro** (web access):
```
Caută informații SEO actuale pentru industria [industrie/nișă].

1. Listează 20 seed keywords relevante pentru un business care [descriere business]
2. Pentru fiecare seed keyword, sugerează 3-5 long-tail variations
3. Clasifică fiecare pe intenție: informațional / comercial / tranzacțional / navigațional
4. Estimează dificultatea relativă: Low / Medium / High
5. Sugerează 5 „question keywords" din Google PAA pe acest topic
6. Identifică 3 keyword-uri trending (volum în creștere) pe acest topic
```

**Pasul 1B — Validare cu tool SEO:**
Verifică în Surfer SEO Keyword Research, Ahrefs, sau Semrush:
- Volume de căutare lunar (minim 100/mo pentru a justifica efortul)
- Keyword Difficulty score (target: <40 KD pentru site-uri noi, <60 pentru site-uri cu autoritate)
- SERP feature analysis: ce tip de conținut rankează (articole, video, tools, e-com)
- Traffic potential: estimat clicks pe lună dacă ajungi pe poziția 1-3

**Pasul 1C — Prioritizare cu matrice:**

| Keyword | Volume | KD | Intenție | Business relevance | PRIORITY |
|---------|--------|----|---------|--------------------|----------|
| [KW1] | 2,400 | 35 | Info | High | **P1** |
| [KW2] | 800 | 22 | Commercial | High | **P1** |
| [KW3] | 5,000 | 65 | Info | Medium | P2 |
| [KW4] | 300 | 15 | Tranzacțional | High | **P1** |

**Prioritizare formula**: P1 = KD<40 AND business relevance High AND volume>200
Quick wins = articole cu ranking existent pe pozițiile 5-20 (optimizare, nu scriere de la zero)

### Pas 2: Cluster — Planifică content cluster (pillar + satellite articles)

Un articol izolat rankează slab. Un cluster de 5-10 articole interconectate rankează strong.

Prompt pentru **Claude Sonnet**:
```
Creează un content cluster plan pentru keyword-ul pillar „[keyword principal]".

BUSINESS: [ce faci]
AUDIENȚA: [pentru cine]
SITE EXISTENT: [are deja articole pe topic sau e de la zero?]

Generează:
1. PILLAR PAGE (articol comprehensiv, 2,500-4,000 cuvinte):
   - Titlu H1 optimizat
   - 8-12 secțiuni H2 (fiecare poate deveni cluster article)
   - Ce acoperă pillar page vs. ce delegă la cluster articles

2. CLUSTER ARTICLES (8-12 articole satellite, 1,000-2,000 cuvinte fiecare):
   Per articol specifică:
   - Keyword target (long-tail derivat din pillar)
   - Titlu H1 sugerat
   - Intenție de căutare
   - Cum face link CĂTRE pillar page (anchor text sugerat)
   - Cum face link DE LA pillar page (anchor text sugerat)
   - Ordine de publicare recomandată (1-12)

3. INTERNAL LINKING MAP:
   - Pillar → linkează toate cluster articles
   - Cluster article → linkează pillar + 2-3 alte cluster articles relevante
   - Vizualizare: hub-and-spoke diagram

4. PUBLISHING SCHEDULE:
   - Ordinea ideală de publicare (pillar first sau cluster first?)
   - Frecvența recomandată (1-2/săptămână)
   - Timeline total: [X săptămâni]
```

### Pas 3: Surfer — Creează Content Editor cu date SERP reale

**Cu Surfer SEO ($89/mo Essential, $179/mo Scale):**

1. Surfer → Content Editor → inserează keyword-ul principal
2. Configurează: locația target (RO/US/UK/etc.), device (desktop), selectează competitori relevanți
3. Surfer generează automat:
   - **Content Score target**: obiectivul (target ≥70/100)
   - **NLP Terms**: 30-80 termeni semantici cu frecvența recomandată
   - **Word Count**: lungimea optimă bazată pe competitori (ex: 1,800-2,500)
   - **Headings**: nr. recomandat de H2, H3, H4
   - **Images**: nr. recomandat de imagini
   - **FAQ**: întrebări frecvente din SERP (People Also Ask)

4. Exportă NLP terms ca listă — vor fi input pentru AI writer

**Fără Surfer (alternative):**
- **Clearscope** ($170/mo) — mai premium, content grading A-F
- **MarketMuse** ($149/mo) — content planning + optimization
- **Frase.io** ($45/mo) — budget-friendly alternative cu SERP analysis
- **NeuronWriter** ($23/mo) — cel mai ieftin cu NLP optimization
- **Manual SERP Analysis** (free) — analizează top 5 manual (mai lent dar funcționează)

### Pas 4: Outline — Construiește structura bazată pe date SERP + content gap

Prompt pentru **Claude Sonnet** (outline optimizat):
```
Creează un outline SEO-optimizat bazat pe aceste date:

KEYWORD: [keyword principal]
SURFER NLP TERMS (top 20): [lista termenilor cu frecvența recomandată]
WORD COUNT TARGET: [din Surfer]
HEADING COUNT TARGET: [din Surfer]
COMPETITOR ANALYSIS: [ce acoperă top 3 competitori + ce lipsește]
PAA QUESTIONS: [din Surfer sau Google]

Generează:
1. H1 optimizat (include keyword exact sau LSI, 55-65 caractere)
2. Meta description (150-160 caractere, include keyword, CTA implicit)
3. Intro: keyword în primele 100 cuvinte, hook + promise (100-150 cuvinte)
4. [X] secțiuni H2, fiecare cu:
   - Titlul H2 (include NLP term sau secondary keyword unde natural)
   - Sub-secțiuni H3 dacă necesare
   - NLP terms de inclus în această secțiune (din lista Surfer)
   - Word count target per secțiune
   - Tip conținut: paragraf / bullet list / tabel / how-to steps
5. FAQ section (3-5 întrebări din PAA, formatate pentru FAQ schema)
6. Concluzie cu CTA (100-150 cuvinte)

CONTENT GAPS (ce lipsește competitorilor — oportunitatea noastră):
[din Pasul 1/2 research]

Regulă: outline-ul trebuie să fie MAI COMPLET decât orice competitor, nu mai lung
(completitudinea bate lungimea).
```

### Pas 5: Write — Generează conținut secțiune cu secțiune cu NLP terms integrat

**Regula critică**: Generează secțiune cu secțiune. Include NLP terms din Surfer ca context explicit.

Prompt pentru **Claude Sonnet** (SEO writing):
```
Scrie secțiunea „[H2 title]" din articolul SEO despre [topic].

KEYWORD PRINCIPAL: [KW]
NLP TERMS de inclus NATURAL în această secțiune: [term1 (2x), term2 (1x), term3 (1x)]
WORD COUNT target: [X] cuvinte (±50)
AUDIENȚA: [nivel cunoaștere]
TON: [brand voice]
SECȚIUNEA ANTERIOARĂ s-a terminat cu: [ultima propoziție]

CERINȚE:
- Include NLP terms natural (nu forțat, nu bold, nu keyword stuffing)
- Dacă secțiunea răspunde unei întrebări din PAA, începe cu răspuns concis (40-60 cuvinte)
  urmat de elaborare (optimizare featured snippet)
- Include 1 exemplu concret sau statistică relevantă
- Paragrafele: max 3-4 propoziții, readability nivel gimnaziu
- Evită: „în lumea de azi", „este important de menționat", repetarea exactă a keyword-ului
  de mai mult de 2 ori per secțiune
- Încheie cu tranziție naturală către „[secțiune următoare]"
```

### Pas 6: Optimize — Iterează până la scor ≥70 în Surfer

**Workflow de optimizare:**
1. Copy-paste draft-ul complet în Surfer Content Editor
2. Verifică scorul inițial (tipic: 50-65 la prima iterare)
3. Identifică NLP terms sub-reprezentați (Surfer marchează roșu)
4. Nu forța keywords — rescrie propoziții natural pentru a include termenul

Prompt pentru adăugarea NLP terms:
```
Următoarele NLP terms trebuie adăugate natural în articolul meu despre [topic]:
[term1] — trebuie inclus de [X] ori mai (actual: [Y] ori)
[term2] — trebuie inclus de [X] ori (actual: 0)
[term3] — trebuie inclus de [X] ori (actual: 0)

Sugerează propoziții naturale care includ acești termeni,
pe care le pot insera în secțiunile cele mai relevante.
Nu forța — dacă un termen nu se potrivește natural, spune-mi.
```

5. **Target scores**:
   - 70-75: Bun — publicabil
   - 75-85: Foarte bun — optimal
   - 85-90: Excelent — dar verifică să nu fie keyword stuffing
   - 90+: Suspiciously high — probabil over-optimized, poate penaliza

### Pas 7: Technical SEO — Checklist complet pre-publicare

```markdown
## ON-PAGE SEO CHECKLIST

### Title & Meta
- [ ] Title tag: 50-60 caractere, include keyword, unic pe site
- [ ] Meta description: 150-160 caractere, include keyword, CTA implicit
- [ ] H1: unic pe pagină, include keyword, diferit de title tag dacă necesar
- [ ] URL slug: scurt (3-5 cuvinte), include keyword, fără stop words, lowercase, hyphens

### Heading Structure
- [ ] Un singur H1 pe pagină
- [ ] H2 pentru secțiuni principale (include NLP terms)
- [ ] H3 pentru sub-secțiuni (nu skip H2→H4)
- [ ] Heading hierarchy logică (nu decorativă)

### Content Signals
- [ ] Keyword în primele 100 cuvinte
- [ ] Keyword density: 0.5-1.5% (natural, nu forțat)
- [ ] Internal links: 3-5 spre pagini relevante de pe site
- [ ] External links: 2-3 spre surse autoritare (studii, publicații)
- [ ] Alt text pe TOATE imaginile (descriptiv, include keyword unde natural)

### Schema Markup
- [ ] Article schema (type: Article, headline, author, datePublished)
- [ ] FAQ schema (dacă ai secțiune FAQ — apare ca rich result)
- [ ] HowTo schema (dacă e articol how-to — apare cu steps în SERP)
- [ ] Breadcrumb schema (navigare ierarhică)

### Featured Snippet Optimization
- [ ] Paragraf concis (40-60 cuvinte) imediat după un H2 formulat ca întrebare
- [ ] Tabel HTML dacă keyword-ul afișează table snippet
- [ ] Numbered list dacă keyword-ul afișează list snippet
- [ ] Definition format: „[Keyword] este [definiție concisă în 1-2 propoziții]."

### Technical
- [ ] Page speed: <3s load time (comprimă imagini, lazy load below-fold)
- [ ] Mobile responsive: verificat pe multiple device widths
- [ ] Canonical URL setat (evită duplicate content)
- [ ] Open Graph tags: og:title, og:description, og:image (pentru social sharing)
- [ ] Hreflang tags (dacă conținut multilingv)
- [ ] Robots.txt: pagina nu e blocată
- [ ] XML sitemap: pagina inclusă
```

### Pas 8: Publish & Index — Asigură indexare rapidă

**Post-publicare (primele 24 ore):**
1. **Google Search Console**: URL Inspection → Request Indexing
2. **Internal linking**: adaugă link din 3-5 pagini existente cu autoritate → noua pagină
3. **Social amplification**: publică pe LinkedIn, Twitter, Facebook (engagement signals timpurii contează)
4. **Email**: trimite în newsletter (trafic direct + engagement = semnal pozitiv)
5. **IndexNow** (opțional): ping Bing instant via IndexNow API

**Primele 48 ore contează**: traficul, engagement-ul (time on page, scroll depth) și click-urile din SERP în primele zile influențează ranking-ul inițial.

### Pas 9: Monitor — Tracking ranking și iterare post-publicare

**Dashboard de monitorizare (Google Sheet + Search Console):**

| Articol | Keyword | Position (D1) | Position (D30) | Position (D60) | CTR | Clicks/mo |
|---------|---------|---------------|-----------------|-----------------|-----|-----------|
| [Articol 1] | [KW] | 45 | 18 | 9 | 3.2% | 240 |

**Acțiuni pe baza datelor (la 30 și 60 zile):**

- **Poziția 1-3**: Maintain. Monitorizează pentru decline.
- **Poziția 4-10**: Optimize meta title/description pentru CTR. Adaugă content freshness (update date, new sections). Construiește 2-3 backlinks quality.
- **Poziția 11-20**: Content gap analysis — ce are competitorul pe poz 1-3 și tu nu? Extinde articolul cu 300-500 cuvinte pe secțiuni lipsă. Improve internal linking.
- **Poziția 20+**: Re-evaluate keyword choice. Content quality issue sau authority issue?
- **CTR sub 2%**: Title tag slab — rescrie cu mai mult hook. Meta description slab — rescrie cu CTA.

Prompt pentru **Claude Sonnet** (SEO analysis):
```
Analizează performanța SEO a acestor [X] articole la 60 zile post-publicare:

[tabel cu date — keyword, position, CTR, clicks, bounce rate, time on page]

Identifică:
1. Top 3 performeri: ce au în comun (structură, lungime, topic type)?
2. Underperformeri (poz 20+): ce le lipsește vs. top 3?
3. Quick wins: articole pe poz 5-15 care pot ajunge top 3 cu optimizare minoră — ce exact?
4. CTR problems: articole cu CTR sub 2% — recomandare title/meta rescrise
5. Content freshness: care articole beneficiază de update (>6 luni de la publicare)?
6. Cluster gaps: ce articole cluster lipsesc pentru a consolida topicul?
```

### Pas 10: Content Audit — Auditarea și optimizarea conținutului existent

**Prompt pentru audit site-wide:**
```
Am [X] articole pe site-ul meu. Aici sunt datele din Google Search Console (ultimele 90 zile):

[Export: URL, impressions, clicks, CTR, average position]

Clasifică fiecare articol în:
1. SCALE (poz 1-3, CTR bun): investește în backlinks, nu modifica conținut
2. OPTIMIZE (poz 4-20): re-optimize cu Surfer, improve title, adaugă content
3. CONSOLIDATE (duplicate/canibalizare): 2+ articole pe keyword similar → merge într-unul
4. PRUNE (0 traffic, 0 impressions, >12 luni): redirect 301 sau noindex
5. REPURPOSE (high impressions, low clicks): vizibilitate dar CTR slab → rescrie title/meta

Prioritizare: ce 5 articole, dacă le optimizez, ar genera cel mai mare impact de trafic?
```

---

## 3. Cortex Logging

```json
{
  "text": "SEO Content Optimization finalizat. Keyword: [X]. Scor Surfer: [Y/100]. Lungime: [Z cuvinte]. Cluster: [pillar + N cluster articles]. Position at D30: [pos]. CTR: [%]. Search Console submitted: [da/nu].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-108",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["seo", "content-optimization", "surfer-seo", "keyword-research", "organic-traffic", "content-cluster", "technical-seo"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice articol sau pagina web destinată traficului organic

### WHEN
La scrierea, optimizarea sau auditarea oricărei piese de conținut SEO

### HOW (violation detection)
- Conținut publicat fără optimization tool (Surfer/Clearscope/manual NLP) → violation
- Scor Surfer sub 70 la publicare → violation
- Title tag sau meta description lipsă/depășește limitele → violation
- Search Console submit sărit → violation
- Monitoring la 30 zile nesetat → violation
- Articol fără internal links → violation
- Content cluster fără internal linking map → violation
- Schema markup absent (article, FAQ, breadcrumb) → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-103 → content pipeline (SEO ca pas integrat în pipeline)
- AIM-110 → competitor analysis (sursa de keywords și content gaps)
- AIM-111 → reporting (SEO metrics în dashboard)

### VERIFY
- [ ] Keyword research complet (volume, KD, intenție, business relevance)?
- [ ] Content cluster planificat (pillar + satellite + linking map)?
- [ ] Content Editor Surfer creat (sau alternativă)?
- [ ] Outline bazat pe SERP data și NLP terms?
- [ ] Scrie secțiune cu secțiune (nu integral)?
- [ ] Scor Surfer ≥70 la publicare?
- [ ] Technical SEO checklist complet?
- [ ] Schema markup implementat?
- [ ] Search Console submit realizat?
- [ ] Monitoring la 30 și 60 zile setat?

**VK-uri:**
1. `✅ [PROC] AIM-108 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI SEO Content Optimization Workflow" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Keyword research live | Gemini Pro | Web access nativ, date actuale |
| Content cluster planning | Claude Sonnet | Structurare logică, coerență tematică |
| Outline cu NLP terms | Claude Sonnet | Integrare naturală, nu keyword stuffing |
| Scriere content SEO | Claude Sonnet | Long-form quality, readability |
| NLP term insertion | Claude Sonnet | Natural integration, nu forțat |
| Technical SEO audit | Claude Sonnet | Checklist rigoros, nu omite |
| Performance analysis | Claude Sonnet | Structured analysis, recomandări |
| Quick SERP checks | DeepSeek | Cost-efficient, task simplu |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Surfer SEO ($89/mo) sau alternativă | Content scoring, NLP terms, SERP analysis |
| Claude Sonnet / ChatGPT | Generare conținut optimizat |
| Gemini Pro | Keyword research live |
| Google Search Console (free) | Indexare, monitoring, CTR |
| Ahrefs ($99/mo) / Semrush ($130/mo) | Keyword research, backlink analysis |
| CMS (WordPress/Webflow/Ghost) | Publicare și technical SEO |

---

## 6. Instrumente

- Surfer SEO ($89/mo Essential) — content editor, NLP optimization, SERP-based scoring
- Claude Pro ($20/mo) — SEO writing, outline, optimization
- Gemini Pro (free/$20/mo) — keyword research, SERP analysis live
- Ahrefs ($99/mo Lite) — keyword research, backlinks, competitor analysis
- Semrush ($130/mo Pro) — all-in-one SEO, position tracking
- Google Search Console (free) — indexare, monitoring ranking, CTR
- Screaming Frog (free up to 500 URLs) — technical SEO audit crawl
- Frase.io ($45/mo) — budget alternative la Surfer
- NeuronWriter ($23/mo) — cheapest NLP optimization tool
- Rank Math / Yoast (free/$99/yr) — WordPress on-page SEO plugin

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Scor Surfer SEO la publicare | ≥ 70/100 |
| Ranking la 60 zile (nou articol) | Top 20 |
| Ranking la 120 zile (nou articol) | Top 10 |
| CTR din Search Console | ≥ 3% |
| Timp scriere+optimizare per articol | ≤ 4 ore |
| Internal links per articol | ≥ 3 |
| Schema markup implementat | 100% |
| Content audit frequency | Trimestrial |
| Cluster articles per pillar | ≥ 5 |
| Cost per articol SEO (AI + tools prorated) | ≤ $15 |
