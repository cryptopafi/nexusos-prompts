---
id: PR-046
title: "Universal Self-Consistency"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-046: Universal Self-Consistency

## Obiectiv

Universal Self-Consistency (Chen et al., 2023) extinde Self-Consistency la task-uri open-ended unde majority vote programatic nu funcționează (text liber, generare, sumarizare). În loc de counting, inserează toate output-urile într-un prompt care cere LLM-ului să identifice răspunsul majoritar/consensual.

## Pași

### Pas 1: Generează N răspunsuri (ca la Self-Consistency standard)
```python
responses = [llm(prompt, temperature=0.7) for _ in range(N)]
```

### Pas 2: Construiește prompt-ul de selecție
```
I asked the same question {N} times and got these responses:

Response 1: "{responses[0]}"
Response 2: "{responses[1]}"
Response 3: "{responses[2]}"
Response 4: "{responses[3]}"
Response 5: "{responses[4]}"

Analyze these responses. Identify the answer that is most consistent across the responses. If there is a clear majority, select it. If responses vary significantly, synthesize the most common elements.

Most consistent answer:
```

### Pas 3: Variantă cu weighted selection
```
I generated multiple answers to: "{original_question}"

Answers:
1. {response_1}
2. {response_2}
3. {response_3}
4. {response_4}
5. {response_5}

Evaluate each answer on correctness and completeness (1-10). Then select the best answer or create a synthesis of the best elements.

Evaluation:
1. Score: _/10 — [reason]
2. Score: _/10 — [reason]
...

Best/synthesized answer:
```

### Pas 4: Implementare
```python
def universal_self_consistency(question, n=5):
    # Step 1: Generate diverse responses
    responses = [llm(question, temperature=0.7) for _ in range(n)]

    # Step 2: LLM-based selection
    selection_prompt = f"""
    I asked: "{question}" and got {n} responses:
    {chr(10).join(f'{i+1}. {r}' for i, r in enumerate(responses))}

    Which response best represents the consensus? Select it or synthesize.
    Best answer:
    """
    return llm(selection_prompt, temperature=0)
```

### Pas 5: Aplică la task-uri open-ended
- Sumarizare: selectează sumarul care capturează cele mai multe puncte comune
- Code generation: selectează codul care apare cel mai des (semantic, nu literal)
- Writing: selectează sau sintetizează versiunea care combină cele mai bune elemente

### Pas 6: Compară cu Self-Consistency clasic pe task-uri close-ended
Pe task-uri cu răspuns discret, SC clasic (majority vote) poate fi superior (mai simplu, mai determinist).

## Verificare

- [ ] N >= 5 răspunsuri generate
- [ ] Prompt-ul de selecție include TOATE răspunsurile
- [ ] Selecția e făcută cu temperature=0 (deterministă)
- [ ] Funcționează pe task-uri open-ended (text liber)
- [ ] Rezultatul e mai bun decât any single response

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Claude Opus | Excelent la evaluare și sinteză |
| GPT-4/5 | Excelent |
| Orice LLM | Funcționează, dar evaluatorul trebuie să fie capabil |

## Note

- **Când să folosești**: Task-uri open-ended; sumarizare; generare text; orice unde majority vote literal nu funcționează.
- **Când NU**: Task-uri cu răspuns discret (SC clasic e mai eficient); task-uri triviale.
- **Greșeli comune**: Evaluator prea slab; N prea mic; evaluarea biased de primul răspuns (primacy bias).
- **Relație**: PR-045 (Self-Consistency clasic), PR-047 (Meta-Reasoning), PR-053 (Self-Refine).
