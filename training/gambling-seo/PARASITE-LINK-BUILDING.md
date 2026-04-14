---
type: procedure
created: 2026-03-22
status: active
slug: parasite-link-building
tags: [procedure, nexus]
---

# TRAIN-SEO-006: PARASITE-LINK-BUILDING — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate Parasite Theory + Tiered Link Building -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / Parasite Support / T2 Architecture -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-006
**Scope**: Construcția sistematică a profilului de backlink pentru o pagină parasite de gambling/casino, de la force index inițial până la mentenanță lunară, cu buget și velocitate calibrate pe tipul de domeniu gazdă.

---

## 1. Problema

Cel mai frecvent motiv pentru care paginile parasite nu rankează nu este lipsa de autoritate — este link building prematur sau dezordonat. Construirea de linkuri înainte de indexare risipește buget (potențial 10x mai mult față de necesarul real). Construirea prea rapidă pe un subdomain (medium.com/user/post) declanșează detecție de pattern unnatural și poate elimina pagina din index. Absența unui split de ancore coerent (keyword-focused vs. branded) generează un profil artificial care nu trece de filtrele Google în 2026.

Această procedură rezolvă toate cele trei probleme: secvențierea corectă (index first → wait → build), velocitatea corectă per tip de domeniu gazdă, și mixul de ancore 50/50 dovedit funcțional în piețele Peru / Kenya / Chile.

**Business Application**: Gambling SEO Project (Leo, 2026-03-04) — creșterea vizibilității paginilor parasite casino în piețele low-competition LATAM + Africa.

---

## 2. Procedura

### Pas 1: FORCE INDEX — Obligatoriu Înainte de Orice Link Building

**Aceasta nu este opțională. Niciun link nu se cumpără înainte de completarea acestui pas.**

1. Publică pagina parasite și copiază URL-ul complet (ex: `https://medium.com/@user/best-online-casino-peru-2026-abc123`)
2. Deschide **Google Search Console** → contul asociat domeniului (dacă e disponibil) → URL Inspection → pastează URL-ul → click "Request Indexing"
3. Dacă nu ai acces la GSC pentru domeniu (parasite extern): folosește **Google Search** → operator `site:medium.com/yourpost` pentru a verifica indexarea organică
4. Alternativ rapid: trimite URL-ul prin **Ahrefs Webmaster Tools** → Site Audit → Add URL (acesta crawlează și trimite semnal)
5. **Așteaptă 24-48 ore**

**Decision tree post-așteptare (48h):**

```
Pagina indexată?
├── NU → Verifică: conținut thin? Penalizat contul? Duplicate? → Remediere → Resubmit → Așteaptă încă 24h
└── DA → Verifică poziția curentă în SERP
    ├── Pagina 1-2 (top 20) → STOP link building. Monitorizează 7 zile. Dacă rămâne → mentenanță minimă.
    ├── Pagina 3-5 (poziții 21-50) → Link building MODERAT (50% din bugetul standard)
    └── Pagina 6+ sau nu apare → Link building FULL (bugetul standard complet, Pas 2+)
```

**Rațional**: Piețele Peru, Kenya, Chile au 60-70% mai puțină saturare față de US/UK. Pagini cu conținut solid pe platforme cu DR 90+ (Medium, Reddit) pot sări pe pagina 1-2 pur din autoritatea domeniului gazdă, fără niciun link extern. Fiecare link cumpărat înainte de a testa aceasta este potențial pierdut.

---

### Pas 2: SERP RD AUDIT — Calculul Bugetului Real

Înainte de a stabili bugetul, calculează exact câte Referring Domains (RD) ai nevoie. Nu folosi numere arbitrare.

**Proces:**

1. În **Ahrefs** → SERP Overview → introdu keyword-ul principal al paginii (ex: `"best online casino Peru"`)
2. Exportează top 3 rezultate cu cel mai mare trafic organic (nu neapărat top 3 ca poziție)
3. Pentru fiecare: notează **RD-urile paginii specifice** (nu ale domeniului), nu DR-ul domeniului
4. Calculează:

```
RD Target = (RD_competitor1 + RD_competitor2 + RD_competitor3) / 3 × 1.3

Exemplu Peru/Kenya/Chile (low-competition):
Competitor 1: 8 RD | Competitor 2: 12 RD | Competitor 3: 7 RD
Media: (8+12+7)/3 = 9 RD
Target: 9 × 1.3 = ~12 RD

→ Buget necesar: 10 PBN ($1,000) + 5 Niche Edits ($250) = $1,250 total
```

