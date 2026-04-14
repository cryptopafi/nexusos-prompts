---
type: procedure
created: 2026-03-20
status: active
slug: casino-affiliate-setup
tags: [procedure, nexus]
---

# TRAIN-SEO-009: CASINO-AFFILIATE-SETUP — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate Affiliate Strategy + Casino Industry Monetization -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Monetization / Affiliate Setup / Revenue Optimization -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-009
**Scope**: Setup complet al unui sistem de afiliere casino — de la înregistrarea brand-ului phantom la aplicarea la programe, optimizarea conversiei și scalarea la venituri pasive lunare recurente.

---

## 1. Problema

Majority of new affiliates apply to a single casino program, use raw affiliate links in content, and skip phantom brand setup — resulting in: account bans (zero footprint violations), commission theft through link hijacking, low approval rates from programs requiring professional presence, and single-revenue-stream fragility. This procedure standardizes the full affiliate stack: phantom brand creation, simultaneous multi-program application, link masking, content conversion optimization, and monthly tracking audit. Without this structure, a $1K article investment fails to convert at the $750/mo target rate.

**Piețe țintă principale**: Peru (Inkabet Partners, Betsson Peru, Codere Peru), Kenya (Betway Partners Africa, 1xBet Partners), Chile (Betsson Chile, offshore pre-regulation)

**Business Application**: Gambling SEO Project (Leo, 2026-03-04) — monetizarea traficului organic generat prin parasite SEO și money sites în piețele LATAM/Africa.

---

## 2. Procedura

### Pas 1: PHANTOM BRAND SETUP (MANDATORY — zero footprint)

Phantom brand = identitate separată de identitatea reală a operatorului. Nu există legătură directă între brand și persoana fizică. Execuție obligatorie ÎNAINTE de orice aplicație la program de afiliere.

**1.1 — Alegerea numelui de brand**

Alege un nume care: (a) include keyword-ul principal al pieței, (b) sună profesional și de încredere, (c) este disponibil ca domeniu .com.

Formule testate:
- `[tara]bestcasinos.com` → ex: `perubestcasinos.com`
- `top[tara]casino.com` → ex: `topkenyacasino.com`
- `casino[tara]guide.com` → ex: `casinochileguide.com`
- `[keyword][tara].com` → ex: `mejorcasinoperu.com`

Verificare disponibilitate domeniu: Namecheap → Domain Search. Target cost: $10-15/an pentru `.com`. Evită `.net`, `.org` — mai puțin trustworthy în nișa casino.

**1.2 — Checklist Phantom Brand Setup**

```
PHANTOM BRAND SETUP CHECKLIST
===============================
[ ] Domeniu .com înregistrat pe Namecheap ($10-15)
    → Folosește date reale (necesare pentru WHOIS) DAR activează WhoisGuard (gratuit pe Namecheap)
    → Nu folosi date personale directe dacă posibil — înregistrează pe entitate separată

[ ] Website one-page creat
    → Namecheap Site Maker: 14 zile gratuit, apoi $3.88/mo
    → Alternativă: WordPress + Astra Theme (hosting $3-5/mo pe shared)
    → Conținut minim pagina principală:
        - Logo profesional (Canva, 30 minute)
        - Tagline: "Your trusted [piata] casino guide"
        - About section: 2-3 paragrafe despre misiunea site-ului
        - Contact section: email de brand
        - Privacy Policy + Terms (generator gratuit: termsofservicegenerator.net)
    → Standard vizual minim: nu trebuie să fie perfect, trebuie să pară legitim

[ ] Email branded creat
    → Format: info@[brand].com sau editor@[brand].com
    → Setup pe Namecheap Private Email ($1/mo) sau Google Workspace ($6/mo)
    → Alternativă gratuită: forward prin Cloudflare Email Routing → Gmail personal
    → FOLOSIT EXCLUSIV pentru aplicații la programe afiliate

[ ] Profil LinkedIn creat
    → Nume: "[Brand Name] Editorial Team" sau "[Brand Name] Media"
    → Completează: Industry (Online Media), About, Website
    → Adaugă 2-3 conexiuni de bază (prieteni/cunoștințe)
    → Avatar: logo de brand sau stock photo profesional

[ ] Profil Twitter/X creat
    → Handle: @[brandname] sau @[brandname]casino
    → Bio: "Expert casino reviews for [piata]. Responsible gambling. 18+"
    → Postează 3-5 tweets înainte de aplicare (retweet știri industrie)

[ ] Opțional dar recomandat:
    [ ] Pagină Facebook de brand (crește credibilitatea cu +20%)
    [ ] Profil Crunchbase (free tier — apare în Google searches ale brand-ului)
```

