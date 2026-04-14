---
type: procedure
created: 2026-03-20
status: active
slug: dpm-003-design-product-testing-hypotheses
tags: [procedure, nexus]
---

# DPM-003 — Design Product Testing Hypotheses
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Product teams run experiments without rigorous hypothesis design — leading to ambiguous results, wasted test cycles, and decisions based on intuition rather than evidence.

## Procedura
### Prerequisites
- Product backlog with candidate features or changes to test
- Baseline metrics from analytics (Amplitude, Mixpanel, or GA4)
- Understanding of statistical significance (p-value, confidence intervals)
- Experiment tracker (Notion, Google Sheets, or Eppo/Statsig)

### Steps
1. **Identify the assumption to test** — For each product decision, extract the underlying assumption. Use Assumption Mapping: plot on a 2x2 of (Importance to success) vs. (Certainty of knowledge). Prioritize high-importance, low-certainty assumptions first. List the top 3-5.
2. **Classify hypothesis type** — Categorize into: (a) Desirability — "Users want this," (b) Usability — "Users can use this," (c) Feasibility — "We can build this," (d) Viability — "This sustains the business." Each type requires different experiment designs and metrics. Use the Design Thinking diamond to guide classification.
3. **Write the hypothesis in structured format** — Template: "We believe that [change/intervention] for [user segment] will result in [measurable outcome] as measured by [specific metric]. We will know this is true when [metric] changes by [threshold] within [timeframe]." Reject vague outcomes like "improved engagement" — specify the metric and delta.
4. **Define primary metric and guardrails** — Select ONE primary metric (e.g., conversion rate, activation rate, 7-day retention). Add 2-3 guardrail metrics that must not degrade (page load time <2s, error rate <1%, support ticket volume). Use the HEART framework (Happiness, Engagement, Adoption, Retention, Task success) for balanced metric selection.
5. **Calculate required sample size** — Use power analysis (Evan Miller's calculator or Statsig). Inputs: baseline conversion rate, minimum detectable effect (MDE), significance level (95%), power (80%). Example: 5% baseline with 10% relative MDE needs ~31,000 samples per variant. Document required runtime before launching.
6. **Select experiment methodology** — Match to context: (a) A/B test — sufficient traffic, causal evidence needed, (b) Multivariate — testing multiple variables, (c) Before/after — low traffic, (d) Fake door test — demand validation before building, (e) Painted door — feature interest signals. Match method to hypothesis type from Step 2.
7. **Design control and treatment precisely** — Document exactly what differs between control and treatment using screenshots or Figma wireframes. Ensure the treatment isolates a single variable. If testing multiple changes, use multivariate design or sequential tests. Never bundle unrelated changes.
8. **Pre-register the hypothesis** — Before launching, record hypothesis, methodology, sample size, primary metric, success threshold, and analysis plan in the experiment tracker. Include the decision framework: "If [metric] improves by >[threshold] with p<0.05, we ship. If guardrails degrade by >X%, we roll back regardless." Pre-registration prevents post-hoc rationalization.
9. **Run and monitor for validity threats** — Launch the experiment. Monitor daily: (a) Implementation correctness (variants exposing correctly?), (b) Sample ratio mismatch — if variant sizes differ by >1%, investigate, (c) Guardrail metric alerts. Do NOT peek at results for early decisions unless using sequential testing with proper stopping rules.
10. **Analyze, document, and share learnings** — At conclusion, run the pre-registered analysis. Document: hypothesis, result (confirmed/rejected/inconclusive), effect size with confidence interval, unexpected findings, and next action. Store in experiment repository. Share in weekly product review. Build institutional learning over time.

### Verification
- [ ] Top 3-5 assumptions mapped on Importance vs. Certainty matrix
- [ ] Each hypothesis written with specific metric, threshold, and timeframe
- [ ] Sample size calculated via power analysis before launch
- [ ] Experiment pre-registered with decision framework
- [ ] Results documented with effect size and confidence interval

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, hypothesis-testing, experimentation, A/B-testing, statistics

## Enforcement Loop
- WHERE: Feature development, growth experiments, UX changes
- WHEN: Before any product change that could be tested rather than assumed
- HOW: Steps 1-10 per experiment cycle; never skip pre-registration (Step 8)
- CONNECT: DPM-053 (inference experiments), DPM-055 (statistical significance), DPM-060 (A/B tests)

## Dependente
- Analytics tool with event tracking configured
- Experimentation platform (Statsig, Eppo, LaunchDarkly, or Optimizely)
- Minimum traffic for statistical power

## Metrics
- Experiment velocity: tests completed per quarter (target: 4-8)
- Hypothesis validation rate: % confirmed vs. total tested
- Decision quality: % of shipped features hitting predicted metric impact within 20%
- Average experiment runtime vs. planned runtime
