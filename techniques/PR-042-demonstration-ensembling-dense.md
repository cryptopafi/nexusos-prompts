---
id: PR-042
title: "Demonstration Ensembling (DENSE)"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-042: Demonstration Ensembling (DENSE)

## Obiectiv

DENSE (Khalifa et al., 2023) creează multiple prompt-uri Few-Shot, fiecare cu un subset diferit de exemplare din training set. Rulează fiecare prompt separat, apoi agregă răspunsurile (majority vote). Principiu: diverse seturi de exemple produc perspective diferite; agregarea le combină.

## Pași

### Pas 1: Pregătește pool-ul de exemplare
Colectează 10-30 exemplare etichetate pentru task-ul tău.

### Pas 2: Creează K prompt-uri cu subseturi diferite
Fiecare prompt conține un subset distinct de exemplare (3-5 per prompt, fără overlap complet).

```python
import random

def create_dense_prompts(all_exemplars, k_prompts=5, exemplars_per_prompt=3):
    prompts = []
    for _ in range(k_prompts):
        subset = random.sample(all_exemplars, exemplars_per_prompt)
        prompt = format_few_shot_prompt(subset)
        prompts.append(prompt)
    return prompts
```

### Pas 3: Rulează fiecare prompt pe input-ul de test
```python
def dense_ensemble(test_input, prompts):
    responses = []
    for prompt in prompts:
        full_prompt = prompt + f"\nInput: {test_input}\nOutput:"
        response = llm(full_prompt)
        responses.append(response)
    return responses
```

### Pas 4: Agregă prin majority vote
```python
from collections import Counter

def aggregate_dense(responses):
    answers = [extract_answer(r) for r in responses]
    vote = Counter(answers).most_common(1)[0]
    return vote[0]  # Most common answer
```

### Pas 5: Exemplu practic
```
# Prompt 1 (exemplare A, B, C):
[Example A] [Example B] [Example C]
Input: "{test}" → Output:

# Prompt 2 (exemplare D, E, F):
[Example D] [Example E] [Example F]
Input: "{test}" → Output:

# Prompt 3 (exemplare A, D, G):
[Example A] [Example D] [Example G]
Input: "{test}" → Output:

# Aggregate results via majority vote
```

### Pas 6: Optimizează K și subset size
Testează: K=3,5,7 prompts; subset sizes 2,3,5. Găsește combinația optimă pe validation set.

## Verificare

- [ ] Minim 3 prompt-uri diferite create
- [ ] Fiecare prompt are un subset distinct de exemplare
- [ ] Toate prompt-urile rulat pe input-ul de test
- [ ] Majority vote calculat corect
- [ ] Performanță superioară vs. single prompt

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API | Multiple calls cu prompt-uri diferite |
| Python | Creare subseturi + agregare |

## Note

- **Când să folosești**: Când acuratețea e critică; când ai suficiente exemplare; sisteme automatizate.
- **Când NU**: One-off prompting; budget limitat de API calls; task-uri unde un singur prompt e suficient.
- **Greșeli comune**: Subseturi prea similare; K prea mic; majority vote pe task-uri fără răspuns discret.
- **Relație**: PR-045 (Self-Consistency), PR-048 (DiVeRSe), PR-044 (Max MI).
