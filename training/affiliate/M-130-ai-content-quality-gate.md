---
type: procedure
created: 2026-03-20
status: active
slug: m-130-ai-content-quality-gate
tags: [procedure, nexus]
---

# M-130 — AI Content Humanization & Quality Gate

**Domeniu**: affiliate | **Sub-domeniu**: content-quality-gate
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Integrare**: Pas obligatoriu post-M-127 (Programmatic SEO), pre-publish. ZERO excepții.

---

## §1. PROBLEMA

### De ce acest gate este existențial pentru operațiunea SEO

**1. Google SpamBrain 2026: 91% precizie de detecție AI**
SpamBrain nu mai este un filtru rudimentar de keyword stuffing — este un clasificator avansat antrenat pe milioane de articole AI cu precizie de 91% pe text patterns. Modelele de limbaj lasă semnături statistice detectabile: distribuție uniformă a lungimii propoziților, absența erorilor tipice umane, coerență excesivă în tranziții, lexic over-formal. Aceste semnături sunt vizibile pentru SpamBrain chiar și fără acces la metadate.

**2. HCU sitewide penalty: CEL MAI PERICULOS TIP DE PENALIZARE SEO**
Helpful Content Update nu penalizează pagini individuale cu conținut AI — penalizează ÎNTREG site-ul dacă o proporție semnificativă din conținut este flagged. Dacă >30% din paginile unui site gambling affiliate sunt detectate ca AI-generated:
- Toate paginile pierd ranking, inclusiv cele scrise complet manual
- Recuperarea durează 12-18 luni (conform Google documentation + industry case studies)
- Un site cu 500 articole pierde TOATĂ investiția de content dintr-un update

**3. Gambling YMYL = scrutin maxim**
Google clasifică gambling și finance ca YMYL (Your Money Your Life) — categoria cu cele mai stricte criterii de E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness). Algoritmul SpamBrain aplică un multiplicator de risc mai mare pe YMYL content. Un site de rețete cu 35% AI score poate trece neobservat. Un site de gambling cu 35% AI score este la risc direct.

**4. Erori factuale: risc de compliance + pierdere user trust**
Conținutul AI generat la scară comite erori factuale predictibile: sume de bonus incorecte, termeni T&C depășiți, numere de licență incorecte sau expirate, metode de plată nedisponibile în GEO respectiv. Aceste erori:
- Invalidează compliance-ul din M-118 (licențe greșite = conținut ilegal potențial)
- Distrug credibilitatea site-ului cu userii care verifică oferta și găsesc discrepanțe
- Expun operațiunea la reclamații din partea regulatorilor (ASJ Peru, BCLB Kenya, SJC Chile)

**5. Starea curentă: M-127 fără gate = risc necontrolat**
M-127 (Programmatic SEO) generează conținut la scară — zeci de articole per batch. Fără M-130, fiecare articol publicat este un vector de risc HCU necontrolat. Cu cât scara crește, cu atât crește probabilitatea statistică de trigger al penalizării sitewide.

**Costul inacțiunii**: O singură penalizare HCU sitewide = pierderea completă a canalului SEO pentru 12-18 luni. La o estimare conservativă de 500 articole × 100 vizite organice/articol/lună = 50,000 vizite organice pierdute pe lună. La o rată de conversie de 2% și CPA de $15 (Peru benchmark) = $15,000/lună pierdut pe durata recuperării.

---

## §2. PROCEDURA

### Pas 1: AI DETECTION SCAN

**Obiectiv**: Scanarea 100% a articolelor din batch-ul M-127 cu Originality.ai și clasificarea lor pe 4 niveluri de acțiune.

#### Tool stack de detecție

| Tool | Cost | Precizie | Utilizare |
|------|------|---------|---------|
| **Originality.ai** | $0.01/100 cuvinte | ~94% (best in class 2026) | Primary tool — OBLIGATORIU |
| **Winston AI** | $0.008/100 cuvinte | ~91% | Alternativă cost-eficientă volum mare |
| **GPTZero** | Gratuit (tier limitat) | ~85% | Spot-check ad-hoc, nu pentru batch complet |

