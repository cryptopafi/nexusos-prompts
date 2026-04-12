---
id: PR-050
title: "Universal Self-Adaptive Prompting (USP)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-050: Universal Self-Adaptive Prompting (USP)

## Obiectiv

USP (Wan et al., 2023) generalizează COSP pentru a funcționa pe orice tip de task, nu doar reasoning. COSP depinde de Self-Consistency (necesită răspunsuri discrete comparabile). USP adaugă mecanisme pentru task-uri open-ended: scoring bazat pe quality signals (confidence, coherence, completeness) în loc de agreement.

## Pași

### Pas 1: Clasifică task-ul
- **Close-ended** (clasificare, math, QA factual): folosește COSP standard (agreement-based)
- **Open-ended** (generare, sumarizare, writing): folosește USP (quality-based)

### Pas 2: Generează răspunsuri candidat pe bootstrapping set
```python
candidates = {}
for input_text in bootstrapping_inputs:
    responses = [llm(f"{input_text}", temperature=0.7) for _ in range(N)]
    candidates[input_text] = responses
```

### Pas 3: Evaluează calitatea răspunsurilor (quality signals)
```python
def score_response(input_text, response, task_type):
    """Score a response using LLM-as-judge"""
    score = llm(f"""
        Task input: {input_text}
        Response: {response}
        Rate this response (1-10) on:
        - Relevance: Does it address the input?
        - Completeness: Does it cover all aspects?
        - Quality: Is it well-written/reasoned?
        Overall score (1-10):
    """, temperature=0)
    return parse_score(score)
```

### Pas 4: Selectează exemplele cu cel mai mare scor
```python
exemplars = []
for input_text, responses in candidates.items():
    scored = [(r, score_response(input_text, r, task_type)) for r in responses]
    best = max(scored, key=lambda x: x[1])
    exemplars.append({
        "input": input_text,
        "output": best[0],
        "score": best[1]
    })

# Sort by score, take top K
exemplars.sort(key=lambda x: x["score"], reverse=True)
selected = exemplars[:K]
```

### Pas 5: Construiește prompt-ul Few-Shot automat
```python
prompt = "Here are examples of high-quality responses:\n\n"
for ex in selected:
    prompt += f"Input: {ex['input']}\nOutput: {ex['output']}\n\n"
prompt += f"Input: {{new_input}}\nOutput:"
```

### Pas 6: Aplică pe orice task type
- Sumarizare: bootstrappează pe texte similare, selectează cele mai bune sumarizări
- Code: bootstrappează pe probleme similare, selectează codul cel mai corect
- Writing: bootstrappează pe topicuri similare, selectează cel mai bun scris

## Verificare

- [ ] Task-ul clasificat corect (close/open-ended)
- [ ] Quality scoring funcționează (1-10 realistic)
- [ ] Top-K exemplare selectate pe baza scorului
- [ ] Prompt-ul final testat pe input-uri noi
- [ ] Performanță superioară vs. zero-shot sau random exemplars

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM (Opus/GPT-4) | Evaluator de calitate |
| LLM (Sonnet/GPT-3.5) | Generator de candidați (cost-eficient) |
| Python | Pipeline USP |

## Note

- **Când să folosești**: Task-uri open-ended fără exemplare; generalizare COSP; sisteme self-improving.
- **Când NU**: Ai deja exemplare de calitate; task-uri close-ended (COSP e suficient); evaluatorul e slab.
- **Greșeli comune**: Evaluator bias (auto-evaluare inflated); bootstrapping set nereprezentativ; K prea mare (noise).
- **Relație**: PR-049 (COSP), PR-045 (Self-Consistency), PR-046 (Universal SC), PR-011 (SG-ICL).
