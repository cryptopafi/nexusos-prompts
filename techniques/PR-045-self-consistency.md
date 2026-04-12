---
id: PR-045
title: "Self-Consistency"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-045: Self-Consistency

## Obiectiv

Self-Consistency (Wang et al., 2022) se bazează pe intuiția că multiple căi de raționament diferite pot duce la același răspuns corect. Generează N răspunsuri cu CoT (temperature > 0), apoi selectează răspunsul majoritar. Simplu, eficient, și îmbunătățește semnificativ acuratețea pe reasoning tasks.

## Pași

### Pas 1: Formulează un prompt CoT standard
Folosește orice variantă CoT: Zero-Shot, Few-Shot, Plan-and-Solve etc.

### Pas 2: Generează N răspunsuri independente
```python
N = 5  # Minimum recomandat; 10-20 pentru precizie maximă

responses = []
for _ in range(N):
    response = llm(
        prompt=cot_prompt,
        temperature=0.7  # Diversitate în raționament
    )
    responses.append(response)
```

### Pas 3: Extrage răspunsul final din fiecare response
```python
answers = [extract_final_answer(r) for r in responses]
# extract_final_answer() parsează textul după "The answer is" sau similar
```

### Pas 4: Selectează prin majority vote
```python
from collections import Counter

vote = Counter(answers)
final_answer = vote.most_common(1)[0][0]
confidence = vote.most_common(1)[0][1] / N

print(f"Answer: {final_answer} (confidence: {confidence:.0%})")
```

### Pas 5: Variantă manuală (fără cod)
```
Solve this problem 5 different ways. For each, show your reasoning then give a final answer.

Problem: {PROBLEM}

Solution 1: [...]  → Answer: ___
Solution 2: [...]  → Answer: ___
Solution 3: [...]  → Answer: ___
Solution 4: [...]  → Answer: ___
Solution 5: [...]  → Answer: ___

Most common answer among the 5 solutions:
Final answer:
```

### Pas 6: Setează N optim
- N=5: bun default, economisește tokens
- N=10: mai robust, cost moderat
- N=20+: maxim acuratețe, cost ridicat
- Regula: N impar (evită tie-breaks)

## Verificare

- [ ] N >= 5 răspunsuri generate
- [ ] Temperature > 0 (diversitate)
- [ ] Majority vote calculat corect
- [ ] Confidența înregistrată (% agreement)
- [ ] Îmbunătățire vs. single CoT

## Instrumente

| Model | Eficacitate |
|-------|-------------|
| Toate modelele | Universală — funcționează cu orice LLM |
| Claude Sonnet/Haiku | Cost-eficient pentru N mare |
| Python | Automatizare voting |

## Note

- **Când să folosești**: Math, logic, clasificare, orice task cu răspuns deterministic; sisteme de producție.
- **Când NU**: Task-uri creative (nu există "răspuns corect" majoritar); task-uri open-ended; budget foarte limitat.
- **Greșeli comune**: Temperature=0 (toate răspunsurile identice); N prea mic; majority vote pe text liber (nu se poate agrega).
- **Relație**: PR-022 (CoT), PR-028 (Uncertainty-Routed), PR-046 (Universal SC), ND-007 (Self-Consistency detailed).
