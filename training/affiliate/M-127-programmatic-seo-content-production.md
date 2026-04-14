---
type: procedure
created: 2026-03-20
status: active
slug: m-127-programmatic-seo-content-production
tags: [procedure, nexus]
---

# M-127 — Programmatic SEO Content Production: Gambling LATAM/Africa

**Domeniu**: affiliate | **Sub-domeniu**: programmatic-seo
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Connections**: M-113 (Content Marketing) · M-116 (Localization Engine) · M-117 (Competitive Intelligence) · M-118 (Compliance) · M-130 (AI Quality Gate) · M-131 (Multi-Touch Attribution)

---

## 1. PROBLEMA

### De ce procedura aceasta este critică

**Bottleneck-ul producției manuale**

În regimul manual, un redactor produce 6-8 articole pe săptămână. 30 de articole în Săptămânile 3-6 este realist. 150+ articole până în Luna 6 nu este posibil fără automatizare — necesită echivalentul a 4-5 redactori full-time, cost lunar ~$3.000-5.000 USD, plus timp de coordonare, briefing, revizuire. La scară de 50-100 pagini/lună, producția manuală devine imposibilă economic.

**Diferența 4-6 luni vs 18 luni la P1**

Google necesită timp pentru a evalua un domeniu nou. Fiecare lună în care nu publici pagini pentru un keyword gap = o lună pierdută din fereastra de maturizare a domeniului. Cu programmatic SEO, primele 50 de pagini se publică în prima lună și intră în indexare imediată. Fără automatizare, aceleași 50 de pagini durează 6-8 luni să fie produse și publicate. Diferența practică: un domeniu care pornește programmatic SEO în Luna 1 poate atinge P1 pe keyword-uri de nișă în 4-6 luni; unul care pornește producția manuală în Luna 1 ajunge la același rezultat în 18-24 luni.

**Oportunități necontested pierdute zilnic**

Keyword-urile identificate în M-117 sunt necontested ACUM:
- "casino con Yape" (Peru) — zero concurenți de calitate, KD <10, volum 2.400/lună
- "casino online con Yape sin verificacion" — long-tail, zero competiție, intent de conversie maxim
- "casino con Webpay" / "casino Khipu Chile" — thin competition, sub-20 domenii cu autoritate
- "bet with M-Pesa Kenya" / "casino M-Pesa" — uncontested în engleză și swahili

Fiecare zi fără pagini publicate pentru aceste keyword-uri = o zi în care un competitor poate intra și ocupa golul. Odată ce un competitor publică 20+ pagini cu backlinkuri, recuperarea durează 12-18 luni.

**Riscurile fără quality gate (Google SpamBrain 2026)**

Google SpamBrain detectează în 2026 conținut AI generat sub 3 semnale principale: uniformitate stilistică în batch-uri mari, lipsa E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness), și factual inaccuracy (date bonus incorecte, pași de plată greșiți). O penalizare HCU (Helpful Content Update) este sitewide — nu afectează doar paginile slabe, ci ÎNTREGUL domeniu. Recuperarea după HCU durează minimum 12 luni și necesită ștergerea sau rescrierea 70%+ din conținut.

**Ce se întâmplă fără această procedură**

Scenariul realist fără M-127: concurentul care adoptă programmatic SEO în Q1 2026 va publica 200-400 de pagini pentru payment method keywords în Peru, Chile, Kenya înainte ca noi să publicăm primele 30. La 6 luni de la lansare, acel competitor deține 80%+ din traficul organic pentru "casino [payment] [GEO]". Fereastra de recuperare: minimum 12-18 luni, probabil imposibil fără investiție masivă în link building. Zero recovery path ieftin.

---

## 2. PROCEDURA

### Pipeline Production Summary

| Page Type | Tool | Cost/Article | Target Volume/Lună | Quality Gate | Publish Lag |
|-----------|------|-------------|---------------------|-------------|-------------|
| Payment method pages | Koala.sh | $0.018 | 30/lună | Surfer≥70 + Orig<50% | <48h după QG |
| Operator reviews | Byword.ai | $0.40 | 40/lună | Surfer≥70 + Orig<50% | <48h după QG |
| Comparison pages | Claude API | $0.03 | 20/lună | Surfer≥70 + Orig<50% + Manual | <72h după QG |
| How-to guides | Koala.sh | $0.018 | 10/lună | Surfer≥70 + Orig<50% | <48h după QG |

---

### Pas 1: KEYWORD TEMPLATE DESIGN

**Scop**: Transformă keyword gaps din M-117 în template-uri de pagini programmatice, fiecare cu câmpuri de date unice care diferențiază paginile între ele.

**Acțiuni concrete**:

1. Deschide M-117 → secțiunea "Keyword Gap Analysis" → exportă tabelul de keyword-uri necontested (KD <20) per GEO în Google Sheets.

