---
type: procedure
created: 2026-03-17
status: active
slug: dpm-055-calculate-statistical-significance-for-product-experiments
tags: [procedure, nexus]
---

# DPM-055 — Calculate Statistical Significance for Product Experiments

## Problema
Teams make ship/kill decisions based on experiment results without understanding statistical significance.

## Procedura
### Prerequisites
- Experiment results data
- Statistics basics
- Calculator or Python/R

### Steps
1. Understand p-value: probability that the observed difference is due to chance (target p < 0.05)
2. Understand confidence interval: range within which the true effect likely falls
3. Calculate sample size needed before the experiment starts (using power analysis)
4. After the experiment, calculate the test statistic (t-test for means, chi-square for proportions)
5. Check for practical significance: is the effect large enough to matter, even if statistically significant?
6. Watch for common pitfalls: peeking at results early, stopping early, multiple comparisons
7. Document the statistical analysis alongside the business recommendation

### Verification
- Sample size pre-calculated
- Statistical test applied to results
- Business recommendation includes statistical context

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, statistics, experimentation

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing digital-product-management skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: Related procedures in same domain

## Dependente
- Experiment results data
- Statistics basics
- Calculator or Python/R

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
