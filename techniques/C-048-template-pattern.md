# Apply the Template Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Uses a pre-defined structural template with explicit placeholders to constrain AI output format, ensuring consistent structure across multiple outputs of the same type.

---

## 1. Problema

When AI outputs need to follow a precise format — for downstream processing, document consistency, or workflow integration — free-form generation produces structural variation that breaks pipelines or requires manual reformatting. The Template Pattern locks the structure while letting the AI fill in the substance.

Situations covered:
- Generating multiple documents of the same type (reports, SOPs, product descriptions, meeting summaries)
- AI output that feeds into a downstream system requiring fixed fields
- Team workflows where everyone needs consistent document structure
- Rapid content production at scale where structure must be guaranteed

---

## 2. Procedura

### Pas 1: Design the Template with Explicit Placeholders
Build the template using clear placeholder syntax, for example `{{PLACEHOLDER_NAME}}` or `[PLACEHOLDER_NAME]`. Every variable element in the output should be a named placeholder. Fixed structural elements (headers, labels, static instructions) should be written in full. Example for a product brief: `# {{PRODUCT_NAME}}\n**Target customer**: {{TARGET_CUSTOMER}}\n**Core benefit**: {{CORE_BENEFIT_ONE_SENTENCE}}`

### Pas 2: Document What Fills Each Placeholder
For each placeholder, write a one-line specification: what type of content goes there, length constraint, tone or format requirements. This is the "fill guide" you will include in your prompt. Without it, the AI makes arbitrary choices about what each placeholder means.

### Pas 3: Provide the Template and Fill Guide to the AI
Submit to the AI: "Fill in the following template using the guidelines provided. Return only the completed template — do not add sections, remove sections, or change any structural elements outside the placeholders." Include the template and the fill guide in the prompt.

### Pas 4: Provide the Source Content or Context
Give the AI the raw material it needs to fill the placeholders: background information, key points, data, source text, or topic. The cleaner and more complete the source material, the better the fills will be.

### Pas 5: Validate Template Completeness
Check every placeholder in the completed output: (a) Was every placeholder filled? (b) Was the fill guide followed for each? (c) Did the AI add, remove, or rename any structural elements? Flag any deviation as a violation and request correction.

### Pas 6: Iterate on Template Design
After using the template 2-3 times in real conditions, review: Which placeholders were consistently filled poorly? Which were ambiguous? Revise the template and fill guide based on observed failures. A well-tested template is a reusable asset.

---

## 3. Cortex Logging

```json
{
  "text": "Template Pattern applied. Template type: [name/purpose]. Placeholders defined: [count]. Fill success rate: [X/Y filled correctly]. Template issues identified: [list]. Template saved for reuse: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-048",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "template"],
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
- Template aplicat fără validarea că toate placeholder-urile sunt completate → violation
- Template reutilizat fără adaptare la noul context → violation
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
1. `✅ [PROC] C-048 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Template Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