2. Clasifică fiecare keyword în unul din 4 page types:

   **Payment Method Pages** (prioritate maximă, necontested):
   - Pattern: `"casino con [payment]"`, `"bet with [payment] [GEO]"`, `"[payment] casino online [GEO]"`, `"casino [payment] sin verificacion"`
   - Exemple: "casino con Yape Peru", "bet with M-Pesa Kenya", "casino online con Webpay Chile"
   - Unique Data Fields: `payment_name`, `payment_logo_url`, `deposit_steps` (3-5 pași), `min_deposit_amount`, `processing_time`, `bonus_for_payment_method`, `operators_accepting_payment` (listă)

   **Operator Review Pages**:
   - Pattern: `"casino [operator] opiniones [GEO]"`, `"[operator] review [GEO]"`, `"[operator] confiable [GEO]"`
   - Exemple: "Betsson opiniones Peru", "1xBet review Kenya", "888 Casino Chile confiable"
   - Unique Data Fields: `operator_name`, `license_number`, `license_authority`, `bonus_welcome`, `min_deposit`, `withdrawal_time`, `payment_methods_list`, `pros_list`, `cons_list`

   **Comparison Pages**:
   - Pattern: `"mejor casino [GEO] 2026"`, `"mejores apuestas deportivas [GEO]"`, `"top casinos [payment] [GEO]"`
   - Exemple: "mejor casino Peru 2026", "mejores casinos con Yape"
   - Unique Data Fields: `comparison_focus` (ex: "payment speed"), `operator_list` (5-10 operatori), `ranking_criteria`, `winner_operator`, `winner_reason`

   **How-To Pages**:
   - Pattern: `"como registrarse en [operator] [GEO]"`, `"como depositar con [payment] en [operator]"`, `"como retirar dinero [operator]"`
   - Exemple: "como depositar con Yape en Betsson Peru"
   - Unique Data Fields: `operator_name`, `payment_name`, `step_by_step` (numbered, 5-8 pași), `screenshots_available` (da/nu), `troubleshooting_common_issues`

3. Pentru fiecare page type, completează tabelul template:

   | Page Type | Keyword Pattern | Unique Data Field | Volume | KD | Prioritate |
   |-----------|----------------|-------------------|--------|----|-----------|
   | Payment method | casino con [payment] | payment_name + deposit_steps + bonus | 2.400 | 8 | MAXIM |
   | Operator review | [operator] opiniones [GEO] | license_number + bonus + pros/cons | 1.200 | 15 | RIDICAT |
   | Comparison | mejor casino [GEO] 2026 | operator_list + ranking_criteria | 3.100 | 22 | MEDIU |
   | How-to | como depositar con [payment] | step_by_step + troubleshooting | 800 | 6 | RIDICAT |

4. Validare: fiecare page type trebuie să aibă cel puțin un câmp de date care face fiecare pagină din batch DIFERITĂ de celelalte. Dacă două pagini au identic 90%+ din conținut = template defectuos, risc HCU.

**Tool**: SEMrush / Ahrefs (date keyword), M-117 Keyword Gap section (input principal), Claude Opus (analiza pattern-urilor și validarea unique data fields)

**Output**: `programmatic-templates-{GEO}.md`
- Exemplu: `programmatic-templates-PE.md`, `programmatic-templates-KE.md`, `programmatic-templates-CL.md`
- Format: tabel complet per page type cu toate câmpurile de mai sus

**Checkpoint PASS/FAIL**:
- PASS: minimum 5 page types definite per GEO, fiecare cu unique data field identificat; minimum 20 keyword patterns per GEO catalogate
- FAIL: orice page type fără unique data field = risc thin content = STOP, redefine template înainte de continuare

---

### Pas 2: DATA SOURCE MAPPING

**Scop**: Populează o foaie de calcul cu toate datele unice necesare pentru fiecare pagină programmatică. Zero rânduri cu celule goale = zero thin content.

**Acțiuni concrete**:

1. Creează un Google Sheets document cu numele `programmatic-data-{GEO}` (ex: `programmatic-data-PE`).

2. Structura sheet-ului (coloane):
   ```
   A: page_id (ex: PE-PAY-001)
   B: page_type (payment_method / operator_review / comparison / howto)
   C: target_keyword (keyword exact pentru H1)
   D: url_slug (ex: /casino-con-yape-peru/)
   E: [unique fields per page type — adaugă coloane specifice]
   ```

3. Pentru **Payment Method Pages** — surse de date per câmp:
   - `payment_name`: Yape / Plin / Webpay / Khipu / M-Pesa / Airtel Money (manual, oficial)
   - `deposit_steps`: extrage din pagina oficială de ajutor a payment provider (Yape.com.pe, safaricom.co.ke) sau test account
   - `min_deposit_amount`: verifică pe site-ul fiecărui operator care acceptă payment method
   - `processing_time`: pagina oficială operator sau test
   - `bonus_for_payment_method`: link afiliat activ → verifică oferta curentă (nu interpolă, verifică live)
   - `operators_accepting_payment`: scrape din paginile de "Metode de plată" ale operatorilor sau Ahrefs competitor pages

