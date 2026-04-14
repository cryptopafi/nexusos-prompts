---
type: procedure
created: 2026-03-22
status: active
slug: m-011-keyword-research-strategy
tags: [procedure, nexus]
---

# Keyword Research and Strategy Development — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Structured keyword research process from seed keyword identification through topic clustering, intent mapping, and content gap prioritization to build a data-driven keyword universe for organic strategy.

---

## 1. Problema

SEO campaigns without structured keyword research target the wrong terms, misalign with user intent, or create cannibalizing content. Without a systematic process, keyword selection is arbitrary and disconnected from business objectives.

Types of situations covered:
- New website or domain — building keyword universe from scratch
- Existing site — expanding into new topic areas or audience segments
- Competitor catch-up — closing keyword gaps vs. top organic competitors
- Content team alignment — providing writers with targeted keyword briefs
- PPC and SEO keyword harmonization

---

## 2. Procedura

### Pas 1: Define Seed Keywords and Business Context
Interview client (or brief document) to identify: core product/service categories, target audience personas, geographic focus, and primary conversion goals. Extract 10-20 seed terms directly from the business model. These are non-negotiable terms that represent the core offering — not yet filtered by data.

### Pas 2: Expand Keyword Universe with Research Tools
Load seed keywords into Semrush Keyword Magic Tool, Ahrefs Keywords Explorer, and Google Keyword Planner. For each seed: pull related, phrase-match, and question variants. Also run Google Search Autocomplete and "People Also Ask" scraping for long-tail discovery. Target a raw list of 500-2000 keywords before filtering.

### Pas 3: Filter by Search Intent
Classify each keyword by intent type:
- **Informational**: "how to", "what is", "guide" — top-of-funnel content
- **Commercial**: "best", "vs", "review" — mid-funnel comparison content
- **Transactional**: "buy", "price", "hire" — bottom-funnel conversion pages
- **Navigational**: brand names — not targeted unless own brand
Remove navigational keywords for competitors. Flag transactional keywords as highest priority for revenue impact.

### Pas 4: Cluster Keywords by Topic
Group keywords semantically into topic clusters. Each cluster = one primary keyword (highest volume, most representative) + secondary and LSI terms (supporting). Use Semrush Keyword Grouping or manual clustering. A cluster becomes one content piece or page. Aim for clusters of 5-20 related terms.

### Pas 5: Prioritize by Keyword Difficulty vs. Search Volume
Score each cluster using a KD vs. Volume matrix:
- Quick wins: Low KD (under 30), moderate volume — target first
- Strategic: High volume, high KD — long-term content investments
- Niche: Low volume, low KD — thin content, FAQ, or programmatic pages
- Avoid: High KD, low volume — poor ROI
Document priority tier (1/2/3) for each cluster.

### Pas 6: Map Keywords to Pages and Identify Gaps
For existing sites: map each cluster to the closest existing URL. Flag cannibalization risks (multiple pages competing for same cluster). For gaps: clusters with no matching page are content gap opportunities. Prioritize gaps in Tier 1 clusters for new content creation.

### Pas 7: Deliver Keyword Strategy Document
Output: structured spreadsheet with columns — Keyword, Intent, Monthly Volume, KD, Cluster Name, Priority Tier, Target URL (existing or new), Notes. Include summary: total keywords, distribution by intent, top 10 priority clusters, and content gap count. Share with content and technical teams.

---

## 3. Cortex Logging

```json
{
  "text": "Keyword Research completed for [domain/project]. Total keywords: [n]. Clusters: [n]. Top priority cluster: [name]. Content gaps identified: [n]. Keyword universe delivered.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-011",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["keyword-research", "topic-clusters", "search-intent", "content-strategy", "seo"],
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
- Keyword list livrată fără intent classification → violation
- Clustere neidentificate sau keywords nemapate la pagini → violation
- Keyword research realizat fără analiza search intent → violation
- Cuvinte cheie selectate fără validarea volumului de search (min. 100/lună) → violation
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
- [ ] Intent classification aplicată pe toate keyword-urile?
- [ ] Topic clusters create și documentate?
- [ ] Priority tiers asignate și content gaps identificate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-011 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Keyword Research and Strategy Development" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Semrush Keyword Magic Tool | Keyword expansion and grouping |
| Ahrefs Keywords Explorer | KD scores and SERP analysis |
| Google Keyword Planner | Volume data and forecasting |
| Google Search Console | Current keyword performance baseline |
| Content calendar / CMS | Keyword-to-page mapping |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Keyword universe size | Minimum 200 clustered keywords |
| Intent classification coverage | 100% of keywords |
| Content gaps documented | All Tier 1 gaps mapped |
