---
id: PR-030
title: "Active Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-030: Active Prompting

## Obiectiv

Active Prompting (Diao et al., 2023) identifică exemplele cu cea mai mare incertitudine (unde modelul e cel mai nesigur) și cere human annotators să rescrie acele exemple cu CoT mai bun. Principiu: îmbunătățește exemplele unde modelul se luptă cel mai mult = impact maxim pe performanță.

## Pași

### Pas 1: Pregătește un set inițial de exemplare
Creează 20-50 exemplare (probleme + răspunsuri) pentru task-ul tău.

### Pas 2: Rulează modelul pe fiecare exemplar de K ori
```python
K = 5  # sampling runs per exemplar
uncertainty_scores = {}

for exemplar in all_exemplars:
    responses = [llm(exemplar["question"], temperature=0.7) for _ in range(K)]
    answers = [extract_answer(r) for r in responses]

    # Calculate disagreement (uncertainty)
    unique_answers = set(answers)
    agreement = max(Counter(answers).values()) / K
    uncertainty_scores[exemplar["id"]] = 1 - agreement
```

### Pas 3: Identifică exemplarele cu cea mai mare incertitudine
```python
# Sort by uncertainty (descending)
uncertain = sorted(uncertainty_scores.items(), key=lambda x: x[1], reverse=True)
top_uncertain = uncertain[:10]  # Top 10 most uncertain
```

### Pas 4: Re-scrie exemplarele incerte cu CoT îmbunătățit
Cere unui expert (uman sau model superior) să scrie un CoT chain detaliat și corect pentru exemplarele unde modelul e cel mai nesigur.

```
The model struggles with this type of problem. Write a detailed, step-by-step solution that covers all the tricky aspects:

Problem: {UNCERTAIN_EXEMPLAR_QUESTION}
Detailed CoT solution:
```

### Pas 5: Înlocuiește exemplarele și re-testează
Înlocuiește exemplarele vechi cu cele rescrise. Re-testează acuratețea pe un set de validare.

```python
# Replace uncertain exemplars with improved versions
for exemplar_id, improved_cot in improved_exemplars.items():
    prompt_exemplars[exemplar_id] = improved_cot

# Re-evaluate
new_accuracy = evaluate(prompt_exemplars, validation_set)
print(f"Accuracy improved: {old_accuracy} → {new_accuracy}")
```

### Pas 6: Iterează
Repetă procesul: reidentifică incertitudinile după update, re-scrie, re-testează. De obicei 2-3 iterații sunt suficiente.

## Verificare

- [ ] Incertitudinea calculată pe K >= 5 sampling runs
- [ ] Top N cele mai incerte exemplare identificate
- [ ] Exemplarele incerte rescrise cu CoT detaliat
- [ ] Acuratețe post-update superioară pre-update
- [ ] Minim 2 iterații de îmbunătățire

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API (temperature > 0) | Sampling pentru calcul incertitudine |
| Python | Calcul agreement/uncertainty |
| Human expert sau Model superior | Rescriere CoT pentru exemplare problematice |

## Note

- **Când să folosești**: Optimizare sistematică a prompt-urilor Few-Shot CoT; sisteme de producție; când ai budget de iterare.
- **Când NU**: One-off prompting; task-uri triviale; când nu ai acces la multiple sampling runs.
- **Greșeli comune**: N prea mic de sampling (incertitudinea e zgomotoasă); rescrierea exemplarelor cu aceeași calitate; neiterarea.
- **Relație**: PR-022 (CoT), PR-045 (Self-Consistency), PR-013 (Advanced Selection).
