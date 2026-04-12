---
id: ND-012
title: "Instruction Engineering"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-012: Instruction Engineering

## Obiectiv

Instruction Engineering e arta de a scrie instrucțiuni clare, neambigue și eficiente pentru LLM-uri. O instrucțiune bine formulată face diferența între output mediocru și excelent. Această procedură oferă principii și pattern-uri pentru instrucțiuni optime.

## Pași

### Pas 1: Aplică principiul ACȚIUNE + OBIECT + CONSTRÂNGERI
```
# Pattern: [VERB] [CE] [CUM/CÂT/UNDE]

# Slab:
"Help me with this data"

# Bun:
"Analyze [ACTION] this sales dataset [OBJECT] and identify the top 3 growth trends. Present findings as bullet points with specific percentages [CONSTRAINTS]."
```

### Pas 2: Folosește verbe de acțiune specifice
| Evită | Folosește |
|-------|----------|
| "Help me with" | "Analyze", "Write", "Compare" |
| "Tell me about" | "Explain", "List", "Describe" |
| "Do something with" | "Extract", "Transform", "Calculate" |
| "Make it better" | "Refactor for readability", "Optimize for speed" |

### Pas 3: Elimină ambiguitatea
```
# Ambiguu: "Write a short summary"
# Clar: "Write a 3-sentence summary"

# Ambiguu: "Explain in simple terms"
# Clar: "Explain so that a high school student with no programming experience can understand"

# Ambiguu: "List the main points"
# Clar: "List exactly 5 key takeaways, each in one sentence"
```

### Pas 4: Structurează instrucțiunile complexe
```
Task: Review this code for quality issues.

Follow these steps:
1. SCAN: Read the entire code and identify the general purpose
2. BUGS: List any bugs or logical errors (with line numbers)
3. STYLE: Note code style issues (naming, formatting)
4. PERFORMANCE: Identify performance concerns
5. SECURITY: Flag any security vulnerabilities
6. PRIORITY: Rank all findings as Critical/High/Medium/Low

Output format: Use markdown headers for each section.
```

### Pas 5: Testează instrucțiunile pe 3 modele/input-uri diferite
Dacă instrucțiunea produce output consistent pe diverse input-uri și modele, e bine formulată. Dacă nu, e ambiguă.

### Pas 6: Creează un instruction style guide
Documentează pattern-uri de instrucțiuni care funcționează pentru team:
```
# Instruction Style Guide v1.0
- Always start with an action verb
- Specify output format explicitly
- Include quantitative constraints (word count, item count)
- Add "Do NOT" section for common errors
- End with explicit output marker ("Output:", "Result:", etc.)
```

## Verificare

- [ ] Instrucțiunea are verb de acțiune specific
- [ ] Obiectul (ce) e clar definit
- [ ] Constrângerile sunt explicite (format, lungime, stil)
- [ ] Nicio ambiguitate identificabilă
- [ ] Testat pe 3+ input-uri cu output consistent

## Instrumente

| Model | Note |
|-------|------|
| Toate modelele | Universală — instrucțiunile clare ajută orice LLM |

## Note

- **Când să folosești**: La orice prompt; e skill-ul fundamental al prompt engineering-ului.
- **Când NU**: N/A — instrucțiuni clare sunt mereu benefice.
- **Greșeli comune**: Verbe vagi; constrângeri implicite; instrucțiuni prea lungi fără structură.
- **Relație**: ND-001 (fundamentals), PR-008 (instruction selection), ND-014 (ambiguity).
