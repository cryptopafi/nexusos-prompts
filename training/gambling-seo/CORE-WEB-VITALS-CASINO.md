---
type: procedure
created: 2026-03-20
status: active
slug: core-web-vitals-casino
tags: [procedure, nexus]
---

# TRAIN-SEO-018: CORE-WEB-VITALS-CASINO — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Koray Tuğberk GÜBÜR Semantic SEO + Google CWV Documentation -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Technical SEO / Performance Optimization -->

---

## §1 Problema

Casino affiliate sites in Peru, Kenya, and Chile run on cheap shared hosting with unoptimized hero banners (>500KB), undeferred affiliate tracking scripts blocking main thread for 2-4 seconds, and dynamically injected bonus banners causing layout shifts of 0.3+ CLS. Google uses Core Web Vitals as a ranking factor since 2021, and in markets where DR differences between top 10 competitors are minimal (Peru DR 15-30, Kenya DR 12-25), CWV becomes the tiebreaker. Local competitors in LATAM/Africa markets overwhelmingly do NOT optimize CWV — creating an immediate ranking advantage for the first site that passes all three metrics on mobile. The specific challenge for casino sites: affiliate tracking pixels, slot game iFrame embeds, real-time bonus update widgets, and multi-provider comparison tables all degrade performance in ways generic CWV guides don't address.

**Market-specific CWV context**:
- **Kenya (95% mobile)**: LCP is the critical metric — most users on 3G/4G with bandwidth constraints. Competitors average 4-6s LCP mobile. Target: <2.5s = massive advantage
- **Peru**: Mid-range mobile devices, improving infrastructure. Focus: INP (interaction delays from tracking scripts) and CLS (bonus banner injection)
- **Chile**: Most sophisticated market, better infrastructure, but audiencee expects fast UX. CWV parity with US/EU sites expected

---

## §2 Procedura

### Pas 1: BASELINE AUDIT — Full CWV Diagnostic Across All Target Pages

Run a comprehensive CWV audit before any optimization:

**1.1 — PageSpeed Insights (per-page audit)**:
- URL: `pagespeed.web.dev` → test EACH money page individually (homepage, top 3 review pages, comparison page, payment method pages)
- Record for each page: LCP score, INP score, CLS score, Performance score (0-100), specific recommendations
- Test both **mobile** AND **desktop** — mobile is the ranking signal but desktop reveals different issues

**1.2 — Chrome DevTools (deep diagnosis)**:
- F12 → Performance tab → Record → interact with page → Stop
- Identify: longest tasks blocking main thread, largest resource loads, layout shift triggers
- F12 → Lighthouse tab → Generate report → Mobile → Performance category

**1.3 — Google Search Console CWV Report**:
- GSC → Core Web Vitals → Review "Poor" and "Needs Improvement" URLs
- Group by issue type (LCP/INP/CLS) and page template (review page, comparison page, etc.)
- Export list of all affected URLs

**1.4 — Competitor CWV Benchmarking**:
- Run PageSpeed Insights on top 5 competitors' equivalent pages
- Document their scores — if all competitors fail CWV, passing = ranking boost
- Use WebPageTest.org for waterfall analysis of competitor load patterns

**1.5 — Real User Metrics (RUM) via CrUX**:
- Chrome UX Report: `https://developer.chrome.com/docs/crux/` → query your domain
- CrUX data represents REAL user experience, not lab data — Google uses this for ranking
- If insufficient data (new site): rely on lab data from PageSpeed Insights

| Metric | Target (PASS) | "Needs Improvement" | Poor | Casino-specific Impact |
|--------|---------------|--------------------|----- |----------------------|
| **LCP** | < 2.5s | 2.5-4.0s | > 4.0s | +15% bounce rate if slow, -12% affiliate click-through |
| **INP** | < 200ms | 200-500ms | > 500ms | -8% conversion on CTA buttons, delayed tracking fire |
| **CLS** | < 0.1 | 0.1-0.25 | > 0.25 | -10% above-fold engagement, bonus banner distrust |

---

### Pas 2: FIX LCP (Largest Contentful Paint) — Casino-Specific Optimizations

**2.1 — Hero Banner / Above-the-Fold Image Optimization**:

Casino sites typically have a hero banner showing featured casino or bonus offer. This is usually the LCP element.

