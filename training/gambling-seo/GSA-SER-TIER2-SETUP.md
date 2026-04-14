---
type: procedure
created: 2026-03-20
status: active
slug: gsa-ser-tier2-setup
tags: [procedure, nexus]
---

# TRAIN-SEO-007: GSA-SER-TIER2-SETUP — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate + GSA SER Configuration + T2 Automation Best Practices -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / T2 Automation / GSA SER Operations -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-007
**Scope**: Setup complet și operare GSA SER pentru Tier 2 link building în campanii casino/gambling SEO, cu arhitectură T0→T1→T2→T3, configurare proxy, content spinning și drip-feed safety.

---

## 1. Problema

Tier 1 (PBN sites / parasite pages) nu pot dobândi autoritate fără un influx constant de backlinks. Link building manual la scară este imposibil economic. Fără T2 automation, T1 stagnează și money site-ul nu rankează. GSA SER rezolvă această problemă prin automatizarea completă a creării de Web 2.0s, blog comments, social bookmarks și forum profiles care pointează exclusiv la T1 — NICIODATĂ direct la money site.

**Riscul principal fără această procedură**: blast necontrolat de sute de linkuri/zi → footprint detectabil → T1 pierde linkuri sau primește penalizare manuală → toată investiția în PBN/parasiți devine inutilă.

**Piețe țintă principale**: Peru + Kenya + Chile (gambling SEO project Leo, 2026-03-04).

**Business Application**: Susținerea T1 assets (PBN + parasite pages) pentru a crește DR al acestora și a transfera link equity spre money site.

---

## 2. Procedura

### Arhitectura de Referință

Înainte de orice configurare, înțelege și documentează arhitectura completă:

```
T0 — Money Site (domeniu principal, ranking target)
  ↑ (linkuri directe, calitate înaltă, manuale)
T1 — PBN Sites + Parasite Pages (Medium, Reddit, news outlets)
       DR țintit: 20-40+ | Conținut unic, manual sau AI de calitate
  ↑ (linkuri automate, GSA SER, volume moderat)
T2 — GSA SER Automated (Web 2.0s, blog comments, forums, bookmarks)
       Volum: 20-50 verified links/zi | Conținut spun 70%+ unique
  ↑ (indexare automată)
T3 — Indexing Layer (Speed-Links.net, GSA Indexer, GSA Search Engine Indexer)
```

**Regula de aur**: GSA SER → T1 EXCLUSIV. Niciun link GSA direct spre T0.

---

### Pas 1: Achiziție și Instalare Software

- [ ] **1.1** Achiziționează licența GSA SER: `https://www.gsa-online.de/` — ~$99 one-time (actualizări incluse pe viață)
- [ ] **1.2** Instalează pe VPS Windows dedicat (recomandat: Windows Server 2019/2022, minim 4GB RAM, SSD)
  - GSA SER rulează 24/7 — NU pe laptop personal
  - VPS recomandat: Contabo, OVH, sau orice provider cu IP curat
- [ ] **1.3** Instalează GSA Captcha Breaker sau configurează integrare cu serviciu extern:
  - 2Captcha (`https://2captcha.com/`) — ~$1-3/1000 captchas
  - Anti-Captcha (`https://anti-captcha.com/`) — similar pricing
  - Notă: fără captcha solver, GSA SER nu poate posta pe majoritatea platformelor
- [ ] **1.4** Instalează article spinner ales (vezi Pas 3) pe același VPS sau local

**VK-1**: Software instalat + licențe active verificate. GSA SER deschide fără erori.

---

### Pas 2: Configurare Proxy

Proxies sunt OBLIGATORII. Fără proxy, IP-ul VPS-ului tău va fi bănuit și blocat de platforme în ore.

**Opțiunea A — Free Scraped Proxies (buget minim)**:
- [ ] **2A.1** Folosește Scrapebox Proxy Scraper (inclus în Scrapebox, ~$97 one-time) sau GSA Proxy Scraper (inclus în GSA SER)
- [ ] **2A.2** În GSA SER → Menu → Tools → Proxy Manager → Import/Scrape
- [ ] **2A.3** Rulează Proxy Tester: filtrează doar proxies cu latency <2000ms și status WORKING
- [ ] **2A.4** Setează auto-refresh proxy list la fiecare 30 minute (proxies gratuite expiră rapid)
- [ ] **2A.5** Target: minim 100 proxies active simultan pentru T2 building