**Thresholds per piață** (estimative, verifică cu date reale):

| Piață | RD Mediu Competitori | RD Target Tău | Buget Estimat |
|-------|---------------------|---------------|---------------|
| Peru | 8-15 RD | 10-20 RD | $1,000-$1,500 |
| Kenya | 6-12 RD | 8-16 RD | $800-$1,200 |
| Chile | 12-25 RD | 16-33 RD | $1,500-$2,500 |
| Romania | 20-40 RD | 26-52 RD | $2,500-$5,000 |
| UK/US | 80-200+ RD | 104-260+ RD | $10,000+ |

**Regula cheie**: Dacă RD target calculat ≤ 20, ești în low-competition territory. Nu supracumpăra linkuri — riscul de pattern unnatural crește proporțional cu depășirea necesarului.

---

### Pas 3: LINK MIX — Structura Standard per Pagină Parasite

Mixul recomandat pentru un RD target de ~15-20 (tipic Peru/Kenya):

**Componentă A: PBN Links (10 linkuri) — ~$1,000 total ($100/link)**
- Surse: HighRiseLinks, rețeaua PBN proprie, BHW marketplace
- Rol: keyword authority — transmit relevanță topică pentru termenii comerciali
- DR minim per link PBN: DR 25+ cu nișă relevantă (gambling/affiliate/finance/news)
- Fiecare link = domeniu unic (nu 10 linkuri de pe același PBN)

**Componentă B: Niche Edits (10 linkuri) — ~$500 total ($50/link)**
- Surse: outreach direct pe site-uri de gambling review deja indexate, agenții specializate
- Rol: branded anchors — creează un profil natural de citare a URL-ului paginii tale
- Niche Edits sunt inserții în articole existente cu trafic real — mai naturale decât guest posts

**Total investiție standard**: $1,500 + content ($100-200) = **$1,600-$1,700**

**Decision tree buget redus:**

```
Buget disponibil < $1,000?
├── $500-$999 → 5 PBN ($500) + 3 Niche Edits ($150) = 8 RD total
│   └── Potrivit pentru: Kenya (low-comp), unele keyword Peru longtail
├── $200-$499 → 3 PBN ($300) + 2 Niche Edits ($100) = 5 RD total
│   └── Potrivit doar pentru: pagini care au rankat organic deja pe pg 3-4 post-index
└── < $200 → Nu cumpăra linkuri fragmentat. Economisește și execută la minim 5 PBN odată.
```

---

### Pas 4: ANCHOR TEXT STRATEGY — Split 50/50 Obligatoriu

Profilul de ancore este cel mai frecvent mod în care paginile parasite sunt detectate ca artificiale. Regula este simplă: **50% keyword-focused (PBN) + 50% branded (Niche Edits)**.

**Ancore pentru PBN Links (keyword-focused — 10 variante pentru 10 linkuri):**

| # | Anchor Text | Tip |
|---|------------|-----|
| 1 | Best Online Casino Peru | Exact match + geo |
| 2 | Top Casinos Chile 2026 | Exact match + year |
| 3 | Kenya betting sites | Partial match |
| 4 | Medium's top casino reviews | Branded parasite |
| 5 | mejores casinos online Peru | Spanish exact match |
| 6 | casino online confiable Chile | Spanish partial |
| 7 | check out this list | Generic (natural) |
| 8 | best casino bonuses Kenya | LSI keyword |
| 9 | casino sites M-Pesa | Long-tail specific |
| 10 | online casino guide Peru | Generic + geo |

**Reguli de formatare PBN anchors:**
- Capitalizare corectă (nu "best online casino peru" — prea exact-match robotic)
- Mix de exact match, partial match și generic — nu toate 10 identice
- Maximum 2 din 10 pot fi exact-match identic cu keyword-ul principal

**Ancore pentru Niche Edits (branded — 10 variante):**

| # | Anchor Text | Tip |
|---|------------|-----|
| 1 | medium.com | Domain branded |
| 2 | Medium | Brand name |
| 3 | This Medium post | Brand + descriptor |
| 4 | https://medium.com/yourpost | Full URL naked |
| 5 | medium.com/@user | Branded with handle |
| 6 | this article on Medium | Brand + natural |
| 7 | via Medium | Prepoziție + brand |
| 8 | as seen on Medium | Editorial branded |
| 9 | Medium review | Brand + category |
| 10 | more details here | Generic naked |

