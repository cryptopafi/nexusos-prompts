---
id: LSEO-101
title: "Semrush Local SEO Audit Workflow"
domain: local-seo
source: "Semrush Academy — Local SEO Essentials with Semrush (Rita Cidre, Wes McDowell)"
version: "2.0"
forge_score: 3.75
business_mapping: "Local business SEO audit — any business with physical location needing local search visibility"
status: ACTIVE
created: 2026-03-20
rule: META-H-002
---

# LSEO-101 — Semrush Local SEO Audit Workflow

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Complete local SEO audit using Semrush Local Toolkit — from GBP connection to ranking analysis and optimization actions.
**Source**: Semrush Academy — Local SEO Essentials with Semrush (Rita Cidre, Wes McDowell)

---

## 1. Obiectiv

Detect and fix local SEO issues that cause businesses to lose customers: incomplete Google Business Profile, inconsistent citations, poor review management, and lack of visibility into local ranking performance. Without a structured audit workflow, these issues go undetected and local search rankings decay.

Situații acoperite:
- New client local SEO onboarding audit
- Quarterly local SEO health check for existing clients
- Post-Google algorithm update local ranking recovery
- Multi-location businesses with inconsistent NAP data

> **Note**: Requires Semrush Local Toolkit subscription (Listing Management, Map Rank Tracker, Review Management). GBP access with Owner/Manager permissions required.

---

## 2. Pași

### Pas 1: Connect Google Business Profile to Semrush Local Toolkit
Open Semrush > Local SEO > Listing Management. Click "Connect Google Business Profile" and authorize OAuth access. Verify the correct business entity is linked (check name, address, phone). If multi-location, repeat for each location. Confirm sync status shows "Connected" with green indicator.

### Pas 2: Audit GBP Profile Completeness
In Semrush Local Toolkit > Review your GBP dashboard. Check each element against the 9 critical ranking factors (per Semrush course): (1) Primary category accuracy, (2) Business name (no keyword stuffing), (3) Address completeness, (4) Phone number (local, not toll-free), (5) Website URL, (6) Business hours (including special hours), (7) Business description (750 chars, keywords natural), (8) Photos (min 10, exterior/interior/team/products), (9) Logo and cover photo. Score each 0-2: 0=missing, 1=partial, 2=optimized. Document in audit spreadsheet.

### Pas 3: Run Local Keyword Research in Semrush
Open Semrush > Keyword Magic Tool. Enter core service + city (e.g., "plumber Austin"). Filter by: Intent = Local/Commercial, Volume > 50, KD < 60. Export top 20-30 keywords. Cross-reference with GBP categories — ensure primary category matches highest-volume keyword cluster. Check "Questions" tab for FAQ content opportunities.

### Pas 4: Analyze Citation Consistency via Listing Management
In Listing Management > check "Directories" tab. Review NAP (Name, Address, Phone) across all listed directories. Flag any inconsistencies: wrong phone, old address, misspelled name, wrong category. Note directories where business is NOT listed (gaps). Priority fix: Google, Apple Maps, Bing, Facebook, Yelp, Yellow Pages, Foursquare — these carry highest citation weight.

### Pas 5: Audit Reviews with Review Management Tool
Open Review Management in Semrush. Analyze: total review count, average rating, review velocity (reviews/month), response rate, response time. Flag unanswered reviews (especially negative ones > 48h old). Check competitor review counts and ratings for benchmark. Use Semrush AI to draft review responses — edit for personal touch before posting.

### Pas 6: Set Up Map Rank Tracker Campaign
Open Map Rank Tracker > Create new campaign. Enter business address as center point. Select grid size (3x3 for single location, 5x5 for competitive markets). Add target keywords from Step 3 (top 10-15). Set scan frequency to weekly. Run first scan. Document baseline: for each keyword, note positions across the grid (green = top 3, yellow = 4-10, red = 11+).

### Pas 7: Analyze Local Competitors
In Map Rank Tracker results, identify top 3 competitors appearing in your grid positions. For each competitor: check their GBP completeness, review count/rating, citation count (via Listing Management competitor lookup). Note their primary/secondary categories. Document competitive gaps — where they beat you and where you can win.

