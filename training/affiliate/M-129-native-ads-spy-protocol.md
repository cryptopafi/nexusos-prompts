---
type: procedure
created: 2026-03-20
status: active
slug: m-129-native-ads-spy-protocol
tags: [procedure, nexus]
---

# M-129 — Native Ads Competitive Intelligence Protocol

**Domeniu**: affiliate | **Sub-domeniu**: native-ads-intelligence
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Cadenta**: Bi-weekly (în fiecare luni, sincronizat cu ciclul M-117)

---

## §1. PROBLEMA

### De ce există acest gap

M-117 acoperă comprehensiv spyul pe Meta Ads și TikTok, dar nu conține **zero protocol specific pentru native advertising**. Acest lucru creează o vulnerabilitate operațională critică pentru că:

**1. Tool-uri diferite, metodologie diferită**
Native ads nu apar în Meta Ad Library. Taboola Trends, AdBeat și Anstrex Native sunt platformele corecte de spy pentru acest canal — iar niciunul nu este menționat sau configurat în M-117.

**2. Ciclu creativ mai lung = fereastră de oportunitate mai mare**
Ciclul creativ pentru native este 45-60 zile vs 30-45 zile social (benchmark M-119). Un competitor poate rula profitabil pe native 2+ luni nedetectat fără M-129. Fiecare ciclu nedetectat = date de optimizare gratuite pe care competitorul le-a plătit și noi le ratăm complet.

**3. Pre-sell pages: format complet diferit față de creativele social**
Native ads direcționează trafic către pagini editorial-style (articole, listicle, review-uri) înainte de pagina operatorului. Spyul social analizează video-uri și bannere. Metodologia de audit a pre-sell pages necesită o procedură separată — structura narativă, plasarea CTA, arcul poveste nu sunt relevante pentru creative TikTok.

**4. Cost de intrare ridicat fără intelligence**
Fără M-129, un GEO nou pe native ads necesită $300-500 în teste pentru a identifica formatele câștigătoare (headline formulas, thumbnail patterns, pre-sell structure). Cu M-129 bine executat, acele formate sunt disponibile gratuit din analiza competitorilor activi.

**5. Network acceptance intelligence**
Unele rețele native sunt mai stricte decât altele în verticala gambling. Fără M-129, alegerea rețelei pentru un GEO nou este arbitrară. Intelligence-ul de network permite intrarea directă pe rețeaua cu cel mai permisiv acceptance policy per GEO.

**Costul inacțiunii**: $300-500 per GEO per ciclu în teste evitabile + 45-60 zile de date de optimizare pierdute față de competitors activi pe native.

---

## §2. PROCEDURA

### Pas 1: TOOL SETUP & SEED COMPETITOR IDENTIFICATION

**Obiectiv**: Configurarea tool-urilor de spy și identificarea a 5-10 campanii concurente active per GEO (PE/KE/CL).

#### Tool stack per GEO

| Tool | Cost | Rol principal | GEO coverage |
|------|------|---------------|--------------|
| Taboola Trends | Gratuit | CTR estimates + top ads | Global (incl. PE/KE/CL) |
| AdBeat | $249/lună | Creative + LP + spend estimates | Global |
| Anstrex Native | $69/lună | MGID/Outbrain specialist | Global |
| SimilarWeb | Freemium | Referral traffic → LP identification | Global |

**Nota buget**: Anstrex Native este alternativa cost-eficientă la AdBeat pentru operațiuni cu buget limitat. La $69/lună vs $249/lună, Anstrex oferă acoperire excelentă MGID și Outbrain — rețelele relevante pentru Kenya ($0.02-0.03 CPC) și Peru ($0.03-0.04 CPC).

#### Workflow identificare competitori

**Taboola Trends (gratuit, rulat primul)**:
1. Accesează trends.taboola.com
2. Filtrează: Country = PE sau KE sau CL | Category = Gambling/Betting/Casino | Date range = Last 30 days
3. Screenshot top 10 ads per GEO (ordonate după CTR estimate)
4. Extrage: Advertiser name, headline, thumbnail URL, estimated CTR, first seen date

