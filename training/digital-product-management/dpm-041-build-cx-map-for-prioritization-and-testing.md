---
type: procedure
created: 2026-03-17
status: active
slug: dpm-041-build-cx-map-for-prioritization-and-testing
tags: [procedure, nexus]
---

# DPM-041 — Build CX Map for Prioritization and Testing

## Problema
Product decisions are made without understanding which moments in the customer experience matter most.

## Procedura
### Prerequisites
- Customer journey data
- CX map (DPM-008)
- Prioritization framework

### Steps
1. Take your CX map and add a 'satisfaction score' (1-5) to each touchpoint based on user feedback
2. Add an 'importance score' (1-5) to each touchpoint based on impact on retention/conversion
3. Calculate opportunity score: importance - satisfaction (high score = biggest opportunity)
4. Rank touchpoints by opportunity score
5. For the top 3 opportunities, define a hypothesis: 'If we improve [touchpoint], [metric] will increase by [X%]'
6. Design a test for each hypothesis (A/B test, prototype test, or service design experiment)
7. Feed prioritized CX improvements into your product roadmap

### Verification
- CX map scored with satisfaction and importance
- Top 3 opportunities identified
- Hypotheses and tests designed

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, customer-experience, prioritization

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Customer journey data
- CX map (DPM-008)
- Prioritization framework

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
