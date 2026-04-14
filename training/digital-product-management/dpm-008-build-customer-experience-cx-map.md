---
type: procedure
created: 2026-03-20
status: active
slug: dpm-008-build-customer-experience-cx-map
tags: [procedure, nexus]
---

# DPM-008 — Build Customer Experience (CX) Map
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Teams lack visibility into the end-to-end customer journey — optimizing individual touchpoints in isolation while missing critical pain points, emotional valleys, and drop-off moments that destroy retention and lifetime value.

## Procedura
### Prerequisites
- Customer research data (interviews, support tickets, analytics)
- Cross-functional team participation (PM, design, engineering, support, marketing)
- Journey mapping tool (Miro, FigJam, UXPressia, or Smaply)
- Product analytics (Amplitude, Mixpanel, FullStory, or Hotjar)

### Steps
1. **Define the journey scope and persona** — Select ONE specific persona and ONE journey to map (onboarding, purchase, renewal, support escalation). Resist the temptation to map "the entire experience" — a focused map is 10x more useful than a comprehensive but shallow one. Write the persona's JTBD as the journey title: "When [persona] wants to [job], the journey through [product] looks like..."
2. **Identify all journey stages** — List the major phases: Awareness → Consideration → Acquisition → Activation → Engagement → Retention → Expansion → Advocacy. Not all stages apply to every journey — keep only the relevant ones. For each stage, define the entry trigger ("user lands on pricing page") and exit criteria ("user completes first task").
3. **Map actions, touchpoints, and channels per stage** — For each stage, document: (a) What the user does (clicks, reads, calls, waits), (b) Touchpoints (landing page, onboarding wizard, support chat, email), (c) Channels (web, mobile, email, phone, in-person). Use analytics data to verify — check Amplitude funnels or FullStory session replays for actual behavior vs. assumed behavior.
4. **Layer emotional experience using the emotion curve** — For each stage, plot the user's emotional state: positive (delighted, confident), neutral, or negative (frustrated, confused, anxious). Use customer interview quotes to anchor each emotion. Data sources: NPS verbatims, support ticket sentiment, session replay observations, and CSAT scores per touchpoint. Draw the emotion curve on the map.
5. **Identify Moments of Truth (MoTs)** — Mark the 3-5 moments where experience makes or breaks the relationship. Classic MoTs: first value delivered (Aha moment), first support interaction, renewal decision, referral prompt. For each MoT, calculate the conversion/retention rate at that point. Example: "Only 34% of users reach Aha moment (first report generated) within 7 days."
6. **Quantify pain points and friction** — For each negative-emotion stage, document: the specific friction (long load time, confusing UI, missing feature), the quantitative impact (drop-off rate, support ticket volume, churn correlation), and the user quote that captures the frustration. Prioritize pain points by: revenue impact = (users affected) x (revenue per user) x (probability of churn due to pain).
7. **Identify opportunities and quick wins** — For each pain point, brainstorm 2-3 potential solutions. Classify as: Quick Win (low effort, high impact — do this sprint), Strategic Investment (high effort, high impact — roadmap item), or Nice-to-Have (low impact — park it). Use the Kano model to differentiate: Must-be fixes (pain removal), Performance improvements (linear satisfaction), and Delighters (unexpected positive moments).
8. **Create the visual CX map** — Build the map in Miro, FigJam, or UXPressia with swim lanes: Stage → Actions → Touchpoints → Emotions → Pain Points → Opportunities. Use color coding: red for pain points, green for delighters, yellow for neutral friction. Include real user quotes alongside each stage. The map should fit on one large screen or printable poster.
9. **Validate the map with real users** — Share the draft map with 3-5 customers in a 30-min session. Ask: "Does this accurately reflect your experience?" Capture corrections and blind spots you missed. Update the map based on feedback. Internal teams always overestimate how smooth the experience is.
10. **Operationalize the map** — Assign each pain point to a team with a target resolution quarter. Set up dashboards in Amplitude or Mixpanel tracking the key conversion rates at each Moment of Truth. Review the CX map quarterly in the product review. Update it after every major feature launch or journey change. The map is a living document, not a one-time exercise.

### Verification
- [ ] Journey scope defined with specific persona and JTBD
- [ ] All stages mapped with actions, touchpoints, channels, and emotions
- [ ] 3-5 Moments of Truth identified with conversion metrics
- [ ] Pain points quantified by revenue impact
- [ ] Visual CX map created and validated with 3+ real users
- [ ] Pain points assigned to teams with target resolution quarters

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, customer-experience, journey-mapping, CX, kano-model

## Enforcement Loop
- WHERE: Product strategy, UX redesigns, onboarding optimization, churn reduction
- WHEN: Quarterly review, after major feature launches, when retention drops below target
- HOW: Steps 1-10; refresh the map quarterly, validate with users semi-annually
- CONNECT: DPM-007 (customer discovery), DPM-013 (acquisition funnel), DPM-023 (personas), DPM-041 (CX prioritization)

## Dependente
- Customer research data (interviews, tickets, analytics)
- Cross-functional participation for touchpoint coverage
- Analytics platform with funnel and retention analysis

## Metrics
- Moment of Truth conversion rates: track monthly for each MoT
- Pain point resolution rate: % of identified pain points addressed per quarter
- Journey NPS: NPS measured at each stage (not just overall)
- Time-to-Aha-Moment: target reduction quarter-over-quarter
