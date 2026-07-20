---
type: procedure
created: 2026-03-20
status: active
slug: m-132-expired-domain-acquisition
tags: [procedure, nexus]
---

# M-132 — Expired Domain Acquisition for PBN & 301 Redirects

**Domeniu**: affiliate | **Sub-domeniu**: link-building-pbn
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Conectat la**: M-127 (Programmatic SEO), M-117 (Competitive Intelligence), seo-2026-full-spectrum.md

---

## §1 — PROBLEMA

Site-urile afiliate de gambling au nevoie de backlink-uri pentru a ranka — achizița naturală de linkuri este lentă (6–12 luni) și scumpă ($300–2,000/link editorial).

**Avantajul economic al domeniilor expirate**:
- PBN Tier 1: $50–150/domeniu + $15–25/lună hosting versus $300–2,000/link editorial → avantaj de cost 10–20×
- 301 redirect de pe domeniu DR ridicat: boost instant de DR pe money site
- Fondarea autorității necesare pentru rankings pe termeni gambling competitivi

**Problemele fără protocol sistematic**:
- Supra-plată pe domenii slabe (fără verificare TF/CF)
- Achiziție domenii penalizate (fără verificare Wayback Machine) → risc penalizare Google pe money site
- Footprint PBN detectabil (same hosting, same themes) → deindexare în masă
- Anchor text exact match excesiv → penalizare Penguin

**Validarea strategiei**: seo-2026-full-spectrum.md confirmă că achizița domeniilor expirate rămâne viabilă în 2026 cu măsuri anti-footprint corecte.

---

## §2 — PROCEDURA

### Pas 1: SETUP UNELTE & SURSARE DOMENII

**Unelte de descoperire domenii** (configurează toate 4):

| Unealtă | Cost | Rol Principal | Prioritate |
|---------|------|---------------|------------|
| DomCop | $67/lună | Filtrare TF/CF, baza de date cea mai mare | Primară |
| Spamzilla | $47/lună | Detectare spam, domenii deindexate | Secundară |
| GoDaddy Auctions | Gratuit (browsing) | Volume, licitații care se încheie curând | Terțiară |
| ExpiredDomains.net | Gratuit | Baza de date mare, filtrare inițială | Inițial |

**Configurare filtre DomCop** (setează o singură dată, salvează ca preset):
```
TF: ≥15
CF: ≥10
DR: ≥20
Referring Domains: ≥20
Domain Age: ≥2 ani
Spam Score: ≤30%
Language: Spanish, English
Status: Expired OR Expiring (next 30 days)
Category: Exclude adult, pharma, gambling (dacă nu cauti tematic relevant)
```

**Workflow scanare zilnică** (15 minute/zi):
1. Deschide DomCop → tab "New Expired" filtrat cu presetul de mai sus
2. Exportă CSV cu candidații noi din ziua curentă
3. Adaugă în coloana "Review Status" = "PENDING"
4. Setează alertă DomCop pentru domenii cu TF≥20 și DR≥35 (notificare email)

**Output**: `domain-candidates-{YYYY-WW}.csv` — export DomCop săptămânal: Domain | TF | CF | DR | RDs | Age | Price | Source | Language | Status
**Checkpoint**: minimum 20 candidați revizuiți săptămânal; unelte de scanare configurate și operaționale

---

### Pas 2: CALIFICARE MANUALĂ

**Pentru fiecare candidat din coada DomCop**, execută toate cele 6 verificări:

**Verificarea 1: Majestic TF/CF (date actualizate)**
- DomCop poate afișa date cu întârziere → verifică direct pe majestic.com
- Confirmă: TF ≥ 15 (PBN) sau ≥ 25 (301)
- Confirmă: TF/CF ≥ 0.5 (linkuri de calitate, nu spam)
- Profilul de linkuri: verifică că TF vine din domenii cu TF propriu ≥ 15, nu de pe 1000 de site-uri TF1