4. Pentru **Operator Review Pages** — surse de date:
   - `license_number` + `license_authority`: M-118 registry (ASJ Peru, SJC Chile, BCLB Kenya)
   - `bonus_welcome`: pagina oficială operator → T&C (nu folosi date vechi)
   - `payment_methods_list`: pagina "Cashier" sau "Deposit" a operatorului
   - `pros_list` + `cons_list`: minimum 3 pro + 3 con per operator (cercetare reală, nu generic)

5. Regula ZERO celule goale: înainte de a genera conținut AI, verifică că fiecare rând din CSV are TOATE câmpurile completate. Un rând cu câmp gol = pagina nu se generează în batch. Completează manual sau marchează ca "PENDING" și exclude din batch curent.

6. Export CSV din Google Sheets: File → Download → CSV (pentru fiecare sheet/GEO).

**Tool**: Google Sheets (structurare date), browser (colectare manuală), M-118 (surse compliance), API scraper (opțional, pentru volume mari de operatori)

**Output**: `programmatic-data-{GEO}.csv`
- Exemplu: `programmatic-data-PE.csv`, `programmatic-data-KE.csv`, `programmatic-data-CL.csv`
- Un rând per pagină target, toate câmpurile unice completate

**Checkpoint PASS/FAIL**:
- PASS: CSV complet, zero celule goale în câmpurile unice; minimum 50 rânduri per GEO pentru payment method pages
- FAIL: orice celulă goală în câmp unic = rând exclus din batch = nu se generează pagina respectivă (marchează ca PENDING pentru batch următor)

---

### Pas 3: CONTENT GENERATION PIPELINE

**Scop**: Generează articolele AI pentru fiecare rând din CSV, folosind toolul potrivit per page type, cu variabilele injectate din CSV.

**Acțiuni concrete**:

**3A. Payment Method Pages — Koala.sh**

1. Accesează app.koala.sh → "Bulk Generation" sau "Single Article".
2. Selectează: Article Type = "Blog Post" sau "Affiliate Review", Language = Español (Peru/Chile) sau English (Kenya).
3. System prompt (copiază exact, înlocuiește variabilele cu datele din CSV):

```
Ești un expert în cazinouri online din [GEO], vorbitor nativ de [LIMBA]. Scrie un articol complet și util despre depunerea la cazinourile online folosind [PAYMENT_NAME].

Date reale de folosit:
- Payment method: [PAYMENT_NAME]
- Pași de depunere: [DEPOSIT_STEPS]
- Depunere minimă: [MIN_DEPOSIT_AMOUNT]
- Timp procesare: [PROCESSING_TIME]
- Bonus disponibil: [BONUS_FOR_PAYMENT_METHOD]
- Cazinouri care acceptă această metodă: [OPERATORS_LIST]

Structură obligatorie:
H1: [TARGET_KEYWORD] (exact, fără modificare)
H2: Ce este [PAYMENT_NAME] și cum funcționează
H2: Cazinouri online care acceptă [PAYMENT_NAME] în [GEO]
H2: Cum să depui la un cazino online cu [PAYMENT_NAME] (pași numerotați)
H2: Bonusuri disponibile pentru depuneri cu [PAYMENT_NAME]
H2: Avantaje și dezavantaje [PAYMENT_NAME] pentru cazinouri
H2: Întrebări frecvente

Reguli obligatorii:
- Nu promite câștiguri garantate
- Include disclaimer: "Jocurile de noroc pot crea dependență. Joacă responsabil."
- Adaptează disclaimer la reglementările locale [GEO]
- Lungime: 1.200-1.800 cuvinte
- Ton: profesional, de ajutor, bazat pe date reale
```

4. Enable SERP analysis în Koala.sh pentru fiecare keyword (opțiunea "Real-Time SERP Data") — aceasta face toolul să analizeze ce e pe P1 înainte de generare.
5. Generează batch de maximum 30 articole per sesiune pentru payment method pages.

**3B. Operator Review Pages — Byword.ai**

1. Accesează byword.ai → "Bulk Articles".
2. Upload CSV cu coloanele: `keyword`, `operator_name`, `license_number`, `bonus_welcome`, `pros`, `cons`.
3. Template Byword (configurează în interfața Byword "Custom Template"):

```
Title: [OPERATOR_NAME] Opiniones [GEO] [YEAR] — ¿Es Confiable?
Target keyword: [TARGET_KEYWORD]
Include: license [LICENSE_NUMBER] issued by [LICENSE_AUTHORITY], welcome bonus [BONUS_WELCOME], minimum deposit [MIN_DEPOSIT], pros: [PROS_LIST], cons: [CONS_LIST]
Language: [LANGUAGE]
Disclaimer: [DISCLAIMER_TEXT from M-116]
```

