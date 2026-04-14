---
id: SEO-110
title: Core Web Vitals Optimization Checklist
domain: seo
source: Advanced SEO 2025 — Technical Performance
version: 2.0
forge_score: "estimated M/3.5"
---

# Core Web Vitals Optimization Checklist

## Obiectiv
Optimizarea completă a celor 3 Core Web Vitals la valorile "Good" (LCP <2.5s, INP <200ms, CLS <0.1) cu tehnici specifice per metric, preload strategies, image optimization, JS defer, font-display, ad container reservation și prioritizare impact.

## Pași

### 1. Înțelegerea CWV (2024+ metrics)
**LCP (Largest Contentful Paint)**: cât de repede se încarcă conținutul principal
- Good: <2.5s | Needs Improvement: 2.5-4s | Poor: >4s

**INP (Interaction to Next Paint)**: cât de repede răspunde pagina la interacțiuni
- Good: <200ms | Needs Improvement: 200-500ms | Poor: >500ms
- INP a înlocuit FID în martie 2024

**CLS (Cumulative Layout Shift)**: cât se mișcă elementele la loading
- Good: <0.1 | Needs Improvement: 0.1-0.25 | Poor: >0.25

**Tools**: GSC → Core Web Vitals (field data), PageSpeed Insights (lab data), Chrome DevTools Lighthouse, WebPageTest.org

### 2. Optimizare LCP (<2.5s target)
**Preload LCP resource**:
```html
<link rel="preload" as="image" href="/hero-image.webp" fetchpriority="high">
```

**Image optimization**:
- Format: WebP (sau AVIF pentru browsere moderne)
- Dimensionare: nu servi 2000px pentru zonă de 600px
- **NU** pune `loading="lazy"` pe imaginea LCP (blochează LCP!)
- Compresie: TinyPNG / Squoosh / ImageOptim

**Elimina render-blocking**:
- CSS non-critical: `<link rel="stylesheet" media="print" onload="this.media='all'">`
- JS non-critical: adaugă `defer` sau `async`

**Server Response Time (TTFB <200ms)**:
- TTFB >800ms = cauza principală LCP slab
- Soluție: hosting rapid + CDN (Cloudflare) + caching agresiv (WP Rocket, LiteSpeed Cache)

### 3. Optimizare INP (<200ms target)
**Diagnosticare**: Chrome DevTools → Performance → Record interacțiune → Long Tasks (>50ms)

**Defer heavy JavaScript**:
- Scripturi non-critice → async/defer obligatoriu
- Third-party (GTM, chat widgets, ads) → lazy load

**Break up Long Tasks**:
- Funcții JS lungi → divide cu `setTimeout()` sau `scheduler.postTask()`
- Web Workers pentru computații intensive

**Reduce Third-Party Impact**:
- Auditează: câte scripts third-party? (PageSpeed → Third-Party Summary)
- Elimina neutilizate complet
- Alternativă lightweight: Plausible vs GA4 (dacă INP critic)

### 4. Optimizare CLS (<0.1 target)
**Dimensionare explicită imagini** (fix #1 — cel mai comun):
```html
<img src="image.jpg" width="800" height="600" alt="...">
```

**Font-display fără FOIT/FOUT**:
```css
@font-face {
  font-family: 'MyFont';
  font-display: optional; /* sau 'swap' + font-size-adjust */
}
```

**Rezervare spațiu ads**:
- Containerele ads = dimensiune fixă CSS ÎNAINTE de ad load
- Fără dimensiune rezervată = layout shift major

**Dynamic content**:
- Cookie banners / newsletter popups → inserare JOS (nu deplasează conținut sus)
- Animare de jos în sus (nu de sus în jos)

### 5. WordPress specific optimizations
- **WP Rocket + Cloudflare** rezolvă 80% din CWV fără development
- WP Rocket: lazy load images (excepție LCP), defer JS, minify CSS/JS, cache
- Cloudflare: CDN global, Argo Smart Routing, Auto Minify, Brotli compression
- **Autoptimize** (alternativă gratuită): CSS/JS minify + defer
- **ShortPixel/Imagify**: auto-WebP conversion pe upload

### 6. Workflow de prioritizare
1. GSC → Core Web Vitals → Failed URLs (identifică pagini cu probleme)
2. Prioritizează pagini importante (trafic, conversii) — nu pagini cu 10 vizite/lună
3. PageSpeed Insights per pagina → Opportunities + Diagnostics
4. Implementează fix-uri în ordinea potențial impact (Lighthouse estimare)
5. Verificare: GSC CWV se actualizează la 28 zile (field data) — PSI lab data imediat

### 7. CWV monitoring ongoing
- GSC Core Web Vitals report: check săptămânal → trend Good/NI/Poor
- PageSpeed Insights: test post fiecare deployment major
- Chrome UX Report (CrUX): field data reale de la utilizatori Chrome
- Alertă: dacă % pagini "Poor" crește >10% → investigare urgentă

### 8. CWV impact pe SEO rankings
- CWV = **tiebreaker signal** — nu te propulsează de la pagina 5 la 1
- DAR: poate fi diferența între poziția 3 și 1 când alți factori sunt egali
- Google confirmă: "page experience" e un factor de ranking, dar "great content" primează
- CDN (Cloudflare) rezolvă TTFB și parțial LCP fără o linie de cod — FIRST STEP recomandat

## Verificare
- [ ] Baseline CWV documentat din GSC (nr URL-uri Good/NI/Poor)
- [ ] LCP image preloaded pe pagini principale cu fetchpriority="high"
- [ ] Imagini convertite WebP + dimensionate corect
- [ ] Third-party scripts auditate și non-esențialii deferați
- [ ] CLS: dimensiuni explicite pe TOATE imaginile + spațiu rezervat ads
- [ ] INP: Long Tasks identificate și breakup-uite
- [ ] Font-display setat (optional sau swap)
- [ ] WordPress: WP Rocket + Cloudflare configurate (dacă WP)

## Instrumente
- PageSpeed Insights (pagespeed.web.dev) — per URL, gratuit
- GSC → Core Web Vitals — date reale agregate
- Chrome DevTools → Lighthouse / Performance — debugging detaliat
- WebPageTest.org — test avansat cu waterfall
- Squoosh.app — optimizare imagini WebP/AVIF gratuit
- WP Rocket — WordPress cache + optimization

## Note
- LCP = cel mai impactant CWV (cel mai frecvent "Poor", cel mai corectabil)
- CDN (Cloudflare) = first step fără development necesar
- WordPress: WP Rocket + Cloudflare = 80% CWV issues rezolvate
- CWV = tiebreaker signal, nu ranking factor principal — content primează
