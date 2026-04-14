---
type: procedure
created: 2026-03-22
status: active
slug: m-018-featured-snippet-optimization
tags: [procedure, nexus]
---

# Featured Snippet Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Targeted optimization process for capturing Google featured snippets (position zero) by identifying snippet opportunities, analyzing snippet formats, restructuring content to match, and tracking snippet capture rate over time.

---

## 1. Problema

Featured snippets (position zero) capture disproportionate click-through rates and are the primary source content for Google AI Overviews. Pages ranking in positions 2-10 for question-type queries are wasting ranking equity by failing to format content for snippet capture. Without structured snippet optimization, competitors monopolize position zero for high-value query types.

Types of situations covered:
- Pages ranking positions 2-10 for question queries — highest-probability snippet candidates
- Informational content — how-to guides, definitions, step-by-step processes
- Comparison content — eligible for table-format snippets
- FAQ sections — eligible for multiple snippet captures per page
- AI Overview optimization — featured snippets are primary source for AI-generated answers

---

## 2. Procedura

### Pas 1: Identify Featured Snippet Opportunities
In Google Search Console, filter queries by: average position between 2 and 10, minimum 100 monthly impressions, and containing question words (what, how, why, when, which, who) or comparison terms (vs, best, difference between). Cross-reference with Semrush Position Tracking filtered for featured snippet SERP feature. Also check Ahrefs "SERP features" filter for keywords where a featured snippet exists but is not owned by your site. Export the combined list — these are your primary opportunities.

### Pas 2: Analyze Current Snippet Format for Each Opportunity
For each opportunity keyword, trigger the SERP and identify the current featured snippet. Categorize the snippet type:
- **Paragraph**: 2-4 sentence direct answer to a question
- **Numbered list**: Step-by-step process or ranked list
- **Bulleted list**: Unranked collection of items, features, or examples
- **Table**: Comparative data with rows and columns
- **Video**: YouTube video optimized for a specific question
Document the snippet format, approximate word count, and structural pattern. Your content must match or improve on this format to compete.

### Pas 3: Restructure Content to Match Snippet Format
For each target page, implement the matching structure:
- **For paragraph snippets**: Add a concise answer block directly under the H2 that mirrors the query. Write 40-60 words. Start with the topic term. No filler phrases like "In this article..." — lead directly with the answer.
- **For list snippets**: Use a properly formatted HTML ordered (numbered) or unordered (bulleted) list immediately following the H2. 5-8 items. Each list item concise and parallel in structure.
- **For table snippets**: Add an HTML table with clear headers, matching the comparison structure in the existing snippet.
Maintain the rest of the page's depth and detail — the snippet block is an addition, not a replacement.

### Pas 4: Concise Answer Block in First 50 Words
Ensure that for every question-type H2 on the page, the first 50 words following it contain a complete, standalone answer to the question. Google extracts snippet content algorithmically — the direct-answer block must be immediately followable by supporting detail, not preceded by preamble. Review all H2/H3 headings on target pages to ensure they are phrased as or close to the search query.

### Pas 5: Structured Q&A Format for FAQ Sections
For pages with FAQ sections, implement a strict format: question as H3, followed immediately by a 2-3 sentence answer, then optional longer explanation. Apply FAQPage schema (JSON-LD) to the entire FAQ block. Each Q&A pair is independently eligible for a featured snippet. Aim for 5-10 Q&A pairs per page targeting distinct related questions from the keyword cluster.

### Pas 6: Monitor and Iterate
Set up weekly position tracking in Semrush or Ahrefs for all snippet opportunity keywords. Track: featured snippet won (yes/no), current position, click-through rate change. After 4-6 weeks post-optimization, review results. For queries still without snippet capture: revisit the format (try alternative structure), adjust word count, or improve factual specificity. Snippets are dynamic — competitors can recapture them. Monitor monthly and re-optimize as needed.

---

## 3. Cortex Logging

```json
{
  "text": "Featured Snippet Optimization completed for [domain/pages]. Opportunities identified: [n]. Pages optimized: [n]. Snippet types targeted: [paragraph/list/table]. Snippets captured post-optimization: [n].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-018",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["featured-snippets", "position-zero", "seo", "content-optimization", "serp-features"],
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
- Pages optimizate fără snippet format analysis prealabil → violation
- FAQ sections fără FAQPage schema aplicat → violation
- Optimizare featured snippet fără verificarea că pagina e deja în top 10 → violation
- Format snippet (paragraph/list/table) ales fără analiza SERP-ului actual → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Conectat cu M-016 (GEO) — featured snippets = primary AI Overview sources

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Snippet format identificat și conținut restructurat?
- [ ] Answer block în primele 50 cuvinte după H2?
- [ ] Tracking setat pentru toate opportunity keywords?

**VK-uri obligatorii:**
1. `✅ [PROC] M-018 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Featured Snippet Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Search Console | Query data for opportunity identification |
| Semrush / Ahrefs Position Tracking | SERP feature tracking and monitoring |
| Google Rich Results Test | FAQPage schema validation |
| SERP analysis (manual) | Snippet format identification |
| CMS | Content restructuring and schema implementation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Snippet opportunities identified | Minimum 20 per domain |
| Pages optimized per sprint | Minimum 5 |
| Snippet capture rate | 20%+ of targeted opportunities within 60 days |
