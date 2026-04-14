---
type: procedure
created: 2026-03-17
status: active
slug: dpm-060-set-up-and-run-a-b-tests
tags: [procedure, nexus]
---

# DPM-060 — Set Up and Run A/B Tests

## Problema
Feature decisions are based on opinions when A/B tests could provide data-driven answers.

## Procedura
### Prerequisites
- A/B testing tool (Optimizely, LaunchDarkly, Google Optimize)
- Hypothesis to test
- Traffic volume

### Steps
1. Define the hypothesis: 'Variant B will improve [metric] by [X%] compared to control A'
2. Calculate required sample size for 95% confidence and 80% power
3. Design the variants: change only one variable between A and B
4. Set up the test in your A/B tool with proper traffic allocation (usually 50/50)
5. Define guardrail metrics that must not regress
6. Run the test until sample size is reached (do not peek and stop early)
7. Analyze results: statistical significance, practical significance, and impact on guardrails

### Verification
- A/B test configured and launched
- Run to full sample size
- Results analyzed with recommendation

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, ab-testing, experimentation

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- A/B testing tool (Optimizely, LaunchDarkly, Google Optimize)
- Hypothesis to test
- Traffic volume

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