**Timp estimat setup**: 4-6 ore prima dată. Template reutilizabil pentru piețe noi: 1-2 ore.

---

### Pas 2: LINK MASKING SETUP (MANDATORY)

NICIODATĂ link-uri raw de afiliat în conținut. Motivele:
1. Comision theft: utilizatorii pot modifica parametrii link-ului
2. Trust factor: link-urile raw arată spam (ex: `betsson.com?ref=abc123&tid=xyz`)
3. CTR: link-urile branded convertesc cu 15-30% mai bine
4. Tracking: poți vedea click-urile per link, nu doar per program

**2.1 — Alegerea platformei de link masking**

| Platformă | Cost | Recomandare |
|-----------|------|-------------|
| Rebrandly | $15/mo | Best pentru branded links cu custom domain |
| Short.io | $10/mo | Alternativă solidă, API mai bun |
| Pretty Links (WordPress plugin) | $79/an | Best dacă ai money site pe WP |
| ThirstyAffiliates (WP) | $99/an | Best pentru disclosure + management pe WP |

**Recomandare**: Rebrandly pentru parasite SEO (ai nevoie de links independente de site). ThirstyAffiliates pentru money sites WordPress.

**2.2 — Structura link-urilor branded**

Format standard:
```
[brand-domain].com/[operator-name]
```

Exemple concrete:
```
perubestcasinos.com/inkabet      → redirect → link afiliat Inkabet
perubestcasinos.com/betsson      → redirect → link afiliat Betsson
perubestcasinos.com/codere       → redirect → link afiliat Codere
topkenyacasino.com/betway        → redirect → link afiliat Betway Partners
topkenyacasino.com/1xbet         → redirect → link afiliat 1xBet Partners
```

Setup în Rebrandly:
1. Add custom domain (branded domain-ul tău)
2. Create link → Destination: link afiliat raw → Short URL: /inkabet
3. Enable tracking (clicks, referrer, device)
4. Test: click link → verifică redirect corect + parametrii afiliați intacți

---

### Pas 3: APLICAREA LA PROGRAME DE AFILIERE

**3.1 — Lista programelor per piață**

**PERU:**
- Inkabet Partners: `partners.inkabet.pe` — auto-approve parțial, rev share 25-30%
- Betsson Peru: `betssonaffiliates.com` — manual review 5-7 zile, CPA $80-150
- Codere Peru: `codereafiliados.com` — manual review, hybrid disponibil
- 1xBet Partners: `1xbet-partners.com` — auto-approve, rev share 25%, global

**KENYA:**
- Betway Partners Africa: `partners.betway.co.ke` — manual review, rev share 20-25%
- 1xBet Partners: `1xbet-partners.com` — auto-approve (aceeași platformă globală)
- SportPesa: verifică disponibilitate program — intermitent open/closed
- Betika Kenya: `betika.com/en-ke/affiliate` — local operator, rev share 20%

**CHILE:**
- Betsson Chile: `betssonaffiliates.com` — aceeași platformă ca Peru, CPA $100-200
- Coolbet (GIG Partners): `gigpartners.com` — rev share 25-35%, quality operator
- Jonbet: offshore, auto-approve, rev share 30%

**REȚELE AFILIATE (opțional — pentru volume):**
- Income Access: rețea specializată gambling, 100+ operatori
- Gambling.com Group: direct deals, premium operatori
- Catena Media: agregator cu deal-uri negociate

**3.2 — Template Aplicație Program Afiliere**

Folosește acest template pentru fiecare aplicație. Adaptează `[variabilele]` per program.