4. Byword generează articolele în bulk (40/lună target). Download ZIP cu toate fișierele .md.

**3C. Comparison Pages — Claude API**

1. Folosește Claude API (model: claude-opus-4-6) cu prompt structurat per pagină (nu bulk — fiecare comparison page are nevoie de raționament custom).
2. Script Python de bază (salvat la `~/.nexus/scripts/generate-comparison.py`):

```python
import anthropic
import csv

client = anthropic.Anthropic()

with open('programmatic-data-{GEO}.csv') as f:
    reader = csv.DictReader(f)
    for row in reader:
        if row['page_type'] != 'comparison':
            continue

        prompt = f"""
        Scrie un articol de comparație pentru keyword: {row['target_keyword']}
        GEO: {row['geo']}
        Operatori de comparat: {row['operator_list']}
        Criteriu principal: {row['comparison_focus']}
        Winner: {row['winner_operator']} pentru că: {row['winner_reason']}

        Structură: H1 (keyword exact), H2 per operator, tabel comparativ, verdict final.
        Lungime: 1.500-2.000 cuvinte. Includeți disclaimer responsabilitate.
        """

        response = client.messages.create(
            model="claude-opus-4-6",
            max_tokens=4000,
            messages=[{"role": "user", "content": prompt}]
        )

        # Salvează output
        with open(f"raw-content-{row['geo']}-batch/{row['page_id']}.md", 'w') as out:
            out.write(response.content[0].text)
```

**Verificare output (obligatorie înainte de Quality Gate)**:
- Deschide 3 articole random din batch și verifică: nu există [VARIABLE] strings neînlocuite
- Verifică că H1 conține exact target keyword (nu parafrazat)
- Verifică că disclaimer-ul este prezent la finalul fiecărui articol

**Tool**: Koala.sh API / Koala.sh web app (payment method pages), Byword.ai bulk (operator reviews), Claude API cu Python script (comparison pages)

**Output**: folder `raw-content-{GEO}-{batch}/` (ex: `raw-content-PE-2026-04/`)
- Un fișier `.md` per pagină
- Naming convention: `{page_id}-{url_slug}.md` (ex: `PE-PAY-001-casino-con-yape-peru.md`)

**Checkpoint PASS/FAIL**:
- PASS: minimum 10 articole generate per batch; zero [VARIABLE] strings rămase în output; H1 conține exact target keyword în toate articolele
- FAIL: orice variabilă neînlocuită = articol exclus din batch, verifică CSV row pentru câmp lipsă

---

### Pas 4: QUALITY GATE 1 — Surfer SEO Score

**Scop**: Asigură că fiecare articol acoperă semantic complet topicul și nu este thin content în raport cu P1 competitors.

**Acțiuni concrete**:

1. Accesează app.surferseo.com → "Content Editor".
2. Pentru fiecare articol din batch:
   - Creează un nou Content Editor document cu target keyword exact (ex: "casino con Yape Peru")
   - Setează GEO (Peru / Chile / Kenya) și limba corectă
   - Paste conținutul articolului AI în editor
   - Notează scorul Surfer (0-100)

3. Clasificare și acțiune per scor:

   **Scor ≥70 → PASS** — articolul trece la Quality Gate 2.

   **Scor 50-69 → REWRITE AI**:
   - Uită-te la panoul Surfer "Missing Terms" și "Keyword Density"
   - Copiază lista de termeni lipsă (ex: "Yape Peru", "depozit minim", "retragere rapidă")
   - Trimite articolul + lista de termeni la Claude Sonnet cu prompt:
     ```
     Rescrie articolul următor incorporând natural acești termeni lipsă: [LISTA TERMENI].
     Nu schimba structura H1/H2. Nu adăuga conținut fals. Articol original: [ARTICOL]
     ```
   - Re-upload articolul rescris în Surfer, verifică că scorul a urcat ≥70

   **Scor <50 → MANUAL REVIEW**:
   - Probabil problemă de template (structura H2-urilor nu acoperă topicul)
   - Nu continua batch-ul pentru page type-ul respectiv
   - Analizează ce acoperă paginile P1 (click pe competitorii din Surfer Editor)
   - Fix template în `programmatic-templates-{GEO}.md` înainte de batch următor

4. Completează tracking sheet `quality-scores-{batch}.csv`:

   | Article ID | Keyword | Surfer Score | Status | Action |
   |-----------|---------|-------------|--------|--------|
   | PE-PAY-001 | casino con Yape Peru | 74 | PASS | — |
   | PE-PAY-002 | casino con Yape sin verificacion | 62 | REWRITE | + 8 termeni |
   | PE-REV-001 | Betsson opiniones Peru | 44 | MANUAL | fix template |

