# Skeleton-of-Thought Prompting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-06
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: For long content generation tasks (reports, articles, structured documents), generate a skeleton/outline first, then fill in each section in parallel or sequentially. Reduces latency and improves structural coherence. NOT for sequential reasoning tasks.

---

## 1. Problema

Generating long documents sequentially (word by word, section by section) is slow and produces structural drift — later sections don't align well with early ones because the model loses global structure. Skeleton-of-Thought (Ning et al., 2023) separates structure from content: first generate a skeleton (outline + section placeholders), then fill each section independently. This enables parallel generation and maintains structural coherence.

CRITICAL ANTI-PATTERN: Do NOT use SoT for tasks requiring sequential reasoning (math proofs, debugging, step-by-step analysis). These have dependencies between steps that make parallelization invalid.

Situations covered:
- Long-form reports and analyses (PRISM reports, market research)
- Structured documents (procedures, project cards, playbooks)
- Multi-section articles or content pieces
- Any output where you know the structure before you know the content

Situations where SoT FAILS:
- Mathematical proofs (step N depends on step N-1)
- Debugging sessions (root cause analysis is sequential)
- Code generation where later functions call earlier ones
- Logical arguments where conclusions depend on premises built in sequence

---

## 2. Procedura

### Pas 1: Confirm SoT Applicability
Check: does the task have sections that are INDEPENDENT of each other, or does section B depend on section A's content to be coherent? If sections are independent → SoT. If sections are dependent → standard sequential generation.

Quick test: can you shuffle the sections in the final output without it making less sense? If yes → independent → SoT applicable.

### Pas 2: Generate the Skeleton
First prompt — skeleton only:

```
Create an outline for [document type] about [topic]. Include:
- Section titles and subtitles
- 1-sentence description of what each section will cover
- Approximate length for each section (short/medium/long)
- Any specific data points or examples that should appear in each section

OUTPUT FORMAT: Numbered sections with subtitles. Do NOT write the actual content yet — skeleton only.
```

### Pas 3: Review and Approve Skeleton
Before filling content: review the skeleton. Check:
- All required sections present?
- Logical flow when read top to bottom?
- Any sections that are actually sequential (if yes, note their order dependency)?
- Missing sections?

Revise skeleton if needed before proceeding.

### Pas 4: Fill Each Section
Option A (Parallel — faster): Generate all sections simultaneously in separate prompts, each receiving: the full skeleton + "Fill in ONLY Section [N]: [title]. Use the skeleton for global context. Length: [target]."

Option B (Sequential — better coherence): Fill section by section, each prompt gets: skeleton + all previously filled sections + "Now fill Section [N]."

For most use cases: Option A for independent sections, Option B when later sections reference earlier content.

### Pas 5: Assemble and Review
Combine all filled sections in skeleton order. Read for:
- Coherence between sections (transitions, consistent terminology)
- Any sections that became inconsistent during parallel fill
- Missing connections between sections

Add transition sentences between sections if needed.

---

## 3. Cortex Logging

```json
{
  "text": "Skeleton-of-Thought applied. Document type: [type]. Sections: [N]. Generation mode: [parallel/sequential]. Skeleton review required changes: [yes/no]. Assembly quality: [smooth/needed transitions/inconsistencies found].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-069",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "skeleton_of_thought", "parallel_generation", "long_form"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
PromptForge v2.2 Phase 2 Extended Techniques — for long content generation

### WHEN
Documents >3 sections, output >3000 words, structured content with known outline

### HOW (violation detection)
- SoT applied to sequential reasoning task → violation (produces wrong output)
- Content generated without skeleton review → soft violation
- Sections filled without providing skeleton as context → violation

### VERIFY
- [ ] SoT applicability confirmed (not sequential reasoning)?
- [ ] Skeleton generated and reviewed before filling?
- [ ] Each section fill prompt includes full skeleton?
- [ ] Assembly reviewed for cross-section coherence?

**VK-uri obligatorii:**
1. `✅ [PROC] C-069 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Skeleton-of-Thought Prompting" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Skeleton generation | Sonnet |
| Section fill (parallel) | Sonnet (multiple simultaneous) |
| Assembly review | Sonnet |
| Quality audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Parallel task execution | For Option A parallel fill |
| Document assembly | Manual or tool-assisted section combination |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Latency improvement vs sequential | >30% on long documents |
| Structural coherence score | No cross-section inconsistencies at assembly |
| SoT misapplication rate | 0% on sequential reasoning tasks |
