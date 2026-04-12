---
id: ND-020
title: "Ethical Considerations in Prompt Engineering"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-020: Ethical Considerations in Prompt Engineering

## Obiectiv

Identificarea și evitarea bias-urilor în prompt-uri, crearea de prompt-uri inclusive și corecte. Prompt-urile pot amplifica sau atenua bias-urile existente în modelele AI. Responsabilitatea e a celui care scrie prompt-ul.

## Pași

### Pas 1: Identifică bias-uri potențiale în prompt
```
# Bias de gen:
BIASED: "A doctor examined his patient." (presupune gen masculin)
FAIR: "A doctor examined their patient."

# Bias cultural:
BIASED: "Name important scientists." (va lista predominant occidentali)
FAIR: "Name important scientists from diverse geographic regions and time periods."

# Bias de confirmare:
BIASED: "Why is X better than Y?" (presupune că X e mai bun)
FAIR: "Compare X and Y, listing strengths and weaknesses of each."
```

### Pas 2: Tehnici de debiasing

**Instrucțiuni explicite:**
```
Analyze these job applications fairly.

Important:
- Evaluate based ONLY on qualifications and experience
- Do not consider names, gender, age, or ethnicity
- Use the same criteria for all candidates
- If you notice a bias in your evaluation, flag it

Applications:
{APPLICATIONS}
```

**Prompt-uri echilibrate:**
```
# Instead of: "What are the benefits of remote work?"
# Use: "What are the benefits AND drawbacks of remote work? Present both sides fairly."
```

### Pas 3: Inclusivitate în prompt-uri
```
# Non-inclusiv:
"Write an article for businessmen about leadership."

# Inclusiv:
"Write an article for business professionals about leadership. Use gender-neutral language throughout."
```

### Pas 4: Evitarea stereotipurilor
```
When generating examples or scenarios:
- Use diverse names from various cultures
- Include varied professional backgrounds
- Avoid reinforcing stereotypes about any group
- If demographics are irrelevant to the task, omit them
```

### Pas 5: Bias audit pentru prompt-uri de producție
```
Review this prompt for potential biases:

Prompt: "{YOUR_PROMPT}"

Check for:
1. Gendered language or assumptions
2. Cultural or geographic bias
3. Age-related assumptions
4. Socioeconomic assumptions
5. Confirmation bias (leading questions)
6. Selection bias (whose perspective is represented?)

Findings:
```

### Pas 6: Documentează deciziile etice
Pentru fiecare prompt de producție, notează:
- Ce biases au fost identificate și adresate
- Ce compromisuri au fost făcute
- Cum se monitorizează bias-ul în output

## Verificare

- [ ] Prompt-ul verificat pentru bias de gen, cultură, vârstă
- [ ] Limbaj inclusiv și neutru (unde relevant)
- [ ] Întrebări ne-leading (nu presupun răspunsul)
- [ ] Diversitate în exemple (dacă incluse)
- [ ] Bias audit documentat

## Instrumente

| Model | Note |
|-------|------|
| Toate modelele | Toate pot perpetua bias-uri — prompt-ul contează |
| LLM-as-auditor | Folosește modelul să-și auditeze propriile prompt-uri |

## Note

- **Când să folosești**: Prompt-uri customer-facing; hiring/evaluation; content generation; orice cu impact social.
- **Când NU**: Task-uri pur tehnice fără component umană (but still good practice).
- **Greșeli comune**: Presupunerea că modelul e "neutru"; leading questions; stereotipuri neintenționate în exemple.
- **Relație**: ND-021 (Security), ND-014 (Clarity), ND-016 (Negative Prompting).