```html
<!-- BEFORE: 800KB PNG hero banner = 3-5s LCP -->
<img src="casino-hero-banner.png" alt="Best Casinos Peru 2026">

<!-- AFTER: WebP with srcset for responsive loading -->
<link rel="preload" as="image" href="casino-hero-400.webp"
      imagesrcset="casino-hero-400.webp 400w, casino-hero-800.webp 800w, casino-hero-1200.webp 1200w"
      imagesizes="(max-width: 600px) 400px, (max-width: 1024px) 800px, 1200px">
<img srcset="casino-hero-400.webp 400w, casino-hero-800.webp 800w, casino-hero-1200.webp 1200w"
     sizes="(max-width: 600px) 400px, (max-width: 1024px) 800px, 1200px"
     src="casino-hero-800.webp" alt="Best Casinos Peru 2026" width="1200" height="400"
     fetchpriority="high">
```

**2.2 — Server Response Time (TTFB) Optimization**:

| Hosting Issue | Fix | Target |
|--------------|-----|--------|
| Shared hosting TTFB >600ms | Upgrade to VPS or use CDN | TTFB <200ms |
| No CDN | Cloudflare free tier (minimum) | Edge caching in LATAM/Africa |
| No edge nodes near target market | Cloudflare with regional optimization | Chile: Santiago PoP, Kenya: Nairobi PoP |
| WordPress slow TTFB | Redis object cache + WP Super Cache/LiteSpeed | <300ms TTFB |

**Cloudflare setup specifics for casino sites**:
- Enable: Auto Minify (JS/CSS/HTML), Polish (WebP auto-conversion), Brotli compression
- Page Rules: Cache Level = Cache Everything for static pages, Edge Cache TTL = 1 month
- Rocket Loader: TEST before enabling — can break affiliate tracking scripts

**2.3 — Font Loading Optimization**:
```css
/* Casino sites often use custom fonts for branding — defer them */
@font-face {
  font-family: 'CasinoBrand';
  src: url('casino-brand.woff2') format('woff2');
  font-display: swap; /* Shows fallback font immediately, swaps when loaded */
}
```

**2.4 — Render-Blocking JavaScript Elimination**:
```html
<!-- Casino sites load 3-7 tracking scripts — ALL must be deferred -->
<!-- BEFORE: render-blocking -->
<script src="affiliate-tracker.js"></script>
<script src="analytics.js"></script>

<!-- AFTER: non-blocking -->
<script src="affiliate-tracker.js" defer></script>
<script src="analytics.js" defer></script>
<!-- OR load after user interaction (best for non-critical tracking): -->
<script>
document.addEventListener('DOMContentLoaded', function() {
  setTimeout(function() {
    ['affiliate-tracker.js', 'analytics.js', 'heatmap.js'].forEach(function(src) {
      var s = document.createElement('script');
      s.src = src;
      s.async = true;
      document.body.appendChild(s);
    });
  }, 3000);
});
</script>
```

---

### Pas 3: FIX INP (Interaction to Next Paint) — Tracking Script & Widget Optimization

**3.1 — Affiliate Tracking Script Audit**:

Casino sites commonly load: affiliate network pixel, Google Analytics/GA4, Facebook Pixel, Hotjar/heatmap, A/B testing script, live chat widget. Each adds 50-200ms to interaction delay.

**Priority framework**:
| Script | Priority | Loading Strategy |
|--------|----------|-----------------|
| Affiliate tracking pixel | Critical (revenue) | Load on DOMContentLoaded, not blocking |
| GA4 / analytics | Important (data) | Load async, defer gtag |
| Facebook Pixel | Optional (retargeting) | Load after 3s delay or on first interaction |
| Hotjar / heatmap | Low (insights) | Load after 5s delay |
| Live chat | Low (support) | Load on scroll or after 10s |
| A/B testing | Medium (optimization) | Anti-flicker snippet ONLY if needed |

**3.2 — Slot Game / Casino Preview iFrame Optimization**:
```html
<!-- Casino comparison pages embed game previews — LAZY LOAD ALL -->
<!-- BEFORE: eager load (blocks INP) -->
<iframe src="https://casino-demo.com/slot-preview" width="600" height="400"></iframe>

<!-- AFTER: lazy load with Intersection Observer -->
<iframe data-src="https://casino-demo.com/slot-preview"
        width="600" height="400" loading="lazy"
        class="lazy-iframe" title="Slot Game Preview"></iframe>
<script>
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const iframe = entry.target;
      iframe.src = iframe.dataset.src;
      observer.unobserve(iframe);
    }
  });
}, {rootMargin: '200px'});
document.querySelectorAll('.lazy-iframe').forEach(f => observer.observe(f));
</script>
```

**3.3 — Comparison Table Optimization**:
- Casino comparison tables with 10+ operators can freeze the main thread on mobile
- Solution: Virtualize table rendering for >5 rows on mobile (show 5, "Load More" button)
- Keep CTA buttons in first 3 rows always visible (conversion priority)

---

### Pas 4: FIX CLS (Cumulative Layout Shift) — Bonus Banner & Ad Stability

