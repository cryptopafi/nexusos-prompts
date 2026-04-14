---
type: procedure
created: 2026-03-20
status: active
slug: dpm-007-conduct-customer-discovery-research
tags: [procedure, nexus]
---

# DPM-007 — Conduct Customer Discovery Research
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Products are designed based on internal assumptions instead of deep customer understanding — leading to features that solve imagined problems while real pain points go unaddressed. Teams skip discovery or do it superficially, then wonder why adoption flatlines.

## Procedura
### Prerequisites
- Target customer segment defined (from DPM-002 or existing persona)
- Recruiting channel: existing users, social media, UserTesting.com, Respondent.io, or partner networks
- Recording tool: Zoom, Grain, or Dovetail
- Synthesis tool: Dovetail, Miro, or Notion

### Steps
1. **Define the research objective and scope** — Write a 1-paragraph research brief: "We need to understand [specific aspect] about [segment] to inform [product decision]." Scope boundaries: what's in (e.g., "how enterprise buyers evaluate security features") and what's out (e.g., "pricing willingness — separate study"). Good objectives answer: "What decision will this research enable?"
2. **Choose the research methodology** — Match method to objective: (a) Generative/exploratory → in-depth interviews (1:1, 45-60 min), (b) Behavioral understanding → contextual inquiry (observe users in natural environment), (c) Pattern validation → surveys (n=50+ for quantitative significance), (d) Concept testing → prototype walkthrough with think-aloud protocol. For early-stage discovery, default to interviews.
3. **Create a discussion guide** — Write 10-12 questions following the "Mom Test" principles: ask about past behavior, not future intent. Structure: (a) Context questions — "Walk me through the last time you [task]," (b) Problem exploration — "What's the hardest part about [task]?", (c) Current solutions — "What tools/workarounds do you use today?", (d) Emotional drivers — "How does [problem] make you feel?", (e) Value signal — "What would solving this be worth to you?" Never ask "Would you use a product that...?"
4. **Recruit 12-15 participants** — Over-recruit by 20% (expect 2-3 no-shows). Screen for: segment fit, recency of experience (problem encountered in last 30 days), diversity across use cases. Offer appropriate incentives: $50-100 for B2C consumers, $150-300 for B2B professionals. Use a screener survey (Google Form) to filter. Track recruitment funnel: invited → screened → scheduled → completed.
5. **Conduct interviews with structured observation** — Two-person team: interviewer (asks questions, builds rapport) and note-taker (captures verbatim quotes, timestamps key moments, tags themes in real-time). Record every session (with consent). Follow the guide loosely — the best insights come from follow-up probes: "Tell me more about that," "Why is that important?" "What happened next?"
6. **Debrief after each session** — Immediately after each interview (within 30 min), capture: top 3 insights, surprise moments, new hypotheses generated, and quotes to highlight. Use a session debrief template (Notion). Pattern recognition improves dramatically when you debrief in real-time rather than batch-processing all interviews later.
7. **Synthesize using affinity mapping** — After all interviews, extract individual insights onto sticky notes (physical or Miro/FigJam). Group into clusters. Name each cluster as a theme with an evidence count (e.g., "Manual data entry is the primary time sink — 9/12 participants"). Rank themes by frequency AND severity. Generate an insight hierarchy: themes → sub-themes → supporting quotes.
8. **Create the discovery deliverable** — Write a Customer Discovery Report containing: (a) Research objective and methodology, (b) Participant demographics table, (c) Top 5-7 findings with supporting quotes (min 3 per finding), (d) Jobs-to-Be-Done map updated with new evidence, (e) Opportunity areas ranked by frequency/severity, (f) Recommended next steps (build, test further, or pivot). Use Dovetail for tagging or Notion for structured documentation.
9. **Present findings and drive action** — Share the report in a 30-min team session. Lead with the most surprising finding. Use video clips from interviews (30-60s each) to create empathy — seeing real users struggle is more persuasive than any slide. End with specific product implications: "Based on this research, we should [action] because [evidence]." Assign follow-up owners and deadlines.
10. **Build a continuous discovery habit** — Customer discovery is not a phase — it is an ongoing practice. Target: minimum 2 customer conversations per week per PM (Teresa Torres' continuous discovery model). Maintain a Research Repository (Dovetail, Notion) where all insights are searchable and tagged. Review and refresh every quarter.

### Verification
- [ ] Research brief written with clear objective and scope
- [ ] Discussion guide created following Mom Test principles
- [ ] 10+ interviews completed and recorded
- [ ] Affinity map synthesized with themed findings
- [ ] Discovery report delivered with actionable recommendations
- [ ] Continuous discovery cadence established (2 conversations/week)

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, customer-discovery, user-research, interviews, mom-test

## Enforcement Loop
- WHERE: Pre-build validation, new market exploration, retention problem diagnosis
- WHEN: Before every major product decision, and continuously at 2 conversations/week
- HOW: Steps 1-10; debrief after every session, synthesize after every cohort of 10+
- CONNECT: DPM-002 (PMF hypotheses), DPM-008 (CX map), DPM-023 (personas), DPM-026 (interview guide), DPM-027 (interview execution)

## Dependente
- Budget for participant incentives ($50-300 per participant)
- Recording and consent workflow
- Synthesis tool (Dovetail, Miro, or Notion)

## Metrics
- Interview completion rate: target 80%+ of recruited participants
- Insight actionability: % of findings that lead to product backlog items within 2 weeks
- Continuous discovery cadence: 2+ customer conversations per PM per week
- Research-to-decision latency: days between report delivery and product decision made
