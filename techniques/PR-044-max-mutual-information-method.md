---
id: PR-044
title: "Max Mutual Information Method"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-044: Max Mutual Information Method

## Obiectiv

Max Mutual Information (Sorensen et al., 2022) creează multiple template-uri de prompt cu stiluri și exemplare variate, apoi selectează template-ul care maximizează mutual information între prompt și output-urile LLM-ului. Idee: template-ul optim e cel care produce output-uri cele mai informative și consistente.

## Pași

### Pas 1: Generează multiple template-uri pentru același task
Variază: formularea instrucțiunii, formatul, ordinea, stilul.

```
# Template A — Direct:
"Classify the following text as positive, negative, or neutral: {text}"

# Template B — Question format:
"What is the sentiment of this text? Answer with positive, negative, or neutral: {text}"

# Template C — Instructional:
"Read the following text carefully. Determine whether the overall sentiment expressed is positive, negative, or neutral. Text: {text}"

# Template D — Role-based:
"As a sentiment analysis expert, classify this text: {text}. Sentiment:"

# Template E — Structured:
"Text: {text}\nTask: Sentiment classification\nCategories: positive, negative, neutral\nSentiment:"
```

### Pas 2: Rulează fiecare template pe un set de calibrare
Folosește 20-50 input-uri de test cu răspunsuri cunoscute.

```python
results = {}
for template_name, template in templates.items():
    correct = 0
    total = len(calibration_set)
    for item in calibration_set:
        response = llm(template.format(text=item["text"]))
        if extract_answer(response) == item["label"]:
            correct += 1
    results[template_name] = correct / total
```

### Pas 3: Calculează mutual information (simplificat)
Proxy practic: template-ul cu cea mai mare acuratețe PE CARE modelul produce și output-uri consistente (low entropy).

```python
def template_score(template, calibration_set, n_runs=3):
    """Score = accuracy * consistency"""
    accuracies = []
    for run in range(n_runs):
        acc = evaluate_accuracy(template, calibration_set)
        accuracies.append(acc)

    avg_accuracy = np.mean(accuracies)
    consistency = 1 - np.std(accuracies)  # Low variance = high consistency
    return avg_accuracy * consistency

best_template = max(templates, key=lambda t: template_score(t, calibration_set))
```

### Pas 4: Selectează template-ul optim
Template-ul cu scorul maxim devine template-ul de producție.

### Pas 5: Variantă manuală simplificată
Dacă nu ai infrastructure programatică:
1. Scrie 3-5 variante de prompt
2. Testează fiecare pe 10 input-uri
3. Alege varianta cu cele mai bune rezultate
4. Documentează-o ca standard

### Pas 6: Re-evaluează periodic
Modelele se actualizează. Re-testează template-urile la fiecare update de model.

## Verificare

- [ ] Minim 3 template-uri testate
- [ ] Set de calibrare >= 20 input-uri
- [ ] Acuratețea și consistența măsurate
- [ ] Template-ul optim selectat obiectiv
- [ ] Template-ul documentat pentru producție

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API | Testare template-uri |
| Python | Calcul metrici |
| Validation dataset | Calibrare |

## Note

- **Când să folosești**: Sisteme de producție; când vrei cel mai bun template posibil; A/B testing de prompt-uri.
- **Când NU**: One-off prompting; task-uri simple; lipsa unui set de calibrare.
- **Greșeli comune**: Set de calibrare prea mic; template-uri prea similare; n_runs insuficiente.
- **Relație**: PR-042 (DENSE), PR-051 (Prompt Paraphrasing), ND-013 (Prompt Optimization).