```
TEMPLATE APLICAȚIE — PROGRAM AFILIERE CASINO
=============================================

CÂMPURI STANDARD ÎN FORMULAR:

Website URL: https://[brand-domain].com
Website Name: [Brand Name]
Monthly Traffic (estimate): 5,000-10,000 visitors (conservator la început)
Traffic Sources: Organic Search (SEO), Content Marketing
Primary Market/GEO: [Peru / Kenya / Chile]
Primary Language: [Spanish / English / Spanish]
Contact Email: info@[brand-domain].com
Skype/Telegram (dacă solicitat): [telegram personal OK]

CÂMP "Tell us about your website / How do you plan to promote?":
---
[Brand Name] is a dedicated casino review platform targeting [piata] players.
We produce in-depth, unbiased casino reviews, bonus comparisons, and responsible
gambling guides. Our content strategy focuses on high-intent SEO keywords
("best online casino [piata]", "[operator] review [piata]", etc.).

Traffic acquisition: 100% organic search via long-form content (2,000-4,000 words
per review). We target [Spanish/English]-speaking users in [country] specifically.

Current content: [X] published reviews. Publishing cadence: 4-6 articles/month.
We maintain strict editorial standards and include responsible gambling notices
on all content per [country] regulations.

We are applying for Revenue Share model (preferred) and are open to discussing
Hybrid arrangements for high-volume performance.
---

CÂMP "Commission Model Preference":
Revenue Share preferred. Open to Hybrid (CPA + Rev Share) for established programs.

CÂMP "How did you hear about us?":
Industry research / Affiliate forum (use generic answer)
```

**3.3 — Strategia de aplicare simultană**

Aplică la **5-15 programe simultan** (nu secvențial):
- Ziua 1: completează toate formularele în aceeași sesiune de 2-3 ore
- Ziua 1-3: programele cu auto-approve trimit credențiale imediat
- Ziua 5-7: programele cu manual review răspund
- Ziua 8: audit — câte aprobări, care au respins și de ce

**Rate de aprobare așteptată**:
- Auto-approve programs: 100% (1xBet, unii offshore)
- Manual review cu website profesional: 70-80%
- Manual review fără website: 20-30%

Dacă ești respins: cere feedback specific ("What improvements would make us eligible?"). Majoritatea programelor explică — fixează și re-aplică după 30 zile.

---

### Pas 4: CONTENT CONVERSION OPTIMIZATION

**4.1 — Structura de plasare a produselor afiliate**

```
ARTICOL STANDARD (2,000-4,000 cuvinte):
=========================================

[TITLU] — ex: "Top 5 Casino Online Peru 2026 — Bonus + Review Complet"

[INTRO — 150-200 cuvinte]
→ Hook + problema utilizatorului + promisiunea articolului
→ Include CTA subtil: "Cel mai bun bonus al lunii: [Operator #1] →"

[PRODUSE AFILIATE — TOP OF CONTENT, PRIMELE 3-4 PARAGRAFE]
→ Operator #1 (cel mai bun comision sau cel mai căutat): 3-4 paragrafe dedicate
   - Bonus exact: "S/. 500 bonus la primul depozit + 50 rotiri gratuite"
   - Cerințe de rulaj (wagering): "30x pe bonus" — fi transparent
   - Metode de plată: "Visa, Mastercard, Yape, Plin, criptomonede"
   - Depozit minim: "S/. 20"
   - CTA button: "Revendică Bonusul S/. 500 →" (nu "Click Here")

→ Operator #2: 3-4 paragrafe (același format)
→ Operator #3: 3-4 paragrafe (același format)

[CONTENT DE SUPORT — MIJLOC]
→ Cum alegi un casino online în [tara]
→ Ce înseamnă wagering requirements
→ Metode de plată locale (Yape/Plin pentru Peru, M-Pesa pentru Kenya)
→ Jocuri disponibile (slots, live dealer, sports)
→ Responsible gambling notice (OBLIGATORIU)

[PRODUSE NON-AFILIATE / COMPARAȚII — FINAL]
→ Menționate scurt, fără detalii extinse
→ Nu le da aceeași proeminență ca produsele afiliate

[FAQ — 5-8 întrebări]
→ Target: featured snippets + voice search
→ Schema markup: FAQPage

[FOOTER LEGAL]
→ "Conținut sponsorizat / Affiliate disclosure"
→ "Joacă responsabil. 18+. Gambling poate crea dependență."
→ Link către organizații de ajutor: ex: Jugarbien.pe (Peru)
```

**4.2 — Checklist Optimizare Conversie Per Articol**