**Opțiunea B — Residential Proxies (recomandat pentru Peru/Kenya/Chile)**:
- [ ] **2B.1** Achiziționează residential proxies cu GEO local din piața țintă:
  - BHW Marketplace (`https://www.blackhatworld.com/forums/proxies-for-sale.177/`) — $30-50/mo
  - Proxy providers cu GEO targeting: Bright Data, Smartproxy, Oxylabs
  - Peru → caută proxies cu IP `.pe` | Kenya → `.ke` | Chile → `.cl`
- [ ] **2B.2** Import în GSA SER: Proxy Manager → Add → paste lista `IP:PORT` sau `IP:PORT:USER:PASS`
- [ ] **2B.3** Setează format corect în GSA SER Settings → Proxies:
  - `Use proxies for all connections`: YES
  - `Test proxies before use`: YES
  - `Remove not working proxies automatically`: YES
  - `Use 1 proxy per submission`: YES (rotație obligatorie)

**VK-2**: Proxy Manager arată minim 50 proxies cu status GREEN. Test conexiune: GSA SER poate accesa target platform fără IP personal.

---

### Pas 3: Configurare Content / Article Spinning

T2 nu necesită conținut de calitate înaltă — necesită volum și unicitate suficientă pentru a nu fi detectat ca duplicate spam.

**Standarde pentru T2 content**:
- Unicitate minimă: **70%** (verificat cu spinner built-in)
- Limbă: adaptată pieței (Spanish pentru Peru/Chile, English pentru Kenya)
- Lungime articol: 300-500 cuvinte suficiente pentru T2
- NICIODATĂ nu folosi conținut spun pentru T1 sau T0

**Spinners recomandate** (alege una):

| Spinner | Preț | Calitate | Integrare GSA |
|---------|------|----------|---------------|
| WordAI | ~$57/mo | Excelentă | API direct |
| Spin Rewriter | ~$47/mo | Bună | API direct |
| Chimp Rewriter | ~$15/mo | Medie | API direct |
| Spinner Chief | ~$10/mo | Medie | API direct |

- [ ] **3.1** Achiziționează unul din spinners de mai sus
- [ ] **3.2** În GSA SER → Options → Content → Spinning:
  - Introdu API key-ul spinner-ului ales
  - Setează `Minimum uniqueness`: 70%
  - Activează `Auto-spin articles before posting`: YES
- [ ] **3.3** Creează template articles de bază în limba pieței:
  - Peru/Chile: scrie 5-10 articole în Spanish despre casino online, bonusuri, jocuri
  - Kenya: 5-10 articole în English despre betting, casino, mobile money
  - Fiecare articol va fi spun automat → mii de variante unice
- [ ] **3.4** Import articole în GSA SER: Project → Content → Articles → Add → paste conținut cu `{spinner|syntax}`

**Sintaxă spinner exemplu**:
```
{Cele mai bune|Top|Joacă la|Descoperă} {cazinouri|casino-uri|site-uri de casino} online {din Peru|în Peru|pentru jucătorii din Peru} {îți oferă|oferă|prezintă} {bonusuri|promoții|oferte} {incredibile|fantastice|atractive}.
```

**VK-3**: Test spin produce 5 versiuni cu unicitate ≥70% verificată. Articolele nu conțin text placeholder neînlocuit.

---

### Pas 4: Configurare Proiect GSA SER pentru T2

- [ ] **4.1** În GSA SER → New Project → introdu:
  - **Project Name**: `T2_[PiataȚintă]_[DataLunar]` ex: `T2_Peru_March2026`
  - **Description**: `Tier 2 pentru T1 assets Peru — casino SEO`