**Adaptare per platformă:**
- Reddit: `reddit.com/r/onlinecasinos/` | `this Reddit thread` | `r/onlinecasinos`
- Quora: `quora.com` | `this Quora answer` | `Quora's casino guide`
- Sites.Google: `sites.google.com` | `this Google Sites review`

---

### Pas 5: LINK VELOCITY — Calendarul de Construire

Velocitatea greșită este a doua cauză majoră de penalizare după profilul de ancore. Există două regimuri complet diferite:

**Regim A — Subdomain parasite** (ex: `medium.com/user/post`, `reddit.com/r/sub/post`):

```
Velocitate: 1-2 linkuri pe săptămână MAXIM

Săptămâna 1: 1-2 PBN links
Săptămâna 2: 1 Niche Edit + pauză
Săptămâna 3: 1-2 PBN links
Săptămâna 4: 1 Niche Edit
...
Total build spre 15-20 RD: 8-12 săptămâni (2-3 luni)
```

**Regim B — Root domain parasite** (ex: `yourparasitedomain.com` cu DR înalt):

```
Velocitate: 2+ linkuri pe zi acceptabil

Săptămâna 1: 5-6 PBN links + 2-3 Niche Edits
Săptămâna 2: 4-5 PBN links + 2 Niche Edits
Săptămâna 3: 2-3 linkuri mentenanță
Total build spre 15-20 RD: 2-3 săptămâni
```

**NEVER**: 20 RD în 7 zile pe un subdomain. Pattern-ul "spike → drop-off brusc" este detectat automat și produce o penalizare care resetează toată munca.

**Calendar vizual recomandat (subdomain, target 15 RD):**

```
Luna 1:
  Săpt 1: 2 PBN links (ancore: "Best Online Casino Peru" + "mejores casinos Peru")
  Săpt 2: 1 Niche Edit (ancore: "medium.com")
  Săpt 3: 2 PBN links (ancore: "Top Casinos Chile 2026" + "check out this list")
  Săpt 4: 1 Niche Edit (ancore: "This Medium post")
  → Total end of month 1: 6 RD

Luna 2:
  Săpt 5: 2 PBN links
  Săpt 6: 1 Niche Edit + 1 PBN
  Săpt 7: 2 PBN links
  Săpt 8: 1 Niche Edit
  → Total end of month 2: 12 RD

Luna 3:
  Săpt 9-10: 2 PBN + 1 Niche Edit
  Săpt 11-12: Mentenanță — 1-2 linkuri total
  → Total end of month 3: 15-16 RD ✓ TARGET ATINS
```

---

### Pas 6: LOCAL CITATIONS — Autoritate Locală Fără GMB

Pentru piețele LATAM/Africa, semnalele locale adaugă relevanță geografică fără a necesita un Google My Business activ.

**Proces:**

1. Identifică **5-10 adrese de business închis** în orașul țintă (ex: Lima, Nairobi, Santiago) din Google Maps — caută afaceri cu status "Permanently closed"
2. Înregistrează citări pe directoare locale și agregatori de date:
   - **Peru**: páginas amarillas Peru, guialocal.pe, qdq.com/pe
   - **Kenya**: kenyanlist.com, jiji.co.ke (business directory), yellowpages.co.ke
   - **Chile**: páginas amarillas Chile, yapo.cl, infobel.com/cl
   - **General**: yelp.com, foursquare.com (cu locație geo corectă)
3. Datele de înregistrare: Nume business fictiv (nu al tău real), adresa business-ului închis, URL-ul paginii parasite, categoria "Entertainment" sau "Online Services"

**Rezultat**: 5-10 citări locale = +1-2 RD de autoritate locală + semnal NAP (Name-Address-Phone) geo-relevant care confirmă Google că pagina e relevantă pentru piața respectivă.

**Disclaimer**: Folosirea adreselor de business închis pentru citări locale este o practică grey-hat. Înainte de a executa, confirmă că e conform cu strategia proiectului și piața acceptă riscul.

---

### Pas 7: ROI CALCULATION — Validarea Investiției

Înainte de a aproba bugetul final, calculează ROI estimat.

**Formula:**