**AdBeat / Anstrex Native**:
1. Filtrează: Network = Taboola + Outbrain + MGID | GEO = target | Vertical = Gambling/Casino
2. Sortează după: Estimated spend (descrescător) sau Days active (descrescător)
3. Identifică top 10 advertiseri unici per GEO
4. Pentru fiecare: click pe campanie → extrage LP URL + creative set complet

**SimilarWeb (cross-validare)**:
1. Introdu URL-ul unui operator de gambling cunoscut din GEO target (ex: betsson.pe, 1xbet.co.ke)
2. Referral traffic tab → filtrează după surse: taboola.com, outbrain.com, mgid.com
3. Dacă trafic semnificativ din aceste surse → operator are campanii native active → adaugă la lista de monitorizat

**Output**: `native-spy-setup-{GEO}.md`

Format tabel output:

| Advertiser | Network | GEO | Ad Format | Est. Spend/mo | First Seen | LP URL | Notes |
|------------|---------|-----|-----------|----------------|------------|--------|-------|
| [Advertiser 1] | MGID | PE | Advertorial | ~$2,400 | 2026-01-15 | [URL] | Active 50+ days |
| [Advertiser 2] | Taboola | KE | Listicle | ~$800 | 2026-02-01 | [URL] | New entrant |

**Checkpoint Pas 1**:
- [ ] Minimum 5 campanii identificate per GEO (minim 15 total pentru PE+KE+CL)
- [ ] Toate 3 tool-uri confirmate operaționale (login activ, filtre salvate)
- [ ] SimilarWeb cross-validare rulată pentru minim 3 operatori per GEO
- [ ] Output `native-spy-setup-{GEO}.md` creat pentru fiecare GEO activ

---

### Pas 2: HEADLINE FORMULA EXTRACTION

**Obiectiv**: Colectarea și clasificarea headline-urilor native din toate campaniile identificate — minimum 20 headline-uri per GEO, 100% clasificate după formula.

#### Formulele principale native gambling (LATAM + Africa)

Native headlines urmează formule predictibile cu CTR ridicat. Clasificare:

| Formula | Pattern | Exemplu Peru | Exemplu Kenya |
|---------|---------|-------------|---------------|
| **Curiosity Gap** | "Oamenii din [GEO] nu știu asta despre..." | "Los peruanos no saben esto sobre casinos online" | "Kenyan bettors discovered this trick" |
| **Number-Led** | "[Număr] [superlativ] din [GEO]" | "5 casinos en Peru que pagan más rápido" | "3 betting sites with instant Mpesa withdrawal" |
| **Warning/Alert** | "Înainte să [acțiune], citește asta" | "Antes de apostar en Chile, lee esto" | "Warning: avoid these betting sites Kenya 2026" |
| **Social Proof Story** | "Cum a câștigat [Prenume local] [sumă]" | "Cómo ganó Juan 2,400 PEN con solo 100 PEN" | "How Moses won KES 45,000 from KES 500" |
| **How-To** | "Cum să [obiectiv] în [GEO]" | "Cómo ganar en apuestas deportivas Perú" | "How to bet on football in Kenya and win" |
| **Exclusive/Insider** | "Secretul pe care [autoritate] nu îl spune" | "Lo que los casinos no quieren que sepas" | "What betting sites don't tell you" |
| **Time-Limited** | "Ofertă specială [perioadă] pentru [GEO]" | "Oferta especial enero Peru: bono sin depósito" | "New player bonus Kenya - limited time" |

#### Workflow extracție

1. Pentru fiecare campanie din `native-spy-setup-{GEO}.md`:
   - Extrage TOATE headline-urile active (un advertiser poate testa multiple variante simultan)
   - Notează CTR estimate din Taboola Trends unde disponibil
   - Notează numărul de zile activ (proxy pentru profitabilitate — dacă rulează 30+ zile, este probabil profitabil)

2. Clasifică fiecare headline în una din cele 7 formule de mai sus

3. Identifică top 5 după CTR estimate sau după days active (dacă CTR nu e disponibil)

4. Notează adaptări locale specifice: prenume locale folosite, sume în valuta locală (PEN/KES/CLP), referințe culturale (Mpesa pentru Kenya, sport local)

**Output**: `headline-database-{GEO}.md`

Format tabel:

| Headline | Formula | CTR Est. | Network | GEO | Days Active | Volume | Notă |
|----------|---------|---------|---------|-----|-------------|--------|------|
| "Los peruanos no saben..." | Curiosity Gap | 0.12% | Taboola | PE | 48 | High | Winner |
| "3 sitios betting Mpesa..." | Number-Led | 0.09% | MGID | KE | 32 | Medium | Active |

**Checkpoint Pas 2**:
- [ ] Minimum 20 headline-uri colectate per GEO
- [ ] 100% din headline-uri clasificate după formula type
- [ ] Top 5 headline-uri per GEO identificate (după CTR sau days active)
- [ ] Adaptări locale documentate (prenume, valute, referințe culturale)
- [ ] Output `headline-database-{GEO}.md` complet pentru fiecare GEO

---

### Pas 3: PRE-SELL PAGE AUDIT

**Obiectiv**: Auditarea detaliată a top 5 pre-sell pages (pagina intermediară între reclama nativă și operatorul final) per GEO.

#### Elementele de auditat per pagină (scor 1-5)

| Element | Ce analizezi | Scor 1 | Scor 5 |
|---------|-------------|--------|--------|
| **Format** | editorial / listicle / review / direct LP | Direct LP fără narațiune | Editorial lung, storytelling complet |
| **H1 Headline** | Oglindește ad headline sau îl expandează? | Mismatch total vs ad | Continuare naturală a promisiunii din ad |
| **Story Arc** | Problemă → soluție → social proof → CTA | Lipsă structură | Arc complet, tranziții fluente |
| **Social Proof Type** | Numere ("500k utilizatori"), testimoniale, expert opinion, screenshots câștiguri | Zero social proof | 3+ tipuri combinate, cu dovezi vizuale |
| **CTA Placement** | Above fold, mid-content, below fold — câte CTA-uri total? | Singur CTA, below fold | 3+ CTA-uri strategice, above fold + repetat |
| **Disclosure** | "Advertorial" / "Sponsored" label prezent? | Lipsă complet | Label clar conform regulator GEO |
| **Mobile Speed** | Imagini optimizate, CTA button accesibil pe mobile | >5s load, CTA mic | <2s load, CTA mare și proeminent |
| **Offer Clarity** | Bonus type, operator, payment methods vizibile | Vag, fără detalii | Bonus amount + T&C + payment methods explicit |

#### Workflow audit

1. Deschide URL-ul LP din `native-spy-setup-{GEO}.md` pentru top 5 campanii per GEO
2. Verifică pe mobile (DevTools responsive mode sau telefon fizic) — native traffic este majoritar mobile
3. Completează tabelul de audit pentru fiecare pagină
4. Coloana "Copy/Improve": notează dacă elementul merită replicat direct (Copy) sau îmbunătățit (Improve) sau evitat (Avoid)
5. Capturează screenshot complet al paginii (tool: GoFullPage extension sau similar)

**Output**: `presell-audit-{GEO}.md`

Format tabel:

| Pagină | Format | H1 | Story Arc | Social Proof | CTA | Disclosure | Mobile | Offer | Total /40 | Acțiune |
|--------|--------|----|-----------|-----------|----|-----------|--------|-------|---------|---------|
| [URL1] | 4 | 5 | 4 | 3 | 4 | 2 | 3 | 4 | 29/40 | Copy structure |
| [URL2] | 3 | 3 | 3 | 5 | 3 | 4 | 4 | 3 | 28/40 | Copy social proof |

**Checkpoint Pas 3**:
- [ ] Minimum 3 pre-sell pages auditate per GEO (target 5)
- [ ] Toate 8 elemente evaluate pentru fiecare pagină (zero câmpuri goale)
- [ ] Screenshot complet salvat per pagină auditată
- [ ] Coloana "Acțiune" completată (Copy / Improve / Avoid) per element
- [ ] Output `presell-audit-{GEO}.md` finalizat

---

### Pas 4: THUMBNAIL PATTERN ANALYSIS

**Obiectiv**: Clasificarea vizuală a thumbnail-urilor native colectate și identificarea top 3 formule cu CTR corelat, gata pentru generare Midjourney.

#### Taxonomia de clasificare thumbnail

**Dimensiunea 1 — Person Type**:
- Celebrity locală (fotbalist cunoscut, vedetă locală)
- Persoană fericită generică (stock photo)
- Jucător sportiv (jersey recognoscibil)
- Persoană surprinsă/șocată (reacție emoțională)
- Fără persoană (produs / simbol monetar)

