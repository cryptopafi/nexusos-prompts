# Apply the Menu Actions Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configures the AI to present a structured menu of available actions at decision points, allowing the user to navigate complex interactions by selecting from pre-defined options rather than constructing new prompts from scratch.

---

## 1. Problema

In complex, multi-step AI interactions, users often face decision paralysis about what to ask next, or they issue inefficient follow-up prompts because they do not know what the AI can do at that stage. The Menu Actions Pattern solves this by making the AI responsible for surfacing relevant options at each step, turning an open-ended conversation into a navigable interaction flow.

Situations covered:
- Multi-step workflows where the next action depends on the current state
- Exploratory sessions where the user does not know all the options available
- UX prototyping or user-testing where you want to simulate a menu-driven interface
- Any scenario where the option space is large and users need guidance on what is possible

---

## 2. Procedura

### Pas 1: Define the Context and Available Actions
Before setting up the menu system, map out the domain: What are the possible actions the AI can take in this context? Group related actions into categories. For each action, define: its name, what it does, its required inputs, and its expected output. This action inventory becomes the basis for the menus.

### Pas 2: Instruct the AI to Present a Menu After Each Response
Tell the AI: "After each response, present a numbered menu of the 3 to 5 most relevant next actions the user can take. Format as: [1] Action name — brief description. Always include an option to [ask a custom question] or [go back to main menu]. Wait for user selection before proceeding."

### Pas 3: Execute Initial Request and Review the First Menu
Submit your first request and review the menu the AI appends. Verify: Are the options relevant to the current state? Are important options missing? Is the menu format clear and scannable? If the first menu is poorly calibrated, refine the action inventory from Pas 1 and re-prompt.

### Pas 4: Navigate the Interaction by Selecting Menu Items
Select a numbered option from the menu. The AI should execute the corresponding action and present a new contextually appropriate menu afterward. Maintain the discipline of always selecting from the menu rather than free-typing new prompts — this is what keeps the interaction structured.

### Pas 5: Handle Custom Actions When Needed
When the task requires an action not on the current menu, use the "custom question" option defined in Pas 2. After the AI executes the custom action, it should return to the menu pattern. If a custom action is needed more than twice, add it to the action inventory for future sessions.

### Pas 6: Extract and Document the Interaction Path
At the end of the session, ask the AI to summarize the sequence of menu selections and their outputs. This creates an audit trail of the decisions made and a reusable workflow path that can be scripted for future automation.

---

## 3. Cortex Logging

```json
{
  "text": "Menu Actions Pattern applied. Context/domain: [description]. Actions defined in inventory: [count]. Menu selections made during session: [count]. Custom actions triggered: [count]. Interaction path documented: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-054",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "menu_actions"],
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
- Meniu creat fără testarea tuturor opțiunilor disponibile → violation
- Acțiuni de meniu nedocumentate în output final → violation
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
1. `✅ [PROC] C-054 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Menu Actions Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
