# Apply the Meta-Language Creation Pattern — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Designs and establishes a domain-specific shorthand language that the AI interprets consistently, replacing verbose repetitive prompt fragments with compact notation for efficiency and precision.

---

## 1. Problema

Power users who interact with AI repeatedly for the same types of tasks spend significant effort re-explaining context, format preferences, and operation types in every prompt. The Meta-Language Creation Pattern solves this by establishing a shared vocabulary and notation that both the user and AI agree on, enabling much shorter and more precise prompts.

Situations covered:
- High-frequency workflows with repetitive prompt patterns (e.g., daily content generation, code review routines)
- Complex prompting requiring multiple simultaneous instructions that are cumbersome to spell out each time
- Team environments where standardized AI interaction language improves consistency
- Advanced workflows where prompt precision directly drives output quality

> **Safety Note**: Meta-language-urile personalizate pot fi exploatate pentru prompt injection sau bypass de filtre de siguranță. Utilizați exclusiv în contexte controlate și nu expuneți meta-limbajele create unor sisteme de producție fără audit de securitate.

---

## 2. Procedura

### Pas 1: Identify Repetitive Prompt Fragments
Audit 10-20 of your recent prompts in a given domain. Highlight patterns that appear repeatedly: recurring instructions ("always respond in JSON"), common output types ("give me a bullet summary"), frequent context tags ("this is for a non-technical audience"), or operation types ("critique this", "expand this", "compare to X"). These are your meta-language candidates.

### Pas 2: Design the Shorthand Notation
Create a compact symbol or keyword system for each identified pattern. Examples:
- `>>JSON` = respond in valid JSON
- `>>ELI5` = explain at a fifth-grade level
- `>>CRIT[n]` = provide n critical objections
- `>>EXPAND[section]` = expand the named section
- `#nontechnical` = assume non-technical audience

Choose notation that is unlikely to appear naturally in content and that is visually distinct from prose.

### Pas 3: Define the Meta-Language in a System Prompt
Write a meta-language definition block that you will prepend to AI sessions where you want to use it. The definition must list every symbol/keyword, its exact meaning, and an example of correct use. Submit this definition to the AI and ask it to confirm each mapping before you begin using the language.

### Pas 4: Test Interpretation with Synthetic Prompts
Write 3-5 test prompts using only your new meta-language notation (no natural language explanation). Verify the AI interprets each correctly. If any symbol is misinterpreted or ignored, revise its definition — often the issue is ambiguity or collision with natural language patterns.

### Pas 5: Use Meta-Language in Real Prompts
Apply your meta-language in actual work prompts. Start with simple cases (one or two symbols per prompt) before combining multiple symbols. Verify that each symbol is being applied correctly in the output.

### Pas 6: Document and Version the Meta-Language
Save the final meta-language definition as a reusable system prompt block. Version it (v1.0, v1.1) and note changes. Share with team members if applicable. A meta-language is only an asset if it is documented and recoverable between sessions.

---

## 3. Cortex Logging

```json
{
  "text": "Meta-Language Creation Pattern applied. Domain: [domain]. Symbols defined: [count and list key ones]. Test interpretation pass rate: [X/Y]. Meta-language version: [v1.0]. Saved as reusable block: [yes/no].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-049",
    "domain": "ai_prompt_engineering",
    "rule_id": "META-H-002",
    "tags": ["prompt_engineering", "ai", "meta_language"],
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
- Meta-limbaj creat fără documentarea sintaxei complete → violation
- Meta-limbaj utilizat în context de producție fără audit de securitate → violation critică
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
1. `✅ [PROC] C-049 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Meta-Language Creation Pattern" | FORGE ✓ | rule: META-H-002 | v1.0`

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