**NOTA**: Originality.ai este standardul industrial. La $0.01/100 cuvinte, un articol de 800 cuvinte costă $0.08 la scan. Un batch de 50 articole = $4. Costul scanului este neglijabil față de costul unei penalizări HCU.

#### Clasificare scoruri AI

| Scor AI | Clasificare | Acțiune | Target % batch |
|---------|------------|---------|----------------|
| **0-30%** | PASS | Publică imediat — merge direct în Pas 3 factual verification | >40% |
| **31-50%** | LIGHT EDIT | 10-15% variație propoziții necesară — timp estimat: 15-20 min/articol | ~40% |
| **51-70%** | MODERATE REWRITE | 30-40% propoziții rescrise — timp estimat: 45-60 min/articol | <15% |
| **71-100%** | HEAVY REWRITE | 50%+ rewrite sau regenerare cu Claude Opus (temperature ridicată) — timp estimat: 90+ min/articol | <5% |

#### Workflow scan

1. Exportă toate articolele din batch M-127 ca fișiere text (plain text, fără HTML)
2. Upload batch în Originality.ai (Project → Batch Scan)
3. Descarcă raportul CSV cu scorurile
4. Clasifică fiecare articol conform tabelului de mai sus
5. Segregează articolele în foldere: `pass/`, `light-edit/`, `moderate-rewrite/`, `heavy-rewrite/`

**Alertă PASS rate scăzut**: Dacă PASS rate (scor 0-30%) este <40% în mod consistent pe 2+ batch-uri consecutive → template-ul M-127 necesită redesign. Trigger redesign M-127: notifică imediat echipa content.

**Output**: `detection-scan-{batch-id}.csv`

Format CSV:
```
Article Title | Keyword | Word Count | AI Score | Classification | Action Required | Priority
"Casino Peru online" | casino peru | 850 | 22% | PASS | Publică direct | LOW
"Apuestas Kenya" | betting kenya | 780 | 47% | LIGHT EDIT | 10-15% rewrite | MEDIUM
"Best slots Chile" | slots chile | 820 | 63% | MODERATE REWRITE | 30-40% rewrite | HIGH
```

**Checkpoint Pas 1**:
- [ ] 100% articole din batch scanate (zero excepții)
- [ ] Zero articole procedează la Pas 2 fără scan completat
- [ ] Clasificare 4 niveluri aplicată tuturor articolelor
- [ ] PASS rate calculat și documentat (alert dacă <40%)
- [ ] Articole segregate în foldere per clasificare
- [ ] Output `detection-scan-{batch-id}.csv` complet

---

### Pas 2: HUMANIZATION TECHNIQUES

**Obiectiv**: Aplicarea tehnicilor de umanizare pe articolele clasificate LIGHT EDIT, MODERATE REWRITE și HEAVY REWRITE, cu re-scan obligatoriu post-umanizare până la atingerea scorului <50%.

#### Tehnicile de umanizare (aplicate în ordine de efort)

**Tehnica 1 — Variație structură propoziție** (aplicabilă la toate nivelurile)
AI generează propoziții consecutive cu lungime similară (pattern detectabil statistic). Target: mix de propoziții scurte (7-8 cuvinte) cu propoziții lungi (20-25 cuvinte).
- Înainte: "Betsson este un casino online popular. Oferă multe jocuri. Bonusul de bun venit este atractiv."
- După: "Betsson e un operator cu prezență solidă în Peru. Dacă ești la primul cont, bonusul de bun venit merită atenție — 100% până la 500 PEN, cu rulaj de 30x pe sloturi."

