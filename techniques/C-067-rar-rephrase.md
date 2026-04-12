# RAR — Rephrase and Respond — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: When a prompt is vague, ambiguous, or underspecified, instruct the model to rephrase the question in its own words before answering. The rephrase step forces explicit interpretation, catches misunderstandings early, and improves answer accuracy on ambiguous queries.

---

## 1. Problema

Ambiguous prompts lead to misaligned answers — the model answers a different question than intended, or picks one interpretation without flagging alternatives. RAR (Rephrase and Respond, Deng et al., 2023) inserts a mandatory interpretation step: the model must restate what it understands the question to be asking before answering. This creates an explicit checkpoint where misalignment is visible and correctable before the full answer is generated.

Situations covered:
- Vague requests ("make this better", "analyze this", "what should I do?")
- Questions with multiple valid interpretations
- Complex compound questions where different interpretations change the answer significantly
- Cross-domain questions where terminology has different meanings

---

## 2. Procedura

### Pas 1: Identify Ambiguity Before Sending
Before sending a prompt, check: could this be interpreted in more than one reasonable way? If yes, apply RAR. If the prompt is precise and unambiguous, skip RAR.

Ambiguity signals:
- Pronouns without clear referents ("fix it", "update this")
- Vague verbs ("improve", "analyze", "review")
- Missing scope ("optimize the code" — which part? by what metric?)
- Context-dependent terms ("make it faster" — faster to write? faster to run?)

### Pas 2: Add RAR Instruction to Prompt
Append to the end of any ambiguous prompt:

```
Before answering, rephrase the question in your own words to show your interpretation. If multiple interpretations are valid, list them and state which one you'll answer. Format:

INTERPRETATION: [Your rephrasing of what you understand is being asked]
ANSWER: [Your response to your interpretation]
```

### Pas 3: Review the Rephrase Before the Answer
When the model responds, READ the INTERPRETATION section before the ANSWER. If the interpretation is wrong or not what you intended:
- Stop: do not read the answer (it answers the wrong question)
- Correct: "Your interpretation is incorrect. What I actually meant was: [clarification]. Please re-answer based on this."

### Pas 4: Confirm Correct Interpretation
If the interpretation is correct, proceed to read the answer. The rephrase step is now complete.

### Pas 5: For Compound Questions — Multiple Interpretations
If the model lists multiple interpretations, explicitly select one before it answers:
"Interpretation #2 is correct. Answer based on that interpretation only."

This prevents partial answers that try to satisfy all interpretations and satisfy none well.

---

## 3. Cortex Logging

```json
{
  "text": "RAR applied. Ambiguity type: [vague verb/pronoun/scope/terminology]. Interpretation correct on first try: [yes/no]. Corrections needed: [N]. Final answer quality improved: [yes/no/not measurable].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-067",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "rar", "disambiguation", "interpretation"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.2 Phase 2 Extended Techniques — apply to ambiguous prompts before structuring

### WHEN
Any prompt with detected ambiguity signals (see Pas 1)

### HOW (violation detection)
- RAR skipped on clearly ambiguous prompt → soft violation
- Interpretation section read but not verified before reading answer → violation
- Wrong interpretation not corrected before answering → violation

### VERIFY
- [ ] Ambiguity detected before applying RAR?
- [ ] RAR instruction added to prompt?
- [ ] Interpretation verified before reading answer?
- [ ] Corrections applied if interpretation was wrong?

**VK-uri obligatorii:**
1. `✅ [PROC] C-067 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "RAR — Rephrase and Respond" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| PromptForge v2.2 Phase 1 SCOPE | Complementary — SCOPE prevents ambiguity, RAR handles residual ambiguity |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Interpretation accuracy (first try) | >80% |
| Corrections needed per session | <1 on average |
| Ambiguity detection rate | Catch ambiguous prompts before they're sent |
