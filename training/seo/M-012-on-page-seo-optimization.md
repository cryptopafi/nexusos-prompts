---
type: procedure
created: 2026-03-22
status: active
slug: m-012-on-page-seo-optimization
tags: [procedure, nexus]
---

# On-Page SEO Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Systematic on-page SEO optimization covering title tags, meta descriptions, heading hierarchy, keyword placement, URL structure, image optimization, internal linking, and schema markup for individual pages or site-wide templates.

---

## 1. Problema

Pages with strong backlink profiles or good content still underperform when on-page signals are poorly configured. Missing or duplicate title tags, flat heading structures, unoptimized images, and absent schema markup leave significant ranking potential uncaptured.

Types of situations covered:
- New page creation — on-page checklist before publish
- Existing page optimization — updating underperforming pages
- Site-wide template audit — fixing systemic on-page issues
- Content refresh — realigning a page to a new or updated keyword target
- E-E-A-T signal strengthening on authoritative content

---

## 2. Procedura

### Pas 1: Title Tag Optimization
Write title tag within 55-60 characters (to avoid SERP truncation). Place primary keyword as close to the beginning as possible. Include brand name at the end separated by a pipe or dash only if space allows. Avoid keyword stuffing — the title must read naturally. Verify uniqueness: no other page on the site should have the same or near-identical title.

### Pas 2: Meta Description Optimization
Write meta description of 140-160 characters. Include the primary keyword and one secondary keyword naturally. Include a clear value proposition or call to action (e.g., "Learn how to...", "Get your free audit..."). Meta descriptions do not directly affect rankings but impact CTR — treat it as ad copy. Ensure uniqueness across all pages.

### Pas 3: Heading Hierarchy (H1/H2/H3)
Confirm exactly one H1 per page. H1 must contain the primary keyword. Structure content with H2s for major sections and H3s for subsections. Each H2 should target a secondary keyword or topically related term. Avoid skipping heading levels. Headings should read as a logical outline when extracted in isolation.

### Pas 4: Keyword Placement and Content Quality
Place primary keyword in: first 100 words, at least one H2, and naturally throughout the body (target 1-2% keyword density — no stuffing). Include LSI terms and semantically related phrases identified in keyword research. Minimum content length: match or exceed the average word count of top 3 ranking pages for the target keyword. Add original insights, data, or examples to improve E-E-A-T signals.

### Pas 5: URL Structure
Ensure URL is: lowercase, hyphenated (no underscores or spaces), contains primary keyword, and is as short as possible while remaining descriptive. Remove stop words (the, a, and, of) unless essential for readability. Avoid dynamic parameters in indexable URLs. If URL needs to change on an existing page, implement a 301 redirect from old to new.

### Pas 6: Image Optimization and Alt Text
Compress all images: use WebP format where supported, target under 100KB per image. Set descriptive alt text on every image — include the primary keyword on the hero/featured image, use descriptive natural language on supporting images. Add title attributes where relevant. Ensure no images are blocking content rendering (lazy load below-the-fold images).

### Pas 7: Internal Linking
Add 3-5 internal links from this page to related deeper-funnel pages (e.g., from blog post to service page). Add internal links from 2-3 existing high-authority pages on the site pointing to this page using keyword-rich anchor text. Avoid over-optimization of anchors — vary the anchor text naturally. Verify all internal links resolve correctly with no redirects.

### Pas 8: Schema Markup Implementation
Identify the appropriate schema type for the page (Article, Product, FAQ, HowTo, LocalBusiness, BreadcrumbList). Implement JSON-LD schema in the page head. Validate with Google Rich Results Test — zero errors required before publish. For FAQ pages: implement FAQPage schema on all question-answer blocks. For articles: include datePublished, dateModified, author, and publisher fields.

---

## 3. Cortex Logging

```json
{
  "text": "On-Page SEO Optimization completed for [URL/page]. Title: [x chars]. Meta: [x chars]. Headings: H1✓ H2:[n] H3:[n]. Schema: [type]. Internal links added: [n]. Optimization complete.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-012",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["on-page-seo", "title-tags", "schema-markup", "internal-linking", "content-optimization"],
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
- Title tag fără keyword sau peste 60 caractere → violation
- Schema markup nevalidat cu Rich Results Test → violation
- Optimizare aplicată fără baseline de trafic documentat → violation
- Meta tags lipsă (title/description) după optimizare → violation
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
- [ ] Title tag 55-60 chars cu primary keyword?
- [ ] Schema markup validat cu zero erori?
- [ ] Internal links adăugate (minim 3)?

**VK-uri obligatorii:**
1. `✅ [PROC] M-012 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "On-Page SEO Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Screaming Frog / Semrush | Bulk on-page element extraction |
| Google Rich Results Test | Schema markup validation |
| Google Search Console | CTR data for meta description optimization |
| PageSpeed Insights | Image and rendering performance |
| CMS (WordPress/Webflow/etc.) | On-page element editing |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Title tag compliance | 100% within 55-60 chars with keyword |
| Schema validation errors | 0 per page |
| Internal links per optimized page | Minimum 3 added |