**Dimensiunea 2 — Color Dominance**:
- Roșu/portocaliu (urgență, acțiune)
- Verde (bani, câștig)
- Albastru (trust, stabilitate)
- Culoarea brandului operatorului
- Gradient mixt

**Dimensiunea 3 — Text Overlay**:
- Suma bonusului ("BONUS 500 PEN")
- "GRATIS" / "FREE" / "FREE BET"
- Săgeată direcțională
- Timer urgență
- Fără text overlay

**Dimensiunea 4 — Image Context**:
- Screenshot telefon cu câștiguri
- Stadion / meci sportiv
- Casino chips / cărți de joc
- Stivă de bani / bancnote
- Logo operator

#### Workflow analiză

1. Descarcă sau screenshot toate thumbnail-urile din campaniile identificate (minim 15 per GEO)
2. Clasifică fiecare thumbnail pe cele 4 dimensiuni
3. Calculează frecvența per categorie
4. Corelează cu CTR estimate din Taboola Trends unde disponibil
5. Identifică top 3 combinații de pattern (ex: "Persoană fericită + Verde + Sumă bonus + Screenshot telefon")
6. Pentru fiecare din top 3: scrie prompt Midjourney ready-to-use

**Exemplu Midjourney prompt din pattern câștigător**:
"Happy young [Peruvian/Kenyan] man holding phone showing green numbers winning screen, casino app interface, green money colors, soft background, professional photo, realistic, 16:9 ratio, bright natural lighting"

**Output**: `thumbnail-patterns-{GEO}.md`

Format:

| Pattern | Person Type | Color | Text Overlay | Context | Frecvență | CTR Est. | Midjourney Prompt |
|---------|------------|-------|-------------|---------|-----------|---------|-------------------|
| Winner-1 | Fericit | Verde | Sumă | Screenshot | 8/15 (53%) | 0.11% | [prompt complet] |
| Winner-2 | Surprins | Roșu | FREE | Stadion | 5/15 (33%) | 0.09% | [prompt complet] |

**Checkpoint Pas 4**:
- [ ] Minimum 15 thumbnail-uri analizate per GEO
- [ ] 100% clasificate pe toate 4 dimensiuni
- [ ] Top 3 pattern-uri identificate cu frecvență calculată
- [ ] CTR corelat notat unde disponibil
- [ ] 3 Midjourney prompts gata de utilizare per GEO
- [ ] Output `thumbnail-patterns-{GEO}.md` complet

---

### Pas 5: NETWORK PERFORMANCE INTELLIGENCE

**Obiectiv**: Per GEO, identificarea rețelei native dominante în verticala gambling și recomandarea de intrare.

#### Metrici de analizat per rețea (Taboola / Outbrain / MGID)

| Metric | Cum se obține | Semnificație |
|--------|-------------|-------------|
| Campanii active | Count din AdBeat/Anstrex | Volum de activitate |
| Estimated spend | AdBeat spend data | Unde alocă bugetul competitorii |
| Top advertiseri | Ranking din tool | Cine domină și pe ce rețea |
| Gambling policy | Test cont sau research forum | Strict / Moderate / Lenient |
| CPC benchmark | M-119 + validare din tool | Cost per click estimat |

#### Gambling policy classification

- **Strict**: Necesită pre-approval, documente de licență, restricții de creative (ex: fără bonus claims în headline)
- **Moderate**: Necesită verificare operator, unele restricții creative dar acceptă gambling cu licență GEO
- **Lenient**: Gambling acceptat cu documente de bază, mai puțin restrictiv pe creative

Sursele pentru policy intelligence:
- Forum affiliate: affpaying.com, affiliate guard dog
- Experiența directă din M-119 (dacă există)
- Contactare account manager rețea

**Output**: `network-intel-{GEO}.md`

Format tabel:

| Rețea | Campanii Active | Est. Spend/mo | Top Advertiser | Gambling Policy | CPC Benchmark | Recomandare |
|-------|----------------|---------------|----------------|----------------|---------------|-------------|
| MGID | 12 | ~$15,000 | Operator X | Lenient | $0.02-0.03 | PRIORITATE 1 |
| Taboola | 7 | ~$8,000 | Operator Y | Moderate | $0.08-0.12 | PRIORITATE 2 |
| Outbrain | 3 | ~$2,000 | Operator Z | Strict | $0.10-0.15 | EVITA pe termen scurt |

