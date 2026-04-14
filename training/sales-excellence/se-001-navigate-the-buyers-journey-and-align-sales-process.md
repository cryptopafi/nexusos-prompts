---
type: procedure
created: 2026-03-20
status: active
slug: se-001-navigate-the-buyers-journey-and-align-sales-process
tags: [procedure, nexus]
---

# SE-001 — Navigate the Buyer's Journey and Align Sales Process

## Problema
Sales reps push their agenda instead of matching the buyer's decision stage, creating friction that kills 67% of deals before they reach proposal (Gartner). Misalignment between seller activities and buyer readiness is the #1 cause of pipeline stall.

## Procedura
### Prerequisites
- CRM access with deal history (HubSpot, Salesforce, Pipedrive)
- Minimum 10 closed/lost deals for retrospective analysis
- Buyer journey mapping template (Lucidchart, Miro, or spreadsheet)

### Steps
1. **Map the buyer journey stages** — Define three stages with buyer-side milestones: (a) Awareness: buyer recognizes a problem, searches for education; (b) Consideration: buyer defines requirements, evaluates categories of solutions; (c) Decision: buyer selects vendor, negotiates terms. Document 3-5 buyer questions per stage.
2. **Audit your current sales process** — List every sales activity you perform (cold call, demo, proposal, etc.). Tag each with the buyer stage it serves. Identify orphan activities that don't map to any stage and activities clustered in one stage.
3. **Build the alignment matrix** — Create a 3-column table: Buyer Stage | Buyer Needs/Questions | Seller Activities & Content. For Awareness: educational blog posts, pain-point discovery calls. For Consideration: case studies, ROI calculators, competitive comparisons. For Decision: proposals, references, pilot programs.
4. **Define stage-gate criteria** — Set specific signals that confirm the buyer has moved stages. Awareness→Consideration: buyer articulates their problem and asks "how." Consideration→Decision: buyer shares budget range, timeline, and decision-making process (BANT or MEDDIC qualified). No advancing deals without gate confirmation.
5. **Retrospectively map 10 recent deals** — Pull 5 won and 5 lost deals from CRM. For each, identify: which stage the deal stalled or accelerated, what content/activity was delivered at each stage, and where misalignment occurred. Use the Sandler Submarine framework to diagnose: did you skip Pain, Budget, or Decision steps?
6. **Identify the #1 alignment gap** — Analyze retrospectives for patterns. Common gaps: (a) jumping to demo before buyer articulates pain (Awareness skip); (b) sending proposals before understanding decision process (Consideration skip); (c) no nurture content for Awareness-stage leads. Quantify the gap: "X% of lost deals stalled at Consideration because we had no case studies."
7. **Create stage-specific playbooks** — For each stage, document: opening questions, content to share, objections to expect, CRM fields to update, and exit criteria. Use SPIN Selling question types: Situation (Awareness), Problem/Implication (Consideration), Need-Payoff (Decision).
8. **Implement CRM stage tracking** — Configure your CRM pipeline stages to mirror the buyer journey (not your internal process). Add required fields at each stage: Awareness requires "Pain Statement," Consideration requires "Budget Range + Timeline," Decision requires "Decision Criteria + Competitors." Set automation alerts for deals stuck >14 days in one stage.
9. **Train and role-play** — Run a 60-minute workshop with the team. Present the alignment matrix. Role-play three scenarios: (a) buyer in Awareness who gets a premature demo, (b) buyer in Consideration who needs competitive ammo, (c) buyer in Decision who needs procurement help. Score each role-play on alignment quality (1-5).
10. **Measure and iterate monthly** — Track: stage conversion rates (Awareness→Consideration, Consideration→Decision), average days per stage, and win rate by entry point. Benchmark: top performers convert Awareness→Consideration at 40%+ and Consideration→Decision at 60%+. Review monthly, adjust playbooks based on data.

### Verification
- Alignment matrix completed with all 3 stages, buyer questions, and seller activities
- 10-deal retrospective documented with gap analysis
- CRM pipeline reconfigured with stage-gate required fields
- Stage conversion rates baselined and tracked

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, sales-process, buyer-journey, SPIN, pipeline-alignment
- version: 2.0

## Enforcement Loop
- WHERE: Training and skill development — sales process design
- WHEN: Quarterly review or when win rate drops below 25%
- HOW: Follow steps sequentially, verify alignment matrix and CRM config
- CONNECT: SE-007 (Advance), SE-010 (Consultative Selling), SE-029 (Pipeline Management)

## Dependente
- CRM with pipeline customization capability
- 10+ historical deals for retrospective
- SPIN Selling or Sandler methodology reference

## Metrics
- Completion: all 10 steps executed, alignment matrix + CRM config live
- Quality: stage conversion rates measured and baselined
- Benchmark: win rate improvement >10% within 90 days of implementation
- Level: ADVANCED
