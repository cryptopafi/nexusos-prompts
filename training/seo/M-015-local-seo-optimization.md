---
type: procedure
created: 2026-03-22
status: active
slug: m-015-local-seo-optimization
tags: [procedure, nexus]
---

# Local SEO Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Complete local SEO optimization process covering Google Business Profile, NAP consistency, local citations, review generation, local keyword targeting, local schema markup, and competitor local analysis for businesses targeting geographic markets.

---

## 1. Problema

Local businesses rank poorly in the Google Local Pack and localized organic results when their GBP profile is incomplete, NAP data is inconsistent across directories, local citations are absent, and review velocity is low. Without structured local SEO, businesses lose foot traffic and calls to better-optimized competitors.

Types of situations covered:
- New local business launch — building local presence from scratch
- Multi-location businesses — optimizing each location separately
- Local pack exclusion — not appearing in map pack for primary keywords
- Reputation management — review profile building alongside SEO
- Local competitor overtaking — closing visibility gap in specific geo area

---

## 2. Procedura

### Pas 1: Google Business Profile Optimization
Claim and verify the GBP listing if not already owned. Complete every available field: business name (exact legal name, no keyword stuffing), primary and secondary categories, full address, phone number, website URL, hours of operation (including special hours), business description (750 chars, keyword-optimized), services/products with descriptions and prices, and attributes (accessibility, payment methods, etc.). Upload minimum 10 high-quality photos: exterior, interior, team, and products/services.

### Pas 2: NAP Consistency Audit
Define the exact NAP (Name, Address, Phone number) format as the canonical version. Search for existing citations using Semrush Listing Management, BrightLocal, or Moz Local. Identify all inconsistencies: different phone numbers, abbreviated vs. full street names, old addresses, and duplicate listings. Correct inconsistencies by claiming and updating each citation. Suppress or merge duplicate listings.

### Pas 3: Local Citation Building
Submit the business to the top 50 local citation sources: Google Business Profile, Bing Places, Apple Maps, Yelp, Facebook, Yellow Pages, Foursquare, and niche-specific directories for the industry (e.g., TripAdvisor for hospitality, Healthgrades for medical). For each citation: use exact canonical NAP, add business description, categories, website, and photos where supported. Track all citations in a citation log spreadsheet.

### Pas 4: Review Generation Strategy
Set up a review acquisition process: identify the best moment in the customer journey to request a review (post-purchase, post-service delivery). Create a direct review link (Google review short URL from GBP dashboard). Send review request via email or SMS with a single clear call to action and direct link. Target minimum 5 new Google reviews per month. Respond to all reviews — positive and negative — within 48 hours.

### Pas 5: Local Keyword Targeting
Create location-specific landing pages if serving multiple areas. Optimize each page for "[service] + [city/neighborhood]" keywords. Include: city name in title tag, H1, meta description, and first paragraph. Add locally relevant content: references to local landmarks, community events, or service area details. Avoid duplicate location pages — each must have unique content.

### Pas 6: Local Schema Markup
Implement LocalBusiness schema (JSON-LD) on the homepage and each location page. Required fields: @type, name, address (PostalAddress), telephone, openingHoursSpecification, geo (latitude/longitude), url, and image. Add Review schema if displaying aggregate review data. Validate all schema with Google Rich Results Test — zero errors before publishing.

### Pas 7: Local Link Building and Competitor Analysis
Identify local link opportunities: chamber of commerce, local news sites, business associations, local sponsorships, and community organizations. Target a minimum of 5 locally relevant referring domains per quarter. Run competitor analysis: check top 3 local pack competitors' GBP profiles, review counts, citation sources, and local backlinks. Identify gaps and replicate their strongest signals.

---

## 3. Cortex Logging

```json
{
  "text": "Local SEO Optimization completed for [business/location]. GBP: complete. Citations: [n] built/corrected. Reviews: [current count]. Local schema: validated. Local links: [n] acquired.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-015",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["local-seo", "google-business-profile", "citations", "nap", "local-pack"],
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
- NAP inconsistencies nerezolvate după audit → violation
- LocalBusiness schema nevalidat → violation
- NAP (Name/Address/Phone) inconsistent între directoare → violation
- Optimizare locală fără verificarea Google Business Profile activ → violation
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
- [ ] GBP complet cu toate câmpurile completate?
- [ ] NAP consistent pe toate citation sources?
- [ ] LocalBusiness schema validat cu zero erori?

**VK-uri obligatorii:**
1. `✅ [PROC] M-015 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Local SEO Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Business Profile | Primary local visibility platform |
| BrightLocal / Semrush Listing Management | Citation audit and management |
| Google Rich Results Test | Local schema validation |
| Google Search Console | Local search performance tracking |
| Review request tool (email/SMS) | Review acquisition workflow |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| GBP profile completeness | 100% fields filled |
| NAP consistency | 100% citations matching canonical NAP |
| New reviews per month | Minimum 5 |
| Local citations built | Top 50 directories covered |
