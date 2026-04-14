---
id: SEO-105
title: PBN Setup and Maintenance SOP
domain: seo
source: Black Hat SEO — PBN Construction
version: 2.0
forge_score: "estimated M/3.5"
---

# PBN Setup and Maintenance SOP

## Obiectiv
Construirea și întreținerea unui PBN optimizat cu hosting diversity (minim 5 provideri), unique WHOIS, CMS diferite, IP class C diversity, content quality thresholds, anchor text strategy și monitoring de-indexare — minimizând detectarea și maximizând durată de viață.

## Pași

### 1. Achiziționarea domeniilor PBN (expired domains)
**Surse**: ExpiredDomains.net, GoDaddy Auctions, NameJet, DomCop
**Criterii calificare**:
- Trust Flow (Majestic) > 15, ideal > 25
- Citation Flow > 15 (ratio TF/CF: TF minim 40% din CF)
- Referring Domains: minim 20 reale (nu spam)
- Wayback Machine: site legitim, nu spam/adult (excepție dacă nișa e gambling)
- Google index: `site:domeniu.com` — NU trebuie penalizat
- Preț: €10-500+ (TF 30+ poate costa €100-2.000)

### 2. Hosting diversity — footprint minimization (CRITICAL)
**Regula**: fiecare site PBN trebuie să pară independent.

**Hosting (minim 5 provideri diferiți pentru 10+ site-uri)**:
- Cloudways, SiteGround, Hostinger, DigitalOcean, Vultr, A2 Hosting, DreamHost
- NEVER: toate pe același hosting (footprint major → detected)
- IP class C diversity: fiecare domeniu pe IP diferit (C-class minim)

**DNS/Nameservers**:
- Nameserveri diferiți per domeniu (nu toți pe Cloudflare)
- WHOIS Privacy activat + date diferite unde posibil

**CMS diversity (nu toate pe WordPress)**:
- WordPress (3-4 site-uri) + Ghost (2) + Hugo (2) + HTML static (2)
- Teme DIFERITE pe fiecare WordPress site
- Plugin-uri diferite (nu aceeași combinație)
- User/admin diferit (nu "admin" pe toate)

### 3. Conținut PBN — quality thresholds
**Minim per site PBN credibil**:
- 5-10 articole publicate ÎNAINTE de primul link plasat
- Lungime: 500-1.000 cuvinte (nu content farm 100 cuvinte)
- Nișă relevantă: conținut în aceeași nișă cu site-ul țintă
- Update: 1-2 articole noi/lună (site activ)
- Pagini obligatorii: About, Contact, Privacy Policy (face site-ul real)
- Images: originale sau stock diferit per site (nu aceleași pe toate)

**AI Content** (acceptabil cu atenție):
- Claude/GPT generate + editare pentru unicitate
- NU duplica între site-urile PBN
- Evita "AI tells" (Google are clasificatoare)
- Adaugă opinii personale, date specifice, experiență simulată

### 4. Link placement strategy — natural looking
**Density per pagină**: max 2-5 linkuri outbound
- 70-80% linkuri spre site-uri autoritative (Wikipedia, news sites)
- 20-30% linkuri spre site-ul tău (money site)

**Anchor text** (CRITIC — over-optimization = detection):
- 40% branded ("Albastru", "Șapte Focuri")
- 30% generic ("click here", "vizitați", "mai multe detalii")
- 20% partial match ("turism rural Albastru")
- 10% exact match — MAX 10% (mai mult = over-optimization → Penguin)

**Plasare naturală**:
- Link în corpul articolului (NU footer/sidebar)
- Context relevant (nu link restaurant într-un articol crypto)
- Variază posting dates (nu toate linkurile în aceeași săptămână)

### 5. Monitoring și întreținere PBN
**Indexare check (lunar)**:
- `site:domeniu-pbn.com` în Google per fiecare site
- De-indexat → oprește linkurile imediat, nu adăuga noi
- Multiple site-uri de-indexate simultan = rețeaua detectată → STOP complet

**Domain renewal**: reminder 60 zile înainte expirare
**Uptime**: UptimeRobot (gratuit) per site PBN
**Content freshness**: 1-2 articole noi/lună per site (minim)

### 6. Escalation — ce faci la de-indexare masivă
- >30% din rețea de-indexat: OPREȘTE TOATE linkurile spre money site
- Evaluează: recuperabile sau de abandonat?
- Nu face modificări bruște — observă 2-4 săptămâni
- Dacă money site afectat: disavow linkurile PBN, rebuild cu altă rețea
- Consider: migrare de la PBN la grey-hat (niche edits, guest posts) — mai sigur termen lung

### 7. PBN cost analysis
```
Costuri per 10 site-uri PBN:
- Domenii: €100-500 (expired domains)
- Hosting: €150-300/an (5 provideri x €2-5/lună)
- Content: €200-500 (50-100 articole AI-assisted)
- Maintenance: 2-4h/lună (content + monitoring)
- Total setup: €450-1.300
- Total anual: €300-600 (hosting + content refresh)

ROI: 10 PBN links DR20+ = echivalent €2.000-5.000 linkuri cumpărate
Break-even: 3-6 luni vs link buying
```

### 8. Gambling SEO PBN specifics
- Gambling PBN-uri necesită domenii din nișa gambling/entertainment/sports (relevanță critică)
- Casino keywords pe PBN: articole "review casino", "bonus fără depunere", "strategie pariuri"
- Tier 2 strategy: PBN → linkuri la tier 2 sites → care linkuiesc la money site (indirectly)
- Detectabilitate: gambling nișa e monitorizată activ de Google — footprint minimization esențial
- ROI gambling PBN: un link DR 30+ poate genera €500-5.000/lună revenue pe casino affiliate

## Verificare
- [ ] Domenii cu TF>15, CF>15, fără penalizări (minim 5 domenii)
- [ ] Hosting diversificat pe minim 5 provideri diferiți
- [ ] CMS diferit (nu toate WordPress) + teme/plugins diferite
- [ ] IP class C diversity verificat (nicio C-class repetată)
- [ ] Conținut inițial: 5-10 articole/site publicate ÎNAINTE de link placement
- [ ] Anchor text distribution documentat (<10% exact match)
- [ ] Monitoring indexare configurat (monthly manual + UptimeRobot)
- [ ] Escalation plan documentat (ce faci la de-indexare)

## Instrumente
- Majestic.com — Trust Flow / Citation Flow per domeniu
- ExpiredDomains.net / DomCop — achiziție expired domains
- Ahrefs — verificare backlink profile domenii
- Wayback Machine (archive.org) — verificare istoricul site
- UptimeRobot — monitoring uptime
- Multiple hosting providers (Cloudways, SiteGround, Hostinger, DO, Vultr)

## Note
- **RISC**: PBN-urile violează politicile Google. Detectarea = penalizare manuală/algoritmică money site
- Riscul crește cu scala (50+ site-uri) și neglijență footprint
- Alternativă mai sigură: guest posting + niche edits (mai lent, zero penalizare risc)
- Best use case: site-uri afiliere/lead gen cu lifecycle mediu (NU brand pe termen lung)
- Gambling: PBN este norma industriei — 90% din competitori folosesc grey/black hat
