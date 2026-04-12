# Apply the Recipe Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Prompts the AI to generate a complete, ordered, actionable plan from a defined goal and known constraints — analogous to a recipe that specifies all steps and ingredients needed to reach the desired output.

---

## 1. Problema

When tackling a complex goal, it is easy to miss steps, misorder operations, or underspecify dependencies. The Recipe Pattern shifts the planning burden to the AI: given a goal and known ingredients (constraints, resources, context), the AI generates a complete recipe (ordered steps) to reach the output.

Situations covered:
- Project planning where you know the end state and some constraints but not all steps
- Technical implementation plans where dependencies and order matter
- Process design for new workflows or procedures
- Any task where missing a step early creates problems downstream

---

## 2. Procedura

### Pas 1: Define the Desired Output Clearly
State the goal with specificity: not just "build a website" but "build a portfolio website that loads under 2 seconds, works on mobile, and can be updated by a non-developer." The more precise the end state, the more precise the recipe.

### Pas 2: Specify Known Ingredients and Constraints
List what you already have or what is fixed: available tools, technologies, budget, time, skills, existing assets, dependencies, and non-negotiable constraints. These are the "given ingredients" — the AI should not ignore them or suggest alternatives unless they conflict with the goal.

### Pas 3: Request the Recipe
Prompt: "Given this goal and these ingredients, provide a complete step-by-step recipe to achieve the goal. Include: all steps in the correct order, what is needed at each step, how to verify each step is complete, and what to do if a step fails. Do not skip steps that seem obvious."

### Pas 4: Validate Recipe Completeness
Review the recipe against your known constraints: Are all ingredients used appropriately? Is the ordering logical (no step requires the output of a later step)? Are there any steps that assume tools or skills not in your ingredient list? Flag gaps and ask the AI to fill them.

### Pas 5: Test the Recipe on a Small Scale
Before committing to the full recipe, execute the first 2-3 steps on a reduced or test case. Verify that the early steps work as described and that the outputs feed correctly into subsequent steps. Revise the recipe based on what you discover.

### Pas 6: Execute, Document Deviations, and Update Recipe
Run the full recipe. Document any steps where you had to deviate from the AI's instructions and why. After completion, update the recipe document to reflect what actually worked. A tested, revised recipe is a reusable SOP.

---

## 3. Cortex Logging

```json
{
  "text": "Recipe Pattern applied. Goal: [summary]. Ingredients provided: [count and key ones]. Steps generated: [count]. Validation issues found: [list]. Recipe tested and updated: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-050",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "recipe"],
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
- Pașii rețetei formulați ambiguu (fără acțiune clară per pas) → violation
- Rețetă aplicată fără verificarea că toate ingredientele/resursele sunt disponibile → violation
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
1. `✅ [PROC] C-050 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Recipe Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