**Tehnica 2 — Variație activ/pasiv** (aplicabilă la LIGHT EDIT și mai sus)
AI folosește voce activă în proporție uniformă excesivă. Inserează 20-30% construcții pasive acolo unde sună natural.
- Înainte: "Operatorul procesează plățile în 24 ore."
- După: "Plățile sunt procesate de obicei în 24 ore, deși în weekend am văzut ocazional întârzieri de până la 48h."

**Tehnica 3 — Injecție de specificitate locală** (OBLIGATORIE la MODERATE REWRITE și HEAVY REWRITE)
Înlocuiește exemplele generice cu referințe GEO-specifice:
- Jucător generic → "un jucător din Lima" / "a bettor from Nairobi" / "un usuario de Santiago"
- Statistici generice → statistici locale cu sursă (ex: "conform MINCETUR, 2.3 milioane de peruani joacă online")
- Metode de plată generice → metode specifice GEO (Yape/Plin pentru Peru, Mpesa pentru Kenya, WebPay pentru Chile)

**Tehnica 4 — Markeri de imperfecțiune** (aplicabilă la MODERATE REWRITE și HEAVY REWRITE)
Adaugă limbaj de hedging și observații de primă persoană acolo unde contextul permite:
- "de obicei", "în general", "în experiența mea", "în testele noastre"
- "nu am putut verifica dacă..." (unde incertitudinea este realistă)
- Colocvialisme ușoare specifice culturii locale (verifica cu M-116)

**Tehnica 5 — Variație tranziții** (aplicabilă la toate nivelurile)
AI folosește tranziții standard repetitiv: "furthermore", "however", "in addition", "additionally". Înlocuiește cu tranziții variate inclusiv informale.
- Standard AI: "Furthermore, the casino offers..."
- Human variation: "Apropo de jocuri...", "Ceva ce merită menționat...", "Un detaliu important:", "Și totuși..."

**Tehnica 6 — Inserție citat** (1 citat atribuit per 500 cuvinte)
- Prioritate: citat real dintr-o sursă verificabilă (forum affiliate, declarație oficial GEO)
- Alternativă acceptabilă: "conform unui specialist în gambling online" sau "după cum explică un account manager de la [operator]"
- NU inventa citate cu persoane reale neverificabile

#### Workflow pentru HEAVY REWRITE (scor 71-100%)

Folosește Claude Opus cu promptul următor:

```
Rescrie această secțiune ca și cum ar fi scrisă de un vorbitor nativ de [limbă] care a testat personal acest casino.
Adaugă observații personale specifice, variază semnificativ structura propozițiilor, include 2-3 imperfecțiuni minore tipice scrisului uman.
Păstrează IDENTICE toate informațiile factuale (sume, operatori, termeni, licențe).
NU adăuga informații factuale noi.
Limbaj: [spaniolă peruviană / swahili mixing / spaniolă chileană conform brief M-116]

[ARTICOL DE REWRITE]
```

**RE-SCAN OBLIGATORIU**: După orice umanizare, re-scanează articolul în Originality.ai. Dacă scorul rămâne ≥50% → aplică tehnici suplimentare și re-scanează din nou. Procesul se repetă până la scor <50%. Nu există derogare de la această regulă.

**Output**: articole umanizate în `humanized-content-{batch-id}/`

**Checkpoint Pas 2**:
- [ ] Toate articolele LIGHT EDIT / MODERATE REWRITE / HEAVY REWRITE procesate cu tehnicile corespunzătoare
- [ ] RE-SCAN completat post-umanizare pentru 100% articole editate
- [ ] 100% articole ating scor <50% înainte de a proceda la Pas 3
- [ ] HEAVY REWRITEs procesate cu Claude Opus (nu Sonnet sau haiku)
- [ ] Articolele PASS din Pas 1 trec direct la Pas 3 (nu necesită Pas 2)

---

### Pas 3: FACTUAL VERIFICATION CHECKLIST

**Obiectiv**: Verificarea factică a 10 elemente critice per articol — zero derogări. Orice item FAIL = articolul nu poate fi publicat.