5. Target per batch: >80% din articole cu scor ≥70 la prima trecere. Dacă <60% trec din prima = problemă sistemică de template, stop batch, fix template.

**Tool**: Surfer SEO Content Editor (app.surferseo.com)

**Output**: `quality-scores-{batch}.csv` — Article | Keyword | Surfer Score | Status (PASS/REWRITE/MANUAL)

**Checkpoint PASS/FAIL**:
- PASS: 100% din articolele din batch au scor ≥70 înainte de a merge la pasul următor
- FAIL: orice articol sub 70 = nu se publică; dacă >20% din batch sunt MANUAL = stop, fix template

---

### Pas 5: QUALITY GATE 2 — AI Detection + Factual Verification

**Scop**: Asigură că conținutul nu va fi detectat ca AI de Google și că toate datele factuale (bonusuri, pași de plată, numere de licență) sunt corecte la momentul publicării.

**Acțiuni concrete**:

**5A. AI Detection — Originality.ai**

1. Accesează originality.ai → "Scan Content".
2. Upload sau paste fiecare articol (după ce a trecut Quality Gate 1 cu Surfer ≥70).
3. Verifică scorul AI (0-100%, unde 100% = scris integral de AI):

   **Scor AI <50% → PASS** — articolul poate merge la publicare.

   **Scor AI 50-80% → HUMANIZE**:
   - Tehnici de aplicat (aplică 3-5 din lista de mai jos per articol):
     * Variați lungimea propozițiilor (alternați propoziții scurte de 5-8 cuvinte cu unele de 20-25 cuvinte)
     * Adăugați o observație personală specifică GEO-ului (ex: "În Peru, Yape este deja folosit de peste 12 milioane de oameni — nu e surprinzător că și cazinourile online l-au adoptat")
     * Înlocuiți fraze generice AI ("Este important de menționat că...") cu construcții directe ("Reține că...")
     * Adăugați o statistică locală verificată (nu inventată)
     * Schimbați ordinea unor paragrafe (nu schimbați logica, doar ordinea unor secțiuni similare)
   - Re-scan în Originality.ai după humanizare

   **Scor AI >80% → REWRITE 30-40%**:
   - Identificați cele mai "robotice" secțiuni (Originality.ai le evidențiază)
   - Rescriați manual sau cu prompt specific Claude: "Rescrie această secțiune în stilul unui jurnalist local din [GEO] care a testat personal serviciul. Adaugă detalii senzoriale și observații practice."
   - Re-scan după rewrite

**5B. Factual Verification Checklist**

Pentru fiecare articol, completează checklist-ul de mai jos ÎNAINTE de aprobare:

```
[ ] 1. Numele operatorului este scris corect (verificat pe site-ul oficial)
[ ] 2. Numărul de licență este corect și activ (verificat în M-118 registry)
[ ] 3. Suma bonusului de bun venit este actuală (verificat pe pagina oficială operator, nu pe alt site)
[ ] 4. Pașii de depunere cu payment method sunt corecți (verificat pe pagina Cashier a operatorului sau test account)
[ ] 5. Disclaimer text corespunde cerințelor GEO (verificat în M-116 cultural brief pentru Peru / Chile / Kenya)
[ ] 6. Link-urile afiliate din articol duc la pagina corectă (click manual pe fiecare CTA)
```

Semnătură verificator: _____ | Data: _____

**Notă**: Rulează M-130 (AI Content Quality Gate) în paralel cu Pasul 5 — M-130 are criterii suplimentare de E-E-A-T care completează Originality.ai.

**Tool**: Originality.ai (AI detection), browser (factual verification), M-130 (AI Quality Gate procedure), M-116 (disclaimer text per GEO)

**Output**:
- `detection-scores-{batch}.csv` — Article | AI Score | Status | Action
- Factual verification checklist completat și semnat per articol (poate fi o coloană adițională în CSV)

**Checkpoint PASS/FAIL**:
- PASS: 100% articole cu AI detection score <50%; factual checklist 100% completat pentru fiecare articol
- FAIL: orice articol >50% AI detection = nu se publică; orice item nevalidat din factual checklist = articol blocat

---

### Pas 6: AUTO-PUBLISH WORDPRESS

**Scop**: Publică articolele aprobate pe site-ul WordPress per GEO prin REST API, cu review uman obligatoriu înainte de publicare finală, urmat de push indexing.

**Acțiuni concrete**:

**6A. Publicare prin WordPress REST API**

Script Python (salvat la `~/.nexus/scripts/wp-publish.py`):

