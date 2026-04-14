---
type: procedure
created: 2026-03-22
status: active
slug: c-062-achieve-framework
tags: [procedure, nexus]
---

# Apply the ACHIEVE Framework for AI Problem Selection — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Systematic evaluation of whether a business problem is suitable for AI using the ACHIEVE framework — with quantitative scoring, Python tooling, and multi-model routing for each evaluation dimension.

---

## 1. Problema

Organizations waste resources building AI solutions for problems that are poorly defined, lack data, or carry unacceptable ethical risk. Ad-hoc "let's try AI" approaches lead to failed pilots and wasted budgets. The ACHIEVE framework provides a structured, scoreable evaluation that produces a GO/NO-GO decision with documented justification.

**When to use**:
- Evaluating whether a new business problem merits AI investment
- Prioritizing an AI project backlog with limited resources
- Building a business case for AI adoption before stakeholders
- Screening problems before committing engineering time

**When NOT to use**:
- Simple automation tasks (rule-based, no ML needed) — use traditional scripting
- Problems where you already have a working non-AI solution that meets all requirements
- Exploratory research where the problem itself is not yet defined — use discovery workshops first
- Tasks where data collection alone would take >6 months — solve the data problem first

---

## 2. Procedura

### Pas 1: Articulate — Formulează problema în format structurat
Write a problem statement following this template:

```
PROBLEM STATEMENT:
WHO: [affected role/team/customer segment]
NEEDS TO: [specific action or capability]
BECAUSE: [business reason / pain point]
BUT: [current obstacle preventing solution]
MEASURED BY: [quantifiable metric that defines success]
```

**Example**:
```
WHO: Customer support team (12 agents)
NEEDS TO: Classify incoming tickets by urgency and department
BECAUSE: Manual triage takes 45 min/day per agent and misroutes 23% of tickets
BUT: Volume has grown 3x in 6 months, can't hire fast enough
MEASURED BY: Triage accuracy >90%, processing time <5 sec/ticket
```

Validate: Is the problem specific? Is the metric measurable? Is the obstacle real and current?

### Pas 2: Categorize — Clasifică tipul de date
Determine the data landscape using this Python assessment:

```python
import json

data_assessment = {
    "primary_data_type": "structured",  # structured | unstructured | hybrid
    "data_sources": [
        {"name": "ticket_db", "type": "PostgreSQL", "rows": 150000, "quality": "high"},
        {"name": "email_corpus", "type": "text", "docs": 50000, "quality": "medium"},
    ],
    "labeled_examples": 5000,  # for supervised learning
    "data_freshness": "daily",  # real-time | daily | weekly | static
    "pii_present": True,
    "bias_risk": "medium",  # low | medium | high
}

# Data type -> Model family mapping
MODEL_MAP = {
    "structured": ["scikit-learn", "XGBoost", "LightGBM"],
    "unstructured": ["Claude Opus/Sonnet", "GPT-4o", "Gemini Pro"],
    "hybrid": ["LLM + structured ML pipeline"],
}
print(f"Recommended: {MODEL_MAP[data_assessment['primary_data_type']]}")
```

### Pas 3: Humans Assess — Evaluează disponibilitatea și calitatea datelor
Human team validates data readiness. Use this checklist:

| Question | Answer | Impact |
|----------|--------|--------|
| Sufficient volume? (>1000 for ML, >100 for LLM few-shot) | YES/NO | Blocks supervised learning |
| Data clean and representative? | YES/NO | Affects model accuracy |
| Historical labeled data exists? | YES/NO | Determines supervised vs unsupervised |
| Data accessible via API/export? | YES/NO | Affects pipeline complexity |
| Data refresh frequency matches need? | YES/NO | Stale data = degraded predictions |

Document every gap with a remediation plan and estimated timeline.

### Pas 4: Identify — Calculează AI Suitability Score

```python
import numpy as np

dimensions = {
    "data_volume": 4,        # 1=<100 examples, 5=>100K examples
    "repetitiveness": 5,     # 1=unique cases, 5=highly repetitive
    "pattern_clarity": 4,    # 1=no patterns, 5=clear patterns
    "error_tolerance": 3,    # 1=zero tolerance, 5=errors acceptable
    "manual_cost": 5,        # 1=cheap manual, 5=expensive manual
}

weights = {
    "data_volume": 1.5,      # data is foundational
    "repetitiveness": 1.2,
    "pattern_clarity": 1.3,
    "error_tolerance": 0.8,
    "manual_cost": 1.2,
}

weighted_score = sum(dimensions[k] * weights[k] for k in dimensions)
max_score = sum(5 * weights[k] for k in weights)
pct = (weighted_score / max_score) * 100

print(f"AI Suitability: {weighted_score:.1f}/{max_score:.1f} ({pct:.0f}%)")
print(f"Decision: {'GO' if pct >= 60 else 'REVIEW' if pct >= 40 else 'NO-GO'}")
# GO: >=60% | REVIEW: 40-59% | NO-GO: <40%
```