#### Checklist complet (10 itemi — TOȚI obligatorii)

```
ARTICOL: [titlu]
KEYWORD: [keyword principal]
DATA VERIFICARE: [dată]
VERIFICATOR: [nume]

□ 1. OPERATOR NAME
   Ortografie corectă verificată pe site-ul oficial al operatorului
   Sursa: [URL site oficial]
   Status: PASS / FAIL
   Note: ___

□ 2. LICENȚĂ NUMĂR & VALIDITATE
   Numărul de licență corespunde registrului regulatorului GEO:
   - Peru: ASJ (Autoridad Nacional de Control del Juego) → asj.gob.pe
   - Kenya: BCLB (Betting Control and Licensing Board) → bclb.go.ke
   - Chile: SJC (Superintendencia de Juegos de Chile) → sjc.gob.cl
   Sursa: [URL registru regulator]
   Status: PASS / FAIL
   Note: ___

□ 3. SUMA BONUS
   Suma din articol coincide cu oferta curentă de pe pagina de landing a link-ului afiliat
   Screenshot evidență atașat: DA / NU
   Sursa: [URL LP afiliat — screenshot obligatoriu]
   Status: PASS / FAIL
   Note: ___

□ 4. T&C BONUS (termeni și condiții)
   Wagering requirement, max bet, perioadă de validitate — toate corecte și actuale
   Sursa: [URL T&C operator]
   Status: PASS / FAIL
   Note: ___

□ 5. METODE DE PLATĂ
   Toate metodele de plată menționate în articol sunt disponibile efectiv pe pagina de deposit a operatorului
   Metode verificate: ___
   Sursa: [URL deposit page]
   Status: PASS / FAIL
   Note: ___

□ 6. DISCLAIMER TEXT
   Textul de disclaimer corespunde cerințelor GEO-specifice din brief-ul M-116
   - Peru: "El juego puede crear adicción. Juega responsablemente. +18. MINCETUR autorizado."
   - Kenya: "Gambling is addictive and can be harmful. Play responsibly. 18+. BCLB licensed."
   - Chile: "El juego en exceso puede ser perjudicial. +18. Licencia SJC."
   Status: PASS / FAIL
   Note: ___

□ 7. LINK AFILIAT FUNCȚIONAL
   Link-ul CTA se rezolvă corect pe pagina operatorului cu parametrii de tracking corecți
   URL testat: [URL complet cu tracking params]
   Tracking params prezente: DA / NU
   Redirect funcțional: DA / NU
   Status: PASS / FAIL
   Note: ___

□ 8. ZERO LIMBAJ DE CÂȘTIG GARANTAT
   Scan pentru: "garantat", "sigur câștig", "guaranteed to win", "sure bet", "asegurado", "ganar siempre"
   Rezultat scan: [cuvinte detectate dacă există]
   Status: PASS (zero instanțe) / FAIL (instanțe găsite — listează)
   Note: ___

□ 9. MENȚIUNE VÂRSTĂ MINIMĂ
   "18+" sau echivalentul local este prezent în articol (poate fi în disclaimer sau în corp)
   Localizare în text: ___
   Status: PASS / FAIL
   Note: ___

□ 10. DATA PUBLICĂRII
   Articolul este datat cu anul curent (2026) — nu conține referință la ani anteriori în titlu sau H1
   Data/Anul în titlu: ___
   Status: PASS / FAIL
   Note: ___

REZULTAT FINAL: ___/10 PASS
DECIZIE: APPROVED FOR PUBLISH / BLOCKED — REQUIRES CORRECTION

Dacă BLOCKED: itemi de corectat: [listă numerotată]
Data corecție target: ___
```

**REGULA ZERO DEROGĂRI**: Dacă ORICE item din checklist este FAIL, articolul nu poate fi publicat până la corectare și re-verificare a itemului respectiv. Nu există excepție pentru deadline-uri sau volume de batch.

