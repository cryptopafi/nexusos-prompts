# Apply the Audience Persona Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configures the AI to tailor all outputs — language, tone, depth, format, and examples — to fit a precisely defined target audience persona rather than producing generic content.

---

## 1. Problema

Content generated without audience specification defaults to a vague "general educated reader" that fits nobody well. The Audience Persona Pattern solves the mismatch between the AI's default register and the actual reader's needs, expertise level, and context.

Situations covered:
- Writing content for a specific professional audience (e.g., CFOs, junior developers, non-technical stakeholders)
- Creating educational material pitched at the right complexity level
- Marketing copy that must resonate with a defined customer segment
- Technical documentation that must not overwhelm or under-inform readers

---

## 2. Procedura

### Pas 1: Define the Audience Persona in Detail
Construct a one-paragraph audience description covering: role/job title, domain expertise level (novice/intermediate/expert), familiarity with the topic specifically, primary goals and pain points, preferred communication style, and any context constraints (time-poor, English as second language, mobile reader, etc.).

### Pas 2: Embed the Audience Persona in the Prompt
Prepend the audience definition to every prompt in the session: "You are writing for [audience description]. Calibrate vocabulary, depth of explanation, use of jargon, and example types to precisely match this audience. Before generating, state in one sentence how you are calibrating for them."

### Pas 3: Specify Format Preferences Tied to Audience
Different audiences have different format preferences. Specify: bullet lists vs. prose, use of headers, length, code vs. no code, visual descriptions, inclusion of references. These preferences should match what your audience would realistically consume and trust.

### Pas 4: Generate Content and Inspect Calibration Statement
Review the one-sentence calibration statement the AI produces before the content. If it misidentifies the audience or uses wrong calibration (e.g., explains too simply for experts), correct it before the full output is generated.

### Pas 5: Verify Appropriateness with Spot Checks
Read the output through the eyes of the target audience. Check: Would they know this term? Is this example relevant to their world? Is this level of detail useful or noise? Flag any section that breaks the audience fit.

### Pas 6: Iterate on Audience Definition Based on Output
If outputs are still miscalibrated after Pas 5 corrections, add more specificity to the persona definition — particularly around prior knowledge and vocabulary preferences. The more precise the persona, the more targeted the output.

---

## 3. Cortex Logging

```json
{
  "text": "Audience Persona Pattern applied. Audience defined: [role + expertise level]. Content type: [blog/doc/email/etc]. Calibration quality: [accurate/needed correction]. Key adjustments made: [list].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-045",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "audience_persona"],
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
- Audiența țintă nedefinită înainte de aplicarea pattern-ului → violation
- Output neverificat față de nivelul de cunoștințe al audienței → violation
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
1. `✅ [PROC] C-045 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Audience Persona Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
