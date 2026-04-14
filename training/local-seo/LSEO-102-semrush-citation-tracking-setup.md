---
id: LSEO-102
title: "Semrush Citation Tracking Setup"
domain: local-seo
source: "Semrush Academy — Local SEO Essentials with Semrush (Rita Cidre, Wes McDowell), Lessons 12-13"
version: "2.0"
forge_score: 3.75
business_mapping: "Local business citation management — any business needing NAP consistency across directories"
status: ACTIVE
created: 2026-03-20
rule: META-H-002
---

# LSEO-102 — Semrush Citation Tracking Setup

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Set up and maintain citation tracking using Semrush Listing Management — ensuring NAP consistency across all directories, suppressing duplicates, and building new citations for local SEO authority.
**Source**: Semrush Academy — Local SEO Essentials with Semrush (Rita Cidre, Wes McDowell), Lessons 12-13

---

## 1. Obiectiv

Ensure consistent business citations (NAP: Name, Address, Phone) across all online directories to maintain local search trust signals. Google cross-references citation data to verify business legitimacy. Duplicate listings, outdated addresses, or phone number changes create conflicting signals that suppress local rankings.

Situații acoperite:
- Initial citation audit and cleanup for new local SEO clients
- Business address/phone change requiring bulk citation update
- Duplicate listing suppression across directories
- New citation building to expand local presence
- Ongoing monthly citation monitoring

> **Note**: Requires Semrush Listing Management tool. Business must have a verified Google Business Profile.

---

## 2. Pași

### Pas 1: Enter Business NAP in Listing Management
Open Semrush > Local SEO > Listing Management. Enter the exact business name, full address, and primary phone number. This becomes the "source of truth" NAP. Important: use the EXACT same format everywhere — "Street" vs "St.", "Suite" vs "#", etc. Verify this matches GBP exactly.

### Pas 2: Run Initial Directory Scan
Click "Start Scanning" to check NAP presence across 70+ directories. Wait for scan completion (2-5 minutes). Review the dashboard: green = correct, yellow = partial match, red = incorrect, grey = not listed. Screenshot or export baseline results. Note total directories found, accuracy percentage, and duplicates detected.

### Pas 3: Fix Critical Directory Errors First
Sort results by directory authority (priority order): Google, Apple Maps, Bing Places, Facebook, Yelp, Yellow Pages, Foursquare, TripAdvisor (if applicable), BBB, Chamber of Commerce. For each red/yellow entry: click to see the specific error (wrong phone, old address, misspelled name, wrong category). Use Semrush's "Fix it" feature for supported directories (auto-push corrected NAP). For unsupported directories: manually visit the directory, claim the listing, update NAP.

### Pas 4: Suppress Duplicate Listings
In the scan results, check the "Duplicates" tab. For each duplicate: verify which listing is the canonical one (most reviews, longest history). Use Semrush's duplicate suppression feature where available. For manual suppression: contact directory support, flag as duplicate, request removal. Document each duplicate and its resolution status. Timeline: some suppressions take 2-6 weeks.

### Pas 5: Build New Citations in Missing Directories
Review grey (not listed) directories. Prioritize by: (a) industry-specific directories (e.g., Healthgrades for medical, Avvo for legal), (b) high-DA general directories, (c) local/regional directories. Create listings with exact source-of-truth NAP from Step 1. Add consistent business description, categories, photos, and hours to each new listing. Target: 10-15 new citations per month until core directories are covered.

### Pas 6: Set Up Ongoing Monitoring Alerts
In Listing Management > Settings, enable change detection alerts. Configure email notifications for: NAP changes detected, new duplicates found, listing status changes. Set scan frequency to weekly. Review alerts within 48h of receipt — unauthorized changes to your listings (by competitors or directory edits) must be corrected immediately.

### Pas 7: Configure Monthly Citation Health Report
Export Listing Management data monthly. Track: total directories listed, accuracy percentage (target >95%), duplicates active (target 0), new citations added, citations corrected. Compare month-over-month. Correlate with Map Rank Tracker position changes from LSEO-101. Create a simple dashboard: citation count trend, accuracy trend, duplicate trend.