**Output**: `factual-verification-{article-slug}.md` (un fișier per articol)

**Checkpoint Pas 3**:
- [ ] Checklist complet (10/10 itemi) completat pentru fiecare articol
- [ ] Screenshot evidență atașat pentru Item 3 (bonus amount) — obligatoriu
- [ ] Zero articole cu FAIL nerezolvat procedează la Pas 4
- [ ] Fișier `factual-verification-{article-slug}.md` creat și arhivat per articol

---

### Pas 4: EDITORIAL REVIEW QUEUE

**Obiectiv**: Review editorial final pentru calitate vizuală și readability înainte de publish, cu SLA de 24h.

#### Workflow review

**1. Intrare în coadă**
- Articolele cu 10/10 PASS din Pas 3 sunt setate la status "Pending Review" în WordPress
- Custom fields WordPress populate obligatoriu:
  - `ai_detection_score`: scorul final post-umanizare
  - `factual_pass`: 10/10
  - `factual_check_date`: data verificării factuale
  - `reviewer`: câmp gol (de completat de editor)
  - `publish_date`: câmp gol (de completat post-aprobare)

**2. Review editorial (checklist editor)**
```
□ Headers: H1 unic + H2/H3 corecte (structură ierarhică)
□ Imagini: alt text prezent + optimizate pentru web (<200KB)
□ Internal links: minimum 2 link-uri interne la articole conexe
□ CTA buttons: vizibile, text clar, culoare contrastantă
□ Readability Flesch-Kincaid:
   - LATAM Spanish target: 60-70 (accesibil)
   - Kenyan English target: 50-60 (ușor mai tehnic OK)
□ Mobile preview: vizualizat pe mobile în WP preview — layout corect
□ Slug URL: SEO-friendly, include keyword principal
□ Meta description: 150-160 caractere, include keyword
```

**3. SLA editorial**
- Entry în Pending Review → aprobare editor: **<24h**
- Articol în Pending Review >48h: alertă automată la content manager

**4. Publish + indexare**
- Status schimbat la "Publish"
- URL trimis la Rapid URL Indexer (conform M-127 Pas 6)
- Data publicare înregistrată în custom field `publish_date`

**Output**: Dashboard WordPress cu custom fields populate (tracking per articol: detection_score, factual_pass, reviewer, publish_date)

**Checkpoint Pas 4**:
- [ ] Zero articole în Pending Review >48h
- [ ] Toate custom fields WordPress populate pentru 100% articolelor publicate
- [ ] Checklist editorial complet (8 itemi) per articol
- [ ] URL submis Rapid URL Indexer la maximum 1h post-publish
- [ ] Fișier `factual-verification-{article-slug}.md` arhivat (nu șters post-publish)

---

### Pas 5: POST-PUBLISH MONITORING

**Obiectiv**: Audit lunar proactiv de sănătate a conținutului publicat — detectarea timpurie a semnalelor de penalizare Google înainte de impact semnificativ.

#### Audit lunar (rulat în prima zi lucrătoare a fiecărei luni)

**1. Re-scan eșantion aleatoriu Originality.ai**
- Selectează 10% din articolele publicate în luna anterioară (aleatoriu)
- Re-scanează în Originality.ai
- Scopul: detectarea oricărei variații în scorul AI (Google poate re-evalua conținut)
- Alert: dacă >20% din eșantion are scor >50% post-publish → audit full al batch-ului respectiv

**2. Google Search Console manual actions check**
- Accesează GSC → Security & Manual Actions → Manual Actions
- Check: zero manual actions active
- Alert CRITIC: orice manual action legată de "spammy content" sau "unnatural links" → escaladare imediată în 24h

**3. Traffic anomaly detection**
- Rulează raport MoM (month-over-month) per pagină publicată cu AI content
- Trigger investigație: orice pagină cu drop >50% trafic organic MoM care are scor AI >40%
- Trigger audit sitewide: drop >30% trafic organic sitewide coincizând cu Google Core Update → audit AI content 100% articole în 24h