- [ ] **4.2** Target URLs — adaugă URL-urile T1 (PBN sites + parasite pages):
  - Project → URLs → Add URLs → paste lista URL-urilor T1
  - NU adăuga URL-ul money site-ului T0
  - Formatul: un URL per linie, ex: `https://medium.com/your-parasite-article`

- [ ] **4.3** Selectează Platform Types (T2 specific):
  - [x] Web 2.0 (Tumblr, WordPress.com, Blogspot, Wix, Google Sites)
  - [x] Social Bookmarks (Reddit, Pinterest, Flipboard, Delicious)
  - [x] Blog Comments
  - [x] Forum Profiles
  - [x] Wikis (low priority)
  - [ ] Article Directories (opțional, calitate variabilă)
  - [ ] Image Sharing (opțional)
  - DEBIFEAZA: RSS, Trackbacks (prea noisy, footprint mare)

- [ ] **4.4** Setări de submission rate (CRITIC — NU MODIFICA FĂRĂ MOTIV):
  - `Threads`: 50-100 (în funcție de RAM VPS)
  - `Verified links per day`: **20-50 maximum** (NU 500)
  - `Submissions per URL`: max 10 pe zi per T1 URL
  - `Skip if already submitted`: YES (evită duplicate)
  - `Wait between submissions`: min 30 secunde

- [ ] **4.5** Email setup pentru înregistrări platforme:
  - Folosește serviciu catch-all email: `@yourdomain.com` catch-all sau
  - Serviciu temporar email: Mailinator, Guerrilla Mail (pentru volume mari)
  - În GSA SER → Options → Email → configurează IMAP access

- [ ] **4.6** Anchor text configuration:
  - Project → Anchor Texts → Add:
    - 40% — branded anchors (ex: `Inkabet Peru`, `casino online Peru`)
    - 30% — generic (ex: `click here`, `visit site`, `mai multe detalii`)
    - 20% — partial match (ex: `casino Peru bonus`)
    - 10% — naked URL (ex: `https://your-pbn.com/page`)
  - Natural mix — evită 100% exact match anchors (footprint)

**VK-4**: Proiect configurat. Preview arată URLs T1 corecte + platform types selectate + rate ≤50 links/zi.

---

### Pas 5: Configurare Indexing Layer (T3)

Linkurile T2 trebuie indexate pentru a transmite link equity. Fără indexare, linkurile există dar nu sunt procesate de Google.

- [ ] **5.1** Speed-Links.net:
  - Creează cont nou la `https://speed-links.net/`
  - **50 linkuri gratuite** la cont nou — folosește pentru primele linkuri T2 verificate
  - Export verified links din GSA SER: Project → right-click → Export Verified URLs
  - Submit la Speed-Links în drip-feed mode (nu toate deodată)

- [ ] **5.2** Google Search Console — manual submission pentru T1 URLs:
  - Adaugă fiecare T1 URL în GSC: URL Inspection → Request Indexing
  - Limitare: ~10-20 URL-uri/zi per proprietate
  - Aplicabil DOAR pentru T1 (parasite pages pe platforme pe care le controlezi)

- [ ] **5.3** GSA SEO Indexer (opțional, add-on separat ~$55):
  - Se integrează direct cu GSA SER
  - Auto-submit verified links la indexers imediat după verificare
  - Activare: GSA SER → Options → Indexing → Enable GSA SEO Indexer

- [ ] **5.4** Medium.com amplification (gratuit):
  - Creează articole Medium care menționează și linkuiesc la T1 pages
  - Medium are DA înalt → link-ul din Medium ajută la indexare T1
  - 1-2 articole Medium/săptămână per piață

**VK-5**: Primele 50 linkuri T2 verificate trimise la Speed-Links. GSC arată cel puțin 5 T1 URLs indexate.

---

### Pas 6: Drip-Feed Schedule și Safety Rules

Aceasta este secțiunea cea mai importantă pentru a evita penalizările.

**Reguli de siguranță — OBLIGATORII**:

