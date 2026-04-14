---
id: SEO-107
title: Penalty Avoidance Checklist
domain: seo
source: Black Hat SEO — Risk Management
version: 2.0
forge_score: "estimated M/3.5"
---

# Penalty Avoidance Checklist

## Obiectiv
Checklist preventiv complet pentru minimizarea riscului de penalizare Google, acoperind link profile health, content quality signals, technical SEO, Core Web Vitals (LCP <2.5s, FID <100ms, CLS <0.1), behavioral signals și early warning monitoring system.

## Pași

### 1. Link Profile Health Check (lunar)
- [ ] Anchor text: exact match <20% din total (>20% = risc Penguin)
- [ ] Link velocity: niciun spike brusc (500+ linkuri/7 zile = suspect)
- [ ] Toxic links: TF <5 și CF <5 = potențial spam
- [ ] C-class IP: linkuri de pe aceeași C-class IP = footprint PBN
- [ ] De-indexed sources: linkuri de pe site-uri de-indexate = asociere negativă
- Acțiune: Ahrefs Alerts zilnic → lunar export backlinks noi → calificare OK/disavow

### 2. Content Quality Signals (trimestrial)
- [ ] Pagini <300 cuvinte fără motiv → noindex sau expand
- [ ] Duplicate intern: Screaming Frog → Near Duplicate check
- [ ] Duplicate extern: Copyscape top 20 pagini organic
- [ ] Keyword stuffing: density >3-4% = over-optimization
- [ ] Click-bait titluri vs conținut real (mismatch = pogo-sticking signal)
- [ ] Doorway pages / programmatic SEO fără valoare = thin content

### 3. Technical SEO risk signals (lunar)
- [ ] Core Web Vitals: **LCP <2.5s, INP <100ms, CLS <0.1** (GSC + PageSpeed Insights)
- [ ] Mobile-Friendly: toate paginile trec Mobile Usability (GSC)
- [ ] Crawl Errors: Coverage Report → 0 erori 5xx
- [ ] Hreflang: corect implementat (dacă multilingv)
- [ ] Schema Markup: fără markup înșelător (claim ce nu e pe pagină → penalizare)
- [ ] Redirects: niciun chain >2, niciun loop
- [ ] robots.txt: nu blochează resurse importante (JS, CSS)

**Semnale critice = penalizare imediată**:
- Cloaking (conținut diferit Google vs users)
- Hidden text (font 0, alb pe alb)
- Doorway pages (create exclusiv pentru motoare de căutare)
- Scraped content la scară (mii de pagini copiate)

### 4. Core Web Vitals optimization targets
| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP | <2.5s | 2.5-4s | >4s |
| INP | <200ms | 200-500ms | >500ms |
| CLS | <0.1 | 0.1-0.25 | >0.25 |

**Quick fixes**: Preload LCP image, defer non-critical JS, dimension explicit pe imagini, WebP format, CDN (Cloudflare)

### 5. Behavioral signals monitoring (GA4)
- Bounce Rate per landing page: >80% pe pagini principale = conținut nu satisface
- Session Duration: <30s pe articole lungi = nu reține
- Pogo-sticking: vin → pleacă → revin la SERP = semnal negativ

**UX minimum**:
- [ ] Niciun interstitial pop-up pe mobile care blochează conținut
- [ ] Max 30% ads above-the-fold
- [ ] Niciun autoplay video cu sunet
- [ ] Butoane click minim 44px pe mobile

### 6. Early warning monitoring system
**GSC**: Email alerts pentru Manual Actions, Security Issues, AMP Errors
**Ahrefs**: noi linkuri zilnic + keywords drop semnificativ săptămânal
**GA4**: alertă automată dacă organic traffic scade >20% vs previous week
**Semrush**: alertă drop >5 poziții pe keywords monitorizate

### 7. Calendar de audit
- **Zilnic**: GSC coverage errors (5 min)
- **Săptămânal**: backlinks noi + keyword ranking changes
- **Lunar**: full link profile audit + content audit top 50 pagini + CWV check
- **Trimestrial**: full technical SEO audit (Screaming Frog) + Core Web Vitals deep

### 8. Penalty avoidance per nișă
**Gambling**: Google monitorizează agresiv → footprint minimization esențial, anchor text conservator, no obvious patterns
**YMYL (health/finance)**: EEAT signals obligatorii, autor expert certificat, surse citate
**E-commerce**: Product schema corect, prețuri reale, reviews autentice, no fake ratings

## Verificare
- [ ] Anchor text ratio calculat și documentat (<20% exact match)
- [ ] Content audit completat (pagini thin remediate)
- [ ] Core Web Vitals verde pe pagini principale (LCP <2.5s, INP <100ms, CLS <0.1)
- [ ] Monitoring alerts active (GSC + Ahrefs + GA4)
- [ ] Calendar audit definit și respectat (zilnic/săptămânal/lunar/trimestrial)
- [ ] Technical SEO clean (zero 5xx, zero broken links, redirects OK)
- [ ] Behavioral signals monitorizate (bounce rate, session duration)
- [ ] Nișă-specific checks completate (gambling/YMYL/e-commerce)

## Instrumente
- Google Search Console — monitoring primary
- Screaming Frog SEO Spider — audit tehnic + content crawl
- Ahrefs / Semrush — backlinks + rankings monitoring
- Copyscape — plagiat extern check
- Google PageSpeed Insights — Core Web Vitals per URL
- GA4 Intelligence Alerts — behavioral signal monitoring

## Note
- 90% din penalizări se pot preveni cu monitoring proactiv
- Cauze comune 2024-2026: link schemes, thin content la scară, cloaking
- "If it feels manipulative, Google will eventually catch it"
- CWV = tiebreaker signal (nu te propulsează de la pag 5 la 1, dar face diferența între pos 3 și 1)
