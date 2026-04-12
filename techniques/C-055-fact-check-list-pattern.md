# Apply the Fact Check List Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Instructs the AI to generate a list of specific verifiable claims alongside any content it produces, enabling systematic fact verification and reducing reliance on unverified AI outputs in high-stakes contexts.

---

## 1. Problema

AI-generated content contains factual claims that may be incorrect, outdated, or hallucinated — and these errors are not always obvious from reading the text. The Fact Check List Pattern makes the AI's claims explicit and auditable by generating them as a structured list alongside the content, enabling efficient targeted verification rather than hoping errors will be noticed on casual reading.

Situations covered:
- Research summaries, reports, or briefings that will be presented as factual
- Content where a single factual error carries reputational or legal risk
- AI-assisted journalism, academic writing, or due diligence
- Any output where "sounds right" is not an acceptable accuracy standard

---

## 2. Procedura

### Pas 1: Request Content Plus Claim List in the Same Prompt
Structure the prompt to produce two outputs simultaneously: "Generate [content type] on [topic]. After the content, append a section titled 'Claims to Verify' that lists every specific factual claim in the content that should be independently verified. Format each claim as: [Claim]: [the specific claim] | [Why verify]: [what type of source would confirm or deny this]."

### Pas 2: Separate Claims by Verification Priority
Once the claim list is generated, sort claims into tiers: (A) High priority — claims that are central to the content's conclusions and that, if wrong, would invalidate the output; (B) Medium priority — supporting claims that affect quality but not the core conclusion; (C) Low priority — peripheral or broadly-true claims where AI error is unlikely.

### Pas 3: Verify Tier A Claims First
Verify every Tier A claim against authoritative sources before using the content. For each claim, note: verified (source), unverified (could not find source), or contradicted (source says otherwise). Do not proceed to distributing or acting on the content until all Tier A claims are resolved.

### Pas 4: Handle Contradicted or Unverifiable Claims
For any claim that is contradicted by a source or that cannot be verified: (a) remove or correct the claim in the content, (b) note in the output that the claim was removed or qualified, (c) ask the AI to regenerate the affected section with the corrected information.

### Pas 5: Mark Verified Content Clearly
After completing verification, mark the final content with its verification status: which claims are verified (with sources), which are qualified ("according to AI, unverified"), and which were corrected or removed. This transparency is especially important for content shared with others.

### Pas 6: Feed Verification Results Back to the AI
Tell the AI which claims were wrong and what the correct information is. Ask it to regenerate the content incorporating the corrections. This creates a feedback loop that improves the final output and teaches you which topics the AI tends to hallucinate on.

---

## 3. Cortex Logging

```json
{
  "text": "Fact Check List Pattern applied. Content type: [description]. Claims identified: [count]. Tier A claims: [count]. Verified: [count]. Contradicted/corrected: [count]. Unverifiable: [count]. Content quality rating post-verification: [assessment].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-055",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "fact_check_list"],
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
- Fact-checking realizat fără surse verificabile pentru fiecare claim → violation
- Listă de facts aplicată pe conținut fără date/statistici verificabile → violation
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
1. `✅ [PROC] C-055 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Fact Check List Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