**Verificarea 2: Ahrefs DR + Profil Referring Domains**
- DR ≥ 20 (PBN) sau ≥ 40 (301)
- Unique RDs ≥ 20 (PBN) sau ≥ 50 (301)
- Verifică velocitatea de linkuri: linkuri adăugate treptat de-a lungul timpului vs spike brusc (spike = semn de manipulare)
- Red flag: >50% din RDs de pe același TLD sau din aceeași rețea de hosting

**Verificarea 3: Wayback Machine** (OBLIGATORIU — cel mai important check)
- Deschide web.archive.org și introdu domeniul
- Verifică: a fost vreodată site de gambling/casino? → ideal pentru relevanță, neutru pentru PBN
- Verifică: a avut vreodată conținut spam, farmaceutic, adult, sau ilegal? → REJECT dacă da
- Verifică: a existat vreodată o notificare de penalizare manuală Google vizibilă în conținut?
- Verifică ultimii 5 ani: schimbări bruște de nișă (semn de domain flipping — risc ridicat)

**Verificarea 4: Indexare Google curentă**
```
Caută în Google: site:domeniu.com
```
- Rezultate indexate → PASS
- Zero rezultate = domeniu deindexat → REJECT (red flag major)
- Verificare adițională: `"domeniu.com" site:google.com/search` → caută știri de penalizare

**Verificarea 5: Spam Score Moz**
- Verifică pe moz.com/link-explorer sau Open Site Explorer
- <30% = PASS pentru PBN
- <20% = PASS pentru 301
- >30% = REJECT indiferent de TF/DR

**Verificarea 6: Relevanță Tematică**
- Pentru **301 redirect**: OBLIGATORIU să fi avut conținut sports/gambling/finance. Verifică în Wayback Machine categoriile principale. Fără relevanță tematică → REJECT pentru 301, poate merge ca PBN.
- Pentru **PBN**: preferabil gambling/sports/finance, dar nu obligatoriu dacă TF este ridicat. Orice nișă legitimă acceptabilă.

**Rezultat calificare**: APROBAT / RESPINS / NECESITĂ REVIZUIRE (cu notă specifică)

**Output**: `domain-qualification-{YYYY-WW}.md` — toți candidații cu rezultatele calificării + note
**Checkpoint**: 100% candidați calificați înainte de cumpărare; zero domenii achiziționate fără verificare Wayback + indexare Google

---

### Pas 3: STRATEGIE ACHIZIȚIE PER TIP DOMENIU

**Două căi de achiziție**:

**Calea A: Expirat (deja căzut)**
- Disponibil pentru înregistrare directă sau în marketplace
- Verifică mai întâi: Namecheap, Porkbun (cel mai ieftin), GoDaddy, Name.com
- Dacă indisponibil direct: caută pe GoDaddy Aftermarket, Sedo, Afternic
- Înregistrează imediat după calificare (poate fi luat de alții în ore)

**Calea B: Expirant (merge la licitație)**
- DomCop "Expiring Soon" filter → setează alertă pentru domenii calificate
- Licitație GoDaddy: stabilește auto-bid cu limita maximă
- Strategie bid: intrați în ultimele 2 ore ale licitației pentru a evita escaladarea prematură
- Monitorizează: dacă prețul depășește limita bugetară → retrage-te, nu licita emoțional

**Limite buget**:

| Tip Utilizare | Limita Maximă/Domeniu | Justificare |
|---------------|----------------------|-------------|
| PBN Tier 1 (TF15-25) | $150 | ROI la 10× economie vs editorial |
| PBN Tier 1 (TF25+) | $250 | Premium justificat pentru TF înalt |
| 301 Redirect (DR40-60) | $350 | Boost DR direct pe money site |
| 301 Redirect (DR60+) | $500 | Valoare excepțională de redirect |

**Budget lunar recomandat** (start operație):
- Faza 1 (primele 3 luni): $300–500/lună = 3–5 domenii PBN
- Faza 2 (luni 4–12): $500–1,000/lună când ai validat că PBN ajută rankings
- 301-uri: oportunistic, max $500/lună separat

**Output**: `domain-acquisition-log.csv` — log running: Domain | Tip (PBN/301) | Data Cumpărare | Preț | Sursă | TF | CF | DR | Note
**Checkpoint**: achiziție în max 24h de la calificare pentru domenii expirant; total cheltuieli în limita bugetului lunar

