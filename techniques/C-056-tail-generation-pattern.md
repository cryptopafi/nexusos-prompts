# Apply the Tail Generation Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Instructs the AI to append relevant follow-up prompts or questions at the end of each response, enabling guided exploration of a topic by surfacing what productive next steps look like from the AI's perspective.

---

## 1. Problema

After receiving an AI response, users often do not know what the most valuable follow-up questions would be — they accept the output as final when deeper or broader exploration was possible. The Tail Generation Pattern addresses this by making the AI responsible for suggesting the next high-value directions, transforming single-turn exchanges into productive dialogues.

Situations covered:
- Research and exploration sessions where you want to go deep on a topic
- Learning scenarios where the AI should guide progression through a subject
- Complex problem-solving where each answer opens new directions worth exploring
- Any session where the natural follow-up to an answer is not obvious to the user

---

## 2. Procedura

### Pas 1: Activate Tail Generation in the Session
At the start of the session, instruct the AI: "After each response, append a section titled 'Where to go next' with 3 suggested follow-up prompts. Format them as ready-to-use prompts, not just topics. Number them. Choose suggestions that represent the most valuable directions from the current response, covering different dimensions (deeper detail, broader context, practical application)."

### Pas 2: Submit Initial Request and Review the Tail
After receiving the first response, read the tail prompts critically. Evaluate: Are these the most valuable next steps given your actual goals? Or are they obvious/superficial follow-ups? If the tail suggestions are weak, tell the AI: "Your tail suggestions were too surface-level. Regenerate them focusing on [specific dimension: edge cases / practical application / contrarian perspectives / etc.]."

### Pas 3: Select and Execute a Tail Prompt
Choose the tail prompt most aligned with your goals. You can use it as-is (copy and paste the numbered suggestion as your next message) or modify it. The act of the AI writing the prompt for you often surfaces questions you would not have thought to ask.

### Pas 4: Maintain Thematic Coherence Across Turns
As the session progresses through multiple tail-driven turns, periodically check that the exploration has direction. If the chain of tail prompts has drifted from your original goal, explicitly redirect: "Return to the topic of [original goal]. Given what we've covered, what are the 3 most important remaining questions?"

### Pas 5: Collect and Evaluate the Exploration Path
At the end of the session, ask the AI to list all the prompts and topics covered in the session in order. Review this map of the exploration: What was covered? What gaps remain? What was most valuable? This evaluation informs how to structure future explorations of the same topic.

### Pas 6: Save High-Value Tail Prompts as Reusable Templates
Identify any tail prompts that proved particularly productive — they surfaced unexpected insights, generated high-quality outputs, or revealed important nuances. Save these as reusable prompt templates for future sessions on the same subject.

---

## 3. Cortex Logging

```json
{
  "text": "Tail Generation Pattern applied. Topic: [description]. Session turns: [count]. Tail prompts generated total: [count]. Tail prompts used: [count]. Prompts saved as reusable: [count]. Exploration coverage quality: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-056",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "tail_generation"],
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
- Pattern aplicat fără definirea criteriilor de continuare/oprire → violation
- Generare continuată dincolo de scopul documentat → violation
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
1. `✅ [PROC] C-056 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Tail Generation Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
