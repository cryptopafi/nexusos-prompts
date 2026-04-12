# Combine Multiple Prompt Patterns — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Selects, combines, and tests 2-3 complementary prompt patterns in a single interaction to address complex tasks that no single pattern can handle adequately, while managing pattern interference and optimizing combined effectiveness.

---

## 1. Problema

Real-world complex tasks often require multiple prompt engineering techniques simultaneously — a persona to set expertise level, chain-of-thought to ensure rigorous reasoning, and a template to enforce output structure, for example. Using patterns in isolation misses these synergies, but combining them naively introduces interference and confusion. This procedure makes multi-pattern combination systematic and reliable.

Situations covered:
- High-stakes, complex tasks requiring expert-level reasoning AND specific output structure AND quality verification
- Workflows where a single pattern repeatedly falls short in one specific dimension
- Advanced prompt engineering sessions where you are building reusable, production-grade prompts
- Any task complex enough that you have already tried 2+ individual patterns and found each insufficient

> **Safety Note**: Combinarea mai multor pattern-uri amplifică atât puterea cât și riscurile. Testați combinațiile în medii sandbox înainte de producție. Pattern-uri combinate pot interacționa în moduri neprevăzute și pot produce outputs greu de auditat.

---

## 2. Procedura

### Pas 1: Analyze the Task Requirements by Dimension
Before selecting patterns, decompose the task into quality dimensions: (a) Who should generate this (expertise/role)? (b) How should the reasoning work? (c) What format is required? (d) What quality checks are needed? (e) How should missing information be handled? Each dimension maps to a pattern category. List which dimensions are critical for this specific task — you only need patterns for critical dimensions.

### Pas 2: Select 2-3 Complementary Patterns
Based on your dimension analysis, select patterns that cover different dimensions. Avoid selecting two patterns that operate on the same dimension (e.g., Persona + Audience Persona — both define who is speaking/for whom, and they will conflict). Good combinations address orthogonal dimensions. Example combinations:
- Persona (who) + Cognitive Verifier (how to reason) + Template (output format)
- Flipped Interaction (input gathering) + Few-Shot CoT (reasoning) + Self-Grading (quality check)
- Audience Persona (audience fit) + Outline Expansion (structure) + Fact Check List (accuracy)

### Pas 3: Design the Combined Prompt Architecture
Write out the combined prompt structure, section by section. Identify which pattern's instructions go first (typically: role/persona first, then reasoning instructions, then format/output instructions, then quality checks). Test for conflicts: do any two pattern instructions contradict each other? Resolve conflicts by making one pattern's instruction conditional ("unless the persona's expertise makes this obvious, always show reasoning steps").

### Pas 4: Run a Minimal Test First
Before applying the combined pattern to your full task, test it on a small, representative example. Observe: (a) Does the AI apply all patterns simultaneously? (b) Do any patterns interfere with each other (one overrides another)? (c) Is the combined prompt overwhelming or confusing to the AI? A 2-3 turn test reveals most interference issues before you invest in a full-scale task.

### Pas 5: Resolve Interference Issues
If the test reveals interference between patterns, adjust in order of priority:
1. Simplify conflicting instructions by making one subordinate to the other
2. Separate pattern application into sequential phases rather than simultaneous (Phase 1: apply Pattern A, Phase 2: apply Pattern B to Phase 1's output)
3. Remove the lowest-value pattern if simplification fails and proceed with 2 patterns

### Pas 6: Execute Full Task and Document the Pattern Stack
Apply the optimized combined pattern to the full task. After completion, document the pattern stack that worked: which patterns were combined, the order of instructions, how conflicts were resolved, and the resulting output quality compared to single-pattern approaches. Name the stack for reuse (e.g., "Expert Research Stack: Persona + Cognitive Verifier + Fact Check List").

### Pas 7: Build a Reusable Pattern Stack Library
Save 3-5 successful pattern stacks as reusable prompt templates, categorized by task type. A library of tested, named stacks is a strategic asset that eliminates the need to redesign complex prompts from scratch for recurring task types.

---

## 3. Cortex Logging

```json
{
  "text": "Multi-Pattern Combination applied. Task type: [description]. Patterns combined: [list with IDs]. Interference issues found: [count and type]. Resolution approach: [describe]. Combined output quality vs. single-pattern: [assessment]. Stack named and saved: [name or N/A].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-061",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "multi_pattern"],
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
- Combinație de pattern-uri testată direct în producție fără sandbox → violation critică
- Interacțiunile dintre pattern-uri combinate nedocumentate → violation
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
1. `✅ [PROC] C-061 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Combine Multiple Prompt Patterns" | FORGE ✓ | rule: META-H-002 | v1.0`

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
