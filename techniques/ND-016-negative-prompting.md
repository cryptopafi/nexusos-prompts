---
id: ND-016
title: "Negative Prompting and Avoiding Undesired Outputs"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-016: Negative Prompting and Avoiding Undesired Outputs

## Obiectiv

Negative prompting specifică explicit ce NU trebuie inclus în output. Complementar instrucțiunilor pozitive ("fă asta"), instrucțiunile negative ("NU fă asta") previn greșeli comune și output nedorit.

## Pași

### Pas 1: Identifică output-urile nedorite frecvente
Ce greșeli face modelul pe task-ul tău? Listează-le:
- Output prea lung / prea scurt
- Ton nepotrivit
- Informații inventate (halucinări)
- Disclaimer-uri excesive
- Format greșit
- Jargon tehnic unde nu e dorit

### Pas 2: Formulează constrângeri negative explicite

```
Write a product review response.

DO NOT:
- Apologize excessively
- Promise things we can't deliver
- Share internal pricing or policies
- Use corporate jargon ("synergy", "leverage", "circle back")
- Write more than 150 words
- Make up information about the product

DO:
- Acknowledge the customer's concern specifically
- Offer one concrete solution
- Use a warm but professional tone
```

### Pas 3: Negative prompting pentru output structurat
```
Generate a JSON object for this user profile.

DO NOT include:
- Nested objects deeper than 2 levels
- Arrays with more than 10 items
- Null values (use empty string "" instead)
- Comments or explanations outside the JSON
- Markdown formatting around the JSON

Input: {USER_DATA}
```

### Pas 4: Negative prompting pentru stilistică
```
Write a blog post about remote work trends.

Avoid:
- Clichés ("in today's fast-paced world", "now more than ever")
- Passive voice
- Sentences longer than 25 words
- Starting paragraphs with "However" or "Moreover"
- Generic advice without specific data
```

### Pas 5: Combină pozitiv + negativ
Regula: pozitivul ÎNAINTE de negativ. Spune CE vrei mai întâi, apoi ce NU vrei.

```
# Bun: pozitiv → negativ
"Write a concise executive summary (3 paragraphs, max 200 words).
Do NOT include technical details, acronyms, or recommendations."

# Slab: doar negativ
"Don't be vague, don't be too long, don't use jargon, don't..."
```

### Pas 6: Testează dacă negativul e respectat
Rulează pe 5+ input-uri. Verifică dacă output-ul evită toate elementele din lista "DO NOT". Dacă nu, reformulează mai explicit.

## Verificare

- [ ] Output-urile nedorite identificate și listate
- [ ] Constrângeri negative formulate explicit
- [ ] Pozitivul precede negativul
- [ ] Testat pe 5+ input-uri
- [ ] Niciun element din "DO NOT" prezent în output

## Instrumente

| Model | Respectare negative constraints |
|-------|-------------------------------|
| Claude | Excelent — respectă bine "do not" |
| GPT-4/5 | Bun — uneori ignoră parțial |
| Modele mici | Moderat — negativul e mai greu de respectat |

## Note

- **Când să folosești**: Output cu greșeli repetitive; customer-facing content; output parsabil; compliance.
- **Când NU**: Când nu știi ce greșeli face modelul (identifică mai întâi); brainstorming (negativul limitează).
- **Greșeli comune**: Doar negativ fără pozitiv; prea multe constrângeri negative; negativ vag ("don't be bad").
- **Relație**: ND-008 (constraints), ND-012 (instructions), ND-014 (clarity).
