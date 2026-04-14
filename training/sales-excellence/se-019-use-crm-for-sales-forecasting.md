---
type: procedure
created: 2026-03-17
status: active
slug: se-019-use-crm-for-sales-forecasting
tags: [procedure, nexus]
---

# SE-019 — Use CRM for Sales Forecasting

## Problema
Forecasts are guesswork, leading to missed targets and poor resource allocation.

## Procedura
### Prerequisites
- CRM with pipeline data
- Historical win rates
- Quota targets

### Steps
1. Pull historical data: win rates by stage, average deal size, average sales cycle
2. Configure CRM forecast categories: Commit, Best Case, Pipeline, Omit
3. Assign each active deal to a forecast category based on stage and signals
4. Calculate weighted forecast: sum of (deal value x stage probability) per category
5. Compare forecast to quota gap and identify deals needed to close the gap
6. Set up a weekly forecast review meeting with standardized CRM report
7. Track forecast accuracy monthly: predicted vs. actual closed revenue

### Verification
- Forecast categories configured
- Current quarter forecast calculated
- Forecast accuracy tracking started

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, forecasting, sales-analytics

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing sales-excellence skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- CRM with pipeline data
- Historical win rates
- Quota targets

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