### Pas 5: Evaluate — Cost-Benefit Analysis

```python
# Simplified ROI calculation
annual_manual_cost = 12 * 8 * 250 * 35  # 12 agents x 8h x 250 days x $35/h
ai_implementation_cost = 50000           # dev + infra + training
ai_annual_running_cost = 12000           # API costs + maintenance
annual_savings = annual_manual_cost * 0.40  # 40% efficiency gain

roi = ((annual_savings - ai_annual_running_cost) / ai_implementation_cost) * 100
breakeven_months = ai_implementation_cost / ((annual_savings - ai_annual_running_cost) / 12)

print(f"Annual manual cost: ${annual_manual_cost:,}")
print(f"Expected annual savings: ${annual_savings:,}")
print(f"ROI: {roi:.0f}% | Break-even: {breakeven_months:.1f} months")
```

### Pas 6: Verify — Ethical and Compliance Check

| Risk | Assessment | Mitigation |
|------|-----------|------------|
| PII/GDPR exposure | YES/NO | Anonymization pipeline |
| Bias in training data | LOW/MED/HIGH | Fairness audit pre-deploy |
| Affected rights/decisions | YES/NO | Human-in-the-loop for high-stakes |
| Regulatory (AI Act, GDPR) | List applicable | Compliance review with legal |
| Explainability requirement | YES/NO | Interpretable models or SHAP |

### Pas 7: Execute — Define and Launch Pilot

```python
pilot_spec = {
    "objective": "Validate ticket classification accuracy on live data",
    "duration_weeks": 3,
    "dataset": "Last 30 days of tickets (n=4500)",
    "success_metrics": {
        "accuracy": ">= 0.90",
        "latency_p95": "<= 2.0 seconds",
        "human_override_rate": "<= 10%",
    },
    "go_no_go_criteria": "All 3 metrics met on held-out test set",
    "rollback_plan": "Revert to manual triage within 1 business day",
}
```

### Pas 8: Document Decision and Archive
Save the complete ACHIEVE assessment (all 7 prior steps) as a decision record. Include: problem statement, scores, ROI, ethical assessment, pilot results, and final GO/NO-GO with justification. Store in project folder and Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "ACHIEVE Framework applied for [problem]. Suitability Score: [X]% ([weighted]/[max]). ROI: [X]%. Ethical risks: [count]. Decision: [GO/REVIEW/NO-GO]. Pilot: [defined/launched/completed].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-062",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["achieve-framework", "problem-selection", "ai-suitability", "roi-analysis", "pilot"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
Before any new AI initiative or when reviewing existing AI project viability

### HOW (violation detection)
- ACHIEVE executed without all 8 steps → violation
- Output without VK → violation
- Suitability Score calculated without weighted dimensions → violation
- GO decision without ethical check (Pas 6) → violation
- High-stakes problem approved without human-in-the-loop → critical violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis
- C-068 (Right Problem for AI) → quick screening before full ACHIEVE

### VERIFY
- [ ] All 8 steps executed with documented outputs?
- [ ] Suitability Score calculated with weighted dimensions?
- [ ] ROI analysis includes break-even timeline?
- [ ] Ethical checklist completed with mitigations?
- [ ] VK emitted?

**VK-uri:**
1. `✅ [PROC] C-062 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the ACHIEVE Framework for AI Problem Selection" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Justification |
|-----------|-------|---------------|
| Suitability scoring | Sonnet | Structured evaluation, fast |
| Ethical review | Opus | Nuanced judgment required |
| ROI calculation | Haiku/Sonnet | Straightforward computation |
| Pilot design | Sonnet | Balanced detail/speed |
| Full ACHIEVE audit | Opus | Deep analysis needed |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Business problem description | Input for Pas 1 articulation |
| Data inventory | Input for Pas 2-3 assessment |
| Financial data (costs, volumes) | Input for Pas 5 ROI |
| Legal/compliance team | Validation for Pas 6 |
| Cortex | Logging and decision archival |
| Python (numpy) | Suitability scoring |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% steps with documented outputs |
| VK emission | 2/2 |
| Suitability Score documented | Yes, with all 5 weighted dimensions |
| ROI break-even calculated | Yes, in months |
| Ethical checklist complete | 100% items assessed |
| Decision archived in Cortex | Yes |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Initial version |
| **2.0** | **2026-03-20** | Opus v2 rewrite: 8 actionable steps, Python scoring code, weighted suitability dimensions, ROI calculator, structured problem template, model routing with justification, when to use/NOT use section |