**4.1 — Reserve Space for Dynamic Elements**:
```html
<!-- Casino bonus banners that load dynamically — ALWAYS reserve space -->
<div style="min-height: 250px; min-width: 300px; contain: layout;">
  <!-- Bonus banner loads here — space pre-reserved -->
</div>

<!-- ALL images must have width + height attributes -->
<img src="casino-logo.webp" width="200" height="80" alt="Casino Logo"
     loading="lazy" decoding="async">

<!-- Operator logos in comparison tables -->
<img src="betsson-logo.webp" width="120" height="40" alt="Betsson"
     loading="lazy" decoding="async">
```

**4.2 — Font Flash Prevention**:
```css
/* Prevent layout shift from font loading */
body {
  font-family: 'CasinoBrand', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}
/* size-adjust prevents layout shift when fallback → custom font swap occurs */
@font-face {
  font-family: 'CasinoBrand';
  src: url('casino-brand.woff2') format('woff2');
  font-display: swap;
  size-adjust: 105%; /* Adjust to match fallback font metrics */
}
```

**4.3 — Ad Slot Stabilization**:
- If running display ads (Ezoic/Mediavine): pre-define ad container dimensions
- Sticky ads: use `position: sticky` not dynamic insertion
- Interstitials: avoid on mobile entirely (Google penalizes mobile interstitials)

**4.4 — Sticky CTA Buttons (Casino-Specific)**:
```css
/* Casino sites benefit from sticky CTA — but it MUST NOT cause CLS */
.sticky-cta {
  position: sticky;
  bottom: 0;
  z-index: 100;
  height: 60px; /* Fixed height prevents CLS */
  contain: layout style;
}
```

---

### Pas 5: IMPLEMENTATION PRIORITY QUEUE — Phased Rollout

**Phase 1 (Day 1-2, Maximum Impact, Minimum Effort)**:
1. Install Cloudflare (free) → CDN + WebP auto-conversion + Brotli + minify
2. Add `loading="lazy"` on ALL images below the fold
3. Add `width` + `height` attributes on ALL `<img>` tags
4. Add `defer` attribute to ALL non-critical scripts
5. Preload LCP image with `<link rel="preload">`

**Phase 2 (Day 3-5, High Impact, Medium Effort)**:
6. Convert ALL images to WebP format (Squoosh.app or Sharp CLI)
7. Implement 3-second delay loading for non-critical tracking scripts
8. Upgrade hosting if TTFB >600ms (Cloudflare Workers if budget-constrained)
9. Reserve space for ALL dynamic elements (bonus banners, ads, lazy content)
10. Add `fetchpriority="high"` to LCP element

**Phase 3 (Day 6-10, Medium Impact, Higher Effort)**:
11. Inline critical CSS (above-the-fold styles) + async load non-critical CSS
12. Implement lazy loading for iFrames (slot previews, video embeds)
13. Font subsetting (load only characters needed for target language)
14. Service Worker for static asset caching (logos, icons, CSS, JS)
15. Implement `contain: layout` on dynamic widget containers

**Phase 4 (Ongoing, Maintenance)**:
16. Monthly CWV audit via GSC Core Web Vitals report
17. Test every new page template before publishing
18. Monitor CrUX data quarterly for real-user regression
19. Re-audit after any plugin update, theme change, or tracking script addition

---

### Pas 6: CASINO-SPECIFIC PERFORMANCE PATTERNS

**6.1 — Comparison Table Performance**:
- Tables with 10+ casino operators, each with logo + rating + bonus + CTA = heavy
- Solution: CSS Grid layout (not HTML table), lazy-load logos below fold, skeleton loading for above-fold entries
- Target: comparison page LCP <2.5s even with 15 operators

**6.2 — Multi-Provider Affiliate Tracking Coexistence**:
- Each casino affiliate program (Betsson, 1xBet, Betway) has its own tracking pixel
- Loading 5+ tracking pixels = 500ms+ main thread blocking
- Solution: Single consolidated tracking handler that loads pixels sequentially after DOMContentLoaded, with 500ms gaps between each

**6.3 — Mobile-First for Kenya**:
- Test on throttled connection: Chrome DevTools → Network → Slow 3G
- Target: full page usable within 4s on 3G (not just LCP, but interactive)
- Simplify mobile layout: single-column comparison, expandable operator details (not all-visible)
- Compress ALL assets aggressively — Kenya bandwidth costs users money per MB

**6.4 — AMP Consideration (Kenya Only)**:
- For Kenya market where 3G dominance is extreme, consider AMP versions of key landing pages
- AMP Casino Pages rank faster and load in <1s on Google's cache
- Limitation: AMP restricts JavaScript → affiliate tracking requires AMP-compatible implementation

---

### Pas 7: VALIDATION & ONGOING MONITORING