---

### Pas 4: SETUP PBN (ANTI-FOOTPRINT)

**Pentru fiecare domeniu PBN achiziționat**, respectă toți cei 6 pași anti-footprint:

**Măsura 1: Hosting Unic**
- Fiecare site PBN pe provider de hosting DIFERIT (nu același IP sau IP bloc)
- Rotație recomandată (minimum 5 provideri diferiți pentru 5 site-uri):
  - SiteGround (shared hosting, C-Class IP diferit)
  - DigitalOcean VPS (IP unic dedicat)
  - Namecheap Hosting
  - Bluehost
  - Hostinger (pentru cost mai mic)
  - DreamHost
- INTERZIS: două site-uri PBN pe același provider cu IP din același /24 bloc

**Măsura 2: Teme WordPress Diferite**
- Temă unică per site PBN (nu același template)
- Folosește teme gratuite din wordpress.org (rotești prin 10+ teme diferite)
- Personalizează: logo, culori, layout — chiar și aceeași temă cu customizări arată diferit
- Evită constructori de pagini premium identici (Elementor pe tot = footprint)

**Măsura 3: Conținut Autentic Minimum**
- Minimum 5 articole pe fiecare site PBN înainte de adăugarea primului link de ieșire
- Articolele: relevante pentru nișa istorică a domeniului, minimum 500 cuvinte fiecare
- Calitate: nu spam AI, nu spinning — conținut coerent care ar putea fi citit de un om
- Umanizare conținut: aplică procedura M-130 (dacă există) sau editează manual
- Publish treptat: 1 articol/săptămână în primele 5 săptămâni (nu toate deodată)

**Măsura 4: Linkuri de Ieșire Controlate**
- Maximum 3–5 linkuri de ieșire per site PBN total
- Mix obligatoriu: money site + linkuri editoriale relevante (Wikipedia, site-uri de știri, autorități din nișă)
- Niciodată: 100% linkuri spre money site = footprint evident

**Măsura 5: WHOIS și Înregistrare Separată**
- Activează privacy/protecție WHOIS la fiecare domeniu (gratuit la majoritatea registrarilor)
- Folosește emailuri diferite per domeniu (alias sau adrese separate)
- Nu folosi același cont GoDaddy/Namecheap pentru toate domeniile PBN — distribuie pe 2–3 conturi

**Măsura 6: Zero Interconectare**
- Site-urile PBN nu se linkuiesc între ele NICIODATĂ
- Nu includere în același Google Search Console
- Nu Google Analytics cu același ID pe PBN-uri multiple

**Output**: `pbn-setup-checklist-{domain}.md` — checklist anti-footprint complet per site
**Checkpoint**: toate 6 măsuri anti-footprint confirmate înainte de adăugarea linkurilor; site-uri live minimum 7 zile înainte de primul link de ieșire

---

### Pas 5: SETUP 301 REDIRECT

**Pentru domenii cu DR ridicat destinate redirect spre money site**:

**Pas 5.1: Configurare DNS**
- Transferă domeniul la registrarul de la care controlezi DNS
- Setează DNS A Record → IP server money site (sau Cloudflare nameservers)

**Pas 5.2: Implementarea Redirect-ului**
Opțiunea recomandată — `.htaccess` (Apache):
```apache
RewriteEngine On
RewriteRule ^(.*)$ https://moneysite.com/$1 [R=301,L]
```

Opțiunea Cloudflare (dacă folosești CF pe money site):
- Dashboard CF → Rules → Redirect Rules
- Match: `domeniuexpirat.com/*`
- Action: Static redirect 301 → `https://moneysite.com/`

EVITĂ: redirect prin WordPress (mai lent, mai fragil, dependență de CMS)

**Pas 5.3: Verificare Lanț de Redirect**
Folosește: httpstatus.io sau redirectdetective.com
```
Verifică: domeniuexpirat.com → 301 → https://moneysite.com/
TREBUIE: un singur redirect, status 301 (nu 302, nu lanț de redirecte)
```
302 = temporar (nu transferă DR) → REJECT, schimbă în 301.

