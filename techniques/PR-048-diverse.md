---
id: PR-048
title: "DiVeRSe"
domain: ai-prompt-engineering
source: "The Prompt Report — https://arxiv.org/html/2406.06608"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# PR-048: DiVeRSe

## Obiectiv

DiVeRSe (Li et al., 2023) creează multiple prompt-uri diverse pentru o problemă, apoi aplică Self-Consistency pe fiecare, generând multiple căi de raționament per prompt. Combinația de diversitate de prompt-uri + diversitate de raționament maximizează robustețea. Double ensemble: prompt-level + reasoning-level.

## Pași

### Pas 1: Creează M prompt-uri diverse pentru aceeași problemă
Variază: instrucțiuni, exemple, formulate, perspective.

```python
prompts = [
    f"Solve step by step: {problem}",
    f"As a math teacher, explain the solution: {problem}",
    f"Break this into sub-problems and solve each: {problem}",
    f"Show your work clearly: {problem}",
    f"Think about this carefully from multiple angles: {problem}",
]
```

### Pas 2: Pentru fiecare prompt, aplică Self-Consistency (N runs)
```python
M = 5  # diverse prompts
N = 3  # self-consistency runs per prompt

all_answers = []
for prompt in prompts:
    for _ in range(N):
        response = llm(prompt, temperature=0.7)
        all_answers.append(extract_answer(response))
```

### Pas 3: Agregă toate răspunsurile (M × N total)
```python
from collections import Counter

# Total: M × N = 15 answers
vote = Counter(all_answers)
final = vote.most_common(1)[0][0]
confidence = vote.most_common(1)[0][1] / (M * N)
```

### Pas 4: Variantă cu scoring per prompt
```python
# Score each prompt's self-consistency separately
prompt_scores = {}
for i, prompt in enumerate(prompts):
    answers = all_answers[i*N : (i+1)*N]
    consistency = max(Counter(answers).values()) / N
    prompt_scores[i] = consistency

# Weight votes by prompt consistency
weighted_votes = defaultdict(float)
for i, prompt in enumerate(prompts):
    answers = all_answers[i*N : (i+1)*N]
    weight = prompt_scores[i]
    for ans in answers:
        weighted_votes[ans] += weight

final = max(weighted_votes, key=weighted_votes.get)
```

### Pas 5: Variantă manuală
```
Solve this problem using 3 different approaches. For each approach, solve it twice independently.

Approach A (step-by-step):
Try 1: [...] → Answer: ___
Try 2: [...] → Answer: ___

Approach B (break into sub-problems):
Try 1: [...] → Answer: ___
Try 2: [...] → Answer: ___

Approach C (work backwards):
Try 1: [...] → Answer: ___
Try 2: [...] → Answer: ___

Most frequent answer across all 6 attempts:
```

### Pas 6: Optimizează raportul M:N
Test: M=5,N=3 (15 calls) vs. M=3,N=5 (15 calls) vs. M=1,N=15 (pure SC). DiVeRSe (M>1) e de obicei superior pure SC.

## Verificare

- [ ] Minim 3 prompt-uri diverse (M >= 3)
- [ ] Minim 2 runs per prompt (N >= 2)
- [ ] Toate M×N răspunsuri agregate
- [ ] Diversitatea prompt-urilor e reală (nu cosmetică)
- [ ] Superior vs. Self-Consistency simplu (M=1)

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API | Multiple calls |
| Python | Orchestrare + agregare |

## Note

- **Când să folosești**: Când Self-Consistency singur nu e suficient; task-uri dificile de reasoning; sisteme de producție cu budget.
- **Când NU**: Budget limitat (M×N calls); task-uri simple; task-uri open-ended (majority vote nu funcționează).
- **Greșeli comune**: Prompt-uri prea similare (diversitate cosmetică); M×N prea mare (cost); aggregation pe text liber.
- **Relație**: PR-045 (Self-Consistency), PR-042 (DENSE), PR-043 (MoRE), PR-046 (Universal SC).