**4. Procedura de urgență sitewide** (trigger: >30% drop trafic organic)
1. Stop publicare oricărui conținut nou — freeze imediat
2. Exportă lista tuturor articolelor publicate cu `ai_detection_score` din WordPress
3. Re-scanează 100% articolelor cu Originality.ai (nu eșantion)
4. Articolele cu scor >50%: delistează temporar (set to Draft) până la rewriting complet
5. Depune reconsideration request în GSC dacă există manual action
6. Timeline: freeze → audit complet → delistare → rewrite → re-publish: maximum 7 zile

**Output**: `content-health-report-{YYYY-MM}.md`

Format raport lunar:
```
## Content Health Report — [Lună] [An]

### 1. AI Score Re-scan Eșantion
Articole scanate: X / Y total (10% eșantion)
Scor mediu eșantion: X%
Articole cu scor >50%: X (lista: ...)
Acțiune: [nicio acțiune / articole identificate trimise în Pas 2 pentru re-umanizare]

### 2. Google Search Console Status
Manual actions active: DA / NU
Manual actions noi față de luna anterioară: X
Acțiune: [nicio acțiune / escaladare]

### 3. Traffic Anomalies
Pagini cu drop >50% MoM: X (lista: ...)
Drop sitewide (dacă aplicabil): X%
Coincidență cu Google Update: DA / NU
Acțiune: [nicio acțiune / investigație / audit sitewide]

### 4. Summary Status
Site status: HEALTHY / WATCH / ALERT / CRITICAL
Recomandări: [bullet points]
```

**Checkpoint Pas 5**:
- [ ] Audit lunar completat în prima zi lucrătoare a lunii
- [ ] Eșantion 10% re-scanat Originality.ai
- [ ] GSC manual actions verificate — zero notificări neadresate >72h
- [ ] Traffic anomalies raportate (orice drop >50% MoM investigat)
- [ ] Drop sitewide >30% → audit complet inițiat în 24h (nu 48h, nu mai mult)
- [ ] Output `content-health-report-{YYYY-MM}.md` complet și arhivat

---

## §3. AI SCORE DISTRIBUTION TARGET

Tabelul de referință pentru evaluarea calității template-ului M-127:

| AI Score Range | Clasificare | Acțiune | Target % batch | Alertă dacă |
|----------------|-------------|---------|----------------|-------------|
| **0-30%** | PASS | Publică direct (după Pas 3) | >40% | Sub 25% = redesign template urgent |
| **31-50%** | LIGHT EDIT | 10-15% rewrite | ~40% | Peste 50% = review prompt M-127 |
| **51-70%** | MODERATE REWRITE | 30-40% rewrite | <15% | Peste 20% = redesign template |
| **71-100%** | HEAVY REWRITE | 50%+ sau regenerare Opus | <5% | Orice >10% = redesign template IMEDIAT |

**Interpretarea distribuției**: Un template M-127 bine calibrat produce >80% articole în primele două categorii (PASS + LIGHT EDIT). Dacă în mod repetat >20% din articole cad în MODERATE sau HEAVY → template-ul nu este calibrat pentru evasion detection și necesită redesign înainte de continuarea producției la scară.

---

## §4. CORTEX SAVE