```
CONVERSION OPTIMIZATION CHECKLIST
====================================
[ ] Produse afiliate plasate în primele 3-4 paragrafe (above the fold pe mobile)
[ ] Fiecare produs afiliat: minim 3 paragrafe dedicate (nu 1-2 propoziții)
[ ] Bonus specificat EXACT (sumă + rotiri + currency local)
[ ] Wagering requirements menționate transparent (builds trust = more conversions)
[ ] Metode de plată locale incluse (Yape/Plin/M-Pesa — hyper-relevant per market)
[ ] CTA buttons: text specific ("Revendică S/. 500" nu "Click Here")
[ ] CTA buttons: culoare contrastantă (verde sau portocaliu, nu gri)
[ ] Link-uri afiliate: TOATE mascate prin Rebrandly/ThirstyAffiliates
[ ] Minim 3 programe afiliate per articol (3 revenue streams)
[ ] Affiliate disclosure prezentă (deasupra fold sau în intro)
[ ] Responsible gambling notice prezentă
[ ] Schema markup: Review + FAQPage + BreadcrumbList
[ ] Internal links: 3-5 link-uri spre alte review-uri de pe site
[ ] Imagini: screenshot-uri reale din casinourile recenzate (trust signal)
[ ] Update date: "Ultima actualizare: [Luna] [An]" — arată freshness
```

**4.3 — Regula celor 3 programe per articol**

Fiecare articol monetizează cu EXACT 3 programe afiliate (nu mai puțin, nu mai mult pentru articolele standard):
- Program #1: Best rev share deal (cel mai bun potențial pe termen lung)
- Program #2: Best CPA deal (venit imediat per conversie)
- Program #3: Best brand recognition (cel mai cunoscut operator local — conversion rate mai mare)

Excepție: articolele de tip "Top 10" pot include 5-10 programe cu paragrafe mai scurte per operator.

---

### Pas 5: TRACKING & REVENUE AUDIT LUNAR

**5.1 — Dashboard tracking per program**

Fiecare program afiliat oferă un dashboard propriu. Setează reminder lunar (prima zi a lunii) pentru audit sistematic:

```
MONTHLY AFFILIATE AUDIT — TEMPLATE
=====================================
Data audit: [YYYY-MM-DD]
Piață auditată: [Peru / Kenya / Chile]

Per program:
| Program      | Clicks | Registrations | Deposits | Revenue | Conv Rate | Status |
|--------------|--------|---------------|----------|---------|-----------|--------|
| Inkabet      |        |               |          |         |           |        |
| Betsson      |        |               |          |         |           |        |
| Codere       |        |               |          |         |           |        |
| 1xBet        |        |               |          |         |           |        |
| TOTAL        |        |               |          |         |           |        |

Acțiuni rezultate:
- PAUSE: programe cu <0.5% conversie registrations/clicks după 90+ zile
- SCALE: programe cu >2% conversie → mai mult conținut dedicat
- NEGOTIATE: programe cu revenue >$500/mo → cere upgrade commission tier
- REPLACE: programe pauzate → înlocuiește cu alternativă din aceeași piață
```

**5.2 — Revenue Streams Paralele**

Pe lângă comisioanele afiliate, monetizează traficul și prin:

**Link Selling (Internal Placements)**:
- Vinde plasamente de link-uri interne pe articolele cu DR ridicat
- Preț: $100-500 per link (depinde de DR-ul paginii)
- IMPORTANT: NU mixa link-uri vândute cu affiliate links în același articol (confuzionează tracking-ul și dilueaza link equity)
- Ține un spreadsheet separat pentru link placements vândute

**Display Ads**:
- Ezoic sau Mediavine (necesită 10K/50K sesiuni/lună)
- Revenue secundar, nu interfere cu affiliate links
- Plasează ads în sidebar sau între secțiuni, niciodată în locul CTA-urilor afiliate

**5.3 — Scale Formula (Floate Model)**

Modelul de scalare validat pentru piețele țintă:

```
PER ARTICOL (DR85 news site / parasite SEO):
- Trafic organic: 500 vizitatori/lună (conservator)
- Rata de conversie clic afiliat: 3%
- Click-uri afiliate: 15/lună
- Rata conversie registrare: 20% (3 click-uri → 1 înregistrare = 3 înregistrări)
- Rata conversie depozit: 50% (3 înregistrări × 50% = 1.5 depozitanți)
- Comision CPA mediu: $50-100 per depozitant
- Revenue lunar per articol: ~$75-150/lună
- Cost articol (scriere + link building): ~$1,000
- Breakeven: 7-13 luni
- Profit după 24 luni: $800-2,600 (net, per articol)

SCALE:
- 20 articole × $750/mo medie = $15,000/mo
- Minus costuri (scriere $2K + link building $3K + tools $500): net $9,500/mo
- Orizont: 18-24 luni pentru 20 articole rankate
```

---

### Pas 6: LINK SELLING SETUP (Revenue Stream Secundar)

**Separare strictă față de afiliere**: link selling = vânzare de plasamente editoriale pe site-urile/paginile tale cu autoritate. Nu este afiliere și nu trebuie amestecat.

