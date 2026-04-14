---
type: procedure
created: 2026-03-17
status: active
slug: index
tags: [procedure, nexus]
---

# INDEX — Analytics & Tracking Procedures

**Directory**: `~/.nexus/procedures/training/analytics-tracking/`
**Domain**: Analytics & Tracking
**Total procedures**: 8
**Last updated**: 2026-03-04
**FORGE version**: 1.3

---

## Procedures

| ID | Filename | Title | Complexity | Status |
|----|----------|-------|------------|--------|
| M-067 | M-067-google-analytics-setup.md | Google Analytics Setup and Configuration | BASIC | FORGED |
| M-068 | M-068-google-search-console-setup.md | Google Search Console Setup and Monitoring | BASIC | FORGED |
| M-069 | M-069-marketing-campaign-analytics.md | Marketing Campaign Analytics and ROI Measurement | INTERMEDIATE | FORGED |
| M-070 | M-070-facebook-insights-analytics.md | Facebook Insights and Social Media Analytics | BASIC | FORGED |
| M-071 | M-071-ga-ecommerce-tracking.md | Google Analytics E-commerce Tracking | INTERMEDIATE | FORGED |
| M-072 | M-072-ga-goals-events.md | Google Analytics Goal and Event Configuration | BASIC | FORGED |
| M-086 | M-086-website-heatmap-analysis.md | Website Heatmap Analysis for Conversion Optimization | INTERMEDIATE | FORGED |
| M-094 | M-094-semrush-competitor-intelligence.md | Semrush Competitor Intelligence System | ADVANCED | FORGED |

---

## Quick Reference

### M-067 — Google Analytics Setup and Configuration (BASIC)
GA4 de la zero: proprietate → GTM install → Enhanced Measurement → events → conversii → linking Ads + GSC.
**Tools**: GA4, GTM, Google Ads, Search Console

### M-068 — Google Search Console Setup and Monitoring (BASIC)
GSC: adaugă proprietate → verificare → sitemap → Coverage → Performance → alerte email → Core Web Vitals.
**Tools**: GSC, Yoast SEO, DNS provider

### M-069 — Marketing Campaign Analytics and ROI Measurement (INTERMEDIATE)
KPIs → UTM setup → Looker Studio dashboard → ROI per canal → top performers → realocare buget → review lunar.
**Tools**: GA4, Looker Studio, UTM Builder, Google Ads / Meta Ads

### M-070 — Facebook Insights and Social Media Analytics (BASIC)
Insights: metrici cheie → performanță per tip conținut → ore optime → monitoring competitori → export → calendar optimizat.
**Tools**: Facebook Business Suite, Google Sheets

### M-071 — GA E-commerce Tracking (INTERMEDIATE)
E-commerce GA4: enable reporting → purchase event → funnel events → enhanced items → Funnel Exploration → monitorizare produse.
**Tools**: GA4, GTM, WooCommerce / Shopify, GTM4WP

### M-072 — GA Goals and Event Configuration (BASIC)
Goals GA4: definire conversii → events custom → marcare conversii → GTM triggers → DebugView test → rapoarte → monitoring săptămânal.
**Tools**: GA4, GTM, Tag Assistant Chrome Extension

### M-086 — Website Heatmap Analysis for Conversion Optimization (INTERMEDIATE)
Heatmap: instalare Clarity/Hotjar → pagini cheie → 500 sesiuni → click heatmap → scroll heatmap → recordings → UX issues → ipoteze.
**Tools**: Microsoft Clarity, Hotjar, GTM

### M-094 — Semrush Competitor Intelligence System (ADVANCED)
Competitor intel: domain overview → organic traffic → backlinks → content audit → PPC → Keyword Gap → plan de acțiune.
**Tools**: Semrush, Google Sheets

---

## Dependency Map

```
M-067 (GA4 Setup) ─── fundație pentru toate procedurile de mai jos
    ├── M-071 (E-commerce Tracking) — necesită GA4 activ
    ├── M-072 (Goals & Events) — necesită GA4 activ
    └── M-069 (Campaign Analytics) — necesită GA4 + UTM

M-068 (Search Console) ─── SEO baseline
    └── M-094 (Semrush Intelligence) — complement GSC cu date competitive

M-086 (Heatmap Analysis)
    └── M-066 (Website CRO) [website-landing-pages/] — upstream pentru A/B testing
```

---

## BASIC Procedures (Foundation)
Start here for new setups: M-067 → M-068 → M-072 → M-070

## INTERMEDIATE Procedures (Optimization)
After foundation: M-069 → M-071 → M-086

## ADVANCED Procedures (Competitive Edge)
For strategic growth: M-094

---

## Domain Tags
`google-analytics` `ga4` `gtm` `search-console` `ecommerce` `goals` `events` `heatmap` `facebook` `semrush` `roi` `utm` `conversii` `analytics`

---

## Rule Association
Toate procedurile: **META-H-002** — FORGE Standard enforcement
