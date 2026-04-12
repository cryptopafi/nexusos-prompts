# Apply the Flipped Interaction Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Inverts the standard prompting dynamic so the AI asks you targeted questions to gather all necessary information before generating output, replacing a poorly-specified one-shot prompt with a structured interview.

---

## 1. Problema

When you lack clarity about what information an AI needs to produce a good output, trying to guess what to include in a prompt leads to incomplete or misguided results. The Flipped Interaction Pattern delegates the information-gathering responsibility to the AI, which asks exactly what it needs to know.

Situations covered:
- You know the desired output type but are unsure what context matters
- Complex deliverables (project plans, legal summaries, strategy docs) where missing context causes failures
- Onboarding scenarios where users need to be interviewed before receiving recommendations
- Any task where the AI knows what parameters drive quality better than the user does at that moment

---

## 2. Procedura

### Pas 1: Define the Goal and Activate Interview Mode
Tell the AI the type of output you want and activate the flipped mode explicitly: "I want you to help me produce [desired output]. Do not generate the output yet. Instead, interview me — ask me the questions you need answered to produce the best possible [output]. Ask one question at a time and wait for my answer before asking the next."

### Pas 2: Respond to Each Question Concretely
Answer each AI question with specific, concrete information. Avoid vague answers. If you don't know the answer to a question, say so — that itself is valuable context the AI needs. Do not try to redirect the AI mid-interview; stay in the interview frame.

### Pas 3: Signal Interview Completion
After several exchanges, the AI may ask if there is anything else or may naturally conclude the interview. If it does not conclude, you can signal: "I think you have enough information now. Please summarize what you've learned about my situation before generating the output."

### Pas 4: Review the AI's Summary of Your Inputs
The AI's summary of what it heard is a critical quality gate. Verify it has captured your inputs accurately. If any key point is missing or distorted, correct it before the AI generates the final output. This prevents misunderstandings from propagating into the deliverable.

### Pas 5: Approve and Trigger Output Generation
Once the summary is accurate, say: "Your summary is correct. Now generate [output type] based on everything I've told you." The output should be notably richer and better-targeted than a first-pass response to a vague prompt would have been.

### Pas 6: Evaluate Interview Efficiency
After the exercise, reflect: Were any questions redundant? Were any critical questions missing? Use this evaluation to pre-load better context in future prompts for similar tasks, reducing the interview rounds needed.

---

## 3. Cortex Logging

```json
{
  "text": "Flipped Interaction Pattern applied. Goal: [output type]. Questions asked by AI: [count]. Key information surfaces: [list main insights AI extracted]. Output quality vs. cold prompt: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-046",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "flipped_interaction"],
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
- Pattern aplicat fără clarificarea domeniului de întrebări permis modelului → violation
- Sesiune terminată înainte ca modelul să colecteze informațiile necesare → violation
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
1. `✅ [PROC] C-046 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Flipped Interaction Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