**Pas 5.4: Monitorizare Transfer DR**
- Adaugă domeniu în Ahrefs monitoring
- Așteptare: 4–8 săptămâni pentru transferul complet al DR
- La 90 zile: verifică dacă DR money site a crescut cu suma așteptată
- Dacă nu: verifică integritatea redirect (poate s-a stricat la un update server)

**Output**: `redirect-config-{domain}.md` — documentație setup redirect + program de monitorizare transfer DR
**Checkpoint**: 301 redirect live în max 48h de la achiziția domeniului; lanț redirect verificat (zero 302-uri); monitorizare DR setată în Ahrefs

---

### Pas 6: MANAGEMENTUL VELOCITĂȚII DE LINKURI

**Obiectiv**: Adăugare de linkuri PBN la money site fără a crea pattern detectabil de Google.

**Limite de velocitate**:
- Maximum **2–3 linkuri PBN noi per money site per lună**
- Interleave obligatoriu cu linkuri legitime: ratie 1 PBN : minimum 2 linkuri legitime (guest posts, citații, linkuri editoriale)
- Nu adăuga mai mult de 1 link PBN pe săptămână la același money site

**Distribuție anchor text** (STRICT):

| Tip Anchor | Procent Target | Exemple |
|------------|----------------|---------|
| Branded | 50% | "MarathonBet Kenya", "bet-kenya.com", "site-ul nostru" |
| Generic | 30% | "click here", "visit this site", "mai multe detalii", "accesează" |
| Partial match | 20% | "pariuri sportive Kenya", "casino online Peru" |
| **Exact match** | **0%** | "pariuri sportive" exact → **INTERZIS ca anchor principal** |

**Monitorizare anchor text**:
- Exportă din Ahrefs: Backlinks → filtrează pe money site → coloana Anchor Text
- Calculează distribuția lunară
- Dacă exact match depășește 15% → oprire completă de linkuri noi 30 zile

**Output**: `link-velocity-{money-site}.md` — calendar linkuri: Lună | Linkuri PBN Adăugate | Linkuri Legitime | Distribuție Anchor | Status
**Checkpoint**: distribuție anchor menținută (niciodată >20% exact match); adăugări lunare în limitele de velocitate

---

### Pas 7: AUDIT LUNAR PORTOFOLIU DOMENII

Prima lună a fiecărui trimestru: audit complet al tuturor domeniilor din portofoliu.

**Checklist audit per domeniu PBN**:
```
[ ] site:domeniu.com → indexat? (zero rezultate = ALERTĂ ROȘIE)
[ ] Referring domains count: a scăzut brusc? (linkuri pierdute = scade valoarea)
[ ] TF/CF curent vs la achiziție (recalculat în Majestic)
[ ] Conținut site: publicat conform planului? Minimum 5 articole?
[ ] Hosting: site accesibil? (testează URL-ul direct)
[ ] Linkuri de ieșire: încă funcționale? (link checker tool)
```

**Checklist audit per domeniu 301**:
```
[ ] Redirect chain: încă 301? (nu s-a schimbat în 302 accidental?)
[ ] Target URL: money site-ul returnează 200 OK?
[ ] DR money site: trending up conform așteptărilor?
[ ] DNS: domeniu înregistrat, nu expirat (verifică data de expirare!)
```

**Protocol domeniu deindexat (PBN)**:
1. Detectare (site: search returnează zero) → **QUARANTINA IMEDIATĂ**
2. Elimină toate linkurile spre money site din acel PBN site (înainte de orice investigație)
3. Investighează cauza: penalizare manuală? Conținut spam detectat? Hosting ban?
4. Decizie: recuperare (contestație penalizare) sau abandonare (nu reinvestești în domeniu penalizat)

**Output**: `domain-portfolio-audit-{YYYY-MM}.md` — status toate domeniile: indexat/deindexat, TF/CF curent, DR (pentru 301-uri), probleme identificate
**Checkpoint**: audit lunar complet; domenii deindexate în carantina în max 24h de la detectare

---

## §3 — SCORECARD CALIFICARE DOMENII