```json
{
  "tool": "cortex_store",
  "domain": "affiliate",
  "sub_domain": "content-quality-gate",
  "procedure_id": "M-130",
  "title": "AI Content Humanization & Quality Gate",
  "summary": "Gate obligatoriu dual pentru conținut AI gambling affiliate: (1) Originality.ai scan <50% AI score + umanizare în 4 niveluri (PASS/Light Edit/Moderate Rewrite/Heavy Rewrite), (2) checklist factual 10 itemi (operator name, licență, bonus, T&C, payment methods, disclaimer, link afiliat, zero guaranteed wins language, vârstă minimă, dată). Integrat post-M-127 pre-publish. Zero excepții. HCU sitewide penalty prevention pe sites gambling YMYL. Google SpamBrain 2026: 91% detection accuracy. Penalizare sitewide = 12-18 luni recuperare.",
  "tags": ["ai-detection", "originality-ai", "humanization", "hcu-penalty", "spam-brain", "ymyl", "gambling-content", "quality-gate", "factual-verification", "compliance", "programmatic-seo", "content-pipeline"],
  "connects_to": ["M-127", "M-113", "M-118", "M-116"],
  "cadence": "per-batch (post-M-127) + audit lunar",
  "version": "1.0",
  "file_path": "~/.nexus/procedures/training/affiliate/M-130-ai-content-quality-gate.md"
}
```

---

## §5. CONNECT (Relații cu alte proceduri)

| Procedură | Tip relație | Direcție | Detaliu |
|-----------|-------------|----------|---------|
| **M-127** | Producer → Gate | M-127 → M-130 (obligatoriu) | M-127 generează articolele la scară. M-130 este Pas obligatoriu post-generare, pre-publish. Nicio publicare fără M-130 complet. |
| **M-113** | Paralel / Standard | M-113 ↔ M-130 | M-113 (affiliate content marketing) produce conținut manual. M-130 se aplică și pe conținut manual dacă există suspiciune de AI (spot-check recomandat). |
| **M-118** | Compliance feed | M-118 → M-130 | Pas 3 Item 2 (număr licență) și Item 6 (disclaimer text) sunt validate direct față de specificațiile GEO din M-118. Actualizare M-118 (licențe noi) → update automat în checklist M-130. |
| **M-116** | Cultural brief | M-116 → M-130 | Pas 3 Item 6 (disclaimer text GEO-specific) și Pas 2 Tehnica 3 (injecție specificitate locală) folosesc brief-ul cultural din M-116. |
| **M-127 Pas 6** | Downstream | M-130 → M-127 Pas 6 | Rapid URL Indexer (M-127 Pas 6) se rulează NUMAI post-M-130 aprobare. Un articol care nu a trecut M-130 NU este submis pentru indexare. |

---

## §6. HOW (VIOLĂRI — IERARHIE STRICTĂ)

### VIOLATION CRITICA (blochează operațiunea)

- **Publicarea oricărui articol fără scan Originality.ai completat** — indiferent de presiunea de deadline sau volumul batch-ului
- **Publicarea cu scor AI >50%** după humanizare (re-scan nu a atins threshold-ul <50%)
- **Ignorarea unui manual action GSC >72h** fără escaladare și plan de acțiune

### VIOLATION STANDARD (blochează articolul individual)

- **Checklist factual incomplet** (unul sau mai mulți itemi nescanați, nu doar FAIL)
- **Publicarea cu orice item FAIL** în checklist factual (cu excepția situației în care corecția a fost aplicată și re-verificată)
- **Articol în Pending Review >48h** fără escaladare la content manager

### VIOLATION MINOR (documentat, corectat în ciclu următor)

- **Lipsă screenshot evidență** pentru Item 3 (bonus amount) — articolul NU se publică fără screenshot, deci aceasta devine VIOLATION STANDARD automat
- **Lipsă arhivare** `factual-verification-{article-slug}.md` post-publish
- **Raport lunar nefinalizat** în prima zi lucrătoare a lunii (grace period: 3 zile lucrătoare)

**NOTA DE PRINCIPIU**: M-130 nu are derogare bazată pe scară. Un batch de 100 articole necesită același nivel de rigoare ca un batch de 5 articole. Dacă capacitatea de execuție a M-130 limitează velocitatea de publicare, soluția este creșterea capacității de review — NU reducerea rigorii gate-ului.

---

## §7. METRICI & TARGETS

