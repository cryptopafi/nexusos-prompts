# Apply Step-Back Prompting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Abstract a complex or ambiguous problem to a higher-level principle before answering the specific question. Improves reasoning quality on scientific, analytical, and multi-step problems by forcing grounding in first principles.

---

## 1. Problema

When faced with a specific complex question, LLMs often jump to pattern-matched surface-level answers without engaging the underlying principles. Step-Back Prompting (Zheng et al., 2023) solves this by explicitly inserting an abstraction step: first identify the general concept or principle the question relates to, answer at that level, then apply the general answer to the specific case.

Situations covered:
- Scientific and technical questions requiring domain principles
- Root cause analysis where symptoms might mislead
- System design where specific implementation must follow architectural principles
- Ambiguous requests where the real need is different from the stated question

---

## 2. Procedura

### Pas 1: Identify Whether Step-Back Applies
Before applying: does this question involve a domain where general principles exist? If yes, proceed. If the question is concrete and well-scoped (e.g., "read this file", "what is the capital of France"), skip this procedure.

Indicators for applying Step-Back:
- Question contains domain-specific jargon that signals principles underlie the answer
- Direct answer would require knowing WHY, not just WHAT
- Question is about a failure/problem (root cause analysis)
- Multiple valid answers exist depending on underlying assumptions

### Pas 2: Formulate the Step-Back Question
Rephrase the specific question as a general/abstract version: "What is the [concept/principle/framework] that governs [the domain this question belongs to]?"

Examples:
- Specific: "Why is my SQL query running slowly?" → Step-back: "What are the principles of relational database query optimization?"
- Specific: "How should I structure this microservice?" → Step-back: "What are the architectural principles for service boundary design?"
- Specific: "This marketing campaign failed — why?" → Step-back: "What factors determine marketing campaign effectiveness in this channel/audience context?"

### Pas 3: Answer the Step-Back Question First
Prompt the model (or reason yourself): "Answer the abstract question: [Step-back question]. Be comprehensive about the underlying principles."

Document the answer at the abstract level before returning to the specific.

### Pas 4: Apply General Principles to the Specific Case
With the abstract answer in hand, return to the original question: "Given these principles: [abstract answer], now apply them specifically to: [original specific question]."

The specific answer should explicitly reference which principles it applies and how.

### Pas 5: Validate Grounding
Check: Does the specific answer trace back to the general principles? If the answer contradicts the abstract principles you identified, either the principles were wrong (revise Step 3) or the specific case is an exception (document why).

---

## 3. Cortex Logging

```json
{
  "text": "Step-Back Prompting applied. Domain: [domain]. Step-back question: [abstract question]. Principles identified: [count]. Answer quality improvement vs direct: [better/same/worse].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-062",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "step_back", "abstraction", "reasoning"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Phase 0 of PromptForge v2.2) + any complex analytical task

### WHEN
When prompt is complex, ambiguous, or involves domain principles

### HOW (violation detection)
- Step-back skipped on complex domain question → violation
- Abstract question formulated but not answered before specific → violation
- Specific answer doesn't reference abstract principles → violation

### VERIFY
- [ ] Step-back applicability checked?
- [ ] Abstract question formulated?
- [ ] Abstract answered before specific?
- [ ] Specific answer grounded in principles?

**VK-uri obligatorii:**
1. `✅ [PROC] C-062 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Step-Back Prompting" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| PromptForge v2.2 Phase 0 | Integration point |
| Domain knowledge | Required for abstract question formulation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Abstraction quality | Principles are genuinely general, not just rephrased specifics |