### Pas 8: Structured Citation Building via Semrush Link Building Tool
Open Semrush > Link Building Tool. Set target domain. Filter prospects by: Type = Directory/Citation, Country = target location. Review suggested directories not yet in Listing Management scope. Outreach to niche/industry directories for high-value citations. Track submissions: date, directory, status (pending/approved/rejected).

### Pas 9: Quarterly Deep Audit
Every 90 days: re-run full scan (Step 2), verify all fixes held, check for new duplicates, assess citation velocity vs. competitors (use Semrush competitor citation report). Update strategy based on findings. Adjust priority directories if business expanded to new locations.

---

## 3. Verificare

- [ ] All 9 steps from section 2 executed?
- [ ] NAP source of truth matches GBP exactly?
- [ ] Initial directory scan completed and baseline documented?
- [ ] Critical directory errors fixed?
- [ ] Duplicates identified and suppression initiated?
- [ ] New citation targets identified and submissions started?
- [ ] Monitoring alerts configured?
- [ ] Monthly report template set up?
- [ ] Quarterly deep audit scheduled?

---

## 4. Instrumente

| Instrument | Rol |
|------------|-----|
| Semrush Listing Management | Directory scanning, NAP distribution, duplicate detection |
| Semrush Link Building Tool | Structured citation prospecting |
| Google Business Profile | Source of truth NAP verification |
| Audit spreadsheet | Tracking fixes, submissions, monthly metrics |

---

## 5. Note

- NAP consistency is a Google trust signal. Even minor format differences ("Street" vs "St.") can create conflicting signals.
- Duplicate suppression can take 2-6 weeks depending on the directory. Document and track each one.
- Priority directories (Google, Apple Maps, Bing, Facebook, Yelp) carry the highest citation weight — fix these first.
- Citation velocity matters: build 10-15 new citations per month until core directories are covered, then maintain.
- Always correlate citation changes with Map Rank Tracker position changes (LSEO-101) to measure impact.

---

## 6. Cortex Logging

```json
{
  "text": "Executed LSEO-102: Semrush Citation Tracking Setup — NAP source of truth set, directory scan complete, errors fixed, duplicates suppressed, new citations built, monitoring alerts active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LSEO-102",
    "domain": "local-seo",
    "rule_id": "META-H-002",
    "tags": ["local-seo", "semrush", "citations", "nap", "listing-management", "directories", "duplicates"],
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
- Procedure executed without all steps from section 2 -> violation
- Output without VK -> violation
- NAP source of truth not established before scanning -> violation
- Critical directories (Google, Bing, Apple Maps) not prioritized -> violation
- Duplicates detected but not addressed within 30 days -> violation
- Monthly monitoring not configured -> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 -> enforcement FORGE standard
- `procedure-health.json` -> add entry at each execution
- Training Mode -> curriculum local-seo
- LSEO-101 -> depends on (audit workflow references citation data)

### VERIFY
- [ ] All steps from section 2 executed?
- [ ] NAP source of truth matches GBP exactly?
- [ ] Initial directory scan completed and baseline documented?
- [ ] Critical directory errors fixed?
- [ ] Duplicates identified and suppression initiated?
- [ ] New citation targets identified and submissions started?
- [ ] Monitoring alerts configured?
- [ ] Monthly report template set up?
- [ ] VK issued in session?

**Mandatory VKs:**
1. `VK [PROC] LSEO-102 | S1-S9 DONE | citation tracking active`
2. `VK [CORTEX] "Semrush Citation Tracking Setup" | FORGE 2.0 | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activity | Model |
|----------|-------|
| Execution | Sonnet |
| Audit | Opus |

---

## 8. Dependente

| Component | Role |
|-----------|------|
| Semrush Listing Management | Directory scanning, NAP distribution, duplicate detection |
| Semrush Link Building Tool | Structured citation prospecting |
| Google Business Profile | Source of truth NAP verification |
| Audit spreadsheet | Tracking fixes, submissions, monthly metrics |

---

## 9. Metrics

| Metric | Target |
|--------|--------|
| Completion rate | 100% steps |
| VK emission | 2/2 |
| NAP accuracy across directories | > 95% |
| Active duplicates | 0 |
| Core directories covered | 100% (top 10) |
| Citation fix turnaround | < 7 days for critical |
| Monthly monitoring cadence | Weekly scans, monthly reports |
