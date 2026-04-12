---
id: ND-015
title: "Prompt Length and Complexity Management"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-015: Prompt Length and Complexity Management

## Obiectiv

Gestionarea lungimii și complexității prompt-urilor pentru a maximiza calitatea output-ului în limitele context window-ului. Prea scurt = ambiguu. Prea lung = pierdere de focus și cost ridicat. Această procedură oferă strategii de echilibrare.

## Pași

### Pas 1: Cunoaște limitele modelului
| Model | Context Window | Recomandare Prompt |
|-------|---------------|-------------------|
| Claude Opus (1M) | 1M tokens | Poți fi detaliat, dar structurat |
| GPT-4 (128K) | 128K tokens | Moderat |
| Claude Sonnet | 200K tokens | Moderat |
| Modele mici | 4-8K tokens | Concis, esențial |

### Pas 2: Regulă 80/20 — Prompt-ul ocupă max 20% din context
Lasă 80% pentru output. Dacă prompt-ul e 10K tokens dar ai doar 8K pentru output, modelul va trunchia.

### Pas 3: Tehnici de compresie

**Eliminare redundanțe:**
```
# Lung și redundant:
"I want you to summarize this article. The summary should be a concise summary that captures the main points. Please make sure your summary is brief and to the point."

# Comprimat:
"Summarize this article in 3 sentences capturing the main points."
```

**Hierarchical context:**
```
# Instead of full document:
"Here's a summary of the document: {SUMMARY}

Relevant sections:
- Section 3.2: {RELEVANT_EXCERPT}
- Section 5.1: {RELEVANT_EXCERPT}

Question: {QUESTION}"
```

**Progressive disclosure:**
```
# Start cu overview, detalii on demand:
Prompt 1: "Analyze this data at a high level. What are the 3 main trends?"
Prompt 2: "Dive deeper into trend #2. Provide supporting data and implications."
```

### Pas 4: Structurare pentru prompt-uri lungi
```
# Structure long prompts with clear sections:
## Context
[background information — keep minimal]

## Task
[what you want done — be specific]

## Constraints
[rules and limitations]

## Output Format
[exact format expected]

## Input
[the actual data/text to process]
```

### Pas 5: Măsoară eficiența prompt-ului
```python
def prompt_efficiency(prompt, output, quality_score):
    """Higher = more efficient"""
    prompt_tokens = count_tokens(prompt)
    output_tokens = count_tokens(output)
    return quality_score / (prompt_tokens + output_tokens)
```

### Pas 6: Optimizare iterativă a lungimii
1. Scrie prompt-ul complet
2. Elimină o secțiune și testează — scade calitatea?
3. Dacă nu, secțiunea era redundantă
4. Repetă până la minimum viable prompt

## Verificare

- [ ] Prompt-ul e sub 20% din context window
- [ ] Nicio informație redundantă
- [ ] Structurat cu secțiuni clare (dacă > 200 cuvinte)
- [ ] Testat cu versiunea scurtă vs. lungă
- [ ] Eficiența calculată

## Instrumente

| Instrument | Rol |
|-----------|-----|
| tiktoken / anthropic tokenizer | Numărare tokens |
| Python | Eficiency tracking |

## Note

- **Când să folosești**: Orice prompt, mai ales cele lungi; când se apropie de limitele context window.
- **Când NU**: Prompt-uri scurte (<50 cuvinte) — nu necesită management.
- **Greșeli comune**: Prompt-uri excesiv de lungi fără structură; informație irelevantă inclusă; nemonitorizarea token count.
- **Relație**: ND-012 (instructions), ND-014 (ambiguity), ND-017 (formatting).
