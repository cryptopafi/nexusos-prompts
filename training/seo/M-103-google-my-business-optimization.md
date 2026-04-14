---
type: procedure
created: 2026-03-22
status: active
slug: m-103-google-my-business-optimization
tags: [procedure, nexus]
---

# Google My Business Optimization for Local Marketing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Complete Google Business Profile (formerly Google My Business) optimization for local businesses, covering profile setup, keyword-optimized description, photo strategy, posts, messaging, review management, Q&A monitoring, and GMB Insights analysis to maximize local pack visibility.

---

## 1. Problema

Google Business Profile is the single most important local SEO asset, directly controlling appearance in the Google Local Pack (the map results shown above organic results). Incomplete profiles, unclaimed listings, poor photo coverage, and unmanaged reviews directly suppress local pack rankings and lose calls, directions, and website visits to better-optimized competitors.

Types of situations covered:
- New business — claiming and fully setting up GMB for the first time
- Unclaimed listing — competitor or auto-generated listing needs to be claimed
- Incomplete profile — existing listing with missing sections reducing rank
- Reputation management — review crisis or review gap requiring active management
- Multi-location business — per-location GMB optimization at scale

---

## 2. Procedura

### Pas 1: Claim and Verify the GMB Listing
Search Google Maps for the business. If a listing exists but is unclaimed, click "Claim this business." If no listing exists, create at business.google.com. Verification options: postcard (5-7 days), phone, email, or instant verification (if GSC already verified). Use the fastest verification method available. Do not make major profile edits before verification is complete — edits may trigger re-verification.

### Pas 2: Complete All Profile Sections
Fill in every available field:
- **Business name**: Exact legal business name. No keyword stuffing — Google may suspend for this.
- **Primary category**: The most specific category that describes the core business. This is the most impactful ranking factor in the local pack.
- **Secondary categories**: Up to 9 additional relevant categories.
- **Address**: Must exactly match the physical location and NAP used across all citations.
- **Phone number**: Local number preferred over 0800/toll-free. Confirm it matches website and citations.
- **Website URL**: Link to the most relevant page (homepage for single-location, location page for multi-location).
- **Hours**: Regular hours + special hours for holidays. Accuracy here affects customer trust signals.
- **Services and products**: List all services/products with descriptions and prices where applicable.
- **Attributes**: Select all applicable (accessibility, payment methods, amenities, dining options, etc.).

### Pas 3: Write Keyword-Optimized Business Description
Write a 750-character (max) business description. Structure:
- First sentence: What the business does + primary location + primary service keyword
- Body: Unique value propositions, years in business, specialties, and differentiators
- Close: Service area and call to action
Include the primary "service + city" keyword naturally in the first 250 characters (visible before "more" truncation). Do not include URLs, promotional prices, or misleading claims — Google may reject the description.

### Pas 4: Upload Photos — Minimum 10 Per Category
Upload the following photo types:
- **Logo**: Square format, high resolution
- **Cover photo**: 16:9 aspect ratio, best-impression storefront or team shot
- **Exterior photos** (3+): From different angles and street view perspectives, to help customers recognize the location
- **Interior photos** (3+): Showing the workspace, atmosphere, or service environment
- **Team photos** (2+): Staff and owner photos improve trust
- **Product/service photos** (5+): Key offerings visually represented
Geotagging photos with the business location can provide additional local relevance signals. Maintain a regular photo update cadence (minimum monthly) — fresh photos signal active management.

### Pas 5: Set Up and Publish Regular Posts
Use Google Posts to publish at minimum 2 posts per month. Post types:
- **What's New**: General updates, news, announcements
- **Offer**: Promotions with start/end date, coupon code, and redemption link
- **Event**: Upcoming events with date, time, and description
Each post: 100-300 words, include a high-quality image, include a Call-to-Action button (Book, Order, Learn more, Get offer). Posts expire after 7 days (What's New) — maintain a content calendar for consistency.

### Pas 6: Enable Messaging and Respond to All Reviews
Enable the Messages feature in GMB dashboard. Set up a welcome message. Aim to respond to all messages within 1 hour during business hours. For reviews: respond to every review — positive and negative — within 48 hours. For positive reviews: thank, mention the service/product, and reinforce the value delivered. For negative reviews: acknowledge the issue, apologize without admitting liability, offer to resolve offline ("please contact us at [phone/email]"). Never argue publicly. Review response velocity and positivity are indirect local ranking factors.

### Pas 7: Monitor Q&A and GMB Insights
Check the Q&A section weekly. Answer all customer questions promptly. Proactively add 5-10 FAQs using the "Ask a question" function — answer your own questions to populate useful information before customers need to ask. Monitor GMB Insights monthly: track searches (direct vs. discovery vs. branded), customer actions (calls, direction requests, website clicks), and photo views vs. competitor benchmarks. Report month-over-month changes in actions as the primary GMB performance metric.

---

## 3. Cortex Logging

```json
{
  "text": "Google My Business Optimization completed for [business name]. Profile completeness: 100%. Photos: [n] uploaded. Posts: active. Reviews: [n total], responded [n]. Messaging: enabled. Insights tracking: active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-103",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["google-my-business", "gmb", "local-seo", "local-pack", "reviews", "gbp"],
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
- Profile completat fără primary category setat → violation
- Recenzii negative fără răspuns → violation
- Profil GMB actualizat fără verificarea că este verificat/revendicat → violation
- Optimizare GMB fără monitorizare recenzii și Q&A activă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Se integrează cu M-015 (Local SEO) ca sub-componentă principală

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Toate câmpurile GMB completate (100% completeness)?
- [ ] Minim 10 poze uploadate per categorie?
- [ ] Toate recenziile au răspuns?

**VK-uri obligatorii:**
1. `✅ [PROC] M-103 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google My Business Optimization for Local Marketing" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Business Profile (business.google.com) | Primary platform for all optimization |
| Google Maps | Listing visibility verification |
| GMB Insights | Performance metrics and customer action tracking |
| Review request tool (email/SMS) | Review acquisition workflow |
| M-015 Local SEO | Broader local optimization context (NAP, citations, schema) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Profile completeness | 100% all available fields |
| Photo count | Minimum 10 published |
| Review response rate | 100% within 48 hours |
| Monthly posts published | Minimum 2 |