| Criteriu | PBN Tier 1 | 301 Redirect | Metodă Verificare |
|----------|-----------|--------------|-------------------|
| TF | ≥15 | ≥25 | Majestic (date directe, nu DomCop) |
| TF/CF ratio | ≥0.5 | ≥0.5 | Majestic |
| DR | ≥20 | ≥40 | Ahrefs |
| Unique RDs | ≥20 | ≥50 | Ahrefs |
| Google Indexed | DA | DA | site:domeniu.com |
| Wayback: penalizat vreodată | NU | NU | web.archive.org |
| Wayback: spam/adult/pharma | NU | NU | web.archive.org |
| Spam Score | <30% | <20% | Moz Link Explorer |
| Relevanță tematică | Preferabil | OBLIGATORIU | Wayback Machine |
| Preț maxim achiziție | $150 (TF15-25) | $500 (DR60+) | Budget guideline |
| Link velocity naturală | DA | DA | Ahrefs graph |

**Scor minim pentru APROBAT**: toate criteriile PASS (zero FAIL sau N/A pentru criterii obligatorii).

---

## §4 — CORTEX KNOWLEDGE BASE

```json
{
  "procedure_id": "M-132",
  "title": "Expired Domain Acquisition for PBN & 301 Redirects",
  "domain": "affiliate",
  "sub_domain": "link-building-pbn",
  "version": "1.0",
  "status": "active",
  "connects_to": ["M-127", "M-117"],
  "reference_docs": ["seo-2026-full-spectrum.md"],
  "output_files": [
    "domain-candidates-{YYYY-WW}.csv",
    "domain-qualification-{YYYY-WW}.md",
    "domain-acquisition-log.csv",
    "pbn-setup-checklist-{domain}.md",
    "redirect-config-{domain}.md",
    "link-velocity-{money-site}.md",
    "domain-portfolio-audit-{YYYY-MM}.md"
  ],
  "quality_criteria": {
    "pbn_tier1_tf_min": 15,
    "pbn_tier1_dr_min": 20,
    "redirect_301_tf_min": 25,
    "redirect_301_dr_min": 40,
    "spam_score_max_pbn": "30%",
    "spam_score_max_301": "20%",
    "tf_cf_ratio_min": 0.5,
    "unique_rds_min_pbn": 20,
    "unique_rds_min_301": 50
  },
  "budget_guidelines": {
    "pbn_tier1_max_per_domain": "$150",
    "redirect_301_max_per_domain": "$500",
    "monthly_acquisition_start": "$300-500"
  },
  "velocity_limits": {
    "max_pbn_links_per_money_site_per_month": 3,
    "ratio_pbn_to_legitimate": "1:2",
    "exact_match_anchor_max": "20%"
  },
  "tags": ["pbn", "expired-domains", "301-redirect", "link-building", "seo", "gambling", "latam", "africa"]
}
```

---

## §5 — CONEXIUNI INTER-PROCEDURI

| Procedură | Tip Conexiune | Detaliu |
|-----------|---------------|---------|
| **M-127** | Client | M-127 Programmatic SEO are nevoie de backlink-uri pentru a ranka pagini — M-132 furnizează autoritatea prin PBN + 301 |
| **M-117** | Sursă | Competitive Intelligence M-117 identifică de unde se linkuiesc competitorii — ghidează tipul de domenii de achiziționat |
| **seo-2026-full-spectrum.md** | Referință | Validarea strategiei PBN + metode avansate de anti-footprint documentate acolo |

---

## §6 — METRICI DE SUCCES

| Metric | Target | Metodă Măsurare |
|--------|--------|-----------------|
| Domenii achiziționate/lună | 3–5 PBN + 1–2 301 | domain-acquisition-log.csv |
| Site-uri PBN live | Creștere cu 3–5/lună | domain-portfolio-audit |
| DR money site (din 301) | +5–15 DR în 90 zile per 301 | Ahrefs monitoring |
| Rata de respingere calificare | >50% REJECT (calitate strictă) | domain-qualification raport |
| PBN site-uri indexate | >95% din portofoliu | Audit lunar site: |
| Anchor text exact match | <20% în orice lună | Ahrefs anchor report |

---

