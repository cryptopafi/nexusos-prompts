---
type: procedure
created: 2026-03-20
status: active
slug: dpm-006-manage-outside-stakeholders
tags: [procedure, nexus]
---

# DPM-006 — Manage Outside Stakeholders
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
External stakeholders (investors, partners, regulators, enterprise customers) receive inconsistent communication — creating surprise escalations, misaligned expectations, and eroded trust that threatens product partnerships and funding.

## Procedura
### Prerequisites
- Complete list of external stakeholders with contact information
- Product roadmap and current milestone status
- CRM or stakeholder tracker (Notion, Salesforce, HubSpot)
- Standard update template

### Steps
1. **Catalog all external stakeholders** — List every external party with influence on or interest in the product: investors/board members, strategic partners, regulatory bodies, enterprise customers (top 10 by revenue), channel partners, press/analysts, and advisory board. For each, document: name, organization, relationship owner, communication preference, and last contact date.
2. **Classify using the Power/Interest matrix** — Plot each stakeholder on a 2x2: (a) High Power/High Interest → Manage Closely (co-create, monthly 1:1s), (b) High Power/Low Interest → Keep Satisfied (quarterly executive summaries), (c) Low Power/High Interest → Keep Informed (monthly newsletter/changelog), (d) Low Power/Low Interest → Monitor (annual touchpoint). This determines communication investment per stakeholder.
3. **Create a communication plan per quadrant** — For each quadrant, specify: cadence, format, depth, and owner. Example for "Manage Closely": monthly 30-min video call + written follow-up within 24h, roadmap preview access, early beta invitations, direct Slack channel. Document in a Communication Matrix (Notion table or Google Sheet).
4. **Build a standard update template** — Structure: (a) Headlines — 3 bullet max of key progress, (b) Metrics — 2-3 KPIs with trend arrows (MRR, DAU/MAU, NPS), (c) Roadmap status — Now/Next/Later with % complete on current items, (d) Risks & blockers — be transparent, (e) Asks — specific requests with deadlines, (f) Next milestone with target date. Keep total update under 500 words. Use Loom for video alternatives.
5. **Implement proactive risk communication** — Never let stakeholders learn bad news from someone else. For any risk that could affect timeline, budget, or scope: communicate within 48 hours. Use the format: "[Situation] happened. [Impact] on [timeline/scope]. [Mitigation] we are taking. [Ask] what we need from you." Bad news delivered early builds trust; surprises destroy it.
6. **Manage expectations with commitment tracking** — Maintain a Commitment Log: every promise made to external stakeholders (feature timeline, integration delivery, pricing guarantee). Track: commitment, made to whom, deadline, status, and owner. Review weekly. Flag any commitment at risk 2 weeks before deadline. Never make commitments without checking with engineering on feasibility first.
7. **Handle competing stakeholder demands** — When stakeholders request conflicting priorities: (a) Acknowledge each request, (b) Score using RICE with stakeholder revenue/strategic weight, (c) Present the trade-off transparently: "Doing A means B moves to Q3," (d) Let data guide, but preserve strategic optionality. Document the decision and share reasoning with all affected parties.
8. **Run quarterly stakeholder reviews** — Structured 60-min sessions for "Manage Closely" stakeholders: product demo of recent launches, metric review, roadmap preview for next quarter, open Q&A. Send a written summary within 24 hours. For investors specifically, include: burn rate, runway, and hiring plan.
9. **Measure stakeholder satisfaction** — Annual NPS survey for top stakeholders (5 questions max). Track relationship health score per stakeholder: Green (engaged, positive), Yellow (responsive but concerns), Red (disengaged or escalating). Target: >80% Green. Escalate any Red to VP/CEO within 1 week.

### Verification
- [ ] All external stakeholders cataloged with Power/Interest classification
- [ ] Communication Matrix created with cadence per quadrant
- [ ] Standard update template built and first round sent
- [ ] Commitment Log active with weekly review cadence
- [ ] Stakeholder satisfaction baseline recorded

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, stakeholder-management, communication, external-relations

## Enforcement Loop
- WHERE: Any product with investors, partners, enterprise customers, or regulatory oversight
- WHEN: At product launch, fundraising, partnership formation, or when stakeholder health turns Yellow/Red
- HOW: Steps 1-9; commitment log reviewed weekly, satisfaction measured annually
- CONNECT: DPM-004 (cross-functional alignment), DPM-081 (roadmap stakeholder management)

## Dependente
- CRM or stakeholder tracking system
- Executive buy-in for transparent communication
- Product metrics dashboard accessible for stakeholder updates

## Metrics
- Stakeholder satisfaction: >80% Green health status
- Commitment delivery rate: % of promises kept on-time (target >90%)
- Surprise escalation frequency: target zero (all risks communicated proactively)
- Update cadence adherence: % of scheduled updates sent on time
