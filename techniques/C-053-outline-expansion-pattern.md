# Apply the Outline Expansion Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Builds complex long-form content by first establishing a structural outline and then expanding each section individually, maintaining consistency and allowing course-correction at the structure level before investing in full content generation.

---

## 1. Problema

Generating long-form content (reports, courses, documentation, books) in one pass produces inconsistent structure, uneven depth across sections, and sections that contradict each other. The Outline Expansion Pattern addresses this by separating structural design from content generation, allowing quality control at each level.

Situations covered:
- Long-form documents where section structure matters (technical documentation, reports, courses)
- Content where maintaining consistent terminology and tone across sections is critical
- Projects where you need stakeholder sign-off on structure before investing in full content
- Any situation where "writing one giant prompt" has previously produced unusable outputs

---

## 2. Procedura

### Pas 1: Generate the High-Level Outline
Ask the AI to produce a top-level outline only: main sections and their one-sentence purpose. Prompt: "Create a high-level outline for [content type] on [topic]. List only the main sections and a one-sentence description of what each section covers. Do not write any content yet." Review and approve or revise the outline before proceeding.

### Pas 2: Expand the Outline to Second Level
With the high-level outline approved, ask the AI to expand it one level deeper: subsections for each main section, again as brief descriptors only. This second-level outline is your master structure. Review it holistically — ensure logical flow, no redundancy between sections, and balanced depth.

### Pas 3: Establish Cross-Section Consistency Rules
Before writing any content, define: the terminology that must be used consistently across sections, the tone and reading level, any data or examples that must be referenced in specific sections, and cross-reference requirements (where one section should point to another). Give these rules to the AI as a style guide for all expansions.

### Pas 4: Expand Sections Individually
Expand each section one at a time. For each, provide: the section's position in the outline, its one-sentence purpose, the relevant consistency rules, and a summary of any preceding sections that this section builds on. Generate section content, review, and approve before moving to the next section.

### Pas 5: Compile and Check for Consistency
After all sections are written, compile the full document. Do a consistency pass: check that terminology is uniform, cross-references are present where specified, and sections flow logically from one to the next. Ask the AI to do a consistency audit: "Review these sections and identify any contradictions, inconsistent terminology, or missing cross-references."

### Pas 6: Write Introduction and Conclusion Last
Write the introduction and conclusion after all body sections are finalized — not before. At this point, the AI can write an accurate introduction that correctly sets up what follows and a conclusion that genuinely summarizes what was covered.

---

## 3. Cortex Logging

```json
{
  "text": "Outline Expansion Pattern applied. Content type: [document/course/report/etc]. Sections in outline: [count]. Expansion approach: [sequential/parallel]. Consistency issues found in audit: [count]. Final document quality: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-053",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "outline_expansion"],
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
- Outline creat fără validarea structurii înainte de expansiune → violation
- Expansiune aplicată pe un outline cu mai puțin de 3 niveluri → violation
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
1. `✅ [PROC] C-053 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Outline Expansion Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
