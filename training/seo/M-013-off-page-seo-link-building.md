---
type: procedure
created: 2026-03-22
status: active
slug: m-013-off-page-seo-link-building
tags: [procedure, nexus]
---

# Off-Page SEO and Link Building Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: End-to-end off-page SEO and link building campaign covering backlink profile audit, competitor analysis, outreach pipeline setup, link acquisition execution, toxic link disavowal, and ongoing monitoring of referring domain growth.

---

## 1. Problema

Domain authority and link equity are critical ranking factors that cannot be improved through on-page optimization alone. Without a structured off-page strategy, sites stagnate in competitive SERPs, lose ground to competitors with active link building, and risk penalty from unmonitored toxic backlinks.

Types of situations covered:
- New domain — building initial authority from zero
- Competitive niche — closing authority gap vs. top-ranking competitors
- Penalty recovery — toxic link disavowal and authority rebuilding
- Brand mention conversion — unlinked mentions to backlinks
- Thought leadership — using content assets to attract editorial links

---

## 2. Procedura

### Pas 1: Backlink Profile Audit
Pull current backlink profile from Ahrefs or Semrush. Export all referring domains with: Domain Rating/Authority, anchor text, link type (dofollow/nofollow), first seen date, and status. Identify the ratio of dofollow to nofollow links, anchor text distribution (branded vs. exact-match vs. generic), and trend of new vs. lost referring domains over the past 6 months.

### Pas 2: Competitor Backlink Analysis
Identify the top 5 organic competitors by keyword overlap. Export their backlink profiles. Run a link gap analysis: domains linking to competitors but not to your site = highest-priority link opportunities (they are already predisposed to link to this topic). Export this list, sorted by Domain Rating descending. Top 50 become the primary outreach target list.

### Pas 3: Identify Link Acquisition Opportunities
Categorize opportunities from the gap analysis:
- **Guest posts**: Blogs/media in niche accepting contributor content
- **Resource/directory links**: Niche directories and curated resource pages
- **HARO/Connectively**: Journalist queries matching expertise
- **Brand mentions**: Unlinked mentions found via Google Alerts or Ahrefs Content Explorer
- **Broken link building**: Find broken links on high-DR pages pointing to dead competitor content
Score each by effort (1-5) and potential DR value. Build prioritized outreach queue.

### Pas 4: Outreach Campaign Setup
Create outreach templates for each opportunity type (guest post pitch, resource suggestion, broken link replacement). Personalize subject lines and opening lines for each contact. Use a CRM or outreach tool (Pitchbox, Hunter.io, BuzzStream) to manage: prospect, contact email, send date, follow-up schedule, and status. Send in batches of 20-30 per week to avoid spam signals.

### Pas 5: Link Acquisition and Content Execution
For confirmed guest post opportunities: deliver high-quality article (1500+ words) with one contextual dofollow link to the target page using agreed anchor text. For resource links: submit the asset URL with a brief pitch on why it deserves inclusion. For broken link building: provide the replacement URL that matches the dead content. Track every acquired link: URL, anchor text, DR, dofollow/nofollow, date acquired.

### Pas 6: Toxic Link Identification and Disavowal
Flag toxic backlinks from the audit: spammy directories, link farms, irrelevant foreign-language sites, sites with very low DR and high spam score. If toxic links cannot be removed manually (via contact request), compile a disavow file in Google format (domain:example.com per line). Submit via Google Search Console Disavow Tool. Document all disavowed domains and review monthly.

### Pas 7: Monitor Referring Domain Growth
Set up weekly monitoring in Ahrefs or Semrush: new referring domains, lost referring domains, DR changes, and anchor text distribution shifts. Track campaign KPIs monthly: total outreach sent, response rate, links acquired, average DR of new links, net referring domain change. Report on Domain Rating trend and its correlation with ranking improvements.

---

## 3. Cortex Logging

```json
{
  "text": "Off-Page SEO Campaign executed for [domain]. Outreach sent: [n]. Links acquired: [n]. Avg DR: [x]. Toxic links disavowed: [n]. Referring domains: [before] → [after].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-013",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["link-building", "off-page-seo", "backlinks", "outreach", "domain-authority"],
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
- Outreach trimis fără competitor gap analysis prealabil → violation
- Toxic links identificate dar nedisavowed → violation
- Link-uri construite pe site-uri cu spam score > 30% → violation critică
- Campanie link building fără monitorizare backlink toxicity → violation
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
- [ ] Backlink profile audit complet cu toxic links flagged?
- [ ] Outreach queue populat cu minim 50 prospects?
- [ ] Acquired links tracked cu toate atributele?

**VK-uri obligatorii:**
1. `✅ [PROC] M-013 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Off-Page SEO and Link Building Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Ahrefs / Semrush | Backlink profile and competitor gap analysis |
| Hunter.io / BuzzStream / Pitchbox | Outreach contact finding and campaign management |
| Google Search Console | Disavow file submission |
| Google Alerts / Ahrefs Content Explorer | Unlinked brand mention detection |
| CRM spreadsheet / Pitchbox | Link acquisition tracking |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Outreach response rate | Minimum 10% |
| Link acquisition rate | Minimum 5% of outreach |
| Average DR of acquired links | DR 40+ |
| Net referring domain growth (monthly) | Positive trend |