```
Revenue Lunar Estimat = (Trafic Estimat Lunar ÷ 2%) × Comision Mediu Afiliat

Trafic Estimat = CTR × Search Volume keyword principal
CTR estimat: Poziția 1 = 28% | Poziția 3 = 10% | Poziția 5 = 6% | Poziția 10 = 2%

Exemplu Peru ("best online casino peru", Vol: 1,000/lună):
→ Target: Poziția 3 → CTR 10% → 100 vizite/lună
→ Conversie 2% → 2 utilizatori noi/lună
→ Comision mediu $50/user (CPA) → $100/lună

Exemplu Kenya ("best betting sites Kenya", Vol: 5,000/lună):
→ Target: Poziția 5 → CTR 6% → 300 vizite/lună
→ Conversie 2% → 6 utilizatori noi/lună
→ Comision mediu $30/user → $180/lună
```

**Template ROI per pagină:**

| Variabilă | Valoare |
|-----------|---------|
| Keyword | ___ |
| Search Volume Lunar | ___ |
| Poziție target | ___ |
| CTR estimat (%) | ___ |
| Trafic lunar estimat | ___ |
| Rata conversie (2%) | ___ |
| Users noi/lună | ___ |
| Comision mediu (CPA) | $___ |
| **Revenue lunar estimat** | **$___** |
| Cost total linkuri | $___ |
| Cost conținut | $___ |
| **Total investiție** | **$___** |
| **Breakeven (luni)** | **___** |

**Decision tree ROI:**

```
Breakeven ≤ 1 lună → EXECUȚIE IMEDIATĂ (ROI excellent)
Breakeven 1-3 luni → EXECUȚIE STANDARD (ROI bun, risc acceptabil)
Breakeven 3-6 luni → ANALIZEAZĂ (risc moderat — există competiție nedetectată?)
Breakeven > 6 luni → STOP sau reconsideră keyword target / piață
```

---

### Pas 8: MONITORING LUNAR — Mentenanță și Drop-off Detection

Un profil de linkuri fără mentenanță pierde autoritate. PBN-urile au un drop-off natural de 10-15% per an.

**Proces lunar (prima luni din lună):**

1. **Ahrefs → Site Explorer → pagina parasite** → Referring Domains → filtru "Lost" (last 30 days)
2. Dacă **> 2 RD pierdute în 7 zile** → alert — investigă cauza (PBN penalizat? Link șters?)
3. Dacă total RD a scăzut sub 80% din target → înlocuire imediată (1-2 PBN links noi)
4. Verifică lunar și **anchor text distribution** → dacă exact-match depășește 30% din total → adaugă branded/generic
5. Verifică dacă pagina parasite este în continuare indexată (`site:` operator)

**Tracking spreadsheet (câmp per lună):**

| Dată | Total RD | RD Noi | RD Pierdute | Poziție SERP | Revenue Estimat | Acțiune |
|------|----------|--------|-------------|--------------|-----------------|---------|
| 2026-03 | ___ | ___ | ___ | ___ | $___ | ___ |
| 2026-04 | ___ | ___ | ___ | ___ | $___ | ___ |

**Thresholds de alertă:**
- RD scăzut cu > 2 în 7 zile → investigare imediată
- Poziție căzută cu > 5 locuri în 14 zile → audit: pierdut linkuri? Competitor construit autoritate?
- Pagina deindexată → stop orice link building, identifică cauza

---

## 3. Cortex Logging