**Cum vinzi link-uri**:
1. Identifică paginile tale cu DR ridicat și trafic organic dovedit
2. Listează pe marketplace-uri: Linkody, The Hoth, Loganix, sau direct pe Slack/forum comunități SEO
3. Preț: calculează ca 10-15% din DR-ul paginii în dolari (ex: DR45 → $45-67/link)
4. Tipuri de link-uri acceptate: review-uri casino non-competitive, tool-uri gambling, resurse responsible gambling
5. REFUZĂ: link-uri spre scam casinos, site-uri fără licență, sporturi interzise în piața ta

**Tracking**: Google Sheet separat cu: cumpărător, URL pagina, anchor text, data publicare, valoare, data expirare (dacă e cazul).

---

## 3. Cortex Logging

```json
{
  "text": "Casino Affiliate Setup executat pentru piața [COUNTRY]. Brand: [brand-domain]. Programs applied: [N]. Programs approved: [N]. Commission model: [RevShare/CPA/Hybrid]. Link masking: [Rebrandly/ThirstyAffiliates]. Content live: [N] articole. Revenue lunar: $[X]. Top performer: [program] — $[Y]. Breakeven status: [on-track/behind].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CASINO-AFFILIATE-SETUP",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-009",
    "tags": ["casino-affiliate", "phantom-brand", "link-masking", "affiliate-programs", "monetization", "gambling", "peru", "kenya", "chile", "revenue-share", "cpa"],
    "has_enforcement_loop": true,
    "version": "2.0",
    "forge_version": "2.0"
  }
}
```

**Skill saves suplimentare** (la milestone-uri):
- La setup phantom brand: `[Skill saved: "Phantom brand setup [piata] — [brand-domain]"]`
- La prima aprobare program: `[Skill saved: "Affiliate approval — [program] — commission: [tip + %]"]`
- La primul revenue generat: `[Skill saved: "First affiliate revenue — [program] — $[suma] — [luna]"]`
- La audit lunar: `[Skill saved: "Affiliate audit [Luna YYYY] — top performer: [program] — $[suma]"]`

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție + La orice task cu scope "affiliate setup", "program application", "monetization" pentru piețele Peru/Kenya/Chile.

### WHEN
La fiecare setup de phantom brand nou. La fiecare aplicație la program de afiliere. La fiecare publicare de conținut cu linkuri afiliate. La audit lunar (prima zi a lunii).

### HOW (violation detection)
- Aplicare la program fără phantom brand complet (GATE 1) → **violation critică** (risc ban cont)
- Publicare conținut cu raw affiliate links (fără link masking, GATE 2) → **violation critică**
- Aplicare la < 5 programe simultan (GATE 3) → violation
- Email personal folosit pentru aplicații (în loc de email branded) → violation
- Articol cu < 3 programe afiliate → violation
- Produse afiliate plasate la final (nu above-the-fold, GATE 4) → violation
- Affiliate disclosure sau responsible gambling notice absente → violation
- Audit lunar absent > 45 zile (GATE 5) → violation
- Revenue tracking absent (Google Sheet necreat) → violation

**Gate checklist** (blocant -- nu continua fara bifat):

**GATE 1 -- Phantom Brand**:
- [ ] Domeniu inregistrat + WhoisGuard activ
- [ ] Website live (chiar si one-page)
- [ ] Email branded functional (test: trimite email test si primeste raspuns)
- [ ] Minim LinkedIn + Twitter/X create
- DACA oricare lipsa: STOP. Nu aplica la programe fara brand setup complet.

**GATE 2 -- Link Masking**:
- [ ] Rebrandly / ThirstyAffiliates configurat cu custom domain
- [ ] Test link creat si testat (click -> redirect corect cu parametrii afiliati intacti)
- DACA lipsa: STOP. Nu publica continut cu raw affiliate links.

**GATE 3 -- Program Application**:
- [ ] Aplica la MINIM 5 programe simultan (nu 1-2)
- [ ] Folosesti emailul de brand (nu email personal)
- [ ] Template aplicatie completat specific per program (nu copy-paste generic)

**GATE 4 -- Content**:
- [ ] Fiecare articol: minim 3 programe afiliate
- [ ] Produse afiliate: primele 3-4 paragrafe (nu la final)
- [ ] CTA buttons cu text specific (nu "Click Here")
- [ ] Affiliate disclosure prezenta
- [ ] Responsible gambling notice prezenta

