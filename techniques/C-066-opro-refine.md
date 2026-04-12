# OPRO — Automatic Prompt Refinement — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Use the LLM itself to generate, score, and iteratively improve prompt variants rather than manually guessing improvements. OPRO (Optimization by PROmpting, Yang et al., 2023) treats prompt optimization as a meta-optimization problem where the LLM is both optimizer and evaluatee.

---

## 1. Problema

Manual prompt iteration is slow and biased by human assumptions about what makes a good prompt. OPRO delegates the search process to the LLM itself: given a task description and a scoring function, the model generates candidate prompts, evaluates them, and generates improved candidates based on which ones scored higher. This is the formalized version of PromptForge Phase 5 Meta-Prompting.

Situations covered:
- Prompts that will be used repeatedly (production prompts, FORGE procedures)
- Tasks where you have a clear quality metric to optimize against
- Prompt engineering for specialized domains where human intuition is limited
- Situations where Phase 5 Meta-Prompting hit its 2-iteration limit but quality is still suboptimal

---

## 2. Procedura

### Pas 1: Define the Task and Quality Metric Precisely
Before running OPRO, define:
- Task: What should the optimal prompt make the LLM do?
- Quality metric: How will you score outputs? (0-100 scale, or pass/fail on specific criteria)
- Examples: 3-5 input/output pairs that represent desired behavior

The quality metric must be checkable without human judgment at each iteration, or be explicitly human-in-the-loop.

### Pas 2: Create the Meta-Prompt (Optimizer Prompt)
The optimizer prompt tells the LLM to generate prompt candidates:

```
You are a prompt optimizer. Your task is to generate improved versions of a prompt for the following task:

TASK: [task description]

QUALITY CRITERIA: [your quality metric — what makes a good output]

EXAMPLES OF DESIRED BEHAVIOR:
Input: [example 1 input] → Good output: [example 1 output]
Input: [example 2 input] → Good output: [example 2 output]

PREVIOUS PROMPT ATTEMPTS AND SCORES:
[Empty on first iteration]

Generate 3 candidate prompts that you predict will score highly on the quality criteria. For each, explain your reasoning for why this prompt should perform better. Format: CANDIDATE 1: [prompt] | REASONING: [why better]
```

### Pas 3: Score Each Candidate
Run each candidate prompt on your test examples. Score using your defined metric. Record: `CANDIDATE N | score: X/100 | strengths: [...] | weaknesses: [...]`

### Pas 4: Feed Scores Back to Optimizer
Update the meta-prompt with scores:
```
PREVIOUS ATTEMPTS:
- Prompt: [candidate 1] | Score: X | Weakness: [what failed]
- Prompt: [candidate 2] | Score: Y | Weakness: [what failed]
- Prompt: [candidate 3] | Score: Z | Weakness: [what failed]

Given these results, generate 3 new candidates that address the identified weaknesses.
```

### Pas 5: Iterate — Max 3 Rounds
Run Steps 3-4 for maximum 3 rounds (9 candidates total). Stop early if a candidate reaches your target score. After 3 rounds, select the highest-scoring candidate.

### Pas 6: Final Validation
Test the winning prompt on a held-out set (different from the examples used during optimization). If performance degrades, you may have overfitted to your examples — run one more diversity-focused iteration.

---

## 3. Cortex Logging

```json
{
  "text": "OPRO Auto-Refinement applied. Task: [description]. Quality metric: [description]. Rounds: [N]. Candidates generated: [N]. Best score: [X/100]. Winning prompt saved to Cortex: [yes/no].",
  "collection": "technical",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-066",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "opro", "auto_optimization", "meta_prompting"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.2 Phase 5 escalation path — when Phase 5 (max 2 iterations) fails to reach score 70+

### WHEN
Production prompts, FORGE procedures, recurring high-value prompts

### HOW (violation detection)
- OPRO run without defined quality metric → violation (cannot score candidates)
- More than 3 rounds run without reaching target → stop and escalate
- Winning prompt not validated on held-out set → soft violation

### VERIFY
- [ ] Task and quality metric defined before starting?
- [ ] Meta-prompt includes previous scores in each round?
- [ ] Maximum 3 rounds enforced?
- [ ] Winning prompt validated on held-out set?
- [ ] Winning prompt saved to Cortex?

**VK-uri obligatorii:**
1. `✅ [PROC] C-066 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "OPRO Auto-Refinement" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Meta-prompt (optimizer) | Opus (for best candidate generation) |
| Candidate evaluation | Sonnet |
| Final validation | Sonnet |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| PromptForge Phase 5 | Prerequisite — OPRO is Phase 5 escalation |
| Cortex | Store winning prompt for reuse |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Candidates per session | 9 (3 rounds × 3 candidates) max |
| Target score achieved | ≥70 within 3 rounds |
| Held-out validation pass | Yes |
