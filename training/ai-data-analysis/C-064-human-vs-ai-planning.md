---
type: procedure
created: 2026-03-22
status: active
slug: c-064-human-vs-ai-planning
tags: [procedure, nexus]
---

# Apply Human vs. AI Task Allocation for Complex Projects — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Systematic decomposition and optimal allocation of sub-tasks between human and AI execution, with scored handoff protocols and Python tooling for allocation matrices.

---

## 1. Problema

Complex tasks fail when AI handles judgment-heavy sub-tasks it cannot reliably perform, or when humans spend time on repetitive sub-tasks AI could automate. This procedure creates a scored allocation plan that maximizes quality and efficiency through deliberate human-AI task partitioning.

**When to use**:
- Multi-step projects mixing creative, analytical, and mechanical work
- Decision workflows requiring both data processing and contextual judgment
- Content production at scale with quality requirements
- Any project where naive "AI does everything" or "human does everything" is suboptimal

**When NOT to use**:
- Fully automated pipelines with no human touchpoints needed
- Purely creative/artistic work where AI is only an optional brainstorming tool
- Simple tasks completable in <5 minutes by either human or AI alone
- Emergency situations requiring immediate action (no time for planning)

---

## 2. Procedura

### Pas 1: Decompose task into atomic sub-tasks

```python
# Task decomposition template
subtasks = [
    {"id": "T1", "name": "Gather raw sales data from 3 sources", "output": "CSV files", "depends_on": []},
    {"id": "T2", "name": "Clean and normalize data", "output": "Clean DataFrame", "depends_on": ["T1"]},
    {"id": "T3", "name": "Statistical analysis and anomaly detection", "output": "Analysis report", "depends_on": ["T2"]},
    {"id": "T4", "name": "Interview stakeholders for context", "output": "Interview notes", "depends_on": []},
    {"id": "T5", "name": "Write executive narrative combining data + context", "output": "Report draft", "depends_on": ["T3", "T4"]},
    {"id": "T6", "name": "Design data visualizations", "output": "Charts", "depends_on": ["T3"]},
    {"id": "T7", "name": "Final review and sign-off", "output": "Approved report", "depends_on": ["T5", "T6"]},
]
```

Target: 5-15 atomic sub-tasks per complex project. Each has one clear output.

### Pas 2: Score AI-suitability per sub-task

```python
import numpy as np

CRITERIA = ["volume_repetitive", "data_driven", "pattern_clear", "output_verifiable", "error_tolerance"]

scores = {
    "T1": [5, 5, 5, 5, 4],  # Data gathering: highly automatable
    "T2": [5, 5, 4, 5, 3],  # Cleaning: automatable with validation
    "T3": [4, 5, 4, 4, 3],  # Analysis: AI strong, needs review
    "T4": [1, 2, 1, 2, 1],  # Interviews: fundamentally human
    "T5": [2, 3, 2, 3, 2],  # Narrative: needs human judgment
    "T6": [3, 4, 3, 4, 3],  # Visualization: AI can draft, human refines
    "T7": [1, 2, 1, 3, 1],  # Sign-off: human decision required
}

for tid, s in scores.items():
    mean = np.mean(s)
    allocation = "AI" if mean >= 3.5 else "HYBRID" if mean >= 2.5 else "HUMAN"
    print(f"{tid}: {mean:.1f}/5 -> {allocation}")
# T1: 4.8 -> AI | T2: 4.4 -> AI | T3: 4.0 -> AI | T4: 1.4 -> HUMAN
# T5: 2.4 -> HUMAN | T6: 3.4 -> HYBRID | T7: 1.6 -> HUMAN
```

### Pas 3: Build allocation matrix with justifications

| Sub-task | Allocation | Justification | Risk if wrong |
|----------|-----------|---------------|---------------|
| T1: Gather data | AI | Repetitive, API-driven | Low — data validation catches errors |
| T2: Clean data | AI | Pattern-based, verifiable | Medium — bad cleaning propagates |
| T3: Statistical analysis | AI + review | AI generates, human validates | High — wrong analysis = wrong decisions |
| T4: Stakeholder interviews | HUMAN | Requires empathy, context | N/A — cannot be delegated |
| T5: Executive narrative | HUMAN + AI draft | Human judgment, AI assists | High — tone/framing matters |
| T6: Visualizations | HYBRID | AI drafts, human refines | Low — visual review is fast |
| T7: Sign-off | HUMAN | Accountability, authority | N/A — must be human |

