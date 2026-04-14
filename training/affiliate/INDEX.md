---
type: procedure
created: 2026-03-17
status: active
slug: index
tags: [procedure, nexus]
---

# Affiliate Marketing — Procedure Index

**Domain**: Affiliate Marketing
**Total proceduri**: 11
**FORGE version**: v1.3
**Creat**: 2026-03-03
**Regulă asociată**: META-H-002
**Colecție Cortex**: training

---

## Proceduri

| # | ID | Titlu | Complexitate | File | Status | GUIDE | Dependențe |
|---|-----|-------|-------------|------|--------|-------|------------|
| 1 | M-001 | Affiliate Niche Selection and Validation | BASIC | `M-001-affiliate-niche-selection.md` | FORGED | - | none |
| 2 | M-002 | Affiliate Program Evaluation and Selection | INTERMEDIATE | `M-002-affiliate-program-evaluation.md` | FORGED | - | M-001 |
| 3 | M-003 | JVZoo Account Setup and Product Research | BASIC | `M-003-jvzoo-setup.md` | FORGED | [GUIDE] | M-001 |
| 4 | M-004 | Affiliate Landing Page Construction | INTERMEDIATE | `M-004-affiliate-landing-page.md` | FORGED | - | M-002 |
| 5 | M-005 | Email Marketing Funnel for Affiliate Promotions | INTERMEDIATE | `M-005-email-funnel-affiliate.md` | FORGED | - | M-004 |
| 6 | M-006 | CPA Network Application and Acceptance | BASIC | `M-006-cpa-network-application.md` | FORGED | [GUIDE] | M-001 |
| 7 | M-007 | CPA Offer Selection and Competitor Analysis | INTERMEDIATE | `M-007-cpa-offer-selection.md` | FORGED | - | M-006 |
| 8 | M-008 | CPA Campaign Tracking Setup with Voluum | ADVANCED | `M-008-cpa-tracking-voluum.md` | FORGED | [GUIDE] | M-007 |
| 9 | M-009 | Facebook Ads for CPA Offers | ADVANCED | `M-009-facebook-ads-cpa.md` | FORGED | - | M-007, M-008 |
| 10 | M-112 | Affiliate Commission Negotiation and Backend Optimization | ADVANCED | `M-112-commission-negotiation.md` | FORGED | - | M-007, M-008 |
| 11 | M-113 | Affiliate Content Marketing (Reviews, How-To, Comparisons) | INTERMEDIATE | `M-113-affiliate-content-marketing.md` | FORGED | - | M-002, M-004 |

---

## Dependency Graph

```
M-001 (Niche Selection)
├── M-002 (Program Evaluation)
│   ├── M-004 (Landing Page)
│   │   ├── M-005 (Email Funnel)
│   │   └── M-113 (Content Marketing) ← also depends on M-002
│   └── M-113 (Content Marketing)
├── M-003 (JVZoo Setup) [GUIDE]
└── M-006 (CPA Network Application) [GUIDE]
    └── M-007 (CPA Offer Selection)
        ├── M-008 (Tracking Voluum) [GUIDE]
        │   ├── M-009 (Facebook Ads) ← also depends on M-007
        │   └── M-112 (Commission Negotiation) ← also depends on M-007
        ├── M-009 (Facebook Ads)
        └── M-112 (Commission Negotiation)
```

---

## Distribuție Complexitate

| Complexitate | Count | Proceduri |
|-------------|-------|-----------|
| BASIC | 3 | M-001, M-003, M-006 |
| INTERMEDIATE | 5 | M-002, M-004, M-005, M-007, M-113 |
| ADVANCED | 3 | M-008, M-009, M-112 |

---

## Proceduri [GUIDE] (Genie planifică, execuție umană)

- M-003: JVZoo Account Setup and Product Research
- M-006: CPA Network Application and Acceptance
- M-008: CPA Campaign Tracking Setup with Voluum

---

## Business Applications

| Business | Proceduri relevante |
|----------|-------------------|
| SMSads | M-001, M-002, M-003, M-004, M-005, M-006, M-007, M-008, M-009, M-112, M-113 |
| AI B2B | M-001, M-003, M-113 |
| Saraimanic | M-005, M-009 |
| Albastru | M-005 |

---

## Cortex Save Template

```json
{
  "text": "Affiliate Marketing procedure index: 11 procedures (3 BASIC, 5 INTERMEDIATE, 3 ADVANCED), 3 GUIDE procedures. Domain: affiliate_marketing. All FORGE v1.3 compliant.",
  "collection": "training",
  "metadata": {
    "type": "procedure_index",
    "domain": "affiliate_marketing",
    "total_procedures": 11,
    "forge_version": "1.3",
    "rule_id": "META-H-002"
  }
}
```
