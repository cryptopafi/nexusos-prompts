# Apply the ReAct Prompting Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implements the Reasoning + Acting (ReAct) loop for complex multi-step tasks by interleaving explicit thought steps with actions that gather information or transform state, producing traceable and verifiable reasoning chains grounded in real observations.

---

## 1. Problema

Complex tasks require both reasoning about what to do next AND taking actions to gather information or change state. Standard prompting conflates these into a single response, producing conclusions that are not grounded in verifiable observations. ReAct forces explicit alternation between Thought (reasoning), Action (what to do next), and Observation (what was learned), creating an auditable and correctable process.

Situations covered:
- Multi-step research or information retrieval tasks
- Complex decision-making where each step changes the context for the next
- Agent-like workflows where the AI must plan, act, and update its plan based on results
- Tasks where you need to audit how the AI reached a conclusion and verify each step

> **Safety Note**: Pattern-ul ReAct permite modelului să execute acțiuni iterative (Reason → Act → Observe). În sisteme cu acces la tool-uri externe sau APIs, validați fiecare pas Act înainte de execuție. Un loop ReAct nesupravegheat poate genera lanțuri de acțiuni cu consecințe neintenționate.

---

## 2. Procedura

### Pas 1: Define the Task and Available Actions
Specify the task clearly and enumerate the actions the AI is allowed to take in this session. In a simulated ReAct scenario (without real tool access), define what each action means and how you will simulate providing observations. Example actions: `Search[query]`, `Lookup[term]`, `Calculate[expression]`, `Summarize[text]`.

**Model target check**: Dacă modelul țintă are reasoning built-in (Opus extended thinking, o1/o3): omite blocul explicit `<thought>` din prompt — reasoning-ul nativ îl acoperă. Include doar `<action>` și `<observation>`.

### Pas 2: Initialize the ReAct Loop
Prompt the AI: "Use the ReAct format to complete this task: [task description]. You must strictly follow this format for every step: Thought: [your reasoning about what to do next and why] Action: [one action from: list of available actions] Observation: [I will provide this — do not generate it yourself] Do not skip to a final answer until you have gathered sufficient observations."

Dacă nu găsești suficiente dovezi după [5] iterații, declară explicit: "Dovezi insuficiente — iată ce am găsit și ce rămâne necunoscut." Nu genera un răspuns confident din observații incomplete.

> **Notă XML**: Pentru Claude ca model țintă, folosește `<thought>`, `<action>`, `<observation>` în loc de text simplu — îmbunătățește aderența la structură.

### Pas 3: Provide Observations for Each Action
After each Action step, you (or your external system) provide the Observation. In a training scenario, simulate realistic observations based on the action taken. In a live system, this is where real tool outputs feed back into the loop. The Observation must be factual and specific — not a summary the AI generated itself.

### Pas 4: Monitor the Reasoning Chain for Quality
Read each Thought step critically: Is the reasoning actually informed by the previous Observation? Is the Action a logical next step given the Thought? If the AI's Thought ignores a prior Observation or jumps to a conclusion without sufficient evidence, interrupt the loop: "Your Thought at step [N] ignored Observation [N-1]. Please re-reason from that observation."

### Pas 5: Trigger Final Answer Only When Sufficient Observations Exist
The AI should reach a final answer only after a minimum number of Observation-grounded reasoning steps. If it attempts to conclude early (after 1-2 steps), instruct: "You do not yet have sufficient observations to answer confidently. Continue the ReAct loop." Define "sufficient" as: the final answer is directly supported by at least 2 independent observations.

**Stop criterion**: Oprește loop-ul ReAct la maximum 5 iterații, indiferent de suficiența observațiilor. Rezumă ce s-a găsit.

### Pas 6: Verify the Final Answer Traces to Observations
After the final answer is given, audit the trace: For each claim in the final answer, identify which specific Observation it is grounded in. If any claim in the final answer cannot be traced to an observation, flag it as unsupported and either remove it or require an additional observation to support it.

---

## 3. Cortex Logging

```json
{
  "text": "ReAct Pattern applied. Task: [summary]. ReAct loop iterations: [count]. Actions used: [list]. Reasoning chain quality: [grounded/partially grounded/ungrounded]. Final answer fully traceable to observations: [yes/partially/no]. Issues found: [list].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-059",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "react"],
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
- Loop ReAct rulat fără limită de iterații definită → violation critică
- Pas Act executat în producție fără validare umană pentru acțiuni ireversibile → violation critică
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
1. `✅ [PROC] C-059 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the ReAct Prompting Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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

---

## Changelog

| Versiune | Data | Modificare |
|----------|------|------------|
| 1.0 | 2026-03-04 | Versiune inițială |
| 1.1 | 2026-03-07 | Fix: model detection (Pas 1), stop criterion (Pas 5), fallback declaration + notă XML (Pas 2) |
