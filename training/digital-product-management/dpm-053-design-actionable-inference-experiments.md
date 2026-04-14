---
type: procedure
created: 2026-03-17
status: active
slug: dpm-053-design-actionable-inference-experiments
tags: [procedure, nexus]
---

# DPM-053 — Design Actionable Inference Experiments

## Problema
Product experiments are designed to show activity but don't actually produce actionable business inferences.

## Procedura
### Prerequisites
- Hypothesis to test
- Analytics infrastructure
- Statistical knowledge basics

### Steps
1. Define the inference you need: 'We need to know if [change] causes [outcome]'
2. Choose experiment type: A/B test (causal), cohort analysis (correlation), or before/after (temporal)
3. Define the primary metric and guardrail metrics (metrics that must not regress)
4. Calculate required sample size for statistical significance (use an online calculator)
5. Design the experiment to isolate the variable: only change one thing
6. Set the experiment duration: long enough for sample size, short enough for business needs
7. Define the decision framework: if metric improves by X%, ship; if flat, iterate; if negative, revert

### Verification
- Experiment designed with one isolated variable
- Sample size calculated
- Decision framework documented

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, experimentation, inference

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Hypothesis to test
- Analytics infrastructure
- Statistical knowledge basics

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
