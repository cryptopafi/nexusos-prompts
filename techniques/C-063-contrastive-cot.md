# Apply Contrastive Chain-of-Thought — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Enhance few-shot examples by pairing each correct reasoning chain with an explicit incorrect reasoning chain showing the same mistake pattern the model is likely to make. Teaching by contrast improves accuracy over positive-only few-shot.

---

## 1. Problema

Standard few-shot with CoT shows the model correct reasoning, but doesn't explicitly rule out the wrong reasoning paths the model would otherwise default to. Contrastive CoT (Chia et al., 2023) adds an "incorrect example" that demonstrates the wrong reasoning, then labels it as wrong and shows why. This forces the model to recognize and avoid the most common error patterns.

Situations covered:
- Classification tasks where the model consistently makes a specific type of error
- Math/logic problems where one wrong shortcut is tempting
- Analysis tasks where surface-level pattern matching leads to wrong conclusions
- Any task where you can predict the model's likely failure mode in advance

---

## 2. Procedura

### Pas 1: Complete Standard Few-Shot CoT First
Follow C-058 (Few-Shot + CoT) to create 2-3 correct examples with reasoning traces. These become the positive examples.

### Pas 2: Identify the Most Likely Wrong Reasoning
For each task type, ask: what is the single most common incorrect reasoning path? This might be:
- Ignoring a key condition
- Reversing a relationship
- Applying a heuristic that works most of the time but fails in this class of cases
- Stopping reasoning one step too early

Document the specific error pattern you will contrast against.

### Pas 3: Create Contrastive Example Pairs
For 1-2 of your examples, add a "wrong" version immediately after (or before) the correct version:

```
WRONG EXAMPLE:
Input: [same input]
Reasoning: [incorrect reasoning path — shows the common mistake]
Output: [wrong output]
Why this is wrong: [explicit explanation of the error]

CORRECT EXAMPLE:
Input: [same input]
Reasoning: [correct reasoning path]
Output: [correct output]
```

### Pas 4: Order Contrastive Examples Strategically
Place contrastive (wrong+correct) pairs BEFORE your plain-correct examples. This primes the model to recognize the wrong pattern first, then see how to avoid it.

### Pas 5: Include Explicit Label — "Now do it correctly"
After the contrastive pairs, add: "For all the following inputs, apply only the correct reasoning pattern. Avoid the errors shown in the WRONG examples above."

### Pas 6: Evaluate — Did Contrast Help?
Compare results with and without the contrastive examples on a held-out set. If accuracy improved specifically on the error type you targeted, the contrastive pair was effective. If not, revise the "Why this is wrong" explanation.

---

## 3. Cortex Logging

```json
{
  "text": "Contrastive CoT applied. Task type: [description]. Error pattern targeted: [description]. Contrastive pairs created: [count]. Improvement vs standard few-shot: [yes/no/not tested].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-063",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "contrastive_cot", "few_shot", "reasoning"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.2 Phase 2 Extended Techniques — use when few-shot quality is critical

### WHEN
When standard few-shot CoT is insufficient or when a specific error pattern has been identified

### HOW (violation detection)
- Contrastive pair created without explicit "Why this is wrong" → violation
- Wrong example not labeled as wrong → violation (could teach wrong pattern)
- Contrastive CoT applied without first having correct examples (C-058) → violation

### VERIFY
- [ ] Correct examples (C-058) complete before adding contrast?
- [ ] Error pattern explicitly identified before creating wrong example?
- [ ] Each wrong example has explicit "Why this is wrong" label?
- [ ] Wrong examples cannot be mistaken for correct ones?

**VK-uri obligatorii:**
1. `✅ [PROC] C-063 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Contrastive Chain-of-Thought" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| C-058 (Few-Shot + CoT) | Prerequisite — correct examples first |
| Error pattern knowledge | Required to create meaningful wrong examples |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Error pattern accuracy | Wrong example actually shows the real failure mode |
