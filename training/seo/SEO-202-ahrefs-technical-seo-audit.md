---
type: procedure
created: 2026-03-22
status: active
slug: seo-202-ahrefs-technical-seo-audit
tags: [procedure, nexus]
---

# SEO-202: Ahrefs Technical SEO Audit

---
id: SEO-202
domain: seo
type: implementation
complexity: medium
forge_score: 3.50
source: "Ahrefs Academy — SEO Course for Beginners Module 4 (Sam Oh), How to Use Ahrefs Course (62 lessons)"
created: 2026-03-20
status: ACTIVE
version: "1.0"
rule: META-H-002
scope: "Comprehensive technical SEO audit using Ahrefs Site Audit — crawl configuration, issue prioritization, fix verification, and ongoing monitoring."
---

## 1. Obiectiv

Comprehensive technical SEO audit workflow using Ahrefs Site Audit. Technical SEO issues silently destroy rankings — broken links, slow pages, missing meta tags, duplicate content, and crawl errors prevent Google from properly indexing and ranking content. This procedure provides a systematic audit workflow from crawl setup to continuous monitoring.

Situații acoperite:
- New website technical SEO baseline audit
- Post-migration technical audit (domain change, CMS migration, redesign)
- Quarterly technical health check
- Diagnosing sudden ranking or traffic drops
- Pre-launch technical readiness check

> **Note**: Requires Ahrefs Standard plan or above with Site Audit access. Site verification required for full crawl.

---

## 2. Pași

### Pas 1: Configure Site Audit Project
Open Ahrefs > Site Audit > "New Project." Enter domain and verify ownership (DNS, HTML file, or meta tag). Configure crawl settings: Crawl limit = all pages (or set cap for large sites >100K pages), Crawl speed = moderate (avoid overloading server), User-agent = Ahrefs Bot, Follow robots.txt = Yes, JavaScript rendering = Enable if SPA/React/Angular. Add sitemap URL. Schedule crawl: weekly for active sites, monthly for stable sites. Run initial crawl.

### Pas 2: Review Health Score and Issue Overview
Once crawl completes, check the Health Score (0-100). Review the "All Issues" dashboard. Ahrefs categorizes issues as: Errors (critical, blocks indexing), Warnings (important, degrades performance), Notices (minor, best practice). Note total pages crawled, pages with issues percentage, and top issue categories. Screenshot the overview for baseline documentation.

### Pas 3: Fix Critical Errors First — Indexability
Filter issues by "Errors" priority. Address in order:
(a) **4xx/5xx HTTP errors** — broken pages returning error codes. Fix: redirect or restore content.
(b) **Noindex on important pages** — pages accidentally blocked from indexing. Fix: remove noindex tag.
(c) **Orphan pages** — pages with no internal links. Fix: add internal links from relevant pages.
(d) **Redirect chains/loops** — Fix: point all redirects to final destination directly.
(e) **Canonical issues** — conflicting or self-referencing canonical tags. Fix: ensure each page has correct canonical.
For each fix, note the URL, issue, and action taken.

### Pas 4: Address Performance Warnings — Speed and Core Web Vitals
In Site Audit > "Performance" report. Check:
(a) **Slow pages** (>3s TTFB) — identify server/hosting issues.
(b) **Large page size** (>3MB) — compress images, minify CSS/JS.
(c) **Large images** — optimize with WebP, lazy loading.
(d) **Render-blocking resources** — defer non-critical JS/CSS.
(e) **Missing image dimensions** — causes CLS (layout shift).
Cross-reference with Google PageSpeed Insights for Core Web Vitals (LCP, FID, CLS) per top 20 traffic pages.

### Pas 5: Audit On-Page SEO Elements
In Site Audit > "Content" section. Check:
(a) **Missing/duplicate title tags** — each page needs unique, keyword-relevant title <60 chars.
(b) **Missing/duplicate meta descriptions** — unique, compelling, <160 chars.
(c) **Missing H1 tags** or multiple H1s — one H1 per page, includes primary keyword.
(d) **Thin content** — pages with <200 words (check if they should be expanded or noindexed).
(e) **Duplicate content** — use "Duplicate content" report, set up canonicals.
(f) **Missing alt text on images** — add descriptive alt text with natural keyword inclusion.

### Pas 6: Check Internal Linking Structure
In Site Audit > "Links" section. Review:
(a) **Internal link distribution** — check "Internal pages" report for pages with 0-1 internal links.
(b) **Link depth** — pages should be reachable within 3 clicks from homepage. Flag pages at depth 4+.
(c) **Broken internal links** — fix or remove immediately.
(d) **Excessive links per page** — pages with >100 outgoing links dilute link equity.
Use Ahrefs "Internal backlinks" column to find high-authority pages that could distribute more link equity via strategic internal links.

### Pas 7: Validate Sitemaps and Robots.txt
Check sitemap(s): do they include all important pages? Are any 4xx/redirected URLs in the sitemap? Does page count in sitemap match crawled pages? Check robots.txt: are any important directories/pages accidentally blocked? Is the sitemap URL referenced in robots.txt? Validate XML sitemap format (no errors in Ahrefs sitemap report).

