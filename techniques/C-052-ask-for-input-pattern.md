# Apply the Ask-for-Input Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Instructs the AI to explicitly request specific input before taking any action, preventing premature or misdirected outputs by making all required inputs explicit before proceeding.

---

## 1. Problema

AI systems often make assumptions when input is missing and proceed anyway, generating outputs that must be discarded and redone. The Ask-for-Input Pattern prevents this by establishing a contract: the AI does not act until it has confirmed it has what it needs. This is especially valuable in automated or semi-automated workflows where silent wrong assumptions are costly.

Situations covered:
- Multi-step workflows where each step requires specific user input before proceeding
- Situations where the AI would otherwise make assumptions you cannot easily detect
- Interactive tools or agents where the user provides data progressively
- Any scenario where missing input produces incorrect rather than just incomplete output

---

## 2. Procedura

### Pas 1: Define What Inputs Are Required for the Task
Before writing the prompt, list every piece of information the AI genuinely needs to complete the task correctly. Distinguish between: (a) inputs that must come from you, (b) inputs the AI can reasonably infer or choose a default for, and (c) inputs that are truly optional with clear defaults. Only category (a) inputs should trigger a hard wait.

### Pas 2: Instruct the AI to Request Before Acting
Write the prompt with an explicit instruction: "Do not begin the task yet. First, ask me for the following inputs one at a time: [list required inputs]. Wait for my answer after each question. Only begin the task once you have received all required inputs." The "one at a time" instruction prevents an overwhelming multi-question dump.

### Pas 3: Respond to Each Input Request
As the AI asks for each input, provide specific, actionable answers. If an input is truly not available, say so and specify whether you want the AI to skip it, apply a default, or block on it. Do not give vague answers that will lead the AI to make assumptions anyway.

### Pas 4: Verify the AI Has Collected All Required Inputs
Before it begins the task, ask the AI to confirm: "List all the inputs you have received and what you will use for any remaining optional fields." This creates a shared understanding of the input state before any work is done.

### Pas 5: Trigger Execution
Once inputs are confirmed, explicitly say "Proceed" or "Begin the task now." This clean separation between input collection and execution makes it easy to restart from the input confirmation step if something needs to change.

### Pas 6: Evaluate Input Completeness After Task Completion
After the output is generated, assess whether any additional input would have improved it. If yes, identify which inputs you should have included in the required list from Pas 1. Update your prompt template for future use.

---

## 3. Cortex Logging

```json
{
  "text": "Ask-for-Input Pattern applied. Task type: [description]. Inputs required: [count and list]. Inputs collected: [count]. AI assumption attempts blocked: [count]. Output quality with vs. without pattern: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-052",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "ask_for_input"],
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
- Pattern aplicat fără specificarea formatului exact al input-ului așteptat → violation
- Input colectat fără validare înainte de procesare → violation
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
1. `✅ [PROC] C-052 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Ask-for-Input Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