**Checkpoint Pas 5**:
- [ ] Toate 3 rețele (Taboola/Outbrain/MGID) analizate per GEO
- [ ] Gambling policy documentată pentru fiecare rețea per GEO
- [ ] Spend distribution estimată (Taboola vs Outbrain vs MGID split procentual)
- [ ] Identificare dacă există rețele active non-M-119 stack (oportunitate neexploatată)
- [ ] Recomandare de rețea per GEO documentată cu justificare
- [ ] Output `network-intel-{GEO}.md` complet

---

### Pas 6: NATIVE INTELLIGENCE BRIEF

**Obiectiv**: Sintetizarea Pas 1-5 într-un brief bi-weekly de 1 pagină, acționabil direct în M-119 și M-117.

#### Structura briefului

**Header**:
- GEO: [PE/KE/CL]
- Perioadă analizată: [dată start] — [dată end]
- Ciclu: #[număr] | Data livrare: [luni]

**Secțiunea 1: Top 3 Headline Formulas de Testat**
(cu exemple adaptate pentru brandul nostru + GEO specific)

| Rank | Formula | Exemplu Adaptat | CTR Est. Competitor | Rețea Recomandată |
|------|---------|----------------|--------------------|--------------------|
| 1 | Curiosity Gap | "[Titlu adaptat brand]" | 0.12% | MGID |
| 2 | Number-Led | "[Titlu adaptat brand]" | 0.09% | MGID |
| 3 | Warning/Alert | "[Titlu adaptat brand]" | 0.08% | Taboola |

**Secțiunea 2: Top 2 Pre-Sell Page Formats de Replicat**
- Format 1: [Tip] — [descriere arc narativ + elemente cheie] — [URL referință]
- Format 2: [Tip] — [descriere] — [URL referință]

**Secțiunea 3: Top 3 Thumbnail Concepts (gata pentru Midjourney)**
- Concept 1: [Midjourney prompt complet]
- Concept 2: [Midjourney prompt complet]
- Concept 3: [Midjourney prompt complet]

**Secțiunea 4: Network Recommendation**
- GEO Peru: [Rețea] — motivul
- GEO Kenya: [Rețea] — motivul
- GEO Chile: [Rețea] — motivul

**Secțiunea 5: New Competitor Alerts**
(campanii nevăzute în ciclul precedent)
- [Advertiser] — [Rețea] — [GEO] — [Format] — [First seen]

**Secțiunea 6: Action Items**
| # | Acțiune | Responsabil | Deadline | Link procedură |
|---|---------|-------------|----------|----------------|
| 1 | Testează headline formula #1 în MGID Kenya | Media buyer | +3 zile | M-119 Pas 3 |
| 2 | Creează pre-sell page format editorial Peru | Content | +5 zile | M-127 + M-116 |
| 3 | Generează 3 thumbnails Midjourney pentru KE | Designer | +2 zile | M-119 Pas 3 |

**Output**: `native-intelligence-brief-{GEO}-{YYYY-MM-DD}.md`

**Feed obligatoriu**:
- M-119 Pas 3 (creative production): headline formulas + thumbnail concepts
- M-117 (weekly spy): adaugă noii competitori nativi la lista de monitoring general

**Checkpoint Pas 6**:
- [ ] Brief livrat în format standard complet (toate 6 secțiuni prezente)
- [ ] Minimum 3 headline formulas cu exemple adaptate brand + GEO
- [ ] Minimum 3 Midjourney prompts ready-to-use
- [ ] Action items table completă (responsabil + deadline per item)
- [ ] Brief distribuit echipei (media buyer + content + designer)
- [ ] New competitor alerts documentate vs ciclu precedent
- [ ] Output `native-intelligence-brief-{GEO}-{YYYY-MM-DD}.md` finalizat

---

## §3. NATIVE CREATIVE PERFORMANCE MATRIX

Matrice de referință rapidă — actualizată la fiecare ciclu bi-weekly:

