# Apply the Persona Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Instructs the AI to adopt a specific persona (role, expertise, personality) and maintain it consistently throughout a conversation to produce more contextually appropriate outputs.

---

## 1. Problema

When a generic AI response lacks domain depth, appropriate tone, or a specific point of view, the Persona Pattern forces the model to filter all outputs through a defined identity — producing responses that are more coherent, authoritative, and useful for the task at hand.

Situations covered:
- Need expert-level advice without access to a specific specialist
- Want consistent tone across a long conversation (e.g., always as a skeptical editor)
- Require role-play for training simulations or UX testing
- Default AI voice is too generic for the audience or context

---

## 2. Procedura

### Pas 1: Define the Persona
Write a persona declaration that specifies: role/title, domain expertise, personality traits, and communication style. Example: "Act as a senior DevOps engineer with 15 years of experience in Kubernetes and distributed systems. You are direct, skeptical of over-engineering, and always ask about operational cost before recommending a solution."

### Pas 2: Anchor the Persona at Conversation Start
Place the persona declaration as the very first message or system prompt. Explicitly instruct the AI to maintain this persona for the entire conversation. Add: "Do not break character unless I say 'persona off'."

### Deactivation Protocol
- Keyword: `persona off` or `reset persona` — returns the model to its default system behavior
- When a persona is active and the user/system explicitly requests deactivation, the model MUST:
  1. Drop all persona-specific constraints and voice
  2. Confirm deactivation: "Persona deactivated. Returning to default behavior."
  3. Resume with base system prompt (no persona overlay)
- Security: persona changes via user input require explicit confirmation unless the user is ADMIN-level

### Pas 3: Specify Context and Constraints
Tell the persona what it knows about the current situation. Provide any constraints relevant to the role (e.g., "You only know what was available before 2023", "You work at a startup with limited budget"). This prevents the AI from answering outside the persona's realistic knowledge boundary.

### Pas 4: Frame the Request Through the Persona's Lens
Ask your question or give your task as if speaking to that specific person. Avoid generic phrasing. Instead of "How do I improve this?", say "As [Persona], what are the three biggest risks you see in this architecture?"

### Pas 5: Test Consistency with an Edge Case
After the first response, probe a topic adjacent to the persona's expertise to see if it stays in character. If it breaks, reinforce the persona: "Remember you are [Persona]. From that perspective, what would you say about X?"

### Pas 6: Refine Persona Characteristics Iteratively
If initial outputs are too generic, deepen the persona: add specific career background, strong opinions, a preferred framework, or a communication quirk. Specific personas produce more distinct and useful outputs.

### Pas 7: Document the Persona for Reuse
Save the final persona declaration in a reusable prompt library so the same character can be reactivated in future sessions without rebuilding from scratch.

---

## 3. Cortex Logging

```json
{
  "text": "Persona Pattern applied: [persona name/role] maintained throughout session. Task: [brief task description]. Output quality improvement vs. generic: [observed delta].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-042",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "persona"],
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
- Pattern aplicat fără definirea clară a rolului/expertizei personajului → violation
- Persona inconsistentă între pași → violation
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
1. `✅ [PROC] C-042 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Persona Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