### Pas 8: Compile Audit Report and Action Plan
Create audit report with scores from each step. Structure as: Executive Summary (overall local SEO health score 0-100), GBP Optimization Actions (from Step 2), Keyword Targets (from Step 3), Citation Fixes (from Step 4, prioritized by authority), Review Strategy (from Step 5, target velocity), Ranking Baseline (from Step 6, grid screenshots), Competitive Gaps (from Step 7). Assign priority: Critical (do this week), High (this month), Medium (next quarter).

### Pas 9: Schedule Monthly Re-Audit Cadence
Set calendar reminder for monthly re-run of Steps 4-6 (citations, reviews, rankings). Quarterly full audit (all steps). Track month-over-month: average grid position, review count/rating, citation accuracy %, GBP profile score.

---

## 3. Verificare

- [ ] All 9 steps from section 2 executed?
- [ ] GBP connected and synced in Semrush?
- [ ] Local keywords researched and documented?
- [ ] Citation consistency checked across directories?
- [ ] Reviews analyzed with response strategy?
- [ ] Map Rank Tracker baseline established?
- [ ] Competitor analysis completed?
- [ ] Audit report with prioritized action plan delivered?
- [ ] Monthly re-audit cadence scheduled?

---

## 4. Instrumente

| Instrument | Rol |
|------------|-----|
| Semrush Listing Management | Directory scanning, NAP consistency check, citation management |
| Semrush Map Rank Tracker | Local ranking grid monitoring |
| Semrush Review Management | Review analysis and response management |
| Semrush Keyword Magic Tool | Local keyword research |
| Google Business Profile | Owner/Manager access for profile optimization |
| Audit spreadsheet template | Score tracking and reporting |

---

## 5. Note

- The 9 critical GBP ranking factors must ALL be checked — partial audits miss critical issues.
- Citation consistency is a trust signal for Google. Even small discrepancies (St. vs Street) can cause ranking issues.
- Review velocity matters as much as review count. A steady stream of reviews signals an active business.
- Map Rank Tracker grid size should match market competitiveness: 3x3 for low competition, 5x5 for competitive markets.
- Multi-location businesses need separate audit cycles per location — do not combine.

---

## 6. Cortex Logging

```json
{
  "text": "Executed LSEO-101: Semrush Local SEO Audit Workflow — GBP connected, profile audited, keywords researched, citations checked, reviews analyzed, Map Rank Tracker baseline set, competitors analyzed, report compiled.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LSEO-101",
    "domain": "local-seo",
    "rule_id": "META-H-002",
    "tags": ["local-seo", "semrush", "gbp", "audit", "citations", "reviews", "map-rank-tracker"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 7. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
At each execution

### HOW (violation detection)
- Audit executed without all 9 steps from section 2 -> violation
- Output without VK -> violation
- GBP not connected before running audit checks -> violation
- Map Rank Tracker not configured with target keywords -> violation
- No competitor analysis included in report -> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 -> enforcement FORGE standard
- `procedure-health.json` -> add entry at each execution
- Training Mode -> curriculum local-seo
- LSEO-102 -> citation tracking setup (downstream)

### VERIFY
- [ ] All steps from section 2 executed?
- [ ] GBP connected and synced in Semrush?
- [ ] Local keywords researched and documented?
- [ ] Citation consistency checked across directories?
- [ ] Reviews analyzed with response strategy?
- [ ] Map Rank Tracker baseline established?
- [ ] Competitor analysis completed?
- [ ] Audit report with prioritized action plan delivered?
- [ ] VK issued in session?

**Mandatory VKs:**
1. `VK [PROC] LSEO-101 | S1-S9 DONE | audit complete`
2. `VK [CORTEX] "Semrush Local SEO Audit Workflow" | FORGE 2.0 | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activity | Model |
|----------|-------|
| Execution | Sonnet |
| Audit | Opus |

---

## 8. Dependente

| Component | Role |
|-----------|------|
| Semrush Local Toolkit | Listing Management, Map Rank Tracker, Review Management |
| Google Business Profile | Owner/Manager access required |
| Semrush Keyword Magic Tool | Local keyword research |
| Audit spreadsheet template | Score tracking and reporting |

---

## 9. Metrics

| Metric | Target |
|--------|--------|
| Completion rate | 100% steps |
| VK emission | 2/2 |
| GBP profile score | > 80/100 |
| Citation accuracy | > 95% NAP consistency |
| Review response rate | 100% within 48h |
| Map Rank Tracker grid coverage | > 60% green (top 3) |