| Headline Formula | Exemplu | CTR Est. | Rețea | GEO | Days Active | Status |
|-----------------|---------|---------|-------|-----|-------------|--------|
| Curiosity Gap | "Lo que los peruanos no saben sobre casinos online" | 0.12% | MGID | PE | 50+ | WINNER — Testează |
| Number-Led | "3 sitios de apuestas con retiro Yape instantáneo" | 0.09% | Taboola | PE | 35 | ACTIVE — Monitorizează |
| Warning/Alert | "Antes de apostar en Perú en 2026: lee esto" | 0.08% | MGID | PE | 28 | ACTIVE — Monitorizează |
| Social Proof | "How Moses won KES 45,000 betting on Harambee Stars" | 0.11% | MGID | KE | 45+ | WINNER — Testează |
| Number-Led | "5 betting sites Kenya cu instant Mpesa withdrawal" | 0.10% | Taboola | KE | 40 | WINNER — Testează |
| Warning/Alert | "Warning: evita estos sitios de apuestas en Chile" | 0.09% | Outbrain | CL | 30 | ACTIVE — Monitorizează |
| Curiosity Gap | "El secreto que los casinos Chile no quieren que sepas" | 0.13% | MGID | CL | 55+ | WINNER — Replicat imediat |

---

## §4. CORTEX SAVE

```json
{
  "tool": "cortex_store",
  "domain": "affiliate",
  "sub_domain": "native-ads-intelligence",
  "procedure_id": "M-129",
  "title": "Native Ads Competitive Intelligence Protocol",
  "summary": "Protocol bi-weekly de spy dedicat native advertising (Taboola/Outbrain/MGID) pentru verticala gambling LATAM/Africa (PE/KE/CL). 6 pași: tool setup + competitor identification, headline formula extraction (min 20/GEO), pre-sell page audit (min 3/GEO), thumbnail pattern analysis (min 15/GEO), network performance intelligence, brief sintetic acționabil. Output direct în M-119 (creative production) și M-117 (monitoring). Completează gap-ul total din M-117 care nu acoperă native.",
  "tags": ["native-ads", "spy-tools", "taboola", "outbrain", "mgid", "headline-formulas", "pre-sell-pages", "thumbnail-patterns", "competitive-intelligence", "gambling-affiliate", "latam", "africa", "peru", "kenya", "chile"],
  "connects_to": ["M-117", "M-119", "M-116", "M-126"],
  "cadence": "bi-weekly",
  "version": "1.0",
  "file_path": "~/.nexus/procedures/training/affiliate/M-129-native-ads-spy-protocol.md"
}
```

---

## §5. CONNECT (Relații cu alte proceduri)

| Procedură | Tip relație | Direcție | Detaliu |
|-----------|-------------|----------|---------|
| **M-117** | Parallel / Complementar | Bidirectional | M-117 = spy social (Meta/TikTok). M-129 = spy native. Briefurile se livrează împreună luni dimineața. M-129 trimite noii competitori nativi în lista M-117. |
| **M-119** | Consumer principal | M-129 → M-119 | M-119 Pas 3 (creative production) primește direct: headline formulas, thumbnail concepts, pre-sell page formats din M-129 brief. |
| **M-116** | Context cultural | M-116 → M-129 | Cultural brief din M-116 alimentează adaptarea headline-urilor și CTA-urilor în Pas 2 și Pas 6 (prenume locale, referințe culturale, valute). |
| **M-126** | Cross-reference | M-126 ↔ M-129 | Competitor SEO discovery poate revela operatori cu campanii native active neidentificate. Cross-referință la Pas 1 (SimilarWeb referral check). |
| **M-127** | Indirect | M-129 → M-127 | Pre-sell page formats identificate în Pas 3 informează structura editorial a paginilor din programmatic SEO. |

---

## §6. METRICI & TARGETS

| Metric | Target | Cum se măsoară | Frecvență |
|--------|--------|----------------|-----------|
| Headline-uri colectate | >20 per GEO per ciclu | Count din `headline-database-{GEO}.md` | Bi-weekly |
| Pre-sell pages auditate | >3 per GEO per ciclu | Count din `presell-audit-{GEO}.md` | Bi-weekly |
| Thumbnail-uri analizate | >15 per GEO per ciclu | Count din `thumbnail-patterns-{GEO}.md` | Bi-weekly |
| Brief delivery rate | 100% (livrat în fiecare ciclu luni) | Data livrare vs data planificată | Bi-weekly |
| Action items implementate | >80% din ciclul precedent | Follow-up la brieful anterior | Bi-weekly |
| Competitor alerts noi | Documentat (orice număr) | Comparație vs ciclu precedent | Bi-weekly |
| Tool stack operațional | 100% la start ciclu | Verificare login + filtre | Bi-weekly |