| Regulă | Valoare Sigură | Valoare Periculoasă |
|--------|---------------|---------------------|
| Linkuri verificate/zi | 20-50 | >200 |
| Lansare campanie | Gradual (prima săptămână 10/zi) | Blast instant |
| Distribuție pe T1 URLs | Spread uniform | Concentrat pe 1 URL |
| Monitor T1 referring domains | Săptămânal | Niciodată |
| Pauze în campanie | Da (weekenduri mai puțin) | Campanie non-stop fără pauze |

- [ ] **6.1** Prima săptămână — warm-up:
  - Zi 1-3: 10 verified links/zi total
  - Zi 4-7: 20 verified links/zi total
  - Distribuit uniform pe toate T1 URLs

- [ ] **6.2** Săptămâna 2-4 — cruise speed:
  - 30-50 verified links/zi
  - Monitorizare continuă în Ahrefs: T1 referring domains trebuie să CREASCĂ, nu să scadă

- [ ] **6.3** Semnale de alarmă — STOP campanie dacă:
  - T1 URL pierde referring domains brusc după activarea T2
  - Ranking T0 scade brusc (posibil penalty în propagare)
  - Ahrefs arată spike anormal în T1 backlinks (footprint)
  - Acțiune: reduce viteza la 10/zi + pauză 2 săptămâni + analizează

- [ ] **6.4** Schedule GSA SER pentru variație naturală:
  - GSA SER → Scheduler → activează
  - Setează ore active: 08:00-22:00 (nu toată noaptea non-stop)
  - Adaugă variație: `Random delay 15-45 minutes between sessions`

**VK-6**: Scheduler activ. Primele 7 zile: max 20 links/zi. Ahrefs monitorizare T1 configurată.

---

### Pas 7: Monitorizare și Optimizare Post-Setup

- [ ] **7.1** Ahrefs monitoring — setup alerts:
  - Adaugă fiecare T1 domain în Ahrefs Site Explorer
  - Activează Email Alerts: new backlinks + lost backlinks pentru T1 domains
  - Frecvență check: săptămânal minim

- [ ] **7.2** Rankings T1 monitoring:
  - Folosește Ahrefs Rank Tracker sau SE Ranking pentru T1 URLs
  - Așteptate: îmbunătățiri de ranking pentru T1 în 2-4 săptămâni de la activarea T2
  - Dacă T1 nu urcă în 4 săptămâni: verifică indexarea linkurilor T2 (Pas 5)

- [ ] **7.3** GSA SER Verified Links export lunar:
  - Export complet verified links din proiect
  - Arhivează în fișier CSV cu data: `T2_Peru_verified_March2026.csv`
  - Salvează în `memory/research/gambling-seo/gsa-data/`

- [ ] **7.4** Recycle links — linkuri pierdute:
  - GSA SER marchează linkuri verificate care devin "lost" (platforma le șterge)
  - Normal: 30-50% din linkuri T2 se pierd în timp — recycle prin resubmission
  - Setează în GSA SER: `Re-submit lost verified URLs after 30 days`

**VK-7**: Ahrefs alerts active pentru toate T1 domains. Prima verificare ranking planificată la 14 zile post-launch.

---

### Pas 8: Buget și ROI Tracking

**Setup costs (one-time)**:

| Item | Cost | Frecvență |
|------|------|-----------|
| GSA SER license | $99 | One-time |
| Scrapebox (proxy scraper) | $97 | One-time |
| GSA Captcha Breaker | $127 | One-time |
| VPS Windows (setup) | $10-20 | Setup |
| **Total one-time** | **~$330-340** | — |

**Ongoing costs (lunar)**:

| Item | Cost | Note |
|------|------|------|
| VPS Windows | $15-25/mo | Contabo/OVH |
| Residential proxies | $30-50/mo | Optional — scraped e gratis |
| Article spinner | $30-50/mo | Chimp Rewriter = $15 opțiune cheap |
| Captcha solving (2Captcha) | $5-20/mo | Depinde de volum |
| Speed-Links.net | Gratuit start | 50 free links/cont nou |
| **Total ongoing** | **~$80-145/mo** | — |

