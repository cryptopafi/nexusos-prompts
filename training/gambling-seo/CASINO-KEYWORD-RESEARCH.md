---
type: procedure
created: 2026-03-20
status: active
slug: casino-keyword-research
tags: [procedure, nexus]
---

# TRAIN-SEO-012: CASINO-KEYWORD-RESEARCH — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Ahrefs Methodology + Casino Keyword Intelligence -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Keyword Research / Content Planning / Market Intelligence -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-012
**Scope**: Cercetare sistematică de keyword-uri pentru afiliați casino în piețe emergente (Peru, Kenya, Chile) — de la seed keywords la cluster-uri gata de publicat, cu prioritizare prin matrice KD/Volume.

---

## 1. Problema

Greșeala clasică în gambling SEO este să ataci keyword-uri generice cu KD 60+ (ex: "online casino", "best casino") înainte de a construi autoritate domeniului. În piețe emergente — Peru, Kenya, Chile — există un avantaj structural: 60-70% mai puțin competitive față de US/UK, cu site-uri DR 15-30 rankând în top 3. Fără o metodologie clară, riskezi să:

1. **Targetezi keyword-uri prea competitive** pentru stadiul actual al domeniului → zero trafic luni de zile
2. **Ratezi longtail-uri high-intent cu KD < 15** care se pot ranka în 30-60 de zile chiar și cu un site nou
3. **Creezi conținut fără clustering** → canibalizare internă → Google nu știe ce pagină să rankeze
4. **Ignori keyword-urile de payment method** — cel mai puțin competitive și cel mai direct legate de conversie (M-Pesa Kenya, billetera peruana, pago movil Chile)
5. **Tratezi cele 3 piețe identic** → fiecare are intent diferit, limbă diferită, operatori locali diferiți

Această procedură produce un master keyword list structurat pe piață, clustere per pagină și un plan de atac săptămânal bazat pe prioritatea matricei KD/Volume.

**Piețe țintă**: Peru (Spanish — `.pe` sau `.com`), Kenya (English — `.co.ke` sau `.com`), Chile (Spanish — `.cl` sau `.com`)

**Business Application**: Gambling SEO Project (Leo, 2026-03-04) — fundația keyword research pentru money site și/sau parasite SEO în Peru + Kenya + Chile.

---

## 2. Procedura

### Pas 1: Setup Ahrefs — Filtre de Bază

Deschide **Ahrefs → Keywords Explorer**. Setează limba și țara corect ÎNAINTE de orice căutare — Ahrefs returnează date greșite dacă uiți filtrul de țară.

Configurare per piață:

| Piață | Country Filter | Language | Seed Language |
|-------|---------------|----------|---------------|
| Peru | PE | Spanish | Spanish |
| Kenya | KE | English | English |
| Chile | CL | Spanish | Spanish |

**Filtre globale aplicabile tuturor piețelor**:
- KD (Keyword Difficulty): **Maximum 25**
- Volume: **Minimum 100/lună**
- Word count: **Minimum 2** (eliminăm broad single-word terms)
- SERP features: bifează "Exclude SERP features" dacă vrei oportunități pure organice

---

### Pas 2: Seed Keywords — Listă Completă per Piață

Introdu seed-urile în Ahrefs câte un set pe sesiune. Nu amesteca piețele — fiecare export separat.

**PERU — Seed Keywords (Spanish)**:
```
casino online Peru
casinos online Peru
mejores casinos online Peru
casino Peru dinero real
tragamonedas online Peru
casino en vivo Peru
poker online Peru
apuestas deportivas Peru
casino online soles
casino acepta PagoEfectivo
casino online Peru bono
casino legal Peru
slots online Peru
blackjack online Peru
ruleta online Peru
```

**KENYA — Seed Keywords (English)**:
```
online casino Kenya
best online casino Kenya
casino Kenya M-Pesa
sports betting Kenya
football betting Kenya
casino games Kenya
online slots Kenya
poker online Kenya
casino bonus Kenya
no deposit casino Kenya
legal gambling Kenya
mobile casino Kenya
casino Kenya real money
live casino Kenya
betting sites Kenya
```

