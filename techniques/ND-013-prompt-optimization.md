---
id: ND-013
title: "Prompt Optimization Techniques"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-013: Prompt Optimization Techniques

## Obiectiv

Optimizarea sistematică a prompt-urilor prin A/B testing, metrici de calitate și refinement iterativ. Trece dincolo de "scrie și speră" la o abordare data-driven de îmbunătățire a prompt-urilor.

## Pași

### Pas 1: Definește metrici de succes
Înainte de optimizare, stabilește CE măsori:
- **Acuratețe**: % răspunsuri corecte (pentru task-uri cu ground truth)
- **Consistență**: % răspunsuri identice pe runs multiple
- **Aderare la format**: % output-uri în formatul cerut
- **Relevanță**: scoring 1-5 al relevanței
- **Eficiență**: tokens consumate per task

### Pas 2: Creează un set de test
```python
test_set = [
    {"input": "text1", "expected": "output1"},
    {"input": "text2", "expected": "output2"},
    # ... 20-50 items
]
```

### Pas 3: A/B Testing — Compară variante de prompt
```python
def ab_test(prompt_a, prompt_b, test_set):
    scores_a, scores_b = [], []
    for item in test_set:
        result_a = llm(prompt_a.format(input=item["input"]))
        result_b = llm(prompt_b.format(input=item["input"]))
        scores_a.append(evaluate(result_a, item["expected"]))
        scores_b.append(evaluate(result_b, item["expected"]))

    return {
        "prompt_a_avg": np.mean(scores_a),
        "prompt_b_avg": np.mean(scores_b),
        "winner": "A" if np.mean(scores_a) > np.mean(scores_b) else "B"
    }
```

### Pas 4: Refinement iterativ
```
Iteration 1: Base prompt → accuracy 72%
  → Problem: format inconsistent
Iteration 2: Added format constraints → accuracy 78%
  → Problem: edge cases mishandled
Iteration 3: Added edge case examples → accuracy 85%
  → Problem: verbose output
Iteration 4: Added conciseness constraint → accuracy 87%, format 95%
  → SHIP
```

### Pas 5: Gradient-free optimization (auto-tuning)
Cere modelului să-și îmbunătățească propriul prompt:
```
Current prompt: "{CURRENT_PROMPT}"
This prompt achieves 78% accuracy on the test set.

Failures on these inputs:
{FAILURE_EXAMPLES}

Suggest 3 improved versions of this prompt that might fix these failures:
```

### Pas 6: Documentează promptul optim
```
# Prompt Registry
Task: Email classification
Model: Claude Sonnet 4.6
Prompt v3.2 (current best):
  "[prompt text]"
Metrics: Accuracy 92%, Format compliance 98%, Avg tokens 45
Test set: 50 items, last run 2026-03-20
Previous versions: v3.1 (89%), v3.0 (85%), v2.0 (78%)
```

## Verificare

- [ ] Metrici de succes definite ÎNAINTE de optimizare
- [ ] Test set >= 20 items cu ground truth
- [ ] Minim 2 variante testate (A/B)
- [ ] Minim 3 iterații de refinement
- [ ] Promptul optim documentat cu metrici

## Instrumente

| Instrument | Rol |
|-----------|-----|
| Python + numpy | Calcul metrici |
| LLM API | Testing |
| Spreadsheet/DB | Tracking versiuni și metrici |

## Note

- **Când să folosești**: Prompt-uri de producție; sisteme cu volum mare; când calitatea contează.
- **Când NU**: One-off prompts; prototipare (optimizează DUPĂ ce ai un prompt funcțional).
- **Greșeli comune**: Optimizare fără metrici; test set prea mic; over-fitting pe test set.
- **Relație**: PR-044 (Max MI), PR-051 (Paraphrasing), ND-014 (ambiguity).