| Metric | Target | Cum se măsoară | Frecvență |
|--------|--------|----------------|-----------|
| PASS rate la primul scan | >60% | Count PASS / total batch în `detection-scan-{batch}.csv` | Per batch |
| Humanization success rate | 100% | Count articole <50% post-umanizare / total editate | Per batch |
| Factual check pass rate | 100% | Count 10/10 / total articole | Per batch |
| Review queue SLA | 100% în <24h | Data entry Pending Review → data aprobare | Per batch |
| HEAVY REWRITE rate | <5% batch | Count HEAVY / total în `detection-scan-{batch}.csv` | Per batch |
| Monthly audit completare | 100% în prima zi lucrătoare | Data raport vs target | Lunar |
| Manual actions GSC | 0 active neadresate | GSC dashboard | Lunar |
| Sitewide traffic anomalii neadresate | 0 | Raport `content-health-report-{month}.md` | Lunar |

---

---

## §8. ENFORCEMENT LOOP (META-H-002)

**WHERE**: Post-generare M-127 (batch content), pre-publicare WordPress — gate obligatoriu în pipeline

**WHEN**:
- La fiecare batch M-127 generat (indiferent de volum): Pas 1-4 obligatorii
- Prima zi lucrătoare a fiecărei luni: Pas 5 (audit lunar post-publish)
- La orice drop trafic organic >30% sitewide: audit de urgență 24h

**HOW — Violations critice**:
- **VIOLAȚIE CRITICĂ**: Publicarea oricărui articol fără scan Originality.ai completat → risc penalizare HCU sitewide
- **VIOLAȚIE CRITICĂ**: Publicarea cu scor AI >50% după humanizare → threshold nerespectate
- **VIOLAȚIE CRITICĂ**: Ignorarea unui manual action GSC >72h fără escaladare
- **VIOLAȚIE**: Checklist factual incomplet (unul sau mai mulți itemi nescanați)
- **VIOLAȚIE**: Articol în Pending Review >48h fără escaladare la content manager

**CONNECT**:
- **M-127** (Programmatic SEO): M-130 este gate obligatoriu post-M-127, pre-publish; nicio publicare fără M-130 complet
- **M-118** (Compliance): Item 2 (licență) și Item 6 (disclaimer) validate față de specificațiile GEO din M-118
- **M-116** (Cultural Brief): Pas 2 Tehnica 3 (specificitate locală) și Item 6 (disclaimer text) folosesc brief M-116
- **M-113** (Affiliate Content Marketing): M-130 se aplică și pe conținut manual dacă există suspiciune AI (spot-check recomandat)

**VERIFY**:
- [ ] 100% articole din batch scanate Originality.ai (zero excepții)?
- [ ] 100% articole ating scor <50% post-umanizare înainte de Pas 3?
- [ ] Checklist factual 10/10 itemi completat per articol?
- [ ] Screenshot evidență atașat pentru Item 3 (bonus amount) per articol?
- [ ] Zero articole în Pending Review >48h?
- [ ] Audit lunar `content-health-report-{YYYY-MM}.md` generat în prima zi lucrătoare?
- [ ] Drop sitewide >30% → audit complet inițiat în <24h?

**VK-uri obligatorii**:
- `✅ [PROC] M-130 | batch: {batch-id} | scanned: {N} | pass-rate: {X}% | factual: {N}/10 | status: APPROVED/BLOCKED`
- `✅ [CORTEX] "M-130 Quality Gate {batch-id}" | rule: TRAIN-S-001 | v1.0`

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-6): HEAVY REWRITE (scor AI 71-100%) — rescrierea articolelor cu scor AI ridicat necesită calitate maximă; interpretare anomalii trafic (Pas 5)
- **Claude Sonnet**: LIGHT EDIT și MODERATE REWRITE (Pas 2), generare raport lunar (Pas 5), formatare output CSV

---

*Procedură FORGE v1.0 — Genie Slim Core — 2026-03-05*