- [ ] **8.1** Documentează toate costurile în `memory/research/gambling-seo/budget-tracker.md`
- [ ] **8.2** Calculează ROI după 60 zile: (T0 traffic increase × CPC value) / total costs
- [ ] **8.3** Break-even tipic pentru T2 setup: 45-90 zile dacă T1 assets sunt deja poziționate

**VK-8**: Budget tracker creat. Toate costurile documentate cu receipts/screenshots.

---

## 3. Cortex Logging

```json
{
  "text": "GSA SER T2 Setup executat pentru piața [COUNTRY]. T1 URLs: [N]. Verified links/zi: [X]. Proxy type: [scraped/residential]. Spinner: [name]. Platform types: [list]. T1 RD delta: [+/-X]. T0 ranking delta: [+/-X]. Acțiune: [continuă/reduce/pauză]. Buget lunar: $[X].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "GSA-SER-TIER2-SETUP",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-007",
    "tags": ["gsa-ser", "tier2", "link-building", "automation", "gambling", "casino", "proxy", "content-spinning", "pbn-support", "peru", "kenya", "chile"],
    "has_enforcement_loop": true,
    "version": "2.0",
    "forge_version": "2.0"
  }
}
```

**Skill saves suplimentare** (la milestone-uri):
- **Skill #GSA-1**: "GSA SER T2 Setup — configurare completă pentru casino SEO [piață]" — platform types, verified links/zi, proxy type
- **Skill #GSA-2**: "T2 Impact pe T1 Rankings — pattern observat" — săptămâna T1 a început să urce + linkuri T2 la momentul respectiv
- **Skill #GSA-3**: "Proxy Performance Report — [piață] [lună]" — tip proxy, rata succes, cost/link verificat

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + La orice setup sau reconfigurare GSA SER pentru campanii gambling SEO T2.

### WHEN
La fiecare setup inițial GSA SER pentru o piață nouă. La fiecare reconfigurare de proiect (URL-uri T1 noi, proxy switch, spinner change). Verificări săptămânale post-activare (Luni/Miercuri/Vineri).

### HOW (violation detection)
- GSA SER configurat să trimită linkuri direct la T0 → **violation critică**
- Blast de 500+ linkuri în prima zi (fără warm-up) → **violation critică**
- Submissions fără proxy (IP VPS expus) → violation
- Conținut spun folosit pentru T1 sau T0 (doar T2/T3 permis) → violation
- Verified links/zi depășește 50 fără aprobare explicită → violation
- Pierdere T1 referring domains >20% ignorată >2 săptămâni → violation
- Captcha solver epuizat fără reîncărcare >48h → violation
- Liste T2 pre-built cumpărate fără verificare footprint → violation

**Verificări săptămânale (MANDATORY)**:

**Luni — GSA SER health check**:
- [ ] GSA SER rulează pe VPS (nu s-a oprit)
- [ ] Verified links din ultima săptămână: minim 140 (20/zi x 7)
- [ ] Proxy list: minim 50 active proxies
- [ ] Captcha solver: sold suficient (dacă folosești 2Captcha)

**Miercuri — T1 monitoring**:
- [ ] Ahrefs: T1 referring domains în creștere sau stabile
- [ ] Niciun T1 URL nu arată spike anormal (>50 linkuri noi într-o zi)
- [ ] GSA SER nu a depășit 50 verified links/zi în nicio zi

**Vineri — Rankings check**:
- [ ] T1 URLs: verifică top keyword rankings
- [ ] T0: verifică ranking-urile principale (trebuie să urce gradual)
- [ ] Documentează în Cortex: pattern săptămânal

**Triggers de escaladare**:

| Situație | Acțiune Imediată |
|----------|-----------------|
| T1 pierde >20% referring domains brusc | STOP campanie GSA + investigare |
| T0 ranking cade >10 poziții | Audit complet T1 + T2 footprint |
| GSA SER blochează toate platformele | Rotire proxies + verificare IP ban |
| Captcha solver epuizat | Reîncarcă sold sau switch la serviciu backup |
| VPS down | Restart VPS + verificare GSA SER auto-start |

