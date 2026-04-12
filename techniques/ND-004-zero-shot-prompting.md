---
id: ND-004
title: "Zero-Shot Prompting"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-004: Zero-Shot Prompting

## Obiectiv

Zero-Shot Prompting cere modelului să execute un task fără niciun exemplu — doar cu instrucțiuni. Modelul se bazează pe cunoștințele din training. E cea mai simplă formă de prompting și punctul de start pentru orice task nou.

## Pași

### Pas 1: Formulează instrucțiunea clar și specific
```
# Slab (vag):
"Analyze this text."

# Bun (specific):
"Identify the 3 main arguments in this text. For each argument, state the claim and the evidence provided."
```

### Pas 2: Specifică formatul de output
```
Classify this email as one of: [Urgent, Normal, Low Priority, Spam].
Respond with ONLY the category name, nothing else.

Email: {EMAIL_TEXT}
Category:
```

### Pas 3: Adaugă constrângeri
```
Summarize this article.

Constraints:
- Maximum 100 words
- Use simple language (8th grade reading level)
- Include the main conclusion
- Do not include author opinions

Article: {TEXT}
Summary:
```

### Pas 4: Testează și evaluează
Rulează pe 5-10 input-uri diverse. Dacă rezultatele sunt inconsistente:
- Instrucțiunea e ambiguă → reformulează
- Task-ul e complex → adaugă structură (CoT) sau exemple (Few-Shot)
- Formatul variază → adaugă constrângeri mai stricte

### Pas 5: Adaugă context când necesar
```
# Fără context (zero-shot pur):
"What is the capital of Bhutan?"

# Cu context (zero-shot cu background):
"You are helping a geography student prepare for an exam.
What is the capital of Bhutan? Include one interesting fact about the city."
```

### Pas 6: Decide dacă Zero-Shot e suficient
Zero-Shot e suficient dacă: task-ul e simplu, modelul e capabil, rezultatele sunt consistente. Dacă nu → treci la Few-Shot (ND-005) sau CoT (ND-006).

## Verificare

- [ ] Instrucțiunea e specifică (nu vagă)
- [ ] Formatul de output e specificat
- [ ] Constrângeri relevante incluse
- [ ] Testat pe 5+ input-uri diverse
- [ ] Dacă inconsistent → escaladat la Few-Shot sau CoT

## Instrumente

| Model | Eficacitate Zero-Shot |
|-------|----------------------|
| Claude Opus | Excelent — task-uri complexe fără exemple |
| GPT-4/5 | Excelent |
| Claude Sonnet/Haiku | Bun pentru task-uri simple/medii |
| Modele mici | Moderat — necesită adesea Few-Shot |

## Note

- **Când să folosești**: Primul test pentru orice task; task-uri simple; clasify, summarize, translate; prototipare.
- **Când NU**: Task-uri cu format specific nestandard; reasoning complex; când rezultatele sunt inconsistente.
- **Greșeli comune**: Instrucțiuni vagi; lipsa formatului de output; neescalarea la Few-Shot când Zero-Shot nu merge.
- **Relație**: ND-005 (Few-Shot — next step), ND-006 (CoT — adaugă reasoning), PR-014-021 (zero-shot techniques).