```python
import requests
import json
import markdown
import csv

WP_URL = "https://{domeniu-GEO}.com/wp-json/wp/v2/posts"
WP_USER = "api_user"
WP_APP_PASSWORD = "xxxx xxxx xxxx xxxx xxxx xxxx"  # WordPress Application Password

def publish_to_draft(article_path, metadata):
    with open(article_path, 'r') as f:
        md_content = f.read()

    # Extrage H1 din articol ca titlu
    title = [line for line in md_content.split('\n') if line.startswith('# ')][0].replace('# ', '')

    # Convertește Markdown → HTML
    html_content = markdown.markdown(md_content)

    payload = {
        "title": title,
        "content": html_content,
        "status": "draft",  # MEREU draft, nu publish direct
        "categories": [metadata['category_id']],  # ID categorie GEO + page_type
        "tags": [metadata['primary_keyword'], metadata['payment_method'], metadata['geo']],
        "meta": {
            "target_keyword": metadata['target_keyword'],
            "surfer_score": metadata['surfer_score'],
            "originality_score": metadata['ai_score'],
            "publish_date": metadata['date']
        }
    }

    response = requests.post(
        WP_URL,
        json=payload,
        auth=(WP_USER, WP_APP_PASSWORD)
    )

    return response.json()['link']  # URL-ul draft-ului

# Rulează pe batch
with open('quality-scores-{batch}.csv') as f:
    reader = csv.DictReader(f)
    published_urls = []

    for row in reader:
        if row['status'] == 'PASS':
            url = publish_to_draft(
                f"raw-content-{GEO}-{batch}/{row['article_id']}.md",
                row
            )
            published_urls.append(url)
            print(f"Draft creat: {url}")
```

**6B. Review uman (obligatoriu, target <24h)**

Editorul verifică fiecare draft în WordPress:
- [ ] Featured image setată (dimensiune minimă 1200x630px, relevantă pentru topic)
- [ ] Internal links funcționează (click pe fiecare link intern din articol)
- [ ] CTA buttons duc la URL-ul afiliat corect (verifică că tracking-ul Voluum e activ)
- [ ] Meta description completată în Yoast/RankMath (max 155 caractere, include keyword)
- [ ] URL slug corect (ex: /casino-con-yape-peru/ — fără caractere speciale)

Dacă toate check-urile sunt OK: schimbă status din "draft" → "publish" (manual din WP dashboard).

**6C. Rapid URL Indexer — push indexing**

1. Accesează rapidurlindexer.com (sau alternativa preferată).
2. Exportă lista URL-urilor proaspăt publicate din `published-urls-{batch}.csv`.
3. Upload lista în Rapid URL Indexer → Submit.
4. Target: indexare confirmată în Google Search Console în maximum 48h.
5. Verificare după 48h: Search Console → URL Inspection → verifică "URL is on Google".

**Tool**: WordPress REST API (Python script `wp-publish.py`), Rapid URL Indexer, WordPress Dashboard (review uman)

**Output**: `published-urls-{batch}.csv`

| URL | Titlu | Publish Date | Surfer Score | AI Score | Status |
|-----|-------|-------------|-------------|----------|--------|
| /casino-con-yape-peru/ | Casino con Yape Peru 2026 | 2026-04-01 | 74 | 38% | PUBLISHED |

**Checkpoint PASS/FAIL**:
- PASS: zero articole cu status "draft" mai mult de 48h după Quality Gate PASS; 100% URL-uri proaspăt publicate trimise la Rapid URL Indexer
- FAIL: draft >48h nereviewed = alert la editor; URL neindexat după 48h = resubmit + investigare robots.txt

---

### Pas 7: PERFORMANCE MONITORING

**Scop**: Urmărește performanța fiecărei pagini publicate și identifică paginile care necesită optimizare sau investigare.

**Acțiuni concrete**:

**7A. Verificări săptămânale (în fiecare luni dimineață)**

1. Deschide Google Search Console → Performance → Filtrează per GEO (query contains "Peru" / "Kenya" / "Chile").
2. Compară: pagini noi indexate vs pagini publicate în săptămâna precedentă.
3. Alerte imediate dacă:
   - O pagină publicată nu apare în GSC în 14 zile → resubmit la indexer, verifică robots.txt, verifică că pagina are internal links din alte pagini

**7B. Review 30 zile (în ziua 30 după primul batch)**

1. Export din GSC: Performance → Filtrează ultimele 30 zile → Export CSV.
2. Notează per page type: care tip de pagini se indexează cel mai rapid? Care au impressions mai mari?
3. Adaugă date în tabelul de monitorizare `seo-performance-{GEO}-monthly.md`.

**7C. Review 90 zile — optimizare și audit**

| Situație | Acțiune |
|----------|---------|
| Poziție 1-10 la 90 zile | Menține, construiește backlinkuri |
| Poziție 11-20 la 60 zile | Update articol cu nouă trecere Surfer + adaugă 200-300 cuvinte |
| Poziție 21-30 la 90 zile | Verifică: backlinkuri lipsă? Titlu tag neoptimizat? Rewrite H1 + meta |
| Poziție >30 la 90 zile | Audit complet: analizează P1 competitors în Ahrefs, identifică gap |
| Zero impresii la 14 zile | Verifică indexare, robots.txt, internal links, canonical tags |

