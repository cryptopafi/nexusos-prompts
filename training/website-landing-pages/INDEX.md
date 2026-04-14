---
type: procedure
created: 2026-03-17
status: active
slug: index
tags: [procedure, nexus]
---

# INDEX — Website & Landing Pages Procedures

**Directory**: `~/.nexus/procedures/training/website-landing-pages/`
**Domain**: Website & Landing Pages
**Total procedures**: 4
**Last updated**: 2026-03-04
**FORGE version**: 1.3

---

## Procedures

| ID | Filename | Title | Complexity | Status |
|----|----------|-------|------------|--------|
| M-064 | M-064-wordpress-website-setup.md | WordPress Website Setup | BASIC | FORGED |
| M-065 | M-065-landing-page-design-optimization.md | Landing Page Design and Optimization | INTERMEDIATE | FORGED |
| M-066 | M-066-website-cro.md | Website Conversion Rate Optimization (CRO) | ADVANCED | FORGED |
| M-104 | M-104-unbounce-landing-page.md | Unbounce Landing Page Creation System | INTERMEDIATE | FORGED |

---

## Quick Reference

### M-064 — WordPress Website Setup (BASIC)
Setup complet WordPress: hosting → instalare → temă → plugin-uri → SSL → pagini esențiale → GA.
**Tools**: SiteGround / WP Engine / Cloudways, Kadence / Astra, Yoast SEO, Wordfence, GA4

### M-065 — Landing Page Design and Optimization (INTERMEDIATE)
Design LP cu 1 obiectiv, message match, above-fold complet, trust section, CTA x3, navigare eliminată, heatmap.
**Tools**: Hotjar / MS Clarity, Google Optimize, Unbounce / WordPress

### M-066 — Website CRO (ADVANCED)
Proces CRO complet: baseline → heatmap → friction points → ipoteze ICE → A/B test → implementare câștigători.
**Tools**: GA4, Hotjar / MS Clarity, Google Optimize / Optimizely

### M-104 — Unbounce Landing Page Creation System (INTERMEDIATE)
Creare LP în Unbounce: template → design → copy → formular → A/B → domeniu custom → CRM → monitoring.
**Tools**: Unbounce, CRM (HubSpot / ActiveCampaign), Zapier

---

## Dependency Map

```
M-064 (WordPress Setup)
    └── M-065 (Landing Page Design)
            └── M-066 (Website CRO)
                    └── M-086 (Heatmap Analysis) [analytics-tracking/]

M-104 (Unbounce LP)
    └── M-066 (Website CRO) — pentru optimizare post-lansare
```

---

## Domain Tags
`wordpress` `landing-page` `cro` `conversion` `heatmap` `ab-testing` `unbounce` `website` `setup`

---

## Rule Association
Toate procedurile: **META-H-002** — FORGE Standard enforcement
