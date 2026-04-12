---
id: ND-022
title: "Evaluating Prompt Effectiveness"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-022: Evaluating Prompt Effectiveness

## Obiectiv

Metode și metrici robuste pentru evaluarea obiectivă a eficienței prompt-urilor. Trece dincolo de "arată bine" la măsurare sistematică, repetabilă și acționabilă.

## Pași

### Pas 1: Definește criteriile de evaluare
| Criteriu | Descriere | Cum se măsoară |
|----------|-----------|----------------|
| Acuratețe | Răspunsul e corect? | % correct vs. ground truth |
| Relevanță | Răspunsul adresează cerința? | Scoring 1-5 |
| Completitudine | Toate aspectele acoperite? | % aspecte adresate |
| Format | Respectă formatul cerut? | Binary (da/nu) |
| Consistență | Același output pe runs multiple? | % agreement pe N runs |
| Eficiență | Tokens consumate | Input + output tokens |

### Pas 2: Creează un evaluation set
```python
eval_set = [
    {
        "id": 1,
        "input": "Classify: 'I love this product!'",
        "expected": "Positive",
        "difficulty": "easy"
    },
    {
        "id": 2,
        "input": "Classify: 'It works but has some issues'",
        "expected": "Neutral",
        "difficulty": "medium"
    },
    {
        "id": 3,
        "input": "Classify: 'Not bad, could be better though'",
        "expected": "Neutral",  # tricky — could be Positive
        "difficulty": "hard"
    },
    # ... 20-50 items covering easy/medium/hard
]
```

### Pas 3: Evaluare automată (task-uri cu ground truth)
```python
def evaluate_prompt(prompt_template, eval_set, model):
    results = {"correct": 0, "incorrect": 0, "total": len(eval_set)}

    for item in eval_set:
        prompt = prompt_template.format(input=item["input"])
        response = model(prompt)
        answer = extract_answer(response)

        if answer.lower() == item["expected"].lower():
            results["correct"] += 1
        else:
            results["incorrect"] += 1
            results.setdefault("errors", []).append({
                "id": item["id"],
                "expected": item["expected"],
                "got": answer,
                "difficulty": item["difficulty"]
            })

    results["accuracy"] = results["correct"] / results["total"]
    return results
```

### Pas 4: Evaluare LLM-as-judge (task-uri open-ended)
```
You are evaluating an AI response. Rate it on a scale of 1-5 for each criterion.

Task: {ORIGINAL_TASK}
Response: {AI_RESPONSE}

Criteria:
1. Relevance (1-5): Does the response address the task?
2. Accuracy (1-5): Is the information correct?
3. Completeness (1-5): Are all aspects covered?
4. Clarity (1-5): Is it well-written and clear?
5. Format (1-5): Does it follow the requested format?

Scores:
Relevance: _/5
Accuracy: _/5
Completeness: _/5
Clarity: _/5
Format: _/5
Overall: _/5
Justification: [brief explanation]
```

### Pas 5: Evaluare comparativă (A/B)
```python
def compare_prompts(prompt_a, prompt_b, eval_set):
    results_a = evaluate_prompt(prompt_a, eval_set)
    results_b = evaluate_prompt(prompt_b, eval_set)

    report = {
        "prompt_a_accuracy": results_a["accuracy"],
        "prompt_b_accuracy": results_b["accuracy"],
        "winner": "A" if results_a["accuracy"] > results_b["accuracy"] else "B",
        "improvement": abs(results_a["accuracy"] - results_b["accuracy"]),
        "a_errors": results_a.get("errors", []),
        "b_errors": results_b.get("errors", []),
    }
    return report
```

### Pas 6: Tracking pe termen lung
```python
evaluation_log = {
    "prompt_id": "classify_v3.2",
    "date": "2026-03-20",
    "model": "claude-sonnet-4.6",
    "eval_set_size": 50,
    "accuracy": 0.92,
    "consistency": 0.96,
    "avg_tokens": 45,
    "error_analysis": "Struggles with sarcasm (3/4 errors were sarcastic inputs)",
    "next_action": "Add sarcasm examples to prompt"
}
```

## Verificare

- [ ] Criteriile de evaluare definite ÎNAINTE de testare
- [ ] Evaluation set >= 20 items cu ground truth
- [ ] Metrici calculate (acuratețe, consistență, format)
- [ ] Error analysis efectuată (tipuri de greșeli)
- [ ] Rezultate documentate cu timestamp și model version
- [ ] Comparație cu versiunea anterioară (dacă există)

## Instrumente

| Instrument | Rol |
|-----------|-----|
| Python | Calcul metrici, automation |
| LLM-as-judge | Evaluare open-ended |
| Spreadsheet/DB | Tracking longitudinal |
| LangSmith / PromptLayer | Tracking specializat |

## Note

- **Când să folosești**: Orice prompt de producție; optimizare sistematică; model updates (re-evaluare).
- **Când NU**: Prototipare inițială (evaluează DUPĂ ce ai un prompt funcțional).
- **Greșeli comune**: Evaluare subiectivă fără metrici; eval set prea mic; lipsa error analysis; nemonitorizarea pe termen lung.
- **Relație**: ND-013 (Optimization), PR-044 (Max MI), PR-051 (Paraphrasing).
