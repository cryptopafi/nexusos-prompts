# Apply Few-Shot Examples with Chain-of-Thought — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Combines multiple worked examples (few-shot) with explicit step-by-step reasoning traces (chain-of-thought) in a single prompt to teach the AI both the correct output format and the reasoning process required to produce it.

---

## 1. Problema

For tasks requiring non-trivial reasoning or specialized output formats, zero-shot prompts produce inconsistent results because the AI has to guess both the desired reasoning process and the output format. Few-Shot alone shows format but not reasoning. Chain-of-Thought alone guides reasoning but not format. Combined, they produce reliable, high-quality outputs on tasks that would otherwise require extensive iteration.

Situations covered:
- Multi-step reasoning tasks (math, logic, categorization, analysis)
- Specialized output formats that need both structure and reasoning to generate correctly
- Tasks where you need the AI to apply a specific analytical framework consistently
- Classification or labeling tasks where the reasoning behind a label matters as much as the label

---

## 2. Procedura

### Pas 1: Identify and Characterize the Target Task
Define the task precisely: What is the input? What is the correct output format? What reasoning steps are required to transform input into output? Write this characterization out — it is the foundation for designing your examples. If you cannot describe the reasoning steps yourself, you cannot teach them to the AI.

### Pas 2: Create 2-3 High-Quality Worked Examples
For each example, write: (a) the input, (b) an explicit reasoning trace showing each step of the thought process, and (c) the final output derived from that reasoning. The reasoning trace should be labeled "Reasoning:" and written in natural language, showing the actual thought process — not just restating the answer.

Example structure:
```
Input: [specific input]
Reasoning: First I identify X because [reason]. Then I check Y, which shows [observation]. Based on X and Y together, the conclusion is [intermediate result]. Therefore the output is [final result].
Output: [formatted final output]
```

### Pas 3: Sequence Examples from Simple to Complex
Order your examples from simplest to most complex. The first example should demonstrate the basic pattern clearly. The second should introduce one complicating factor. The third (if used) should handle an edge case or exception. This progression teaches both the base pattern and its variations.

### Pas 4: Construct the Full Prompt
Assemble the prompt: "I will show you [N] examples of [task] with step-by-step reasoning. After the examples, I will give you a new input. Apply the same reasoning process and output format. [Example 1] [Example 2] [Example 3] Now apply the same process to this input: [new input]."

### Pas 5: Evaluate Whether the AI Followed the Reasoning Pattern
After receiving the output, read the reasoning trace: Did the AI apply the same reasoning steps you demonstrated? Did it skip any steps? Did it introduce reasoning steps not in your examples? If the reasoning diverged, identify which example failed to demonstrate that case and add or revise an example.

### Pas 6: Optimize Example Count and Quality
Test: Does adding a third example improve results, or does two work just as well? In some cases, one very precise example outperforms three mediocre ones. Find the minimum number of high-quality examples that produce consistent results. Document the final optimized example set for reuse.

---

## 3. Cortex Logging

```json
{
  "text": "Few-Shot + Chain-of-Thought applied. Task type: [description]. Examples created: [count]. Reasoning steps per example: [average count]. AI reasoning pattern adherence: [followed/partial/diverged]. Example set optimized: [yes/no]. Final example count: [N].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-058",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "few_shot_cot"],
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
- Exemple few-shot conțin bias neidentificat → violation
- Chain-of-thought aplicat fără verificarea că pașii de raționament sunt corecți → violation
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
1. `✅ [PROC] C-058 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Few-Shot Examples with Chain-of-Thought" | FORGE ✓ | rule: META-H-002 | v1.0`

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