### Pas 4: Design handoff protocols

```python
handoffs = [
    {
        "from": "AI (T2)", "to": "AI (T3)",
        "input_format": "pandas DataFrame, cleaned, dtypes validated",
        "validation": "assert df.isnull().sum().sum() == 0",
        "acceptance": "All columns present, no nulls in critical fields",
    },
    {
        "from": "AI (T3)", "to": "HUMAN (T5)",
        "input_format": "JSON report with sections: summary, anomalies, trends",
        "validation": "Human reads and marks accuracy: CONFIRMED / NEEDS_REVISION",
        "acceptance": "All statistical claims verified against raw data",
    },
    {
        "from": "HUMAN (T4)", "to": "HUMAN+AI (T5)",
        "input_format": "Structured interview notes (markdown, key quotes tagged)",
        "validation": "Notes contain at least 3 key insights per stakeholder",
        "acceptance": "Insights mapped to data findings from T3",
    },
]
```

### Pas 5: Execute with allocation discipline
Follow the matrix strictly. Do NOT ad-hoc reassign sub-tasks without re-scoring. At each handoff point, verify input completeness before proceeding. Log actual execution time per sub-task for future calibration.

### Pas 6: Model routing for AI sub-tasks

| Sub-task | Model | Why |
|----------|-------|-----|
| T1: Data gathering | Haiku + scripts | Simple, high-volume |
| T2: Data cleaning | Sonnet | Pattern recognition in messy data |
| T3: Statistical analysis | Opus | Nuanced anomaly detection |
| T6: Visualization drafts | Sonnet | Creative + structured |

### Pas 7: Post-execution retrospective

```python
retrospective = {
    "subtasks_total": 7,
    "ai_executed": 3,
    "human_executed": 3,
    "hybrid": 1,
    "ai_rework_needed": ["T3 — one anomaly was a data entry error, not real"],
    "human_could_have_been_ai": ["T6 — AI draft was 90% usable"],
    "ai_could_not_have_done": ["T4 — stakeholder nuance critical"],
    "time_saved_vs_all_human": "62%",
    "quality_impact": "Equivalent quality, 3x faster",
}
```

Update scoring baselines for similar future projects based on retrospective.

---

## 3. Cortex Logging

```json
{
  "text": "Human vs AI Planning for [project]. Sub-tasks: [X total], AI: [Y], Human: [Z], Hybrid: [W]. Time saved vs all-human: [%]. Rework needed: [count].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-064",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["human-ai-planning", "task-decomposition", "allocation-matrix", "handoff-protocol"],
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
Any project combining human and AI execution

### HOW (violation detection)
- Allocation made without scoring → violation
- High-stakes sub-task delegated to AI without human review → critical violation
- Handoff without defined input/output format → violation
- No retrospective after completion → soft violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis

### VERIFY
- [ ] All sub-tasks scored on 5 criteria?
- [ ] Allocation matrix documented?
- [ ] Handoff protocols defined?
- [ ] Retrospective completed?
- [ ] VK emitted?

**VK-uri:**
1. `✅ [PROC] C-064 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Human vs. AI Task Allocation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Justification |
|-----------|-------|---------------|
| Task decomposition | Sonnet | Structured planning |
| Scoring assistance | Haiku | Simple evaluation |
| Handoff design | Sonnet | Protocol definition |
| Retrospective analysis | Opus | Deep quality review |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Task specification | Input for decomposition |
| Scoring rubric (5 criteria) | Objective allocation |
| Handoff templates | Structured transitions |
| Cortex | Logging |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Sub-tasks with documented allocation | 100% |
| Handoff protocols defined | 100% of transitions |
| Retrospective completed | Yes, within 1 day |
| Time saved vs all-human baseline | Documented |
| AI rework rate | < 15% |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Initial version |
| **2.0** | **2026-03-20** | Opus v2: 7 steps with Python code, scoring matrix, handoff protocols, model routing per sub-task, retrospective template, when to use/NOT use |
