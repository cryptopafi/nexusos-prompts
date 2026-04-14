---
type: procedure
created: 2026-03-22
status: active
slug: m-010-technical-seo-audit
tags: [procedure, nexus]
---

# Technical SEO Audit — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Full technical SEO audit covering crawlability, indexability, Core Web Vitals, structured data, and site architecture to identify and prioritize technical issues blocking organic growth.

---

## 1. Problema

A website may have excellent content but rank poorly due to undetected technical barriers. Without a structured technical SEO audit, crawl errors, indexability issues, poor Core Web Vitals, and missing structured data remain invisible until traffic drops significantly.

Types of situations covered:
- New client onboarding — baseline technical health assessment
- Traffic drop investigation — identifying technical root cause
- Pre-launch site migration or redesign review
- Quarterly technical health check for ongoing SEO clients
- Penalty recovery diagnosis

---

## 2. Procedura

### Pas 1: Crawl the Site with Screaming Frog or Semrush
Configure Screaming Frog (or Semrush Site Audit) with the target domain. Set crawl depth to full, enable JavaScript rendering for SPAs, and connect Google Analytics and Search Console integrations. Export the full crawl report. Flag all 4xx/5xx status codes, redirect chains longer than 2 hops, and pages with missing or duplicate title/meta tags.

### Pas 2: Analyze Core Web Vitals
Pull Core Web Vitals data from Google Search Console (CWV report) and PageSpeed Insights for both mobile and desktop. Record LCP, INP, and CLS scores for the top 20 most-trafficked URLs. Identify pages in the "Poor" or "Needs Improvement" band. Cross-reference with Lighthouse audit to isolate root causes (render-blocking resources, large images, layout shifts).

### Pas 3: Check Indexability, robots.txt, and Sitemap
Review robots.txt for unintended disallow rules blocking important pages. Validate XML sitemap: confirm it is submitted in Search Console, all URLs return 200 status, canonical tags match sitemap URLs, and no noindex pages are included. Check for orphan pages (indexed but not in sitemap and not internally linked).

### Pas 4: Audit Internal Linking Architecture
Map internal link distribution using the crawl data. Identify pages with zero internal links (orphan pages), pages with excessive inbound links (over-concentrated authority), and broken internal links. Verify that the most strategically important pages receive the highest internal PageRank. Flag anchor text diversity issues and generic anchors ("click here").

### Pas 5: Validate Structured Data
Use Google's Rich Results Test and Schema Markup Validator on key page templates (homepage, product pages, articles, local pages). Check for errors, warnings, and missing required fields. Confirm that implemented schema types match the content type on each page. Verify rich result eligibility in Search Console's Enhancement reports.

### Pas 6: Mobile-First and PageSpeed Analysis
Run Google Mobile-Friendly Test on 5 representative page templates. Confirm viewport meta tag presence, tap target sizing, and absence of intrusive interstitials. Run PageSpeed Insights for mobile. Document Time to First Byte (TTFB), First Contentful Paint (FCP), and Total Blocking Time (TBT). Prioritize fixes by impact: server response time first, then render-blocking resources, then image optimization.

### Pas 7: Fix Prioritization and Remediation Roadmap
Compile all issues into a prioritized matrix: Critical (blocks crawling/indexing), High (directly impacts rankings), Medium (performance and UX), Low (cosmetic/minor). Assign estimated fix effort (hours) and expected impact. Deliver a remediation roadmap with owner, deadline, and acceptance criteria for each item.

---

## 3. Cortex Logging

```json
{
  "text": "Technical SEO Audit completed for [domain]. Critical issues: [n]. High priority: [n]. CWV status: LCP [x]s / INP [x]ms / CLS [x]. Remediation roadmap delivered.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-010",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["technical-seo", "audit", "core-web-vitals", "crawl", "indexability"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Audit finalizat fără remediation roadmap prioritizat → violation
- CWV data absent din raport → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Crawl data exportat și analizat?
- [ ] Core Web Vitals documentate pentru top 20 URL-uri?
- [ ] Remediation roadmap cu prioritizare livrată?

**VK-uri obligatorii:**
1. `✅ [PROC] M-010 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Technical SEO Audit" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Screaming Frog / Semrush Site Audit | Full site crawl and issue detection |
| Google Search Console | Index coverage, CWV data, rich results |
| Google PageSpeed Insights / Lighthouse | Performance scoring and diagnostics |
| Google Rich Results Test | Structured data validation |
| Google Analytics | Traffic data for prioritization |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Critical issues identified | 0 undetected post-audit |
| Remediation roadmap coverage | 100% issues triaged |
| CWV documentation | All top 20 URLs scored |
