---
type: procedure
created: 2026-03-17
status: active
slug: dpm-059-instrument-product-for-quantitative-ux-analytics
tags: [procedure, nexus]
---

# DPM-059 — Instrument Product for Quantitative UX Analytics

## Problema
UX decisions are based on qualitative opinions because the product lacks quantitative behavior tracking.

## Procedura
### Prerequisites
- Product codebase access
- Analytics tool (Mixpanel, Amplitude, Heap)
- UX metrics knowledge

### Steps
1. Define the key UX metrics to track: task completion rate, time on task, error rate, navigation paths
2. Map the critical user flows and identify instrumentation points
3. Implement event tracking for: page views, clicks, form submissions, errors, feature usage
4. Add user properties: segment, plan type, tenure, device for filtering
5. Create UX dashboards: funnel drop-offs, feature adoption, error frequency
6. Set up alerts for UX anomalies: sudden drop in completion rate, spike in errors
7. Run weekly UX analytics review to identify issues and opportunities

### Verification
- Critical flows instrumented
- UX dashboard created
- Weekly review cadence established

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, ux-analytics, instrumentation

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Product codebase access
- Analytics tool (Mixpanel, Amplitude, Heap)
- UX metrics knowledge

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
