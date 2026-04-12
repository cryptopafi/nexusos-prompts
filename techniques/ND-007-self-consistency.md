---
id: ND-007
title: "Self-Consistency and Multiple Paths of Reasoning"
domain: ai-prompt-engineering
source: "NirDiamant Prompt Engineering Guide — https://github.com/NirDiamant/Prompt_Engineering"
version: 1.0
forge_score: 3.5
business_mapping: ["all"]
---

# ND-007: Self-Consistency and Multiple Paths of Reasoning

## Obiectiv

Self-Consistency generează multiple răspunsuri la aceeași întrebare prin căi de raționament diferite, apoi selectează răspunsul cel mai consistent (majoritar). Combate inconsistența output-urilor LLM prin redundanță și vot majoritar.

## Pași

### Pas 1: Generează multiple răspunsuri cu temperature > 0
```python
responses = []
for i in range(5):
    response = llm(
        prompt=f"{question}\nLet's think step by step.",
        temperature=0.7
    )
    responses.append(response)
```

### Pas 2: Extrage răspunsul final din fiecare
```python
answers = []
for r in responses:
    answer = extract_final_answer(r)
    answers.append(answer)
# answers = ["42", "42", "38", "42", "42"]
```

### Pas 3: Votează — selectează majoritarul
```python
from collections import Counter
vote = Counter(answers)
final = vote.most_common(1)[0]
print(f"Answer: {final[0]} ({final[1]}/5 votes = {final[1]/5:.0%} confidence)")
# Answer: 42 (4/5 votes = 80% confidence)
```

### Pas 4: Variantă manuală (single prompt)
```
Solve this problem 3 different ways:

Problem: {PROBLEM}

Method 1 (algebraic): [solve]
Answer 1: ___

Method 2 (arithmetic): [solve]
Answer 2: ___

Method 3 (estimation then precise): [solve]
Answer 3: ___

All three methods should give the same answer. If they don't, identify the error.
Consensus answer:
```

### Pas 5: Interpretează divergența
- 5/5 agree → High confidence
- 4/5 agree → Good confidence (check the outlier)
- 3/5 agree → Moderate (investigate divergence)
- No majority → Problem is ambiguous or very hard

### Pas 6: Combină cu CoT and Few-Shot
Self-Consistency funcționează cel mai bine pe CoT outputs:
```python
# Few-Shot CoT + Self-Consistency
prompt = few_shot_cot_prompt + f"\nQ: {question}\nA: Let's think step by step."
responses = [llm(prompt, temperature=0.7) for _ in range(N)]
```

## Verificare

- [ ] N >= 5 responses generate
- [ ] Temperature > 0 (diversitate)
- [ ] Majority vote calculat
- [ ] Confidence score înregistrat
- [ ] Divergențe investigate

## Instrumente

| Instrument | Rol |
|-----------|-----|
| LLM API cu temperature | Generare diverse |
| Python | Voting logic |
| LangChain | Chain orchestration |

## Note

- **Când să folosești**: Math, logic, QA factual — orice cu răspuns deterministic.
- **Când NU**: Text generativ (nu există "corect"); task-uri open-ended; budget limitat.
- **Greșeli comune**: Temperature=0 (identice); N prea mic; vote pe text liber.
- **Relație**: PR-045 (Self-Consistency original), PR-046 (Universal SC), PR-028 (Uncertainty-Routed).