**7D. Tabel de monitorizare lunar**

Format `seo-performance-{GEO}-monthly.md`:

| URL | Keyword | Poziție (azi) | Poziție (30z) | Clicks | Impressions | CTR | Status |
|-----|---------|--------------|--------------|--------|-------------|-----|--------|
| /casino-con-yape-peru/ | casino con Yape Peru | 4 | 12 | 89 | 2.100 | 4.2% | CRESTE |
| /bet-with-mpesa-kenya/ | bet with M-Pesa Kenya | 1 | 3 | 210 | 4.800 | 4.4% | P1 |
| /casino-con-webpay-chile/ | casino con Webpay | 18 | — | 12 | 890 | 1.3% | UPDATE |

**Tool**: Google Search Console (Performance report), Ahrefs Rank Tracker (monitorizare zilnică automată), M-117 (comparare snapshot competitor)

**Output**: `seo-performance-{GEO}-monthly.md` (actualizat în prima zi a fiecărei luni)

**Checkpoint PASS/FAIL**:
- PASS: raport lunar generat pentru toate paginile publicate; toate paginile cu poziție >30 la 90 zile marcate pentru audit manual
- FAIL: raport lunar negenerat = VIOLATION; pagini fără date de monitorizare după 30 zile = investigare imediată

---

## 3. CORTEX LOGGING

La finalizarea fiecărui batch de producție, salvează în Cortex:

```json
{
  "collection": "procedures",
  "document": {
    "title": "M-127 Programmatic SEO Content Production Gambling LATAM Africa",
    "content": "{pipeline output + performance data}",
    "metadata": {
      "type": "procedure",
      "procedure": "M-127",
      "rule_id": "TRAIN-S-001",
      "domain": "affiliate",
      "sub_domain": "programmatic-seo",
      "pipeline": "content-production",
      "version": "2.0",
      "status": "ACTIVE",
      "tags": ["affiliate", "SEO", "programmatic", "gambling", "LATAM", "Africa", "content-production", "training"]
    }
  }
}
```

Salvează separat în Cortex după fiecare batch și după fiecare raport lunar de performanță:
- Batch output: `pages_published`, `avg_surfer_score`, `avg_ai_score`, `total_cost`, `GEO`
- Performanță lunară: `pages_p1_3`, `pages_p10`, `total_organic_clicks`, `affiliate_clicks_from_organic`

---

## 4. ENFORCEMENT LOOP (META-H-002)

**WHERE**: La planificarea calendarului de conținut (prima zi a lunii) + la onboarding GEO nou

**WHEN**: Ciclu lunar de producție (1 ale lunii); declanșat de identificarea de keyword gaps în M-117

**HOW — Violations critice**:
- **VIOLATION CRITICA**: Publicarea de conținut AI fără verificare Originality.ai = risc penalizare HCU sitewide (recuperare: minimum 12 luni, cost: pierdere 100% trafic organic)
- **VIOLATION**: Publicarea de articol cu Surfer score <70 = thin content, risc penalizare per-pagină
- **VIOLATION**: Publicarea fără factual verification checklist completat = date incorecte live (bonusuri expirate, pași greșiți = pierdere de conversii + risc reputațional)
- **VIOLATION**: M-130 (AI Quality Gate) nerunat înainte de publicare = procedura invalidă, batch considerat neconform
- **VIOLATION**: Draft WordPress rămas >48h nepublicat după Quality Gate PASS = blocaj de pipeline, pierdere fereastra de indexare

**CONNECT**:
- **M-113** (Content Marketing): structura editorială și calendaristica bazei de conținut
- **M-116** (Localization Engine): brief cultural → text disclaimer per GEO, ton de voce, termeni locali
- **M-117** (Competitive Intelligence): keyword gaps → queue de conținut programmatic (input principal)
- **M-118** (Compliance): număr licență operator, text disclaimer regulatoriu, restricții de conținut per GEO
- **M-130** (AI Quality Gate): rulat în paralel cu Pasul 5, criterii E-E-A-T suplimentare
- **M-131** (Multi-Touch Attribution): trackează content organic → click afiliat → conversie în Voluum

**VERIFY — 6 verificări concrete**:

1. **Surfer Score 100%**: Extrage `quality-scores-{batch}.csv` → filtrează coloana "Status" → verifică zero rânduri cu "FAIL" sau scor <70. Dacă există: articolele respective nu au intrat în publicare.

2. **Originality.ai 100%**: Extrage `detection-scores-{batch}.csv` → verifică că coloana "AI Score" are zero valori ≥50%. Dacă există: articolele au fost returnate pentru humanizare.

3. **Factual Checklist 100%**: Verifică `published-urls-{batch}.csv` → coloana "Factual Check" = "DONE" pentru fiecare articol. Dacă există "PENDING": articolul nu a trecut la publicare.

