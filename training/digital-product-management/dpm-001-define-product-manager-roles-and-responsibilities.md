---
type: procedure
created: 2026-03-20
status: active
slug: dpm-001-define-product-manager-roles-and-responsibilities
tags: [procedure, nexus]
---

# DPM-001 — Define Product Manager Roles and Responsibilities
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Teams conflate PM with project manager, product owner, and UX lead — creating accountability gaps, duplicated effort, and slow decision-making that erodes product velocity.

## Procedura
### Prerequisites
- Organizational chart and reporting lines for the product team
- Access to stakeholders across engineering, design, marketing, sales, and support
- Template: RACI matrix (Responsible, Accountable, Consulted, Informed)

### Steps
1. **Audit existing role definitions** — Pull job descriptions for PM, PO, Project Manager, UX Designer, and Engineering Lead. Highlight overlapping responsibilities in a shared spreadsheet (Google Sheets or Notion table). Flag any responsibility claimed by 2+ roles.
2. **Map the PM competency model** — Use the 4-pillar framework: (a) Product Strategy (vision, market positioning, competitive moats), (b) Customer Insight (user research, persona management, JTBD), (c) Execution (roadmap, sprint planning, release management), (d) Influence (stakeholder alignment, cross-functional leadership, escalation). Rate current team maturity 1-5 on each pillar.
3. **Build the RACI chart** — For each key activity (roadmap creation, backlog grooming, sprint planning, customer interviews, go-to-market, post-launch review), assign exactly one R and one A. Use Confluence or Notion. Validate: no activity should have zero A.
4. **Define decision rights with the RAPID framework** — For the top 10 recurring product decisions (feature prioritization, pricing changes, technical debt allocation, partner integrations), specify who Recommends, who Agrees, who Performs, who has Input, and who Decides. Document in a single-page Decision Rights Matrix.
5. **Create the PM Role Charter** — One-page document containing: mission statement, 5-7 core responsibilities, decision rights summary, success metrics (e.g., feature adoption rate >60%, NPS delta, on-time delivery %), and explicit non-responsibilities. Use the charter template from Productboard or Notion.
6. **Validate with cross-functional leads** — Schedule 30-min 1:1s with engineering lead, design lead, and marketing lead. Walk through the charter. Capture objections in a feedback log. Resolve conflicts by referencing the RACI and RAPID outputs.
7. **Socialize and ratify** — Present the charter in a team all-hands. Get explicit sign-off from VP/Director of Product. Store the charter in the team wiki (Confluence, Notion, or Google Docs) with a quarterly review reminder.
8. **Instrument role clarity metrics** — Run a quarterly 5-question Likert survey (e.g., "I know who decides feature priority" — 1-5). Target: >4.0 average within two quarters. Track in Amplitude or a simple Google Form dashboard.
9. **Iterate quarterly** — At each quarter boundary, review the charter against actual behavior. Update RACI for any new activities (e.g., AI feature governance). Archive previous version for audit trail.

### Verification
- [ ] RACI chart completed with zero unassigned accountability rows
- [ ] RAPID Decision Rights Matrix covers top 10 decisions
- [ ] PM Role Charter ratified by VP/Director with date stamp
- [ ] Role clarity survey baseline score recorded
- [ ] Charter stored in team wiki with quarterly review calendar event

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, role-definition, RACI, RAPID, organizational-design

## Enforcement Loop
- WHERE: Product team onboarding, org restructures, new PM hires
- WHEN: At team formation, quarterly reviews, or when role confusion surfaces
- HOW: Follow steps 1-9 sequentially; validate via verification checklist
- CONNECT: DPM-004 (cross-functional alignment), DPM-005 (PM vs PO), DPM-066 (team charter)

## Dependente
- Org chart and reporting structure
- Access to cross-functional leads for RACI validation
- Wiki/documentation tool (Confluence, Notion, or Google Docs)

## Metrics
- Role clarity survey score: target >4.0/5.0
- Time-to-decision on feature prioritization: baseline vs. post-charter
- Escalation frequency: should decrease 30%+ after charter adoption
