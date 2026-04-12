---
id: PR-028
title: "Uncertainty-Routed CoT Prompting"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-028: Uncertainty-Routed CoT Prompting

## Obiectiv

Uncertainty-Routed CoT (Google, 2023) generează multiple căi de raționament CoT, apoi aplică routing bazat pe incertitudine: dacă majoritatea răspunsurilor concordă peste un threshold, selectează acel răspuns. Dacă nu, face greedy sampling și selectează acel răspuns. Combină Self-Consistency cu routing inteligent bazat pe confidence.

## Pași

### Pas 1: Generează N căi de raționament independente
Rulează același prompt CoT de N ori (N=5-10), cu temperature > 0 (0.5-0.7) pentru diversitate.

```python
responses = []
for i in range(N):
    response = llm(
        prompt=f"{PROBLEM}\nLet's think step by step.",
        temperature=0.7
    )
    responses.append(extract_answer(response))
```

### Pas 2: Calculează agreement (concordanța)
```python
from collections import Counter

answer_counts = Counter(responses)
most_common_answer, count = answer_counts.most_common(1)[0]
agreement_ratio = count / len(responses)
```

### Pas 3: Aplică threshold routing
```python
THRESHOLD = 0.7  # Calculat pe validation data

if agreement_ratio >= THRESHOLD:
    # High confidence — use majority answer
    final_answer = most_common_answer
else:
    # Low confidence — use greedy (temperature=0) sampling
    final_answer = llm(
        prompt=f"{PROBLEM}\nLet's think step by step.",
        temperature=0.0
    )
```

### Pas 4: Calibrează threshold-ul pe date de validare
```python
# Rulează pe 50+ probleme cu răspunsuri cunoscute
# Găsește threshold-ul care maximizează acuratețea
for threshold in [0.5, 0.6, 0.7, 0.8, 0.9]:
    accuracy = evaluate_with_threshold(validation_set, threshold)
    print(f"Threshold {threshold}: accuracy {accuracy}")
```

### Pas 5: Implementare practică simplificată
Dacă nu ai infrastructure de sampling, aproximare manuală:

```
Solve this problem 3 different ways:

Problem: {PROBLEM}

Approach 1: [solve with method A]
Approach 2: [solve with method B]
Approach 3: [solve with method C]

Do all three approaches give the same answer?
- If YES: That answer is likely correct.
- If NO: Re-examine the approaches that disagree and determine which is correct.

Final answer:
```

### Pas 6: Monitorizează rata de routing
Dacă >50% din queries ajung la greedy fallback, prompt-ul de bază e prea slab — îmbunătățește-l.

## Verificare

- [ ] N >= 5 căi de raționament generate
- [ ] Threshold calibrat pe validation data
- [ ] Routing funcționează (majority sau greedy)
- [ ] Rata de greedy fallback < 50%
- [ ] Acuratețe superioară vs. single CoT

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API cu temperature | Sampling divers |
| Python | Agregare + routing |
| Validation dataset | Calibrare threshold |

## Note

- **Când să folosești**: Sisteme de producție unde acuratețea e critică; math/logic automated; QA systems.
- **Când NU**: Prompting manual (overhead prea mare); task-uri creative (nu există "răspuns corect"); budget limitat de API calls.
- **Greșeli comune**: N prea mic (3 nu e suficient); threshold necalibrat; ignorarea cazurilor de low-agreement.
- **Relație**: PR-045 (Self-Consistency), PR-029 (Complexity-based), PR-043 (MoRE).
