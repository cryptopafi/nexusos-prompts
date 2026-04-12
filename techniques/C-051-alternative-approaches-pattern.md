# Apply the Alternative Approaches Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Systematically requests multiple distinct solutions to a problem, with explicit diversity requirements, enabling informed selection between genuinely different approaches rather than variations of the same approach.

---

## 1. Problema

When you ask an AI for a solution to a problem, you typically receive the most common or default approach — often without knowing that fundamentally different solutions exist. The Alternative Approaches Pattern forces the AI to surface diverse options across different dimensions (tools, methods, frameworks, trade-offs), enabling better decision-making.

Situations covered:
- Technical decisions where multiple architectures or tools could work
- Strategic choices where different frameworks lead to different conclusions
- Creative tasks where you want approaches that differ in method, not just detail
- Any situation where knowing the option space matters before committing to one path

---

## 2. Procedura

### Pas 1: Frame the Problem Without Biasing Toward a Solution
Describe the problem, goal, and constraints without mentioning the approach you are already considering. If you mention a preferred solution in the prompt, the AI will anchor on it and generate variations rather than true alternatives. Save your preference for the evaluation phase.

### Pas 2: Request N Approaches with Explicit Diversity Requirements
Prompt: "Generate [3-5] meaningfully different approaches to this problem. Each approach must differ at the method or tool level — not just in implementation detail. For each approach, specify: the core mechanism, the tools or methods it uses, the primary trade-off, and the best-fit scenario where it would be the right choice."

### Pas 3: Check for True Diversity
Review the approaches the AI generates. If two approaches are essentially the same mechanism with surface-level differences, challenge the AI: "Approaches 2 and 3 use the same underlying method. Replace one with an approach that uses a fundamentally different mechanism." True alternatives should differ in at least one core dimension: tool, framework, process, or principle.

### Pas 4: Define Evaluation Criteria for Your Context
Before evaluating the options, explicitly list your selection criteria and their relative weights: e.g., speed of implementation (30%), maintainability (30%), cost (20%), team familiarity (20%). Defining criteria before seeing the approaches prevents rationalization bias toward the most familiar option.

### Pas 5: Score Each Approach Against Your Criteria
Apply your weighted criteria to each approach systematically. Ask the AI to assist: "Score each of the [N] approaches on a 1-5 scale for each of these criteria: [list]. Then calculate weighted totals and recommend the approach that best fits these priorities." Verify the reasoning behind each score.

### Pas 6: Document the Decision Including Rejected Options
Record which approach was selected, why, and what the key trade-offs of the rejected options were. This creates a decision record that is valuable for future review and for explaining the choice to others.

---

## 3. Cortex Logging

```json
{
  "text": "Alternative Approaches Pattern applied. Problem: [summary]. Approaches generated: [count]. Diversity verified: [yes/needed revision]. Evaluation criteria: [list]. Selected approach: [which one and why]. Rejected options documented: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-051",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "alternative_approaches"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Pattern aplicat fără documentare → violation
- Alternative generate fără criterii de evaluare pre-definite → violation
- Mai puțin de 3 alternative generate → violation
- Runner: Opus audit periodic + FORGE-AUDIT batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum AI/Prompt Engineering

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pattern corect aplicat în minim un exemplu real?

**VK-uri obligatorii:**
1. `✅ [PROC] C-051 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Alternative Approaches Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | FORGE-AUDIT |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| LLM access (Claude/ChatGPT) | Pattern application and testing |
| Prompt testing environment | Iteration and validation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Pattern applications | Minim 2 exemple per execuție |