**7.1 — Post-Fix Validation**:
- Re-run PageSpeed Insights on ALL optimized pages
- Target: ALL metrics green (LCP <2.5s, INP <200ms, CLS <0.1) on mobile
- If any metric still failing: identify specific element (DevTools Performance tab) and iterate

**7.2 — Regression Prevention**:
- Before adding ANY new script/plugin/widget: test impact on staging first
- WordPress: use Query Monitor plugin to track script load impact
- Set up automated CWV monitoring: SpeedCurve ($10/mo) or free CrUX Dashboard in Looker Studio

**7.3 — Competitive Monitoring**:
- Re-audit top 3 competitors quarterly
- If competitor improves CWV significantly → re-optimize to maintain advantage
- Track correlation: CWV improvement → ranking change (document in Cortex)

---

## §3 Cortex Logging

```json
{
  "text": "CWV optimization executed for {domain} targeting {market}. LCP: {before}→{after}s. INP: {before}→{after}ms. CLS: {before}→{after}. PageSpeed mobile: {before}→{after}/100. Phase {N} fixes applied: {list}. Competitor comparison: our CWV vs top 3 average.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CORE-WEB-VITALS-CASINO",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-018",
    "version": "2.0",
    "tags": ["cwv", "core-web-vitals", "lcp", "inp", "cls", "performance", "casino", "technical-seo", "mobile-first", "kenya", "peru", "chile"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: Every casino money site at launch. Post-redesign. Monthly audit. After any plugin/script addition.

**WHEN**: Before launch (baseline + Phase 1-3 fixes). Monthly (GSC CWV report review). Quarterly (full re-audit with competitor benchmarking).

**HOW**:
1. Run PageSpeed Insights → document baseline scores per page
2. Identify top 3 issues per metric (LCP/INP/CLS) using DevTools
3. Apply fixes in priority order (Phase 1 → 2 → 3 → 4)
4. Re-run PageSpeed → confirm improvement → document delta
5. GSC → Core Web Vitals → verify "Poor" URLs resolved
6. Competitor CWV benchmark → confirm advantage maintained
7. Set up automated monitoring for regression alerts

**Violations**:
- Site launched without CWV baseline audit → violation CRITICAL
- "Poor" CWV scores ignored for >30 days → violation
- New tracking script added without CWV impact test → violation
- Kenya pages not tested on throttled 3G connection → violation
- CWV regression after plugin update not investigated within 48h → violation

**CONNECT**:
- → `SCHEMA-MARKUP-CASINO.md` (TRAIN-SEO-017) — technical SEO suite
- → `SEMANTIC-TOPICAL-AUTHORITY-CASINO.md` (TRAIN-SEO-016) — CWV + topical authority = complete ranking signal
- → `CASINO-MARKET-ANALYSIS.md` (TRAIN-SEO-001) — include CWV competitive analysis in market audit
- → `KENYA-ATTACK-STRATEGY.md` (TRAIN-SEO-014) — mobile-first CWV is critical path for Kenya
- → `CHILE-ATTACK-STRATEGY.md` (TRAIN-SEO-015) — sophisticated audience expects fast UX

**VERIFY**:
`[VK-018-A] PageSpeed mobile ALL GREEN: LCP <2.5s, INP <200ms, CLS <0.1 — per-page scores documented`
`[VK-018-B] CDN active: TTFB <200ms confirmed from target country (VPN test)`
`[VK-018-C] All images: WebP format, width+height specified, lazy loading below fold`
`[VK-018-D] Tracking scripts: ALL deferred or delayed — zero render-blocking JS`
`[VK-018-E] Kenya mobile: page usable within 4s on throttled 3G connection`

---

## §5 Dependențe

| Tool | Purpose | Cost |
|------|---------|------|
| **PageSpeed Insights** | Per-page CWV audit (lab + field data) | Free |
| **Chrome DevTools** | Deep performance profiling, waterfall analysis | Free |
| **Google Search Console** | CWV report per URL, regression tracking | Free |
| **Cloudflare** (free tier minimum) | CDN + WebP + Brotli + minify + edge caching | Free-$20/mo |
| **WebPageTest.org** | Waterfall analysis, competitor benchmarking | Free |
| **Squoosh.app** / Sharp CLI | Image compression to WebP | Free |
| **CrUX Dashboard** (Looker Studio) | Real-user CWV monitoring | Free |
| **SpeedCurve** (optional) | Automated CWV monitoring with alerts | $10+/mo |
| **Query Monitor** (WordPress) | Script/plugin performance impact tracking | Free |

**DISCLAIMER**: This procedure describes web performance optimization techniques for casino/gambling affiliate sites. These are standard web development practices applicable to any website. Verify the legality of casino operations in each target jurisdiction.