**GATE 5 -- Tracking**:
- [ ] Fiecare program: dashboard accesat si bookmark-uit
- [ ] Reminder lunar setat pentru audit (prima zi a lunii)
- [ ] Google Sheet revenue tracking creat

### CONNECT
- TRAIN-SEO-009 → enforcement casino affiliate setup gambling SEO
- TRAIN-SEO-001 (CASINO-MARKET-ANALYSIS) → prerequisit: piata selectata + decizie ENTER
- TRAIN-SEO-006 (PARASITE-LINK-BUILDING) → link building pentru rankare articole afiliate
- TRAIN-SEO-010 (AI-CONTENT-CASINO) → content creation pipeline pentru articolele afiliate
- `procedure-health.json` → adauga entry
- Training Mode → curriculum monetizare gambling SEO
- MEM-H-002 → skill saving obligatoriu la milestone-uri

### VERIFY
La finalul fiecarei executii, verifica:
- [ ] Phantom brand complet: domeniu + website + email + social (Pas 1)?
- [ ] Link masking configurat si testat cu redirect corect (Pas 2)?
- [ ] Minim 5 programe aplicate simultan cu template specific (Pas 3)?
- [ ] Continut optimizat: 3 programe/articol, CTA specific, disclosure (Pas 4)?
- [ ] Tracking configurat: dashboards + reminder lunar + Google Sheet (Pas 5)?
- [ ] Link selling separat de afiliere (daca aplicabil, Pas 6)?
- [ ] VK emis in sesiune?

**VK-uri obligatorii:**
1. `[PROC] CASINO-AFFILIATE-SETUP | S1 S2 S3 S4 VER | complete`
2. `[CORTEX] "Casino Affiliate Program Setup — Gambling SEO" | FORGE | rule: TRAIN-SEO-009 | v1.0`

**Frecventa review-ului procedurii**: La fiecare audit lunar (simultan cu Pas 5). Update versiune daca comisioanele sau programele se schimba semnificativ.

---

## 5. Dependențe

**Precedente (trebuie completate ÎNAINTE de această procedură)**:
- `CASINO-MARKET-ANALYSIS.md` (TRAIN-SEO-001): piața țintă selectată + decizie ENTER
- Keyword research finalizat pentru piața aleasă (minim 20 target keywords)
- Strategie de conținut decisă (parasite SEO / money site / hibrid)

**Post-execuție (ce se deblochează după această procedură)**:
- Content creation workflow (scriere articole cu structura de conversie de la Pas 4)
- Link building campaign (pentru a ranka articolele și activa traficul)
- PBN / parasite SEO deployment (dacă strategia aleasă include aceste tactici)

**Tools necesare**:
| Tool | Scop | Cost | Alternativă gratuită |
|------|------|------|----------------------|
| Namecheap | Domeniu + hosting brand | $10-15/an domeniu | N/A |
| Rebrandly | Link masking | $15/mo | Pretty Links (WP, paid) |
| Google Sheets | Revenue tracking | Gratuit | Airtable (free tier) |
| Canva | Logo brand | Gratuit | N/A |
| Namecheap Site Maker | Website brand | $3.88/mo | WordPress pe shared hosting |
| Namecheap Private Email | Email brand | $1/mo | Cloudflare Email Routing (gratuit) |

**Regulile asociate**:
- SEC-VET-001: Vetting securitate înainte de orice script de tracking extern
- TRAIN-SEO-001 → TRAIN-SEO-009: Procedura curentă este step 9 în seria de gambling SEO
- MEM-H-002: Skill saving obligatoriu la fiecare milestone (brand setup, prima aprobare, primul revenue)

**Contact / Suport programe**:
- Betsson Affiliates: affiliates@betssongroup.com
- 1xBet Partners: partners@1xbet-partners.com
- Betway Africa: africanaffiliates@betway.com

---

**DISCLAIMER**: Această procedură descrie tehnici de affiliate marketing în industria gambling/casino, inclusiv crearea de phantom brands și aplicarea la programe de afiliere. Gambling online are statut legal diferit în fiecare jurisdicție (Peru - reglementat, Kenya - reglementat BCLB, Chile - grey market). Verifică legislația locală înainte de a opera în orice piață. Procedura este documentată în scop educațional și de cercetare affiliate marketing. Utilizatorul este singurul responsabil pentru conformitatea cu reglementările locale și consecințele implementării.

---

*Procedură validată pentru piețele Peru, Kenya, Chile — 2026-03-04. Review la schimbări legislative sau la modificări ale structurilor de comisioane.*