**CHILE — Seed Keywords (Spanish)**:
```
casino online Chile
mejores casinos online Chile
casino Chile dinero real
tragamonedas online Chile
casino en vivo Chile
poker online Chile
apuestas deportivas Chile
casino acepta WebPay
casino online Chile pesos
casino bono Chile
casino legal Chile
slots online Chile
blackjack online Chile
casino movil Chile
casino chileno confiable
```

---

### Pas 3: Export și Filtrare Primară

După ce rulezi seed-urile prin Ahrefs cu filtrele din Pas 1:

1. **Export CSV** — minim 500 keyword-uri per piață (dacă < 500 → relaxează filtrul Volume la 50)
2. Deschide în Google Sheets sau Excel
3. Adaugă coloană **Priority** cu formula:
   - `=IF(AND(KD<15, Volume>500), "TOP", IF(AND(KD<25, Volume>200), "HIGH", IF(AND(KD<35, Volume>500), "MEDIUM", "LOW")))`
4. Sortează descrescător după **Traffic Potential** (coloana TP din Ahrefs — mai relevantă decât Volume brut)
5. Reține primele **100 keyword-uri** cu Priority TOP sau HIGH

**Matrice Prioritate**:

| KD | Volume/lună | Prioritate | Timeline Atac |
|----|-------------|------------|---------------|
| < 15 | > 500 | TOP — atacă PRIMUL | Luna 1 |
| < 25 | > 200 | HIGH — plan lunar 1-2 | Luna 1-2 |
| < 35 | > 500 | MEDIUM — după autoritate | Luna 3+ |
| > 35 | orice | LOW — skip până la DR 30+ | Amână |

---

### Pas 4: Categorisire pe Intent și Tip Pagină

Clasifică fiecare keyword din lista filtrată în una din cele 5 categorii de intent. Această clasificare determină tipul de pagină creat și monetizarea directă.

**Categoria 1 — Review Keywords (Commercial Intent — HIGHEST)**

Cel mai mare potențial de conversie. Vizitatorul compară și e gata să depoziteze.

Exemple Peru:
- `mejores casinos online Peru 2026` → pagina "Top 10 Best Casinos Peru"
- `casino online Peru real money` → review comparativ cu CTA depozit
- `casino online Peru confiable` → ghid trust cu licențe și securitate
- `Inkabet review Peru` → review brand specific (longtail ușor)
- `Betsson Peru opiniones` → review brand + testimoniala

Exemple Kenya:
- `best online casino Kenya` → pagina hub "Best Casinos Kenya 2026"
- `top 10 casinos Kenya 2026` → listicle comparativ
- `Betway Kenya review` → review brand specific
- `SportPesa casino review` → review brand + alternativele
- `online casino Kenya real money` → ghid depozit/retragere

Exemple Chile:
- `mejores casinos online Chile 2026` → pagina hub principală
- `casino online Chile dinero real` → ghid depozit Chile
- `casino chileno confiable` → pagina trust/licențe
- `Betsson Chile review` → review brand
- `casino online Chile opiniones` → agregator review-uri

**Categoria 2 — Payment Method Keywords (High Conversion, LOW Competition)**

Acestea sunt oportunități ascunse. Keyword-uri hiperspecifice la care competiția globală nu concurează, dar conversia e maximă (utilizatorul știe exact ce metodă vrea).

Exemple Peru:
- `casino online PagoEfectivo Peru` — plata locală #1 Peru
- `casino acepta billetera peruana` — generic local wallet
- `casino online soles peruanos` — depozit în moneda locală
- `casino online Yape Peru` — Yape (app plată Perù viral)
- `casino online Plin Peru` — metodă plată locală

Exemple Kenya:
- `casino online M-Pesa Kenya` — M-Pesa = 90%+ din tranzacții Kenya
- `casino M-Pesa deposit Kenya` — intent transacțional pur
- `casino Kenya Airtel Money` — alternativa la M-Pesa
- `casino mobile money Kenya` — generic mobile payment
- `casino Mpesa withdrawal Kenya` — intent retragere (high value)

Exemple Chile:
- `casino online WebPay Chile` — gateway de plată nr. 1 Chile
- `casino acepta pago movil Chile` — generic mobile payment
- `casino online transferencia bancaria Chile` — transfer bancar
- `casino pesos chilenos` — depozit în CLP
- `casino online Khipu Chile` — Khipu = metodă populară Chile

