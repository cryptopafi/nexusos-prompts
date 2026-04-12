# Apply the Question Refinement Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Uses the AI to surface ambiguities in an initial prompt by generating targeted clarifying questions, then rewrites the prompt with those answers incorporated to produce a significantly higher-quality final output.

---

## 1. Problema

Most first-draft prompts contain hidden assumptions, vague scope, or missing constraints that lead to mediocre outputs. The Question Refinement Pattern externalizes the prompt improvement process by making the AI identify exactly what information it lacks before answering.

Situations covered:
- Initial prompt is broad or underspecified
- Output from first attempt was off-target in unexpected ways
- Task has multiple valid interpretations and you need the right one
- Working in an unfamiliar domain where you don't know what context matters

---

## 2. Procedura

### Pas 1: Write and Submit the Initial Prompt
Write your prompt as clearly as you can right now — do not over-think it. Submit it with the instruction: "Before answering, identify the 3 to 5 most important ambiguities or missing details in this prompt that would significantly affect your answer. List them as questions."

### Pas 2: Review the Clarifying Questions
Read all questions the AI generates. Mark each question as: (A) I know the answer, (B) I didn't realize this mattered, or (C) the AI is asking about something I want it to decide. This categorization reveals blind spots in your original prompt.

### Pas 3: Answer the Category A and B Questions
Provide concrete, specific answers to every A and B question. For C questions, instruct the AI to make a reasonable default choice and state what it chose. Avoid vague answers — specificity here directly determines output quality.

### Pas 4: Request Prompt Rewrite
Ask the AI: "Now rewrite the original prompt incorporating all of these answers as explicit constraints and context. Show me the refined prompt before answering it." Reviewing the rewrite gives you one more chance to correct misinterpretations.

### Pas 5: Approve or Correct the Refined Prompt
Read the rewritten prompt critically. If the AI introduced incorrect assumptions or missed any of your answers, correct them now. Only proceed once the rewritten prompt accurately represents your intent.

### Pas 6: Generate Final Output from Refined Prompt
Instruct the AI to now answer using the refined prompt. Compare the output quality against what an answer to the original prompt would have produced. Note the specific improvements.

---

## 3. Cortex Logging

```json
{
  "text": "Question Refinement Pattern applied. Original prompt: [summary]. Clarifying questions generated: [count]. Key ambiguities resolved: [list main ones]. Output quality delta: [observed improvement].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-043",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "question_refinement"],
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
- Întrebări reformulate fără verificarea alinierii cu intenția originală → violation
- Rafinare aplicată fără iterații documentate → violation
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
1. `✅ [PROC] C-043 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Question Refinement Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
