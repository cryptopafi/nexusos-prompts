# Apply the Semantic Filter Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Applies semantic criteria (tone, sentiment, relevance, audience-appropriateness, or other meaning-based criteria) as explicit filters on AI-generated output, ensuring content meets qualitative standards that cannot be enforced by simple keyword rules.

---

## 1. Problema

AI outputs often include content that is technically correct but semantically inappropriate for the context — wrong tone, mismatched sentiment, off-brand language, or irrelevant tangents. Simple keyword filtering cannot catch these issues. The Semantic Filter Pattern addresses this by instructing the AI to apply meaning-based evaluation criteria as part of the generation or post-generation process.

Situations covered:
- Brand voice compliance (content must match a specific tone profile)
- Sentiment control for customer-facing communications (no negative framing)
- Relevance filtering for research summaries (exclude tangential information)
- Audience-appropriateness screening (no content inappropriate for the target group)
- Any output where "what it means" matters as much as "what it says"

---

## 2. Procedura

### Pas 1: Define the Semantic Filter Criteria
Write precise, operational definitions for each filter criterion. Vague criteria ("professional tone") produce inconsistent results. Precise criteria work better: "Tone must be confident but not arrogant — use declarative statements, avoid hedging language like 'might' or 'could be considered', but also avoid superlatives like 'best' or 'unmatched'."

Define criteria as: (a) what to include, (b) what to exclude, and (c) a concrete example of a passing vs. failing phrase for each criterion.

### Pas 2: Apply the Filter at Generation Time
Include filter criteria directly in the generation prompt: "Generate [content] applying the following semantic filters throughout: [list criteria with definitions]. Do not generate content that violates any filter criterion — if you would naturally include something that violates a filter, omit it or rephrase."

### Pas 3: Request a Post-Generation Self-Audit
After generation, ask the AI to audit its own output: "Review the content you just generated. For each filter criterion, identify any section that may violate it and explain why. Mark it with [FILTER_ISSUE: criterion name]." This surfaces likely violations for human review.

### Pas 4: Review Flagged Sections
Read every section flagged by the AI's self-audit. For each: (a) agree the flag is valid and request a fix, (b) disagree and note why the AI's filter interpretation was too strict or too loose, or (c) accept the content as-is with a documented exception. Updating your criteria definitions based on disagreements sharpens the filter over time.

### Pas 5: Regenerate Flagged Sections
For all valid flags, request targeted rewrites: "Rewrite [section] so it passes the [criterion] filter. The issue was [specific problem identified in Pas 4]." Do not regenerate the entire document for one section — targeted rewrites preserve what was working.

### Pas 6: Calibrate Filter Sensitivity
After using the filter across several outputs, assess: Is the filter too strict (flagging acceptable content) or too loose (missing real violations)? Adjust the precision of your criteria definitions accordingly. Document the calibrated filter as a reusable prompt component.

---

## 3. Cortex Logging

```json
{
  "text": "Semantic Filter Pattern applied. Content type: [description]. Filter criteria defined: [count and list]. Self-audit flags generated: [count]. Valid flags: [count]. Criteria calibrated after session: [yes/no]. Filter saved as reusable block: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-057",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "semantic_filter"],
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
- Criterii de filtrare nedefinite explicit înainte de aplicare → violation
- Filtrare aplicată fără compararea output-ului pre/post-filtru → violation
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
1. `✅ [PROC] C-057 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Semantic Filter Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