### Pas 8: Structured Data and Schema Validation
Check for existing schema markup using Site Audit structured data report. Verify: Organization schema on homepage, LocalBusiness (if applicable), Article/BlogPosting on content pages, Product schema on product pages, BreadcrumbList for navigation, FAQ schema where applicable. Flag pages missing expected schema types. Validate via Google Rich Results Test for top 10 traffic pages.

### Pas 9: Compile Audit Report and Track Fixes
Export all issues from Ahrefs Site Audit. Create prioritized fix list:
- **P0 (this week)**: Errors blocking indexing — 4xx, noindex, redirect loops
- **P1 (this month)**: Performance issues — slow pages, Core Web Vitals fails
- **P2 (next month)**: Content issues — duplicate titles, thin content, missing alt text
- **P3 (ongoing)**: Best practices — internal linking improvements, schema additions
Assign each fix to responsible team member. Set re-crawl date after P0 fixes to verify resolution. Compare Health Score before and after fixes.

### Pas 10: Set Up Continuous Monitoring
Enable Ahrefs Site Audit scheduled crawls (weekly). Set up email alerts for: Health Score drops > 5 points, new Errors detected, pages dropping from index. Review crawl comparison report after each scheduled crawl — "Changes" tab shows new vs. fixed issues. Monthly: export trend data (Health Score, total issues, pages crawled) for reporting.

---

## 3. Verificare

- [ ] Site Audit project configured with correct crawl settings?
- [ ] Health Score baseline documented?
- [ ] Critical errors (P0) identified and fixed?
- [ ] Core Web Vitals checked for top traffic pages?
- [ ] On-page elements audited (titles, metas, H1s, alt text)?
- [ ] Internal linking structure reviewed?
- [ ] Sitemaps and robots.txt validated?
- [ ] Schema markup verified?
- [ ] Prioritized fix list delivered with assignments?
- [ ] Scheduled crawls and alerts configured?
- [ ] All steps from §2 executed?
- [ ] VK emis in sesiune?

---

## 4. Instrumente

| Instrument | Rol |
|-----------|-----|
| Ahrefs Site Audit | Crawling, issue detection, health scoring, monitoring |
| Ahrefs Site Explorer | Cross-reference traffic data for prioritization |
| Google PageSpeed Insights | Core Web Vitals validation (LCP, FID, CLS) |
| Google Search Console | Index coverage verification, crawl stats |
| Google Rich Results Test | Schema markup validation |
| Screaming Frog (optional) | Complementary desktop crawl for deep audits |

---

## 5. Note

- Always fix P0 (indexing blockers) before P1 (performance) — priority order matters
- Always schedule a re-crawl after fixing P0 issues to verify resolution
- Never skip JavaScript rendering configuration if site uses SPA/React/Angular — crawl misses content otherwise
- Core Web Vitals should be checked on actual top-traffic pages, not just homepage
- Internal link depth >4 clicks buries pages from both users and crawlers — restructure navigation
- Excessive links per page (>100) dilute link equity — prune or nofollow non-essential links
- Schema markup should match actual page content — misleading schema triggers manual actions
- Continuous monitoring prevents regression — weekly crawls catch new issues before they impact rankings

---

## 6. Cortex Logging

```json
{
  "text": "Executed SEO-202: Ahrefs Technical SEO Audit — crawl configured, health score baselined, critical errors fixed, performance optimized, on-page audited, internal links reviewed, sitemaps validated, schema checked, monitoring active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SEO-202",
    "domain": "seo",
    "rule_id": "META-H-002",
    "tags": ["seo", "technical-seo", "ahrefs", "site-audit", "core-web-vitals", "crawl", "indexing", "schema"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 7. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Crawl run without proper configuration (JS rendering, sitemap) → violation
- Critical errors (P0) not prioritized before warnings → violation
- No re-crawl scheduled after fixes → violation
- Continuous monitoring not enabled → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum seo
- SEO-201 → complements (link-worthy pages need technical health)
- SEO-203 → depends on (keyword research informs which pages to prioritize)
- SEO-204 → complements (competitor technical audit comparison)

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Site Audit project configured with correct crawl settings?
- [ ] Health Score baseline documented?
- [ ] Critical errors (P0) identified and fixed?
- [ ] Core Web Vitals checked for top traffic pages?
- [ ] On-page elements audited (titles, metas, H1s, alt text)?
- [ ] Internal linking structure reviewed?
- [ ] Sitemaps and robots.txt validated?
- [ ] Schema markup verified?
- [ ] Prioritized fix list delivered with assignments?
- [ ] Scheduled crawls and alerts configured?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] SEO-202 | S1-S10 DONE | technical audit complete`
2. `VK [CORTEX] "Ahrefs Technical SEO Audit" | FORGE 1.3 | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 8. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs Site Audit | Crawling, issue detection, health scoring |
| Ahrefs Site Explorer | Cross-reference traffic data for prioritization |
| Google PageSpeed Insights | Core Web Vitals validation |
| Google Search Console | Index coverage verification |
| Google Rich Results Test | Schema markup validation |

---

## 9. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Health Score | > 85/100 |
| Critical errors (P0) | 0 after fix cycle |
| Core Web Vitals pass rate | > 75% of top pages |
| Pages with unique title + meta | 100% |
| Internal link depth | < 4 clicks for all important pages |
| Scheduled crawl cadence | Weekly |