```json
{
  "text": "Parasite Link Building executat pentru [URL pagină] — piața [COUNTRY]. Keyword: [keyword principal]. RD build: [N] total. Mix: [N] PBN + [N] Niche Edits. Buget: $[X]. ROI estimat: $[Y]/lună. Breakeven: [Z] luni. Velocitate: [SLOW/FAST] (tip domeniu: subdomain/root). Force index test: [PASSED/PENDING]. Poziție post-build: [P].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PARASITE-LINK-BUILDING",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-006",
    "tags": ["parasite-seo", "link-building", "pbn", "niche-edits", "gambling", "casino", "peru", "kenya", "chile", "anchor-text", "force-index", "roi"],
    "has_enforcement_loop": true,
    "version": "2.0",
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + Pre-build orice pagină parasite gambling nouă

### WHEN
La fiecare campanie de link building pe o pagină parasite nouă sau la repornirea unui build oprit. Minim o execuție per pagină per piață, înainte de orice achiziție de link.

### HOW (violation detection)
- Link building executat fără Force Index Test completat → **violation critică** (waste de buget)
- RD target stabilit fără SERP RD Audit (Pas 2) → violation
- Anchor text split non-50/50 (ex: 80% exact-match) → violation
- Velocitate > 2 RD/zi pe subdomain parasite → violation
- 20+ RD construiți în 7 zile → violation critică
- ROI calculation absent înainte de aprobare buget → violation
- Monitoring lunar absent > 45 zile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise**: Cumpărarea de linkuri înainte de Force Index Test, construirea mai mult de 2 RD/săptămână pe subdomain, folosirea de ancore 100% exact-match, ignorarea drop-off-ului lunar.

### CONNECT
- TRAIN-SEO-006 → enforcement link building parasite SEO gambling
- TRAIN-SEO-001 (CASINO-MARKET-ANALYSIS) → prerequisit: RD target vine din Pas 5 al analizei de piață
- M-013 (Off-Page SEO and Link Building Campaign) → procedura generală din care aceasta e specializarea gambling/parasite
- `procedure-health.json` → adaugă entry
- Training Mode → curriculum SEO specializat nișă gambling, nivel avansat
- Outputs alimentează: tracking ROI, content brief parasite, expansie la noi piețe

### VERIFY
La finalul fiecărei execuții, verifică:
- [ ] Force Index Test completat și rezultat documentat (Pas 1)?
- [ ] SERP RD Audit finalizat și RD target calculat cu formula × 1.3 (Pas 2)?
- [ ] Link mix stabilit: N PBN + N Niche Edits (Pas 3)?
- [ ] Anchor text list completă (10 PBN + 10 branded) fără > 30% exact-match (Pas 4)?
- [ ] Calendar de velocitate corect per tip domeniu — subdomain SLOW / root FAST (Pas 5)?
- [ ] Local citations planificate sau excluse cu justificare (Pas 6)?
- [ ] ROI calculation complet cu breakeven ≤ 3 luni pentru aprobare (Pas 7)?
- [ ] Monitoring tracker setat și prima coloană completată (Pas 8)?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PARASITE-LINK-BUILDING | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Parasite Page Link Building — Gambling SEO" | FORGE ✓ | rule: TRAIN-SEO-006 | v1.0`

### MODEL ROUTING

| Activitate | Model |
|-----------|-------|
| Execuție procedură + ROI calc | Sonnet (Genie) |
| SERP interpretation + strategy | Opus |
| PBN anchor text review | Sonnet |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Search Console | Force Index Test — URL Inspection + Request Indexing (Pas 1) |
| Ahrefs | SERP RD Audit competitori (Pas 2), monitoring lunar referring domains (Pas 8), anchor distribution check |
| HighRiseLinks | Sursă primară PBN links ($100/link, DR 25+) |
| BHW Marketplace (BlackHatWorld) | Sursă alternativă PBN + Niche Edits, peer reviews de furnizori |
| Rețea PBN proprie | Linkuri controlate pentru anchors exact-match critice (dacă disponibilă) |
| Site-uri gambling review (outreach) | Sursă Niche Edits branded ($50/link, inserții în articole existente) |
| Google Maps | Identificare adrese business închis pentru local citations (Pas 6) |
| Directoare locale (per piață) | Construire citări locale — páginas amarillas, kenyanlist, jiji.co.ke |
| Spreadsheet tracking | Monitoring lunar RD, poziție SERP, revenue estimat, acțiuni (Pas 8) |
| CASINO-MARKET-ANALYSIS (TRAIN-SEO-001) | Prerequisit — furnizează RD target inițial + keyword principal + decizia ENTER |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Force Index Test executat | 100% înainte de orice link cumpărat |
| Completion rate pași 1-8 | 100% per campanie |
| VK emisie | 2/2 per execuție |
| RD build time (subdomain) | 15-20 RD în 10-12 săptămâni |
| Anchor text split | 45-55% keyword / 45-55% branded (toleranță ±5%) |
| Velocitate subdomain | ≤ 2 RD/săptămână |
| Velocitate root domain | ≤ 10 RD/săptămână |
| ROI breakeven | ≤ 3 luni pentru aprobare campanie |
| Monitoring gap | ≤ 30 zile între verificări |
| RD drop-off alertă | > 2 RD pierdute în 7 zile → acțiune imediată |
| Total investiție per pagină (Peru/Kenya) | $1,000-$1,700 |
| Revenue target per pagină (minim viabil) | $500/lună la 3 luni post-build |
