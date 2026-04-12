---
id: PR-013
title: "Advanced Exemplar Selection Techniques (LENS, UDR, Active Example Selection)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-013: Advanced Exemplar Selection Techniques

## Obiectiv

Tehnici avansate de selecție a exemplelor care depășesc KNN simplu: LENS (Li & Qiu, 2023) folosește un model auxiliar pentru a selecta exemple; UDR (Li et al., 2023) folosește un retriever unificat antrenat pe multiple task-uri; Active Example Selection (Zhang et al.) selectează iterativ exemple care maximizează performanța. Această procedură oferă implementări practice simplificate ale acestor concepte.

## Pași

### Pas 1: Evaluează dacă ai nevoie de tehnici avansate
Tehnicile simple (KNN, Vote-K) sunt suficiente pentru 80% din cazuri. Treci la tehnici avansate doar dacă:
- KNN dă rezultate inconsistente
- Task-ul are variabilitate mare
- Ai resurse pentru pipeline complex

### Pas 2: LENS-inspired — Selecție prin model auxiliar
Folosește un model mai mic/rapid pentru a evalua utilitatea fiecărui exemplu.

```
# Prompt pentru modelul evaluator:
Rate how helpful this example would be for solving the given task.

Task: Classify customer intent from support messages.
Test input: "I need to change my delivery address"

Candidate example: "Can I update my shipping info?" → Address Change
Helpfulness (1-5):

Candidate example: "Your product is defective" → Returns
Helpfulness (1-5):
```

Selectează exemplele cu scor maxim.

### Pas 3: UDR-inspired — Retriever cross-task
Antrenează sau folosește un retriever care funcționează pe multiple tipuri de task-uri:
```python
# Conceptual: retriever antrenat pe diverse task-uri
retriever = UnifiedRetriever(tasks=[
    "classification", "extraction", "summarization"
])

# La runtime, selectează exemple relevante indiferent de task type
examples = retriever.retrieve(test_input, task_type="classification", k=3)
```

### Pas 4: Active Selection — Selecție iterativă
1. Începe cu un set random de exemple
2. Evaluează performanța pe validare
3. Adaugă/înlocuiește exemplul care îmbunătățește cel mai mult performanța
4. Repetă până la convergență

```python
def active_select(pool, validation_set, k=3):
    selected = random.sample(pool, k)
    best_score = evaluate(selected, validation_set)

    for candidate in pool:
        for i in range(k):
            trial = selected.copy()
            trial[i] = candidate
            score = evaluate(trial, validation_set)
            if score > best_score:
                selected = trial
                best_score = score
    return selected
```

### Pas 5: Combină tehnicile
Folosește LENS pentru pre-filtrare → KNN pentru selecție finală → Active Selection pentru fine-tuning.

### Pas 6: Documentează pipeline-ul și cache-ează rezultatele
Selecția avansată e costisitoare. Cache-ează exemplele selectate per tip de input.

## Verificare

- [ ] Tehnica avansată justificată (KNN simplu insuficient)
- [ ] Pipeline implementat end-to-end
- [ ] Performanța comparată cu KNN baseline
- [ ] Îmbunătățire măsurabilă (>5% peste KNN)
- [ ] Rezultate cache-ate pentru eficiență

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM mic (Haiku/GPT-3.5) | Evaluator LENS |
| SentenceTransformers | Embedding retrieval |
| Python | Pipeline orchestration |
| Redis/SQLite | Cache selecții |

## Note

- **Când să folosești**: Sisteme de producție cu volum mare; când KNN simplu nu e suficient; când ai resurse de compute.
- **Când NU**: Prompting manual; task-uri simple; prototipare (overhead prea mare).
- **Greșeli comune**: Over-engineering pentru task-uri simple; lipsa baseline-ului KNN pentru comparație; pipeline prea lent pentru producție.
- **Relație**: PR-009 (KNN), PR-010 (Vote-K), PR-030 (Active Prompting).
