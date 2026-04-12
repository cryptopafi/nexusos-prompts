---
id: PR-034
title: "Decomposed Prompting (DECOMP)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-034: Decomposed Prompting (DECOMP)

## Obiectiv

DECOMP (Khot et al., 2022) descompune un task complex în sub-task-uri, fiecare rezolvat de un "handler" specializat (funcție, tool, sau alt prompt). Modelul învață prin Few-Shot să delege sub-task-uri la funcții specifice (search, calculate, translate etc.). Fundament pentru agentic prompting.

## Pași

### Pas 1: Definește funcțiile/handler-ele disponibile
Listează tool-urile pe care modelul le poate folosi:

```
Available functions:
- SEARCH(query): searches the web and returns relevant information
- CALCULATE(expression): evaluates a mathematical expression
- TRANSLATE(text, language): translates text to target language
- EXTRACT(text, field): extracts a specific field from text
```

### Pas 2: Furnizează exemple Few-Shot de cum să folosească funcțiile

**Template:**
```
You can use these functions to solve problems:
- SEARCH(query) — web search
- CALCULATE(expression) — math
- EXTRACT(text, field) — extract info

Example:
Q: What is the GDP per capita of the country with the tallest building?
A:
Step 1: SEARCH("tallest building in the world")
→ "The Burj Khalifa in Dubai, UAE, is the tallest building at 828m."
Step 2: EXTRACT(result, "country")
→ "UAE"
Step 3: SEARCH("GDP per capita UAE 2024")
→ "$43,103"
Final answer: $43,103

Now solve:
Q: {NEW_QUESTION}
A:
```

### Pas 3: Implementare agentic (programmatic)
```python
def decomp_solve(question, functions):
    # LLM generates decomposition plan
    plan = llm(f"""
        Available functions: {list(functions.keys())}
        Question: {question}
        Generate a step-by-step plan using these functions.
    """)

    # Execute each step
    results = {}
    for step in parse_steps(plan):
        func_name, args = parse_function_call(step)
        results[step.id] = functions[func_name](*args)

    # LLM synthesizes final answer
    return llm(f"Given these results: {results}\nAnswer: {question}")
```

### Pas 4: Definește handler-e per domeniu
- **Data analysis**: QUERY_DB(), AGGREGATE(), VISUALIZE()
- **Research**: SEARCH(), CITE(), SUMMARIZE()
- **Code**: READ_FILE(), RUN_CODE(), TEST()

### Pas 5: Gestionează erorile de funcții
```
If a function returns an error or no results:
1. Try rephrasing the query
2. Try an alternative function
3. If no function works, use your own knowledge with a caveat
```

### Pas 6: Validează planul de descompunere
Verifică: (1) Funcțiile sunt folosite corect, (2) Ordinea e logică, (3) Toate informațiile necesare sunt obținute înainte de sinteza finală.

## Verificare

- [ ] Funcțiile disponibile sunt definite clar
- [ ] Minim 1 exemplu Few-Shot de utilizare a funcțiilor
- [ ] Fiecare pas din plan folosește o funcție disponibilă
- [ ] Ordinea pașilor e logică
- [ ] Error handling definit
- [ ] Sinteza finală integrează toate rezultatele

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent — tool use nativ |
| GPT-4/5 | Excelent — function calling built-in |
| Modele cu tool use | Ideal — DECOMP se mapează direct pe tool use |

## Note

- **Când să folosești**: Task-uri care necesită multiple surse/tools; agentic workflows; sisteme cu API-uri disponibile.
- **Când NU**: Task-uri rezolvabile dintr-un singur prompt; când nu ai funcții/tools disponibile.
- **Greșeli comune**: Funcții prea vagi; prea multe funcții (confuzie); lipsa exemplelor de utilizare; nevalidarea planului.
- **Relație**: PR-033 (Least-to-Most), PR-036 (ToT), C-075 (Agentic Tool Use).
