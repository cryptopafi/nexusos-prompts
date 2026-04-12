# Apply the Cognitive Verifier Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Decomposes complex questions into targeted sub-questions that are answered individually before synthesizing a verified, comprehensive final answer.

---

## 1. Problema

Complex questions answered in one pass often skip sub-components, make hidden assumptions, or produce shallow reasoning. The Cognitive Verifier Pattern forces the AI to map the full problem space before answering, resulting in more complete and defensible outputs.

Situations covered:
- Multi-part questions that normally get partial answers
- Decisions with multiple interacting factors
- Research questions where missing one sub-topic makes the answer wrong
- Any scenario where the AI's first-pass answer feels incomplete or rushed

---

## 2. Procedura

### Pas 1: Pose the Complex Question
Submit the question to the AI with the explicit instruction: "Before answering, decompose this question into 3 to 5 sub-questions that must each be answered to construct a complete and accurate final answer. List the sub-questions first."

### Pas 2: Review the Sub-Question Decomposition
Evaluate whether the AI has identified the right sub-questions. Check for: missing critical sub-topics, overly broad sub-questions that need further splitting, and redundant questions that can be merged. Edit the list if needed before proceeding.

### Pas 3: Answer Each Sub-Question Separately
Instruct the AI: "Now answer each sub-question individually, clearly labeling each answer. Do not attempt the final synthesis yet." This forces dedicated reasoning for each component rather than blending everything together prematurely.

### Pas 4: Synthesize the Final Answer
Ask the AI: "Using only the answers you just provided to the sub-questions, synthesize a final comprehensive answer to the original question. Explicitly reference which sub-answers support each claim in the synthesis."

### Pas 5: Verify Completeness
Check the synthesis against the sub-answers: every sub-question's answer should be represented in the final synthesis. If a sub-answer is missing from the synthesis, ask the AI to integrate it explicitly.

### Pas 6: Identify Remaining Uncertainty
Ask: "Which of the sub-questions did you answer with the least confidence, and what additional information would improve those answers?" This surfaces where the output should be treated with caution or requires external verification.

---

## 3. Cortex Logging

```json
{
  "text": "Cognitive Verifier Pattern applied. Question: [summary]. Sub-questions generated: [count and key ones]. Synthesis quality: [complete/partial]. Uncertainty flags: [any identified].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-044",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "cognitive_verifier"],
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
- Criterii de verificare nedefinite înainte de aplicare → violation
- Verificare aplicată fără compararea cu output-ul necorectat → violation
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
1. `✅ [PROC] C-044 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Cognitive Verifier Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
