---
type: procedure
created: 2026-03-20
status: active
slug: se-009-map-and-plan-sales-territory-coverage
tags: [procedure, nexus]
---

# SE-009 — Map and Plan Sales Territory Coverage

## Problema
Reps waste 40% of selling time on low-value accounts while high-potential prospects get neglected. Without a data-driven coverage model, territory potential goes unrealized and competitors fill the gaps.

## Procedura
### Prerequisites
- Territory SWOT completed (SE-008)
- Account list exported from CRM with current revenue, potential revenue, and industry
- Geographic mapping tool (Google Maps, Maptive, or CRM mapping feature)
- Previous year's account performance data

### Steps
1. **Export and enrich account data** — Pull all accounts from CRM with: account name, current annual revenue, estimated total addressable spend (what they COULD buy from you), industry, location, last contact date, and assigned tier. If total addressable spend is unknown, estimate using industry benchmarks: companies of size X in industry Y typically spend $Z on your category.
2. **Calculate Share of Wallet (SoW) for each account** — SoW = (your revenue / their total addressable spend) x 100. Segment: <20% SoW = growth opportunity, 20-60% SoW = defend and expand, >60% SoW = maximize and retain. Accounts with high total spend but low SoW are your biggest growth targets.
3. **Apply ABC segmentation with the Pareto rule** — A accounts (top 20% of revenue, typically 80% of total) = strategic, named. B accounts (next 30% revenue) = growth potential. C accounts (bottom 50% revenue) = maintain or automate. D accounts = prospects not yet customers. For each tier, set engagement standards: A = weekly touchpoint, B = bi-weekly, C = monthly automated, D = prospecting cadence.
4. **Map accounts geographically** — Plot A and B accounts on a map. Identify clusters (3+ accounts within 30-minute drive). These clusters become your "power zones" — areas where you can stack meetings and maximize face-to-face time. Identify white spaces: areas with no accounts but matching ICP demographics.
5. **Build the coverage model** — Calculate available selling days: 250 working days - 20 vacation - 15 internal meetings - 10 training = 205 selling days. A accounts need 48 touches/year (weekly), B need 24 (bi-weekly), C need 12 (monthly). Total touches needed = (A count x 48) + (B count x 24) + (C count x 12). If total exceeds capacity, either reduce C coverage, automate C with email sequences, or request territory adjustment.
6. **Create weekly routing plans** — Design 4-5 recurring weekly routes based on account clusters. Monday: North cluster (A accounts). Tuesday: East cluster. Wednesday: office day (calls, admin, virtual meetings). Thursday: South cluster. Friday: prospecting and white space exploration. Each route should include 1 A account and 2-3 B/C accounts. Block travel time between meetings (30-45 min buffer).
7. **Set quarterly coverage goals** — Define targets: (a) A-account retention rate >95%, (b) B-account upgrade rate: move 20% of B accounts to A within 12 months, (c) New account acquisition: open X new accounts per quarter from white space, (d) SoW growth: increase average SoW by 5% per quarter. Tie goals to territory revenue target.
8. **Implement a "hunting day" protocol** — Reserve 1 day per week (or 2 half-days) exclusively for new business development in white space areas. No existing account visits on hunting days. Focus on: D-tier prospects identified in territory SWOT, referrals from A accounts, inbound leads in white space areas. Track hunting day ROI separately.
9. **Build a territory dashboard** — Create a visual dashboard with: (a) Territory map showing account tiers (color-coded), (b) Coverage compliance: actual vs. planned touches per tier, (c) Revenue by tier vs. target, (d) SoW trend by top accounts, (e) White space pipeline. Review weekly in 15-minute self-check. Present monthly to manager.
10. **Optimize quarterly based on data** — Every 90 days: (a) Reassess account tiers (promote/demote based on revenue trajectory), (b) Evaluate route efficiency (which routes generate highest revenue per visit), (c) Adjust white space targeting based on prospecting results, (d) Reallocate time from low-ROI segments. Benchmark: structured territory plans yield 15-25% higher revenue per rep vs. unstructured.

### Verification
- All accounts tiered (A/B/C/D) with SoW calculated
- Geographic map created with clusters and white spaces identified
- Coverage model calculated (touches needed vs. capacity)
- Weekly routing plan for 4-5 recurring routes
- Quarterly goals set with specific metrics

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, territory-management, account-planning, coverage-model
- version: 2.0

## Enforcement Loop
- WHERE: Training and skill development — territory planning
- WHEN: Quarterly planning cycle; weekly route execution
- HOW: Segment → map → model capacity → route → execute → measure → optimize
- CONNECT: SE-008 (Territory SWOT), SE-033 (AI Territory Mapping), SE-015 (Activity Goals)

## Dependente
- Territory SWOT completed (SE-008)
- CRM with revenue and account data
- Geographic mapping tool

## Metrics
- Completion: all 10 steps executed, routing plan active
- Quality: coverage compliance >85% (planned vs. actual touches)
- Benchmark: 15-25% revenue uplift from structured territory planning
- Level: ADVANCED
