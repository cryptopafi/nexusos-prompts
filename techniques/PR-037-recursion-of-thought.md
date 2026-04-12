---
id: PR-037
title: "Recursion-of-Thought"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-037: Recursion-of-Thought

## Obiectiv

Recursion-of-Thought (Lee & Kim, 2023) funcționează ca CoT standard, dar când întâlnește o sub-problemă complicată în mijlocul raționamentului, o delegă la un alt prompt/LLM call separat. Sub-problema e rezolvată independent, rezultatul e inserat înapoi în chain-ul principal. Similar cu recursivitatea în programare.

## Pași

### Pas 1: Identifică pattern-ul recursiv
Potrivit pentru probleme unde o parte a raționamentului necesită propriul context separat: calcule complexe, sub-probleme de alt tip, sau pași care necesită propriul CoT.

### Pas 2: Definește mecanismul de delegare

**Template cu marcaj de delegare:**
```
Solve this problem step by step. If any step requires complex sub-calculation or sub-reasoning, mark it as [DELEGATE] and solve it separately.

Problem: What is the total cost of 3 items at $12.99 each with 8.25% tax, plus 2 items at $7.50 each with no tax, minus a 15% discount on the entire order?

Step 1: Calculate cost of taxed items
[DELEGATE: What is 3 × $12.99 with 8.25% tax?]
→ 3 × $12.99 = $38.97, tax = $38.97 × 0.0825 = $3.215, total = $42.19

Step 2: Calculate cost of non-taxed items
2 × $7.50 = $15.00

Step 3: Subtotal before discount
$42.19 + $15.00 = $57.19

Step 4: Apply discount
[DELEGATE: What is 15% of $57.19?]
→ $57.19 × 0.15 = $8.58

Step 5: Final total
$57.19 - $8.58 = $48.61
```

### Pas 3: Implementare programatic
```python
def recursive_solve(problem, depth=0, max_depth=3):
    if depth > max_depth:
        return llm(f"Solve directly: {problem}")

    response = llm(f"""
    Solve step by step. If a step is too complex, wrap it in [DELEGATE]...[/DELEGATE].
    Problem: {problem}
    """)

    # Find delegated sub-problems
    delegates = re.findall(r'\[DELEGATE\](.*?)\[/DELEGATE\]', response)
    for delegate in delegates:
        sub_result = recursive_solve(delegate, depth + 1)
        response = response.replace(f"[DELEGATE]{delegate}[/DELEGATE]", sub_result)

    return response
```

### Pas 4: Aplică la probleme multi-domeniu
```
Main problem: "Design a marketing campaign for a tech product launching in Q3"

Step 1: Define target audience
[DELEGATE: "Analyze the typical buyer persona for enterprise SaaS products"]
→ [sub-result inserted]

Step 2: Select channels
[DELEGATE: "Compare ROI of LinkedIn ads vs Google Ads for B2B SaaS"]
→ [sub-result inserted]

Step 3: Create timeline using audience + channel data
[continues with enriched context]
```

### Pas 5: Setează limita de recursivitate
Max 3 niveluri de recursie — mai mult = pierdere de context și overhead.

### Pas 6: Validează integrarea rezultatelor
Sub-rezultatele trebuie integrate coerent în chain-ul principal. Verifică: contextul e menținut? Informația din sub-rezultate e folosită corect?

## Verificare

- [ ] Sub-problemele complexe sunt delegate explicit
- [ ] Fiecare delegare produce un rezultat clar
- [ ] Rezultatele sunt integrate înapoi în chain-ul principal
- [ ] Max depth respectat (3 niveluri)
- [ ] Chain-ul final e coerent și complet

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — gestionează recursie profundă |
| GPT-4/5 | Excelent |
| Multi-model setup | Ideal — delegă la modele specializate |

## Note

- **Când să folosești**: Probleme multi-domeniu; task-uri cu sub-probleme de complexitate variabilă; pipelines agentic.
- **Când NU**: Probleme simple; când overhead-ul de multiple calls nu se justifică.
- **Greșeli comune**: Recursie prea adâncă; pierderea contextului între niveluri; sub-probleme triviale delegate inutil.
- **Relație**: PR-022 (CoT), PR-033 (Least-to-Most), PR-034 (DECOMP), PR-036 (ToT).