### CONNECT
- TRAIN-SEO-007 → enforcement GSA SER T2 link building gambling
- TRAIN-SEO-008 (TIERED-LINK-ARCHITECTURE) → arhitectura T0-T3 în care T2 operează
- TRAIN-SEO-006 (PARASITE-LINK-BUILDING) → T1 assets care primesc T2 links
- `procedure-health.json` → adaugă entry
- Training Mode → curriculum SEO avansat, automatizare link building

### VERIFY
La finalul fiecărei execuții, verifică:
- [ ] Software instalat: GSA SER + Captcha Solver + Spinner + VPS funcționale (Pas 1)?
- [ ] Proxies configurate: minim 50 active, test conexiune OK (Pas 2)?
- [ ] Content spinning: test produce 5 variante cu unicitate >=70% (Pas 3)?
- [ ] Proiect GSA SER: URLs T1 corecte, platforme selectate, rate <=50/zi (Pas 4)?
- [ ] Indexing layer: Speed-Links sau GSA Indexer configurat (Pas 5)?
- [ ] Drip-feed: warm-up respectat (10/zi primele 3 zile), scheduler activ (Pas 6)?
- [ ] Monitoring: Ahrefs alerts active, ranking check planificat (Pas 7)?
- [ ] Budget tracker creat cu toate costurile (Pas 8)?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `[PROC] GSA-SER-TIER2-SETUP | S1 S2 S3 S4 VER | complete`
2. `[CORTEX] "GSA SER T2 Setup — Gambling SEO" | FORGE | rule: TRAIN-SEO-007 | v1.0`

---

## 5. Dependențe

### Depinde de (trebuie completate ÎNAINTE):

| Procedură | Fișier | Relevanță |
|-----------|--------|-----------|
| Casino Market Analysis | `CASINO-MARKET-ANALYSIS.md` | Identifică piețele țintă + keywords |
| PBN Setup | `PBN-SETUP.md` (de creat) | T1 assets trebuie să existe înainte de T2 |
| Parasite SEO Setup | `PARASITE-SEO-SETUP.md` (de creat) | T1 parasite pages ca target pentru T2 |

### Este dependență pentru:

| Procedură | Fișier | Relevanță |
|-----------|--------|-----------|
| T3 Indexing | (integrat în Pas 5) | Speed-Links + GSA Indexer |
| Rankings Monitoring | `RANKINGS-MONITORING.md` (de creat) | Tracking impact T2 pe T0 |
| Penalty Recovery | `PENALTY-RECOVERY.md` (de creat) | Dacă T2 provoacă footprint |

### Tooluri externe necesare:

| Tool | Tip | URL | Cost |
|------|-----|-----|------|
| GSA SER | Software (mandatory) | gsa-online.de | $99 one-time |
| Scrapebox | Software (proxy scraping) | scrapebox.com | $97 one-time |
| 2Captcha / Anti-Captcha | Service (mandatory) | 2captcha.com | ~$10-20/mo |
| Ahrefs | Monitoring (mandatory) | ahrefs.com | $99+/mo |
| WordAI / Spin Rewriter | Spinner (mandatory) | wordai.com | $47-57/mo |
| Speed-Links.net | Indexer (T3) | speed-links.net | Gratuit start |
| VPS Windows | Infrastructure | Contabo/OVH | $15-25/mo |
| Residential Proxies | Optional/Recomandat | BHW Marketplace | $30-50/mo |

### Cunoștințe prerequisite:

- Basic understanding al link building tiers (T1/T2/T3)
- Acces la cel puțin 2-3 T1 assets active (PBN sau parasite pages)
- Cont Ahrefs cu Site Explorer access
- VPS Windows configurat și accesibil via Remote Desktop (RDP)

---

**DISCLAIMER**: Această procedură descrie tehnici de Grey Hat SEO (Tier 2 automation) utilizate în industria afiliată de gambling. GSA SER și link building automatizat încalcă Google Webmaster Guidelines. Riscurile includ penalizări manuale sau algoritmice pe T1 assets și, în cazuri extreme, pe money site. Procedura este documentată exclusiv în scop educațional și de cercetare SEO avansată. Utilizatorul este singurul responsabil pentru deciziile de implementare și consecințele lor.
