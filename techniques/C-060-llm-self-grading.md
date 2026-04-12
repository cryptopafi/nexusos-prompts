# Use LLM Self-Grading for Quality Assurance — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Uses the AI to evaluate the quality of its own outputs against defined criteria using a structured scoring rubric, creating an automated quality assurance loop that catches low-quality outputs before they are used or distributed.

---

## 1. Problema

Manually evaluating every AI output is time-consuming and inconsistent across evaluators. AI models have sufficient metacognitive capability to meaningfully assess their own outputs against defined criteria — and doing so explicitly surfaces weaknesses that would otherwise be missed. LLM Self-Grading creates a scalable, systematic quality check that costs one extra prompt.

Situations covered:
- High-volume content generation where manual review of every item is impractical
- Tasks with clear quality criteria that can be operationalized as a rubric (clarity, completeness, accuracy, tone)
- Iterative improvement workflows where the AI grades, then improves, then grades again
- Any output where "good enough" is not sufficient and systematic quality measurement is needed

---

## 2. Procedura

### Pas 1: Define the Grading Rubric Before Generating Content
Write the rubric before generating any content — not after. This prevents the rubric from being unconsciously designed to validate whatever the AI already produced. The rubric should have: 3-6 criteria, a 1-5 or 1-10 scale for each criterion, and a precise operational definition of what each score means (not "good/bad" but specific observable characteristics).

Example rubric criterion: "Clarity (1-5): 1 = Contains unexplained jargon or ambiguous antecedents. 3 = Clear to a reader with domain background. 5 = Clear to a non-specialist reader with no background explanation required."

### Pas 2: Generate the Initial Output
Generate the content using your standard prompting approach. Do not mention the grading rubric during generation — grade it afterward. This avoids the AI optimizing for the rubric appearance rather than actual quality.

### Pas 3: Request a Structured Self-Grade
Submit a separate grading prompt: "Grade the output you just produced using this rubric: [rubric]. For each criterion: (a) assign a score, (b) quote the specific section of the output that most influenced the score, and (c) explain in one sentence why that section earned that score. Then calculate a weighted total score."

### Pas 4: Review the Grade and Reasoning
Read each criterion's score and justification. Assess: Is the AI's self-evaluation accurate? Does it identify the same weaknesses you notice? Does it miss any problems you can see? Note where the AI's self-assessment diverges from your evaluation — these divergences reveal where the rubric needs refinement or where the AI has systematic blind spots.

### Pas 5: Request Targeted Improvements for Low Scores
For any criterion scoring below your minimum threshold (e.g., below 3/5), instruct: "You scored [criterion] at [score] because [reason]. Rewrite [specific section] to score at least [target score] on this criterion. Keep all other sections unchanged." Apply improvements criterion by criterion, not all at once.

### Pas 6: Re-Grade After Improvements
After improvements are applied, run the grading prompt again on the revised output. Verify scores improved on the targeted criteria and did not decrease on others. If a previously passing criterion degraded, this indicates the improvement was too broad and affected adjacent sections.

---

## 3. Cortex Logging

```json
{
  "text": "LLM Self-Grading applied. Content type: [description]. Rubric criteria: [count and list]. Initial weighted score: [X/max]. Criteria below threshold: [list]. Post-improvement score: [X/max]. Score delta: [+N]. Rubric accuracy vs. human evaluation: [aligned/diverged on: list].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-060",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "self_grading"],
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
- Auto-grading aplicat fără rubric de evaluare pre-definit → violation
- Score auto-grading acceptat fără verificare pe sample manual → violation
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
1. `✅ [PROC] C-060 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Use LLM Self-Grading for Quality Assurance" | FORGE ✓ | rule: META-H-002 | v1.0`

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