---

## §7. HOW (VIOLĂRI)

**VIOLATION CRITICA**:
- Publicarea de campanii native fără a rula M-129 în ultimele 14 zile pentru GEO-ul respectiv
- Utilizarea M-117 ca substitut pentru native spy (metodologie incompatibilă)

**VIOLATION STANDARD**:
- Brief livrat cu <3 headline formulas sau <3 Midjourney prompts
- Pre-sell audit cu <8 elemente evaluate per pagină
- Action items fără responsabil sau deadline atribuit

**NOTA**: M-129 nu blochează lansarea campaniilor native în cazul unui GEO complet nou la prima intrare — în acel caz se execută Pas 1-3 înainte de lansare, iar Pas 4-6 se completează în primele 7 zile post-lansare.

---

---

## §8. ENFORCEMENT LOOP (META-H-002)

**WHERE**: La planificarea campaniilor native ads per GEO + la fiecare ciclu bi-weekly (luni dimineața)

**WHEN**:
- Înainte de lansarea oricărei campanii native pe un GEO nou: Pas 1-3 obligatorii pre-lansare
- Ciclu bi-weekly (luni dimineața, sincronizat cu M-117): Pas 1-6 complet
- La fiecare campanie native lansată: intelligence brief livrat în max 24h pre-lansare

**HOW — Violations critice**:
- **VIOLAȚIE CRITICĂ**: Campanie native lansată fără M-129 rulat în ultimele 14 zile → risc $300-500 pierdut în teste evitabile
- **VIOLAȚIE CRITICĂ**: Utilizarea M-117 ca substitut pentru M-129 (metodologie incompatibilă — native ≠ social)
- **VIOLAȚIE**: Brief livrat cu <3 headline formulas sau <3 Midjourney prompts gata de utilizare
- **VIOLAȚIE**: Pre-sell audit cu <8 elemente evaluate per pagină
- **VIOLAȚIE**: Action items fără responsabil sau deadline atribuit

**CONNECT**:
- **M-117** (Competitive Intelligence): M-129 trimite noii competitori nativi în lista de monitoring M-117; briefurile se livrează împreună luni dimineața
- **M-119** (TikTok/Native Ads Production): preia direct headline formulas + thumbnail concepts + pre-sell formats din M-129 brief
- **M-116** (Localization Engine): cultural brief din M-116 alimentează adaptarea headline-urilor și CTA-urilor (prenume locale, valute, referințe culturale)
- **M-126** (SEO Intelligence): cross-referință la Pas 1 — competitor SEO discovery poate revela operatori cu campanii native neidentificate
- **M-127** (Programmatic SEO): pre-sell page formats din Pas 3 informează structura paginilor programmatic SEO

**VERIFY**:
- [ ] Tool stack operațional (login DomCop/AdBeat/Anstrex/SimilarWeb) verificat la start ciclu?
- [ ] Minimum 5 campanii identificate per GEO activ (15 total PE+KE+CL)?
- [ ] Minimum 20 headline-uri colectate per GEO, 100% clasificate după formula?
- [ ] Brief livrat în ziua de luni a ciclului (data planificată respectată)?
- [ ] Action items din ciclul precedent: >80% implementate de echipă?
- [ ] Noii competitori documentați și adăugați în M-117?

**VK-uri obligatorii**:
- `✅ [PROC] M-129 | ciclu: #{N} | GEO: {PE/KE/CL} | headlines: {N} | competitors: {N} noi | brief: LIVRAT`
- `✅ [CORTEX] "M-129 Native Intel Brief {GEO} {data}" | rule: TRAIN-S-001 | v1.0`

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-6): analiza pattern-urilor headline + clasificare formule (Pas 2), pre-sell page audit (Pas 3 — interpretare strategică)
- **Claude Sonnet**: generare tabel output (Pas 1-5), sintetizare brief (Pas 6), formatare rapoarte

---

*Procedură FORGE v1.0 — Genie Slim Core — 2026-03-05*