**Categoria 3 — Game-Specific Keywords (Informational → Commercial)**

Trafic mare, conversie medie. Strategia: pagini de joc cu review-uri casino + CTA pentru respectivul joc.

Exemple Peru:
- `tragamonedas online Peru gratis` — volume mare, intent informational
- `mejores slots online Peru` — commercial cu potențial
- `poker online Peru real dinero` — commercial direct
- `blackjack online Peru` — joc specific
- `ruleta online Peru dinero real` — comercial
- `casino en vivo Peru` — live casino, conversie ridicată

Exemple Kenya:
- `sports betting Kenya football` — enorm în Kenya (football = sport #1)
- `online slots Kenya` — slots populare
- `live casino Kenya` — live dealer, conversie bună
- `poker online Kenya` — nișă mai mică
- `casino games Kenya free` — informational, nurturing funnel

Exemple Chile:
- `tragamonedas online Chile` — slots = cel mai căutat tip de joc
- `casino en vivo Chile` — live dealer, conversie ridicată
- `poker online Chile` — nișă activă
- `sports betting Chile` — apuestas deportivas în creștere
- `blackjack online Chile` — clasic

**Categoria 4 — Legal/Info Keywords (Trust Building)**

Conversie redusă direct, dar esențiale pentru SEO topical authority și pentru utilizatorii în faza de research. Linkate strategic spre pagini comerciale.

Exemple Peru:
- `casino online legal Peru` — întrebarea #1 a noilor utilizatori
- `es legal el casino online en Peru` — interogare directă Google
- `regulacion casinos online Peru` — research profund
- `MTC casinos Peru licencia` — autoritate de licențiere Peru
- `como depositar casino online Peru` — ghid educațional cu CTA

Exemple Kenya:
- `is online gambling legal in Kenya` — top 3 întrebări Kenya
- `gambling laws Kenya 2026` — informational, topical authority
- `BCLB license Kenya casino` — BCLB = Betting Control and Licensing Board
- `safe online casino Kenya` — trust query
- `how to deposit casino Kenya M-Pesa` — ghid educațional

Exemple Chile:
- `casino online legal Chile` — întrebarea principală
- `ley de juegos online Chile` — legal research
- `SCA licencia casino Chile` — SCA = Superintendencia de Casinos
- `casino seguro Chile` — trust query
- `como jugar casino online Chile` — ghid începători

**Categoria 5 — Bonus Keywords (Acquisition Intent)**

Utilizatorul caută activ un bonus → intent tranzacțional ridicat → conversie bună dacă oferi comparație clară.

Exemple Peru:
- `casino bono bienvenida Peru` — bun de bun venit, volume OK
- `casino sin deposito Peru` — no deposit bonus, conversie ridicată
- `casino 100 giros gratis Peru` — free spins, super longtail, ușor
- `mejor bono casino Peru 2026` — comercial cu date
- `casino bono deposito Peru` — deposit match bonus

Exemple Kenya:
- `casino no deposit bonus Kenya` — keyword de aur, volume bun
- `casino welcome bonus Kenya` — standard acquisition
- `free spins casino Kenya` — conversie ridicată
- `casino bonus without deposit Kenya` — variație longtail
- `best casino bonuses Kenya 2026` — comercial cu dată

Exemple Chile:
- `casino bono bienvenida Chile` — acquistion standard
- `casino sin deposito Chile` — no deposit, conversie maximă
- `giros gratis casino Chile` — free spins
- `mejor bono casino Chile 2026` — comparație cu data
- `casino bono deposito Chile` — deposit match

---

### Pas 5: SERP Analysis — Top 20 Manual Check

Pentru primele 20 keyword-uri din lista TOP + HIGH priority, verifică manual SERP-ul în Ahrefs → SERP Overview:

**Ce cauți**:
1. **DR mediu top 10** → dacă e < 30 = oportunitate clară
2. **Tipul de site-uri care rankează**:
   - Parasiți (Medium, Reddit, Quora, site-uri știri) → semnal că nișa e UȘOARĂ
   - DR < 30 money sites → poți concura din luna 1-2
   - DR 60+ money sites dominant → skip pentru acum
3. **URL-uri specifice sau homepages?** → URL-uri specifice rankând = pagini dedicate câștigă
4. **Conținut slab în top 10?** → articole < 500 cuvinte rankând = oportunitate de 10x content

**Notație în spreadsheet**:
- Coloana `SERP Signal`: EASY / MEDIUM / HARD
- `EASY` = DR mediu < 30 SAU parasiți în top 5
- `MEDIUM` = DR mediu 30-50, mix parasiți + money sites
- `HARD` = DR mediu > 50, no parasites, autoritate consolidată

Keyword-urile cu Priority HIGH + SERP Signal EASY → **lista finală de atac imediată**.

---

### Pas 6: Content Gap Analysis — Fură Keyword-urile Competiției

1. Identifică **top 3 site-uri affiliate** din piața țintă (din SERP-urile analizate la Pas 5)
2. Ahrefs → **Content Gap** → introdu cele 3 domenii ale competiției
3. Setează domeniul tău (sau platforma parasite aleasă) în câmpul "But the following target doesn't rank for"
4. Filtru: **Intersect = 3** (rankează toate 3 competitorii, tu nu rankezi)
5. Aplică filtrele KD < 25, Volume > 100
6. Export → adaugă lista la master spreadsheet, marchează ca sursă `GAP_ANALYSIS`

Aceste keyword-uri sunt validate de piață (3 concurenți deja câștigă trafic din ele) și au gap real (tu nu ești prezent). Prioritate automată: HIGH.

---

### Pas 7: LSI și Co-occurring Keywords

Pentru fiecare pagină din cluster, adaugă termeni LSI pentru a semnaliza topical depth Google. Nu keyword stuffing — utilizare naturală în text.

**Surse pentru LSI**:
1. Ahrefs → "Also rank for" pe keyword-ul principal
2. Google Search → "People also ask" și "Related searches" (scroll jos)
3. Top 10 content analysis → citește articolele rankante și extrage termeni recurenți

**Termeni LSI standard per piață**:

Peru (Spanish):
```
licencia, seguridad, bono, tragamonedas, depósito mínimo, retiro,
moneda soles, confiable, regulado, MTC Peru, atención al cliente,
casino movil, casino app, juego responsable, métodos de pago Peru
```

Kenya (English):
```
BCLB license, regulated, M-Pesa deposit, withdrawal, bonus terms,
wagering requirements, mobile casino, live dealer, fair play,
responsible gambling, customer support Kenya, KES currency
```

Chile (Spanish):
```
SCA licencia, regulado, WebPay, depósito mínimo, retiro pesos,
bono sin apuesta, casino confiable, casino movil, tragamonedas,
juego responsable, atención cliente Chile, CLP pesos chilenos
```

---

### Pas 8: Keyword Clustering — Mapare pe Pagini

Nu crea câte o pagină pentru fiecare keyword. Grupează keyword-uri semantice similare pe aceeași pagină: 1 keyword principal + 3-5 supporting keywords.

**Regula de clustering**:
- Keyword-uri cu același intent + același tip de pagină → același cluster
- Dacă SERP-urile pentru 2 keyword-uri arată aceleași URL-uri în top 10 → același cluster (Google le tratează similar)
- Dacă SERP-urile diferă complet → pagini separate

**Template Cluster (completează per piață)**:

```
CLUSTER: [Numele paginii]
URL: /[slug-pagina]
Main Keyword: [keyword principal KD < 25]
Supporting Keywords:
  - [keyword 2]
  - [keyword 3]
  - [keyword 4]
  - [keyword 5]
LSI: [5-7 termeni LSI]
Intent: REVIEW / PAYMENT / GAME / LEGAL / BONUS
Priority: TOP / HIGH / MEDIUM
SERP Signal: EASY / MEDIUM / HARD
Target DR competitor: [DR mediu top 10]
```

**Exemple Clustere Prioritare**:

Cluster Peru — Hub Principal:
```
CLUSTER: Cele Mai Bune Cazinouri Online Peru 2026
URL: /mejores-casinos-online-peru
Main Keyword: mejores casinos online Peru (KD: ~18, Vol: ~800/lună)
Supporting: casino online Peru confiable | casino Peru dinero real | top casino Peru 2026 | casino online Peru real money
LSI: licencia, soles, PagoEfectivo, MTC Peru, bono bienvenida
Intent: REVIEW
Priority: TOP
SERP Signal: EASY (DR mediu ~22 în Peru)
```

Cluster Kenya — Hub Principal:
```
CLUSTER: Best Online Casinos Kenya 2026
URL: /best-online-casino-kenya
Main Keyword: best online casino Kenya (KD: ~15, Vol: ~1200/lună)
Supporting: top casino Kenya | online casino Kenya real money | safe casino Kenya | trusted casino Kenya
LSI: M-Pesa, BCLB license, KES, mobile casino, welcome bonus
Intent: REVIEW
Priority: TOP
SERP Signal: EASY (DR mediu ~18 în Kenya)
```

Cluster Chile — Hub Principal:
```
CLUSTER: Mejores Casinos Online Chile 2026
URL: /mejores-casinos-online-chile
Main Keyword: mejores casinos online Chile (KD: ~22, Vol: ~600/lună)
Supporting: casino online Chile confiable | casino Chile dinero real | top casinos Chile 2026
LSI: SCA licencia, WebPay, pesos chilenos, bono, tragamonedas
Intent: REVIEW
Priority: TOP
SERP Signal: MEDIUM (DR mediu ~28 în Chile)
```

Cluster Kenya — Payment Method (oportunitate ascunsă):
```
CLUSTER: Casino M-Pesa Kenya
URL: /casino-mpesa-kenya
Main Keyword: casino online M-Pesa Kenya (KD: ~8, Vol: ~400/lună)
Supporting: casino M-Pesa deposit Kenya | M-Pesa casino withdrawal | mobile money casino Kenya
LSI: Safaricom, instant deposit, KES, no fees, mobile
Intent: PAYMENT
Priority: TOP
SERP Signal: EASY (KD 8 = aproape zero competiție)
```

---

### Pas 9: Master Keyword Spreadsheet — Structura Finală

Documentul final livrabil are aceste coloane:

| Keyword | Piata | KD | Volume | TP | Category | Intent | Cluster | Priority | SERP Signal | Source |
|---------|-------|----|---------|----|----------|--------|---------|----------|-------------|--------|
| mejores casinos online Peru | PE | 18 | 800 | 2100 | REVIEW | Commercial | Hub-Peru | TOP | EASY | Ahrefs |
| casino online M-Pesa Kenya | KE | 8 | 400 | 900 | PAYMENT | Transactional | MPesa-Kenya | TOP | EASY | Ahrefs |
| casino bono bienvenida Chile | CL | 20 | 350 | 800 | BONUS | Commercial | Bonus-Chile | HIGH | EASY | Ahrefs |

**Target minim**: 100 keyword-uri per piață = 300 total, din care minim 30 TOP priority.

---

### Pas 10: Plan de Atac — Primele 8 Săptămâni

Bazat pe Priority Matrix și SERP Signal:

**Săptămâna 1-2**: Publică paginile hub principale pentru fiecare piață (3 pagini — cele 3 clustere hub de mai sus). Acestea sunt ancora site-ului.

**Săptămâna 3-4**: Publică payment method pages (TOP priority, EASY SERP). Kenya M-Pesa page = primul candidat pentru ranking rapid.

**Săptămâna 5-6**: Publish bonus pages (acquisition intent) și 2-3 review-uri brand-uri locale (Inkabet Peru, Betway Kenya, Betsson Chile).

**Săptămâna 7-8**: Legal/info pages pentru topical authority. Publică "is online gambling legal in Kenya", "casino online legal Peru", "casino legal Chile".

---

## 3. Cortex Logging

La finalul fiecărei sesiuni de keyword research, salvează în Cortex:

**Entry tip**: SKILL

**Template log**:
```json
{
  "type": "SKILL",
  "title": "Casino Keyword Research — [Piata] — [Data]",
  "date": "YYYY-MM-DD",
  "tags": ["gambling-seo", "keyword-research", "[piata]"],
  "data": {
    "piata": "[Peru/Kenya/Chile]",
    "keywords_total": "[număr]",
    "top_priority": "[număr]",
    "high_priority": "[număr]",
    "serp_easy": "[număr]",
    "best_keyword": "[keyword]",
    "best_keyword_kd": "[X]",
    "best_keyword_vol": "[Y]",
    "best_keyword_tp": "[Z]",
    "content_gap_keywords": "[număr]",
    "clusters_created": "[număr]",
    "spreadsheet_path": "[path/link]",
    "note": "[observație]"
  }
}
```

**Trigger obligatoriu**: Orice keyword nou cu KD < 10 și Volume > 200 = save IMEDIAT, nu aștepți end of session.

---

## 4. Enforcement Loop

### WHERE / WHEN / HOW / CONNECT / VERIFY

- **WHERE**: Ahrefs Keywords Explorer + Google Sheets (master spreadsheet) + Google Search (incognito SERP validation)
- **WHEN**: La fiecare sesiune nouă de keyword research (per piață). Post-research: la finalul fiecărei piețe. Ongoing: săptămânal la publicarea conținutului bazat pe clustere.
- **HOW**: Exporturi CSV din Ahrefs → filtrare în Sheets → clustering manual → SERP check top 20 → content gap analysis → master spreadsheet final cu 300+ keywords.
- **CONNECT**: Output spre CASINO-CONTENT-STRATEGY.md (clustere devin plan de conținut). Output spre PBN-CONSTRUCTION-GAMBLING.md (anchor text strategy). Input din CASINO-MARKET-ANALYSIS.md (piața validată).
- **VERIFY**: VK-uri obligatorii (vezi mai jos).

### VK-uri Obligatorii

```
VK-CASINO-KW-001: [Keyword Research Complete] — Piața {piata}, {N} keywords exportate, {N} TOP priority, {N} clustere create, SERP check top 20 finalizat
VK-CASINO-KW-002: [Keyword Opportunity Found] — "{keyword}" KD {X} Vol {Y} TP {Z}, piața {piata}, SERP Signal {EASY/MEDIUM/HARD}, salvat Cortex
```

**Checklist Pre-Research** (înainte de Ahrefs):

- [ ] Filtrul de țară setat corect în Ahrefs (PE / KE / CL)
- [ ] KD max = 25 setat
- [ ] Volume min = 100 setat
- [ ] Spreadsheet nou creat cu toate coloanele din Pas 9
- [ ] Piețele separate — nu amestec keyword-uri din țări diferite în același export

**Checklist Post-Research** (după fiecare piață):

- [ ] Minim 100 keyword-uri cu Priority TOP sau HIGH exportate
- [ ] SERP manual check făcut pentru primele 20 keyword-uri
- [ ] Content Gap Analysis rulat (Pas 6)
- [ ] Clustering complet — fiecare keyword atribuit unui cluster
- [ ] LSI termeni adăugați per cluster
- [ ] Cortex log salvat
- [ ] Plan de atac 8 săptămâni actualizat cu noile keyword-uri

**Red Flags — Oprește și Reevaluează**:

- Keyword-urile TOP găsite au de fapt KD > 30 în SERP real (Ahrefs poate fi inconsistent) → verifică manual DR-ul competiției din SERP
- Top 10 dominat de casino brands cu DR > 70 (Bet365, 888Casino, PokerStars) → piața e mai dificilă decât arată KD → mută focus pe longtail-uri cu KD < 15
- Volume raportat în Ahrefs < 50 pentru "best" keyword-uri în piață → piața e prea mică → evaluează dacă merită investiția sau combini cu altă piață

**Verificare calitate cluster**:

- Un cluster NU are mai mult de 7 keyword-uri → dacă ai 10+ keyword-uri similare → split în 2 pagini separate
- Fiecare cluster are cel puțin 1 LSI term local (ex: "MTC Peru" sau "M-Pesa" sau "WebPay Chile") → dacă nu → adaugă
- Niciun keyword duplicate între clustere → canibalizare = eroare critică

---

## 5. Dependențe

**Proceduri anterioare** (trebuie completate înainte):
- `CASINO-MARKET-ANALYSIS.md` (TRAIN-SEO-001) — analiza de piață confirmă că piața e ENTER (nu NO-ENTER) înainte de a investi timp în keyword research
- `PARASITE-PLATFORM-SELECTION.md` — dacă strategia e parasite SEO, platforma aleasă influențează ce keyword-uri targetezi (ex: Medium rankează diferit față de un money site)

**Tool-uri necesare**:
- **Ahrefs** (obligatoriu) — Keywords Explorer + Content Gap + SERP Overview
- **Google Sheets** — master spreadsheet
- **Google Search** (incognito) — validare manuală SERP + "People also ask" + LSI

**Proceduri următoare** (se execută după):
- `CASINO-CONTENT-STRATEGY.md` — clusterele produse aici devin planul de conținut
- `PBN-CONSTRUCTION-GAMBLING.md` (TRAIN-SEO-008) — anchor text strategy bazată pe keyword-urile principale identificate
- `EXPIRED-DOMAIN-ACQUISITION.md` — dacă strategia include money site, domeniul se achiziționează după ce știi exact ce keyword-uri targetezi

**Input necesar de la Leo**:
- Buget tool-uri (Ahrefs = $99+/lună — obligatoriu pentru această procedură)
- Piața prioritară (Peru / Kenya / Chile) dacă se lucrează pe rând, nu simultan
- Tip site ales (money site / parasite / PBN) — influențează ce keyword-uri sunt realiste ca target

---

**DISCLAIMER**: Această procedură este material educațional pentru SEO și marketing afiliat. Cercetarea de keyword-uri trebuie executată cu respectarea ToS-ului tool-urilor utilizate (Ahrefs, Google) și a legislației locale privind publicitatea pentru gambling din fiecare piață.

### Pas 11: COMMERCIAL INTENT TIER ANALYSIS — Market-Specific Difficulty

Beyond KD scores, evaluate actual SERP competition per market:

**Peru SERP Competition Profile**:
- KD 0-10: Virtually uncontested. Payment method keywords (PagoEfectivo, Yape) rank with content alone, zero links needed. Parasite on Medium = page 1 in 14-30 days.
- KD 10-20: Light competition from local affiliates (DR 15-25). 5-8 PBN links sufficient.
- KD 20-35: Mix of local and international affiliates. Requires money site + PBN + niche edits.
- KD 35+: Major operators (Betsson, Codere) + established affiliates. Reserved for month 6+ with DR 30+ site.

**Kenya SERP Competition (Mobile-First)**:
- KD 0-8: M-Pesa keywords, Swahili variants — zero competition. First quality content = page 1.
- KD 8-15: Basic "best casino Kenya" variants. Parasites dominate. 3-5 links move needle.
- KD 15-25: Sports betting keywords (football, EPL). Moderate competition from betting sites.
- KD 25+: Generic gambling terms. International operators present. Requires authority.
- **Mobile-first factor**: 95% of Kenya searches are mobile. Test ALL keyword SERPs on mobile view.

**Chile Regulated Market Difficulty**:
- KD 0-15: Payment method + legal queries. Compliant content with SCJ references ranks well.
- KD 15-30: Brand reviews, bonus comparisons. DR 25-35 competitors present.
- KD 30-45: Category keywords (casinos online Chile). Established affiliates + operators.
- KD 45+: Head terms. Major publishers (Emol, La Tercera) + licensed operators. Year 2+ territory.
- **Regulatory advantage**: Ley 21.456 compliant content has E-E-A-T advantage over generic affiliate sites.

### Pas 12: EXPIRED DOMAIN KEYWORD MATCHING

When acquiring expired domains for PBN or money sites (see EXPIRED-DOMAIN-ACQUISITION procedure), match domain history to keyword clusters:

1. Check Archive.org: was the expired domain in a related niche? (sports, entertainment, finance = good for gambling)
2. Check Ahrefs: what keywords did the domain previously rank for?
3. If domain has history ranking for casino/betting terms → premium asset, use as money site
4. If domain has gambling-adjacent history (sports, entertainment) → use as PBN T1
5. Map matched domains to specific keyword clusters in your master spreadsheet

**VK**: `[VK-CASINO-KW-001: Procedura CASINO-KEYWORD-RESEARCH v2.0 — 5 categorii intent, 3 piețe, 3-tier difficulty analysis, template clustere + plan 8 săptămâni]`
