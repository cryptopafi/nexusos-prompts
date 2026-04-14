---
type: procedure
created: 2026-03-22
status: active
slug: m-014-content-audit-gap-analysis
tags: [procedure, nexus]
---

# Content Audit and Gap Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Systematic content audit process covering full page inventory, performance classification (keep/update/merge/delete), competitive gap identification, and prioritized content execution roadmap.

---

## 1. Problema

Sites accumulate content over time that becomes outdated, duplicative, thin, or misaligned with current keyword targets. Without regular content audits, thin content dilutes domain authority, cannibalization fragments ranking signals, and genuine content gaps leave high-value traffic opportunities uncaptured.

Types of situations covered:
- Annual content strategy reset — full site inventory and health check
- Pre-migration planning — deciding what content to migrate vs. retire
- Traffic decline investigation — identifying underperforming content as root cause
- Competitor content parity — closing gaps vs. top-ranking competing sites
- E-E-A-T improvement — identifying and upgrading low-quality content

---

## 2. Procedura

### Pas 1: Crawl All Indexable Pages
Use Screaming Frog or Semrush to crawl the full site. Export all indexable URLs (status 200, no noindex). Combine with Google Search Console URL list to capture any pages not found in crawl. Remove pagination, tag/category archives (unless they rank), and parameter URLs from the working list. Final inventory = all canonicalized, indexable content pages.

### Pas 2: Pull Performance Data for Every Page
For each URL in the inventory, attach from Google Analytics and Search Console: organic sessions (last 12 months), organic impressions, average position for primary keyword, backlinks count (from Ahrefs/Semrush), and conversions or goal completions. Create a unified spreadsheet with all metrics per URL. This is the content audit master sheet.

### Pas 3: Categorize Each Page (Keep / Update / Merge / Delete)
Apply classification rules:
- **Keep**: High traffic + good rankings + strong backlinks + recent content. No action needed.
- **Update**: Moderate or declining traffic, outdated information, or ranking in positions 5-20. Refresh content, update statistics, expand coverage.
- **Merge**: Multiple thin pages covering same topic. Combine into one comprehensive page. 301 redirect merged pages to the winner URL.
- **Delete (+ redirect)**: Zero or near-zero organic traffic, no backlinks, thin content with no strategic value. 301 redirect to closest relevant page. Do not delete without redirecting.
Flag cannibalizing pages (two or more pages competing for the same primary keyword) — these require merge or differentiation.

### Pas 4: Competitive Content Gap Analysis
Identify top 5 organic competitors. Export their top-ranking content topics using Semrush Content Gap or Ahrefs Content Gap tool. Identify topics/keywords that competitors rank for but your site has zero content for. These are content gap opportunities. Cross-reference with keyword research (M-011) to prioritize gaps by volume and KD. Document as "New Content Required" items.

### Pas 5: Prioritize Updates and New Content
Score every action item (Update/Merge/Delete/New) by:
- **Traffic opportunity**: Estimated organic sessions if ranking improved
- **Effort**: Hours to execute (rewrite, merge, create from scratch)
- **Strategic fit**: Alignment with core business objectives
Build prioritized content backlog: Tier 1 (quick wins, high ROI), Tier 2 (strategic investments), Tier 3 (long-term). Tier 1 actions execute first.

### Pas 6: Execution Roadmap Delivery
Produce a 12-month content roadmap with: action type, target URL or new page title, assigned owner, target keyword, estimated publish/update date, and expected impact. Include a delete/redirect implementation plan with staging review before execution. Present to client/stakeholder for sign-off before content changes are made.

---

## 3. Cortex Logging

```json
{
  "text": "Content Audit completed for [domain]. Total pages audited: [n]. Keep: [n] | Update: [n] | Merge: [n] | Delete: [n]. Content gaps identified: [n]. Roadmap delivered.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-014",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["content-audit", "gap-analysis", "content-strategy", "seo", "content-pruning"],
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
- Pages categorized fără performance data atașat → violation
- Delete actions fără redirect plan → violation
- Audit realizat fără exportul complet al URL-urilor indexate → violation
- Decizie de consolidare/ștergere fără analiza traficului per pagină → violation
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
- [ ] Toate URL-urile clasificate în una din cele 4 categorii?
- [ ] Delete actions au redirect plan asociat?
- [ ] Content gaps documentate și prioritizate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-014 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Content Audit and Gap Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Screaming Frog / Semrush | Full site crawl and page inventory |
| Google Analytics | Traffic and conversion data per URL |
| Google Search Console | Impressions, clicks, and average position |
| Ahrefs / Semrush Content Gap | Competitor content gap analysis |
| Content management spreadsheet | Audit master sheet and classification |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Audit coverage | 100% of indexable pages classified |
| Delete actions with redirect | 100% (no unredirected deletions) |
| Content gaps documented | All Tier 1 and Tier 2 gaps captured |
