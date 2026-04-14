---
id: SEO-108
title: SEO Audit-to-Action Full SOP
domain: seo
source: Advanced SEO 2025 — SEO Audit Methodology
version: 2.0
forge_score: "estimated M/3.5"
---

# SEO Audit-to-Action Full SOP

## Obiectiv
SOP complet pentru audit SEO full (tehnic + on-page + off-page + conținut), cu Ahrefs/Semrush workflow integrat, scoring system 10/10, roadmap prioritizat (urgent/short/long term), Core Web Vitals audit și Gambling SEO-specific checks.

## Pași

### 1. Setup și colectare date (Ziua 1-2)
**Tool stack**: Screaming Frog, GSC, GA4, Ahrefs/Semrush, PageSpeed Insights
**Acces client**: GSC Owner/Full User, GA4 Analyst/Editor, CMS (opțional)
**Screaming Frog crawl**: unlimited depth, JS rendering ON, Googlebot user-agent → export All pages, Broken links, Redirects, Images, Structured Data

### 2. Audit tehnic (Ziua 2-3)
**Indexare & Crawlability**:
- [ ] robots.txt: nu blochează resurse importante
- [ ] sitemap.xml: există, actualizat, submis GSC
- [ ] Canonical tags: corecte, fără self-referencing greșit
- [ ] Pagini importante indexate: `site:domeniu` vs sitemap count
- [ ] Pagini eronat noindex → recovery imediat

**URL Structure & Redirects**:
- [ ] URL-uri curate, descriptive, fără parametri excesivi
- [ ] Zero redirect chains >2 salturi
- [ ] HTTP→HTTPS corect global
- [ ] WWW vs non-WWW consistent cu canonical

**Core Web Vitals** (per pagini principale):
- LCP <2.5s | INP <100ms | CLS <0.1
- PageSpeed Insights per URL principal → document scores + opportunities

### 3. Audit On-Page (Ziua 3-4)
**Top 50 pagini organic** (după trafic) — analiză individuală:
- Title tags: unique, 50-60 caractere, keyword principal
- Meta descriptions: unique, 150-160 caractere, CTA inclus
- H1: exact 1 per pagină, conține keyword
- Content depth: mai complet decât top 3 competitori?
- Internal linking: linkuri spre/de la pagini relevante?
- Schema Markup: Article, Product, FAQ, Breadcrumb implementat?

**Quick wins identificare**:
- Pagini poziția 4-20 cu volum decent → optimizare on-page = urcare
- CTR <2% vs poziție → meta title/description optimization
- Canibalizare: 2+ pagini pe același keyword → consolidare/diferențiere

### 4. Audit Off-Page și Competitor Analysis (Ziua 4-5)
**Link Profile**: Total Referring Domains vs competitor, TF/DR comparativ, anchor text distribution, link velocity trend, toxic links identified

**Competitor Gap Analysis (Ahrefs)**:
- **Content Gap**: ce topics acoperă competitorii și tu nu?
- **Backlink Gap**: ce domenii linkuiesc la competitori și nu la tine? (Link Intersect)
- **Keyword Gap**: ce keywords rankează competitorii unde tu nu ești top 20?

### 5. Audit Content Quality
- Total pagini vs pagini cu trafic → ratio quality (target >30% active)
- Pagini <300 cuvinte → noindex/expand/delete
- Duplicate content intern (Screaming Frog Near Duplicate)
- AI content detection: spot check top 20 pagini
- Freshness: pagini cu date >12 luni vechi → refresh candidates

### 6. Scoring system și raport
```
EXECUTIVE SUMMARY (1 pagină):
- Score per categorie (/10)
- Top 3 probleme critice
- Top 3 quick wins
- Potențial trafic adițional estimat

SCORES:
- Technical SEO: X/10
- On-Page: X/10
- Off-Page / Authority: X/10
- Content Quality: X/10
- UX / Core Web Vitals: X/10

FINDINGS per categorie:
Issue | Severity (Critical/High/Medium/Low) | Impact | Effort

ROADMAP:
- URGENT (1-2 săpt): indexing blocks, broken redirects, security
- SHORT TERM (1-3 luni): on-page optimization, quick wins
- LONG TERM (3-12 luni): link building, content strategy

BASELINE METRICI:
- Trafic organic actual | Poziții | DR/TF | Conversii
```

### 7. Ahrefs/Semrush workflow integrat
- **Site Audit**: Ahrefs → crawl → errors/warnings/notices prioritizate automat
- **Keywords**: Semrush Position Tracking → baseline + monitoring
- **Competitors**: Ahrefs → Competing Domains → top 5 + gap analysis
- **Backlinks**: Ahrefs → Backlinks → New/Lost → trend analysis
- **Content**: Ahrefs → Top Pages → sort by traffic decline → refresh candidates

### 8. Gambling SEO audit specifics
- **Link profile**: verifică PBN footprint (C-class IP diversity, hosting patterns)
- **Anchor text**: exact match pe casino keywords >15% = risc ridicat
- **Content**: review pagini cu "bonus fără depunere", "casino online" — depth vs competitors
- **Compliance**: verifică ONJN licență mentions, responsible gambling disclaimers
- **Programmatic**: verifică qualitatea paginilor auto-generate per sport/liga/meci

## Verificare
- [ ] Toate 4 categorii audit completate (tehnic, on-page, off-page, conținut)
- [ ] Findings documentate cu severitate + impact + effort
- [ ] Scoring system completat (5 categorii x /10)
- [ ] Raport cu Executive Summary + plan acțiune prioritizat
- [ ] Baseline metrici documentate (trafic, poziții, DR/TF)
- [ ] Roadmap cu responsabili și deadlines
- [ ] Quick wins identificate și prioritizate
- [ ] Competitor gap analysis completat

## Instrumente
- Screaming Frog SEO Spider — crawl tehnic complet
- Google Search Console + GA4 — date reale performanță
- Ahrefs Site Audit + Backlink Checker — audit comprehensive
- PageSpeed Insights — Core Web Vitals
- Google Slides / Notion — prezentare raport

## Note
- Audit fără plan de acțiune = document PDF frumos care nu face nimic
- Prioritizează: impact mare + efort mic (quick wins) > impact mare + efort mare
- Primul audit: 5-10 zile; follow-up quarterly: 1-2 zile
- Gambling audit: adaugă PBN footprint check + anchor text risk assessment