## §7 — VIOLĂRI HOW (Interzis)

- **VIOLAȚIE CRITICĂ**: Cumpărarea unui domeniu fără verificarea Wayback Machine → risc de moștenire penalizare Google spre money site
- **VIOLAȚIE CRITICĂ**: Două sau mai multe site-uri PBN pe același hosting/IP bloc → footprint evident, risc deindexare în masă
- **VIOLAȚIE CRITICĂ**: Anchor text exact match >20% → risc penalizare Penguin
- **VIOLAȚIE**: Adăugarea de linkuri PBN fără a publica minimum 5 articole pe site-ul PBN → risc detectare ca PBN pur
- **VIOLAȚIE**: 301 redirect cu 302 în loc de 301 → DR nu se transferă, investiție risipită
- **VIOLAȚIE**: Ignorarea deindexarii unui PBN site fără a elimina imediat linkul spre money site → expunere continuă la risc

---

---

## §8 — ENFORCEMENT LOOP (META-H-002)

**WHERE**: La planificarea lunară a bugetului de link building + la calificarea fiecărui domeniu candidat

**WHEN**:
- Scanare zilnică DomCop (15 min/zi): Pas 1 workflow zilnic
- La fiecare domeniu candidat calificat: Pas 2 calificare manuală obligatorie înainte de orice achiziție
- Prima lună a fiecărui trimestru: audit complet portofoliu (Pas 7)
- La detectarea deindexarii oricărui PBN: carantină imediată în 24h

**HOW — Violations critice**:
- **VIOLAȚIE CRITICĂ**: Achiziție domeniu fără verificarea Wayback Machine → risc moștenire penalizare Google pe money site
- **VIOLAȚIE CRITICĂ**: Două sau mai multe site-uri PBN pe același hosting/IP bloc → footprint evident, risc deindexare în masă
- **VIOLAȚIE CRITICĂ**: Anchor text exact match >20% → risc penalizare Penguin
- **VIOLAȚIE**: Adăugarea de linkuri PBN fără minimum 5 articole publicate pe site-ul PBN
- **VIOLAȚIE**: 301 redirect cu 302 în loc de 301 → DR nu se transferă
- **VIOLAȚIE**: PBN site deindexat detectat fără carantinare imediată (eliminare link spre money site)

**CONNECT**:
- **M-127** (Programmatic SEO): M-132 furnizează autoritatea prin PBN + 301 pentru rankarea paginilor din M-127
- **M-117** (Competitive Intelligence): competitive intelligence identifică sursele de linkuri ale competitorilor → ghidează tipul de domenii de achiziționat
- **seo-2026-full-spectrum.md**: referință pentru validarea strategiei PBN + metode avansate anti-footprint

**VERIFY**:
- [ ] Minimum 20 candidați revizuiți săptămânal din DomCop?
- [ ] 100% domenii achiziționate au trecut calificarea completă (6 verificări Pas 2)?
- [ ] Zero domenii cumpărate fără verificare Wayback Machine + indexare Google?
- [ ] Toate site-uri PBN au minimum 5 articole publicate înainte de primul link de ieșire?
- [ ] Distribuție anchor text menținută: branded ≥50%, exact match <20%?
- [ ] Audit trimestrial portofoliu generat (`domain-portfolio-audit-{YYYY-MM}.md`)?
- [ ] Orice PBN deindexat → link eliminat din money site în <24h?

**VK-uri obligatorii**:
- `✅ [PROC] M-132 | săptămâna: {YYYY-WW} | candidați: {N} revizuiți | achiziții: {N} | buget: ${X} | portofoliu: {N} domenii`
- `✅ [CORTEX] "M-132 Domain Acquisition {YYYY-WW}" | rule: TRAIN-S-001 | v1.0`

**MODEL ROUTING**:
- **Claude Opus** (claude-opus-4-8): analiza profilurilor de linkuri complexe (Pas 2 — interpretare red flags), decizie achiziție la limita criteriilor
- **Claude Sonnet**: generare output CSV, formatare rapoarte calificare, actualizare logs de rutină

---

*Procedură creată conform standardului FORGE v1.2 — Genie Slim Core*