4. **M-130 confirmat**: Verifică că M-130 a fost rulat pentru batch (output M-130 trebuie să existe cu date din același batch). Dacă M-130 output lipsește: batch invalid.

5. **Indexare 48h**: La 48h după publicare, verifică Google Search Console URL Inspection pentru fiecare URL nou. Target: >90% indexate. Dacă <90%: investigare robots.txt și internal links.

6. **Raport lunar generat**: În prima zi a lunii, `seo-performance-{GEO}-monthly.md` trebuie să existe și să fie actualizat. Verifică că toate URL-urile publicate în luna precedentă sunt incluse. Dacă lipsesc URL-uri: raport incomplet = regenerare.

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-6): template design (Pasul 1 — analiza pattern-urilor), comparison pages (Pasul 3C — articole complexe)
- **Koala.sh**: payment method pages + how-to guides (bulk, SERP-aware)
- **Byword.ai**: operator reviews (bulk cu template variables)
- **Surfer SEO**: Quality Gate 1 (semantic scoring)
- **Originality.ai**: Quality Gate 2 (AI detection)
- **Claude Sonnet**: data structuring (Pasul 2 — organizare CSV), rewrites de humanizare (Pasul 5)

---

## 5. DEPENDENTE

**Tools necesare**:
- Koala.sh (cont activ, minimum plan $49/lună pentru bulk)
- Byword.ai (cont activ, plan $200/lună pentru 40 articole/lună)
- Surfer SEO (Content Editor, plan minimum $89/lună)
- Originality.ai (credits minimum 200 scans/lună)
- WordPress REST API (Application Password activat în WP Settings → Users)
- Rapid URL Indexer (cont activ, credits per URL)
- Google Search Console (proprietate verificată per domeniu GEO)
- Ahrefs / SEMrush (pentru Pasul 7 — rank tracking)

**Input din proceduri**:
- **M-117** (Competitive Intelligence): keyword gaps → queue de keyword patterns (input pentru Pasul 1)
- **M-116** (Localization Engine): brief cultural → disclaimer text + ton de voce per GEO (input pentru Pasul 3)
- **M-118** (Compliance): număr licență operator + text disclaimer regulatoriu (input pentru Pasul 2 și Pasul 5)

**Output către proceduri**:
- **M-130** (AI Quality Gate): rulat în paralel cu Pasul 5; preia output-ul de conținut pentru verificare suplimentară E-E-A-T
- **M-131** (Multi-Touch Attribution): preia `published-urls-{batch}.csv` pentru setup tracking Voluum content → conversie

**Infrastructură necesară**:
- WordPress site funcțional per GEO (minim: hosting, tema, Yoast/RankMath instalat)
- Python environment cu librăriile: `requests`, `markdown`, `csv`, `anthropic` (`pip install` toate)
- Google Sheets (acces la internet, cont Google)
- `~/.nexus/scripts/` folder cu scripturile `wp-publish.py` și `generate-comparison.py`

---

## 6. METRICS

| Metric | Target | Frecventa |
|--------|--------|-----------|
| Pagini produse per lună | 50-100 | Lunar |
| Surfer score ≥70 la prima trecere | >80% din batch | Per batch |
| AI detection <50% după humanizare | 100% din batch | Per batch |
| Pagini indexate în 48h | >90% | Săptămânal |
| Pagini ajunse la P1-3 la 90 zile (payment method) | >30% | Trimestrial |
| Cost per articol publicat (medie) | <$0.50 | Lunar |
| Clicks afiliate din trafic organic | +20% MoM | Lunar |

---

## CHECKLIST PRE-PUBLICARE

```
[x] 1. Keyword pattern definit și clasificat în page type (Pasul 1)
[x] 2. Data CSV completă, zero celule goale în câmpuri unice (Pasul 2)
[x] 3. Conținut generat cu toolul corect per page type (Pasul 3)
[x] 4. Zero [VARIABLE] strings neînlocuite în articol (Pasul 3 — verificare output)
[x] 5. Surfer SEO score ≥70 confirmat (Pasul 4)
[x] 6. Originality.ai score <50% confirmat (Pasul 5A)
[x] 7. Factual verification checklist completat și semnat (Pasul 5B)
[x] 8. M-130 (AI Quality Gate) rulat și aprobat (Pasul 5 — paralel)
[x] 9. Draft WordPress creat prin REST API cu toate custom fields (Pasul 6A)
[x] 10. Review uman completat: featured image, internal links, CTA affiliate URL (Pasul 6B)
[x] 11. URL publicat și trimis la Rapid URL Indexer (Pasul 6C)
[x] 12. URL adăugat în `published-urls-{batch}.csv` pentru monitorizare (Pasul 7)
```

---

*M-127 v2.0 — Programmatic SEO Content Production: Gambling LATAM/Africa*
*Connections: M-113 · M-116 · M-117 · M-118 · M-130 · M-131*
